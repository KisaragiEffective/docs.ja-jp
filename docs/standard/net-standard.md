---
title: .NET Standard
description: .NET Standard、そのバージョン、.NET Standard をサポートする .NET 実装について説明します。
ms.date: 02/13/2020
ms.technology: dotnet-standard
ms.custom: updateeachrelease
ms.assetid: c044882c-af15-45f2-96d1-534557a5ee9b
ms.openlocfilehash: 00b40b771a8608bad7e3f992e3c99367ff6bb131
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77452592"
---
# <a name="net-standard"></a>.NET Standard

[.NET Standard](https://github.com/dotnet/standard) は、すべての .NET 実装で使用できるようにすることを目的とした .NET API の正式な仕様です。 .NET Standard の背後にある意図は、.NET エコシステムの高度な統一性を確立することです。 [ECMA 335](https://github.com/dotnet/runtime/blob/master/docs/project/dotnet-standards.md) では引き続き .NET 実装の動作に対する統一性が確立されます。ECMA 335 では標準ライブラリの小さなセットが指定されていますが、.NET Standard 仕様には幅広い .NET API が含まれています。

.NET Standard により、次の主なシナリオが可能になります。

- ワークロードに関係なく、すべての .NET 実装用に統一された BCL API のセットを定義し、実装する。
- 開発者が、この同じ API のセットを使用して、.NET 実装間で使用可能なポータブル ライブラリを生成できるようにする。
- .NET API による共有ソースの条件付きコンパイルを削減あるいはなくして、OS API に対してのみにする。

さまざまな .NET 実装が、.NET Standard の特定のバージョンを対象とします。 各 .NET 実装バージョンは、それがサポートしている最高の .NET Standard バージョンをアドバタイズし、そのことは以前のバージョンもサポートしていることを意味します。 たとえば、.NET Framework 4.6 は .NET Standard 1.3 を実装しており、このことは、.NET Standard のバージョン 1.0 ～ 1.3 で定義されているすべての API を公開していることを意味します。 同様に、.NET Framework 4.6.1 は .NET Standard 1.4 を実装し、.NET Core 1.0 は .NET Standard 1.6 を実装しています。

## <a name="net-implementation-support"></a>.NET 実装のサポート

次の表は、.NET Standard の各バージョンをサポートするプラットフォームの**最小**バージョンを一覧表示します。 つまり、一覧に示されたプラットフォームの以降のバージョンでは、対応する .NET Standard バージョンもサポートされます。 たとえば、.NET Core 2.2 では、.NET Standard 2.0 以前がサポートされます。

[!INCLUDE [net-standard-table](../../includes/net-standard-table.md)]

対象にすることができる最高バージョンの .NET Standard を確認するには、次の手順を実行します。

1. 実行する .NET 実装を示す行を探します。
2. この行内で、右から左の方向に、バージョンを示す列を探します。
3. 列ヘッダーには、ターゲットでサポートされている .NET Standard バージョンが示されます。 任意の古い .NET Standard バージョンをターゲットにすることもできます。 .NET Standard の今後のバージョンでも実装はサポートされます。
4. 対象にするプラットフォームごとに、この手順を繰り返します。 複数のターゲット プラットフォームがある場合は、その中から低いバージョンを選択することをお勧めします。 たとえば、.NET Framework 4.5 と .NET Core 1.0 で実行する場合、使用できる .NET Standard の最高のバージョンは .NET Standard 1.1 です。

### <a name="which-net-standard-version-to-target"></a>対象にする .NET Standard バージョン

.NET Standard バージョンを選択する場合、次のトレードオフを考慮する必要があります。

- バージョンが高くなるほど、使用できる API が多くなる。
- バージョンが低くなるほど、実装しているプラットフォームが多くなる。

一般的に、可能な限り*最小*バージョンの .NET Standard を対象にすることが推奨されます。 そのため、対象にする最高バージョンの .NET Standard を見つけたら、次の手順を実行します。

1. 1 つ低いバージョンの .NET Standard を対象にしてプロジェクトをビルドします。
2. プロジェクトのビルドが成功したら、手順 1 を繰り返します。 失敗した場合は、対象を次に高いバージョンに変更します。これが対象のバージョンです。

ただし、下位バージョンの .NET Standard を対象にしている場合、さまざまな依存関係のサポートが導入されます。 お使いのプロジェクトが .NET Standard 1.x を対象としている場合、.NET Standard 2.0 "*も*" 対象にすることをお勧めします。 これにより、.NET Standard 2.0 と互換性のあるフレームワーク上で実行されるライブラリのユーザーでは、依存関係グラフが簡素化され、ダウンロードが必要になるパッケージ数が減少します。

### <a name="net-standard-versioning-rules"></a>.NET Standard のバージョン管理規則

バージョン管理規則は主に 2 つあります。

- 追加式。 .NET Standard バージョンは論理的に同心円形です。高位のバージョンには、旧バージョンのすべての API が組み込まれています。 バージョン間に互換性に影響する変更はありません。
- 不変: 出荷後、.NET Standard のバージョンは固定されます。 新しい API は、特定の .NET 実装 (.NET Core など) でまず使用できるようになります。 .NET Standard の審査会が、新しい API をすべての .NET 実装で使用できるようにした方がよいと判断すると、新しい .NET Standard バージョンに追加されます。

## <a name="specification"></a>仕様

.NET Standard の仕様は、標準化された API のセットです。 この仕様は、.NET 実装、具体的には Microsoft (.NET Framework、.NET Core、Mono を含む) と Unity によって管理されています。 [GitHub](https://github.com/dotnet/standard) 経由の新しい .NET Standard のバージョンの構築の一環として、公開フィードバック プロセスが使われています。

### <a name="official-artifacts"></a>正式な成果物

正式な仕様は標準に含まれる API を定義する一連の .cs ファイルです。 [dotnet/standard リポジトリ](https://github.com/dotnet/standard/tree/master/src/netstandard/ref)の [ref](https://github.com/dotnet/standard) ディレクトリに.NET Standard API が定義されています。

[NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library) メタパッケージ ([ソース](https://github.com/dotnet/standard/blob/master/src/netstandard/pkg/NETStandard.Library.dependencies.props)) は、1 つ以上の .NET Standard のバージョンを定義する (一部) ライブラリのセットについて説明しています。

`System.Runtime` などの特定のコンポーネントは、次のように説明されています。

- .NET Standard の一部 (そのスコープだけ)。
- そのスコープの .NET Standard の複数のバージョン。

便利に読めるようにし、特定の開発者シナリオ (コンパイラを使用するなど) を可能にするために、派生成果物が提供されています。

- [マークダウン形式の API 一覧](https://github.com/dotnet/standard/tree/master/docs/versions)
- [NuGet パッケージ](../core/packages.md)として配布され、[NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) メタパッケージによって参照される参照アセンブリ。

### <a name="package-representation"></a>パッケージ表現

.NET Standard 参照アセンブリの主要な配布手段は [NuGet パッケージ](../core/packages.md)です。 実装は、各 .NET 実装に適切なさまざまな方法で配布されます。

NuGet パッケージは 1 つまたは複数の[フレームワーク](frameworks.md)を対象にしています。 .NET Standard パッケージは、".NET Standard" フレームワークを対象にしています。 `netstandard` [コンパクト TFM](frameworks.md) (`netstandard1.4` など) を使用して、.NET Standard フレームワークを対象にすることができます。 複数のランタイムでの実行を意図したライブラリは、このフレームワークを対象とする必要があります。 広範なセットの API の場合、.NET Standard 1.6 から 2.0 の間に使用できる API の数は 2 倍以上になったため、`netstandard2.0` を対象としてください。

[`NETStandard.Library`](https://www.nuget.org/packages/NETStandard.Library/) メタパッケージは .NET Standard を定義する NuGet パッケージの完全なセットを参照しています。  `netstandard` を対象とする最も一般的な方法は、このメタパッケージを参照することです。 それは、.NET Standard を定義する約 40 の .NET ライブラリと関連する API について説明しており、それらにアクセスできるようにしています。 `netstandard` を対象とする追加のパッケージを参照することで、追加の API へのアクセス権を取得できます。

### <a name="versioning"></a>バージョン管理

仕様は 1 つだけではありませんが、段階的に拡張され、直線的にバージョン管理された API のセットです。 標準の最初のバージョンは、API のベースライン セットを構築しています。 それ以降のバージョンでは、API が追加され、以前のバージョンによって定義された API を継承しています。 標準から API を削除するために確立された取り決めはありません。

.NET Standard は、1 つの .NET 実装に固有ではなく、それらの実装のいずれかのバージョン管理スキームに一致することもありません。

いずれかの実装 (.NET Framework、.NET Core、Mono など) に追加された API は、本質的に基本的であると考えられる場合に、仕様に追加される候補とみなされる可能性があります。 新しい[.NET Standard のバージョン](https://github.com/dotnet/standard/blob/master/docs/versions.md) は、.NET 実装のリリースに基づいて作成されるので、.NET Standard PCL から新しい API を対象にすることができます。 バージョン管理メカニズムの詳細は、「[.NET Core Versioning](../core/versions/index.md)」(.NET Core のバージョン管理) で説明されています。

.NET Standard のバージョン管理は、使用する場合に重要です。 .NET Standard のバージョンが指定されている場合、それと同じか以下のバージョンを対象とするライブラリを使用できます。 次の方法では、.NET Standard の対象設定に固有の、.NET Standard PCL の使用のワークフローを説明します。

- PCL に使用する .NET Standard のバージョンを選択します。
- .NET Standard と同じかそれ以下のバージョンに依存するライブラリを使用します。
- それより上の .NET Standard バージョンに依存するライブラリを見つけた場合は、それと同じバージョンを採用するか、そのライブラリを使用しないようにする必要があります。

## <a name="targeting-net-standard"></a>.NET Standard を対象とする

[ フレームワークと NETStandard.Library メタパッケージの組み合わせを使用して、](../core/tutorials/libraries.md).NET Standard Library をビルド`netstandard`できます。 [.NET Core ツールを使用して .NET Standard を対象とする](../core/packages.md)例を参照できます。

## <a name="net-framework-compatibility-mode"></a>.NET Framework 互換モード

.NET Standard 2.0 以降、.NET Framework 互換モードが導入されました。 この互換モードにより、.NET Standard プロジェクトは、あたかも .NET Standard にコンパイルされたかのように .NET Framework ライブラリを参照できます。 .NET Framework ライブラリの参照は、Windows Presentation Foundation (WPF) API を使用するライブラリのような、一部のプロジェクトでは機能しません。

詳細については、「[.NET Framework 互換モード](../core/porting/third-party-deps.md#net-framework-compatibility-mode)」を参照してください。

## <a name="net-standard-libraries-and-visual-studio"></a>.NET Standard ライブラリと Visual Studio

Visual Studio で .NET Standard ライブラリを作成するには、Windows の場合 [Visual Studio 2017 バージョン 15.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) 以降、macOS の場合 [Visual Studio for Mac バージョン 7.1](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 以降がインストールされている必要があります。

プロジェクトで .NET Standard 2.0 ライブラリを使用するだけである場合は、Visual Studio 2015 でこれを行うこともできます。 ただし、NuGet クライアント 3.6 以降がインストールされている必要があります。 Visual Studio 2015 用の NuGet クライアントは、[NuGet のダウンロード](https://www.nuget.org/downloads)のページからダウンロードできます。

## <a name="comparison-to-portable-class-libraries"></a>ポータブル クラス ライブラリとの比較

.NET Standard は、[ポータブル クラス ライブラリ (PCL)](./cross-platform/cross-platform-development-with-the-portable-class-library.md) に代わるものです。 .NET Standard は、標準 BCL を精選し、結果として、.NET 実装間で高度な統一性を確立することによって、ポータブル ライブラリの作成の方法を向上しています。 .NET Standard を対象とするライブラリは、PCL つまり ".NET Standard ベースの PCL" です。 既存の PCL は、"プロファイルベースの PCL" です。

.NET Standard と PCL プロファイルは、似たような目的で作成されましたが、主な点が異なります。

類似点:

- バイナリ コード共有に使用できる API を定義します。

相違点:

- .NET Standard は精選された API のセットで、PCL プロファイルは、既存のプラットフォームの交差部分によって定義されています。
- .NET Standard は線形的にバージョン管理されていますが、PCL プロファイルはそうではありません。
- PCL プロファイルは Microsoft のプラットフォームを表し、.NET Standard はプラットフォームに依存しません。

### <a name="pcl-compatibility"></a>PCL 互換性

.NET Standard は、PCL プロファイルのサブセットと互換性があります。 .NET Standard 1.0、1.1、1.2 はそれぞれ PCL プロファイルのセットと重複します。 この重複は 2 つの理由で作成されました。

- .NET Standard ベースの PCL がプロファイルベースの PCL を参照できるようにする。
- プロファイルベースの PCL を .NET Standard ベースの PCL としてパッケージできるようにする。

プロファイルベースの PCL の互換性は、[Microsoft.NETCore.Portable.Compatibility](https://www.nuget.org/packages/Microsoft.NETCore.Portable.Compatibility) NuGet パッケージで提供されています。 プロファイルベースの PCL を含む NuGet パッケージを参照するときに、この依存関係が必要です。

`netstandard` としてパッケージされたプロファイルベースの PCL は、通常パッケージされるプロファイルベースの PCL より使いやすくなります。 `netstandard` パッケージは、既存のユーザーと互換性があります。

標準 .NET と互換性がある PCL プロファイルのセットを確認できます。

| PCL プロファイル | .NET Standard | PCL プラットフォーム
|:-----------:|:-------------:|------------------------------------------------------------------------------
| Profile7    | 1.1           | .NET Framework 4.5、Windows 8
| Profile31   | 1.0           | Windows 8.1、Windows Phone Silverlight 8.1
| Profile32   | 1.2           | Windows 8.1、Windows Phone 8.1
| Profile44   | 1.2           | .NET Framework 4.5.1、Windows 8.1
| Profile49   | 1.0           | .NET Framework 4.5、Windows Phone Silverlight 8
| Profile78   | 1.0           | .NET Framework 4.5、Windows 8、Windows Phone Silverlight 8
| Profile84   | 1.0           | Windows Phone 8.1、Windows Phone Silverlight 8.1
| Profile111  | 1.1           | .NET Framework 4.5、Windows 8、Windows Phone 8.1
| Profile151  | 1.2           | .NET Framework 4.5.1、Windows 8.1、Windows Phone 8.1
| Profile157  | 1.0           | Windows 8.1、Windows Phone 8.1、Windows Phone Silverlight 8.1
| Profile259  | 1.0           | .NET Framework 4.5、Windows 8、Windows Phone 8.1、Windows Phone Silverlight 8

## <a name="see-also"></a>参照

- [.NET Standard のバージョン](https://github.com/dotnet/standard/blob/master/docs/versions.md)
- [.NET Standard ライブラリを構築する](../core/tutorials/library-with-visual-studio.md)
- [クロス プラットフォーム ターゲット](./library-guidance/cross-platform-targeting.md)
