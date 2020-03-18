---
title: dotnet テストと xUnit を使用した .NET Core での単体テスト F#
description: dotnet テストおよび xUnit を使用したサンプル ソリューションを段階的に構築していく対話型エクスペリエンスを通じて、.NET Core における F# の単体テストの概念について説明します。
author: billwagner
ms.author: wiwagn
ms.date: 08/30/2017
ms.openlocfilehash: 5fe4a8faddd87334439513368f24d808abc58e65
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157311"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-xunit"></a>dotnet テストと xUnit を使用した .NET Core での単体テスト F# ライブラリ

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-with-fsharp/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a>ソース プロジェクトの作成

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-with-fsharp* というディレクトリを作成します。
この新しいディレクトリ内で `dotnet new sln` を実行して、ソリューションを新たに作成します。 こうすることで、クラス ライブラリと単体テスト プロジェクトの両方を管理しやすくなります。
ソリューションのディレクトリ内で、*MathService* ディレクトリを作成します。 現時点のディレクトリとファイルの構造は次のようになっています。

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

*MathService* を現在のディレクトリにし、`dotnet new classlib -lang "F#"` を実行してソース プロジェクトを作成します。 計算サービスのエラーが発生する実装を作成します。

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

*unit-testing-with-fsharp* ディレクトリに戻ります。 `dotnet sln add .\MathService\MathService.fsproj` を実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

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

*MathService.Tests* ディレクトリを現在のディレクトリにし、`dotnet new xunit -lang "F#"` を使用して新しいプロジェクトを作成します。 これで、テスト ライブラリとして xUnit を使用するテスト プロジェクトが作成されます。 生成されたテンプレートで、*MathServiceTests.fsproj* のテスト ランナーが構成されます。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0-preview-20170628-02" />
  <PackageReference Include="xunit" Version="2.2.0" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0" />
</ItemGroup>
```

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` によって、xUnit と xUnit ランナーが追加されています。 ここで、プロジェクトに別の依存関係として `MathService` クラス ライブラリを追加します。 次の `dotnet add reference` コマンドを使用します。

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
        MathServiceTests.fsproj
```

`dotnet sln add .\MathService.Tests\MathService.Tests.fsproj`unit-testing-with-fsharp*ディレクトリで* を実行します。

## <a name="creating-the-first-test"></a>最初のテストの作成

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *Tests.fs* を開き、次のコードを追加します。

```fsharp
[<Fact>]
let ``My test`` () =
    Assert.True(true)

[<Fact>]
let ``Fail every time`` () = Assert.True(false)
```

`[<Fact>]` 属性は、テスト ランナーによって実行されるテスト メソッドを表します。 *unit-testing-with-fsharp* で `dotnet test` を実行してテストとクラス ライブラリをビルドし、それからテストを実行します。 xUnit テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

この 2 つのテストは、最も基本的な成功テストと失敗テストです。 `My test` は成功し、`Fail every time` は失敗します。 今度は `squaresOfOdds` メソッドのテストを作成します。 `squaresOfOdds` メソッドは、入力シーケンスに含まれるすべての奇数の整数値を 2 乗した値のシーケンスを返します。 これらの関数をすべて一度に書き込むのではなく、機能を検証するテストを繰り返し作成することができます。 各テストを成功させることで、メソッドに必要な機能を作成することになります。

最も簡単に記述できるテストは、すべて偶数である数字を `squaresOfOdds` に渡して呼び出すことです。このテストの結果は、整数の空のシーケンスになります。  次に示すのがそのテストです。

```fsharp
[<Fact>]
let ``Sequence of Evens returns empty collection`` () =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

テストが失敗します。 実装はまだ作成していません。 最も単純な動作のコードを `MathService` クラスに記述して、このテストが成功するようにします。

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

*unit-testing-with-fsharp* ディレクトリで、もう一度 `dotnet test` を実行します。 `dotnet test` コマンドは `MathService` プロジェクトのビルドを実行してから、`MathService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、この単一テストが実行されます。 成功します。

## <a name="completing-the-requirements"></a>要件の完成

テストが成功したので、他のテストも記述してみましょう。 次の単純な例は、奇数は `1` のみが含まれるシーケンスで機能します。 数値の 1 は簡単です。なぜなら 1 の 2 乗は 1 になるためです。 次のテストを以下に示します。

```fsharp
[<Fact>]
let ``Sequences of Ones and Evens returns Ones`` () =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

`dotnet test` を実行するとテストが実行され、新しいテストが失敗することがわかります。 今度は、新しいテストに対応するために `squaresOfOdds` メソッドを更新します。 このテストを成功させるには、フィルター処理でシーケンスからすべての偶数を除外します。 小さなフィルター関数を記述し、`Seq.filter` を使用することで実現できます。

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

実施する手順がもう一つあります。各奇数を 2 乗します。 テストを新たに記述するところから始めます。

```fsharp
[<Fact>]
let ``SquaresOfOdds works`` () =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.Equal(expected, actual)
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

- [dotnet new](../tools/dotnet-new.md)
- [dotnet sln](../tools/dotnet-new.md)
- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
