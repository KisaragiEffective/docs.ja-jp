---
title: dotnet-sos 診断ツール - .NET CLI
description: dotnet-sos CLI ツールをインストールして使用し、Windows および Linux のネイティブ デバッガーで使われる SOS デバッガー拡張機能を管理する方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: 83458ac0933034886dee51e0258acaeef9a8ed89
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740593"
---
# <a name="sos-installer-dotnet-sos"></a>SOS インストーラー (dotnet-sos)

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="install"></a>インストール

`dotnet-sos` をダウンロードしてインストールするには、次の 2 つの方法があります。

- **dotnet グローバル ツール:**

  `dotnet-sos` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-sos)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

  ```dotnetcli
  dotnet tool install --global dotnet-sos
  ```

- **直接ダウンロード:**

  ご利用のプラットフォームに適したツールの実行可能ファイルをダウンロードします。

  | OS  | プラットフォーム |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-sos/win-x86) \| [x64](https://aka.ms/dotnet-sos/win-x64) \| [arm](https://aka.ms/dotnet-sos/win-arm) \| [arm-x64](https://aka.ms/dotnet-sos/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-sos/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-sos/linux-x64) \| [arm](https://aka.ms/dotnet-sos/linux-arm) \| [arm64](https://aka.ms/dotnet-sos/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-sos/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-sos/linux-musl-arm64) |

## <a name="synopsis"></a>構文

```console
dotnet-sos [-h|--help] [options] [command]]
```

## <a name="description"></a>説明

`dotnet-sos` グローバル ツールによって [SOS デバッガー拡張機能](sos-debugging-extension.md)が追加されます。 この拡張機能では、lldb や windbg のようなネイティブ デバッガーからマネージド .NET Core の状態を検査できます。

> [!NOTE]
> `dotnet-sos` ツールから SOS をインストールすることは、Linux または macOS の場合のみ必要です。  古いデバッグ ツールを使用している場合、Windows でも必要になることがあります。 [Windows Debugger](/windows-hardware/drivers/debugger/debugger-download-tools) (>= WinDbg または cdb のバージョン 10.0.18317.1001) の最近のバージョンでは、Microsoft 拡張機能ギャラリーから自動的に SOS がロードされます。  

## <a name="options"></a>オプション

- **`--version`**

  バージョン情報を表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## <a name="dotnet-sos-install"></a>dotnet-sos install

.NET Core プロセスをデバッグするために、[SOS 拡張機能](sos-debugging-extension.md)をローカルにインストールします。 macOS および Linux では、lldb の起動時に拡張機能が自動的に読み込まれるように、 *.lldbinit* ファイルが更新されます。 古いデバッグ ツール (バージョン 10.0.18317.1001 より前) を使用して Windows に SOS をインストールする場合は、WinDbg または cdb で `.load %USERPROFILE%\.dotnet\sos\sos.dll` を実行して、デバッガーで拡張機能を手動で読み込む必要があります。

### <a name="synopsis"></a>概要

```console
dotnet-sos install [--architecture <arch>]
```

### <a name="options"></a>オプション

- **`--architecture <arch>`**

  インストールする SOS バイナリのプロセッサ アーキテクチャを指定します。 既定では、`dotnet-sos` によってホスト コンピューターのアーキテクチャがインストールされます。 dotnet ホスト アーキテクチャとは異なるアーキテクチャに対して SOS をインストールするときにこのオプションを使用します。 たとえば、Arm64 ホストから Arm32 バイナリを実行している場合、`dotnet-sos install --architecture Arm` で SOS をインストールする必要があります。

  次のアーキテクチャを利用できます。

  - `Arm`
  - `Arm64`
  - `X86`
  - `X64`

## <a name="dotnet-sos-uninstall"></a>dotnet-sos uninstall

[SOS 拡張機能](sos-debugging-extension.md)をアンインストールします。Linux と macOS の場合、lldb 構成から削除します。

### <a name="synopsis"></a>構文

```console
dotnet-sos uninstall
```
