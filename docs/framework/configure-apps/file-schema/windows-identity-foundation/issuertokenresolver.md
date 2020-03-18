---
title: <issuerTokenResolver>
ms.date: 03/30/2017
ms.assetid: f74392f6-3f5b-4880-bd8a-3a9130d31e65
author: BrucePerlerMS
ms.openlocfilehash: 451750a1facd9a886b53427a8d54580d1a939fa5
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968505"
---
# <a name="issuertokenresolver"></a>\<issuerTokenResolver >
トークンハンドラーコレクションのハンドラーによって使用される発行者トークンリゾルバーを登録します。 発行者トークンリゾルバーは、受信トークンとメッセージの署名トークンを解決するために使用されます。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<** ](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<の構成 >** ](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**securityTokenHandlers >** ](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<securityTokenHandlerConfiguration >** ](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**issuerTokenResolver >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <issuerTokenResolver type=xs:string>  
        </issuerTokenResolver>  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|type|発行者トークンリゾルバーの種類を指定します。 <xref:System.IdentityModel.Tokens.IssuerTokenResolver> クラスであるか、または <xref:System.IdentityModel.Tokens.IssuerTokenResolver> クラスから派生した型である必要があります。 必須です。|  
  
### <a name="child-elements"></a>子要素  
 None  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration >](securitytokenhandlerconfiguration.md)|セキュリティトークンハンドラーのコレクションの構成を提供します。|  
  
## <a name="remarks"></a>Remarks  
 発行者トークンリゾルバーは、受信トークンとメッセージの署名トークンを解決するために使用されます。 署名の確認に使用される暗号化マテリアルを取得するために使用されます。 `type` 属性を指定する必要があります。 指定する型は <xref:System.IdentityModel.Tokens.IssuerTokenResolver> か、または <xref:System.IdentityModel.Tokens.IssuerTokenResolver> クラスから派生したカスタム型にすることができます。  
  
 一部のトークンハンドラーでは、構成で発行者トークンリゾルバーの設定を指定できます。 個々のトークンハンドラーの設定は、セキュリティトークンハンドラーコレクションで指定された設定よりも優先されます。  
  
> [!NOTE]
> `<issuerTokenResolver>` 要素を\<の [identity [configuration >](identityconfiguration.md)要素の子要素として指定することは非推奨とされていますが、旧バージョンとの互換性のために引き続きサポートされています。 `<securityTokenHandlerConfiguration>` 要素の設定は、`<identityConfiguration>` 要素の設定よりも優先されます。  
  
## <a name="example"></a>例  
 次の XML は、<xref:System.IdentityModel.Tokens.IssuerTokenResolver>から派生したカスタムクラスに基づく発行者トークンリゾルバーの構成を示しています。 トークンリゾルバーは、クラスに対して定義されたカスタム構成要素 (`<AddAudienceKeyPair>`) から初期化される、ユーザーとキーのペアのディクショナリを保持します。 クラスは、<xref:System.IdentityModel.Selectors.SecurityTokenResolver.LoadCustomConfiguration%2A> メソッドをオーバーライドして、この要素を処理します。 オーバーライドを次の例に示します。ただし、簡潔にするために呼び出すメソッドは表示されません。 完全な例については、`CustomToken` のサンプルを参照してください。  
  
```xml  
<issuerTokenResolver type="SimpleWebToken.CustomIssuerTokenResolver, SimpleWebToken">  
  <AddAudienceKeyPair  symmetricKey="wAVkldQiFypTQ+kdNdGWCYCHRcee8XmXxOvgmak8vSY=" audience="http://localhost:19851/" />  
</issuerTokenResolver>  
```  
  
## <a name="example"></a>例
  
```csharp
public override void LoadCustomConfiguration(System.Xml.XmlNodeList nodelist)  
{  
    foreach (XmlNode node in nodelist)  
    {  
        XmlDictionaryReader rdr = XmlDictionaryReader.CreateDictionaryReader(new XmlTextReader(new StringReader(node.OuterXml)));  
        rdr.MoveToContent();  
  
        string symmetricKey = rdr.GetAttribute("symmetricKey");  
        string audience = rdr.GetAttribute("audience");  
  
        this.AddAudienceKeyPair(audience, symmetricKey);  
    }  
}  
``` 
  
## <a name="see-also"></a>関連項目

- <xref:System.IdentityModel.Tokens.IssuerTokenResolver>
