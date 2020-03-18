---
title: WSE 3.0 Web サービスの WCF への移行
ms.date: 03/30/2017
ms.assetid: 7bc5fff7-a2b2-4dbc-86cc-ecf73653dcdc
ms.openlocfilehash: 50939253027ff82a256b9627305c26fb69e13be9
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76212144"
---
# <a name="migrating-wse-30-web-services-to-wcf"></a>WSE 3.0 Web サービスの WCF への移行
WSE 3.0 Web サービスを Windows Communication Foundation (WCF) に移行する利点には、パフォーマンスの向上と、追加のトランスポート、追加のセキュリティシナリオ、および WS-* 仕様のサポートが含まれます。 WSE 3.0 から WCF に移行された Web サービスは、最大200% から400% のパフォーマンス向上を実現できます。 WCF でサポートされているトランスポートの詳細については、「[トランスポートの選択](../../../../docs/framework/wcf/feature-details/choosing-a-transport.md)」を参照してください。 WCF でサポートされるシナリオの一覧については、「[一般的なセキュリティシナリオ](../../../../docs/framework/wcf/feature-details/common-security-scenarios.md)」を参照してください。 WCF でサポートされている仕様の一覧については、「 [Web サービスプロトコルの相互運用性ガイド](../../../../docs/framework/wcf/feature-details/web-services-protocols-interoperability-guide.md)」を参照してください。  
  
 次のセクションでは、WSE 3.0 Web サービスの特定の機能を WCF に移行する方法について説明します。  
  
## <a name="general"></a>[全般]  
 WSE 3.0 と WCF アプリケーションには、ワイヤレベルの相互運用性と、一般的な一連の用語が含まれています。 WSE 3.0 および WCF アプリケーションは、両方がサポートする WS-* 仕様のセットに基づいて、ネットワークレベルの相互運用が可能です。 WSE 3.0 または WCF アプリケーションの開発時には、WSE のターンキーセキュリティアサーションの名前や認証モードなど、一般的な一連の用語があります。  
  
 WCF と ASP.NET または WSE 3.0 のプログラミングモデルには多くの類似した側面がありますが、これらは異なります。 WCF プログラミングモデルの詳細については、「[基本的なプログラミングライフサイクル](../../../../docs/framework/wcf/basic-programming-lifecycle.md)」を参照してください。  
  
> [!NOTE]
> WSE Web サービスを WCF に移行するには、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)ツールを使用してクライアントを生成できます。 ただし、このクライアントには、WCF サービスの開始点として使用できるインターフェイスとクラスも含まれます。 生成されるインターフェイスの <xref:System.ServiceModel.OperationContractAttribute> 属性は、<xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> プロパティが `*` に設定されたコントラクトのメンバーに適用されます。 WSE クライアントがこの設定で Web サービスを呼び出すと、次の例外がスローされます。 **Services3: WSE910: 応答メッセージの処理中にエラーが発生したため、内部例外でエラーを見つけることができ**ます。 このエラーを軽減するには、<xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> 属性の <xref:System.ServiceModel.OperationContractAttribute> プロパティを `null` 以外の値 (`http://Microsoft.WCF.Documentation/ResponseToOCAMethod` など) に設定します。  
  
## <a name="security"></a>セキュリティ  
  
### <a name="wse-30-web-services-that-are-secured-using-a-policy-file"></a>ポリシー ファイルを使用してセキュリティ保護される WSE 3.0 Web サービス  
 WCF サービスでは、構成ファイルを使用してサービスをセキュリティで保護できます。このメカニズムは、WSE 3.0 ポリシーファイルに似ています。 WSE 3.0 でポリシー ファイルを使用して Web サービスをセキュリティ保護するときは、設定不要のセキュリティ アサーションまたはカスタム ポリシー アサーションを使用します。 ターンキーセキュリティアサーションは、WCF セキュリティバインド要素の認証モードに厳密にマップされます。 WCF 認証モードと WSE 3.0 のターンキーセキュリティアサーションだけでなく、同じまたは同様の名前が付けられているため、同じ資格情報の種類を使用してメッセージをセキュリティで保護します。 たとえば、WSE 3.0 の `usernameForCertificate` ターンキーセキュリティアサーションは、WCF の `UsernameForCertificate` 認証モードにマップされます。 次のコード例では、WSE 3.0 の `usernameForCertificate` ターンキーセキュリティアサーションを使用する最小ポリシーが、カスタムバインドの WCF の `UsernameForCertificate` 認証モードにマップされる方法を示します。  
  
 **WSE 3.0**  
  
