---
title: dotnet publish コマンド
description: dotnet publish コマンドを実行すると、.NET Core プロジェクトまたはソリューションをディレクトリに発行できます。
ms.date: 02/24/2020
ms.openlocfilehash: c34618409c9a539043c84c7e03daa8aa249d64f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146556"
---
# <a name="dotnet-publish"></a>dotnet publish

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet publish` - ホスティング システムへの展開のため、アプリケーションとその依存関係をフォルダーに発行します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet publish [<PROJECT>|<SOLUTION>] [-c|--configuration]
    [-f|--framework] [--force] [--interactive] [--manifest]
    [--no-build] [--no-dependencies] [--no-restore] [--nologo]
    [-o|--output] [-r|--runtime] [--self-contained]
    [--no-self-contained] [-v|--verbosity] [--version-suffix]

dotnet publish [-h|--help]
```

## <a name="description"></a>説明

`dotnet publish` はアプリケーションをコンパイルし、プロジェクト ファイルに指定されたその依存関係を読み取り、結果のファイル セットをディレクトリに発行します。 出力には次のアセットが含まれます。

- アセンブリの中間言語 (IL) コード (*dll* 拡張子)。
- *.deps.json* ファイル。プロジェクトのすべての依存関係が含まれます。
- *.runtimeconfig.json* ファイル。アプリケーションが想定する共有ランタイムと、ランタイム用の他の構成オプション (ガベージ コレクションの種類など) を指定します。
- アプリケーションの依存関係。NuGet キャッシュから出力フォルダーにコピーされます。

`dotnet publish` コマンドの出力は、実行のためにホスト システム (サーバー、PC、Mac、ラップトップなど) にすぐに展開できます。 これは、アプリケーションの展開を準備するための正式にサポートされている唯一の方法です。 プロジェクトに指定されている展開の種類によっては、ホスティング システムに .NET Core 共有ランタイムがインストールされている場合とされていない場合があります。

## <a name="arguments"></a>引数

