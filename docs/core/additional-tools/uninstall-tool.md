---
title: アンインストール ツール
description: .NET CORE アンインストール ツールの概要です。これは、.NET Core SKD とランタイムの制御されたクリーンアップを可能にするガイド付きツールです。
author: sfoslund
ms.date: 01/06/2020
ms.openlocfilehash: 4944c983cbd02b456c3a09a1b03bc28ba6e458cc
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75714552"
---
# <a name="net-core-uninstall-tool"></a>.NET Core アンインストール ツール

[.NET Core アンインストール ツール](https://github.com/dotnet/cli-lab/releases) (`dotnet-core-uninstall`) を使用すると、.NET Core SDK とランタイムをシステムから削除できます。 一連のオプションを使用して、アンインストールするバージョンを指定できます。

このツールでは、Windows と macOS がサポートされています。 Linux は現在サポートされていません。

Windows では、ツールは次のいずれかのインストーラーを使用してインストールされた SDK とランタイムのみをアンインストールできます。

- .NET Core SDK およびランタイム インストーラー。
- Visual Studio 2019 バージョン 16.3 より前のバージョンの Visual Studio インストーラー。

macOS では、このツールでアンインストールできるのは */usr/local/share/dotnet* フォルダーにある SDK とランタイムのみです。

これらの制限のため、このツールでは、コンピューター上の一部の .NET Core SDK とランタイムをアンインストールできない可能性があります。 `dotnet --info` コマンドを使用して、インストールされているすべての .NET Core SDK とランタイム (このツールで削除できない SDK やランタイムを含む) を検出できます。 `dotnet-core-uninstall list` コマンドを実行すると、このツールでアンインストールできる SDK が表示されます。

## <a name="install-the-tool"></a>ツールをインストールする

.NET Core アンインストール ツールは、[dotnet/cli-lab](https://github.com/dotnet/cli-lab/releases) GitHub リポジトリからダウンロードできます。

> [!NOTE]
> このツールでは、.NET Core SDK とランタイムをアンインストールするために昇格が必要です。 そのため、Windows では *C:\Program Files*、macOS では */usr/local/bin* などの書き込み保護されたディレクトリにインストールする必要があります。 「[dotnet コマンドの特権アクセス](../tools/elevated-access.md)」も参照してください。 詳細なインストール手順については、[GitHub リリース ページ](https://github.com/dotnet/cli-lab/releases)をご覧ください。

## <a name="run-the-tool"></a>ツールを実行します。

次の手順は、アンインストール ツールを実行するための推奨方法を示しています。

- [手順 1 - インストールされている .NET Core SDK とランタイムを表示する](#step-1---display-installed-net-core-sdks-and-runtimes)
- [手順 2 - ドライ ランを実行する](#step-2---do-a-dry-run)
- [手順 3 - .NET Core SDK とランタイムをアンインストールする](#step-3---uninstall-net-core-sdks-and-runtimes)
- [手順 4 - NuGet フォールバック フォルダーを削除する (省略可能)](#step-4---delete-the-nuget-fallback-folder-optional)

### <a name="step-1---display-installed-net-core-sdks-and-runtimes"></a>手順 1 - インストールされている .NET Core SDK とランタイムを表示する

`dotnet-core-uninstall list` コマンドを実行すると、このツールで削除できる、インストールされている .NET Core SDK とランタイムが一覧表示されます。 一部の SDK とランタイムは、Visual Studio で必要な場合があり、それらのアンインストールが推奨されない理由を示す注釈と共に表示されます。

**dotnet-core-uninstall list**

#### <a name="synopsis"></a>構文

```console
dotnet-core-uninstall list [options]
```

#### <a name="options"></a>オプション

## <a name="windows"></a>[Windows](#tab/windows)

* **`--aspnet-runtime`**

  このツールでアンインストールできるすべての ASP.NET Core ランタイムを一覧表示します。

* **`--hosting-bundle`**

  このツールでアンインストールできるすべての .NET Core ランタイムとホスティングのバンドルを一覧表示します。

* **`--runtime`**

  このツールでアンインストールできるすべての .NET Core ランタイムを一覧表示します。

* **`--sdk`**

  このツールでアンインストールできるすべての .NET Core SDK を一覧表示します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。

* **`--x64`**

  このツールでアンインストールできるすべての x64 .NET Core SDK とランタイムを一覧表示します。

* **`--x86`**

  このツールでアンインストールできるすべての x86 .NET Core SDK とランタイムを一覧表示します。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--runtime`**

  このツールでアンインストールできるすべての .NET Core ランタイムを一覧表示します。

* **`--sdk`**

  このツールでアンインストールできるすべての .NET Core SDK を一覧表示します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。
  
---

#### <a name="examples"></a>使用例

* このツールで削除できるすべての .NET Core SDK とランタイムを一覧表示します。

  ```console
  dotnet-core-uninstall list
  ```

* すべての x64 .NET Core SDK とランタイムを一覧表示します。

  ```console
  dotnet-core-uninstall list --x64
  ```

* すべての x86 .NET Core SDK を一覧表示します。

  ```console
  dotnet-core-uninstall list --sdk --x86
  ```

### <a name="step-2---do-a-dry-run"></a>手順 2 - ドライ ランを実行する

`dotnet-core-uninstall dry-run` および `dotnet-core-uninstall whatif` コマンドは、アンインストールを実行せずに指定されたオプションに基づいて削除される .NET Core SDK とランタイムを表示します。 これらのコマンドはシノニムです。

**dotnet-core-uninstall dry-run and dotnet-core-uninstall whatif**

#### <a name="synopsis"></a>構文

```console
dotnet-core-uninstall dry-run [options] [<VERSION>...]

dotnet-core-uninstall whatif [options] [<VERSION>...]
```

#### <a name="arguments"></a>引数

* **`VERSION`**

  アンインストールする指定されたバージョン。 スペースで区切って、複数のバージョンを順々に指定できます。 応答ファイルもサポートされています。

  > [!TIP]
  > すべてのバージョンをコマンド ラインに指定する代わりに応答ファイルを使用できます。
  > それらはテキスト ファイルで、通常、\*.rsp 拡張子が付いています。各バージョンは別々の行に指定されます。
  > `VERSION` 引数の応答ファイルを指定するには、\@ 文字を使用し、そのすぐ後に応答ファイル名を指定します。

#### <a name="options"></a>オプション

## <a name="windows"></a>[Windows](#tab/windows)

* **`--all`**

  すべての .NET Core SDK とランタイムを削除します。

* **`--all-below <VERSION>`**

  指定したバージョンよりも小さいバージョンの .NET Core SDK とランタイムのみを削除します。 指定したバージョンはインストールされたままになります。

* **`--all-but <VERSIONS>`**

  指定したバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-but-latest`**

  最上位の 1 つのバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-lower-patches`**

  上位の修正プログラムによって置き換えられた .NET Core SDK とランタイムを削除します。 このオプションは、global. json を保護します。

* **`--all-previews`**

  プレビューとしてマークされている .NET Core SDK とランタイムを削除します。

* **`--all-previews-but-latest`**

  最上位の 1 つのプレビューを除き、プレビューとしてマークされているすべての .NET Core SDK とランタイムを削除します。

* **`--aspnet-runtime`**

  ASP.NET Core ランタイムのみを削除します。

* **`--hosting-bundle`**

  .NET Core ランタイムとホスティングのバンドルのみを削除します。

* **`--major-minor <MAJOR_MINOR>`**

  指定した `major.minor` バージョンに一致する .NET Core SDK とランタイムを削除します。

* **`--runtime`**

  .NET Core ランタイムのみを削除します。

* **`--sdk`**

  .NET Core SDK のみを削除します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。

* **`--x64`**

  x64 SDK またはランタイムを削除するには、`--sdk`、`--runtime`、および `--aspnet-runtime` と共に使用する必要があります。

* **`--x86`**

  x86 SDK またはランタイムを削除するには、`--sdk`、`--runtime`、および `--aspnet-runtime` と共に使用する必要があります。

* **`--force`** Visual Studio によって使用される可能性があるバージョンを強制的に削除します。

メモ:

1. `--sdk`、`--runtime`、`--aspnet-runtime`、および `--hosting-bundle` の 1 つだけが必要です。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor`、および `[<VERSION>...]` は排他的です。
3. `--x64` または `--x86` が指定されていない場合は、x64 と x86 の両方が削除されます。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--all`**

  すべての .NET Core SDK とランタイムを削除します。

* **`--all-below <VERSION>`**

  指定したバージョンより下の .NET Core SDK とランタイムを削除します。 指定したバージョンはそのまま残ります。

* **`--all-but <VERSIONS>`**

  指定したバージョンを除き、.NET Core SDK とランタイムを削除します。

* **`--all-but-latest`**

  最上位の 1 つのバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-lower-patches`**

  上位の修正プログラムによって置き換えられた .NET Core SDK とランタイムを削除します。 このオプションは、global. json を保護します。

* **`--all-previews`**

  プレビューとしてマークされている .NET Core SDK とランタイムを削除します。

* **`--all-previews-but-latest`**

  最上位の 1 つのプレビューを除き、プレビューとしてマークされているすべての .NET Core SDK とランタイムを削除します。

* **`--major-minor <MAJOR_MINOR>`**

  指定した `major.minor` バージョンに一致する .NET Core SDK とランタイムを削除します。

* **`--runtime`**

  .NET Core ランタイムのみを削除します。

* **`--sdk`**

  .NET Core SDK のみを削除します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。
  
* **`--force`** Visual Studio または SDK によって使用される可能性があるバージョンを強制的に削除します。

メモ:

1. `--sdk` と `--runtime` のうち 1 つだけが必要です。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor`、および `[<VERSION>...]` は排他的です。

---

#### <a name="examples"></a>使用例

> [!NOTE]
> 既定で、Visual Studio またはその他の SDK に必要な可能性がある .NET Core SDK とランタイムは `dotnet-core-uninstall dry-run` 出力には含まれません。 次の例では、コンピューターの状態によっては、指定された SDK とランタイムの一部が出力に含まれない可能性があります。 すべての SDK とランタイムを含めるには、それらを引数として明示的に指定するか、`--force` オプションを使用してください。

* 上位の修正プログラムによって置き換えられたすべての .NET Core ランタイムを削除するドライ ラン。

  ```console
  dotnet-core-uninstall dry-run --all-lower-patches --runtime
  ```

* バージョン `2.2.301` より下のすべての .NET Core SDK を削除するドライ ラン。

  ```console
  dotnet-core-uninstall whatif --all-below 2.2.301 --sdk
  ```

### <a name="step-3---uninstall-net-core-sdks-and-runtimes"></a>手順 3 - .NET Core SDK とランタイムをアンインストールする

`dotnet-core-uninstall remove` は、一連のオプションによって指定された .NET Core SDK とランタイムをアンインストールします。 このツールを使用して、バージョン 5.0 以上の SDK とランタイムをアンインストールすることはできません。

このツールには破壊的動作があるため、remove コマンドを実行する前に、ドライ ランを実行することを**強く**お勧めします。 ドライランにより、`remove` コマンドを使用したときに削除される .NET Core SDK とランタイムが示されます。 削除しても安全な SDK とランタイムを確認するには、「[バージョンを削除する必要はあるか](../versions/remove-runtime-sdk-versions.md#should-i-remove-a-version)」を参照してください。

> [!CAUTION]
> 次の注意事項に留意してください。
>
>- このツールは、コンピューター上の `global.json` ファイルに必要な .NET Core SDK のバージョンをアンインストールできます。 .NET Core SDK は、「[.NET Core のダウンロード](https://dotnet.microsoft.com/download/dotnet-core)」ページから再インストールできます。
>- このツールは、コンピューター上のフレームワークに依存するアプリケーションに必要な .NET Core ランタイムのバージョンをアンインストールできます。 .NET Core ランタイムは、「[.NET Core のダウンロード](https://dotnet.microsoft.com/download/dotnet-core)」ページから再インストールできます。
>- このツールは、Visual Studio が依存する .NET Core SDK とランタイムのバージョンをアンインストールできます。 Visual Studio のインストールを中断した場合は、Visual Studio インストーラーで [修復] を実行すると稼働状態に戻ります。

既定で、すべてのコマンドは、Visual Studio またはその他の SDK に必要な可能性がある .NET Core SDK とランタイムを保持します。 これらの SDK とランタイムは、引数として明示的に指定するか、`--force` オプションを使用することによってアンインストールできます。

このツールでは、.NET Core SDK とランタイムをアンインストールするために昇格が必要です。 Windows では管理者コマンド プロンプトで、macOS では `sudo` を使用してツールを実行してください。 `dry-run` および `whatif` コマンドには、昇格は必要ありません。

**dotnet-core-uninstall remove**

#### <a name="synopsis"></a>構文

```console
dotnet-core-uninstall remove [options] [<VERSION>...]
```

#### <a name="arguments"></a>引数

* **`VERSION`**

  アンインストールする指定されたバージョン。 スペースで区切って、複数のバージョンを順々に指定できます。 応答ファイルもサポートされています。

  > [!TIP]
  > すべてのバージョンをコマンド ラインに指定する代わりに応答ファイルを使用できます。
  > それらはテキスト ファイルで、通常、\*.rsp 拡張子が付いています。各バージョンは別々の行に指定されます。
  > `VERSION` 引数の応答ファイルを指定するには、\@ 文字を使用し、そのすぐ後に応答ファイル名を指定します。

#### <a name="options"></a>オプション

## <a name="windows"></a>[Windows](#tab/windows)

* **`--all`**

  すべての .NET Core SDK とランタイムを削除します。

* **`--all-below <VERSION>`**

  指定したバージョンよりも小さいバージョンの .NET Core SDK とランタイムのみを削除します。 指定したバージョンはインストールされたままになります。

* **`--all-but <VERSIONS>`**

  指定したバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-but-latest`**

  最上位の 1 つのバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-lower-patches`**

  上位の修正プログラムによって置き換えられた .NET Core SDK とランタイムを削除します。 このオプションは、global. json を保護します。

* **`--all-previews`**

  プレビューとしてマークされている .NET Core SDK とランタイムを削除します。

* **`--all-previews-but-latest`**

  最上位の 1 つのプレビューを除き、プレビューとしてマークされているすべての .NET Core SDK とランタイムを削除します。

* **`--aspnet-runtime`**

  ASP.NET Core ランタイムのみを削除します。

* **`--hosting-bundle`**

  .NET Core ランタイムとホスティングのバンドルのみを削除します。

* **`--major-minor <MAJOR_MINOR>`**

  指定した `major.minor` バージョンに一致する .NET Core SDK とランタイムを削除します。

* **`--runtime`**

  .NET Core ランタイムのみを削除します。

* **`--sdk`**

  .NET Core SDK のみを削除します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。

* **`--x64`**

  x64 SDK またはランタイムを削除するには、`--sdk`、`--runtime`、および `--aspnet-runtime` と共に使用する必要があります。

* **`--x86`**

  x86 SDK またはランタイムを削除するには、`--sdk`、`--runtime`、および `--aspnet-runtime` と共に使用する必要があります。

* **`-y, --yes`** Yes または No の確認を要求せずにコマンドを実行します。

* **`--force`** Visual Studio によって使用される可能性があるバージョンを強制的に削除します。

メモ:

1. `--sdk`、`--runtime`、`--aspnet-runtime`、および `--hosting-bundle` の 1 つだけが必要です。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor`、および `[<VERSION>...]` は排他的です。
3. `--x64` または `--x86` が指定されていない場合は、x64 と x86 の両方が削除されます。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--all`**

  すべての .NET Core SDK とランタイムを削除します。

* **`--all-below <VERSION>`**

  指定したバージョンより下の .NET Core SDK とランタイムを削除します。 指定したバージョンはそのまま残ります。

* **`--all-but <VERSIONS>`**

  指定したバージョンを除き、.NET Core SDK とランタイムを削除します。

* **`--all-but-latest`**

  最上位の 1 つのバージョンを除く、すべての .NET Core SDK とランタイムを削除します。

* **`--all-lower-patches`**

  上位の修正プログラムによって置き換えられた .NET Core SDK とランタイムを削除します。 このオプションは、global. json を保護します。

* **`--all-previews`**

  プレビューとしてマークされている .NET Core SDK とランタイムを削除します。

* **`--all-previews-but-latest`**

  最上位の 1 つのプレビューを除き、プレビューとしてマークされているすべての .NET Core SDK とランタイムを削除します。

* **`--major-minor <MAJOR_MINOR>`**

  指定した `major.minor` バージョンに一致する .NET Core SDK とランタイムを削除します。

* **`--runtime`**

  .NET Core ランタイムのみを削除します。

* **`--sdk`**

  .NET Core SDK のみを削除します。

* **`-v, --verbosity <LEVEL>`**

  詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。 既定値は `normal` です。

* **`-y, --yes`** Y/N の確認を要求せずにコマンドを実行します。
  
* **`--force`** Visual Studio または SDK によって使用される可能性があるバージョンを強制的に削除します。

メモ:

1. `--sdk` と `--runtime` のうち 1 つだけが必要です。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor`、および `[<VERSION>...]` は排他的です。

---

#### <a name="examples"></a>使用例

> [!NOTE]
> 既定で、Visual Studio またはその他の SDK に必要な可能性がある .NET Core SDK とランタイムは保持されます。 次の例では、コンピューターの状態によっては、指定された SDK とランタイムの一部が保持される可能性があります。 すべての SDK とランタイムを削除するには、それらを引数として明示的に指定するか、`--force` オプションを使用してください。

* Y/N の確認を要求せずに、バージョン `3.0.0-preview6-27804-01` を除くすべての .NET Core ランタイムを削除します。

  ```console
  dotnet-core-uninstall remove --all-but 3.0.0-preview6-27804-01 --runtime --yes
  ```

* Y/N の確認を要求せずに、すべての .NET Core 1.1 SDK を削除します。

  ```console
  dotnet-core-uninstall remove --sdk --major-minor 1.1 -y
  ```

* コンソールに出力せずに、.NET Core 1.1.11 SDK を削除します。

  ```console
  dotnet-core-uninstall remove 1.1.11 --sdk --yes --verbosity q
  ```

* このツールで安全に削除できるすべての .NET Core SDK を削除します。

  ```console
  dotnet-core-uninstall remove --all --sdk
  ```

* Visual Studio で必要な可能性がある SDK を含め、このツールで削除できるすべての .NET Core SDK を削除します (推奨されません)。

  ```console
  dotnet-core-uninstall remove --all --sdk --force
  ```

* 応答ファイル `versions.rsp` に指定されたすべての .NET Core SDK を削除します。

  ```console
  dotnet-core-uninstall remove --sdk @versions.rsp
  ```

  *versions.rsp* の内容は次のとおりです。
  
  ```text
  2.2.300
  2.1.700
  ```

### <a name="step-4---delete-the-nuget-fallback-folder-optional"></a>手順 4 - NuGet フォールバック フォルダーを削除する (省略可能)

場合によっては、`NuGetFallbackFolder` が不要になり、削除することもできます。 このフォルダーの削除の詳細については、[NuGetFallbackFolder の削除](../versions/remove-runtime-sdk-versions.md#remove-the-nuget-fallback-folder)に関する記事を参照してください。

## <a name="uninstall-the-tool"></a>ツールをアンインストールする

## <a name="windows"></a>[Windows](#tab/windows)

1. **[プログラムの追加と削除]** を開きます。
2. `Microsoft .NET Core SDK Uninstall Tool` を検索します。
3. **[アンインストール]** を選択します。

## <a name="macos"></a>[macOS](#tab/macos)

ダウンロードした *dotnet-core-uninstall.tar.gz* ファイルをインストールしたディレクトリから削除します。 このファイルの内容を別のディレクトリに解凍した場合は、必ずその内容も削除してください。

---
