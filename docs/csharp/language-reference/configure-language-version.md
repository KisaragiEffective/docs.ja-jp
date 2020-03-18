---
title: C# 言語のバージョン管理 - C# ガイド
description: C# 言語のバージョンがプロジェクトに基づいて決定されるしくみとその選択の背後にある理由について説明します。 既定値を手動でオーバーライドする方法について説明します。
ms.date: 02/21/2020
ms.openlocfilehash: ef7275aad7638f52ecbfca1dfbdb962ae242fb48
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398233"
---
# <a name="c-language-versioning"></a>C# 言語のバージョン管理

最新の C# コンパイラでは、プロジェクトのターゲット フレームワーク (1 つまたは複数) に基づいて既定の言語バージョンが決定されます。 Visual Studio には値を変更するための UI がありませんが、それは *csproj* ファイルを編集することで変更できます。 既定値を選択すれば、ターゲット フレームワークと互換性がある最新の言語バージョンが使用されます。 プロジェクトのターゲットと互換性がある最新の言語機能にアクセスできるという利点があります。 また、このように既定値を選択すると、ターゲット フレームワークで利用できない型や実行時動作を必要とする言語が使用されません。 既定値より新しい言語バージョンを選択すると、コンパイル時間や実行時エラーの診断が困難になることがあります。

この記事の規則は、Visual Studio 2019 または .NET Core 3.0 SDK に付属するコンパイラに適用されます。 Visual Studio 2017 インストールまたは以前の .NET Core SDK バージョンに含まれる C# コンパイラは、既定で C# 7.0 を対象とします。

C# 8.0 (以降) は .NET Core 3.x 以降のバージョンでのみサポートされています。 最新機能の多くには、.NET Core 3.x で導入されたライブラリとランタイムの機能が必要になります。

- 既定のインターフェイス メンバー実装には、.NET Core 3.0 CLR の新機能が必要になります。
- 非同期ストリームには、<xref:System.IAsyncDisposable?displayProperty=nameWithType>、<xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType>、<xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType> という新しい型が必要になります。
- インデックスと範囲には、<xref:System.Index?displayProperty=nameWithType> と <xref:System.Range?displayProperty=nameWithType> という新しい型が必要です。
- null 参照型では、効果的に警告を与える目的でいくつかの[属性](../nullable-attributes.md)が利用されます。 その属性は .NET Core 3.0 で追加されました。 他のターゲット フレームには、そのような属性に関する注釈が付けられていません。 つまり、null 許容警告は潜在的な問題を正確に反映していない可能性があります。

## <a name="defaults"></a>[既定値]

コンパイラでは、以下の規則に基づいて既定値が決定されます。

|ターゲット フレーム|version|C# 言語の既定のバージョン|
|----------------|-------|---------------------------|
|.NET Core|3.x|C# 8.0|
|.NET Core|2.x|C# 7.3|
|.NET Standard|2.1|C# 8.0|
|.NET Standard|2.0|C# 7.3|
|.NET Standard|1.x|C# 7.3|
|.NET Framework|all|C# 7.3|

ご自分のプロジェクトが、対応するプレビュー バージョンの言語を持つプレビュー フレームワークをターゲットにしている場合、使用される言語バージョンはプレビュー バージョンの言語です。 環境を問わず、そのプレビューでは最新の機能が使用されます。リリース済みの .NET Core バージョンをターゲットにするプロジェクトに影響はありません。

## <a name="override-a-default"></a>既定値のオーバーライド

C# のバージョンを明示的に指定する必要がある場合は、いくつかの方法で実行できます。

- [プロジェクト ファイル](#edit-the-project-file)を手動で編集する。
- [サブディレクトリ内の複数のプロジェクトに対して](#configure-multiple-projects)言語バージョンを設定する。
- [`-langversion` コンパイラ オプション](compiler-options/langversion-compiler-option.md)を構成する。

### <a name="edit-the-project-file"></a>プロジェクト ファイルを編集する

プロジェクト ファイルで言語のバージョンを設定できます。 たとえば、プレビュー機能に明示的にアクセスしたい場合は、次のように要素を追加します。

```xml
<PropertyGroup>
   <LangVersion>preview</LangVersion>
</PropertyGroup>
```

値 `preview` では、コンパイラでサポートされている使用可能な最新のプレビュー C# 言語バージョンが使用されます。

### <a name="configure-multiple-projects"></a>複数のプロジェクトを構成する

複数のプロジェクトを構成するには、`<LangVersion>` 要素を含む **Directory.build.props** ファイルを作成します。 この操作は通常、ソリューション ディレクトリで実行します。 ソリューション ディレクトリ内の **Directory.Build.props** ファイルに以下を追加します。

```xml
<Project>
 <PropertyGroup>
   <LangVersion>preview</LangVersion>
 </PropertyGroup>
</Project>
```

そのファイルが含まれるディレクトリのすべてのサブディレクトリ内のビルドで、プレビュー C# バージョンが使用されます。 詳しくは、「[ビルドのカスタマイズ](/visualstudio/msbuild/customize-your-build)」を参照してください。

## <a name="c-language-version-reference"></a>C# 言語バージョン リファレンス

次の表では、現在のすべての C# 言語バージョンを示します。 コンパイラが古い場合、一部の値が正しく解釈されない可能性があります。 .NET Core 3.0 以降をインストールすれば、一覧にあるすべてにアクセスできます。

|[値]|説明|
|------------|-------------|
|preview|コンパイラは、最新のプレビュー バージョンの有効な言語構文をすべて受け入れます。|
|latest|コンパイラは、最新リリース バージョンのコンパイラ (マイナー バージョンを含む) の構文を受け入れます。|
|latestMajor|コンパイラは、最新リリースのメジャー バージョンのコンパイラの構文を受け入れます。|
|8.0|コンパイラは、C# 8.0 以下に含まれている構文のみを受け入れます。|
|7.3|コンパイラは、C# 7.3 以下に含まれている構文のみを受け入れます。|
|7.2|コンパイラは、C# 7.2 以下に含まれている構文のみを受け入れます。|
|7.1|コンパイラは、C# 7.1 以下に含まれている構文のみを受け入れます。|
|7|コンパイラは、C# 7.0 以下に含まれている構文のみを受け入れます。|
|6|コンパイラは、C# 6.0 以下に含まれている構文のみを受け入れます。|
|5|コンパイラは、C# 5.0 以下に含まれている構文のみを受け入れます。|
|4|コンパイラは、C# 4.0 以下に含まれている構文のみを受け入れます。|
|3|コンパイラは、C# 3.0 以下に含まれている構文のみを受け入れます。|
|ISO-2|コンパイラは、ISO/IEC 23270:2006 C# (2.0) に含まれている構文のみを受け入れます。 |
|ISO-1|コンパイラは、ISO/IEC 23270:2003 C# (1.0/1.2) に含まれている構文のみを受け入れます。 |
