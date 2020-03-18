---
title: .NET Core について
description: .NET Core について説明します。
ms.date: 09/17/2019
ms.openlocfilehash: 89740b67b294650f78cf36361548c2fe24ac80cb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79147361"
---
# <a name="about-net-core"></a>.NET Core について

.NET Core には次の特徴があります。

- **クロスプラットフォーム:** Windows、macOS、Linux の[オペレーティング システム](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md)で実行されます。
- **アーキテクチャ全体で一貫性がある:** x64、x86、ARM を含めた複数のアーキテクチャ上でコードが同じ動作で実行されます。
- **コマンドライン ツール:** ローカル開発と継続的インテグレーションのシナリオで使用できる、使いやすいコマンドライン ツールが含まれます。
- **柔軟な展開:** アプリに含めることも、横並びにインストールすること (ユーザー全体またはシステム全体のインストール) もできます。 [Docker コンテナー](docker/introduction.md)で使用できます。
- **互換性:** .NET Core は、[.NET Standard](../standard/net-standard.md) 経由で .NET Framework、Xamarin、Mono と互換性があります。
- **オープン ソース:** .NET Core プラットフォームはオープン ソースであり、MIT および Apache 2 ライセンスを使用します。 .NET Core は [.NET Foundation](https://dotnetfoundation.org/) プロジェクトです。
- **Microsoft によるサポート:** .NET Core は、[.NET Core サポート](https://dotnet.microsoft.com/platform/support/policy)ごとに Microsoft によってサポートされます。

## <a name="languages"></a>言語

.NET Core のアプリケーションとライブラリを記述するには、C#、Visual Basic および F# 言語を使用できます。 これらの言語は、任意のテキスト エディターまたは統合開発環境 (IDE) で使用できます。これには次のものが含まれます。

- [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)
- [Visual Studio Code](https://code.visualstudio.com/download)
- サブライム テキスト
- Vim

この統合は、[OmniSharp](https://www.omnisharp.net/) および [Ionide](http://ionide.io) プロジェクトの共同作成者によって、一部提供されています。

## <a name="apis"></a>API

.NET Core は多くのシナリオに対応する API を公開しています。次のうちのいくつかを次に示します。

- <xref:System.Boolean?displayProperty=nameWithType> や <xref:System.Int32?displayProperty=nameWithType> などのプリミティブ型。
- <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> や <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> などのコレクション。
- <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> や <xref:System.IO.FileStream?displayProperty=nameWithType> などのユーティリティ型。
- <xref:System.Data.DataSet?displayProperty=nameWithType> や [DbSet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/) などのデータ型。
- <xref:System.Numerics.Vector?displayProperty=nameWithType> や [Pipelines](../standard/io/pipelines.md) などの高パフォーマンス型。

.NET core では [.NET Standard](../standard/net-standard.md) 仕様を実装することで .NET Framework や Mono の API との互換性を提供します。

## <a name="frameworks"></a>Frameworks

.NET Core 上には、次のような複数のフレームワークが構築されています。

- [ASP.NET Core](/aspnet/core/)
- [Windows 10 ユニバーサル Windows プラットフォーム (UWP)](https://developer.microsoft.com/windows)
- [Tizen](https://developer.tizen.org/development/training/.net-application)

## <a name="composition"></a>コンポジション

.NET Core は、次の部分で構成されます。

- 型システム、アセンブリ読み込み、ガベージ コレクター、ネイティブ相互運用機能、およびその他の基本的なサービスを提供する [.NET Core ランタイム](https://github.com/dotnet/runtime/tree/master/src/coreclr)。 [.NET Core フレームワーク ライブラリ](https://github.com/dotnet/runtime/tree/master/src/libraries)はプリミティブ データ型、アプリ コンポジションの種類、および基本的なユーティリティを提供します。
- Web アプリ、IoT アプリ、モバイル バックエンドなど、最新のクラウド ベースのインターネットに接続されているアプリケーションを構築するためのフレームワークを提供する [ASP.NET Core ランタイム](https://github.com/dotnet/aspnetcore)。
- .NET Core の開発者エクスペリエンスを実現する [.NET Core CLI](https://github.com/dotnet/sdk) および言語コンパイラ ([Roslyn](https://github.com/dotnet/roslyn) と [F#](https://github.com/microsoft/visualfsharp))。
- .NET Core アプリと CLI コマンドの起動に使用する [dotnet コマンド](./tools/dotnet.md)。 ランタイムの選択、ランタイムのホスト、アセンブリ読み込みポリシーの提供、アプリおよびツールの起動を行います。

これらのコンポーネントは、次の方法で配布されます。

- [.NET Core ランタイム](https://dotnet.microsoft.com/download) - .NET Core のランタイムおよびフレームワーク ライブラリが含まれています。
- [ASP.NET Core ランタイム](https://dotnet.microsoft.com/download) - ASP.NET Core ランタイムおよび .NET Core のランタイムおよびフレームワーク ライブラリが含まれています。
- [.NET core SDK](https://dotnet.microsoft.com/download) -- .NET Core CLI、ASP.NET Core ランタイム、.NET Core ランタイムおよびフレームワークが含まれています。

### <a name="open-source"></a>ソースを開く

[.NET Core](https://github.com/dotnet/core) はオープン ソース ([MIT ライセンス](https://github.com/dotnet/core/blob/master/LICENSE.TXT)) であり、2014 年に Microsoft によって [.NET Foundation](https://dotnetfoundation.org) に提供されたものです。 現在では、最もアクティブな .NET Foundation プロジェクトの 1 つとなっています。 個人、学術、商用などの目的で、個人および企業が使用できます。 複数の企業が、アプリ、ツール、新しいプラットフォーム、およびホスティング サービスの一部として .NET Core を使用しています。 これらの企業の一部は、GitHub の .NET Core に多大な貢献をしており、[.NET Foundation テクニカル ステアリング グループ](https://dotnetfoundation.org/blog/tsg-welcome)の一員として製品の方向性に関するガイダンスを提供しています。

### <a name="designed-for-adaptability"></a>適応できる設計

.NET Core は、その他の .NET 製品と比較すると似ていますが、独自の製品として構築されています。 新しいプラットフォームとワークロードに幅広く適応できるように設計されており、複数の OS および CPU ポートを使用できます (また、移植先が増える可能性があります)。

この製品は複数の部分に分割されており、さまざまな時間で新しいプラットフォームにさまざまな部分を適応させることができます。 ランタイムとプラットフォーム固有の基本的なライブラリは、ユニットとして移植する必要があります。 プラットフォームに依存しないライブラリは、構造により、すべてのプラットフォームでそのまま機能します。 開発者の効率性を高めるために、プロジェクトではプラットフォーム固有の実装を低減する傾向にあり、その方向でアルゴリズムまたは API を完全または部分的に実装できる場合は、プラットフォームに依存しない C# コードが常に優先されます。

ユーザーから、複数のオペレーティング システムをサポートするには .NET Core をどのように実装すべきかという質問をよく受けます。 多いのは、個別の実装を行うのか、または[条件付きコンパイル](https://en.wikipedia.org/wiki/Conditional_compilation)を使用するのかという質問です。 どちらも正しいですが、条件付きコンパイルが特に好まれる傾向にあります。

次の図に示すように、[.NET Core ライブラリ](https://github.com/dotnet/runtime/tree/master/src/libraries)の大部分は、すべてのプラットフォーム間で共有されるプラットフォームに依存しないコードです。 プラットフォームに依存しないコードは、すべてのプラットフォームで使用される 1 つのポータブル アセンブリとして実装できます。

![CoreFX:プラットフォームごとのコードの行](../images/corefx-platforms-loc.png)

Windows 実装と Unix 実装はほぼ同じサイズです。 .NET Core ライブラリでは、[Microsoft.Win32.Registry](https://github.com/dotnet/runtime/tree/master/src/libraries/Microsoft.Win32.Registry) などの Windows 専用の機能がいくつか実装されていますが、Unix 専用の概念はまだあまり実装されていないため、Windows の実装の方が大きくなります。 また、Linux 実装と macOS 実装の大部分は Unix 実装全体で共有されており、Linux 固有の実装と macOS 固有の実装はほぼ同じサイズです。

.NET Core には、プラットフォーム固有のライブラリとプラットフォームに依存しないライブラリが混在しています。 このパターンは次のいくつかの例に見られます。

- [CoreCLR](https://github.com/dotnet/runtime/tree/master/src/coreclr) はプラットフォーム固有です。 メモリ マネージャーやスレッド スケジューラのような OS サブシステムの上に構築します。
- [System.IO](https://github.com/dotnet/runtime/tree/master/src/libraries/System.IO) と [System.Security.Cryptography.Algorithms](https://github.com/dotnet/runtime/tree/master/src/libraries/System.Security.Cryptography.Algorithms) は、記憶域および暗号化 API が各 OS で異なるので、プラットフォーム固有です。
- [System.Collections](https://github.com/dotnet/runtime/tree/master/src/libraries/System.Collections) と [System.Linq](https://github.com/dotnet/runtime/tree/master/src/libraries/System.Linq) は、データ構造上で作成および操作を行うので、プラットフォームに依存しません。

## <a name="comparisons-to-other-net-implementations"></a>その他の .NET 実装との比較

.NET Core のサイズとシェイプは、既存の .NET 実装と比較するとよくわかります。

### <a name="comparison-with-net-framework"></a>.NET Framework との比較

.NET は、最初に 2000 年に Microsoft によって発表され、進化してきました。 .NET Framework はその後 20 年近くにわたり、Microsoft によって製造される主要な .NET 実装となっています。

.NET Core と .NET Framework の主な違いは、次のとおりです。

- **アプリ モデル** -- .NET Core は一部の .NET Framework アプリ モデルをサポートしていません。 具体的には、ASP.NET Web フォームと ASP.NET MVC はサポートしませんが、ASP.NET Core MVC をサポートしています。 また、.Net Core 3.0 以降、.NET Core は Windows 上の WPF と Windows フォームのみをサポートしています。
- **API** -- .NET Core には、ファクタリングが異なる (アセンブリ名が異なる、型に対して公開されているメンバーが主要なケースで異なる) .NET Framework 基本クラス ライブラリの大規模なサブセットが含まれています。 場合によっては、これらの違いにより、.NET Core へのポート ソースの変更が必要になることがあります。 詳細については、[.NET Portability Analyzer](../standard/analyzers/portability-analyzer.md) に関するページを参照してください。 .NET Core は [.NET Standard](../standard/net-standard.md) API 仕様を実装します。
- **サブシステム** -- .NET Core は、より単純な実装とプログラミング モデルを目的として、.NET Framework 内のサブシステムのサブセットを実装します。 たとえば、コード アクセス セキュリティ (CAS) はサポートされていませんが、リフレクションはサポートされています。
- **プラットフォーム** -- .NET Framework は Windows と Windows Server をサポートしており、.NET Core は macOS と Linux もサポートしています。
- **オープン ソース** -- .NET Core はオープン ソースであり、[.NET Framework の読み取り専用のサブセット](https://github.com/microsoft/referencesource)はオープン ソースです。

.NET Core はユニークな製品で、.NET Framework およびその他の .NET 実装とは大きな違いがありますが、ソースまたはバイナリの共有技術を使用してこれらの実装の間でコードを簡単に共有できます。

.NET Core は横並びのインストールに対応しており、そのランタイムは .NET Framework から完全に独立しているため、.NET Framework がインストールされているコンピューターに何の問題もなくインストールできます。

### <a name="comparison-with-mono"></a>Mono との比較

[Mono](https://www.mono-project.com/) は .NET の元のクロスプラットフォーム実装です。 .NET Framework の[オープンソース](https://github.com/mono/mono)の代替として始まり、iOS および Android デバイスが普及するにつれてモバイル デバイスをターゲットとするように移行してきました。 .NET Framework のコミュニティの複製として考えることができます。 Mono プロジェクト チームは、互換性のある実装を提供するために、Microsoft によって発行されたオープン [.NET standards](https://github.com/dotnet/runtime/blob/master/docs/project/dotnet-standards.md) (特に ECMA 335) に依存していました。

.NET Core と .NET Mono の主な違いは、次のとおりです。

- **アプリモデル** -- Mono は、Xamarin 製品を介して .NET Framework アプリモデル (たとえば、Windows フォーム) のサブセット、およびモバイル開発用のいくつかの追加のもの (たとえば、[Xamarin.iOS](https://www.xamarin.com/platform)) をサポートしています。 .NET Core では、Xamarin をサポートしていません。
- **API** -- Mono は、.NET Framework API の[大規模なサブセット](http://docs.go-mono.com/?link=root%3a%2fclasslib)をサポートしており、同じアセンブリ名およびファクタリングを使用します。
- **プラットフォーム** -- Mono は、さまざまなプラットフォームおよび CPU をサポートしています。
- **オープン ソース** --Mono と .NET Core は両方とも MIT ライセンスを使用しており、.NET Foundation プロジェクトです。
- **フォーカス** -- 近年、Mono はモバイル プラットフォームに重点を置いており、.NET Core はクラウドとデスクトップのワークロードを重視しています。

## <a name="the-future"></a>将来

.NET 5 は .NET Core の次期リリースであり、プラットフォームの統合を表していることが発表されました。 このプロジェクトは、次のいくつかの主要な方法で .NET を改善することを目的としています。

- 共通のランタイム動作と開発者エクスペリエンスを備えた、あらゆる場所で使用できる 1 つの .NET ランタイムとフレームワークを生成します。
- .Net Core、.NET Framework、Xamarin、Mono を最大限に活用して .NET の機能を拡張します。
- 開発者 (Microsoft とコミュニティ) が連携して取り組んで拡張し、すべてのシナリオを向上させることができる単一のコードベースから製品を構築します。

.NET 5 向けの計画内容の詳細については、「[Introducing .NET 5](https://devblogs.microsoft.com/dotnet/introducing-net-5/)」(.NET 5 の概要) を参照してください。
