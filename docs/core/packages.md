---
title: パッケージ、メタパッケージ、フレームワーク - .NET Core
description: パッケージ、メタパッケージ、フレームワークの用語を説明します。
author: richlander
ms.date: 06/20/2016
ms.openlocfilehash: 657519edf1c0860ee3222c71ce85723e19029a9d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397933"
---
# <a name="packages-metapackages-and-frameworks"></a>パッケージ、メタパッケージ、フレームワーク

.NET Core は、NuGet パッケージで作成されたプラットフォームです。 製品のエクスペリエンスには、粒度の細かいパッケージの定義から恩恵を受けるものもあれば、粒度の粗いパッケージの定義から恩恵を受けるものもあります。 この二重性に対応するために、.NET Core は、粒度の細かいパッケージ セットとして配布され、かつ、俗に[メタパッケージ](#metapackages)と呼ばれるパッケージの種類を使用して粒度の粗いチャンク単位で配布されます。

各 .NET Core パッケージは、フレームワークとして表現される、複数の .NET 実装での実行に対応しています。 このようなフレームワークには、`net46` (.NET Framework に相当する) のような従来のフレームワークもあれば、 フレームワークを定義するために新しいモデルを確立し、"パッケージ ベースのフレームワーク" と見なすことができる新しいフレームワークもあります。 パッケージ ベースのフレームワークは、完全にパッケージとして作成および定義されるので、パッケージとフレームワークの間に強力なリレーションシップが形成されます。

## <a name="packages"></a>パッケージ

.NET core は、プリミティブ、上位レベルのデータ型、アプリ コンポジション型、および一般的なユーティリティを提供する一連のパッケージに分割されます。 これらのパッケージはいずれも、同じ名前の単一アセンブリです。 たとえば、[System.Runtime パッケージ](https://www.nuget.org/packages/System.Runtime)には、System.Runtime.dll が含まれています。

粒度の細かい方法でパッケージを定義することには、次の利点があります。

- 粒度の細かいパッケージの場合は、ほかのパッケージのテストが比較的に抑えられるので、独自のスケジュールに合わせて配布することができます。
- 粒度の細かいパッケージでは、さまざまな OS および CPU をサポートできます。
- 粒度の細かいパッケージは、1 つライブラリのみに固有の依存関係を持つことができます。
- アプリ配布の対象に未参照のパッケージが含まれないので、アプリのサイズが小さくなります。

これらの利点の一部は、特定の状況でのみ使用されます。 たとえば、NET Core パッケージは、通常、同じプラットフォーム サポートを使用して、同じスケジュールで配布されます。 サポートを行う場合は、修正プログラムをサイズの小さな単一のパッケージ更新プログラムとして配布およびインストールすることができます。 変更の範囲が狭いため、検証と修正プログラムを使用できるようにするまでの時間は、単一のライブラリで必要とされる内容に限定されます。

.NET Core 用の主な NuGet パッケージを次に一覧します。

- [System.Runtime](https://www.nuget.org/packages/System.Runtime) - 最も基本的な .NET Core パッケージです。<xref:System.Object>、<xref:System.String>、<xref:System.Array>、<xref:System.Action>、<xref:System.Collections.Generic.IList%601> などがあります。
- [System.Collections](https://www.nuget.org/packages/System.Collections) - (主な) ジェネリック コレクションのセットです。<xref:System.Collections.Generic.List%601> や <xref:System.Collections.Generic.Dictionary%602> などがあります。
- [System.Net.Http](https://www.nuget.org/packages/System.Net.Http) - HTTP ネットワーク通信の種類のセットです。<xref:System.Net.Http.HttpClient> や <xref:System.Net.Http.HttpResponseMessage> などがあります。
- [System.IO.FileSystem](https://www.nuget.org/packages/System.IO.FileSystem) - ローカルまたはネットワークに接続されたディスク ベースの記憶域に対する読み取りおよび書き込みの種類のセットです。<xref:System.IO.File> や <xref:System.IO.Directory> などがあります。
- [System.Linq](https://www.nuget.org/packages/System.Linq) - オブジェクトに対するクエリの種類のセットです。`Enumerable` や <xref:System.Linq.ILookup%602> などがあります。
- [System.Reflection](https://www.nuget.org/packages/System.Reflection) - 読み込み、検査、およびアクティブ化の種類のセットです。<xref:System.Reflection.Assembly>、<xref:System.Reflection.TypeInfo>、<xref:System.Reflection.MethodInfo> などがあります。

通常、各パッケージを含めるより[メタパッケージ](#metapackages)を含める方がより簡単であり、より堅牢です。 ただし、単一のパッケージが必要な場合、下の例のように追加できます。[System.Runtime](https://www.nuget.org/packages/System.Runtime/) パッケージを参照しています。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard1.6</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="System.Runtime" Version="4.3.0" />
  </ItemGroup>
</Project>
```

## <a name="metapackages"></a>メタパッケージ

メタパッケージは、統合して意味をなすパッケージ セットを記述するための NuGet パッケージの規則です。 メタパッケージでは、このパッケージのセットを表すために、それらを依存関係にします。 メタパッケージを使うと、必要に応じて、フレームワークを指定して、パッケージのセットのフレームワークを確立することができます。

.NET Core ツールの以前のバージョンは (project.json ツールと csproj-based ツールの両方)、既定では、フレームワークとメタパッケージの両方を指定していました。 ただし、現時点では、各メタパッケージがターゲット フレームワークに関連付けられるように、ターゲット フレームワークによってメタパッケージが暗黙的に参照されます。 たとえば、`netstandard1.6` フレームワークは NetStandard.Library バージョン 1.6.0 メタパッケージを参照します。 同様に、`netcoreapp2.1` フレームワークは Microsoft.NETCore.App バージョン 2.1.0 メタパッケージを参照します。 詳細については、「[Implicit metapackage package reference in the .NET Core SDK](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)」 (.NET Core SDK のメタパッケージの暗黙的パッケージ参照) を参照してください。

フレームワークをターゲットにし、メタパッケージを暗黙的に参照することは、各依存パッケージの参照を 1 つのジェスチャとして追加することを実質的に意味します。 これらのパッケージのライブラリはすべて、IntelliSense (または同様のエクスペリエンス) とアプリの公開で利用できます。

メタパッケージを使用する利点は次のとおりです。

- 一連の粒度の細かいパッケージを参照するのに便利なユーザー エクスペリエンスを提供できます。
- テストされ一体となって優れた機能を発揮するパッケージ セット (特定のバージョンを含む) を定義できます。

.NET Standard のメタパッケージ:

- [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library) - .NET Standard に含まれるライブラリを記述したものです。 .NET Standard をサポートするすべての .NET 実装 (.NET Framework、.NET Core、Mono など) に適用されます。 `netstandard` フレームワークを確立します。

主な .NET Core メタパッケージ:

- [Microsoft.NETCore.App](https://www.nuget.org/packages/Microsoft.NETCore.App) - .NET Core 配布に含まれるライブラリについて記述します。 `.NETCoreApp` フレームワークを確立します。 よりサイズの小さな `NETStandard.Library` に依存します。
- [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) - ASP.NET Core と Entity Framework Core からのパッケージで、サポートされているすべてのパッケージが含まれます。ただし、サードパーティの依存関係が含まれるものを除きます。 詳細については、「[ASP.NET Core 2.1 以降に対応した Microsoft.AspNetCore.App メタパッケージ](/aspnet/core/fundamentals/metapackage-app)」を参照してください。
- [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) - ASP.NET Core、Entity Framework Core、および ASP.NET Core と Entity Framework Core によって使用される内部依存関係およびサード パーティの依存関係からの、サポートされるすべてのパッケージが含まれます。 詳しくは、「[ASP.NET Core 2.x 用 Microsoft.AspNetCore.All メタパッケージ](/aspnet/core/fundamentals/metapackage)」をご覧ください。
- [Microsoft.NETCore.Portable.Compatibility](https://www.nuget.org/packages/Microsoft.NETCore.Portable.Compatibility) - mscorlib ベースのポータブル クラス ライブラリ (PCL) を .NET Core で実行できるようにするための互換性ファサードのセットです。

## <a name="frameworks"></a>フレームワーク

.NET Core の各パッケージでは、一連のランタイム フレームワークがサポートされます。 フレームワークでは、特定のフレームワークを対象とするときに依存することができる使用可能な API セット (および、場合によってはその他の特性) について記述します。 フレームワークのバージョンは、新しい API が追加されると更新されます。

たとえば、 [System.IO.FileSystem](https://www.nuget.org/packages/System.IO.FileSystem) では、次のフレームワークをサポートします。

- .NETFramework,Version=4.6
- .NETStandard,Version=1.3
- 6 Xamarin プラットフォーム (xamarinios10 など)

上記フレームワークの最初の 2 つの例は、フレームワークの 2 種類の定義方法を示したものであるので、この 2 つを比べることには意義があります。

- `.NETFramework,Version=4.6` フレームワークは、.NET Framework 4.6 で使用できる API を表します。 .NET Framework 4.6 参照アセンブリでコンパイルされたライブラリを生成し、NuGet パッケージの net46 lib フォルダーに置かれているそれらのライブラリを配布することができます。 このフレームワークは、.NET Framework 4.6 を対象とするアプリ、または .NET Framework 4.6 と互換性のあるアプリで使用されます。 これは、すべてのフレームワークの従来のしくみです。

- `.NETStandard,Version=1.3` フレームワークは、パッケージ ベースのフレームワークです。 これは、フレームワークに関して API を定義して公開するためにフレームワークを対象とするパッケージに依存します。

## <a name="package-based-frameworks"></a>パッケージ ベースのフレームワーク

フレームワークとパッケージの間には双方向のリレーションシップがあります。 リレーションシップの 1 番目の部分は、特定のフレームワーク (`netstandard1.3` など) で使用可能な API を定義することです。 `netstandard1.3` (または`netstandard1.0` のような互換性のあるフレームワーク) を対象とするパッケージは、`netstandard1.3` で使用可能な API を定義します。 循環定義のように思われるかもしれませんが、そうではありません。 "パッケージ ベース" であるため、フレームワークの API 定義は、パッケージから取得されます。 フレームワーク自体で、API の定義は行われません。

リレーションシップの 2 番目の部分は、資産の選択です。 パッケージには、複数のフレームワークの資産を含めることができます。 パッケージ セットおよび/またはメタパッケージ セットへの参照を指定する場合は、選択すべき資産 (たとえば、`net46` または `netstandard1.3`) を決定するためにフレームワークが必要です。 正しい資産を選択することが重要です。 たとえば、`net46` 資産が .NET Framework 4.0 または .NET Core 1.0 と互換性を持たない可能性があります。

このリレーションシップは次の図のようになります。 *API* は、*フレームワーク*を対象とし、定義します。 *フレームワーク*は、*資産の選択*で使用されます。 *資産* は、API を提供します。

![パッケージ ベースのフレームワークのコンポジション](./media/packages/package-framework.png)

.NET Core では、次の 2 つのパッケージ ベースのプライマリ フレームワークが使用されます。

- `netstandard`
- `netcoreapp`

### <a name="net-standard"></a>.NET Standard

.NET Standard ([ターゲット フレームワーク モニカー](../standard/frameworks.md): `netstandard`) フレームワークは、[.NET Standard](../standard/net-standard.md) によって定義され、.NET Standard の上に構築された API を表します。 複数のランタイムでの実行を意図したライブラリは、このフレームワークを対象とする必要があります。 それらのライブラリは、.NET Core、.NET Framework、Mono/Xamarin など、.NET Standard 準拠のランタイムでサポートされます。 このようなランタイムはそれぞれが、実装される API に応じて、一連の .NET Standard バージョンをサポートします。

`netstandard` フレームワークは [`NETStandard.Library`](https://www.nuget.org/packages/NETStandard.Library) メタパッケージを暗黙的に参照します。 たとえば、次の MSBuild プロジェクト ファイルは、プロジェクトが `netstandard1.6` をターゲットにすることを示します。これは [`NETStandard.Library` バージョン 1.6](https://www.nuget.org/packages/NETStandard.Library/1.6.0) メタパッケージを参照します。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard1.6</TargetFramework>
  </PropertyGroup>
</Project>
```

ただし、プロジェクト ファイルのフレームワーク参照とメタパッケージ参照が一致する必要はありません。プロジェクト ファイルの `<NetStandardImplicitPackageVersion>` 要素を利用し、メタパッケージ バージョンより下のフレームワーク バージョンを指定できます。 たとえば、次のプロジェクト ファイルが有効です。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard1.3</TargetFramework>
    <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
  </PropertyGroup>
</Project>
```

`netstandard1.3` を対象とするのに `NETStandard.Library` のバージョン 1.6.0 を使用するのは、奇妙に思えるかもしれません。 これは、メタパッケージでは古い `netstandard` バージョンのサポートが継続されていることから、使用例として有効です。 メタパッケージのバージョン 1.6.0 が標準とされている状況で、これをすべてのライブラリ (`netstandard` の各種バージョンを対象とする) で使用する場合が考えられます。 この方法を使用した場合、`NETStandard.Library` 1.6.0 を復元する必要があるだけで、それより前のバージョンの復元は必要ありません。

逆は無効になります (`netstandard1.6` を対象とするが、`NETStandard.Library` のバージョン 1.3.0 を使用する)。 上位フレームワークと下位メタパッケージの組み合わせを対象とすることはできません。下位バージョンのメタパッケージはその上位フレームワークに対して資産を公開しないことが理由です。 メタパッケージのバージョン管理スキームは、メタパッケージが、記述したフレームワークの最上位バージョンと一致することをアサートします。 バージョン管理スキームにより、`NETStandard.Library` 資産が含まれている場合、`netstandard1.6` の最初のバージョンは v1.6.0 となります。 (前の例と対称の場合、ここでは v 1.3.0 が使用されますが、実際には存在しません)。

### <a name="net-core-application"></a>.NET Core アプリケーション

.NET Core アプリケーション ([ターゲット フレームワーク モニカー](../standard/frameworks.md): `netcoreapp`) フレームワークは、.NET Core 配布に付属のパッケージおよび関連する API と、.NET Core 配布によって提供されるコンソール アプリケーション モデルを表します。 .NET Core アプリは、コンソール アプリケーション モデルを対象とすることから、.NET Core での実行のみを意図するライブラリとして、このフレームワークを使用する必要があります。 このフレームワークを使用すると、アプリとライブラリは .NET Core でのみの実行に限定されます。

`Microsoft.NETCore.App` メタパッケージは、`netcoreapp` フレームワークを対象とします。 これは、約 60 のライブラリにアクセスできるようにします (約 40 個は `NETStandard.Library` パッケージによって提供され、さらに約 20 個の追加分)。 `netcoreapp` または互換性を持つフレームワーク (`netstandard` など) を対象とする追加のライブラリを参照することで、追加の API にアクセスできます。

`Microsoft.NETCore.App` によって提供される追加ライブラリの大部分も、依存関係がほかの `netstandard` ライブラリによって満足されるならば、`netstandard` を対象とします。 つまり、`netstandard` ライブラリは、それらのパッケージを依存関係として参照することもできます。
