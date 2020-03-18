---
title: <add>
ms.date: 03/30/2017
ms.assetid: 4712a888-f154-4395-8887-ef14a88a6497
author: BrucePerlerMS
ms.openlocfilehash: 83ba51cbbd5100bf7412f9914a270cac88f7faa1
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73973802"
---
# <a name="add"></a>\<add>
指定されたセキュリティトークンハンドラーをトークンハンドラーコレクションに追加します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<** ](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<の構成 >** ](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**securityTokenHandlers >** ](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**追加 >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <add type=xs:string>  
        <optionalConfigurationElement>  
        </optionalConfigurationElement>  
      </add>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|type|追加するトークンハンドラーの CLR 型名。 `type` 属性を指定する方法の詳細については、「[カスタム型参照](https://docs.microsoft.com/previous-versions/windows-identity-foundation/gg638728(v=msdn.10)#custom-type-references)」を参照してください。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<samlSecurityTokenRequirement >](samlsecuritytokenrequirement.md)|<xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> クラス、<xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> クラス、またはこれらのクラスのいずれかの派生クラスの構成を提供します。|  
|[\<sessionTokenRequirement >](sessiontokenrequirement.md)|<xref:System.IdentityModel.Tokens.SessionSecurityTokenHandler> クラスまたは派生クラスの構成を提供します。|  
|[\<userNameSecurityTokenHandlerRequirement >](usernamesecuritytokenhandlerrequirement.md)|<xref:System.IdentityModel.Services.Tokens.MembershipUserNameSecurityTokenHandler> クラスまたは派生クラスの構成を提供します。|  
|[\<x509SecurityTokenHandlerRequirement >](x509securitytokenhandlerrequirement.md)|<xref:System.IdentityModel.Tokens.X509SecurityTokenHandler> クラスまたは派生クラスのオプションの構成を提供します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<securityTokenHandlers >](securitytokenhandlers.md)|エンドポイントに登録されているセキュリティトークンハンドラーのコレクションを指定します。|  
  
## <a name="remarks"></a>Remarks  
 `<add>` 要素は、トークンハンドラーの構成を指定する1つの子要素を受け取ることができます。 これは、`<add>` 要素の `type` 属性を通じて参照されるハンドラークラスがこの機能をサポートするかどうかによって異なります。 この機能を提供するトークンハンドラークラスは、<xref:System.Xml.XmlElement> オブジェクトを受け取るコンストラクターを公開する必要があります。  

```csharp  
public class CustomTokenHandler : Microsoft.IdentityModel.Tokens.SecurityTokenHandler  
{  
    public CustomTokenHandler( XmlElement customConfig )  
    {  
    }  
}  
```  
  
 いくつかの組み込みセキュリティトークンハンドラークラスは、この機能を提供します。 これらのクラスは、<xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler>、<xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler>、<xref:System.IdentityModel.Services.Tokens.MembershipUserNameSecurityTokenHandler>、<xref:System.IdentityModel.Tokens.X509SecurityTokenHandler>、および <xref:System.IdentityModel.Tokens.SessionSecurityTokenHandler>です。  
  
> [!IMPORTANT]
> トークンハンドラーコレクションには、指定された任意の型の1つのハンドラーのみを含めることができます。 これは、たとえば、<xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> クラスから派生したハンドラーをコレクションに追加する場合は、まず、既定で存在する <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler>をコレクションから削除する必要があることを意味します。 [\<remove >](remove.md)要素を使用してコレクションから1つのハンドラーを削除することも、 [\<clear >](clear.md)要素を使用してコレクションからすべてのハンドラーを削除することもできます。  
  
 ハンドラーに対して指定された設定は、 [\<securityTokenHandlerConfiguration >](securitytokenhandlerconfiguration.md)要素の下にあるトークンハンドラーコレクションで指定された同等の設定をオーバーライドし、 [\<ユーザー構成 >](identityconfiguration.md)要素の下のサービスレベルで指定されます。  
  
## <a name="example"></a>例  
 次の XML は、`<add>` と `<remove>` の要素を使用して、既定のセッショントークンハンドラーをカスタムセッショントークンハンドラーに置き換える方法を示しています。 XML は `ClaimsAwareWebFarm` サンプルから抜粋したものです。  
  
```xml  
<securityTokenHandlers>  
  <remove type="System.IdentityModel.Tokens.SessionSecurityTokenHandler, System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
  <add type="System.IdentityModel.Services.Tokens.MachineKeySessionSecurityTokenHandler, System.IdentityModel.Services, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
</securityTokenHandlers>  
```
