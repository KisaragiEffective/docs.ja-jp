---
title: NUnit と .NET Core による単体テスト C#
description: dotnet テストおよび NUnit を使用したサンプル ソリューションを段階的に構築していく対話型エクスペリエンスを通じて、C# および .NET Core の単体テストの概念について説明します。
author: rprouse
ms.date: 08/31/2018
ms.openlocfilehash: 283aa5a28ed213d4290eb3c73a98af56ec074ad0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240884"
---
# <a name="unit-testing-c-with-nunit-and-net-core"></a>NUnit と .NET Core による単体テスト C#

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>前提条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降のバージョン。
- ユーザーが選んだテキスト エディターまたはコード エディター。

## <a name="creating-the-source-project"></a>ソース プロジェクトの作成

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-using-nunit* というディレクトリを作成します。 この新しいディレクトリ内で、次のコマンドを実行して、クラス ライブラリとテスト プロジェクト用の新しいソリューション ファイルを作成します。

```dotnetcli
dotnet new sln
```

次に、*PrimeService* ディレクトリを作成します。 これまでのところ、ディレクトリとファイルの構造は次のアウトラインのようになっています。

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
```

*PrimeService* を現在のディレクトリとし、次のコマンドを実行してソース プロジェクトを作成します。

```dotnetcli
dotnet new classlib
```

*Class1.cs* の名前を *PrimeService.cs* に変更します。 `PrimeService` クラスのエラーが発生する実装を作成します。

```csharp
using System;

namespace Prime.Services
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            throw new NotImplementedException("Please create a test first.");
        }
    }
}
```

*unit-testing-using-nunit* ディレクトリに戻ります。 次のコマンドを実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add PrimeService/PrimeService.csproj
```

## <a name="creating-the-test-project"></a>テスト プロジェクトの作成

次に、*PrimeService.Tests* ディレクトリを作成します。 次の一覧はディレクトリ構造を示したものです。

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

*PrimeService.Tests* ディレクトリを現在のディレクトリとし、次のコマンドを使用して新しいプロジェクトを作成します。

```dotnetcli
dotnet new nunit
```

[dotnet new](../tools/dotnet-new.md) コマンドによって、テスト ライブラリとして NUnit を使用するテスト プロジェクトが作成されます。 生成されたテンプレートによって、*PrimeService.Tests.csproj* ファイル内にテスト ランナーが構成されます。

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService.Tests.csproj#Packages)]

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` では Microsoft テスト SDK、NUnit テスト フレームワーク、NUnit テスト アダプターを追加しました。 ここで、プロジェクトに別の依存関係として `PrimeService` クラス ライブラリを追加します。 次の [`dotnet add reference`](../tools/dotnet-add-reference.md) コマンドを使用します。

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

全体のファイルは GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService.Tests.csproj)で確認できます。

ソリューションの最終的なレイアウトは次のアウトラインのようになります。

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.csproj
```

*unit-testing-vb-nunit* ディレクトリ内で次のコマンドを実行します。

```dotnetcli
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

## <a name="creating-the-first-test"></a>最初のテストの作成

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *PrimeService.Tests*ディレクトリ内で、*UnitTest1.cs*ファイルの名前を *PrimeService_IsPrimeShould.cs* に変更し、その内容全体を次のコードに置き換えます。

[!code-csharp[Sample_FirstTest](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_FirstTest)]

`[TestFixture]` 属性は、単体テストを含むクラスを表します。 `[Test]` 属性は、メソッドがテスト メソッドであることを表します。

このファイルを保存し、[`dotnet test`](../tools/dotnet-test.md) を実行してテストとクラス ライブラリをビルドしてから、テストを実行します。 NUnit テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

テストが失敗します。 実装はまだ作成していません。 最も単純な動作のコードを `PrimeService` クラスに記述して、このテストが成功するようにします。

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Please create a test first.");
}
```

*unit-testing-using-nunit* ディレクトリで、もう一度 `dotnet test` を実行します。 `dotnet test` コマンドは `PrimeService` プロジェクトのビルドを実行してから、`PrimeService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、この単一テストが実行されます。 成功します。

## <a name="adding-more-features"></a>他の機能の追加

テストが成功したので、他のテストも記述してみましょう。 素数に関する、いくつかの単純なケースが他にもあります (0、-1)。 `[Test]` 属性を使用すると新しいテストを追加できますが、すぐに煩雑になります。 一連の類似のテストを記述できるようになる、他の NUnit 属性があります。  `[TestCase]` 属性は同じコードを実行するものの、異なる入力引数が含まれる一連のテストを作成するために使用します。 `[TestCase]` 属性を使用して、そのような入力の値を指定することができます。

新しいテストを作成するのではなく、この属性を適用することで 1 つのデータ駆動テストを作成します。 そのデータ駆動テストとは、複数の 2 未満の値を調べて、最も小さい素数を特定するという手法です。

[!code-csharp[Sample_TestCode](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

`dotnet test` を実行して、これらの 2 つのテストが失敗したとします。 すべてのテストを成功させるために、`if`PrimeService.cs`Main` ファイルで *メソッドの先頭にある* 句を変更します。

```csharp
if (candidate < 2)
```

他のテスト、理論、コードをメイン ライブラリに追加して、反復を続けます。 [テストの最終版](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.cs)ができ、[ライブラリの完全な実装](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService/PrimeService.cs)が完了しました。

これで、小さなライブラリとそのライブラリの単体テストのセットが構築されました。 ソリューションを構築したことで、新しいパッケージとテストの追加が通常のワークフローに組み込まれました。 アプリケーションの目標を達成することに時間と労力の多くを割き、集中して取り組みました。
