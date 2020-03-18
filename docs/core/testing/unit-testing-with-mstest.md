---
title: MSTest と .NET Core による単体テスト C#
description: dotnet テストおよび MSTest を使用したサンプル ソリューションを段階的に構築していく対話型エクスペリエンスを通じて、C# および .NET Core の単体テストの概念について説明します。
author: ncarandini
ms.author: wiwagn
ms.date: 09/08/2017
ms.openlocfilehash: bd7891243d84277a7578089f8b4629ff5bada577
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240910"
---
# <a name="unit-testing-c-with-mstest-and-net-core"></a>MSTest と .NET Core による単体テスト C#

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="create-the-source-project"></a>ソース プロジェクトを作成する

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-using-mstest* というディレクトリを作成します。 この新しいディレクトリ内で [`dotnet new sln`](../tools/dotnet-new.md) を実行して、クラス ライブラリとテスト プロジェクト用の新しいソリューション ファイルを作成します。 次に、*PrimeService* ディレクトリを作成します。 現時点のディレクトリとファイルの構造は次のアウトラインのようになっています。

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
```

*PrimeService* を現在のディレクトリにし、[`dotnet new classlib`](../tools/dotnet-new.md) を実行してソース プロジェクトを作成します。 *Class1.cs* の名前を *PrimeService.cs* に変更します。 `PrimeService` クラスのエラーが発生する実装を作成します。

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

*unit-testing-using-mstest* ディレクトリに戻ります。 [`dotnet sln add PrimeService/PrimeService.csproj`](../tools/dotnet-sln.md) を実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

## <a name="create-the-test-project"></a>テスト プロジェクトの作成

次に、*PrimeService.Tests* ディレクトリを作成します。 次の一覧はディレクトリ構造を示したものです。

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

*PrimeService.Tests* ディレクトリを現在のディレクトリにし、[`dotnet new mstest`](../tools/dotnet-new.md) を使用して新しいプロジェクトを作成します。 dotnet new コマンドによって、テスト ライブラリとして MSTest を使用するテスト プロジェクトが作成されます。 生成されたテンプレートで、*PrimeServiceTests.csproj* ファイルのテスト ランナーが構成されます。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` により、MSTest SDK、MSTest テスト フレームワーク、MSTest ランナーが追加されました。 ここで、プロジェクトに別の依存関係として `PrimeService` クラス ライブラリを追加します。 次の [`dotnet add reference`](../tools/dotnet-add-reference.md) コマンドを使用します。

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

全体のファイルは GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService.Tests.csproj)で確認できます。

ソリューションの最終的なレイアウトは次のアウトラインのようになります。

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeServiceTests.csproj
```

*unit-testing-using-mstest* ディレクトリで [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.csproj`](../tools/dotnet-sln.md) を実行します。

## <a name="create-the-first-test"></a>最初のテストを作成する

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *PrimeService.Tests* ディレクトリから *UnitTest1.cs* を削除し、*PrimeService_IsPrimeShould.cs* という名前の新しい C# ファイルを作成します。コンテンツは次のようになります。

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestClass]
    public class PrimeService_IsPrimeShould
    {
        private readonly PrimeService _primeService;

        public PrimeService_IsPrimeShould()
        {
            _primeService = new PrimeService();
        }

        [TestMethod]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var result = _primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}
```

[TestClass 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute)は、単体テストを含むクラスを表します。 [TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)は、メソッドがテスト メソッドであることを示します。

このファイルを保存し、[`dotnet test`](../tools/dotnet-test.md) を実行してテストとクラス ライブラリをビルドしてから、テストを実行します。 MSTest テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

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

*unit-testing-using-mstest* ディレクトリで、もう一度 `dotnet test` を実行します。 `dotnet test` コマンドは `PrimeService` プロジェクトのビルドを実行してから、`PrimeService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、この単一テストが実行されます。 成功します。

## <a name="add-more-features"></a>その他の機能を追加する

テストが成功したので、他のテストも記述してみましょう。 素数に関する、いくつかの単純なケースが他にもあります(0、-1)。 [TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)を使用すると新しいテストを追加できますが、すぐに煩雑になります。 一連の類似のテストを記述できるようになる、他の MSTest 属性があります。  [DataTestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataTestMethodAttribute)は同じコードを実行するものの、異なる入力引数が含まれる一連のテストを表します。 [DataRow 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataRowAttribute)を使用して、そのような入力の値を指定することができます。

新しいテストを作成するのではなく、この 2 つの属性を適用することで 1 つのデータ駆動テストを作成できます。 そのデータ駆動テストとは、複数の 2 未満の値を調べて、最も小さい素数を特定するという手法です。

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-mstest/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

`dotnet test` を実行して、これらの 2 つのテストが失敗したとします。 すべてのテストを成功させるために、メソッドの先頭にある `if` 句を変更します。

```csharp
if (candidate < 2)
```

他のテスト、理論、コードをメイン ライブラリに追加して、反復を続けます。 [テストの最終版](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.cs)ができ、[ライブラリの完全な実装](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService/PrimeService.cs)が完了しました。

これで、小さなライブラリとそのライブラリの単体テストのセットが構築されました。 ソリューションを構築したことで、新しいパッケージとテストの追加が通常のワークフローに組み込まれました。 アプリケーションの目標を達成することに時間と労力の多くを割き、集中して取り組みました。

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualStudio.TestTools.UnitTesting>
- [単体テストでの MSTest フレームワークの使用](/visualstudio/test/using-microsoft-visualstudio-testtools-unittesting-members-in-unit-tests)
- [MSTest V2 テスト フレームワーク ドキュメント](https://github.com/Microsoft/testfx-docs)
