---
title: dotnet テストと NUnit を使用した .NET Core での単体テスト F#
description: dotnet テストおよび NUnit を使用したサンプル ソリューションを段階的に構築していく対話型エクスペリエンスを通じて、.NET Core における F# の単体テストの概念について説明します。
author: rprouse
ms.date: 10/04/2018
dev_langs:
- fsharp
ms.openlocfilehash: 3347e5b90c31589e9a0f99ac0d9298927a717f56
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715444"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-nunit"></a>dotnet テストと NUnit を使用した .NET Core での単体テスト F# ライブラリ

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-with-fsharp-nunit/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>前提条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降のバージョン。
- ユーザーが選んだテキスト エディターまたはコード エディター。

## <a name="creating-the-source-project"></a>ソース プロジェクトの作成

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-with-fsharp* というディレクトリを作成します。
この新しいディレクトリ内で、次のコマンドを実行して、クラス ライブラリとテスト プロジェクト用の新しいソリューション ファイルを作成します。

```dotnetcli
dotnet new sln
```

次に、*MathService* ディレクトリを作成します。 これまでのところ、ディレクトリとファイルの構造は次のアウトラインのようになっています。

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

*MathService* を現在のディレクトリとし、次のコマンドを実行してソース プロジェクトを作成します。

```dotnetcli
dotnet new classlib -lang "F#"
```

計算サービスのエラーが発生する実装を作成します。

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

*unit-testing-with-fsharp* ディレクトリに戻ります。 次のコマンドを実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add .\MathService\MathService.fsproj
```

## <a name="creating-the-test-project"></a>テスト プロジェクトの作成

次に、*MathService.Tests* ディレクトリを作成します。 次の一覧はディレクトリ構造を示したものです。

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

*MathService.Tests* ディレクトリを現在のディレクトリとし、次のコマンドを使用して新しいプロジェクトを作成します。

```dotnetcli
dotnet new nunit -lang "F#"
```

これにより、テスト フレームワークとして NUnit を使用するテスト プロジェクトが作成されます。 生成されたテンプレートで、*MathServiceTests.fsproj* のテスト ランナーが構成されます。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="NUnit" Version="3.9.0" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.9.0" />
</ItemGroup>
```

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` では、NUnit と NUnit のテスト アダプターを追加しました。 ここで、プロジェクトに別の依存関係として `MathService` クラス ライブラリを追加します。 次の `dotnet add reference` コマンドを使用します。

```dotnetcli
dotnet add reference ../MathService/MathService.fsproj
```

全体のファイルは GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj)で確認できます。

ソリューションの最終的なレイアウトは次のようになります。

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
        Test Source Files
        MathService.Tests.fsproj
```

*unit-testing-with-fsharp* ディレクトリ内で次のコマンドを実行します。

```dotnetcli
dotnet sln add .\MathService.Tests\MathService.Tests.fsproj
```

## <a name="creating-the-first-test"></a>最初のテストの作成

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *UnitTest1.fs* を開き、次のコードを追加します。

```fsharp
namespace MathService.Tests

open System
open NUnit.Framework
open MathService

[<TestFixture>]
type TestClass () =

    [<Test>]
    member this.TestMethodPassing() =
        Assert.True(true)

    [<Test>]
     member this.FailEveryTime() = Assert.True(false)
```

`[<TestFixture>]` 属性は、テストを含むクラスを表します。 `[<Test>]` 属性は、テスト ランナーによって実行されるテスト メソッドを表します。 *unit-testing-with-fsharp* ディレクトリで `dotnet test` を実行してテストとクラス ライブラリをビルドし、それからテストを実行します。 NUnit テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

この 2 つのテストは、最も基本的な成功テストと失敗テストです。 `My test` は成功し、`Fail every time` は失敗します。 今度は `squaresOfOdds` メソッドのテストを作成します。 `squaresOfOdds` メソッドは、入力シーケンスに含まれるすべての奇数の整数値を 2 乗した値のシーケンスを返します。 これらの関数をすべて一度に書き込むのではなく、機能を検証するテストを繰り返し作成することができます。 各テストを成功させることで、メソッドに必要な機能を作成することになります。

最も簡単に記述できるテストは、すべて偶数である数字を `squaresOfOdds` に渡して呼び出すことです。このテストの結果は、整数の空のシーケンスになります。  次に示すのがそのテストです。

```fsharp
[<Test>]
member this.TestEvenSequence() =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

`expected` シーケンスがリストに変換されていることに注意してください。 NUnit ライブラリは、標準的な .NET 型の多くに依存しています。 この依存関係は、お使いのパブリック インターフェイスおよび期待される結果が、<xref:System.Collections.ICollection> でなく <xref:System.Collections.IEnumerable> をサポートしていることを意味します。

テストを実行すると、失敗することがわかります。 実装はまだ作成していません。 このテストに合格するには、動作する MathService プロジェクトの *Library.fs* クラスに、ごく簡単なコードを記述します。

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

*unit-testing-with-fsharp* ディレクトリで、もう一度 `dotnet test` を実行します。 `dotnet test` コマンドは `MathService` プロジェクトのビルドを実行してから、`MathService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、このテストが実行されます。 今回は 2 つのテストが合格します。

## <a name="completing-the-requirements"></a>要件の完成

テストが成功したので、他のテストも記述してみましょう。 次の単純な例は、奇数は `1` のみが含まれるシーケンスで機能します。 数値の 1 は簡単です。なぜなら 1 の 2 乗は 1 になるためです。 次のテストを以下に示します。

```fsharp
[<Test>]
member public this.TestOnesAndEvens() =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

`dotnet test` を実行すると、新しいテストは失敗します。 新しいテストに対応するには `squaresOfOdds` メソッドを更新する必要があります。 このテストを成功させるには、フィルター処理でシーケンスからすべての偶数を除外する必要があります。 小さなフィルター関数を記述し、`Seq.filter` を使用することで実現できます。

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

`Seq.toList` を呼び出していることに注意してください。 これにより、<xref:System.Collections.ICollection> インターフェイスを実装するリストが作成されます。

実施する手順がもう一つあります。各奇数を 2 乗します。 テストを新たに記述するところから始めます。

```fsharp
[<Test>]
member public this.TestSquaresOfOdds() =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

テストを修正するには、フィルター処理したシーケンスをパイプでつなぎ、各奇数の 2 乗を計算するマップ操作をします。

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
```

これで、小さなライブラリとそのライブラリの単体テストのセットが構築されました。 ソリューションを構築したことで、新しいパッケージとテストの追加が通常のワークフローに組み込まれました。 アプリケーションの目標を達成することに時間と労力の多くを割き、集中して取り組みました。

## <a name="see-also"></a>参照

- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
