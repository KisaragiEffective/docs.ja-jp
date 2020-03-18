---
title: システム標準の相互運用性バインディングがサポートしている Web サービス プロトコル
ms.date: 03/30/2017
helpviewer_keywords:
- WS-protocols
- Web services protocols
- Windows Communication Foundation, Web service protocols
ms.assetid: 1f7fc4ff-30fe-4e46-adda-91caad3b06c6
ms.openlocfilehash: 80fa0e501f425bd5b917059e90f2811f075db651
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746897"
---
# <a name="web-services-protocols-supported-by-system-provided-interoperability-bindings"></a>システム標準の相互運用性バインディングがサポートしている Web サービス プロトコル
Windows Communication Foundation (WCF) は、web サービス仕様と呼ばれる一連の仕様をサポートする Web サービスと相互運用できるように構築されています。 相互運用性のベストプラクティスのサービス構成を簡素化するために、WCF では、<xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType>、<xref:System.ServiceModel.WSHttpBinding?displayProperty=nameWithType>、<xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType>の3つの相互運用可能なシステム指定バインディングが導入されています。 構造化情報標準 (OASIS) 標準の進歩のために組織との相互運用性を確保するために、WCF には、相互運用可能なシステム指定のバインディングとして <xref:System.ServiceModel.WS2007HttpBinding?displayProperty=nameWithType>が1つ含まれています。 メタデータの公開のために、WCF には、 [\<mexHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpbinding.md)と[\<mexHttpsBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpsbinding.md)という2つの相互運用可能なシステム指定のバインディングが含まれています。 このトピックでは、システム指定の相互運用可能なバインディングがサポートする仕様を示します。  
  
## <a name="web-services-protocols-supported-by-basichttpbinding-wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding-bindings"></a>basicHttpBinding、wsHttpBinding、ws2007HttpBinding、および wsDualHttpBinding の各バインディングでサポートされる Web サービス プロトコル  
  
### <a name="all-bindings"></a>すべてのバインディング  
 [\<basicHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)、 [\<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)、および[\<ws2007HttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/ws2007httpbinding.md)バインドでは、次のプロトコルがサポートされています。  
  
> [!NOTE]
> メタデータの公開に使用するバインディングについては、このトピックで後述する「システム指定のメタデータ バインディング」を参照してください。  
  
