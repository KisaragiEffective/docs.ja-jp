---
title: .NET Core と .NET Standard の単体テスト
description: この記事では、.NET Core と .NET Standard プロジェクト用の単体テストの概要を簡単に説明します。
author: ardalis
ms.author: wiwagn
ms.date: 08/30/2017
ms.openlocfilehash: 1263bfe337b9d6609c0ca7df70aa299a61a7f1a0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78157402"
---
# <a name="unit-testing-in-net-core-and-net-standard"></a>.NET Core と .NET Standard の単体テスト

.NET core では、簡単に単体テストを作成できます。 この記事では、単体テストについて紹介し、その他の種類のテストとの違いを示します。 ページの下部にあるリンクされたリソースは、テスト プロジェクトをソリューションに追加する方法を示します。 テスト プロジェクトを設定した後は、コマンドラインまたは Visual Studio を使用して単体テストを実行することができます。

**ASP.NET Core** プロジェクトをテストしている場合は、「[ASP.NET Core の統合テスト](/aspnet/core/test/integration-tests#test-app-prerequisites)」を参照してください。

.NET Core 2.0 以降では [.NET Standard 2.0](../../standard/net-standard.md) がサポートされます。単体テストはそのライブラリを使用して説明します。

C#、F#、Visual Basic 向けに組み込まれている .NET Core 2.0 以降の単体テスト プロジェクト テンプレートを個人プロジェクトの開始点として使用することができます。

## <a name="what-are-unit-tests"></a>単体テストとは

自動テストを備えることは、ソフトウェア アプリケーションを作成者の意図どおりに確実に動作させるための最良の方法です。 ソフトウェア アプリケーションには複数の種類のテストがあります。 統合テスト、Web テスト、ロード テストなどが含まれます。 **単体テスト**は、個々 のソフトウェア コンポーネントとメソッドをテストします。 単体テストでは、開発者のコントロール内のコードのみがテストされる必要があります。 インフラストラクチャの懸念事項をテストすべきではありません。 インフラストラクチャの懸念事項には、データベース、ファイル システム、ネットワーク リソースが含まれます。

テストを記述するためのベスト プラクティスもあります。 たとえば、[テスト駆動開発 (TDD)](https://deviq.com/test-driven-development/) では、チェックされるコードよりも前に単体テストが記述されます。 TDD は本を書く前にアウトラインを作成するのと似ています。 開発者が簡潔で読みやすく、効率的なコードを記述できるように支援します。

> [!NOTE]
> ASP.NET チームは[この規則](https://github.com/dotnet/aspnetcore/wiki/Engineering-guidelines#unit-tests-and-functional-tests)に従って、開発者がテスト クラスとメソッドに適した名前を考えられるように支援します。

単体テストを記述するときは、インフラストラクチャに対する依存関係を設けようとしないでください。 テストが低速で不安定になるため、これは統合テストで行います。 アプリケーションでこれらの依存関係を回避するには、[明示的な依存関係の原則](https://deviq.com/explicit-dependencies-principle/)に従い、[依存関係の挿入](/aspnet/core/fundamentals/dependency-injection)を使用します。 個別のプロジェクトの単体テストを統合テストと区別することもできます。 これにより、単体テスト プロジェクトとインフラストラクチャ パッケージの間に参照や依存関係がなくなります。

## <a name="next-steps"></a>次のステップ

.NET Core プロジェクトでの単体テストの詳細については、次を参照してください。

次に対する .NET Core 単体テスト プロジェクトがサポートされます。

- [C#](../../csharp/index.yml)
- [F#](../../fsharp/index.yml)
- [Visual Basic](../../visual-basic/index.yml)

また、次から選択することもできます。

- [xUnit](https://xunit.github.io)
- [NUnit](https://nunit.org)
- [MSTest](https://github.com/Microsoft/testfx-docs)

次のチュートリアルでさらに詳しく学習できます。

- [*xUnit* と *C#* を使用して .NET Core CLI で単体テストを作成する](unit-testing-with-dotnet-test.md)。
- [*NUnit* と *C#* を使用して .NET Core CLI で単体テストを作成する](unit-testing-with-nunit.md)。
- [*MSTest* と *C#* を使用して .NET Core CLI で単体テストを作成する](unit-testing-with-mstest.md)。
- [*xUnit* と *F#* を使用して .NET Core CLI で単体テストを作成する](unit-testing-fsharp-with-dotnet-test.md)。
- [*NUnit* と *F#* を使用して .NET Core CLI](unit-testing-fsharp-with-nunit.md) で単体テストを作成する。
- [*MSTest* と *F#* を使用して .NET Core CLI で単体テストを作成する](unit-testing-fsharp-with-mstest.md)。
- [*xUnit* と *Visual Basic* を使用して .NET Core CLI で単体テストを作成する](unit-testing-visual-basic-with-dotnet-test.md)。
- [*NUnit* と *Visual Basic* を使用して .NET Core CLI で単体テストを作成する](unit-testing-visual-basic-with-nunit.md)。
- [*MSTest* と *Visual Basic* を使用して .NET Core CLI で単体テストを作成する](unit-testing-visual-basic-with-mstest.md)。

次の記事でさらに詳しく学習できます。

- Visual Studio Enterprise は、.NET Core の優れたテスト ツールを提供します。 [Live Unit Testing](/visualstudio/test/live-unit-testing) または[コード カバレッジ](https://github.com/Microsoft/vstest-docs/blob/master/docs/analyze.md#working-with-code-coverage)をご確認ください。
- 選択的単体テストの実行方法に関する詳細については、「[Running selective unit tests](selective-unit-tests.md)」(選択的単体テストの実行)、または [Visual Studio を使用したテストの組み込みと除外](/visualstudio/test/live-unit-testing#include-and-exclude-test-projects-and-test-methods)に関するトピックをご覧ください。
- [.NET Core と Visual Studio で xUnit を使用する方法](https://xunit.github.io/docs/getting-started-dotnet-core.html)
