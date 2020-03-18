---
title: project.json と csproj の比較
description: 「project.json 要素と csproj 要素の間のマッピング」を参照してください。
author: natemcmaster
ms.date: 03/13/2017
ms.openlocfilehash: abe515007b47b415ac33e3350a29edced1784d68
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77451106"
---
# <a name="a-mapping-between-projectjson-and-csproj-properties"></a>project.json プロパティと csproj プロパティの間のマッピング

作成者: [Nate McMaster](https://github.com/natemcmaster)

.NET Core ツールの開発中、重要なデザイン変更が行われました。*project.json* ファイルのサポートが終了となり、代わりに.NET Core プロジェクトが MSBuild/csproj 形式に移行されました。

この記事では、*project.json* の設定が MSBuild/csproj 形式でどのように表示されるか説明します。最新バージョンのツールにプロジェクトをアップグレードするとき、新しい形式の利用方法を知り、移行ツールで行われた変更を理解できます。

## <a name="the-csproj-format"></a>csproj 形式

新しい形式の \*.csproj は XML ベースの形式です。 次の例は、`Microsoft.NET.Sdk` を利用した .NET Core プロジェクトのルート ノードです。 Web プロジェクトの場合、使用される SDK は `Microsoft.NET.Sdk.Web` です。

```xml
<Project Sdk="Microsoft.NET.Sdk">
...
</Project>
```

## <a name="common-top-level-properties"></a>一般的な最上位プロパティ

### <a name="name"></a>name

```json
{
  "name": "MyProjectName"
}
```

サポート対象から除外されました。 csproj では、これはプロジェクト ファイル名により決定され、通常はディレクトリ名と一致します。 たとえば、「 `MyProjectName.csproj` 」のように入力します。

既定では、プロジェクト ファイル名により、`<AssemblyName>` プロパティと `<PackageId>` プロパティの値も指定されます。

```xml
<PropertyGroup>
  <AssemblyName>MyProjectName</AssemblyName>
  <PackageId>MyProjectName</PackageId>
</PropertyGroup>
```

project.json に `<AssemblyName>` プロパティが定義されている場合、`<PackageId>` には `buildOptions\outputName` 以外の値が設定されます。
詳細については、「[その他の共通ビルド オプション](#other-common-build-options)」を参照してください。

### <a name="version"></a>version

```json
{
  "version": "1.0.0-alpha-*"
}
```

`VersionPrefix` プロパティおよび `VersionSuffix` プロパティを使用します。

```xml
<PropertyGroup>
  <VersionPrefix>1.0.0</VersionPrefix>
  <VersionSuffix>alpha</VersionSuffix>
</PropertyGroup>
```

`Version` プロパティを使用することもできますが、これにより、パッケージ処理中にバージョン設定がオーバーライドされることがあります。

```xml
<PropertyGroup>
  <Version>1.0.0-alpha</Version>
</PropertyGroup>
```

### <a name="other-common-root-level-options"></a>その他の共通のルートレベル オプション

```json
{
  "authors": [ "Anne", "Bob" ],
  "company": "Contoso",
  "language": "en-US",
  "title": "My library",
  "description": "This is my library.\r\nAnd it's really great!",
  "copyright": "Nugetizer 3000",
  "userSecretsId": "xyz123"
}
```

```xml
<PropertyGroup>
  <Authors>Anne;Bob</Authors>
  <Company>Contoso</Company>
  <NeutralLanguage>en-US</NeutralLanguage>
  <AssemblyTitle>My library</AssemblyTitle>
  <Description>This is my library.
And it's really great!</Description>
  <Copyright>Nugetizer 3000</Copyright>
  <UserSecretsId>xyz123</UserSecretsId>
</PropertyGroup>
```

## <a name="frameworks"></a>frameworks

### <a name="one-target-framework"></a>1 つのターゲット フレームワーク

```json
{
  "frameworks": {
    "netcoreapp1.0": {}
  }
}
```

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp1.0</TargetFramework>
</PropertyGroup>
```

### <a name="multiple-target-frameworks"></a>複数のターゲット フレームワーク

```json
{
  "frameworks": {
    "netcoreapp1.0": {},
    "net451": {}
  }
}
```

`TargetFrameworks` プロパティを使用し、ターゲット フレームワークの一覧を定義します。 複数のフレームワーク値を区切るには、セミコロンを使用します。

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp1.0;net451</TargetFrameworks>
</PropertyGroup>
```

## <a name="dependencies"></a>dependencies

> [!IMPORTANT]
> 依存関係がパッケージではなく、**プロジェクト**の場合、形式は異なります。
> 詳細については、「[依存関係の種類](#dependency-type)」セクションを参照してください。

### <a name="netstandardlibrary-metapackage"></a>NETStandard.Library のメタパッケージ

```json
{
  "dependencies": {
    "NETStandard.Library": "1.6.0"
  }
}
```

```xml
<PropertyGroup>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

### <a name="microsoftnetcoreapp-metapackage"></a>Microsoft.NETCore.App のメタパッケージ

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": "1.0.0"
  }
}
```

```xml
<PropertyGroup>
  <RuntimeFrameworkVersion>1.0.3</RuntimeFrameworkVersion>
</PropertyGroup>
```

移行されたプロジェクトの `<RuntimeFrameworkVersion>` 値はインストールした SDK のバージョンにより決定されます。

### <a name="top-level-dependencies"></a>最上位の依存関係

```json
{
  "dependencies": {
    "Microsoft.AspNetCore": "1.1.0"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore" Version="1.1.0" />
</ItemGroup>
```

### <a name="per-framework-dependencies"></a>フレームワーク別の依存関係

```json
{
  "framework": {
    "net451": {
      "dependencies": {
        "System.Collections.Immutable": "1.3.1"
      }
    },
    "netstandard1.5": {
      "dependencies": {
        "Newtonsoft.Json": "9.0.1"
      }
    }
  }
}
```

```xml
<ItemGroup Condition="'$(TargetFramework)'=='net451'">
  <PackageReference Include="System.Collections.Immutable" Version="1.3.1" />
</ItemGroup>

<ItemGroup Condition="'$(TargetFramework)'=='netstandard1.5'">
  <PackageReference Include="Newtonsoft.Json" Version="9.0.1" />
</ItemGroup>
```

### <a name="imports"></a>インポート

```json
{
  "dependencies": {
    "YamlDotNet": "4.0.1-pre309"
  },
  "frameworks": {
    "netcoreapp1.0": {
      "imports": [
        "dnxcore50",
        "dotnet"
      ]
    }
  }
}
```

```xml
<PropertyGroup>
  <PackageTargetFallback>dnxcore50;dotnet</PackageTargetFallback>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="YamlDotNet" Version="4.0.1-pre309" />
</ItemGroup>
```

### <a name="dependency-type"></a>依存関係の種類

#### <a name="type-project"></a>type: project

```json
{
  "dependencies": {
    "MyOtherProject": "1.0.0-*",
    "AnotherProject": {
      "type": "project"
    }
  }
}
```

```xml
<ItemGroup>
  <ProjectReference Include="..\MyOtherProject\MyOtherProject.csproj" />
  <ProjectReference Include="..\AnotherProject\AnotherProject.csproj" />
</ItemGroup>
```

> [!NOTE]
> `dotnet pack --version-suffix $suffix` がプロジェクト参照の依存関係バージョンを決定する方法が無効になります。

#### <a name="type-build"></a>type: build

```json
{
  "dependencies": {
    "Microsoft.EntityFrameworkCore.Design": {
      "version": "1.1.0",
      "type": "build"
    }
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="1.1.0" PrivateAssets="All" />
</ItemGroup>
```

#### <a name="type-platform"></a>type: platform

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": {
      "version": "1.1.0",
      "type": "platform"
    }
  }
}
```

csproj には同等のものがありません。

## <a name="runtimes"></a>runtimes

```json
{
  "runtimes": {
    "win7-x64": {},
    "osx.10.11-x64": {},
    "ubuntu.16.04-x64": {}
  }
}
```

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win7-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="standalone-apps-self-contained-deployment"></a>スタンドアロン アプリ (自己完結型の展開)

project.json では、`runtimes` セクションを定義することは、ビルドと公開の間にアプリがスタンドアロンであったことを意味します。
MSBuild では、ビルド中、すべてのプロジェクトが*移植可能*ですが、スタンドアロンとして公開できます。

`dotnet publish --framework netcoreapp1.0 --runtime osx.10.11-x64`

詳細については、「[自己完結型の展開 (SCD)](../deploying/index.md#publish-self-contained)」を参照してください。

## <a name="tools"></a>ツール

```json
{
  "tools": {
    "Microsoft.EntityFrameworkCore.Tools.DotNet": "1.0.0-*"
  }
}
```

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.0" />
</ItemGroup>
```