```xml  
<policies>  
  <policy name="MyPolicy">  
    <usernameForCertificate messageProtectionOrder="SignBeforeEncrypt"  
                            requireDeriveKeys="true"/>  
  </policy>  
</policies>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <security authenticationMode="UserNameForCertificate"   
              messageProtectionOrder="SignBeforeEncrypt"  
              requireDerivedKeys="true"/>  
  </binding>  
</customBinding>  
```  
  
 ポリシーファイルで指定されている WSE 3.0 Web サービスのセキュリティ設定を WCF に移行するには、構成ファイル内にカスタムバインドを作成し、それに対応する認証モードに設定する必要があります。 さらに、WSE 3.0 クライアントがサービスと通信するときに 2004 年 8 月版の WS-Addressing 仕様を使用するように、カスタム バインディングを構成する必要もあります。 移行した WCF サービスが WSE 3.0 クライアントとの通信を必要とせず、セキュリティパリティのみを維持する必要がある場合は、カスタムバインドを作成するのではなく、適切なセキュリティ設定を使用して WCF システム定義バインディングを使用することを検討してください。  
  
 次の表は、WSE 3.0 ポリシーファイルと WCF の同等のカスタムバインディングとの対応関係を示しています。  
  
|WSE 3.0 の設定不要のセキュリティ アサーション|WCF のカスタム バインド構成|  
|----------------------------------------|--------------------------------------|  
|\<usernameOverTransportSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UserNameOverTransport" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate10Security/>|`<customBinding>   <binding name="MyBinding">     <security messageSecurityVersion="WSSecurity10WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10" authenticationMode="MutualCertificate" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<usernameForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UsernameForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<anonymousForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="AnonymousForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<kerberosSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="Kerberos"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate11Security/>|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="MutualCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
  
 WCF でカスタムバインドを作成する方法の詳細については、「[カスタムバインド](../../../../docs/framework/wcf/extending/custom-bindings.md)」を参照してください。  
  
### <a name="wse-30-web-services-that-are-secured-using-application-code"></a>アプリケーション コードを使用してセキュリティ保護される WSE 3.0 Web サービス  
 WSE 3.0 または WCF が使用されているかどうかにかかわらず、セキュリティ要件は、構成ではなくアプリケーションコードで指定できます。 WSE 3.0 でこれを行うには、`Policy` クラスの派生クラスを作成してから、`Add` メソッドを呼び出して要件を追加します。 コードでセキュリティ要件を指定する方法の詳細については、「[方法: ポリシーファイルを使用せずに Web サービスをセキュリティで保護](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa528763(v=msdn.10))する」を参照してください。 WCF では、コードでセキュリティ要件を指定するには、<xref:System.ServiceModel.Channels.BindingElementCollection> クラスのインスタンスを作成し、<xref:System.ServiceModel.Channels.SecurityBindingElement> のインスタンスを <xref:System.ServiceModel.Channels.BindingElementCollection>に追加します。 セキュリティ アサーション要件は、<xref:System.ServiceModel.Channels.SecurityBindingElement> クラスの静的認証モード ヘルパー メソッドを使用して設定します。 WCF を使用してコードのセキュリティ要件を指定する方法の詳細については、「方法: 指定した認証モードのための[カスタムバインディングを作成](how-to-create-a-custom-binding-using-the-securitybindingelement.md)する」および「[方法: 指定した認証モードの保護を作成する](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)」を参照してください。  
  
### <a name="wse-30-custom-policy-assertion"></a>WSE 3.0 カスタム ポリシー アサーション  
 WSE 3.0 には、2 種類のカスタム ポリシー アサーションがあります。一方は SOAP メッセージをセキュリティで保護し、もう一方は SOAP メッセージをセキュリティで保護しません。 セキュリティで保護された SOAP メッセージをセキュリティで保護するポリシーアサーションは、WSE 3.0 `SecurityPolicyAssertion` クラスから派生し、WCF の概念と同等のものが <xref:System.ServiceModel.Channels.SecurityBindingElement> クラスです。  
  
 重要な点は、WSE 3.0 のターンキーセキュリティアサーションは、WCF 認証モードのサブセットであることです。 WSE 3.0 でカスタムポリシーアサーションを作成した場合、同等の WCF 認証モードが存在する可能性があります。 たとえば、WSE 3.0 は、設定不要の `UsernameOverTransport` セキュリティ アサーションと等価な CertificateOverTransport セキュリティ アサーションを提供しませんが、クライアントを認証するために X.509 証明書を使用します。 このシナリオで独自のカスタムポリシーアサーションを定義した場合は、WCF によって移行が簡単になります。 WCF では、このシナリオの認証モードを定義するため、静的認証モードヘルパーメソッドを利用して WCF<xref:System.ServiceModel.Channels.SecurityBindingElement>を構成できます。  
  
 SOAP メッセージをセキュリティで保護するカスタムポリシーアサーションに相当する WCF 認証モードがない場合は、<xref:System.ServiceModel.Channels.TransportSecurityBindingElement>、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> または <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>WCF クラスからクラスを派生させ、同等のバインド要素を指定します。 詳細については、「[方法: カスタムバインディングを使用してカスタムバインディングを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)する」を参照してください。  
  
 SOAP メッセージをセキュリティで保護しないカスタムポリシーアサーションを変換する方法については、「[フィルター処理](../../../../docs/framework/wcf/feature-details/filtering.md)」および「[カスタムメッセージインターセプター](../../../../docs/framework/wcf/samples/custom-message-interceptor.md)のサンプル」を参照してください。  
  
