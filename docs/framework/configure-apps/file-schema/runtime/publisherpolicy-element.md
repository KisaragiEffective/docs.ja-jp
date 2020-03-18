---
title: <publisherPolicy> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/publisherPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/publisherPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#publisherPolicy
helpviewer_keywords:
- publisherPolicy element
- container tags, <publisherPolicy> element
- <publisherPolicy> element
ms.assetid: 4613407e-d0a8-4ef2-9f81-a6acb9fdc7d4
ms.openlocfilehash: 89fa8a991cc7d0352eb0a13cdfd3a6063ea468e7
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73115843"
---
# <a name="publisherpolicy-element"></a>\<Publisherpolicy apply > 要素
ランタイムが発行元ポリシーを適用するかどうかを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp; &nbsp; &nbsp; &nbsp;[ **\<assemblyBinding**](assemblybinding-element-for-runtime.md) > \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<dependentAssembly >** ](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**publisherpolicy apply >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<publisherPolicy apply="yes|no"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`apply`|発行者ポリシーを適用するかどうかを指定します。|  
  
## <a name="apply-attribute"></a>属性の適用  
  
|[値]|説明|  
|-----------|-----------------|  
|`yes`|発行者ポリシーを適用します。 これは、既定の設定です。|  
|`no`|発行者ポリシーは適用されません。|  
  
### <a name="child-elements"></a>子要素  

なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`assemblyBinding`|アセンブリ バージョンのリダイレクトおよびアセンブリの位置に関する情報が含まれます。|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`dependentAssembly`|各アセンブリのバインディング ポリシーとアセンブリの場所をカプセル化します。 アセンブリごとに1つの `<dependentAssembly>` 要素を使用します。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>Remarks  
 コンポーネントベンダーがアセンブリの新しいバージョンをリリースする場合、ベンダーには発行者ポリシーを含めることができるため、以前のバージョンを使用するアプリケーションでは新しいバージョンが使用されるようになりました。 特定のアセンブリに対して発行者ポリシーを適用するかどうかを指定するには、 **\<publisherpolicy apply >** 要素を **\<dependentAssembly >** 要素に配置します。  
  
 **適用**属性の既定の設定は **[はい]** です。 **適用**属性を [**いいえ** **]** に設定すると、アセンブリの以前のすべての設定がオーバーライドされます。  
  
 アプリケーションがアプリケーション構成ファイルの[\<publisherpolicy apply apply = "no"/>](publisherpolicy-element.md)要素を使用して発行者ポリシーを明示的に無視するためのアクセス許可が必要です。 このアクセス許可は、<xref:System.Security.Permissions.SecurityPermission>に <xref:System.Security.Permissions.SecurityPermissionFlag> フラグを設定することによって付与されます。 詳細については、「[アセンブリバインディングリダイレクトのセキュリティアクセス許可](../../assembly-binding-redirection-security-permission.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、アセンブリの発行者ポリシーをオフにし、`myAssembly`します。  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                                    publicKeyToken="32ab4ba45e0a69a1"  
                                    culture="neutral" />  
            <publisherPolicy apply="no"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [ランタイムがアセンブリを検索する方法](../../../deployment/how-the-runtime-locates-assemblies.md)
- [アセンブリ バージョンのリダイレクト](../../redirect-assembly-versions.md)
