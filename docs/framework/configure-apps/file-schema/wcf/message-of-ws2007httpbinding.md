---
title: <message> の <ws2007HttpBinding>
ms.date: 03/30/2017
ms.assetid: 9ffd8db6-84a8-4b38-a9fe-2cb1a87a1c97
ms.openlocfilehash: 3396f74f76d790759f4c32de2907607486701b1a
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738948"
---
# <a name="message-of-ws2007httpbinding"></a>\<ws2007HttpBinding > の \<メッセージ >
[\<ws2007HttpBinding >](ws2007httpbinding.md)要素のメッセージレベルのセキュリティの設定を定義します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<ws2007HttpBinding >** ](ws2007httpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**セキュリティ >** ](security-of-ws2007httpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**メッセージ >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<ws2007HttpBinding>
  <binding>
    <security>
      <message clientCredentialType="None/Windows/UserName/Certificate/IssuedToken"
               establishSecurityContext="Boolean"
               negotiateServiceCredential="Boolean"
               algorithmSuite="Enumeration. See algorithmSuite Attribute below."
               defaultProtectionLevel="None/Sign/EncryptionAndSign" />
    </security>
  </binding>
</ws2007HttpBinding>
```  
  
## <a name="type"></a>[種類]  
 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp>  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`algorithmSuite`|メッセージの暗号化とキー ラップ アルゴリズムを設定します。 アルゴリズムとキー サイズは、<xref:System.ServiceModel.Security.SecurityAlgorithmSuite> クラスにより決まります。 これらのアルゴリズムは、Security Policy Language (WS-SecurityPolicy) の仕様で指定されているアルゴリズムに対応付けられています。<br /><br /> 既定値は Basic256 です。|  
|`clientCredentialType`|省略可能です。 セキュリティ モード `Message` または `TransportWithMessageCredentials` を使用してクライアント認証を実行するときに使用される資格情報の種類を指定します。 次の表の列挙値を参照してください。 既定は Windows です。<br /><br /> この属性は <xref:System.ServiceModel.MessageCredentialType> 型です。|  
|`establishSecurityContext`|セキュリティで保護されたセッションをセキュリティ チャネルが確立するかどうかを示す値。 セキュリティで保護されたセッションは、セキュリティ コンテキスト トークン (SCT) を確立してからアプリケーション メッセージを交換します。 SCT が確立されると、セキュリティ チャネルは上位のチャネルに <xref:System.ServiceModel.Channels.ISession> インターフェイスを提供します。 セキュリティで保護されたセッションの詳細については、「[方法: セキュリティで保護さ](../../../wcf/feature-details/how-to-create-a-secure-session.md)れたセッションを作成する」を参照してください。<br /><br /> 既定値は `true`です。|  
|`negotiateServiceCredential`|省略可能です。 サービス資格情報がクライアントの帯域外で提供されるか、ネゴシエーションのプロセスによってサービスからクライアントに取得されるかを指定する値。 そのようなネゴシエーションは、通常のメッセージ交換の準備です。<br /><br /> `clientCredentialType` 属性が None、Username、または Certificate に等しい場合、この属性を `false` に設定することは、サービス証明書がクライアントの帯域外で利用可能であり、クライアントがサービス証明書を指定する必要があることを意味します (を使用し\<て)。 [serviceCertificate >](servicecertificate-of-servicecredentials.md)) を[\<serviceCredentials >](servicecredentials.md)サービスの動作にします。 このモードは、WS-Trust および WS-SecureConversation が実装された SOAP スタックと同時に使用できます。<br /><br /> `ClientCredentialType` 属性が `Windows` に設定されている場合、この属性を `false` に設定すると Kerberos ベースの認証が指定されます。 これは、クライアントおよびサービスが同じ Kerberos ドメインの一部でなければならないことを意味します。 このモードは、Kerberos トークン プロファイル (OASIS WSS TC で定義) に加えて WS-Trust および WS-SecureConversation が実装された SOAP スタックと同時使用できます。<br /><br /> この属性が `true` の場合、SOAP メッセージを介して <xref:System.ServiceModel.Security.Tokens.ServiceModelSecurityTokenTypes.Spnego%2A> 交換をトンネル化する .NET SOAP ネゴシエーションが行われます。<br /><br /> 既定値は、 `true`です。|  
  
## <a name="algorithmsuite-attribute"></a>algorithmSuite 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|Basic128|Aes128 暗号化を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic192|Aes192 暗号化を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic256|Aes256 暗号化を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic256Rsa15|メッセージの暗号化には Aes256 を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa15 を使用します。|  
|Basic192Rsa15|メッセージの暗号化には Aes192 を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa15 を使用します。|  
|TripleDes|TripleDes 暗号化を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic128Rsa15|メッセージの暗号化には Aes128 を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa15 を使用します。|  
|TripleDesRsa15|TripleDes 暗号化を使用し、メッセージ ダイジェストには Sha1 を、キー ラップには Rsa15 を使用します。|  
|Basic128Sha256|メッセージの暗号化には Aes256 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic192Sha256|メッセージの暗号化には Aes192 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic256Sha256|メッセージの暗号化には Aes256 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|TripleDesSha256|メッセージの暗号化には TripleDes を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa-oaep-mgf1p を使用します。|  
|Basic128Sha256Rsa15|メッセージの暗号化には Aes128 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa15 を使用します。|  
|Basic192Sha256Rsa15|メッセージの暗号化には Aes192 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa15 を使用します。|  
|Basic256Sha256Rsa15|メッセージの暗号化には Aes256 を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa15 を使用します。|  
|TripleDesSha256Rsa15|メッセージの暗号化には TripleDes を使用し、メッセージ ダイジェストには Sha256 を、キー ラップには Rsa15 を使用します。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|`None`|サービスが匿名クライアントと対話できるようになります。 サービス側では、サービスがクライアントの資格情報を必要としないことを示しています。 クライアント側では、クライアントがクライアントの資格情報を提示しないことを示しています。|  
|`Certificate`|証明書を使用したクライアントの認証を、サービスで要求することが可能になります。 `message` セキュリティ モードが使用されており、`negotiateServiceCredential` 属性が `false` に設定されている場合、クライアントをサービス証明書で準備する必要があります。|  
|`IssuedToken`|通常はセキュリティ トークン サービス (STS) により発行されるカスタム トークンを指定します。|  
|`UserName`|`UserName` 資格情報を使用したクライアントの認証を、サービスで要求することが可能になります。 WCF では、パスワードダイジェストの送信や、パスワードを使用したキーの派生、およびメッセージセキュリティのためのこのようなキーの使用はサポートされていません。 そのため、`UserName` の資格情報を使用する場合、WCF はトランスポートがセキュリティで保護されることを強制します。 この資格情報モードは、`negotiateServiceCredential` 属性に基づいて、同時実行可能な交換か、同時実行できないネゴシエーションのいずれかになります。|  
|`Windows`|SOAP 交換を、`Windows` 資格情報の認証されたコンテキストで行うことが可能になります。 `negotiateServiceCredential` 属性が `true` に設定されている場合、SSPI ネゴシエーションと Kerberos (同時使用可能な標準) のいずれかが実行されます。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\< セキュリティ >](security-of-ws2007httpbinding.md)|[\<ws2007HttpBinding >](ws2007httpbinding.md)のセキュリティ設定を定義します。|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.NonDualMessageSecurityOverHttp>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement.Message%2A>
- <xref:System.ServiceModel.WSHttpSecurity.Message%2A>
- <xref:System.ServiceModel.Configuration.NonDualMessageSecurityOverHttpElement>
- [サービスおよびクライアントのセキュリティ保護](../../../wcf/feature-details/securing-services-and-clients.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding >](bindings.md)
