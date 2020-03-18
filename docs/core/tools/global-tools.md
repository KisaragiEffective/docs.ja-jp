---
title: .NET Core ツール
description: .NET Core ツールをインストール、使用、更新、および削除する方法。 グローバル ツール、tool-path ツール、およびローカル ツールについて説明します。
author: KathleenDollard
ms.date: 02/12/2020
ms.openlocfilehash: d8ee30df3fe063fd41a85072d145b1b5eec7d0d0
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543392"
---
# <a name="how-to-manage-net-core-tools"></a>.NET Core ツールの管理方法

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

.NET Core ツールは、コンソール アプリケーションを含む特殊な NuGet パッケージです。 ツールは、次の方法でコンピューターにインストールできます。

* グローバル ツールとして。

  ツールのバイナリは、PATH 環境変数に追加された既定のディレクトリにインストールされます。 その場所を指定しなくても、マシン上の任意のディレクトリからツールを呼び出すことができます。 1 つのバージョンのツールがマシン上のすべてのディレクトリに使用されます。

* カスタムの場所のグローバル ツールとして (tool-path ツールとも呼ばれます)。

  ツールのバイナリは、指定した場所にインストールされます。 ツールを呼び出すには、インストール ディレクトリから実行するか、コマンド名と共にディレクトリを指定するか、ディレクトリを PATH 環境変数に追加します。 1 つのバージョンのツールがマシン上のすべてのディレクトリに使用されます。

* ローカル ツールとして (.NET Core SDK 3.0 以降に適用されます)。

  ツールのバイナリは、既定のディレクトリにインストールされます。 ツールは、インストール ディレクトリまたはそのいずれかのサブディレクトリから起動します。 ディレクトリごとに、同じツールの異なるバージョンを使用できます。
  
  .NET CLI では、マニフェスト ファイルを使用して、ディレクトリにローカルとしてインストールされているツールを追跡します。 マニフェスト ファイルがソース コード リポジトリのルート ディレクトリに保存されると、共同作成者は、リポジトリを複製し、1 つの .NET Core CLI コマンドを呼び出して、マニフェスト ファイルに一覧表示されているすべてのツールがインストールすることができます。

> [!IMPORTANT]
> .NET Core ツールは完全な信頼で実行されます。 作成者を信頼していない場合は、その .NET Core ツールをインストールしないでください。

## <a name="find-a-tool"></a>ツールを検索する

現在、.NET Core にはツールの検索機能がありません。 ツールを見つける方法をいくつか紹介します。

