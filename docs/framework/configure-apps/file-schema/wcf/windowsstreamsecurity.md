---
title: <windowsStreamSecurity>
ms.date: 03/30/2017
ms.assetid: 926bea29-90c7-4a26-9cf0-fb4aa44f6f70
ms.openlocfilehash: dab8505a9ddb348a6f7fe16ae9acb3a0119a8b06
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73735887"
---
# <a name="windowsstreamsecurity"></a>windowsStreamSecurity > の \<
カスタム バインディングの Windows ストリーム セキュリティ設定を指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**windowsStreamSecurity >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<windowsStreamSecurity protectionLevel="None/Sign/EncryptAndSign" />
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|protectionLevel|メッセージ レベルのセキュリティを定義します。 メッセージに署名すると、転送中のメッセージが第三者によって改ざんされるリスクを軽減します。 暗号化により、転送中にデータレベルのプライバシーが提供されます。 以下の値が有効です。<br /><br /> -None: 保護がありません。<br />-Sign: メッセージは署名されています。<br />-EncryptAndSign: メッセージは署名され、暗号化されます。<br /><br /> 既定値は EncryptAndSign です。<br /><br /> この属性は <xref:System.Net.Security.ProtectionLevel> 型です。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<binding >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>Remarks  
 TCP などのストリーム指向プロトコルおよび名前付きパイプを使用するトランスポートは、ストリーム ベースのトランスポート アップグレードをサポートします。 特に、WCF にはセキュリティ アップグレードが用意されています。 このトランスポートセキュリティの構成は、この構成要素および[\<sslStreamSecurity >](sslstreamsecurity.md)によってカプセル化されます。これは、構成してカスタムバインドに追加することができます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Configuration.WindowsStreamSecurityElement>
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>
- [バインディング](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
