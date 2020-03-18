---
title: <dns>
ms.date: 03/30/2017
ms.assetid: 81819dae-4825-43b7-bccd-f16d2d3d2f06
ms.openlocfilehash: e49a564c9793b371425b2b787586bb9d3cbed58b
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452228"
---
# <a name="dns"></a>dns > の \<
予想されるサーバーの ID を指定します。 この ID は、同じ値を持つ DNS がサーバーの証明書に含まれていれば、X509 証明書の認証モードで有効です。 さらに、SPN に同じ値があれば、Windows 認証モードでも有効です。  
  
要素の値の設定の詳細については、「[サービス id と認証](../../../wcf/feature-details/service-identity-and-authentication.md)」を参照してください。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<クライアント**](client.md)>\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**エンドポイント >** ](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**id >** ](identity.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**dns\<**  
  
## <a name="syntax"></a>構文  
  
```xml  
<dns value = "String" />
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|value|証明書の DNS です。 DNS は、IP ベースのネットワーク上でコンピューターを特定するために使用される業界標準のプロトコルです。 ユーザーは `https://go.microsoft.com/fwlink/?prd=10929` や `https://go.microsoft.com/fwlink/?LinkID=96165`などの表示名を、207.46.131.137 などの数字ベースのアドレスよりも覚えやすくすることができます。|  
  
### <a name="child-elements"></a>子要素  
 [なし]。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<id >](identity.md)|クライアントで認証するサービスの ID を指定します。|  
  
## <a name="example"></a>例  
 次の構成コードは、サーバーの認証に使用される X.509 証明書の DNS を指定します。  
  
```xml  
<identity>
  <dns value = "www.cohowinery.com" />
</identity>
```  
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- <xref:System.ServiceModel.DnsEndpointIdentity>
- [サービス ID と認証](../../../wcf/feature-details/service-identity-and-authentication.md)
- [\<id >](identity.md)
