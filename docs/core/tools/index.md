---
title: .NET Core CLI
titleSuffix: ''
description: .NET Core CLI とその機能に関する概要です。
ms.date: 02/13/2020
ms.openlocfilehash: 45a40063f70a621e807abf5e01ceecb125aecd7c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397600"
---
# <a name="net-core-cli-overview"></a>.NET Core CLI の概要

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

.NET Core コマンド ライン インターフェイス (CLI) は、.NET Core アプリケーションを開発、ビルド、実行、発行するためのクロスプラットフォーム ツールチェーンです。

.NET Core CLI は、[.NET Core SDK](../sdk.md) に含まれています。 .NET Core SDK をインストールする方法については、「[.NET Core SDK をインストールする](../install/sdk.md)」をご覧ください。

## <a name="cli-commands"></a>CLI コマンド

既定では、次のコマンドがインストールされます。

### <a name="basic-commands"></a>基本的なコマンド

- [`new`](dotnet-new.md)
- [`restore`](dotnet-restore.md)
- [`build`](dotnet-build.md)
- [`publish`](dotnet-publish.md)
- [`run`](dotnet-run.md)
- [`test`](dotnet-test.md)
- [`vstest`](dotnet-vstest.md)
- [`pack`](dotnet-pack.md)
- [`migrate`](dotnet-migrate.md)
- [`clean`](dotnet-clean.md)
- [`sln`](dotnet-sln.md)
- [`help`](dotnet-help.md)
- [`store`](dotnet-store.md)

### <a name="project-modification-commands"></a>プロジェクトの変更コマンド

- [`add package`](dotnet-add-package.md)
- [`add reference`](dotnet-add-reference.md)
- [`remove package`](dotnet-remove-package.md)
- [`remove reference`](dotnet-remove-reference.md)
- [`list reference`](dotnet-list-reference.md)

### <a name="advanced-commands"></a>高度なコマンド

- [`nuget delete`](dotnet-nuget-delete.md)
- [`nuget locals`](dotnet-nuget-locals.md)
- [`nuget push`](dotnet-nuget-push.md)
- [`msbuild`](dotnet-msbuild.md)
- [`dotnet install script`](dotnet-install-script.md)

### <a name="tool-management-commands"></a>ツール管理コマンド

- [`tool install`](dotnet-tool-install.md)
- [`tool list`](dotnet-tool-list.md)
- [`tool update`](dotnet-tool-update.md)
- [`tool restore`](global-tools.md#install-a-local-tool) .NET Core SDK 3.0 以降で利用できます。
- [`tool run`](global-tools.md#invoke-a-local-tool) .NET Core SDK 3.0 以降で利用できます。
- [`tool uninstall`](dotnet-tool-uninstall.md)

ツールは、NuGet パッケージからインストールされ、コマンド プロンプトから呼び出されるコンソール アプリケーションです。 ツールは自分で作成することも、サードパーティによって作成されたツールをインストールすることもできます。 ツールは、グローバル ツール、ツールパス ツール、およびローカル ツールとも呼ばれます。 詳細については、[.NET Core ツールの概要](global-tools.md)に関するページを参照してください。

## <a name="command-structure"></a>コマンド構造

CLI コマンド構造は、[ドライバー ("dotnet")](#driver) と[コマンド](#command)、また場合によってはコマンドの[引数](#arguments)と[オプション](#options)で構成されます。 *my_app* という名前のディレクトリから実行する場合、次のコマンドで示されているように、新しいコンソール アプリの作成やコマンド ラインからの実行など、ほとんどの CLI 操作でこのパターンが見られます。

```dotnetcli
dotnet new console
dotnet build --output /build_output
dotnet /build_output/my_app.dll
```

### <a name="driver"></a>ドライバー

ドライバーは [dotnet](dotnet.md) という名前で、2 つの役割 ([フレームワークに依存するアプリ](../deploying/index.md)の実行と、コマンドの実行) があります。

フレームワークに依存するアプリを実行するには、`dotnet /path/to/my_app.dll` など、ドライバーの後にアプリを指定します。 アプリの DLL が存在するフォルダーからコマンドを実行する場合は、`dotnet my_app.dll` を実行するだけです。 .NET Core ランタイムの特定のバージョンを使用する場合は、`--fx-version <VERSION>` オプションを使用してください (「[dotnet コマンド](dotnet.md)」リファレンスを参照)。

ドライバーにコマンドを指定すると、`dotnet.exe` は CLI コマンドの実行プロセスを開始します。 次に例を示します。

```dotnetcli
dotnet build
```

最初に、ドライバーは使用する SDK のバージョンを決定します。 [global.json](global-json.md) ファイルがない場合は、利用可能な SDK の最新バージョンを使用します。 コンピューターで最新のプレビューまたは安定したバージョンになります。  SDK バージョンが決定されたら、コマンドを実行します。

### <a name="command"></a>コマンド

コマンドは、アクションを実行します。 たとえば、`dotnet build` はコードをビルドします。 `dotnet publish` はコードを発行します。 コマンドは、`dotnet {command}` 規則を使用してコンソール アプリケーションとして実装されます。

### <a name="arguments"></a>引数

コマンド ラインで渡す引数は、呼び出されたコマンドへの引数です。 たとえば、`dotnet publish my_app.csproj` を実行した場合、`my_app.csproj` 引数は、発行するプロジェクトを示し、`publish` コマンドに渡されます。

### <a name="options"></a>オプション

コマンド ラインで渡すオプションは、呼び出されたコマンドへのオプションです。 たとえば、`dotnet publish --output /build_output` を実行した場合、`--output` オプションとその値は `publish` コマンドに渡されます。

## <a name="see-also"></a>関連項目

- [dotnet/sdk GitHub リポジトリ](https://github.com/dotnet/sdk/)
- [.NET Core のインストール ガイド](../install/sdk.md)
