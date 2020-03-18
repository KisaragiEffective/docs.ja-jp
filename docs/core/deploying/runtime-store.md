---
title: ランタイム パッケージ ストア
description: .NET Core で使用されるランタイム パッケージ ストアとターゲット マニフェストの使用方法について説明します。
ms.date: 08/12/2017
ms.openlocfilehash: 7a833ed95147608c6fb403f8f0dec179d2a73833
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77448959"
---
# <a name="runtime-package-store"></a>ランタイム パッケージ ストア

NET Core 2.0 以降、ターゲット環境に存在する既知のパッケージのセットに対して、アプリをパッケージ化して展開できるようになりました。 この利点は、展開を高速化し、ディスク領域の使用を減らし、場合によっては起動時のパフォーマンスを向上させることです。

この機能は、パッケージが保存されているディスク上のディレクトリ (通常は、macOS/Linux では */usr/local/share/dotnet/store*、Windows では *C:/Program Files/dotnet/store*) である、*ランタイム パッケージ ストア*として実装されます。 このディレクトリには、アーキテクチャと[ターゲット フレームワーク](../../standard/frameworks.md)のサブディレクトリがあります。 ファイル レイアウトは、[NuGet アセットがディスク上に配置される](/nuget/create-packages/supporting-multiple-target-frameworks#framework-version-folder-structure)方法と同様です。

```
\dotnet
    \store
        \x64
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
        \x86
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
```

*ターゲット マニフェスト* ファイルには、ランタイム パッケージ ストアのパッケージが一覧されます。 開発者は、自社のアプリを発行するときに、このマニフェストをターゲットにすることができます。 ターゲット マニフェストは通常、対象となる運用環境の所有者によって提供されます。

## <a name="preparing-a-runtime-environment"></a>ランタイム環境を準備する

ランタイム環境の管理者は、ランタイム パッケージ ストアとそれに対応するターゲット マニフェストを作成して、アプリを最適化し、展開を高速化してディスク領域の使用を減らすことができます。

最初のステップは、ランタイム パッケージ ストアを構成するパッケージを一覧した*パッケージ ストア マニフェスト*を作成することです。 このファイル形式は、プロジェクトのファイル形式 (*csproj*) と互換性があります。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="<NUGET_PACKAGE>" Version="<VERSION>" />
    <!-- Include additional packages here -->
  </ItemGroup>
</Project>
```

**例**

次のパッケージ ストア マニフェスト (*packages.csproj*) の例は、[`Newtonsoft.Json`](https://www.nuget.org/packages/Newtonsoft.Json/) と [`Moq`](https://www.nuget.org/packages/moq/) をランタイム パッケージ ストアに追加するために使用されます。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.3" />
    <PackageReference Include="Moq" Version="4.7.63" />
  </ItemGroup>
</Project>
```

パッケージ ストア マニフェスト、ランタイム、フレームワークと共に `dotnet store` を実行して、ランタイム パッケージ ストアをプロビジョニングします。

```dotnetcli
dotnet store --manifest <PATH_TO_MANIFEST_FILE> --runtime <RUNTIME_IDENTIFIER> --framework <FRAMEWORK>
```

**例**

```dotnetcli
dotnet store --manifest packages.csproj --runtime win10-x64 --framework netcoreapp2.0 --framework-version 2.0.0
```

複数のターゲット パッケージ ストアのマニフェストのパスは、コマンドにオプションとパスを繰り返すことで、単一の [`dotnet store`](../tools/dotnet-store.md) コマンドで渡すことができます。

既定では、コマンドの出力は、ユーザーのプロファイルの *.dotnet/store* サブディレクトリにあるパッケージ ストアに出力されます。 `--output <OUTPUT_DIRECTORY>` オプションを使って、別の場所を指定することができます。 ストアのルート ディレクトリには、ターゲット マニフェストの *artifact.xml* ファイルが含まれます。 このファイルは、ダウンロードに使用でき、発行時にはこのストアをターゲットにするアプリの作成者が使用できます。

**例**

次の *artifact.xml* ファイルは、前の例を実行した後に生成されます。 [`Castle.Core`](https://www.nuget.org/packages/Castle.Core/) は `Moq` の依存関係であるため、自動的に *artifacts.xml* マニフェスト ファイルに含められ、表示されます。

```xml
<StoreArtifacts>
  <Package Id="Newtonsoft.Json" Version="10.0.3" />
  <Package Id="Castle.Core" Version="4.1.0" />
  <Package Id="Moq" Version="4.7.63" />
</StoreArtifacts>
```

## <a name="publishing-an-app-against-a-target-manifest"></a>ターゲット マニフェストに対してアプリを発行する

ディスクにターゲット マニフェスト ファイルがある場合、[`dotnet publish`](../tools/dotnet-publish.md) コマンドでアプリを発行するときに、ファイルへのパスを指定します。

```dotnetcli
dotnet publish --manifest <PATH_TO_MANIFEST_FILE>
```

**例**

```dotnetcli
dotnet publish --manifest manifest.xml
```

結果として発行されるアプリを、ターゲット マニフェストに示されているパッケージがある環境に展開します。 この操作に失敗すると、アプリの起動に失敗することになります。

オプションとパス (例: `--manifest manifest1.xml --manifest manifest2.xml`) を繰り返して、アプリを発行するときに複数のターゲット マニフェストを指定します。 この操作を行うときに、アプリはコマンドに指定されたターゲット マニフェスト ファイルで指定されたパッケージの和集合に対してトリミングされます。

## <a name="specifying-target-manifests-in-the-project-file"></a>プロジェクト ファイルでターゲット マニフェストを指定する

[`dotnet publish`](../tools/dotnet-publish.md) コマンドでターゲット マニフェストを指定する代わりに、 **\<TargetManifestFiles>** タグにセミコロンで区切られたパスのリストとして、プロジェクト ファイルに指定することができます。

```xml
<PropertyGroup>
  <TargetManifestFiles>manifest1.xml;manifest2.xml</TargetManifestFiles>
</PropertyGroup>
```

アプリのターゲット環境が .Net Core プロジェクトのようによく知られている場合にのみ、プロジェクト ファイルにターゲット マニフェストを指定します。 これは、オープンソース プロジェクトには該当しません。 オープンソース プロジェクトのユーザーは通常、さまざまな運用環境にアプリを展開します。 一般的に、これらの運用環境には、さまざまなインストール済みのパッケージのセットがあります。 このような環境でターゲット マニフェストについて想定することができないため、`--manifest`[`dotnet publish` の ](../tools/dotnet-publish.md) オプションを使用する必要があります。

## <a name="aspnet-core-implicit-store"></a>ASP.NET Core の暗黙的なストア

ASP.NET Core の暗黙的なストアは、ASP.NET Core 2.0 にのみ適用されます。 暗黙的なストアを使用**しない** ASP.NET Core 2.1 以降を、アプリケーションで使用することを強くお勧めします。 ASP.NET Core 2.1 以降では、共有フレームワークを使用します。

このランタイム パッケージ ストア機能は、アプリが[フレームワークに依存する展開 (FDD)](index.md#publish-runtime-dependent) のアプリとして展開されるときに、ASP.NET Core アプリによって暗黙的に使用されます。 [`Microsoft.NET.Sdk.Web`](https://github.com/aspnet/websdk) のターゲットには、ターゲット システムの暗黙的なパッケージ ストアを参照しているマニフェストが含まれます。 さらに、`Microsoft.AspNetCore.All` パッケージによって異なる FDD アプリは、アプリとそのアセットのみを含み、`Microsoft.AspNetCore.All` メタパッケージに一覧されるパッケージは含まないアプリとして発行されます。 それらのパッケージは、ターゲット システムに存在すると想定されます。

ランタイム パッケージ ストアは、.NET Core SDK がインストールされるときに、ホストにインストールされます。 その他のインストーラーでは、Zip/tarball インストールの .NET Core SDK、`apt-get`、Red Hat Yum、.NET Core Windows Server Hosting バンドル、および手動のランタイム パッケージ ストアのインストールなど、ランタイム パッケージ ストアを提供する場合があります。

[フレームワークに依存する展開 (FDD)](index.md#publish-runtime-dependent) アプリを展開するときに、ターゲット環境に .NET Core SDK がインストールされていることを確認します。 アプリが ASP.NET Core を含まない環境に展開される場合、次の例のように、プロジェクト ファイルで **\<PublishWithAspNetCoreTargetManifest>** を `false` に指定して、暗黙的なストアを無効にできます。

```xml
<PropertyGroup>
  <PublishWithAspNetCoreTargetManifest>false</PublishWithAspNetCoreTargetManifest>
</PropertyGroup>
```

> [!NOTE]
> [自己完結型の展開 (SCD)](index.md#publish-self-contained) アプリの場合、ターゲット システムには必ずしも必要なマニフェスト パッケージを含んでいないと想定されます。 そのため、SCD アプリの場合、 **\<PublishWithAspNetCoreTargetManifest>** を `true` に設定することはできません。

展開に存在するマニフェストの依存関係を持つアプリケーションを展開する場合は (アセンブリは *bin* フォルダーに存在します)、ランタイム パッケージ ストアはそのアセンブリ用のホスト上では*使用されません*。 ホスト上のランタイム パッケージ ストアに存在するかに関係なく、*bin* フォルダーのアセンブリが使用されます。

マニフェストに示された依存関係のバージョンは、ランタイム パッケージ ストアの依存関係のバージョンに一致する必要があります。 ターゲット マニフェストの依存関係とランタイム パッケージ ストアに存在するバージョンの間に一致しないバージョンがあり、アプリにその展開に必要なバージョンのパッケージが含まれていない場合、アプリを開始することはできません。 この例外には、ランタイム パッケージ ストアのアセンブリを呼び出したターゲット マニフェストの名前があり、これは、この不一致のトラブルシューティングに役立ちます。

展開が発行時に*トリミング*されると、指定した特定のバージョンのマニフェスト パッケージのみが、発行された出力から引かれます。 アプリを開始するには、指定されたバージョンのパッケージがホスト上に存在する必要があります。

## <a name="see-also"></a>参照

- [dotnet-publish](../tools/dotnet-publish.md)
- [dotnet-store](../tools/dotnet-store.md)
