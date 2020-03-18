---
title: '方法 : WCF サービスと WSE 3.0 クライアントを相互運用するために構成する'
ms.date: 03/30/2017
ms.assetid: 0f38c4a0-49a6-437c-bdde-ad1d138d3c4a
ms.openlocfilehash: bd9f2bec94ca45f76590f64366428a00edd5d6ea
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141747"
---
# <a name="how-to-configure-wcf-services-to-interoperate-with-wse-30-clients"></a>方法 : WCF サービスと WSE 3.0 クライアントを相互運用するために構成する

Windows Communication Foundation (WCF) サービスは、WCF サービスが WS-ADDRESSING 仕様の8月2004バージョンを使用するように構成されている場合に、Web サービス拡張 Microsoft .NET 3.0 (WSE) クライアントとのワイヤレベルの互換性があります。

### <a name="to-enable-a-wcf-service-to-interoperate-with-wse-30-clients"></a>WCF サービスを WSE 3.0 クライアントと相互運用できるようにするには

1. WCF サービスのカスタムバインドを定義します。

    メッセージ エンコーディングに 2004 年 8 月版の WS-Addressing 仕様が使用されるように指定するには、カスタム バインディングを作成する必要があります。

    1. サービスの構成ファイルの[\<バインド](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)に子[\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)を追加します。

    2. バインドの名前を指定します。そのためには、 [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)を[\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)に追加し、`name` 属性を設定します。

    3. 認証モードと、 [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)に子[\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)を追加することによって、WSE 3.0 と互換性のあるメッセージをセキュリティで保護するために使用する ws-security 仕様のバージョンを指定します。

        認証モードを設定するには、 [\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)の `authenticationMode` 属性を設定します。 認証モードは、WSE 3.0 の設定不要のセキュリティ アサーションとほぼ同等です。 次の表では、WCF の認証モードを WSE 3.0 のターンキーセキュリティアサーションにマップします。

        |WCF 認証モード|WSE 3.0 の設定不要のセキュリティ アサーション|
        |-----------------------------|----------------------------------------|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate>|`anonymousForCertificateSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.Kerberos>|`kerberosSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate10Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate11Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameOverTransport>|`usernameOverTransportSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameForCertificate>|`usernameForCertificateSecurity`|

        `mutualCertificate10Security` と `mutualCertificate11Security` ターンキーのセキュリティアサーションの主な相違点の1つ \*、WSE が SOAP メッセージをセキュリティで保護するために使用する WS-SECURITY 仕様のバージョンです。 `mutualCertificate10Security` では WS-Security 1.0 が使用され、`mutualCertificate11Security` では WS-Security 1.1 が使用されます。 WCF の場合、WS-SECURITY 仕様のバージョンは、 [\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)の `messageSecurityVersion` 属性で指定されます。

        SOAP メッセージのセキュリティ保護に使用する WS-SECURITY 仕様のバージョンを設定するには、 [\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)の `messageSecurityVersion` 属性を設定します。 WSE 3.0 と相互運用するには、`messageSecurityVersion` 属性の値を <xref:System.ServiceModel.MessageSecurityVersion.WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10%2A> に設定します。

    4. [\<textMessageEncoding >](../../../../docs/framework/configure-apps/file-schema/wcf/textmessageencoding.md)を追加し、その値を <xref:System.ServiceModel.Channels.MessageVersion.Soap11WSAddressingAugust2004%2A>に `messageVersion` 設定することによって、WCF によって ws-addressing 仕様の8月2004バージョンが使用されるように指定します。

        > [!NOTE]
        > SOAP 1.2 の使用時には、`messageVersion` 属性を <xref:System.ServiceModel.Channels.MessageVersion.Soap12WSAddressingAugust2004%2A> に設定します。

2. サービスがカスタム バインドを使用するように指定します。

    1. [\<エンドポイントの >](../../../../docs/framework/configure-apps/file-schema/wcf/endpoint-element.md)要素の `binding` 属性を `customBinding`に設定します。

    2. [\<エンドポイントの >](../../../../docs/framework/configure-apps/file-schema/wcf/endpoint-element.md)要素の `bindingConfiguration` 属性を、カスタムバインドの[\<binding >](../../configure-apps/file-schema/wcf/bindings.md)の `name` 属性で指定された値に設定します。

## <a name="example"></a>例

次のコード例では、`Service.HelloWorldService` が WSE 3.0 クライアントと相互運用するためにカスタム バインドを使用するように指定します。 カスタム バインドには 2004 年 8 月版の WS-Addressing が指定され、WS-Security 1.1 の一連の仕様が、交換されるメッセージのエンコードに使用されます。 メッセージは、<xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate> 認証モードを使用してセキュリティ保護されます。

```xml
<configuration>
  <system.serviceModel>
    <services>
      <service
        behaviorConfiguration="ServiceBehavior"
        name="Service.HelloWorldService">
        <endpoint binding="customBinding" address=""
          bindingConfiguration="ServiceBinding"
          contract="Service.IHelloWorld"></endpoint>
      </service>
    </services>

    <bindings>
      <customBinding>
        <binding name="ServiceBinding">
          <security authenticationMode="AnonymousForCertificate"
                  messageProtectionOrder="SignBeforeEncrypt"
                  messageSecurityVersion="WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10"
                  requireDerivedKeys="false">
          </security>
          <textMessageEncoding messageVersion ="Soap11WSAddressingAugust2004"></textMessageEncoding>
          <httpTransport/>
        </binding>
      </customBinding>
    </bindings>
    <behaviors>
      <behavior name="ServiceBehavior" returnUnknownExceptionsAsFaults="true">
        <serviceCredentials>
          <serviceCertificate findValue="CN=WCFQuickstartServer" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectDistinguishedName"/>
        </serviceCredentials>
      </behavior>
    </behaviors>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>関連項目

- [方法 : システム指定のバインディングをカスタマイズする](../../../../docs/framework/wcf/extending/how-to-customize-a-system-provided-binding.md)
