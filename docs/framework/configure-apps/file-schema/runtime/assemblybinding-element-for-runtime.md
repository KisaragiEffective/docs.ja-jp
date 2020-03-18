---
title: <runtime> の <assemblyBinding> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding
helpviewer_keywords:
- <assemblyBinding> element
- assemblyBinding element
- container tags, <assemblyBinding> element
ms.assetid: 964cbb35-ab49-4498-8471-209689e5dada
ms.openlocfilehash: c688353583f5e452950d63b7d02c48505b6ae999
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73118129"
---
# <a name="assemblybinding-element-for-runtime"></a>\<ランタイムの assemblyBinding > 要素を \<
アセンブリ バージョンのリダイレクトおよびアセンブリの位置に関する情報が含まれます。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<assemblyBinding >**  
  
## <a name="syntax"></a>構文  
  
```xml  
      <assemblyBinding    
   xmlns="urn:schemas-microsoft-com:asm.v1" appliesTo="v1.0.3705">  
</assemblyBinding>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|**xmlns**|必須の属性です。<br /><br /> アセンブリのバインディングに必要な XML 名前空間を指定します。 値として、文字列 "urn:schemas-microsoft-com:asm.v1" を使用します。|  
|**appliesTo**|.NET Framework アセンブリのリダイレクトを適用するランタイムのバージョンを指定します。 このオプションの属性では、.NET Framework バージョン番号を使用して、適用するバージョンを指定します。 **appliesTo** 属性が指定されていない場合、 **\<assemblyBinding>** 要素は、.NET Framework のすべてのバージョンに適用されます。 **AppliesTo**属性は .NET Framework バージョン1.1 で導入されました.NET Framework バージョン1.0 では無視されます。 これは **appliesTo** 属性が指定されている場合でも、.NET Framework version 1.0 を使用している場合 **\<assemblyBinding>** のすべての要素が適用されることを意味します。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<dependentAssembly>](dependentassembly-element.md)|アセンブリのバインディング ポリシーとアセンブリの場所をカプセル化します。 アセンブリごとに1つの **\<dependentAssembly >** タグを使用します。|  
|[\<probing>](probing-element.md)|アセンブリの読み込み時に共通言語ランタイムが検索するサブディレクトリを指定します。|  
|[\<publisherPolicy>](publisherpolicy-element.md)|ランタイムが発行元ポリシーを適用するかどうかを指定します。|  
|[\<qualifyAssembly>](qualifyassembly-element.md)|部分名が使用された場合に動的に読み込む必要があるアセンブリの完全名を指定します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="example"></a>例  
 あるアセンブリ バージョンを別のバージョンにリダイレクトし、コードベースを提供する例を示します。  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
            <codeBase version="2.0.0.0"  
                      href="http://www.litwareinc.com/myAssembly.dll"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
 次の例は、 **appliesTo**属性を使用して .NET Framework アセンブリのバインドをリダイレクトする方法を示しています。  
  
```xml  
<runtime>  
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1" appliesTo="v1.0.3705">  
      <dependentAssembly>   
         <assemblyIdentity name="mscorcfg" publicKeyToken="b03f5f7f11d50a3a" culture=""/>  
         <bindingRedirect oldVersion="0.0.0.0-65535.65535.65535.65535" newVersion="1.0.3300.0"/>  
      </dependentAssembly>  
   </assemblyBinding>  
</runtime>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [アセンブリ バージョンのリダイレクト](../../redirect-assembly-versions.md)
