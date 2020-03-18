---
title: .NET Core CLI を使用したライブラリの開発
description: .NET Core CLI を使用して .NET Core ライブラリを作成する方法について説明します。 複数のフレームワークをサポートするライブラリを作成します。
author: cartermp
ms.date: 05/01/2017
ms.openlocfilehash: c23c1f027b4d6d09c50eb2257d34f72ec56302f4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503511"
---
# <a name="develop-libraries-with-the-net-core-cli"></a>.NET Core CLI を使用したライブラリの開発

この記事では、.NET Core CLI を使用して .NET 用ライブラリを作成する方法について説明します。 CLI は、サポートされる任意の OS で動作する効率的で低レベルのエクスペリエンスを提供します。 Visual Studio でライブラリを構築することもできます。Visual Studio で構築する場合は、[Visual Studio ガイドを参照](library-with-visual-studio.md)してください。

## <a name="prerequisites"></a>前提条件

[.NET Core SDK と CLI](https://dotnet.microsoft.com/download) がコンピューターにインストールされている必要があります。

このドキュメントで [.NET Framework](https://dotnet.microsoft.com) バージョンについて扱うセクションでは、.NET Framework が Windows コンピューターにインストールされている必要があります。

また、古い .NET Framework ターゲットをサポートする場合、[.NET ダウンロードのアーカイブ ページ](https://dotnet.microsoft.com/download/archives)から Targeting Pack または Developer Pack をインストールする必要があります。 次の表を参照してください。

| .NET Framework のバージョン | ダウンロードするもの                                       |
| ---------------------- | ------------------------------------------------------ |
| 4.6.1                  | .NET Framework 4.6.1 Targeting Pack                    |
| 4.6                    | .NET Framework 4.6 Targeting Pack                      |
| 4.5.2                  | .NET Framework 4.5.2 Developer Pack                    |
| 4.5.1                  | .NET Framework 4.5.1 Developer Pack                    |
| 4.5                    | Windows 8 用 Windows ソフトウェア開発キット         |
| 4.0                    | Windows SDK for Windows 7 および .NET Framework 4         |
| 2.0、3.0、および 3.5      | .NET Framework 3.5 SP1 Runtime (または Windows 8 以降のバージョン) |

## <a name="how-to-target-the-net-standard"></a>.NET Standard をターゲット設定する方法

.NET Standard にあまりなじみがない場合、詳細については、「[.NET Standard](../../standard/net-standard.md)」をご覧ください。

この記事には、.NET Standard バージョンを多様な実装にマップする表が掲載されています。

[!INCLUDE [net-standard-table](../../../includes/net-standard-table.md)]

ライブラリを作成する目的でこの表の意味について説明します。

選択する .NET Standard のバージョンは、最新の API にアクセスできることと、より多くの .NET 実装と .NET Standard バージョンをターゲット設定できることのトレードオフで決まります。 ターゲット設定可能なプラットフォームとバージョンの範囲は、`netstandardX.X` のバージョンを選択し (`X.X` はバージョン番号です)、プロジェクト ファイル (`.csproj` または `.fsproj`) に追加することで制御します。

.NET Standard をターゲット設定する場合、ニーズに応じて 3 つの主要なオプションがあります。

1. テンプレートで提供されている既定のバージョンの .NET Standard (`netstandard1.4`) を使用できます。これにより、UWP、.NET Framework 4.6.1、.NET Standard 2.0 との互換性を保ちながら、.NET Standard 上のほとんどの API にアクセスできます。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <TargetFramework>netstandard1.4</TargetFramework>
      </PropertyGroup>
    </Project>
    ```

2. プロジェクト ファイルの `TargetFramework` ノードの値を変更することで、.NET Standard の下位または上位バージョンを使用することができます。

    .NET Standard バージョンは下位互換性があります。 つまり、`netstandard1.0` ライブラリは、`netstandard1.1` プラットフォーム以降で実行されます。 ただし、上位互換性はありません。 下位の .NET Standard プラットフォームは、上位のものを参照できません。 つまり、`netstandard1.0` ライブラリは、`netstandard1.1` 以降をターゲットとするライブラリを参照できません。 API とプラットフォームを適切に組み合わせ、ニーズに対応できる Standard バージョンを選択してください。 現時点では `netstandard1.4` が推奨されます。

3. .NET Framework バージョン 4.0 以前をターゲットにする場合、または .NET Framework では利用できて .NET Standard では利用できない API (`System.Drawing` など) を使用する場合は、マルチターゲットの方法について次のセクションを参照してください。

## <a name="how-to-target-net-framework"></a>.NET Framework をターゲット設定する方法

> [!NOTE]
> 次の手順では、お使いのマシンに .NET Framework がインストールされていると想定しています。 依存関係のインストールについては、「[前提条件](#prerequisites)」を参照してください。

ここで使用されている .NET Framework バージョンには、現在サポートされていないものもあります。 サポートされないバージョンについては、「[.NET Framework Support Lifecycle Policy FAQ](https://support.microsoft.com/gp/framework_faq/en-us)」(.NET Framework のサポート ライフサイクル ポリシーについてよく寄せられる質問) を参照してください。

最大数の開発者とプロジェクトを対象にしたい場合、基準となるターゲットは .NET Framework 4.0 です。 .NET Framework をターゲット設定するには、まず、サポートしたい .NET Framework バージョンに対応する正しいターゲット フレームワーク モニカー (TFM) を使用します。

| .NET Framework のバージョン | TFM      |
| ---------------------- | -------- |
| .NET Framework 2.0     | `net20`  |
| .NET Framework 3.0     | `net30`  |
| .NET Framework 3.5     | `net35`  |
| .NET Framework 4.0     | `net40`  |
| .NET Framework 4.5     | `net45`  |
| .NET Framework 4.5.1   | `net451` |
| .NET Framework 4.5.2   | `net452` |
| .NET Framework 4.6     | `net46`  |
| .NET Framework 4.6.1   | `net461` |
| .NET Framework 4.6.2   | `net462` |
| .NET Framework 4.7     | `net47`  |
| .NET Framework 4.8     | `net48`  |

この TFM をプロジェクト ファイルの `TargetFramework` セクションに挿入します。 .NET Framework 4.0 をターゲットとするライブラリを作成する例を次に示します。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net40</TargetFramework>
  </PropertyGroup>
</Project>
```

これで終了です。 これは .NET Framework 4 向けにのみコンパイルされていますが、新しいバージョンの .NET Framework のライブラリを使用できます。

## <a name="how-to-multitarget"></a>マルチターゲットを設定する方法

> [!NOTE]
> 次の手順では、.NET Framework がコンピューターにインストールされている想定です。 インストールする必要がある依存関係と、そのダウンロードできる場所については、「[前提条件](#prerequisites)」セクションを参照してください。

プロジェクトが .NET Framework と .NET Core の両方をサポートしている場合、状況によっては古いバージョンの .NET Framework をターゲットにする必要があります。 このシナリオで、新しい API と新しいターゲット向けの言語構成を使用する場合、コードで `#if` ディレクティブを使用します。 また、必要に応じて、ターゲットにする各プラットフォームに応じて異なるパッケージと依存関係追加して、それぞれに異なる必要な API を含めます。

たとえば、HTTP 上でネットワークキング操作を行うライブラリがあるとします。 .NET Standard と .NET Framework バージョン 4.5 以降の場合、`HttpClient` 名前空間の `System.Net.Http` クラスを使用できます。 ただし、それより前のバージョンの .NET Framework に `HttpClient` クラスはないので、代わりに `WebClient` 名前空間の `System.Net` クラスを使用できます。

プロジェクト ファイルは次のようになります。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFrameworks>netstandard1.4;net40;net45</TargetFrameworks>
  </PropertyGroup>

  <!-- Need to conditionally bring in references for the .NET Framework 4.0 target -->
  <ItemGroup Condition="'$(TargetFramework)' == 'net40'">
    <Reference Include="System.Net" />
  </ItemGroup>

  <!-- Need to conditionally bring in references for the .NET Framework 4.5 target -->
  <ItemGroup Condition="'$(TargetFramework)' == 'net45'">
    <Reference Include="System.Net.Http" />
    <Reference Include="System.Threading.Tasks" />
  </ItemGroup>
</Project>
```

主な変更点が 3 つあります。

1. `TargetFramework` ノードは `TargetFrameworks` で置き換えられ、3 つの TFM が内部に表現されています。
1. 1 つの .NET Framework 参照を取り込む `<ItemGroup>` ターゲットの `net40` ノードがあります。
1. .NET Framework の参照 2 に取り込む `<ItemGroup>` ターゲットの `net45` ノードがあります。

ビルド システムは `#if` ディレクティブで使用される次のプリプロセッサ シンボルを認識します。

[!INCLUDE [Preprocessor symbols](../../../includes/preprocessor-symbols.md)]

次に、ターゲットごとに条件付きコンパイルを利用する例を示します。

```csharp
using System;
using System.Text.RegularExpressions;
#if NET40
// This only compiles for the .NET Framework 4 targets
using System.Net;
#else
 // This compiles for all other targets
using System.Net.Http;
using System.Threading.Tasks;
#endif

namespace MultitargetLib
{
    public class Library
    {
#if NET40
        private readonly WebClient _client = new WebClient();
        private readonly object _locker = new object();
#else
        private readonly HttpClient _client = new HttpClient();
#endif

#if NET40
        // .NET Framework 4.0 does not have async/await
        public string GetDotNetCount()
        {
            string url = "https://www.dotnetfoundation.org/";

            var uri = new Uri(url);

            string result = "";

            // Lock here to provide thread-safety.
            lock(_locker)
            {
                result = _client.DownloadString(uri);
            }

            int dotNetCount = Regex.Matches(result, ".NET").Count;

            return $"Dotnet Foundation mentions .NET {dotNetCount} times!";
        }
#else
        // .NET 4.5+ can use async/await!
        public async Task<string> GetDotNetCountAsync()
        {
            string url = "https://www.dotnetfoundation.org/";

            // HttpClient is thread-safe, so no need to explicitly lock here
            var result = await _client.GetStringAsync(url);

            int dotNetCount = Regex.Matches(result, ".NET").Count;

            return $"dotnetfoundation.org mentions .NET {dotNetCount} times in its HTML!";
        }
#endif
    }
}
```

`dotnet build` を使用してこのプロジェクトをビルドすると、`bin/` フォルダー以下に 3 つのディレクトリがあることがわかります。

```
net40/
net45/
netstandard1.4/
```

各ディレクトリには、各ターゲットの `.dll` ファイルがあります。

## <a name="how-to-test-libraries-on-net-core"></a>.NET Core でライブラリをテストする方法

プラットフォーム全体でテストできることが重要です。 [xUnit](https://xunit.github.io/) または MSTest はそのまま利用できます。 どちらも、.NET Core 上のライブラリの単体テストに最適です。 テスト プロジェクトでソリューションをセットアップする方法は、[ソリューションの構造](#structuring-a-solution)によって異なります。 次の例は、テスト ディレクトリとソース ディレクトリが同じ最上位ディレクトリにある場合です。

> [!NOTE]
> ここでは、いくつかの [.NET Core CLI](../tools/index.md) コマンドが使用されます。 詳細については、「[dotnet new](../tools/dotnet-new.md)」と「[dotnet sln](../tools/dotnet-sln.md)」を参照してください。

1. ソリューションを設定します。 次のコマンドで実行することができます。

   ```dotnetcli
   mkdir SolutionWithSrcAndTest
   cd SolutionWithSrcAndTest
   dotnet new sln
   dotnet new classlib -o MyProject
   dotnet new xunit -o MyProject.Test
   dotnet sln add MyProject/MyProject.csproj
   dotnet sln add MyProject.Test/MyProject.Test.csproj
   ```

   これでプロジェクトが作成され、ソリューション内のプロジェクトがリンクされます。 `SolutionWithSrcAndTest` のディレクトリは次のようになります。

   ```
   /SolutionWithSrcAndTest
   |__SolutionWithSrcAndTest.sln
   |__MyProject/
   |__MyProject.Test/
   ```

1. テスト プロジェクトのディレクトリに移動し、`MyProject.Test` から `MyProject` への参照を追加します。

   ```dotnetcli
   cd MyProject.Test
   dotnet add reference ../MyProject/MyProject.csproj
   ```

1. パッケージを復元し、プロジェクトをビルドします。

   ```dotnetcli
   dotnet restore
   dotnet build
   ```

   [!INCLUDE[DotNet Restore Note](../../../includes/dotnet-restore-note.md)]

1. `dotnet test` コマンドを実行して、xUnit が実行されることを確認します。 MSTest を使用する場合は、MSTest コンソール実行ツールが実行されることを確認します。

これで終了です。 コマンド ライン ツールを使用して、すべてのプラットフォームでライブラリをテストできるようになりました。 すべてをセットアップしてテストに進む場合、ライブラリのテストはとても単純です。

1. ライブラリに変更を加えます。
1. コマンド ラインから、`dotnet test` コマンドを使用してテスト ディレクトリでテストを実行します。

`dotnet test` コマンドを呼び出すと、コードは自動的にリビルドされます。

## <a name="how-to-use-multiple-projects"></a>複数のプロジェクトを使用する方法

大規模なライブラリで一般的なニーズは、複数のプロジェクトに機能を配置することです。

たとえば、慣用的な C# や F# で使用できるライブラリをビルドするとします。 これは、ライブラリのユーザーは、C# または F# に対して自然な方法で使用することを意味します。 たとえば、C# の場合、次のようにライブラリを使用できます。

```csharp
using AwesomeLibrary.CSharp;

public Task DoThings(Data data)
{
    var convertResult = await AwesomeLibrary.ConvertAsync(data);
    var result = AwesomeLibrary.Process(convertResult);
    // do something with result
}
```

F# の場合は次のようになります。

```fsharp
open AwesomeLibrary.FSharp

let doWork data = async {
    let! result = AwesomeLibrary.AsyncConvert data // Uses an F# async function rather than C# async method
    // do something with result
}
```

このような使用シナリオは、アクセスされる API が C# と F# 用に異なる構造を持つ必要があることを示します。  これを実現する一般的なアプローチとして、ライブラリのすべてのロジックをコア プロジェクトに取り入れ、C# および F# プロジェクトでコア プロジェクトに呼び出す API レイヤーを定義する方法があります。  以降のセクションでは、次の名前を使用します。

* **AwesomeLibrary.Core** - ライブラリのすべてのロジックを含むコア プロジェクト
* **AwesomeLibrary.CSharp** - C# で使用するためのパブリック API を含むプロジェクト
* **AwesomeLibrary.FSharp** - F# で使用するためのパブリック API を含むプロジェクト

自分の端末で次のコマンドを実行し、このガイドと同じ構造を作成することができます。

```dotnetcli
mkdir AwesomeLibrary && cd AwesomeLibrary
dotnet new sln
mkdir AwesomeLibrary.Core && cd AwesomeLibrary.Core && dotnet new classlib
cd ..
mkdir AwesomeLibrary.CSharp && cd AwesomeLibrary.CSharp && dotnet new classlib
cd ..
mkdir AwesomeLibrary.FSharp && cd AwesomeLibrary.FSharp && dotnet new classlib -lang "F#"
cd ..
dotnet sln add AwesomeLibrary.Core/AwesomeLibrary.Core.csproj
dotnet sln add AwesomeLibrary.CSharp/AwesomeLibrary.CSharp.csproj
dotnet sln add AwesomeLibrary.FSharp/AwesomeLibrary.FSharp.fsproj
```

これで上記の 3 つのプロジェクトと、それらをリンクするソリューション ファイルが追加されます。 ソリューション ファイルとリンク プロジェクトを作成すると、最上位レベルからプロジェクトを復元し、ビルドできるようになります。

### <a name="project-to-project-referencing"></a>プロジェクト間参照

プロジェクトを参照するには、.NET Core CLI を使用してプロジェクト参照を追加することをお勧めします。 **AwesomeLibrary.CSharp** と **AwesomeLibrary.FSharp** のプロジェクト ディレクトリから、次のコマンドを実行できます。

```dotnetcli
dotnet add reference ../AwesomeLibrary.Core/AwesomeLibrary.Core.csproj
```

**AwesomeLibrary.CSharp** と **AwesomeLibrary.FSharp** の両方のプロジェクト ファイルは、**ターゲットとして**AwesomeLibrary.Core`ProjectReference` を参照するようになります。  この参照を確認するには、プロジェクト ファイルに以下の行があることを確認します。

```xml
<ItemGroup>
  <ProjectReference Include="..\AwesomeLibrary.Core\AwesomeLibrary.Core.csproj" />
</ItemGroup>
```

この .NET Core CLI を使用しない場合は、このセクションを各プロジェクト ファイルに手動で追加します。

### <a name="structuring-a-solution"></a>ソリューションの構築

マルチプロジェクト ソリューションのもう 1 つの重要な側面は、全体のプロジェクト構造を適切に構築することです。 ただし、`dotnet sln add` でソリューション ファイルに各プロジェクト ファイルがリンクされ、ソリューション レベルで `dotnet restore` と `dotnet build` を実行できる限り、好みに応じてコードを整理することができます。
