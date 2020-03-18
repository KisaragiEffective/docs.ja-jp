---
title: <transport> の <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 49462e0a-66e1-463f-b3e1-c83a441673c6
ms.openlocfilehash: 4ef08ad73a03dea21d27217364a7bacb46a3848e
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73735935"
---
# <a name="transport-of-nettcpbinding"></a>\<netTcpBinding> の \<transport>
[\<netTcpBinding >](nettcpbinding.md)で構成されるエンドポイントのメッセージレベルのセキュリティ要件の種類を定義します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<netTcpBinding >** ](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**セキュリティ >** ](security-of-nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**トランスポート >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<netTcpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential">
      <transport clientCredentialType="None|Windows|Certificate"
                 protectionLevel="None|Sign|EncryptAndSign"
                 sslProtocols="Tls|Tls11|Tls12">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|clientCredentialType|省略可能です。 トランスポート セキュリティを使用してクライアント認証を実行するときに使用される資格情報の種類を指定します。<br /><br /> -既定値は `Windows`です。<br />-この属性の型は <xref:System.ServiceModel.TcpClientCredentialType>です。|  
|protectionLevel|省略可能です。 TCP トランスポートのレベルでセキュリティを定義します。 メッセージに署名すると、転送中のメッセージが第三者によって改ざんされるリスクを軽減します。 暗号化により、転送中にデータレベルのプライバシーが提供されます。<br /><br /> 既定値は `EncryptAndSign`です。|  
|sslProtocols|どの SslProtocols がサポートされているのかを指定する SslProtocols 列挙型フラグの値。 既定値は Tls&#124;Tls11&#124;Tls12 です。|  
|policyEnforcement|この列挙体は、<xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> を適用するタイミングを指定します。<br /><br /> 1. never –ポリシーは適用されません (拡張保護は無効になります)。<br />2. WhenSupported –ポリシーは、クライアントが拡張保護をサポートしている場合にのみ適用されます。<br />3. always –ポリシーは常に適用されます。 拡張保護をサポートしていないクライアントは認証に失敗します。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|None|クライアントは匿名です。 これには、サービスの証明書が必要です。|  
|Windows|SP ネゴシエーション (Kerberos ネゴシエーション) を使用して、クライアントの Windows 認証を指定します。|  
|証明書|クライアントは、証明書を使用して認証されます。 これは SSL ネゴシエーションを使用し、サービスの証明書が必要です。|  
  
## <a name="protectionlevel-attribute"></a>protectionLevel 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|None|保護されません。|  
|Sign|メッセージは署名されます。|  
|EncryptAndSign|-メッセージは暗号化され、署名されます。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\< セキュリティ >](security-of-nettcpbinding.md)|[\<netTcpBinding >](nettcpbinding.md)のセキュリティ機能を指定します。|  
  
## <a name="remarks"></a>Remarks  
 トランスポート セキュリティは、SOAP メッセージの整合性と機密性の保護、および相互認証に使用します。 このセキュリティ モードがバインディング上で選択された場合、チャネル スタックはセキュリティ トランスポートを使用して構成され、SOAP メッセージは Windows (ネゴシエート) や TCP 上の SSL などのトランスポート セキュリティを使用して保護されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.TcpTransportSecurity>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetTcpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [サービスおよびクライアントのセキュリティ保護](../../../wcf/feature-details/securing-services-and-clients.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding >](bindings.md)
