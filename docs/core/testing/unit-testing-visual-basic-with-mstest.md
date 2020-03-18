---
title: dotnet テストと MSTest を使用した .NET Core での単体テスト Visual Basic
description: MSTest を使用した Visual Basic ソリューションのサンプルを段階的に構築していく対話型エクスペリエンスを通じて、.NET Core の単体テストの概念について説明します。
author: billwagner
ms.author: wiwagn
ms.date: 09/01/2017
ms.openlocfilehash: df167e0559c841510df17ba39801e43315036241
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240936"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-mstest"></a>dotnet テストと MSTest を使用した .NET Core ライブラリでの単体テスト Visual Basic

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-vb-mstest/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a>ソース プロジェクトの作成

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-vb-mstest* というディレクトリを作成します。
この新しいディレクトリ内で [`dotnet new sln`](../tools/dotnet-new.md) を実行して、ソリューションを新たに作成します。 こうすることで、クラス ライブラリと単体テスト プロジェクトの両方を管理しやすくなります。
ソリューションのディレクトリ内で、*PrimeService* ディレクトリを作成します。 現時点のディレクトリとファイルの構造は次のようになっています。

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
```

*PrimeService* を現在のディレクトリにし、[`dotnet new classlib -lang VB`](../tools/dotnet-new.md) を実行してソース プロジェクトを作成します。 *Class1.VB* の名前を *PrimeService.VB* に変更します。 `PrimeService` クラスのエラーが発生する実装を作成します。

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first")
        End Function
    End Class
End Namespace
```

*unit-testing-vb-using-mstest* ディレクトリに戻ります。 [`dotnet sln add .\PrimeService\PrimeService.vbproj`](../tools/dotnet-sln.md) を実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

## <a name="creating-the-test-project"></a>テスト プロジェクトの作成

次に、*PrimeService.Tests* ディレクトリを作成します。 次の一覧はディレクトリ構造を示したものです。

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

*PrimeService.Tests* ディレクトリを現在のディレクトリにし、[`dotnet new mstest -lang VB`](../tools/dotnet-new.md) を使用して新しいプロジェクトを作成します。 このコマンドによって、テスト ライブラリとして MSTest を使用するテスト プロジェクトが作成されます。 生成されたテンプレートで、*PrimeServiceTests.vbproj* のテスト ランナーが構成されます。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` によって、MSTest と MSTest ランナーが追加されています。 ここで、プロジェクトに別の依存関係として `PrimeService` クラス ライブラリを追加します。 次の [`dotnet add reference`](../tools/dotnet-add-reference.md) コマンドを使用します。

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

全体のファイルは GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService.Tests/PrimeService.Tests.vbproj)で確認できます。

ソリューションの最終的なレイアウトは次のようになります。

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
        Test Source Files
        PrimeServiceTests.vbproj
```

*unit-testing-vb-mstest* ディレクトリで [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj`](../tools/dotnet-sln.md) を実行します。

## <a name="creating-the-first-test"></a>最初のテストの作成

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *PrimeService.Tests* ディレクトリから *UnitTest1.vb* を削除し、*PrimeService_IsPrimeShould.VB* という名前の新しい Visual Basic ファイルを作成します。 次のコードを追加します。

```vb
Imports Microsoft.VisualStudio.TestTools.UnitTesting

Namespace PrimeService.Tests
    <TestClass>
    Public Class PrimeService_IsPrimeShould
        Private _primeService As Prime.Services.PrimeService = New Prime.Services.PrimeService()

        <TestMethod>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.IsFalse(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

`<TestClass>` 属性は、テストを含むクラスを表します。 `<TestMethod>` 属性は、テスト ランナーによって実行されるメソッドを表します。 *unit-testing-vb-mstest* で [`dotnet test`](../tools/dotnet-test.md) を実行してテストとクラス ライブラリをビルドし、それからテストを実行します。 MSTest テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

テストが失敗します。 実装はまだ作成していません。 最も単純な動作のコードを `PrimeService` クラスに記述して、このテストが成功するようにします。

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

*unit-testing-vb-mstest* ディレクトリで、もう一度 `dotnet test` を実行します。 `dotnet test` コマンドは `PrimeService` プロジェクトのビルドを実行してから、`PrimeService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、この単一テストが実行されます。 成功します。

## <a name="adding-more-features"></a>他の機能の追加

テストが成功したので、他のテストも記述してみましょう。 素数に関する、いくつかの単純なケースが他にもあります(0、-1)。 `<TestMethod>` 属性を使用すると、これらの例を新しいテストとして追加できますが、すぐに煩雑になります。 一連の類似のテストを記述できるようになる、他の MSTest 属性があります。  `<DataTestMethod>` 属性は同じコードを実行するものの、異なる入力引数が含まれる一連のテストを表します。 `<DataRow>` 属性を使用して、そのような入力の値を指定することができます。

新しいテストを作成するのではなく、この 2 つの属性を適用することで 1 つの理論を作成できます。 その理論とは、複数の 2 未満の値を調べて、もっとも小さい素数を特定するという手法です。

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-mstest/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

`dotnet test` を実行して、これらの 2 つのテストが失敗したとします。 すべてのテストを成功させるために、メソッドの先頭にある `if` 句を変更します。

```vb
if candidate < 2
```

他のテスト、理論、コードをメイン ライブラリに追加して、反復を続けます。 [テストの最終版](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.vb)ができ、[ライブラリの完全な実装](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService/PrimeService.vb)が完了しました。

これで、小さなライブラリとそのライブラリの単体テストのセットが構築されました。 ソリューションを構築したことで、新しいパッケージとテストの追加が通常のワークフローに組み込まれました。 アプリケーションの目標を達成することに時間と労力の多くを割き、集中して取り組みました。