|カテゴリ|Protocol|仕様と用途|  
|--------------|--------------|-----------------------------|  
|トランスポート|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> `BasicHttpBinding`、`WSHttpBinding`、および `WS2007HttpBinding` は、HTTP トランスポートおよび HTTPS トランスポートを使用します。|  
|メッセージング|MTOM|[MTOM](https://www.w3.org/TR/soap12-mtom/)<br /><br /> `basicHttpBinding`、`wsHttpBinding`、および `ws2007HttpBinding` は、MTOM (Message Transmission Optimization Mechanism) をサポートしています。 既定では使用されません。 MTOM を使用するには、`messageEncoding` 属性を `"Mtom"` に設定します。<br /><br /> 例:<br /><br /> `<wsHttpBinding> <binding messageEncoding="Mtom"/> </wsHttpBinding>`|  
|メタデータ|WSDL 1.1|[WSDL 1.1](https://www.w3.org/TR/wsdl/)<br /><br /> WCF では、Web サービス記述言語 (WSDL) を使用してサービスを記述します。|  
|メタデータ|WS-Policy|[WS-POLICY](https://www.w3.org/Submission/WS-Policy/)<br /><br /> WCF では、ドメイン固有のアサーションと共に WS-POLICY 仕様を使用して、サービスの要件と機能を記述します。|  
|メタデータ|WS-Policy 1.5|[WS-POLICY 1.5](https://www.w3.org/TR/2007/CR-ws-policy-20070605/)<br /><br /> WCF では、ドメイン固有のアサーションと共に WS-POLICY 仕様を使用して、サービスの要件と機能を記述します。|  
|メタデータ|WS-PolicyAttachment|[WS-POLICY 添付ファイル](http://specs.xmlsoap.org/ws/2004/09/policy/ws-policyattachment.pdf)<br /><br /> WCF では、Web サービス記述言語 (WSDL) のさまざまなスコープでポリシー式をアタッチするために、WS-POLICY 添付ファイルが実装されています。|  
|メタデータ|WS-MetadataExchange|[Ws-metadataexchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF では、XML スキーマ、WSDL、および WS-POLICY を取得するために、Ws-metadataexchange が実装されています。|  
  
### <a name="basichttpbinding"></a>basicHttpBinding  
  
|カテゴリ|Protocol|仕様と用途|  
|--------------|--------------|-----------------------------|  
|メッセージング|SOAP 1.1|[SOAP 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)<br /><br /> Basic Profile 1.1 に従って、`basicHttpBinding` 要素は SOAP 1.1 メッセージ プロトコルを実装しています。|  
|Security|WSS SOAP Message Security 1.0|[WSS SOAP メッセージセキュリティ1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> Basic Security Profile に従って、`basicHttpBinding` 要素は、ユーザー名/パスワードおよび X.509 ベースのセキュリティを実現するために、WSS (Web Services Security) SOAP Message Security 1.0 仕様を実装しています。<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential &#124;                     "Message" .../> </binding> </basicHttpBinding>`|  
|Security|WSS SOAP Message Security UsernameToken Profile 1.0|[WSS SOAP Message Security UsernameToken Profile 1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential"> <transport clientCredentialType="Basic"/> </security> </basicHttpBinding>`|  
|Security|WSS SOAP Message Security x.509 証明書トークンプロファイル1.0|[WSS SOAP Message Security x.509 証明書トークンプロファイル1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding>   <security mode="Message"> <message clientCredentialType="Certificate"/> </security> </basicHttpBinding>`|  
  
### <a name="wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding"></a>wsHttpBinding、ws2007HttpBinding、および wsDualHttpBinding  
  
|カテゴリ|Protocol|仕様と用途|  
|--------------|--------------|-----------------------------|  
|メッセージング|[SOAP 1.2 を使用する]|[基本](https://www.w3.org/TR/soap12-part0/)<br /><br /> [メッセージングフレームワーク](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (HTTP バインディングを含む)](https://www.w3.org/TR/soap12-part2/)|  
|メッセージング|WS-ADDRESSING 2005/08|[Web サービスアドレス指定 1.0-コア](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web サービスアドレス指定 1.0-SOAP](https://www.w3.org/TR/ws-addr-soap/)<br /><br /> `wsHttpBinding`、`ws2007HttpBinding`、および `wsDualHttpBinding` は、非同期メッセージング、メッセージ相関、およびトランスポート中立のアドレス指定機構を有効にするために、W3C (World Wide Web Consortium) WS-Addressing 勧告を実装しています。<br /><br /> WCF は WS-Addressing ヘッダーの暗号化をサポートしていませんが、これは WS-* 仕様によって許可されています。|  
|メッセージング|WS-Addressing 1.0 - メタデータ|[Ws-addressing 1.0 メタデータ](https://www.w3.org/2007/05/addressing/metadata/)このプロトコルのサポートを有効にするには、ServiceMetadata の動作でポリシーのバージョンを設定します。 policyversion が 1.2 (既定値) に設定され、wsdl の説明が WS-ADDRESSING wsdl に準拠し、policyversion が1.5 に設定されている場合、wsdl の説明は ws-addressing メタデータに準拠しています。<br /><br /> WCF は WS-Addressing ヘッダーの暗号化をサポートしていませんが、これは WS-* 仕様によって許可されています。|  
|Security|WSS SOAP Message Security 1.0|[WSS SOAP メッセージセキュリティ1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> `securityMode` 属性が "wsSecurityOverHttp" (既定) に設定され、`wsSecurity` 子要素を使用してパラメーターが構成されている場合に使用します。<br /><br /> `<wsHttpBinding>   <binding name="myBinding">      <security mode="Message" .../>   </binding> </wsHttpBinding>`|  
|Security|WSS SOAP Message Security UsernameToken Profile 1.1|[WSS SOAP Message Security UsernameToken Profile 1.0](https://www.oasis-open.org/committees/download.php/16782/wss-v1.1-spec-os-UsernameTokenProfile.pdf)<br /><br /> `wsSecurity` 要素の `authenticationMode` 属性が "Username" に設定されている場合に使用します。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="UserName        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security> </binding> </wsHttpBinding>`|  
|Security|WSS SOAP Message Security X.509 Certificate Token Profile 1.1|[WSS SOAP Message Security x.509 証明書トークンプロファイル1.1](https://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf)<br /><br /> `wsSecurity` 要素の `authenticationMode` 属性が "Username"、"Certificate"、または "None" に設定されている場合に、メッセージを保護するために使用します。 また、`wsSecurity` 要素の `authenticationMode` 属性が "Certificate" に設定されている場合は、クライアント認証に使用します。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Certificate"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|Security|WSS SOAP Message Security Kerberos Token Profile 1.1|[WSS SOAP メッセージセキュリティ Kerberos トークンプロファイル1.1](https://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf)<br /><br /> `wsSecurity` 要素の `authenticationMode` 属性が "Windows" に設定されている場合に、認証とメッセージの保護に使用します。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Windows"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|Security|WS-SecureConversation|[Ws-secureconversation](http://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)<br /><br /> `security/@mode` 属性が "Message" に設定され、`message/@establishSecurityContext` 属性が "true" (既定値) に設定されている場合に、セッションをセキュリティで保護するために使用します。|  
|Security|WS-Trust|[WS-TRUST](http://specs.xmlsoap.org/ws/2005/02/trust/ws-trust.pdf)<br /><br /> WS-SecureConversation で使用されます (上記を参照)。|  
|信頼できるメッセージ機能|WS-ReliableMessaging|[WS-ReliableMessaging](http://specs.xmlsoap.org/ws/2005/02/rm/ws-reliablemessaging.pdf)<br /><br /> バインディングが `reliableSession` を使用するように構成されている場合に使用します。<br /><br /> `<wsHttpBinding>  <binding name="myBinding">    <reliableSession/>   </binding> </wsHttpBinding>`|  
|トランザクション|WS-AtomicTransaction|[WS-AtomicTransaction](http://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf)<br /><br /> トランザクション マネージャー間の通信に使用します。 WCF クライアントとサービスは、常にローカルトランザクションマネージャーを使用します。|  
|トランザクション|WS-Coordination|[WS-ATOMICTRANSACTION](https://docs.microsoft.com/previous-versions/ms951231(v=msdn.10))<br /><br /> `flowTransactions` 属性が "Allowed" または "Required" に設定されている場合に、トランザクション コンテキストをフローするために使用します。<br /><br /> `<wsHttpBinding>   <binding transactionFlow="true"/> </wsHttpBinding>`|  
  
## <a name="wsfederationhttpbinding-and-ws2007federationhttpbinding"></a>wsFederationHttpBinding および ws2007FederationHttpBinding  
 [\<wsFederationHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wsfederationhttpbinding.md)と[\<の ws2007FederationHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/ws2007federationhttpbinding.md)要素は、サードパーティがクライアントの認証に使用するトークンを発行するフェデレーションシナリオのサポートを提供するために導入されています。 `wsHttpBinding` で使用されるプロトコルに加えて、`wsFederationHttpBinding` では次のものを使用します。  
  
- トークンを発行するための `WS-Trust`  
  
- 最も一般的に発行されるトークンの形式のための WSS SAML (Security Assertions Markup Language) Token Profile 1.0 および 1.1  
  
 例:  
  
```xml  
<wsFederationHttpBinding>  
  <binding name="myBinding">  
     <security mode="Message">  
       <message issuedKeyType="Symmetric"   
                issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">  
         <issuerMetadata address =   
         'http://localhost/FederationSample/HomeRealmSTS/STS.svc/mex'>  
       </message>  
     </security>  
  </binding>  
</wsFederationHttpBinding>  
```  
  
 詳細については、「[フェデレーション](../../../../docs/framework/wcf/feature-details/federation.md)」を参照してください。  
  
## <a name="system-provided-metadata-bindings"></a>システム指定のメタデータ バインディング  
 次の表に、<xref:System.ServiceModel.Description.MetadataExchangeBindings?displayProperty=nameWithType> クラスによって公開される、システム指定の相互運用可能なメタデータ バインディングによってサポートされるプロトコルを示します。  
  
### <a name="mexhttpbinding"></a>mexHttpBinding  
 [\<mexHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpbinding.md) binding は、次のプロトコルをサポートしています。 このバインディングの使用方法の詳細については、「[メタデータの公開](../../../../docs/framework/wcf/feature-details/publishing-metadata.md)」を参照してください。  
  
|カテゴリ|Protocol|仕様と用途|  
|--------------|--------------|-----------------------------|  
|トランスポート|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)|  
|メッセージング|[SOAP 1.2 を使用する]|[基本](https://www.w3.org/TR/soap12-part0/)<br /><br /> [メッセージングフレームワーク](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (HTTP バインディングを含む)](https://www.w3.org/TR/soap12-part2/)|  
|メッセージング|WS-ADDRESSING 2005/08|[Web サービスアドレス指定 1.0-コア](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web サービスアドレス指定 1.0-SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|メタデータ|WS-MetadataExchange|[Ws-metadataexchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF では、XML スキーマ、WSDL、および WS-POLICY を取得するために、Ws-metadataexchange が実装されています。|  
  
### <a name="mexhttpsbinding"></a>mexHttpsBinding  
 [\<mexHttpsBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpsbinding.md)では、次のプロトコルがサポートされています。 このバインディングの使用方法の詳細については、「[メタデータの公開](../../../../docs/framework/wcf/feature-details/publishing-metadata.md)」を参照してください。  
  
|カテゴリ|Protocol|仕様と用途|  
|--------------|--------------|-----------------------------|  
|トランスポート|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> トランスポート セキュリティは有効です。|  
|メッセージング|[SOAP 1.2 を使用する]|[基本](https://www.w3.org/TR/soap12-part0/)<br /><br /> [メッセージングフレームワーク](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (HTTP バインディングを含む)](https://www.w3.org/TR/soap12-part2/)|  
|メッセージング|WS-ADDRESSING 2005/08|[Web サービスアドレス指定 1.0-コア](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web サービスアドレス指定 1.0-SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|メタデータ|WS-MetadataExchange|[Ws-metadataexchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF では、XML スキーマ、WSDL、および WS-POLICY を取得するために、Ws-metadataexchange が実装されています。|  
  
## <a name="see-also"></a>参照

- [システム標準のバインディング](../../../../docs/framework/wcf/system-provided-bindings.md)
- [\<basicHttpBinding>](../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)
- [\<wsHttpBinding>](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)
- [\<wsDualHttpBinding>](../../../../docs/framework/configure-apps/file-schema/wcf/wsdualhttpbinding.md)
- [\<mexHttpsBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpsbinding.md)
- [\<mexHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/mexhttpbinding.md)
