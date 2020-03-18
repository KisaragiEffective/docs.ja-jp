---
title: <providerOption> 要素
ms.date: 03/30/2017
f1_keywords:
- provideroption
helpviewer_keywords:
- <provideroption> element
- providerOptions
- provideroption element
ms.assetid: 014f2e0b-c0b5-4fc4-92d3-73f02978b2a1
ms.openlocfilehash: a6718173e84ecffc4ba0641f6e865e777aa6b1a4
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74088671"
---
# <a name="provideroption-element"></a>\<providerOption > 要素
言語プロバイダーのコンパイラバージョン属性を指定します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<システムの >** ](system-codedom-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<コンパイラ**](compilers-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**コンパイラ >** ](compiler-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**providerOption >**

## <a name="syntax"></a>構文  
  
```xml  
<providerOption  
  name="option-name"  
  value="option-value"  
/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`name`|必須の属性です。<br /><br /> オプションの名前を指定します。たとえば、"CompilerVersion" のようになります。|  
|`value`|必須の属性です。<br /><br /> オプションの値を指定します。たとえば、"v 3.5" のようになります。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<configuration> 要素](../configuration-element.md)|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|[\<システムの codedom > 要素](system-codedom-element.md)|使用可能な言語プロバイダーのコンパイラ構成設定を指定します。|  
|[\<コンパイラ > 要素](compilers-element.md)|コンパイラ構成要素のコンテナー。0個以上の `<compiler>` 要素が含まれています。|  
|[\<compiler> 要素](compiler-element.md)|言語プロバイダーのコンパイラ構成属性を指定します。|  
  
## <a name="remarks"></a>Remarks  
 .NET Framework バージョン3.5 では、Code Document Object Model (CodeDOM) コードプロバイダーは `<providerOption>` 要素を使用してプロバイダー固有のオプションをサポートできます。  
  
 .NET Framework 3.5 には、更新された .NET Framework 2.0 アセンブリが含まれており、新しい型を含む新しいバージョンの3.5 アセンブリが用意されています。 Microsoft C#および Visual Basic コードプロバイダーは .NET Framework 2.0 アセンブリに含まれていますが、バージョン3.5 コンパイラをサポートするように更新されています。 既定では、更新されたコードプロバイダーはバージョン2.0 コンパイラのコードを生成します。 `<providerOption>` 要素を使用して、ターゲットコンパイラのバージョンを3.5 に変更できます。 これを行うには、`name` 属性に "CompilerVersion" を指定し、`value` 属性に「v 3.5」を指定します。 バージョン番号の前には、小文字の "v" を使用する必要があります。  
  
 .NET Framework 2.0 machine.config またはルートの web.config ファイルに `<providerOption>` 要素を追加することで、バージョン指定をグローバルにすることができます。 Machine.config ファイルで既定のコンパイラのバージョンを3.5 に更新した場合は、アプリケーション構成ファイルの `<providerOption>` 要素を使用して、アプリケーションごとに2.0 に戻すことができます。  
  
 CodeDOM コードプロバイダーの実装者は、<xref:System.Collections.Generic.IDictionary%602>型の `providerOptions` パラメーターを受け取るコンストラクターを指定することによって、カスタムオプションを処理できます。  
  
## <a name="example"></a>例  
 次の例は、 C#コードプロバイダーのバージョン3.5 を使用するように指定する方法を示しています。  
  
```xml  
<configuration>  
  <system.codedom>  
    <compilers>  
      <!-- zero or more compiler elements -->  
      <compiler  
        language="c#;cs;csharp"  
        extension=".cs"  
        type="Microsoft.CSharp.CSharpCodeProvider, System,   
          Version=2.0.3600.0, Culture=neutral,   
          PublicKeyToken=b77a5c561934e089"  
        compilerOptions="/optimize"  
        warningLevel="1" >  
          <providerOption  
            name="CompilerVersion"  
            value="v3.5" />  
      </compiler>  
    </compilers>  
  </system.codedom>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.CodeDom.Compiler.CompilerInfo>
- <xref:System.CodeDom.Compiler.CodeDomProvider>
- [構成ファイル スキーマ](../index.md)
- [\<コンパイラ > 要素](compilers-element.md)
- [完全修飾型名の指定](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)
- [コンパイル用コンパイラのコンパイラ要素 (ASP.NET Settings スキーマ)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/a15ebt6c(v=vs.100))
