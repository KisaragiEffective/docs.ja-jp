---
title: .NET Core の csproj 形式に追加されたもの
description: 既存の csproj ファイルと .NET Core の csproj ファイルの違いについて説明します
ms.date: 04/08/2019
ms.openlocfilehash: 2fb00e830380c5c4cbf7b6dcd2c8a585e1617b4b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397945"
---
# <a name="additions-to-the-csproj-format-for-net-core"></a>.NET Core の csproj 形式に追加されたもの

ここでは、*project.json* から *csproj* および [MSBuild](https://github.com/Microsoft/MSBuild) への移行に伴ってプロジェクト ファイルに追加された変更について説明します。 一般的なプロジェクト ファイルの構文とリファレンスの詳細については、[MSBuild プロジェクト ファイル](/visualstudio/msbuild/msbuild-project-file-schema-reference)のドキュメントを参照してください。

## <a name="implicit-package-references"></a>暗黙的なパッケージ参照

メタパッケージは、プロジェクト ファイルの `<TargetFramework>` または `<TargetFrameworks>` プロパティに指定されている対象フレームワークに基づいて暗黙的に参照されています。 `<TargetFramework>` を指定すると、順序に関係なく `<TargetFrameworks>` は無視されます。 詳細については、「[パッケージ、メタパッケージ、フレームワーク](../packages.md)」を参照してください。

```xml
 <PropertyGroup>
   <TargetFramework>netcoreapp2.1</TargetFramework>
 </PropertyGroup>
 ```

 ```xml
 <PropertyGroup>
   <TargetFrameworks>netcoreapp2.1;net462</TargetFrameworks>
 </PropertyGroup>
 ```

### <a name="recommendations"></a>推奨事項

`Microsoft.NETCore.App` または `NETStandard.Library` メタパッケージは暗黙的に参照されるので、ベスト プラクティスとして以下が推奨されます。

- .NET Core または .NET Standard を対象とするとき、プロジェクト ファイルの `<PackageReference>` アイテム経由で `Microsoft.NETCore.App` または `NETStandard.Library` メタパッケージを明示的に参照しないようにします。
- .NET Core を対象にするとき、特定バージョンのランタイムが必要な場合、メタパッケージを参照するのではなく、プロジェクト内で `<RuntimeFrameworkVersion>` プロパティを使用します (`1.0.4` など)。
  - [自己完結型の展開](../deploying/index.md#publish-self-contained)を使用し、特定のパッチ バージョンの 1.0.0 LTS ランタイムが必要な場合などにこの問題が発生する可能性があります。
- .NET Standard を対象にするとき、特定バージョンの `NETStandard.Library` メタパッケージが必要な場合、`<NetStandardImplicitPackageVersion>` プロパティを使用し、必要なバージョンを設定できます。
- .NET Framework プロジェクトでは、`Microsoft.NETCore.App` または `NETStandard.Library` メタパッケージに参照を明示的に追加したり、更新したりしないでください。 .NET Standard ベースの NuGet パッケージを使用するとき、何らかのバージョンの `NETStandard.Library` が必要であれば、NuGet はそのバージョンを自動的にインストールします。

## <a name="implicit-version-for-some-package-references"></a>一部のパッケージ参照に対する暗黙的なバージョン

[`<PackageReference>`](#packagereference) を使用するほとんどの場合に、`Version` 属性を設定して、使用する NuGet パッケージのバージョンを指定する必要があります。 ただし、.NET Core 2.1 または 2.2 を使用し、[Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) または [Microsoft.AspNetCore.All](/aspnet/core/fundamentals/metapackage) を参照するときは、その属性は必要ありません。 .NET Core SDK では、使用する必要があるこれらのパッケージのバージョンを自動的に選択できます。

### <a name="recommendation"></a>推奨事項

`Microsoft.AspNetCore.App` または `Microsoft.AspNetCore.All` パッケージを参照するときは、それらのバージョンを指定しないでください。 バージョンを指定すると、SDK で警告 NETSDK1071 が発生する可能性があります。 この警告を解決するには、次の例のようなパッケージのバージョンを削除します。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

> 既知の問題: .NET Core 2.1 SDK では、プロジェクトでも Microsoft.NET.Sdk.Web が使用されている場合にのみ、この構文がサポートされます。 これは、.NET Core 2.2 SDK で解決されます。

ASP.NET Core メタパッケージに対するこれらの参照では、ほとんどの通常の NuGet パッケージとは動作が若干異なります。 これらのメタパッケージを使用するアプリケーションの[フレームワーク依存の展開](../deploying/index.md#publish-runtime-dependent)では、ASP.NET Core 共有フレームワークが自動的に利用されます。 メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に展開**されません**。ASP.NET Core 共有フレームワークにはこれらの資産が含まれています。 共有フレームワーク内の資産は、アプリケーションの起動時間を向上させるため、ターゲット プラットフォームに対して最適化されています。 共有フレームワークについて詳しくは、「[.NET Core の配布パッケージ](../distribution-packaging.md)」をご覧ください。

バージョンを "*指定する*" と、フレームワーク依存の展開では ASP.NET Core の共有フレームワークの "*最小*" バージョンとして扱われ、自己完結型の展開では "*厳密な*" バージョンとして扱われます。 これには、次のような影響があります。

- サーバーにインストールされている ASP.NET Core のバージョンが、PackageReference で指定されているバージョンより小さい場合、.NET Core プロセスの起動は失敗します。 Azure などのホスティング環境でメタパッケージの更新プログラムが使用できるようになる前に、NuGet.org で更新プログラムが利用可能になることがよくあります。 PackageReference でのバージョンを ASP.NET Core に更新すると、展開されているアプリケーションが失敗する可能性があります。
- アプリケーションが[自己完結型の展開](../deploying/index.md#publish-self-contained)として展開されている場合、アプリケーションに .NET Core の最新のセキュリティ更新プログラムが含まれていない可能性があります。 バージョンを指定しないと、SDK は自己完結型の展開に ASP.NET Core の最新バージョンを自動的に含めることができます。

## <a name="default-compilation-includes-in-net-core-projects"></a>.NET Core プロジェクトの既定のコンパイルの include

最新バージョンの SDK の *csproj* 形式に移行すると共に、コンパイル項目と、SDK プロパティ ファイルに埋め込みリソースの既定の include と exclude を SDK プロパティ ファイルに移行しました。 つまり、これらの項目をプロジェクト ファイルに指定する必要はなくなりました。

これを行う主な理由は、プロジェクト ファイルを見やすくするためです。 SDK の既定値は、最も一般的な使用例に対応しているので、作成するプロジェクトごとに繰り返す必要はありません。 その結果、プロジェクト ファイルが小さくなり、わかりやすく、編集が必要な場合に編集しやすくなります。

次の表は、SDK に含まれる、および除外される要素と [glob](https://en.wikipedia.org/wiki/Glob_(programming)) の一覧です。

| 要素           | 含まれる glob                              | 除外される glob                                                  | glob の削除              |
|-------------------|-------------------------------------------|---------------------------------------------------------------|----------------------------|
| Compile           | \*\*/\*.cs (または他の言語拡張機能) | \*\*/\*.user;  \*\*/\*.\*proj;  \*\*/\*.sln;  \*\*/\*.vssscc  | N/A                      |
| EmbeddedResource  | \*\*/\*.resx                              | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | N/A                      |
| None              | \*\*/\*                                   | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | \*\*/\*.cs; \*\*/\*.resx   |

> [!NOTE]
> **除外される glob** では、`./bin` と `./obj` フォルダーが常に除外されます。これらはそれぞれ MSBuild プロパティ `$(BaseOutputPath)` と `$(BaseIntermediateOutputPath)` で表されます。 全体として、すべての除外は `$(DefaultItemExcludes)` で表されます。

プロジェクトに glob があり、最新の SDK を使用してビルドしようとすると、次のエラーが発生します。

> 重複するコンパイル項目が含まれていました。 .NET SDK には、既定でプロジェクト ディレクトリのコンパイル項目が含まれています。 これらの項目をプロジェクト ファイルから削除するか、プロジェクト ファイルに明示的に含める場合は 'EnableDefaultCompileItems' プロパティを 'false' に設定することができます。

このエラーを回避するには、前の表にあるものと一致する明示的な `Compile` 項目を削除するか、次のように `<EnableDefaultCompileItems>` プロパティを `false` に設定します。

```xml
<PropertyGroup>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```

このプロパティを `false` に設定すると、暗黙的に含める動作が無効になり、以前の SDK の動作に戻り、プロジェクトに既定の glob が指定されます。

この変更で、他の include の主なしくみは変わりません。 ただし、たとえばアプリで発行する一部のファイルを指定する場合は、*csproj* で既知のしくみ (たとえば `<Content>` 要素) を使用することができます。

`<EnableDefaultCompileItems>` は `Compile` glob のみを無効にし、\*.cs 項目にも適用される暗黙的 `None` glob など、他の Glob には影響しません。 そのため、**ソリューション エクスプローラー**は \*.cs 項目を `None` 項目として含まれた、プロジェクトの一部として引き続き表示します。 同様に、次のように `<EnableDefaultNoneItems>` を false に設定し、暗黙的 `None` glob を無効にできます。

```xml
<PropertyGroup>
    <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
</PropertyGroup>
```

**暗黙的 glob をすべて**無効にするには、`<EnableDefaultItems>` プロパティを `false` に設定します。次の例をご覧ください。

```xml
<PropertyGroup>
    <EnableDefaultItems>false</EnableDefaultItems>
</PropertyGroup>
```

## <a name="how-to-see-the-whole-project-as-msbuild-sees-it"></a>MSBuild と同じようにプロジェクト全体を表示する方法

これらの csproj 変更でプロジェクト ファイルが大幅に簡素化されますが、SDK とそのターゲットが追加されたとき、MSBuild と同様にプロジェクト全体を表示すると便利なことがあります。 [`dotnet msbuild`](dotnet-msbuild.md) コマンドの [`/pp` スイッチ](/visualstudio/msbuild/msbuild-command-line-reference#preprocess)でプロジェクトを事前処理します。インポートされるファイル、そのソース、ビルドに対するその貢献が、実際にプロジェクトをビルドすることなく、表示されます。

`dotnet msbuild -pp:fullproject.xml`

プロジェクトにターゲット フレームワークが複数存在する場合、MSBuild プロパティとして指定し、1 つだけにコマンドの結果を集中させてください。

`dotnet msbuild -p:TargetFramework=netcoreapp2.0 -pp:fullproject.xml`

## <a name="additions"></a>追加

### <a name="sdk-attribute"></a>SDK 属性

*.csproj* ファイルのルート `<Project>` 要素には、`Sdk` という新しい属性があります。 `Sdk` は、プロジェクトで使用される SDK を指定します。 [レイヤー化のドキュメント](cli-msbuild-architecture.md)で説明されているように、SDK は、.NET Core コードをビルドできる MSBuild [タスク](/visualstudio/msbuild/msbuild-tasks)および[ターゲット](/visualstudio/msbuild/msbuild-targets)のセットです。 .NET Core では、次の SDK を利用できます。

1. ID が `Microsoft.NET.Sdk` の .NET Core SDK
2. ID が `Microsoft.NET.Sdk.Web` の .NET Core Web SDK
3. ID が `Microsoft.NET.Sdk.Razor` の .NET Core Razor クラス ライブラリ SDK
4. ID が `Microsoft.NET.Sdk.Worker` の .NET Core Worker Service (.NET Core 3.0 以降)
5. ID が `Microsoft.NET.Sdk.WindowsDesktop` の .NET Core WinForms および WPF (.NET Core 3.0 以降)

.NET Core ツールを使用し、コードをビルドするには、`Sdk` 属性を `<Project>` 要素の ID のいずれかに設定する必要があります。

### <a name="packagereference"></a>PackageReference

`<PackageReference>` 項目要素では、[プロジェクトでの NuGet の依存関係](/nuget/consume-packages/package-references-in-project-files)を指定します。 `Include` 属性は、パッケージ ID を指定します。

```xml
<PackageReference Include="<package-id>" Version="" PrivateAssets="" IncludeAssets="" ExcludeAssets="" />
```

#### <a name="version"></a>バージョン

必須の `Version` 属性では、復元するパッケージのバージョンを指定します。 この属性は、[NuGet バージョン管理](/nuget/reference/package-versioning#version-ranges-and-wildcards)スキームの規則に従います。 既定の動作は、最小一致バージョンです。 たとえば、`Version="1.2.3"` を指定すると、NuGet 表記の `[1.2.3, )` と同等になります。つまり、パッケージのバージョンは、使用できる場合は 1.2.3 になり、使用できない場合はその後のバージョンになります。

#### <a name="includeassets-excludeassets-and-privateassets"></a>IncludeAssets、ExcludeAssets、PrivateAssets

`IncludeAssets` 属性は、`<PackageReference>` で指定されているパッケージに属するアセットのうち、使う必要があるものを指定します。 既定では、パッケージのすべてのアセットが含まれます。

`ExcludeAssets` 属性は、`<PackageReference>` で指定されているパッケージに属するアセットのうち、使う必要がないものを指定します。

`PrivateAssets` 属性は、`<PackageReference>` で指定されているパッケージに属するアセットで、使う必要はあるが、次のプロジェクトに渡してはならないものを指定します。 この属性が存在しない場合、`Analyzers`、`Build`、`ContentFiles` アセットは既定でプライベートになります。

> [!NOTE]
> `PrivateAssets` は *project.json*/*xproj* `SuppressParent` 要素と同等です。

これらの属性には以下の項目を 1 つ以上含めることができ、複数ある場合はセミコロン `;` 文字で区切ります。

- `Compile` - コンパイルで使用できる *lib* フォルダーの内容です。
- `Runtime` - 配布する *runtime* フォルダーの内容です。
- `ContentFiles` - 使用する *contentfiles* フォルダーの内容です。
- `Build` - 使用する *build* フォルダーのプロパティ/ターゲットです。
- `Native` - ランタイムの *output* フォルダーにコピーするネイティブ アセットの内容です。
- `Analyzers` - アナライザーが使用されます。

代わりに、次の値を属性に含めることもできます。

- `None` - いずれのアセットも使用されません。
- `All` - すべてのアセットが使用されます。

### <a name="dotnetclitoolreference"></a>DotNetCliToolReference

`<DotNetCliToolReference>` 項目要素は、プロジェクトのコンテキストでユーザーが復元を望む CLI ツールを指定します。 *project.json* の `tools` ノードに代わるものです。

```xml
<DotNetCliToolReference Include="<package-id>" Version="" />
```

`DotNetCliToolReference` は[現在非推奨であり](https://github.com/dotnet/announcements/issues/107)、[.NET Core Local Tools](https://aka.ms/local-tools) の使用が推奨されています。

#### <a name="version"></a>バージョン

`Version` は、復元するパッケージのバージョンを指定します。 この属性は、[NuGet バージョン管理](/nuget/create-packages/dependency-versions#version-ranges)スキームの規則に従います。 既定の動作は、最小一致バージョンです。 たとえば、`Version="1.2.3"` を指定すると、NuGet 表記の `[1.2.3, )` と同等になります。つまり、パッケージのバージョンは、使用できる場合は 1.2.3 になり、使用できない場合はその後のバージョンになります。

### <a name="runtimeidentifiers"></a>RuntimeIdentifiers

`<RuntimeIdentifiers>` プロパティ要素では、プロジェクトの[ランタイム識別子 (RID)](../rid-catalog.md) のセミコロン区切りリストを指定できます。
RID により、自己完結型の展開を発行できます。

```xml
<RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
```

### <a name="runtimeidentifier"></a>RuntimeIdentifier

`<RuntimeIdentifier>` プロパティ要素では、プロジェクトの[ランタイム識別子 (RID)](../rid-catalog.md) を 1 つだけ指定できます。 RID により、自己完結型の展開を発行できます。

```xml
<RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```

複数のランタイムに対して発行する必要がある場合、代わりに `<RuntimeIdentifiers>` (複数) を使用します。 `<RuntimeIdentifier>` では、必要なランタイムが 1 つだけのとき、ビルドが速くなります。

### <a name="packagetargetfallback"></a>PackageTargetFallback

`<PackageTargetFallback>` プロパティ要素では、パッケージの復元時に使用する、互換性のある一連のターゲットを指定できます。 dotnet [TxM (Target x Moniker)](/nuget/schema/target-frameworks) を使用するパッケージに、dotnet TxM を宣言しないパッケージで動作することを許可するように設計されています。 プロジェクトで dotnet TxM を使用せず、依存するすべてのパッケージに dotnet TxM を与える必要がある場合、非 dotnet プラットフォームを dotnet 対応にするためにプロジェクトに `<PackageTargetFallback>` を追加します。

次の例では、プロジェクトのすべてのターゲットにフォールバックを提供しています。

```xml
<PackageTargetFallback>
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

次の例では、`netcoreapp2.1` ターゲットにのみフォールバックを指定しています。

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='netcoreapp2.1'">
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

## <a name="build-events"></a>ビルド イベント

プロジェクト ファイルでビルド前およびビルド後のイベントを指定する方法が変更されました。 $ (ProjectDir) などのマクロが解決されないため、PreBuildEvent および PostBuildEvent プロパティは SDK スタイルのプロジェクト形式では推奨されません。 たとえば、次のコードはサポートされなくなりました。

```xml
<PropertyGroup>
    <PreBuildEvent>"$(ProjectDir)PreBuildEvent.bat" "$(ProjectDir)..\" "$(ProjectDir)" "$(TargetDir)" />
</PropertyGroup>
```

SDK スタイルのプロジェクトでは、`PreBuild` または `PostBuild` という名前の MSBuild ターゲットを使用し、`PreBuild` の `BeforeTargets` プロパティまたは `AfterTargets` の `PostBuild` プロパティを設定します。 前の例では、次のコードを使用します。

```xml
<Target Name="PreBuild" BeforeTargets="PreBuildEvent">
    <Exec Command="&quot;$(ProjectDir)PreBuildEvent.bat&quot; &quot;$(ProjectDir)..\&quot; &quot;$(ProjectDir)&quot; &quot;$(TargetDir)&quot;" />
</Target>

<Target Name="PostBuild" AfterTargets="PostBuildEvent">
   <Exec Command="echo Output written to $(TargetDir)" />
</Target>
```

> [!NOTE]
>MSBuild ターゲットには任意の名前を使用できますが、Visual Studio IDE では `PreBuild` と `PostBuild` ターゲットが認識されるため、Visual Studio IDE でコマンドを編集できるようにするには、これらの名前を使用することをお勧めします。

## <a name="nuget-metadata-properties"></a>NuGet メタデータ プロパティ

MSBuild への移行に伴い、*project.json* ファイルから *csproj* ファイルに NuGet パッケージをパックするときに使用される入力メタデータを移動しました。 入力は MSBuild プロパティなので、`<PropertyGroup>` グループ内で行う必要があります。 次に示すのは、`dotnet pack` コマンドまたは SDK の一部である `Pack` MSBuild ターゲットを使用するときに、パッキング プロセスへの入力として使用されるプロパティの一覧です。

### <a name="ispackable"></a>IsPackable

プロジェクトをパックできるかどうかを示すブール値。 既定値は `true` です。

### <a name="packageversion"></a>PackageVersion

結果のパッケージのバージョンを指定します。 すべてのフォームの NuGet バージョン文字列を受け入れます。 既定値は `$(Version)` です。つまり、プロジェクトのプロパティ `Version` の値です。

### <a name="packageid"></a>PackageId

結果のパッケージの名前を指定します。 指定しない場合、`pack` 操作の既定では、`AssemblyName` またはディレクトリ名をパッケージ名として使用します。

### <a name="title"></a>Title

人が読みやすいパッケージのタイトル。通常、nuget.org と、Visual Studio のパッケージ マネージャーの UI 画面で使用されます。 指定しない場合、パッケージ ID が代わりに使用されます。

### <a name="authors"></a>Authors

nuget.org のプロファイル名と一致するパッケージ作成者をセミコロンで区切った一覧。これらは nuget.org の NuGet ギャラリーに表示され、同じ作成者によるパッケージの相互参照に使用されます。

### <a name="packagedescription"></a>PackageDescription

UI 画面用のパッケージの長い説明。

### <a name="description"></a>説明

アセンブリの長い説明。 `PackageDescription` が指定されていない場合、このプロパティはパッケージの説明としても使用されます。

### <a name="copyright"></a>Copyright

パッケージの著作権の詳細。

### <a name="packagerequirelicenseacceptance"></a>PackageRequireLicenseAcceptance

クライアントがユーザーに対して、パッケージのインストール前にパッケージ ライセンスに同意することを必須にするかどうかを示すブール値。 既定値は、`false` です。

### <a name="developmentdependency"></a>DevelopmentDependency

開発専用の依存関係としてパッケージをマークするかどうかを指定するブール値。指定すると、そのパッケージは他のパッケージに依存関係として含まれなくなります。 PackageReference (NuGet 4.8 以降) では、このフラグは、コンパイルからコンパイル時アセットが除外されることも意味します。 詳しくは、「[DevelopmentDependency support for PackageReference (PackageReference に対する DevelopmentDependency のサポート)](https://github.com/NuGet/Home/wiki/DevelopmentDependency-support-for-PackageReference)」をご覧ください。

### <a name="packagelicenseexpression"></a>PackageLicenseExpression

[SPDX ライセンス識別子](https://spdx.org/licenses/)、または式です。 たとえば、`Apache-2.0` のようにします。

[SPDX ライセンス識別子](https://spdx.org/licenses/)の完全な一覧はこちらをご覧ください。 ライセンス タイプ式を使用する場合、NuGet.org では OSI または FSF で承認されたライセンスのみが受け付けられます。

ライセンス式の正確な構文については、[ABNF](https://tools.ietf.org/html/rfc5234) をご覧ください。

```abnf
license-id            = <short form license identifier from https://spdx.org/spdx-specification-21-web-version#h.luq9dgcle9mo>

license-exception-id  = <short form license exception identifier from https://spdx.org/spdx-specification-21-web-version#h.ruv3yl8g6czd>

simple-expression = license-id / license-id”+”

compound-expression =  1*1(simple-expression /
                simple-expression "WITH" license-exception-id /
                compound-expression "AND" compound-expression /
                compound-expression "OR" compound-expression ) /
                "(" compound-expression ")" )

license-expression =  1*1(simple-expression / compound-expression / UNLICENSED)
```

> [!NOTE]
> 一度に指定できるのは、`PackageLicenseExpression`、`PackageLicenseFile`、`PackageLicenseUrl` のどれか 1 つだけです。

### <a name="packagelicensefile"></a>PackageLicenseFile

SPDX 識別子が割り当てられていないライセンス、またはカスタム ライセンスを使用している場合、パッケージ内のライセンス ファイルへのパス (それ以外の場合は、`PackageLicenseExpression` が優先されます)

`PackageLicenseUrl` が置き換えられ、`PackageLicenseExpression` と組み合わせることはできず、Visual Studio バージョン 15.9.4 および.NET SDK 2.1.502 または 2.2.101 以降が必要です。

プロジェクトに明示的に追加することによって、ライセンス ファイルをパックする必要があります。使用例を次に示します。

```xml
<PropertyGroup>
  <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
</PropertyGroup>
<ItemGroup>
  <None Include="licenses\LICENSE.txt" Pack="true" PackagePath="$(PackageLicenseFile)"/>
</ItemGroup>
```

### <a name="packagelicenseurl"></a>PackageLicenseUrl

パッケージに適用されるライセンスの URL。 ("_Visual Studio 15.9.4、.NET SDK 2.1.502 および 2.2.101 以降では非推奨_")

### <a name="packageiconurl"></a>PackageIconUrl

UI 画面のパッケージのアイコンとして使用する背景が透明な 64x64 の画像の URL。

### <a name="packagereleasenotes"></a>PackageReleaseNotes

パッケージのリリース ノート。

### <a name="packagetags"></a>PackageTags

パッケージを指定するタグのセミコロン区切りの一覧。

### <a name="packageoutputpath"></a>PackageOutputPath

パックされたパッケージをドロップする出力パスを指定します。 既定値は `$(OutputPath)` です。

### <a name="includesymbols"></a>IncludeSymbols
このブール値は、プロジェクトをパックするときに、パッケージが追加のシンボル パッケージを作成するかどうかを指定します。 シンボル パッケージの形式は、`SymbolPackageFormat` プロパティで制御します。

### <a name="symbolpackageformat"></a>SymbolPackageFormat
シンボル パッケージの形式を指定します。 "symbols.nupkg" では、 *.symbols.nupkg* 拡張子と共に、PDB、DLL、およびその他の出力ファイルを含む従来のシンボル パッケージが作成されます。 "snupkg" では、ポータブル PDB を含む snupkg シンボル パッケージが作成されます。 既定値は "symbols.nupkg" です。

### <a name="includesource"></a>IncludeSource

このブール値は、パック プロセスでソース パッケージを作成するかどうかを示します。 ソース パッケージには、ライブラリのソース コードと PDB ファイルが含まれます。 ソース ファイルは、結果のパッケージ ファイルの `src/ProjectName` ディレクトリに置かれます。

### <a name="istool"></a>IsTool

すべての出力ファイルを *lib* フォルダーではなく *tools* フォルダーにコピーするかどうかを指定します。 *.csproj* ファイルに `PackageType` を設定することで指定される `DotNetCliTool` とは異なります。

### <a name="repositoryurl"></a>RepositoryUrl

パッケージのソース コードがある、またはビルド元のリポジトリの URL を指定します。

### <a name="repositorytype"></a>RepositoryType

リポジトリの種類を指定します。 既定値は "git" です。

### <a name="repositorybranch"></a>RepositoryBranch
リポジトリ内のソース ブランチの名前を指定します。 プロジェクトが NuGet パッケージにパッケージ化されると、パッケージ メタデータに追加されます。

### <a name="repositorycommit"></a>RepositoryCommit
任意のリポジトリ コミットまたは変更セット。パッケージがどのソースに対してビルドされたかを示します。 このプロパティを含めるには、`RepositoryUrl` も指定する必要があります。 プロジェクトが NuGet パッケージにパッケージ化されると、このコミットまたは変更セットがパッケージ メタデータに追加されます。

### <a name="nopackageanalysis"></a>NoPackageAnalysis

パッケージのビルド後に、パックでパッケージの分析を実行しないことを指定します。

### <a name="minclientversion"></a>MinClientVersion

nuget.exe および Visual Studio パッケージ マネージャーで強制する、このパッケージをインストールできる NuGet クライアントの最小バージョンを指定します。

### <a name="includebuildoutput"></a>IncludeBuildOutput

このブール値は、ビルド出力アセンブリを *.nupkg* ファイルにパッケージ化するかどうかを指定します。

### <a name="includecontentinpack"></a>IncludeContentInPack

このブール値は、種類が `Content` の項目を結果のパッケージに自動的に含めるかどうかを指定します。 既定値は、`true` です。

### <a name="buildoutputtargetfolder"></a>BuildOutputTargetFolder

出力アセンブリを配置するフォルダーを指定します。 出力アセンブリ (および他の出力ファイル) は、各フレームワーク フォルダーにコピーされます。

### <a name="contenttargetfolders"></a>ContentTargetFolders

このプロパティには、`PackagePath` が指定されていない場合に、すべてのコンテンツ ファイルを配置する既定の場所を指定します。 既定値は "content;contentFiles" です。

### <a name="nuspecfile"></a>NuspecFile

パックに使用する *.nuspec* ファイルの相対パスまたは絶対パス。

> [!NOTE]
> *.nuspec* ファイルを指定すると、情報のパッケージに**のみ**使用され、プロジェクト内の情報は使用されません。

### <a name="nuspecbasepath"></a>NuspecBasePath

*.nuspec* ファイルのベース パス。

### <a name="nuspecproperties"></a>NuspecProperties

キー=値ペアのセミコロン区切りの一覧。

## <a name="assemblyinfo-properties"></a>AssemblyInfo プロパティ

通常、*AssemblyInfo* ファイル内に存在していた[アセンブリ属性](../../standard/assembly/set-attributes.md)は、プロパティから自動的に生成されるようになりました。

### <a name="properties-per-attribute"></a>属性ごとのプロパティ

次の表に示すように、各属性にはその内容を制御するプロパティと、その生成を無効にするプロパティがあります。

| 属性                                                      | プロパティ               | 無効にするプロパティ                             |
|----------------------------------------------------------------|------------------------|-------------------------------------------------|
| <xref:System.Reflection.AssemblyCompanyAttribute>              | `Company`              | `GenerateAssemblyCompanyAttribute`              |
| <xref:System.Reflection.AssemblyConfigurationAttribute>        | `Configuration`        | `GenerateAssemblyConfigurationAttribute`        |
| <xref:System.Reflection.AssemblyCopyrightAttribute>            | `Copyright`            | `GenerateAssemblyCopyrightAttribute`            |
| <xref:System.Reflection.AssemblyDescriptionAttribute>          | `Description`          | `GenerateAssemblyDescriptionAttribute`          |
| <xref:System.Reflection.AssemblyFileVersionAttribute>          | `FileVersion`          | `GenerateAssemblyFileVersionAttribute`          |
| <xref:System.Reflection.AssemblyInformationalVersionAttribute> | `InformationalVersion` | `GenerateAssemblyInformationalVersionAttribute` |
| <xref:System.Reflection.AssemblyProductAttribute>              | `Product`              | `GenerateAssemblyProductAttribute`              |
| <xref:System.Reflection.AssemblyTitleAttribute>                | `AssemblyTitle`        | `GenerateAssemblyTitleAttribute`                |
| <xref:System.Reflection.AssemblyVersionAttribute>              | `AssemblyVersion`      | `GenerateAssemblyVersionAttribute`              |
| <xref:System.Resources.NeutralResourcesLanguageAttribute>      | `NeutralLanguage`      | `GenerateNeutralResourcesLanguageAttribute`     |

メモ:

- `AssemblyVersion` と `FileVersion` の既定値は、サフィックスなしで `$(Version)` の値を受け取ることです。 たとえば、`$(Version)` が `1.2.3-beta.4` の場合、値は `1.2.3` です。
- `InformationalVersion` の既定値は、`$(Version)` の値です。
- プロパティが存在する場合、`InformationalVersion` の末尾には `$(SourceRevisionId)` が付加されます。 `IncludeSourceRevisionInInformationalVersion` を使用して無効にすることができます。
- NuGet メタデータには、`Copyright` および `Description` プロパティも使用されます。
- `Configuration` はすべてのビルド プロセスで共有され、設定には `dotnet` コマンドの`--configuration` パラメーターを使用します。

### <a name="generateassemblyinfo"></a>GenerateAssemblyInfo

すべての AssemblyInfo 生成を有効または無効にするブール値。 既定値は `true` です。

### <a name="generatedassemblyinfofile"></a>GeneratedAssemblyInfoFile

生成されたアセンブリ情報ファイルのパス。 既定値は `$(IntermediateOutputPath)` (obj) ディレクトリ内のファイルです。