> [!NOTE]
> ツールの `imports` は、csproj ではサポートされません。 インポートを必要とするツールは、新しい `Microsoft.NET.Sdk` で機能しません。

## <a name="buildoptions"></a>buildOptions

「[Files](#files)」も参照してください。

### <a name="emitentrypoint"></a>emitEntryPoint

```json
{
  "buildOptions": {
    "emitEntryPoint": true
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

`emitEntryPoint` が `false` であった場合、`OutputType` の値は `Library` に変換されます。これが既定値です。

```json
{
  "buildOptions": {
    "emitEntryPoint": false
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Library</OutputType>
  <!-- or, omit altogether. It defaults to 'Library' -->
</PropertyGroup>
```

### <a name="keyfile"></a>keyFile

```json
{
  "buildOptions": {
    "keyFile": "MyKey.snk"
  }
}
```

`keyFile` 要素は、MSBuild で 3 つのプロパティになりました。

```xml
<PropertyGroup>
  <AssemblyOriginatorKeyFile>MyKey.snk</AssemblyOriginatorKeyFile>
  <SignAssembly>true</SignAssembly>
  <PublicSign Condition="'$(OS)' != 'Windows_NT'">true</PublicSign>
</PropertyGroup>
```

### <a name="other-common-build-options"></a>その他の共通ビルド オプション

```json
{
  "buildOptions": {
    "warningsAsErrors": true,
    "nowarn": ["CS0168", "CS0219"],
    "xmlDoc": true,
    "preserveCompilationContext": true,
    "outputName": "Different.AssemblyName",
    "debugType": "portable",
    "allowUnsafe": true,
    "define": ["TEST", "OTHERCONDITION"]
  }
}
```

```xml
<PropertyGroup>
  <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  <NoWarn>$(NoWarn);CS0168;CS0219</NoWarn>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <PreserveCompilationContext>true</PreserveCompilationContext>
  <AssemblyName>Different.AssemblyName</AssemblyName>
  <DebugType>portable</DebugType>
  <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
  <DefineConstants>$(DefineConstants);TEST;OTHERCONDITION</DefineConstants>
</PropertyGroup>
```

## <a name="packoptions"></a>packOptions

「[Files](#files)」も参照してください。

### <a name="common-pack-options"></a>共通パック オプション

```json
{
  "packOptions": {
    "summary": "numl is a machine learning library intended to ease the use of using standard modeling techniques for both prediction and clustering.",
    "tags": ["machine learning", "framework"],
    "releaseNotes": "Version 0.9.12-beta",
    "iconUrl": "http://numl.net/images/ico.png",
    "projectUrl": "http://numl.net",
    "licenseUrl": "https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md",
    "requireLicenseAcceptance": false,
    "repository": {
      "type": "git",
      "url": "https://raw.githubusercontent.com/sethjuarez/numl"
    },
    "owners": ["Seth Juarez"]
  }
}
```

```xml
<PropertyGroup>
  <!-- summary is not migrated from project.json, but you can use the <Description> property for that if needed. -->
  <PackageTags>machine learning;framework</PackageTags>
  <PackageReleaseNotes>Version 0.9.12-beta</PackageReleaseNotes>
  <PackageIconUrl>http://numl.net/images/ico.png</PackageIconUrl>
  <PackageProjectUrl>http://numl.net</PackageProjectUrl>
  <PackageLicenseUrl>https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md</PackageLicenseUrl>
  <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
  <RepositoryType>git</RepositoryType>
  <RepositoryUrl>https://raw.githubusercontent.com/sethjuarez/numl</RepositoryUrl>
  <!-- owners is not supported in MSBuild -->
</PropertyGroup>
```

MSBuild では、`owners` 要素に相当するものはありません。
`summary` の場合、MSBuild の `<Description>` プロパティを利用できます (ただし、`summary` の値はそのプロパティに自動的に移行されません)。そのプロパティが [`description`](#other-common-root-level-options) 要素にマッピングされているためです。

## <a name="scripts"></a>スクリプト

```json
{
  "scripts": {
    "precompile": "generateCode.cmd",
    "postpublish": [ "obfuscate.cmd", "removeTempFiles.cmd" ]
  }
}
```

MSBuild でこれに相当するものは[ターゲット](/visualstudio/msbuild/msbuild-targets)です。

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="generateCode.cmd" />
</Target>

<Target Name="MyPostCompileTarget" AfterTargets="Publish">
  <Exec Command="obfuscate.cmd" />
  <Exec Command="removeTempFiles.cmd" />
</Target>
```

## <a name="runtimeoptions"></a>runtimeOptions

```json
{
  "runtimeOptions": {
    "configProperties": {
      "System.GC.Server": true,
      "System.GC.Concurrent": true,
      "System.GC.RetainVM": true,
      "System.Threading.ThreadPool.MinThreads": 4,
      "System.Threading.ThreadPool.MaxThreads": 25
    }
  }
}
```

"System.GC.Server" プロパティを除き、このグループのすべての設定がプロジェクト フォルダーの *runtimeconfig.template.json* というファイルに配置されます。オプションは移行プロセス中にルート オブジェクトに移動します。

```json
{
  "configProperties": {
    "System.GC.Concurrent": true,
    "System.GC.RetainVM": true,
    "System.Threading.ThreadPool.MinThreads": 4,
    "System.Threading.ThreadPool.MaxThreads": 25
  }
}
```

"System.GC.Server" プロパティは csproj ファイルに移行されます。

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

ただし、csproj のこれらの値はすべて MSBuild プロパティと共に設定できます。

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
  <ConcurrentGarbageCollection>true</ConcurrentGarbageCollection>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
  <ThreadPoolMaxThreads>25</ThreadPoolMaxThreads>
</PropertyGroup>
```

## <a name="shared"></a>shared

```json
{
  "shared": "shared/**/*.cs"
}
```

csproj ではサポートされていません。 代わりに、 *.nuspec* ファイルにコンテンツ ファイルを追加する必要があります。
詳細については、「[Including content files](/nuget/schema/nuspec#including-content-files)」 (コンテンツ ファイルを追加する) を参照してください。

## <a name="files"></a>ファイル

*project.json* では、ビルドとパックは、複数のフォルダーからのコンパイルと埋め込みまで拡張できます。
MSBuild では、これは[項目](/visualstudio/msbuild/common-msbuild-project-items)の使用により行われます。 次の例は一般的な変換です。

```json
{
  "buildOptions": {
    "compile": {
      "copyToOutput": "notes.txt",
      "include": "../Shared/*.cs",
      "exclude": "../Shared/Not/*.cs"
    },
    "embed": {
      "include": "../Shared/*.resx"
    }
  },
  "packOptions": {
    "include": "Views/",
    "mappings": {
      "some/path/in/project.txt": "in/package.txt"
    }
  },
  "publishOptions": {
    "include": [
      "files/",
      "publishnotes.txt"
    ]
  }
}
```

```xml
<ItemGroup>
  <Compile Include="..\Shared\*.cs" Exclude="..\Shared\Not\*.cs" />
  <EmbeddedResource Include="..\Shared\*.resx" />
  <Content Include="Views\**\*" PackagePath="%(Identity)" />
  <None Include="some/path/in/project.txt" Pack="true" PackagePath="in/package.txt" />
  
  <None Include="notes.txt" CopyToOutputDirectory="Always" />
  <!-- CopyToOutputDirectory = { Always, PreserveNewest, Never } -->

  <Content Include="files\**\*" CopyToPublishDirectory="PreserveNewest" />
  <None Include="publishnotes.txt" CopyToPublishDirectory="Always" />
  <!-- CopyToPublishDirectory = { Always, PreserveNewest, Never } -->
</ItemGroup>
```

> [!NOTE]
> 既定の [Glob パターン](https://en.wikipedia.org/wiki/Glob_(programming))の多くは .NET Core SDK により自動的に追加されます。
> 詳細については、「[Default Compile Item Values](https://aka.ms/sdkimplicititems)」 (既定のコンパイル項目値) を参照してください。

すべての MSBuild `ItemGroup` 要素で `Include`、`Exclude`、`Remove` がサポートされています。

.nupkg 内のパッケージ レイアウトは `PackagePath="path"` で変更できます。

`Content` を除き、ほとんどの項目グループで、パッケージに `Pack="true"` を明示的に追加する必要があります。 MSBuild の `Content` プロパティが既定で  *に設定されているため、* はパッケージの`<IncludeContentInPack>`コンテンツ`true` フォルダーに置かれます。
詳細については、「[Including content in a package](/nuget/schema/msbuild-targets#including-content-in-a-package)」 (パッケージにコンテンツを追加する) を参照してください。

`PackagePath="%(Identity)"` は、パッケージ パスをプロジェクト関連のファイル パスに設定する簡単な方法です。

## <a name="testrunner"></a>testRunner

### <a name="xunit"></a>xUnit

```json
{
  "testRunner": "xunit",
  "dependencies": {
    "dotnet-test-xunit": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="xunit" Version="2.2.0-*" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0-*" />
</ItemGroup>
```

### <a name="mstest"></a>MSTest

```json
{
  "testRunner": "mstest",
  "dependencies": {
    "dotnet-test-mstest": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.12-*" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.11-*" />
</ItemGroup>
```

## <a name="see-also"></a>参照

- [CLI の変更の概要](../tools/cli-msbuild-architecture.md)