- **`PROJECT|SOLUTION`**

  発行するプロジェクトまたはソリューション。
  
  * `PROJECT` は、[C#](csproj.md)、F#、または Visual Basic のプロジェクト ファイルのパスおよびファイル名か、C#、F#、または Visual Basic のプロジェクト ファイルを含むディレクトリへのパスです。 ディレクトリが指定されていない場合は、既定で現在のディレクトリに設定されます。

  * `SOLUTION` は、ソリューション ファイルのパスとファイル名 ( *.sln* 拡張子)、またはソリューション ファイルを含むディレクトリのパスです。 ディレクトリが指定されていない場合は、既定で現在のディレクトリに設定されます。 .NET Core 3.0 SDK 以降で使用できます。

## <a name="options"></a>オプション

- **`-c|--configuration <CONFIGURATION>`**

  ビルド構成を定義します。 ほとんどのプロジェクトの既定値は `Debug` ですが、プロジェクトでビルド構成設定をオーバーライドできます。

- **`-f|--framework <FRAMEWORK>`**

  指定した[ターゲット フレームワーク](../../standard/frameworks.md)のアプリケーションを発行します。 ターゲット フレームワークはプロジェクト ファイルで指定する必要があります。

- **`--force`**

  最後の復元が成功した場合でも、すべての依存関係が強制的に解決されます。 このフラグを指定することは、*project.assets.json* ファイルを削除することと同じです。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`--interactive`**

  コマンドを停止して、ユーザーの入力または操作のために待機させることができます。 たとえば、認証を完了する場合があります。 .NET Core 3.0 SDK 以降で使用できます。

- **`--manifest <PATH_TO_MANIFEST_FILE>`**

  アプリによって公開された一連のパッケージをトリミングするために使用する[ターゲット マニフェスト](../deploying/runtime-store.md)を 1 つまたは複数指定します。 マニフェスト ファイルは、[`dotnet store` コマンド](dotnet-store.md)の出力の一部です。 複数のマニフェストを指定するには、マニフェストごとに `--manifest` を追加します。

- **`--no-build`**

  発行の前にプロジェクトをビルドしません。 また、`--no-restore` フラグが暗黙的に設定されます。

- **`--no-dependencies`**

  プロジェクト間参照を無視し、ルート プロジェクトのみを復元します。

- **`--nologo`**

  著作権情報を表示しません。 .NET Core 3.0 SDK 以降で使用できます。

- **`--no-restore`**

  コマンドを実行するときに、暗黙的な復元を実行しません。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  出力ディレクトリのパスを指定します。 指定しない場合、ランタイムに依存する実行可能ファイルおよびクロスプラットフォーム バイナリの既定値は *./bin/[configuration]/[framework]/publish/* に設定されます。 自己完結型の実行可能ファイルの場合、既定値は *./bin/[configuration]/[framework]/[runtime]/publish/* です。

  パスが相対的である場合、生成された出力ディレクトリは、現在の作業ディレクトリではなく、プロジェクト ファイルの場所に相対的なパスとなります。

- **`--self-contained [true|false]`**

  アプリケーションと一緒に .NET Core ランタイムを発行します。これにより、ランタイムをターゲット コンピューターにインストールする必要がなくなります。 ランタイム識別子が指定されている場合、既定値は `true` です。 詳細については、[.NET Core アプリケーションの発行](../deploying/index.md)に関する記事と「[.NET Core CLI を使用して .NET Core アプリを発行する](../deploying/deploy-with-cli.md)」を参照してください。

- **`--no-self-contained`**

  これは、`--self-contained false` に相当します。 .NET Core 3.0 SDK 以降で使用できます。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  指定されたランタイムのアプリケーションを発行します。 ランタイム ID (RID) の一覧については、[RID カタログ](../rid-catalog.md)に関するページをご覧ください。 詳細については、[.NET Core アプリケーションの発行](../deploying/index.md)に関する記事と「[.NET Core CLI を使用して .NET Core アプリを発行する](../deploying/deploy-with-cli.md)」を参照してください。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `minimal`にする必要があります。

- **`--version-suffix <VERSION_SUFFIX>`**

  プロジェクト ファイルのバージョン フィールドでアスタリスク (`*`) を置き換えるバージョン サフィックスを定義します。

## <a name="examples"></a>使用例

- 現在のディレクトリ内にプロジェクト用の[ランタイムに依存するクロスプラットフォーム バイナリ](../deploying/index.md#produce-a-cross-platform-binary)を作成します。

  ```dotnetcli
  dotnet publish
  ```

  .NET Core 3.0 SDK 以降の場合、この例では、現在のプラットフォーム用の[ランタイムに依存する実行可能ファイル](../deploying/index.md#publish-runtime-dependent)も作成されます。

- 特定のランタイムに対して、現在のディレクトリ内にプロジェクト用の[自己完結型の実行可能ファイル](../deploying/index.md#publish-self-contained)を作成します。

  ```dotnetcli
  dotnet publish --runtime osx.10.11-x64
  ```

  RID をプロジェクト ファイルに含める必要があります。

- 特定のプラットフォーム用に、プロジェクト用の[ランタイムに依存する実行可能ファイル](../deploying/index.md#publish-runtime-dependent)を現在のディレクトリに作成します。

  ```dotnetcli
  dotnet publish --runtime osx.10.11-x64 --self-contained false
  ```

  RID をプロジェクト ファイルに含める必要があります。 この例は、.NET Core 3.0 SDK 以降のバージョンに適用されます。

- 特定のランタイムとターゲット フレームワーク用に、現在のディレクトリのプロジェクトを発行します。

  ```dotnetcli
  dotnet publish --framework netcoreapp3.1 --runtime osx.10.11-x64
  ```

- 指定されたプロジェクト ファイルを発行します。

  ```dotnetcli
  dotnet publish ~/projects/app1/app1.csproj
  ```

- 現在のアプリケーションを発行します。ただし、復元操作中はルート プロジェクトのみを復元し、プロジェクト間 (P2P) 参照は復元しないでください。

  ```dotnetcli
  dotnet publish --no-dependencies
  ```

## <a name="see-also"></a>関連項目

- [.NET Core アプリケーションの発行の概要](../deploying/index.md)
- [.NET Core CLI を使用して .NET Core アプリを発行する](../deploying/deploy-with-cli.md)
- [ターゲット フレームワーク](../../standard/frameworks.md)
- [ランタイム識別子 (RID) のカタログ](../rid-catalog.md)
- [macOS Catalina の公証に対応する](../install/macos-notarization-issues.md) 詳細については、次のリソースを参照してください。
- [発行されたアプリケーションのディレクトリ構造](/aspnet/core/hosting/directory-structure)