### <a name="wse-30-custom-security-token"></a>WSE 3.0 カスタム セキュリティ トークン  
 カスタムトークンを作成するための WCF プログラミングモデルは、WSE 3.0 とは異なります。 WSE でカスタムトークンを作成する方法の詳細については、「[カスタムセキュリティトークンの作成](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa529304(v=msdn.10))」を参照してください。 WCF でカスタムトークンを作成する方法の詳細については、「[方法: カスタムトークンを作成](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)する」を参照してください。  
  
### <a name="wse-30-custom-token-manager"></a>WSE 3.0 カスタム トークン マネージャー  
 カスタムトークンマネージャーを作成するためのプログラミングモデルは、WCF では WSE 3.0 とは異なります。 カスタムトークンマネージャーと、カスタムセキュリティトークンに必要なその他のコンポーネントを作成する方法の詳細については、「[方法: カスタムトークンを作成](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)する」を参照してください。  
  
> [!NOTE]
> カスタム `UsernameToken` セキュリティトークンマネージャーを作成した場合、WCF では、カスタムセキュリティトークンマネージャーを作成するよりも簡単な方法で認証ロジックを指定できます。 詳細については、「[方法: カスタムユーザー名とパスワード検証を使用](../../../../docs/framework/wcf/feature-details/how-to-use-a-custom-user-name-and-password-validator.md)する」を参照してください。  
  
### <a name="wse-30-web-services-that-use-mtom-encoded-soap-messages"></a>MTOM エンコードされた SOAP メッセージを使用する WSE 3.0 Web サービス  
 WSE 3 アプリケーションと同様に、WCF アプリケーションでは構成で MTOM メッセージエンコーディングを指定できます。 この設定を移行するには、サービスのバインドに[\<mtomMessageEncoding >](../../../../docs/framework/configure-apps/file-schema/wcf/mtommessageencoding.md)を追加します。 次のコード例は、WCF と同等のサービスに対して WSE 3.0 で MTOM エンコードを指定する方法を示しています。  
  
 **WSE 3.0**  
  
```xml  
<messaging>  
    <mtom clientMode="On"/>  
</messaging>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <mtomMessageEncoding/>  
  </binding>  
</customBinding>  
```  
  
## <a name="messaging"></a>メッセージング  
  
### <a name="wse-30-applications-that-use-the-wse-messaging-api"></a>WSE メッセージング API を使用する WSE 3.0 アプリケーション  

 クライアントと Web サービス間でやり取りされる XML に直接アクセスするために WSE メッセージング API が使用されている場合は、"Plain Old XML" (POX) を使用するようにアプリケーションを変換できます。 POX の詳細については、「 [POX アプリケーションとの相互運用性](interoperability-with-pox-applications.md)」を参照してください。 WSE メッセージング API の詳細については、「 [Wse メッセージング Api を使用した SOAP メッセージの送受信](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa529293(v=msdn.10))」を参照してください。  
  
## <a name="transports"></a>トランスポート  
  
### <a name="tcp"></a>TCP  
 既定では、TCP トランスポートを使用して SOAP メッセージを送信する WSE 3.0 クライアントおよび Web サービスは、WCF クライアントおよび Web サービスと相互運用できません。 このような非互換性は、TCP プロトコルで使用されるフレームの違いとパフォーマンス上の理由に起因します。 ただし、WCF サンプルでは、WSE 3.0 と相互運用するカスタム TCP セッションを実装する方法について詳しく説明します。 このサンプルの詳細については、「 [Transport: WSE 3.0 TCP 相互運用性](../../../../docs/framework/wcf/samples/transport-wse-3-0-tcp-interoperability.md)」を参照してください。  
  
 WCF アプリケーションで TCP トランスポートを使用するように指定するには、 [\<netTcpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/nettcpbinding.md)を使用します。  
  
### <a name="custom-transport"></a>カスタム トランスポート  
 WCF での WSE 3.0 カスタムトランスポートに相当するものは、チャネル拡張です。 チャネル拡張の作成の詳細については、「[チャネルレイヤーの拡張](../../../../docs/framework/wcf/extending/extending-the-channel-layer.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [基本的なプログラミング ライフサイクル](../../../../docs/framework/wcf/basic-programming-lifecycle.md)
- [カスタム バインディング](../../../../docs/framework/wcf/extending/custom-bindings.md)
- [方法 : SecurityBindingElement を使用してカスタム バインディングを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [方法 : 指定した認証モード用の SecurityBindingElement を作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
