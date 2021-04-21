---
description: '詳細情報: <netTcpBinding> の <security>'
title: <security> の <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 286cd191-4fd5-4c4e-a223-9c71cf7fdead
ms.openlocfilehash: ad924f5a6ea9e003f6427ee76d3aef3afde9d083
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99683074"
---
# <a name="security-of-nettcpbinding"></a>\<security> の \<netTcpBinding>

バインディングのセキュリティ設定を定義します。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netTcpBinding>**](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>構文  
  
```xml  
<security mode="Message/None/Transport/TransportWithCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             protectionLevel="None/Sign/EncryptAndSign" />
  <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           clientCredentialType="Certificate/IssuedToken/None/UserName/Windows" />
</security>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  

 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|mode|任意。 適用するセキュリティの種類を指定します。 有効な値を次に示します。 既定値は `Transport` です。<br /><br /> この属性は <xref:System.ServiceModel.SecurityMode> 型です。|  
  
## <a name="mode-attribute"></a>mode 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|なし|セキュリティを無効にします。|  
|トランスポート|トランスポート セキュリティは、TCP 経由の TLS または SPNaego を使用して提供されます。 サービスは、SSL 証明書を使用して設定する必要があります。 このモードでは保護レベルを制御することができます。|  
|メッセージ|セキュリティは、SOAP メッセージ セキュリティを使用して確保されます。 既定では、SOAP 本文は暗号化および署名されます。 このモードは、サービス資格情報をクライアントの帯域外で使用可能にするかどうか、使用するアルゴリズム スイート、メッセージ本文に適用する保護レベルなど、さまざまな機能を提供します。 クライアント認証はセッションごとに 1 回実行され、認証の結果はセッションの存続中にキャッシュされます。|  
|TransportWithMessageCredential|トランスポート セキュリティは、メッセージ セキュリティと組み合わせて使用されます。 トランスポート セキュリティは、TCP 経由の TLS または SPNego によって提供され、整合性、機密性、およびサーバー認証が保証されます。 SOAP メッセージ セキュリティは、クライアント認証を提供します。 既定では、クライアント認証はセッションごとに 1 回実行され、認証の結果はセッションの存続中にキャッシュされます。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<transport>](transport-of-nettcpbinding.md)|トランスポートのセキュリティ設定を定義します。 この要素は <xref:System.ServiceModel.Configuration.TcpTransportSecurityElement> 型です。|  
|[\<message>](message-element-of-nettcpbinding.md)|メッセージのセキュリティ設定を定義します。 この要素は <xref:System.ServiceModel.Configuration.MessageSecurityOverTcpElement> 型です。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|binding|[\<netTcpBinding>](nettcpbinding.md) のバインディング要素です。|  
  
## <a name="remarks"></a>解説  

 標準バインディングにはそれぞれ、転送セキュリティ要件を制御するためのパラメーターが用意されています。 通常、これらのパラメーターには、使用されるセキュリティ レベル (メッセージ レベルまたはトランスポート レベル) を指定したセキュリティ モードと、クライアント資格情報の種類が含まれています。 これらのパラメーターが提示するオプションの選択に基づいて、適切なセキュリティが設定されたチャネル スタックが構築されます。  
  
 WCF (Windows Communication Foundation) に用意されているシステム標準のバインディグは、最も一般的なシナリオ要件の一部を満たすように設計されたセットです。 これらのバイディングでは、特定のシナリオを対象とするセキュリティ要件を指定できます。  
  
 この構成要素によって、`netTcpBinding` のセキュリティ仕様が提供されます。 これは、コンピューター間通信に適した、安全で信頼できる最適化されたバインディングです。 既定では、このバインディングは、メッセージを配信するための TCP、メッセージの保護と認証を行うための Windows セキュリティ、信頼性を高めるための WS-ReliableMessaging、およびバイナリ メッセージのエンコーディングをサポートするランタイム通信スタックを生成します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.NetTcpSecurity>
- <xref:System.ServiceModel.NetTcpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetTcpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [サービスおよびクライアントのセキュリティ保護](../../../wcf/feature-details/securing-services-and-clients.md)
- [バインド](../../../wcf/bindings.md)
- [システムが提供するバインディングの構成](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [サービスとクライアントを構成するためのバインディングの使用](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