* ツールの一覧については、[natemcmaster/dotnet-tools](https://github.com/natemcmaster/dotnet-tools) GitHub リポジトリを参照してください。
* [ToolGet](https://www.toolget.net/) を使用して .NET ツールを検索します。
* ASP.NET Core チームが作成したツールのソース コードについては、[dotnet/aspnetcore GitHub リポジトリの Tools ディレクトリ](https://github.com/dotnet/aspnetcore/tree/master/src/Tools)を参照してください。
* 診断ツールについては、[.NET Core dotnet 診断ツール](../diagnostics/index.md#net-core-dotnet-diagnostic-global-tools)に関する記事を参照してください。
* [NuGet](https://www.nuget.org) Web サイトを検索します。 ただし、NuGet サイトには、ツール パッケージのみを検索できる機能はまだありません。

## <a name="check-the-author-and-statistics"></a>作成者と統計情報の確認

.NET Core ツールは完全な信頼で実行され、グローバル ツールが PATH 環境変数に追加されるため、非常に強力です。 信頼できないユーザーからツールをダウンロードしないでください。

ツールが NuGet でホストされている場合は、ツールを検索することで作成者と統計情報を確認できます。

## <a name="install-a-global-tool"></a>グローバル ツールをインストールする

ツールをグローバル ツールとしてインストールするには、次の例に示すように、[dotnet tool install](dotnet-tool-install.md) の `-g` または `--global` オプションを使用します。

```dotnetcli
dotnet tool install -g dotnetsay
```

出力には、次の例のように、ツールの起動に使用されたコマンドとインストールされているバージョンが表示されます。

```output
You can invoke the tool using the following command: dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
```

ツールのバイナリの既定の場所は、オペレーティング システムによって異なります。

| OS          | パス                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

この場所は、SDK を初めて実行したときにユーザーのパスに追加されるため、ツールの場所を指定せずに任意のディレクトリからグローバル ツールを呼び出すことができます。

ツールへのアクセスはユーザー固有であり、マシン グローバルではありません。 グローバル ツールは、ツールをインストールしたユーザーのみが使用できます。

### <a name="install-a-global-tool-in-a-custom-location"></a>カスタムの場所にグローバル ツールをインストールする

カスタムの場所にグローバル ツールとしてツールをインストールするには、次の例に示すように、[dotnet tool install](dotnet-tool-install.md) の `--tool-path` オプションを使用します。

Windows の場合:

```dotnetcli
dotnet tool install dotnetsay --tool-path c:\dotnet-tools
```

Linux または macOS の場合:

```dotnetcli
dotnet tool install dotnetsay --tool-path ~/bin
```

この場所は、.NET Core SDK によって PATH 環境変数に自動的に追加されません。 [tool-path ツールを呼び出す](#invoke-a-tool-path-tool)には、次のいずれかの方法を使用して、コマンドを使用できることを確認する必要があります。

* インストール ディレクトリを PATH 環境変数に追加します。
* ツールを起動するときは完全なパスを指定します。
* インストール ディレクトリ内からツールを呼び出します。

## <a name="install-a-local-tool"></a>ローカル ツールをインストールする

**.NET Core 3.0 SDK 以降に適用されます。**

ローカル アクセス専用のツール (現在のディレクトリとサブディレクトリ用) をインストールするには、ツール マニフェスト ファイルに追加する必要があります。 ツール マニフェスト ファイルを作成するには、`dotnet new tool-manifest` コマンドを実行します。

```dotnetcli
dotnet new tool-manifest
```

このコマンドにより、 *.config* ディレクトリ以下に *dotnet-tools.json* というマニフェスト ファイルが作成されます。 ローカル ツールをマニフェスト ファイルに追加するには、次の例に示すように、[dotnet tool install](dotnet-tool-install.md) コマンドを使用し、`--global` および `--tool-path` オプションを**省略**します。

```dotnetcli
dotnet tool install dotnetsay
```

コマンド出力には、次の例のように、新しくインストールされたツールが含まれるマニフェスト ファイルが表示されます。

```console
You can invoke the tool from this directory using the following command:
dotnet tool run dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
Entry is added to the manifest file /home/name/botsay/.config/dotnet-tools.json.
```

2 つのローカル ツールがインストールされたマニフェスト ファイルの例を次に示します。

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    },
    "dotnetsay": {
      "version": "2.1.3",
      "commands": [
        "dotnetsay"
      ]
    }
  }
}
```

通常、ローカル ツールは、リポジトリのルート ディレクトリに追加します。 マニフェスト ファイルをリポジトリにチェックインした後は、リポジトリからコードをチェック アウトした開発者が最新のマニフェスト ファイルを取得します。 マニフェスト ファイルに記載されているすべてのツールをインストールするには、`dotnet tool restore` コマンドを実行します。

```dotnetcli
dotnet tool restore
```

出力には、復元されたツールが示されます。

```console
Tool 'botsay' (version '1.0.0') was restored. Available commands: botsay
Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
Restore was successful.
```

## <a name="install-a-specific-tool-version"></a>特定のツールバー ジョンをインストールする

プレリリース バージョンまたは特定バージョンのツールをインストールするには、次の例に示すように、`--version` オプションを使用してバージョン番号を指定します。

```dotnetcli
dotnet tool install dotnetsay --version 2.1.3
```

## <a name="use-a-tool"></a>ツールを使用する

ツールを呼び出すために使用するコマンドは、インストールするパッケージの名前と異なる場合があります。 現在のユーザーのマシンに現在インストールされているすべてのツールを表示するには、[dotnet tool list](dotnet-tool-list.md) コマンドを使用します。

```dotnetcli
dotnet tool list
```

出力には、次の例のように、各ツールのバージョンとコマンドが表示されます。

```console
Package Id      Version      Commands       Manifest
-------------------------------------------------------------------------------------------
botsay          1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
dotnetsay       2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
```

この例に示すように、一覧にはローカル ツールが表示されます。 グローバル ツールを表示するには、`--global` オプションを使用します。また、tool-path ツールを表示するには、`--tool-path` オプションを使用します。

### <a name="invoke-a-global-tool"></a>グローバル ツールを呼び出す

グローバル ツールの場合は、ツール コマンドを単独で使用します。 たとえば、コマンドが `dotnetsay` または `dotnet-doc` の場合は、そのコマンドを使用してコマンドを呼び出します。

```console
dotnetsay
dotnet-doc
```

コマンドがプレフィックス `dotnet-` で始まる場合、ツールを呼び出すもう 1 つの方法は、`dotnet` コマンドを使用して、ツール コマンド プレフィックスを省略することです。 たとえば、コマンドが `dotnet-doc` の場合、次のコマンドを使ってツールを呼び出します。

```dotnetcli
dotnet doc
```

ただし、次のシナリオでは、`dotnet` コマンドを使ってグローバル ツールを呼び出すことはできません。

* グローバル ツールとローカル ツールには、同じコマンドの先頭に `dotnet-` が付きます。
* ローカル ツールのスコープ内にあるディレクトリからグローバル ツールを呼び出します。

このシナリオでは、`dotnet doc` および `dotnet dotnet-doc` によってローカル ツールが呼び出されます。 グローバル ツールを呼び出すには、コマンドを単独で使用します。

```dotnetcli
dotnet-doc
```

### <a name="invoke-a-tool-path-tool"></a>tool-path ツールを呼び出す

`tool-path` オプションを使用してインストールされたグローバル ツールを呼び出すには、[この記事で前述](#install-a-global-tool-in-a-custom-location)したように、コマンドを使用できることを確認します。

### <a name="invoke-a-local-tool"></a>ローカル ツールを呼び出す

ローカル ツールを呼び出すには、インストール ディレクトリ内から `dotnet` コマンドを使用する必要があります。 次の例に示すように、長い形式 (`dotnet tool run <COMMAND_NAME>`) または短い形式 (`dotnet <COMMAND_NAME>`) を使用できます。

```dotnetcli
dotnet tool run dotnetsay
dotnet dotnetsay
```

コマンドの先頭に `dotnet-` が付いている場合、ツールを呼び出すときにプレフィックスを含めるか省略することができます。 たとえば、コマンドが `dotnet-doc` の場合、次の例のいずれかを使ってローカル ツールを呼び出します。

```dotnetcli
dotnet tool run dotnet-doc
dotnet dotnet-doc
dotnet doc
```

## <a name="update-a-tool"></a>ツールを更新する

ツールを更新するには、ツールをアンインストールしてから、最新の安定バージョンで再インストールする必要があります。 ツールを更新するには、ツールのインストールに使用したものと同じオプションを指定して [dotnet tool update](dotnet-tool-update.md) コマンドを使用します。

```dotnetcli
dotnet tool update --global <packagename>
dotnet tool update --tool-path <packagename>
dotnet tool update <packagename>
```

ローカル ツールの場合、SDK を使うと、現在のディレクトリと親ディレクトリを検索してパッケージ ID を含む最初のマニフェスト ファイルを見つけることができます。 マニフェスト ファイルにそのようなパッケージ ID がない場合は、SDK によって、最も近いマニフェスト ファイルに新しいエントリが追加されます。

## <a name="uninstall-a-tool"></a>ツールをアンインストールする

[dotnet tool uninstall](dotnet-tool-uninstall.md) コマンドを使用して、ツールのインストールに使用したものと同じオプションを使用してツールを削除します。

```dotnetcli
dotnet tool uninstall --global <packagename>
dotnet tool uninstall --tool-path<packagename>
dotnet tool uninstall <packagename>
```

ローカル ツールの場合、SDK を使うと、現在のディレクトリと親ディレクトリを検索してパッケージ ID を含む最初のマニフェスト ファイルを見つけることができます。

## <a name="get-help-and-troubleshoot"></a>ヘルプとトラブルシューティングを取得する

使用できる `dotnet tool` コマンドの一覧を取得するには、次のコマンドを入力します。

```dotnetcli
dotnet tool --help
```

ツールの使用手順を取得するには、次のコマンドのいずれかを入力するか、ツールの Web サイトを参照してください。

```dotnetcli
<command> --help
dotnet <command> --help
```

ツールのインストールまたは実行に失敗した場合は、「[.NET Core ツールの使用に関する問題のトラブルシューティング](troubleshoot-usage-issues.md)」を参照してください。
