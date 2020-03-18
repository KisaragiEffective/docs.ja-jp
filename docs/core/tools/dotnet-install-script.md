---
title: dotnet-install スクリプト
description: .NET Core SDK と共有ランタイムをインストールするための dotnet-install スクリプトについて学習します。
ms.date: 01/23/2020
ms.openlocfilehash: bf28f872be3ac2b4115b1d5e5c06e32afec0b49e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77092864"
---
# <a name="dotnet-install-scripts-reference"></a>dotnet-install スクリプト リファレンス

## <a name="name"></a>name

`dotnet-install.ps1` | `dotnet-install.sh` - .NET Core SDK と共有ランタイムをインストールするために使うスクリプトです。

## <a name="synopsis"></a>構文

Windows の場合:

```powershell
dotnet-install.ps1 [-Channel] [-Version] [-JSonFile] [-InstallDir] [-Architecture]
    [-Runtime] [-DryRun] [-NoPath] [-Verbose] [-AzureFeed] [-UncachedFeed] [-NoCdn] [-FeedCredential]
    [-ProxyAddress] [-ProxyUseDefaultCredentials] [-SkipNonVersionedFiles] [-Help]
```

Linux または macOS の場合:

```bash
dotnet-install.sh [--channel] [--version] [--jsonfile] [--install-dir] [--architecture]
    [--runtime] [--dry-run] [--no-path] [--verbose] [--azure-feed] [--uncached-feed] [--no-cdn] [--feed-credential]
    [--runtime-id] [--skip-non-versioned-files] [--help]
```

## <a name="description"></a>[説明]

`dotnet-install` スクリプトは、.NET Core CLI や共有ランタイムを含む .NET Core SDK の非管理者インストールを実行するために使用されます。

安定したバージョンのスクリプトを使用することをお勧めします。

- Bash (Linux、macOS): <https://dot.net/v1/dotnet-install.sh>
- PowerShell (Windows): <https://dot.net/v1/dotnet-install.ps1>

これらのスクリプトの主な有用性は、オートメーションのシナリオと管理者以外のインストールにおいてです。 2 つのスクリプトがあります。1 つは Windows 上で動作する PowerShell スクリプトで、もう 1 つは Linux/macOS 上で動作する bash スクリプトです。 スクリプトの動作は両方とも同じです。 bash スクリプトは PowerShell のスイッチも読み取るので、Linux/macOS システムのスクリプトで PowerShell のスイッチを使うことができます。

インストール スクリプトは CLI ビルド ドロップから ZIP/tarball ファイルをダウンロードし、既定の場所または `-InstallDir|--install-dir` で指定された場所へのインストールに進みます。 既定では、インストール スクリプトは SDK をダウンロードしてインストールします。 共有ランタイムの取得だけを行いたい場合は、`-Runtime|--runtime` 引数を指定します。

既定では、スクリプトはインストールの場所を現在のセッションの $PATH に追加します。 `-NoPath|--no-path` 引数を指定することによってこの既定の動作をオーバーライドします。

スクリプトを実行する前に、必要な[依存関係](../install/dependencies.md)をすべてインストールします。

`-Version|--version` 引数を使用して、特定のバージョンをインストールすることができます。 バージョンは 3 つの部分からなるバージョン (`2.1.0` など) を指定する必要があります。 指定しない場合は、`latest` バージョンが使用されます。

## <a name="options"></a>オプション

