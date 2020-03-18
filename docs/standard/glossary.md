---
title: .NET 用語集
description: .NET のドキュメントで使われている用語からいくつか選択してその意味を説明します。
ms.date: 01/22/2019
ms.technology: dotnet-standard
ms.openlocfilehash: 8da1d858835210590a80a624fb8989fbfe8e0a91
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398749"
---
# <a name="net-glossary"></a>.NET 用語集

この用語集の主な目的は、.NET のドキュメントで定義なしに頻繁に出現する用語と頭字語の意味を明確にすることです。

## <a name="aot"></a>AOT

Ahead Of Time コンパイラ。

[JIT](#jit) と同様に、このコンパイラも [IL](#il) をマシン コードに変換します。 JIT コンパイルとは異なり、AOT コンパイルはアプリケーションが実行される前に行われ、通常は、別のコンピューターで実行されます。 AOT ツール チェーンは実行時にコンパイルしないので、コンパイルに費やされる時間を最小限に抑える必要はありません。 つまり、より多くの時間を最適化に費やすことができます。 AOT のコンテキストはアプリケーション全体であるため、AOT コンパイラはモジュール間のリンクとプログラム全体の分析も実行します。これは、すべての参照が追跡されて、1 つの実行可能ファイルが生成されることを意味します。

「[CoreRT](#corert)」と「[.NET Native](#net-native)」を参照してください。

## <a name="aspnet"></a>ASP.NET

.NET Framework に付属している ASP.NET の元の実装。

ASP.NET は、ASP.NET Core を含む ASP.NET の両方の実装を指す包括的な用語として使われることがあります。 どちらを意味するかはコンテキストによって決まります。 両方の実装を意味するために ASP.NET を使っているのではないことを明確にしたい場合は、ASP.NET 4.x を参照してください。

[ASP.NET のドキュメント](/aspnet/#pivot=aspnet)をご覧ください。

## <a name="aspnet-core"></a>ASP.NET Core

.NET Core 上に構築された ASP.NET のクロスプラットフォームで高パフォーマンスなオープン ソースの実装。

[ASP.NET Core のドキュメント](/aspnet/#pivot=core)をご覧ください。

## <a name="assembly"></a>アセンブリ (assembly)

アプリケーションまたは他のアセンブリから呼び出すことができる API のコレクションを含む *.dll*/ *.exe* ファイル。

アセンブリは、インターフェイス、クラス、構造体、列挙型、デリゲートなどの種類を含むことができます。 プロジェクトの *bin* フォルダー内のアセンブリは、"*バイナリ*" と呼ばれることもあります。 「[ライブラリ](#library)」もご覧ください。

## <a name="clr"></a>CLR

共通言語ランタイム (Common Language Runtime)。

厳密な意味はコンテキストによって異なりますが、通常は、.NET Framework のランタイムを指します。 CLR は、メモリの割り当てと管理を行います。 CLR は、アプリの実行だけでなく、[JIT](#jit) コンパイラを使って実行時にコードを生成してコンパイルする仮想マシンでもあります。 現在の Microsoft CLR の実装は Windows だけです。

## <a name="coreclr"></a>CoreCLR

.NET Core 共通言語ランタイム (.NET Core Common Language Runtime)。

この CLR は、CLR と同じコード ベースから作成されます。 もともと、CoreCLR は Silverlight のランタイムであり、複数のプラットフォーム (具体的には Windows と OS X) で実行するように設計されていました。現在の CoreCLR は .NET Core の一部であり、CLR の簡素化されたバージョンを表します。 まだ[クロスプラットフォーム](#cross-platform) ランタイムであり、多くの Linux ディストリビューションのサポートを含むようになっています。 CoreCLR は、JIT とコード実行機能を備えた仮想マシンでもあります。

## <a name="corefx"></a>CoreFX

.NET Core 基本クラス ライブラリ (BCL)

System.\* (および限られた範囲の Microsoft.\*) 名前空間を構成するライブラリのセット。 BCL は汎用の下位レベル フレームワークであり、ASP.NET Core などの上位レベル アプリケーション フレームワークはそれを基にして構築されています。 .NET Core BCL のソース コードは [.NET Core ランタイム リポジトリ](https://github.com/dotnet/runtime)に含まれます。 ただし、.NET Core API の大部分は .NET Framework でも使うことができるため、CoreFX は .NET Framework BCL が分岐したものと考えることができます。

## <a name="corert"></a>CoreRT

.NET Core ランタイム。

CLR/CoreCLR とは異なり、CoreRT は仮想マシンではありません。つまり、[JIT](#jit) が含まれないため、実行時にコードを生成して実行する機能はありません。 ただし、[GC](#gc) およびランタイム型識別 (RTTI) とリフレクションの機能は備えています。 ただ、CoreRT の型システムはリフレクション用のメタデータが必要ないように設計されています。 これにより、[AOT](#aot) ツール チェーンで余分なメタデータのリンクを削除し、(さらに重要なこととして) アプリが使っていないコードを特定することができます。 CoreRT は開発中です。

[.NET Native と CoreRT の概要](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md)をご覧ください。

## <a name="cross-platform"></a>クロスプラットフォーム

Linux、Windows、iOS など、複数の異なるオペレーティング システム上で使用できるアプリケーションを開発し、実行することができます。OS ごとに作成し直す必要はありません。 そのため、異なるプラットフォーム上のアプリケーション間でコードを再利用し、一貫性を保つことができます。

## <a name="ecosystem"></a>エコシステム

特定のテクノロジ用のアプリケーションを構築して実行するために使われるすべての実行時ソフトウェア、開発ツール、およびコミュニティ リソース。

".NET エコシステム" という用語は ".NET スタック" などの用語と似ていますが、サードパーティのアプリとライブラリを含む点が異なります。 文章での使用例を次に示します。

- "[.NET Standard](#net-standard) の背後にある意図は、.NET エコシステムの高度な統一性を確立することです。"

## <a name="framework"></a>フレームワーク

一般に、特定のテクノロジに基づくアプリケーションの開発と展開を容易にする API の包括的なコレクション。 この一般的な意味でのアプリケーション フレームワークの例としては、ASP.NET Core や Windows フォームなどがあります。 「[ライブラリ](#library)」もご覧ください。

以下の語句で使われている "フレームワーク" という用語には、さらに具体的な技術的意味があります。

- [.NET Framework](#net-framework)
- [ターゲット フレームワーク](#target-framework)
- [TFM (ターゲット フレームワーク モニカー)](#tfm)

既存のドキュメントでは、[.NET の実装](#implementation-of-net)を指して "フレームワーク" が使われていることがあります。 たとえば、.NET Core をフレームワークと呼んでいる場合があります。 このような混乱を招く使用法をドキュメントから除去することが予定されています。

## <a name="gc"></a>[GC]

ガベージ コレクター (Garbage Collector)。

ガベージ コレクターは、自動メモリ管理の実装です。  GC は、使われなくなったオブジェクトによって占有されているメモリを解放します。

「[ガベージ コレクション](garbage-collection/index.md)」をご覧ください。

## <a name="il"></a>IL

中間言語 (Intermediate Language)。

C# などの高レベル .NET 言語は、中間言語 (IL) と呼ばれるハードウェアに依存しない命令セットにコンパイルされます。 IL は、MSIL (Microsoft IL) や CIL (Common IL) などと呼ばれることもあります。

## <a name="jit"></a>JIT

Just-In-Time コンパイラ。

[AOT](#aot) と同様に、このコンパイラは [IL](#il) をプロセッサが理解するマシン コードに変換します。 AOT とは異なり、JIT のコンパイルはオンデマンドで行われ、コードを実行する必要があるコンピューター上で実行されます。 JIT コンパイルはアプリケーションの実行中に行われるため、コンパイル時間は実行時間の一部になります。 したがって、JIT コンパイラでは、コードの最適化に要する時間と、結果のコードによって得られる時間の節約のバランスを考える必要があります。 ただし、JIT は実際のハードウェアを認識するので、開発者はさまざまな実装を出荷する必要がなくなります。

## <a name="implementation-of-net"></a>.NET の実装

.NET の実装には次のものが含まれます。

- 1 つまたは複数のランタイム。 たとえば、CLR、CoreCLR CoreRT などです。
- .NET Standard の 1 つのバージョンを実装し、他の API を含むことができるクラス ライブラリ。 たとえば、.NET Framework 基本クラス ライブラリや .NET Core 基本クラス ライブラリなどです。
- 必要に応じて、1 つまたは複数のアプリケーション フレームワーク。 たとえば、.NET Framework には ASP.NET、Windows フォーム、WPF が含まれます。
- 必要に応じて、開発ツール。 一部の開発ツールは、複数の実装間で共有されます。

.NET の実装の例:

- [.NET Framework](#net-framework)
- [.NET Core](#net-core)
- [ユニバーサル Windows プラットフォーム (UWP)](#uwp)

## <a name="library"></a>library

アプリまたは他のライブラリで呼び出すことができる API のコレクション。 .NET ライブラリは 1 つ以上の[アセンブリ](#assembly)で構成されます。

ライブラリと[フレームワーク](#framework)は同義語として使われることがよくあります。

## <a name="metapackage"></a>メタパッケージ

それ自体のライブラリを持たず、依存するもののリストのみを含む NuGet パッケージ。 含まれるパッケージは、必要に応じて、ターゲット フレームワーク用の API を確立できます。

「[パッケージ、メタパッケージ、フレームワーク](../core/packages.md)」をご覧ください。

## <a name="mono"></a>Mono

Mono はオープン ソースであり、主に小規模なランタイムが必要な場合に使用される[クロスプラットフォーム](#cross-platform)の .NET 実装です。 Android、Mac、iOS、tvOS、および watchOS 上の Xamarin アプリケーションで利用されるランタイムで、フットプリントが小さいアプリに重点を置いています。

現在公開されているすべての .NET Standard バージョンをサポートしています。

これまで Mono は .NET Framework の多数の API を実装し、Unix で人気の高い機能の一部をエミュレートしていました。 また、Unix のそのような機能に依存する .NET アプリケーションを実行するために使用されることもあります。

一般的に Mono は、Just-In-Time コンパイラと共に使用されますが、iOS のようなプラットフォームに使用される完全な静的コンパイラ (Ahead Of Time コンパイル) としても機能します。

Mono について詳しくは、[Mono のドキュメント](https://www.mono-project.com/docs/)をご覧ください。

## <a name="net"></a>.NET

[.NET Standard](#net-standard) とすべての [.NET の実装](#implementation-of-net)およびワークロードを表す包括的な用語。 常にすべて大文字で表し、".Net" とは表記されません。

「[.NET ガイド](index.md)」をご覧ください。

## <a name="net-core"></a>.NET Core

.NET のクロスプラットフォームで高パフォーマンスなオープン ソースの実装。 Core 共通言語ランタイム (CoreCLR)、Core AOT ランタイム (CoreRT、開発中)、Core 基本クラス ライブラリ、Core SDK が含まれます。

「[.NET Core](../core/index.md)」をご覧ください。

## <a name="net-core-cli"></a>.NET Core CLI

.NET Core アプリケーション開発用のクロスプラットフォーム ツールチェーン。

[.NET Core CLI](../core/tools/index.md) に関する記事をご覧ください。

## <a name="net-core-sdk"></a>.NET Core SDK

一連のライブラリとツールであり、開発者はこれを利用して .NET Core のアプリケーションやライブラリを作成できます。 アプリ構築用の [.NET Core CLI](#net-core-cli)、アプリの構築および実行用の .NET Core ライブラリとランタイム、CLI コマンドとアプリケーションを実行する dotnet 実行可能ファイル (*dotnet.exe*) が含まれます。

「[.NET Core SDK 概要](../core/sdk.md)」をご覧ください。

## <a name="net-framework"></a>.NET Framework

Windows でのみ動作する .NET の実装。 共通言語ランタイム (CLR)、基本クラス ライブラリ、および ASP.NET、Windows フォーム、WPF などのアプリケーション フレームワーク ライブラリが含まれます。

「[.NET Framework ガイド](../framework/index.md)」をご覧ください。

## <a name="net-native"></a>.NET Native

Just-In-Time (JIT) ではなく Ahead Of Time (AOT) でネイティブ コードを生成するコンパイラ ツール チェーン。

コンパイルは、C++ のコンパイラやリンカーと同様に、開発者のコンピューターで行われます。 未使用のコードが削除され、より多くの時間がコードの最適化に費やされます。 ライブラリからコードを抽出し、実行可能ファイルにそれらをマージします。 結果は、アプリ全体を表す 1 つのモジュールです。

UWP は、.NET Native によってサポートされる最初のアプリケーション フレームワークでした。 現在では、Windows、macOS、Linux 用のネイティブ コンソール アプリの構築がサポートされています。

「[Intro to .NET Native and CoreRT](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md)」(.NET Native と CoreRT の概要) をご覧ください。

## <a name="net-standard"></a>.NET Standard

.NET の各実装で使用可能な .NET API の正式な仕様。

.NET Standard の仕様は、ドキュメントではライブラリとも呼ばれます。 ライブラリには仕様 (インターフェイス) だけでなく API の実装も含まれるため、.NET Standard を "ライブラリ" と呼ぶのは誤解を招きます。 .NET Standard メタパッケージ (`NETStandard.Library`) の名前を参照する場合を除き、そのような使用法をドキュメントから消去する予定です。

「[.NET Standard](net-standard.md)」をご覧ください。

## <a name="ngen"></a>NGEN

ネイティブ (イメージ) 生成。

このテクノロジは、永続的な JIT コンパイラと考えることができます。 通常はコードが実行されるコンピューター上でコードをコンパイルしますが、一般にコンパイルはインストール時に行われます。

## <a name="package"></a>パッケージ

NuGet パッケージ &mdash; または単にパッケージ &mdash; は、同じ名前の 1 つまたは複数のアセンブリと、作成者名などの追加メタデータを含む、 *.zip* ファイルです。

*.zip* ファイルは、拡張子が *.nupkg* であり、複数のターゲット フレームワークとバージョンで使う *.dll* ファイルや *.xml* ファイルなどのアセットを含むことができます。 アプリまたはライブラリでインストールされるときに、アプリまたはライブラリで指定されているターゲット フレームワークに基づいて適切なアセットが選択されます。 インターフェイスを定義するアセットは *ref* フォルダーにあり、実装を定義するアセットは *lib* フォルダーにあります。

## <a name="platform"></a>platform

オペレーティング システムとそれが動作するハードウェア (Windows、macOS、Linux、iOS、Android)。

文章での使用例を次に示します。

- ".NET Core は、.NET のクロスプラットフォームの実装です。"
- "PCL プロファイルは Microsoft のプラットフォームを表し、.NET Standard はプラットフォームに依存しません。"

.NET のドキュメントでは、.NET の実装またはすべての実装を含む .NET スタックの意味で ".NET プラットフォーム" が使われることがよくあります。 これらの使用法はどちらも本来の (OS/ハードウェア) の意味と紛らわしい場合があるので、ドキュメントから削除される予定です。

## <a name="runtime"></a>ランタイム

マネージド プログラムの実行環境。

OS は、ランタイム環境の一部ですが、.NET ランタイムの一部ではありません。 .NET ランタイムの例を次に示します。

- 共通言語ランタイム (CLR)
- Core 共通言語ランタイム (CoreCLR)
- .NET Native (UWP の場合)
- Mono ランタイム

.NET のドキュメントでは、.NET の実装の意味で "ランタイム" が使われることがあります。 たとえば、次の文章の "ランタイム" は "実装" に置き換える必要があります。

- "さまざまな .NET ランタイムで、.NET Standard の特定のバージョンが実装されます。"
- "複数のランタイムでの実行を意図したライブラリは、このフレームワークを対象とする必要があります。" (.NET Standard を指している場合)
- "さまざまな .NET ランタイムで、.NET Standard の特定のバージョンが実装されます。 ... .NET ランタイムの各バージョンは、サポートしている .NET Standard の最高のバージョンをアドバタイズします …"

このような一貫性のない使用法は除去される予定です。

## <a name="stack"></a>スタック

全体としてアプリケーションの構築と実行に使われるプログラミング テクノロジのセット。

".NET スタック" は、.NET Standard および .NET のすべての実装を指します。 ".NET スタック" という語句が .NET の 1 つの実装を示すこともあります。

## <a name="target-framework"></a>対象フレーム

.NET アプリまたはライブラリが依存する API のコレクション。

アプリまたはライブラリは、.NET Standard の 1 つのバージョン (.NET Standard 2.0 など) をターゲットにできます。 .NET Standard は、.NET のすべての実装で標準化された API のセットの仕様です。 また、アプリまたはライブラリは、.NET の特定の実装のバージョンをターゲットにすることもでき、その場合は実装固有の API にアクセスできます。 たとえば、Xamarin.iOS をターゲットにするアプリは、Xamarin が提供する iOS API ラッパーにアクセスできます。

一部のターゲット フレームワーク (.NET Framework など) では、使用可能な API は .NET の実装がシステムにインストールするアセンブリによって定義され、アプリケーション フレームワーク API (たとえば ASP.NET、WinForms) を含む場合があります。 パッケージ ベースのターゲット フレームワーク (.NET Standard、.NET Core など) では、フレームワーク API はアプリまたはライブラリでインストールされるパッケージによって定義されます。 その場合、ターゲット フレームワークでは、全体としてフレームワークを構成するすべてのパッケージを参照するメタパッケージが暗黙的に指定されます。

「[ターゲット フレームワーク](frameworks.md)」をご覧ください。

## <a name="tfm"></a>TFM

ターゲット フレームワーク モニカー (Target Framework Moniker)。

.NET アプリまたはライブラリのターゲット フレームワークを指定するための標準化されたトークン形式。 通常、ターゲット フレームワークは短い名前によって参照されます (`net462` など)。 長い形式の TFM (.NETFramework,Version=4.6.2 など) も存在しますが、一般に、ターゲット フレームワークの指定には使われません。

「[ターゲット フレームワーク](frameworks.md)」をご覧ください。

## <a name="uwp"></a>UWP

ユニバーサル Windows プラットフォーム (Universal Windows Platform)。

モノのインターネット (IoT) のために最新のタッチ対応の Windows アプリケーションとソフトウェアを構築するために使われる .NET の実装。 PC、タブレット、ファブレット、携帯電話、Xbox など、ターゲットにする可能性があるさまざまな種類のデバイスを統一するように設計されています。 UWP は、一元的なアプリ ストア、実行環境 (AppContainer)、Win32 の代わりに使う Windows API のセット (WinRT) など、多くのサービスを提供します。 アプリは、C++、C#、Visual Basic、および JavaScript で記述することができます。 C# と Visual Basic を使うときは、.NET Core によって .NET API が提供されます。

## <a name="see-also"></a>参照

- [.NET のガイド](index.md)
- [.NET Framework ガイド](../framework/index.md)
- [.NET Core](../core/index.md)
- [ASP.NET の概要](/aspnet/index#pivot=aspnet)
- [ASP.NET Core の概要](/aspnet/index#pivot=core)
