---
title: dotnet テストと NUnit を使用した .NET Core での単体テスト Visual Basic
description: NUnit を使用した Visual Basic ソリューションのサンプルを段階的に構築していく対話型エクスペリエンスを通じて、.NET Core の単体テストの概念について説明します。
author: rprouse
ms.date: 10/04/2018
ms.openlocfilehash: a33447457344b241b4c2376d777b0deb7f556874
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240923"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-nunit"></a>dotnet テストと NUnit を使用した Visual Basic .NET Core ライブラリでの単体テスト

このチュートリアルでは、単体テストの概念について学習するためにサンプル ソリューションを段階的に構築する対話型のエクスペリエンスを示します。 構築済みのソリューションを使用してチュートリアルに従う場合は、開始する前に[サンプル コードを参照またはダウンロード](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-vb-nunit/)してください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>前提条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降のバージョン。
- ユーザーが選んだテキスト エディターまたはコード エディター。

## <a name="creating-the-source-project"></a>ソース プロジェクトの作成

シェル ウィンドウを開きます。 ソリューションを保存するための *unit-testing-vb-nunit* というディレクトリを作成します。 この新しいディレクトリ内で、次のコマンドを実行して、クラス ライブラリとテスト プロジェクト用の新しいソリューション ファイルを作成します。

```dotnetcli
dotnet new sln
```

次に、*PrimeService* ディレクトリを作成します。 これまでのところ、ファイルの構造は次のアウトラインのようになっています。

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
```

*PrimeService* を現在のディレクトリとし、次のコマンドを実行してソース プロジェクトを作成します。

```dotnetcli
dotnet new classlib -lang VB
```

*Class1.VB* の名前を *PrimeService.VB* に変更します。 `PrimeService` クラスのエラーが発生する実装を作成します。

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first.")
        End Function
    End Class
End Namespace
```

*unit-testing-vb-using-mstest* ディレクトリに戻ります。 次のコマンドを実行して、クラス ライブラリ プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add .\PrimeService\PrimeService.vbproj
```

## <a name="creating-the-test-project"></a>テスト プロジェクトの作成

次に、*PrimeService.Tests* ディレクトリを作成します。 次の一覧はディレクトリ構造を示したものです。

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

*PrimeService.Tests* ディレクトリを現在のディレクトリとし、次のコマンドを使用して新しいプロジェクトを作成します。

```dotnetcli
dotnet new nunit -lang VB
```

[dotnet new](../tools/dotnet-new.md) コマンドによって、テスト ライブラリとして NUnit を使用するテスト プロジェクトが作成されます。 生成されたテンプレートで、*PrimeServiceTests.vbproj* ファイルのテスト ランナーが構成されます。

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService.Tests.vbproj#Packages)]

テスト プロジェクトには、単体テストを作成して実行するための、他のパッケージが必要です。 前の手順の `dotnet new` では、NUnit と NUnit のテスト アダプターを追加しました。 ここで、プロジェクトに別の依存関係として `PrimeService` クラス ライブラリを追加します。 次の [`dotnet add reference`](../tools/dotnet-add-reference.md) コマンドを使用します。

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

全体のファイルは GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService.Tests.vbproj)で確認できます。

ソリューションの最終的なレイアウトは次のようになります。

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.vbproj
```

*unit-testing-vb-nunit* ディレクトリ内で次のコマンドを実行します。

```dotnetcli
dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj
```

## <a name="creating-the-first-test"></a>最初のテストの作成

失敗するテストを 1 つ作成してそれを合格させる、というプロセスを繰り返します。 *PrimeService.Tests* ディレクトリ内で、*UnitTest1.vb* ファイルの名前を *PrimeService_IsPrimeShould.VB* に変更し、その内容全体を次のコードに置き換えます。

```vb
Imports NUnit.Framework

Namespace PrimeService.Tests
    <TestFixture>
    Public Class PrimeService_IsPrimeShould
        Private _primeService As Prime.Services.PrimeService = New Prime.Services.PrimeService()

        <Test>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.False(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

`<TestFixture>` 属性は、テストを含むクラスを表します。 `<Test>` 属性は、テスト ランナーによって実行されるメソッドを表します。 *unit-testing-vb-nunit* で [`dotnet test`](../tools/dotnet-test.md) を実行してテストとクラス ライブラリをビルドし、それからテストを実行します。 NUnit テスト ランナーには、テストを実行するためのプログラムのエントリ ポイントが含まれています。 `dotnet test` を実行すると、作成した単体テスト プロジェクトを使用してテスト ランナーが開始されます。

テストが失敗します。 実装はまだ作成していません。 最も単純な動作のコードを `PrimeService` クラスに記述して、このテストが成功するようにします。

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

*unit-testing-vb-nunit* ディレクトリで `dotnet test` をもう一度実行します。 `dotnet test` コマンドは `PrimeService` プロジェクトのビルドを実行してから、`PrimeService.Tests` プロジェクトのビルドを実行します。 両方のプロジェクトをビルドすると、この単一テストが実行されます。 成功します。

## <a name="adding-more-features"></a>他の機能の追加

テストが成功したので、他のテストも記述してみましょう。 素数に関する、いくつかの単純なケースが他にもあります (0、-1)。 `<Test>` 属性を使用すると、これらの例を新しいテストとして追加できますが、すぐに煩雑になります。 一連の類似のテストを記述できるようになる、他の xUnit 属性があります。  `<TestCase>` 属性は同じコードを実行するものの、異なる入力引数が含まれる一連のテストを表します。 `<TestCase>` 属性を使用して、そのような入力の値を指定することができます。

新しいテストを作成する代わりに、この 2 つの属性を活用して、最も小さな素数である 2 未満の複数の値をテストする一連のテストを作成しましょう。

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

`dotnet test` を実行して、これらの 2 つのテストが失敗したとします。 すべてのテストを成功させるために、`if`PrimeServices.cs`Main` ファイルで *メソッドの先頭にある* 句を変更します。

```vb
if candidate < 2
```

他のテスト、理論、コードをメイン ライブラリに追加して、反復を続けます。 [テストの最終版](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.vb)ができ、[ライブラリの完全な実装](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService/PrimeService.vb)が完了しました。

これで、小さなライブラリとそのライブラリの単体テストのセットが構築されました。 ソリューションを構築したことで、新しいパッケージとテストの追加が通常のワークフローに組み込まれました。 アプリケーションの目標を達成することに時間と労力の多くを割き、集中して取り組みました。
