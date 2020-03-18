---
title: .NET アーキテクチャ コンポーネント
description: .NET Standard、.NET 実装、.NET ランタイム、ツールなど、.NET アーキテクチャ コンポーネントについて説明します。
author: cartermp
ms.date: 08/23/2017
ms.technology: dotnet-standard
ms.openlocfilehash: eadcf05069edfa32a52c5e73045b4cebd1a9a6ac
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398761"
---
# <a name="net-architectural-components"></a>.NET アーキテクチャ コンポーネント

.NET アプリは、1 つまたは複数の *.NET 実装*向けに開発され、実行されます。  .NET 実装には、.NET Framework、.NET Core、および Mono が含まれます。 すべての .NET 実装に共通する API 仕様があり、それを.NET Standard と呼びます。 この記事では、それぞれの概念について簡単に説明します。

## <a name="net-standard"></a>.NET Standard

.NET Standard は、.NET 実装の基本クラス ライブラリで実装されている API のセットです。 さらに厳密に言うと、コードのコンパイル対象である統一されたコントラクトのセットを構成する .NET API の仕様です。 これらのコントラクトが各 .NET 実装に実装されています。 これにより異なる .NET 実装間で移植することができるため、実質的にコードをどこでも実行できるようになります。

.NET Standard は、[ターゲット フレームワーク](glossary.md#target-framework)でもあります。 コードが .NET Standard のバージョンをターゲットにする場合、.NET Standard のそのバージョンをサポートするすべての .NET 実装でそのコードを実行できます。

.NET Standard とそのターゲットの設定方法については、「[.NET Standard](net-standard.md)」を参照してください。

## <a name="net-implementations"></a>.NET 実装

各 .NET 実装には、次のコンポーネントが含まれています。

- 1 つまたは複数のランタイム。 次に例を示します。 CLR for .NET Framework、CoreCLR、CoreRT for .NET Core などです。
- .NET Standard を実装し、他の API も実装する可能性があるクラス ライブラリ。 たとえば、.NET Framework 基本クラス ライブラリや .NET Core 基本クラス ライブラリなどです。
- 必要に応じて、1 つまたは複数のアプリケーション フレームワーク。 次に例を示します。 .NET Framework と .NET Core には、[ASP.NET](https://www.asp.net/)、[Windows フォーム](../framework/winforms/windows-forms-overview.md)、[Windows Presentation Foundation (WPF)](../framework/wpf/index.md) が含まれます。
- 必要に応じて、開発ツール。 一部の開発ツールは、複数の実装間で共有されます。

Microsoft が積極的に開発し保守している主要な .NET 実装としては、.NET Core、.NET Framework、Mono、UWP の 4 つがあります。

### <a name="net-core"></a>.NET Core

.NET Core は .NET のクラスプラットフォーム実装であり、サーバーとクラウドのワークロードをその規模に応じて処理するように設計されています。 Windows、macOS、および Linux で実行されます。 .NET Standard を実装しているので、.NET Standard をターゲットとするすべてのコードを .NET Core 上で実行できます。 [ASP.NET](https://dotnet.microsoft.com/learn/aspnet/what-is-aspnet-core)、[Windows フォーム](../framework/winforms/windows-forms-overview.md)、[Windows Presentation Foundation (WPF)](../framework/wpf/index.md) はすべて、.NET Core で実行されます。

.NET Core の詳細については、[.NET Core に関するページ](../core/index.md)、および「[サーバー アプリ用 .NET Core と .NET Framework の選択](choosing-core-framework-server.md)」を参照してください。

### <a name="net-framework"></a>.NET Framework

.Net Framework は、2002 年からリリースされている元の .NET 実装です。 既存の .NET 開発者が常に使用してきたものと同じ .NET Framework です。 バージョン 4.5 以降では .NET Standard を実装しているので、.NET Standard をターゲットとするすべてのコードが .NET Framework 4.5 以降で実行できます。 Windows フォームと WPF での Windows デスクトップ開発用 API など、追加の Windows 固有 API が含まれます。 .NET Framework は、Windows デスクトップ アプリケーション開発用に最適化されています。

.NET Framework について詳しくは、[.NET Framework に関するページ](../framework/index.md)をご覧ください。

### <a name="mono"></a>Mono

Mono は、主に小規模なランタイムが必要な場合に使用される .NET 実装です。 Android、macOS、iOS、tvOS、および watchOS 上の Xamarin アプリケーションで利用されるランタイムで、フットプリントが小さいことに重点を置いています。 Mono は、Unity エンジンを使用して構築されたゲームでも利用されます。

現在公開されているすべての .NET Standard バージョンをサポートしています。

これまで Mono は .NET Framework の多数の API を実装し、Unix で人気の高い機能の一部をエミュレートしていました。 また、Unix のそのような機能に依存する .NET アプリケーションを実行するために使用されることもあります。

一般的に Mono は、Just-In-Time コンパイラと共に使用されますが、iOS のようなプラットフォームに使用される完全な静的コンパイラ (Ahead Of Time コンパイル) としても機能します。

Mono について詳しくは、[Mono のドキュメント](https://www.mono-project.com/docs/)をご覧ください。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

UWP は、モノのインターネット (IoT) 用に最新のタッチ対応の Windows アプリケーションとソフトウェアを構築するために使われる .NET 実装です。 PC、タブレット、ファブレット、携帯電話、Xbox など、ターゲットにする可能性があるさまざまな種類のデバイスを統一するように設計されています。 UWP は、一元的なアプリ ストア、実行環境 (AppContainer)、Win32 の代わりに使う Windows API のセット (WinRT) など、多くのサービスを提供します。 アプリは、C++、C#、Visual Basic、および JavaScript で記述することができます。 C# と Visual Basic を使うときは、.NET Core によって .NET API が提供されます。

UWP の詳細については、「[ユニバーサル Windows プラットフォームの紹介](/windows/uwp/get-started/universal-application-platform-guide)」を参照してください。

## <a name="net-runtimes"></a>.NET ランタイム

ランタイムは、マネージド プログラムの実行環境です。 OS は、ランタイム環境の一部ですが、.NET ランタイムの一部ではありません。 .NET ランタイムの例を次に示します。

- .NET Framework 用共通言語ランタイム (CLR)
- .NET Core 用共通言語ランタイム (CoreCLR)
- ユニバーサル Windows プラットフォーム用 .NET Native
- Xamarin.iOS、Xamarin.Android、Xamarin.Mac、Mono デスクトップ フレームワーク用ランタイム

## <a name="net-tooling-and-common-infrastructure"></a>.NET のツールと共通インフラストラクチャ

すべての .NET 実装と連携する多様なツールやインフラストラクチャ コンポーネントにアクセスできます。 その一部を次に示します。

- .NET 言語とコンパイラ
- .NET プロジェクト システム ( *.csproj*、 *.vbproj*、および *.fsproj* ファイルに基づく)
- [MSBuild](/visualstudio/msbuild/msbuild) (プロジェクトのビルドに使用されるビルド エンジン)
- [NuGet](/nuget/) (Microsoft の .NET 用パッケージ マネージャー)
- オープン ソースのビルド オーケストレーション ツール ([CAKE](https://cakebuild.net/)、[FAKE](https://fake.build/) など)

## <a name="applicable-standards"></a>適用可能な標準

C# 言語および共通言語基盤 (CLI) の仕様は、[Ecma International®](https://www.ecma-international.org/) を介して標準化されています。 これらの標準の初版は、2001 年 12 月に Ecma によって公開されました。

標準の後続の改訂は、Programming Languages Technical Committee ([TC49](https://www.ecma-international.org/memento/tc49.htm)) 内の TC49-TG2 (C#) および TC49-TG3 (CLI) タスク グループによって展開され、Ecma General Assembly で採用され、その後、ISO Fast-Track プロセスを介して ISO/IEC JTC 1 で採用されました。

### <a name="latest-standards"></a>最新の標準

[C#](http://www.ecma-international.org/publications/standards/Ecma-334.htm) および [CLI](http://www.ecma-international.org/publications/standards/Ecma-335.htm) ([TR-84](http://www.ecma-international.org/publications/techreports/E-TR-084.htm)) については、次の公式の Ecma ドキュメントを入手できます。

- **The C# Language Standard (version 5.0)** (C# 言語標準 (バージョン 5.0)):[ECMA-334.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf)
- **The Common Language Infrastructure** (共通言語基盤):これは、[pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf) 形式と [zip](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.zip) 形式で入手できます。
- **Information Derived from the Partition IV XML File** (Partition IV XML ファイルから派生した情報):これは、[pdf](https://www.ecma-international.org/publications/files/ECMA-TR/ECMA%20TR-084.pdf) および [zip](https://www.ecma-international.org/publications/files/ECMA-TR/TR-084.zip) 形式で入手できます。

公式の ISO/IEC ドキュメントは、ISO/IEC の「[Publicly Available Standards](https://standards.iso.org/ittf/PubliclyAvailableStandards/)」(公開されている標準) ページから入手できます。 そのページのリンクを次に示します。

- **情報技術 - プログラミング言語 - C#** :[ISO/IEC 23270:2018](https://standards.iso.org/ittf/PubliclyAvailableStandards/c075178_ISO_IEC_23270_2018.zip)
- **情報技術 - 共通言語基盤 (CLI) パーティション I から VI**:[ISO/IEC 23271:2012](https://standards.iso.org/ittf/PubliclyAvailableStandards/c058046_ISO_IEC_23271_2012(E).zip)
- **情報技術 - 共通言語基盤 (CLI) - Partition IV XML ファイルから派生した情報に関するテクニカル レポート**:[ISO/IEC TR 23272:2011](https://standards.iso.org/ittf/PubliclyAvailableStandards/c057955_ISO_IEC_TR_23272_2011.zip)

## <a name="see-also"></a>関連項目

- [サーバー アプリ用 .NET Core と .NET Framework の選択](choosing-core-framework-server.md)
- [.NET Standard](net-standard.md)
- [.NET Core のガイド](../core/index.md)
- [.NET Framework ガイド](../framework/index.md)
- [C# のガイド](../csharp/index.yml)
- [F# のガイド](../fsharp/index.yml)
- [Visual Basic のガイド](../visual-basic/index.yml)
