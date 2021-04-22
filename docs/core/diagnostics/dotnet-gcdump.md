---
title: dotnet-gcdump 診断ツール - .NET CLI
description: dotnet-gcdump CLI ツールをインストールして使用し、.NET EventPipe を使ってライブ .NET プロセスの GC (ガベージ コレクター) ダンプを収集する方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: 71f0ff3ee39ae89d56a271d3cc6dd9624681885c
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740788"
---
# <a name="heap-analysis-tool-dotnet-gcdump"></a>ヒープ分析ツール (dotnet-gcdump)

**この記事の対象:** ✔️ .NET Core 3.1 SDK 以降のバージョン

## <a name="install"></a>インストール

`dotnet-gcdump` をダウンロードしてインストールするには、次の 2 つの方法があります。

- **dotnet グローバル ツール:**

  `dotnet-gcdump` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-gcdump)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

  ```dotnetcli
  dotnet tool install --global dotnet-gcdump
  ```

- **直接ダウンロード:**

  ご利用のプラットフォームに適したツールの実行可能ファイルをダウンロードします。

  | OS  | プラットフォーム |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-gcdump/win-x86) \| [x64](https://aka.ms/dotnet-gcdump/win-x64) \| [arm](https://aka.ms/dotnet-gcdump/win-arm) \| [arm-x64](https://aka.ms/dotnet-gcdump/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-gcdump/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-gcdump/linux-x64) \| [arm](https://aka.ms/dotnet-gcdump/linux-arm) \| [arm64](https://aka.ms/dotnet-gcdump/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-gcdump/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-gcdump/linux-musl-arm64) |

> [!NOTE]
> x86 アプリ上で `dotnet-gcdump` を使用するには、対応する x86 バージョンのツールが必要です。

## <a name="synopsis"></a>構文

```console
dotnet-gcdump [-h|--help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-gcdump` グローバル ツールで [EventPipe](./eventpipe.md) を使用して、ライブ .NET プロセスの GC (ガベージ コレクター) ダンプを収集します。 GC ダンプを作成するには、ターゲット プロセスで GC をトリガーし、特殊なイベントを有効にし、イベント ストリームからオブジェクト ルートのグラフを再生成します。 このプロセスにより、プロセスの実行中のオーバーヘッドを最小限に抑えながら GC ダンプを収集できます。 このようなダンプは、いくつかのシナリオで役立ちます。

- 複数の時点でヒープ上にあるオブジェクト数を比較する。
- オブジェクトのルートを分析する ("この種類に対する参照がまだ残っているものはあるか" などの質問に回答する場合)。
- ヒープ上のオブジェクト数に関する一般的な統計情報を収集する。

### <a name="view-the-gc-dump-captured-from-dotnet-gcdump"></a>dotnet-gcdump からキャプチャされた GC ダンプを表示する

Windows では、分析のために [PerfView](https://github.com/microsoft/perfview) で、または Visual Studio で `.gcdump` ファイルを表示できます。 現在、Windows 以外のプラットフォームで `.gcdump` を開く方法はありません。

複数の `.gcdump` を収集し、それらを Visual Studio で同時に開いて、比較を行うことができます。

## <a name="options"></a>Options

- **`--version`**

  `dotnet-gcdump` ユーティリティのバージョンを表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## `dotnet-gcdump collect`

現在実行中のプロセスから GC ダンプを収集します。

> [!WARNING]
> GC ヒープをウォークするために、このコマンドによって第 2 世代の (完全な) ガベージ コレクションがトリガーされます。これにより、特に GC ヒープが大きい場合に、ランタイムが長時間中断されるおそれがあります。 GC ヒープが大きい場合、パフォーマンスを重視する環境ではこのコマンドを使用しないでください。

### <a name="synopsis"></a>構文

```console
dotnet-gcdump collect [-h|--help] [-p|--process-id <pid>] [-o|--output <gcdump-file-path>] [-v|--verbose] [-t|--timeout <timeout>] [-n|--name <name>]
```

### <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`-p|--process-id <pid>`**

  GC ダンプを収集するプロセス ID。

- **`-o|--output <gcdump-file-path>`**

  収集された GC ダンプが書き込まれるパス。 既定値は *.\\YYYYMMDD\_HHMMSS\_\<pid>.gcdump* です。

- **`-v|--verbose`**

  GC ダンプの収集時にログを出力します。

- **`-t|--timeout <timeout>`**

  この秒数より長くかかる場合は、GC ダンプの収集をあきらめてください。 既定値は 30 です。

- **`-n|--name <name>`**

  GC ダンプを収集するプロセスの名前。

> [!NOTE]
> Linux と macOS 上でこのコマンドを使用するには、ターゲット アプリケーションと `dotnet-gcdump` で同じ `TMPDIR` 環境変数が共有されることが前提とされています。 それ以外の場合、このコマンドはタイムアウトします。

> [!NOTE]
> `dotnet-gcdump` を使用して GC ダンプを収集するには、ターゲット プロセスを実行しているユーザーと同じユーザーとして、またはルートとしてそれを実行する必要があります。 それ以外の場合、このツールでターゲット プロセスとの接続を確立することはできません。

## `dotnet-gcdump ps`

GC ダンプを収集できる dotnet プロセスを一覧表示します。

### <a name="synopsis"></a>概要

```console
dotnet-gcdump ps
```

## `dotnet-gcdump report <gcdump_filename>`

以前に生成された GC ダンプまたは実行中のプロセスからレポートを生成し、`stdout` に書き込みます。

### <a name="synopsis"></a>構文

```console
dotnet-gcdump report [-h|--help] [-p|--process-id <pid>] [-t|--report-type <HeapStat>]
```

### <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`-p|--process-id <pid>`**

  GC ダンプを収集するプロセス ID。

- **`-t|--report-type <HeapStat>`**

  生成するレポートの種類。 使用可能なオプション: heapstat (既定値)。

## <a name="troubleshoot"></a>トラブルシューティング

- gcdump に型情報はありません。

   .NET Core 3.1 より前のバージョンでは、EventPipe を使って呼び出されたときに、gcdump 間で型キャッシュがクリアされない問題がありました。 これにより、型情報を判別するために必要なイベントが 2 番目以降の gcdump に対して送信されなくなりました。 これは .NET Core 3.1-preview2 で修正されました。

- COM 型と静的な型は GC ダンプに含まれていません。

   .NET Core 3.1-preview2 より前のバージョンでは、EventPipe を介して GC ダンプが呼び出されたときに静的な型と COM 型が送信されない問題がありました。 これは .NET Core 3.1-preview2 で修正されました。