- **`-Channel|--channel <CHANNEL>`**

  インストールのソース チャネルを指定します。 指定できる値は、

  - `Current` - 最新リリース。
  - `LTS` - 長期的なサポート チャネル (サポートされている最新リリース)。
  - 特定のリリースを表す X.Y 形式の 2 部構成のバージョン (たとえば、`2.1` または `3.0`)。
  - ブランチ名: たとえば、`release/3.1.1xx` または `master` (夜間リリース用) プレビュー チャネルからバージョンをインストールするには、このオプションを使用します。 「[インストーラーとバイナリ](https://github.com/dotnet/core-sdk#installers-and-binaries)」に記載されているチャネルの名前を使用します。

  既定値は `LTS` です。 .NET のサポート チャネルの詳細については、「[.NET Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)」(.NET のサポート ポリシー) ページを参照してください。

- **`-Version|--version <VERSION>`**

  特定のビルド バージョンを表します。 指定できる値は、

  - `latest` - チャネルの最新ビルド (`-Channel` オプションで使用)。
  - `coherent` - チャネルの最新のコヒーレント ビルド。最新の安定版パッケージの組み合わせを使用します (ブランチ名の `-Channel` オプションで使用)。
  - 特定のビルド バージョンを表す X.Y.Z 形式の 3 部構成のバージョン。`-Channel` オプションよりも優先されます。 たとえば、`2.0.0-preview2-006120` のように指定します。

  指定しない場合、`-Version` の既定値は `latest` です。

- **`-JSonFile|--jsonfile <JSONFILE>`**

  SDK バージョンを決定するために使用される [global.json](global-json.md) ファイルへのパスを指定します。 *global.json* ファイルには `sdk:version` の値が必要です。

- **`-InstallDir|--install-dir <DIRECTORY>`**

  インストール パスを指定します。 存在しない場合は、ディレクトリが作成されます。 既定値は *%LocalAppData%\Microsoft\dotnet*です。 バイナリは、このディレクトリに直接配置されます。

- **`-Architecture|--architecture <ARCHITECTURE>`**

  インストールする .NET Core バイナリのアーキテクチャです。 指定できる値は、`<auto>`、`amd64`、`x64`、`x86`、`arm64`、および `arm` です。 既定値は `<auto>` です。これは実行中の OS アーキテクチャを示します。

- **`-SharedRuntime|--shared-runtime`**

  > [!NOTE]
  > このパラメーターは非推奨であり、今後のバージョンのスクリプトでは削除される可能性があります。 別の方法として、`-Runtime|--runtime` オプションを使用することをお勧めします。

  SDK 全体ではなく共有ランタイム ビットのみがインストールされます。 このオプションは、`-Runtime|--runtime dotnet` を指定することと同じです。

- **`-Runtime|--runtime <RUNTIME>`**

  SDK 全体ではなく共有ランタイムのみがインストールされます。 指定できる値は、

  - `dotnet` - `Microsoft.NETCore.App` 共有ランタイム。
  - `aspnetcore` - `Microsoft.AspNetCore.App` 共有ランタイム。
  - `windowsdesktop` - `Microsoft.WindowsDesktop.App` 共有ランタイム。

- **`-DryRun|--dry-run`**

  設定すると、スクリプトでインストールは実行されません。 代わりに、現在要求されているバージョンの .NET Core CLI を一貫してインストールするために使用するコマンド ラインが表示されます。 たとえば、バージョン `latest` を指定すると、そのバージョンのリンクが表示されるので、ビルド スクリプトで確定的にこのコマンドを使用できます。 また、自分でインストールまたはダウンロードしたい場合、バイナリの場所も表示されます。

- **`-NoPath|--no-path`**

  設定すると、インストール フォルダーは現在のセッションのパスにはエクスポートされません。 既定では、スクリプトによって PATH が変更されます。これにより、インストール後すぐに .NET Core CLI を使用できるようになります。

- **`-Verbose|--verbose`**

  診断情報を表示します。

- **`-AzureFeed|--azure-feed`**

  Azure フィードの URL をインストーラーに指定します。 この値は変更しないことをお勧めします。 既定値は `https://dotnetcli.azureedge.net/dotnet` です。

- **`-UncachedFeed|--uncached-feed`**

  このインストーラーで使用されている、キャッシュされていないフィードの URL を変更することを許可します。 この値は変更しないことをお勧めします。

- **`-NoCdn|--no-cdn`**

  [Azure Content Delivery Network (CDN)](https://docs.microsoft.com/azure/cdn/cdn-overview) からのダウンロードを無効にし、キャッシュされていないフィードを直接使用します。

- **`-FeedCredential|--feed-credential`**

  Azure フィードに付加するクエリ文字列として使用されます。 非公開の BLOB ストレージ アカウントを使用するように URL を変更することができます。

- **`--runtime-id`**

  ツールのインストール先の[ランタイム識別子](../rid-catalog.md)を指定します。 Portable Linux には `linux-x64` を使用します。 (Linux または macOS でのみ有効)

- **`-ProxyAddress`**

  設定すると、インストーラーで Web 要求を行うときにプロキシが使われます。 (Windows でのみ有効)

- **`ProxyUseDefaultCredentials`**

  設定すると、プロキシ アドレスの使用時に、インストーラーでは現在のユーザーの資格情報が使用されます。 (Windows でのみ有効)

- **`-SkipNonVersionedFiles|--skip-non-versioned-files`**

  *dotnet.exe* など、バージョン管理されていないファイルが既に存在する場合は、そのインストールをスキップします。

- **`-Help|--help`**

  スクリプトのヘルプを出力します。

## <a name="examples"></a>例

- 最新の長期サポート (LST) バージョンを既定の場所にインストールします。

  Windows の場合:

  ```powershell
  ./dotnet-install.ps1 -Channel LTS
  ```

  macOS/Linux の場合:

  ```bash
  ./dotnet-install.sh --channel LTS
  ```

- 3\.1 チャネルから、最新バージョンを指定した場所にインストールします。

  Windows の場合:

  ```powershell
  ./dotnet-install.ps1 -Channel 3.1 -InstallDir C:\cli
  ```

  macOS/Linux の場合:

  ```bash
  ./dotnet-install.sh --channel 3.1 --install-dir ~/cli
  ```

- 共有ランタイムの 3.0.0 バージョンをインストールします。

  Windows の場合:

  ```powershell
  ./dotnet-install.ps1 -Runtime dotnet -Version 3.0.0
  ```

  macOS/Linux の場合:

  ```bash
  ./dotnet-install.sh --runtime dotnet --version 3.0.0
  ```

- スクリプトを入手し、会社のプロキシの背後に 2.1.2 バージョンをインストールします (Windows のみ)。

  ```powershell
  Invoke-WebRequest 'https://dot.net/v1/dotnet-install.ps1' -Proxy $env:HTTP_PROXY -ProxyUseDefaultCredentials -OutFile 'dotnet-install.ps1';
  ./dotnet-install.ps1 -InstallDir '~/.dotnet' -Version '2.1.2' -ProxyAddress $env:HTTP_PROXY -ProxyUseDefaultCredentials;
  ```

- スクリプトを入手し、.NET Core CLI の 1 行コードのサンプルをインストールします。

  Windows の場合:

  ```powershell
  # Run a separate PowerShell process because the script calls exit, so it will end the current PowerShell session.
  &powershell -NoProfile -ExecutionPolicy unrestricted -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; &([scriptblock]::Create((Invoke-WebRequest -UseBasicParsing 'https://dot.net/v1/dotnet-install.ps1'))) <additional install-script args>"
  ```

  macOS/Linux の場合:

  ```bash
  curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin <additional install-script args>
  ```

## <a name="see-also"></a>参照

- [.NET Core のリリース](https://github.com/dotnet/core/releases)
- [.NET Core ランタイムと SDK ダウンロード アーカイブ](https://github.com/dotnet/core/blob/master/release-notes/download-archive.md)
