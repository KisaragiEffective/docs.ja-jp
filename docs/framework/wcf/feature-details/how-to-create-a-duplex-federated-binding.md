---
title: '方法 : 双方向フェデレーション バインディングを作成する'
ms.date: 03/30/2017
ms.assetid: 4331d2bc-5455-492a-9189-634a82597726
ms.openlocfilehash: 3ecd9e2dbeb30bb255cbf66488b50a9b20219431
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141718"
---
# <a name="how-to-create-a-duplex-federated-binding"></a>方法 : 双方向フェデレーション バインディングを作成する

<xref:System.ServiceModel.WSFederationHttpBinding> はデータグラムと要求/応答メッセージの交換コントラクトのみをサポートします。 双方向メッセージ交換コントラクトを使用するには、カスタム バインディングを作成する必要があります。 次の手順は、構成の中でそれを行う方法を説明します。HTTP と TCP トランスポートにはメッセージ モード セキュリティを、TCP トランスポートには混合モード セキュリティを使用します。 このトピックの最後に 3 つのバインディングすべてのサンプル コードがあります。

バインディングはコード内でも作成できます。 作成するバインド要素スタックの説明については、「[方法: カスタムバインディングを使用してカスタムバインディングを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)する」を参照してください。

## <a name="to-create-a-duplex-federated-custom-binding-with-http"></a>HTTP で双方向カスタム フェデレーション バインディングを作成するには

1. 構成ファイルの[\<バインド >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)ノードで、 [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素を作成します。

2. [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素内に、`name` 属性が `FederationDuplexHttpMessageSecurityBinding`に設定された[\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素を作成します。

3. [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)要素内で、`authenticationMode` 属性が `SecureConversation`に設定された[\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素を作成します。

4. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素内で、`authenticationMode` 属性を `IssuedTokenForCertificate` または `IssuedTokenForSslNegotiated`に設定して、 [\<secureConversationBootstrap >](../../../../docs/framework/configure-apps/file-schema/wcf/secureconversationbootstrap.md)要素を作成します。

5. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素の後に、空の[\<compositeDuplex >](../../../../docs/framework/configure-apps/file-schema/wcf/compositeduplex.md)要素を作成します。

6. [\<compositeDuplex >](../../../../docs/framework/configure-apps/file-schema/wcf/compositeduplex.md)要素の後に、空の[\<oneWay >](../../../../docs/framework/configure-apps/file-schema/wcf/oneway.md)要素を作成します。

7. [\<oneWay >](../../../../docs/framework/configure-apps/file-schema/wcf/oneway.md)要素の後に、空の[\<httptransport >](../../../../docs/framework/configure-apps/file-schema/wcf/httptransport.md)要素を作成します。

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-message-security-mode"></a>TCP メッセージ セキュリティ モードで双方向カスタム フェデレーション バインディングを作成するには

1. 構成ファイルの[\<バインド >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)ノードで、 [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素を作成します。

2. [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素内に、`name` 属性が `FederationDuplexTcpMessageSecurityBinding`に設定された[\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素を作成します。

3. [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)要素内で、`authenticationMode` 属性が `SecureConversation`に設定された[\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素を作成します。

4. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素内で、`authenticationMode` 属性を `IssuedTokenForCertificate` または `IssuedTokenForSslNegotiated`に設定して、 [\<secureConversationBootstrap >](../../../../docs/framework/configure-apps/file-schema/wcf/secureconversationbootstrap.md)要素を作成します。

5. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素に従って、空の[\<tcptransport >](../../../../docs/framework/configure-apps/file-schema/wcf/tcptransport.md)要素を作成します。

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-mixed-security-mode"></a>TCP 混合セキュリティ モードで双方向カスタム フェデレーション バインディングを作成するには

1. 構成ファイルの[\<バインド >](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)ノードで、 [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素を作成します。

2. [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)要素内に、`name` 属性が `FederationDuplexTcpTransportSecurityWithMessageCredentialBinding`に設定された[\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素を作成します。

3. [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)要素内で、`authenticationMode` 属性が `SecureConversation`に設定された[\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素を作成します。

4. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素内で、`authenticationMode` 属性を `IssuedTokenForCertificate` または `IssuedTokenForSslNegotiated`に設定して、 [\<secureConversationBootstrap >](../../../../docs/framework/configure-apps/file-schema/wcf/secureconversationbootstrap.md)要素を作成します。

5. [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)要素の後に、空の[\<sslstreamsecurity >](../../../../docs/framework/configure-apps/file-schema/wcf/sslstreamsecurity.md)要素を作成します。

6. [\<sslStreamSecurity >](../../../../docs/framework/configure-apps/file-schema/wcf/sslstreamsecurity.md)要素に従って、空の[\<tcptransport >](../../../../docs/framework/configure-apps/file-schema/wcf/tcptransport.md)要素を作成します。

## <a name="code-sample"></a>コード サンプル

### <a name="sample-with-3-bindings"></a>3 つのバインディングの例

1. 次のコードを構成ファイルに挿入します。

## <a name="example"></a>例

```xml
<bindings>
   <customBinding>
      <binding name="FederationDuplexHttpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <compositeDuplex />
          <oneWay />
          <httpTransport />
       </binding>
<!-- duplex over https is not supported -->
       <binding name="FederationDuplexTcpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <tcpTransport />
       </binding>
       <binding name="FederationDuplexTcpTransportSecurityWithMessageCredentialsBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenOverTransport" />
          </security>
<!-- requireClientCertificate = true or <windowsStreamSecurity /> can be used, but does not make sense for most scenarios -->
          <sslStreamSecurity />
          <tcpTransport />
       </binding>
    </customBinding>
</bindings>
```
