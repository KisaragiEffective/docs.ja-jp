---
title: -appconfig (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /appconfig
helpviewer_keywords:
- /appconfig compiler option [C#]
- -appconfig compiler option [C#]
- appconfig compiler option [C#]
ms.assetid: 1cdbcbcc-7813-4010-b5b8-e67c107c5a98
ms.openlocfilehash: 7a7e8e61f65704a2e99385a1be320048d950324c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69922520"
---
# <a name="-appconfig-c-compiler-options"></a>-appconfig (C# コンパイラ オプション)
**-appconfig** コンパイラ オプションを利用すると、C# アプリケーションで、アセンブリのバインド時に共通言語ランタイム (CLR) にアセンブリのアプリケーション構成 (app.config) ファイルの場所を指定できます。  
  
## <a name="syntax"></a>構文  
  
```console  
-appconfig:file  
```  
  
## <a name="arguments"></a>引数  
 `file`  
 必須です。 アセンブリ バインド設定を含むアプリケーション構成ファイル。  
  
## <a name="remarks"></a>解説  
 **-appconfig** の用途の 1 つは、1 つのアセンブリが特定の参照アセンブリの .NET Framework バージョンと .NET Framework for Silverlight バージョンの両方を同時に参照する必要がある高度なシナリオです。 たとえば、WPF (Windows Presentation Foundation) で作成された XAML デザイナーにおいて、デザイナーのユーザー インターフェイスとして WPF デスクトップを参照すると共に、Silverlight に組み込まれている WPF のサブセットも参照する必要がある場合があります。 同じデザイナー アセンブリで両方のアセンブリにアクセスする必要があります。 既定では、この 2 つのアセンブリはアセンブリ バインディングで同等と見なされるため、別々に参照するとコンパイラ エラーが発生します。  
  
 **-appconfig** コンパイラ オプションを利用すると、`<supportPortability>` タグを利用して既定の動作を無効にする app.config ファイルの場所を指定できます。次の例をご覧ください。  
  
 `<supportPortability PKT="7cec85d7bea7798e" enable="false"/>`  
  
 このコンパイラは、CLR のアセンブリ バインド ロジックにファイルの場所を渡します。  
  
> [!NOTE]
> Microsoft Build Engine (MSBuild) を利用してアプリケーションを構築する場合、プロパティ タグを .csproj ファイルに追加し、 **-appconfig** コンパイラ オプションを設定できます。 プロジェクトに既に設定されている app.config ファイルを使用するには、プロパティ タグ `<UseAppConfigForCompiler>` を .csproj ファイルに追加し、その値を `true` に設定します。 異なる app.config ファイルを指定するには、プロパティ タグ `<AppConfigForCompiler>` を追加し、その値をファイルの場所に設定します。  
  
## <a name="example"></a>例  
 .NET Framework と .NET Framework for Silverlight の両方の実装に存在する .NET Framework アセンブリについて、その両方の実装をアプリケーションで参照できるようにする app.config ファイルの例を次に示します。 **-appconfig** コンパイラ オプションにより、この app.config ファイルの場所が指定されます。  
  
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
  
## <a name="see-also"></a>参照

- [\<supportPortability> 要素](../../../framework/configure-apps/file-schema/runtime/supportportability-element.md)
- [アルファベット順の C# コンパイラ オプションの一覧](./listed-alphabetically.md)
