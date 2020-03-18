---
title: <supportPortability> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- supportPortability element
- <supportPortability> element
ms.assetid: 6453ef66-19b4-41f3-b712-52d0c2abc9ca
ms.openlocfilehash: 63c309a8a93c1d31ed8f73a495cf5154c3590d56
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73115653"
---
# <a name="supportportability-element"></a>> 要素の \<supportPortability
.NET Framework の 2 つの異なる実装にある同じアセンブリを 1 つのアプリケーションから参照できるように、既定の動作を無効にすることができます。既定の動作では、アプリケーションの移植性を高めるために、このようなアセンブリは同等のものとして扱われます。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp; &nbsp; &nbsp; &nbsp;[ **\<assemblyBinding**](assemblybinding-element-for-runtime.md) > \
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; **\<supportPortability >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<supportPortability PKT="public_key_token" enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  

以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|PKT|必須の属性です。<br /><br /> 対象となるアセンブリの公開キー トークンを文字列で指定します。|  
|enabled|省略可能な属性です。<br /><br /> 指定した .NET Framework アセンブリの複数の実装間での移植性に対するサポートを有効にするかどうかを指定します。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|TRUE|指定した .NET Framework アセンブリの複数の実装間での移植性に対するサポートを有効にします。 既定値です。|  
|False|指定した .NET Framework アセンブリの複数の実装間での移植性に対するサポートを無効にします。 この場合、指定したアセンブリの複数の実装をアプリケーションで参照できます。|  
  
### <a name="child-elements"></a>子要素  

なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
|`assemblyBinding`|アセンブリ バージョンのリダイレクトおよびアセンブリの位置に関する情報が含まれます。|  
  
## <a name="remarks"></a>Remarks  

.NET Framework 4 以降、.NET Framework の2つの実装のいずれかを使用できるアプリケーションに対して、サポートが自動的に提供されます。たとえば、.NET Framework の実装や、Silverlight の実装のための .NET Framework です。 特定の .NET Framework アセンブリの 2 つの実装は、アセンブリ バインダーで同等と見なされます。 一部のシナリオでは、アプリケーションの移植性を高めるためのこの機能が問題になります。 そのようなシナリオでは、`<supportPortability>` 要素を使用して、この機能を無効にすることができます。  
  
一例として、特定の参照アセンブリの .NET Framework の実装と .NET Framework for Silverlight の実装の両方を参照する必要があるアセンブリが挙げられます。 たとえば、WPF (Windows Presentation Foundation) で作成された XAML デザイナーにおいて、デザイナーのユーザー インターフェイスとして WPF デスクトップの実装を参照すると共に、Silverlight の実装に組み込まれている WPF のサブセットも参照する必要がある場合があります。 既定では、この 2 つのアセンブリはアセンブリ バインディングで同等と見なされるため、別々に参照するとコンパイラ エラーが発生します。 この要素を使用すると、既定の動作を無効にし、コンパイルを正常に実行できます。  
  
> [!IMPORTANT]
> コンパイラから共通言語ランタイムのアセンブリ バインディング ロジックに情報を渡すには、`/appconfig` コンパイラ オプションを使用して、この要素を含む app.config ファイルの場所を指定する必要があります。  
  
## <a name="example"></a>例  

.NET Framework と .NET Framework for Silverlight の両方の実装に存在する .NET Framework アセンブリについて、その両方の実装をアプリケーションで参照できるようにする例を次に示します。 `/appconfig` コンパイラ オプションを使用して、この app.config ファイルの場所を指定する必要があります。  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding>  
         <supportPortability PKT="7cec85d7bea7798e" enable="false"/>  
         <supportPortability PKT="31bf3856ad364e35" enable="false"/>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [-appconfig (C# コンパイラ オプション)](../../../../csharp/language-reference/compiler-options/appconfig-compiler-option.md)
- [.NET Framework アセンブリの統一の概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/db7849ey(v=vs.100))
