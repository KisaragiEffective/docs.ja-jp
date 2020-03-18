---
title: .NET Portability Analyzer - .NET
description: .NET Portability Analyzer ツールを使って、さまざまな .NET の実装 (.NET Core、.NET Standard、UWP、Xamarin など) の間でのコードの移植性を評価する方法について説明します。
ms.date: 09/13/2019
ms.technology: dotnet-standard
ms.assetid: 0375250f-5704-4993-a6d5-e21c499cea1e
ms.openlocfilehash: e0a5c791926b36fe5a35c5446471c3dcdb75cd7b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "72774390"
---
# <a name="the-net-portability-analyzer"></a>.NET Portability Analyzer

ライブラリでマルチプラットフォームをサポートしたい場合や、 .NET Framework アプリケーションを .NET Core で実行するのに必要な作業量を知りたい場合、 [.NET Portability Analyzer](https://github.com/microsoft/dotnet-apiport) というツールがアセンブリを分析し、指定された対象の .NET プラットフォームでアプリケーションまたはライブラリを移植するために不足している .NET API の詳細なレポートを提供します。 [Visual Studio 拡張機能](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)として提供されている Portability Analyzer ではプロジェクトごとにアセンブリが 1 つ分析され、[ApiPort コンソール アプリ](https://aka.ms/apiportdownload)として提供されている Portability Analyzer では指定したファイルまたはディレクトリごとにアセンブリが分析されます。

.NET Core などの新しいプラットフォームを対象とするようにプロジェクトを変換したら、Roslyn ベースの [API Analyzer ツール](api-analyzer.md)を使用して、<xref:System.PlatformNotSupportedException> の例外とその他の互換性の問題をスローする API を特定することができます。

## <a name="common-targets"></a>一般的なターゲット

- [.NET Core](../../core/index.md): モジュール型の設計で、side-by-side を採用しており、クロスプラットフォームのシナリオを対象としています。 side-by-side 機能により、他のアプリに影響を与えることなく新しい .NET Core バージョンを導入することができます。 クロスプラットフォームをサポートする .NET Core にアプリを移植することが目的である場合は、これが推奨されるターゲットです。
- [.NET Standard](../../standard/net-standard.md): .NET のすべての実装で使用できる .NET Standard API が含まれています。 .NET でサポートされるすべてのプラットフォームでライブラリを実行させることが目的である場合は、これが推奨されるターゲットです。
- [ASP.NET Core](/aspnet/core): .NET Core 上に構築された最新の Web フレームワークです。 複数のプラットフォームをサポートするために .NET Core に Web アプリを移植することが目的である場合は、これが推奨されるターゲットです。
- .NET Core と[プラットフォーム拡張機能](../../core/porting/windows-compat-pack.md): Windows 互換機能パックに加えて .NET Core API が含まれます。 .NET Framework で利用可能なテクノロジの多くが提供されます。 Windows で .NET Framework から .NET Core にアプリを移植する場合は、これが推奨されるターゲットです。
- .NET Standard と[プラットフォーム拡張機能](../../core/porting/windows-compat-pack.md): Windows 互換機能パックに加えて .NET Standard API が含まれます。 .NET Framework で利用可能なテクノロジの多くが提供されます。 Windows で .NET Framework から .NET Core にライブラリを移植する場合は、これが推奨されるターゲットです。

## <a name="how-to-use-the-net-portability-analyzer"></a>.NET Portability Analyzer の使用方法

Visual Studio で .NET Portability Analyzer を使用するには、[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) から拡張機能をダウンロードし、インストールする必要があります。 Visual Studio 2017 以降のバージョンで機能します。 Visual Studio で構成するには、 **[Analyze]\(分析\)**  >  **[Portability Analyzer Settings]\(Portability Analyzer の設定\)** でターゲット プラットフォームを選択します。ターゲット プラットフォームは、現在のアセンブリがビルドされているプラットフォーム/バージョンと比較して移植性のギャップを評価する .NET プラットフォーム/バージョンです。

![移植性アナライザーのスクリーンショット。](./media/portability-analyzer/portability-screenshot.png)

ApiPort コンソール アプリケーションを使用して、[ApiPort リポジトリ](https://aka.ms/apiportdownload)からダウンロードすることもできます。 `listTargets` コマンド オプションを使って使用可能なターゲットの一覧を表示した後、`-t` または `--target` コマンド オプションを指定することによってターゲット プラットフォームを選択できます。

### <a name="analyze-portability"></a>移植性を分析する
Visual Studio でプロジェクト全体を分析するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、 **[Analyze Assembly Portability]\(アセンブリの移植性を分析する\)** を選択します。 または、 **[分析]** メニューで **[Analyze Assembly Portability]** (アセンブリの移植性を分析) を選択します。 そこから、プロジェクトの実行可能ファイルまたは DLL を選択します。

![ソリューション エクスプローラーからの移植性アナライザーのスクリーンショット。](./media/portability-analyzer/portability-solution-explorer.png)

[ApiPort コンソール アプリ](https://aka.ms/apiportdownload)を使うこともできます。

- 現在のディレクトリを分析するには、次のコマンドを入力します。`ApiPort.exe analyze -f .`
- 特定の .dll ファイルの一覧を分析するには、次のコマンドを入力します。`ApiPort.exe analyze -f first.dll -f second.dll -f third.dll`
- 詳細なヘルプを表示するには `ApiPort.exe -?` を実行します

自分が所有していて移植したいすべての関連する exe と dll ファイルを含め、アプリが依存しているけれども自分で所有しているのではなく移植できないファイルを除外することをお勧めします。 これにより、最も関連のある移植性レポートが得られます。

### <a name="view-and-interpret-portability-result"></a>移植性の結果を表示して解釈する

レポートには、ターゲット プラットフォームによってサポートされていない API のみが表示されます。
Visual Studio で分析を実行すると、.NET 移植性レポート ファイルのリンクがポップアップ表示されます。 [ApiPort コンソール アプリ](https://aka.ms/apiportdownload)を使った場合は、.NET 移植性レポートは指定した形式のファイルとして保存されます。 既定では、現在のディレクトリの Excel ファイル ( *.xlsx*) です。

#### <a name="portability-summary"></a>移植性の概要

![移植性の概要のスクリーンショット。](./media/portability-analyzer/api-catalog-portablility-summary.png)

レポートの [Portability Summary]\(移植性の概要\) セクションでは、実行に含まれる各アセンブリの移植性の割合が示されます。 前の例では、`svcutil` アプリで使われている .NET Framework API の 71.24% が、.NET Core とプラットフォーム拡張機能で使用できます。 複数のアセンブリに対して .NET Portability Analyzer ツールを実行した場合、移植性の概要レポートでは各アセンブリが 1 行に表示されます。

#### <a name="details"></a>説明

![移植性の詳細のスクリーンショット。](./media/portability-analyzer/api-catalog-portablility-details.png)

レポートの **[説明]** セクションには、選択した**ターゲット プラットフォーム**のいずれからも欠落している API が一覧表示されます。

- [Target type]\(ターゲットの型\): 型にターゲット プラットフォームに存在しない API があります
- [Target member]\(ターゲットのメンバー\): メンバーがターゲット プラットフォームにありません
- [Assembly name]\(アセンブリ名\): ない API が存在する .NET Framework アセンブリです。
- 選択されているターゲット プラットフォームごとに 1 つの列 (".NET Core" など): [Not supported]\(サポートされていません\) という値は、その API がこのターゲット プラットフォームでサポートされていないことを意味します。
- [Recommended Changes]\(推奨される変更\): それに変更することが推奨される API またはテクノロジです。 現時点では、このフィールドは、多くの API で空または期限切れです。 API の数が多いので、最新の状態に保つのは大変な作業です。 お客様に役に立つ情報を提供する代わりのソリューションを探しています。

#### <a name="missing-assemblies"></a>足りないアセンブリ

![足りないアセンブリのスクリーンショット。](./media/portability-analyzer/api-catalog-missing-assemblies.png)

レポートには [Missing Assemblies]\(足りないアセンブリ\) セクションが含まれる場合があります。 そこに示されているアセンブリの一覧は、分析されたアセンブリによって参照されていますが、分析されませんでした。 自分が所有しているアセンブリの場合は、API Portability Analyzer の実行にそれを含めて、そのアセンブリに関する API レベルの詳細な移植性レポートを取得できます。 サード パーティのライブラリである場合は、ターゲット プラットフォームをサポートしている新しいバージョンがあるかどうかを調べます。 ある場合は、新しいバージョンへの移行を検討します。 最終的に、この一覧には、アプリが依存していて、ターゲット プラットフォームをサポートするバージョンがあることが確認された、すべてのサード パーティ アセンブリが含まれることが期待されます。

.NET Portability Analyzer の詳細については、[GitHub ドキュメント](https://github.com/Microsoft/dotnet-apiport#documentation)にアクセスし、Channel 9 動画の「[A Brief Look at the .NET Portability Analyzer](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)」 (.NET Portability Analyzer の概要) をご覧ください。
