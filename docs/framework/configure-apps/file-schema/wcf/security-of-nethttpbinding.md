---
title: <security> の <netHttpBinding>
ms.date: 03/30/2017
ms.assetid: dc41f6f7-cabc-4a64-9fa0-ceabf861b348
ms.openlocfilehash: 97c52fa4f062ed0c65d5b1a8ca47a1439ab04cf5
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73736483"
---
# <a name="security-of-nethttpbinding"></a>\<netHttpBinding > のセキュリティ > の \<

[\<netHttpBinding >](nethttpbinding.md)のセキュリティ機能を定義します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<netHttpBinding >** ](nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**セキュリティ >**  

## <a name="syntax"></a>構文

```xml
<security mode="Message/None/Transport/TransportWithCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             realm="string" />
  <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           clientCredentialType="Certificate/IssuedToken/None/UserName/Windows" />
</security>
```

## <a name="attributes-and-elements"></a>属性と要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|モード|省略可能です。 使用されるセキュリティの種類を指定します。 既定値は、 `None`です。 この属性は <xref:System.ServiceModel.BasicHttpSecurityMode> 型です。|

## <a name="mode-attribute"></a>mode 属性

|[値]|説明|
|-----------|-----------------|
|None|-メッセージは、転送中にセキュリティ保護されません。|
|Transport|セキュリティは、HTTPS トランスポートを使用して提供されます。 SOAP メッセージは、HTTPS を使用したセキュリティで保護されます。 サービスは、サービスの X.509 証明書を使用してクライアントに認証されます。 クライアントは、提供される ClientCredentialType を使用して認証されます。|
|[メッセージ]|セキュリティは、SOAP メッセージ セキュリティを使用して確保されます。 既定では、本文は暗号化および署名されます。 このバインディングの場合、サーバー証明書をクライアントの帯域外で提供するように要求されます。 このバインディングの唯一の有効な `ClientCredentialType` は、`Certificate` です。|
|TransportWithMessageCredential|整合性、機密性、およびサーバー認証は、トランスポート セキュリティによって提供されます。 クライアント認証は、SOAP メッセージ セキュリティで提供されます。 このモードは、ユーザーがユーザー名およびパスワードを使用して認証し、メッセージ転送をセキュリティで保護するために既存の HTTP が配置されている場合に関連します。|
|TransportCredentialOnly|このモードは、メッセージの整合性と機密性を提供しません。 http ベースのクライアント認証を提供します。 このモードを使用するときは、十分に注意する必要があります。 トランスポートセキュリティが他の方法 (IPSec など) によって提供され、WCF インフラストラクチャによってクライアント認証のみが提供される環境で使用する必要があります。|

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[\<transport >](transport-of-nethttpbinding.md)|基本 HTTP サービスのトランスポート セキュリティ設定を定義します。 この要素は、<xref:System.ServiceModel.HttpTransportSecurity> に対応しています。|
|[\< メッセージ >](message-of-nethttpbinding.md)|基本 HTTP サービスのメッセージ セキュリティ設定を定義します。 この要素は、<xref:System.ServiceModel.BasicHttpMessageSecurity> に対応しています。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|バインド|[\<basicHttpBinding >](basichttpbinding.md)のバインド要素。|

## <a name="remarks"></a>Remarks

 既定では、SOAP メッセージはセキュリティで保護されず、クライアントは認証されません。 この要素を使用すると、`netHttpBinding` 要素に追加のセキュリティ設定を構成できます。

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.NetHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetHttpBindingElement.Security%2A>  
- [サービスおよびクライアントのセキュリティ保護](../../../wcf/feature-details/securing-services-and-clients.md)
- [資格情報の種類の選択](../../../wcf/feature-details/selecting-a-credential-type.md)
- [バインディング](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding >](bindings.md)
