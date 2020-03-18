---
title: dotnet-trace ツール - .NET Core
description: dotnet-trace コマンドライン ツールのインストールおよび使用。
ms.date: 11/21/2019
ms.openlocfilehash: b19b159636fbf57fa2d461b398fcf9234aab491c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76737644"
---
# <a name="dotnet-trace-performance-analysis-utility"></a>dotnet-trace パフォーマンス分析ユーティリティ

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

## <a name="install-dotnet-trace"></a>dotnet-trace をインストールする

[dotnet tool install](../tools/dotnet-tool-install.md) コマンドで `dotnet-trace` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-trace)をインストールします。

```dotnetcli
dotnet tool install --global dotnet-trace
```

## <a name="synopsis"></a>構文

```console
dotnet-trace [-h, --help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-trace` ツール:

* クロスプラットフォーム .NET Core ツールです。
* ネイティブ プロファイラーなしで実行中のプロセスの .NET Core トレースを回収できます。
* .NET Core ランタイムのクロスプラットフォームの `EventPipe` テクノロジを中心に構築されています。
* Windows、Linux または macOS でも同じエクスペリエンスを提供します。

## <a name="options"></a>オプション

- **`--version`**

  dotnet-counters ユーティリティのバージョンを表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## <a name="commands"></a>コマンド

| コマンド                                                     |
| ----------------------------------------------------------- |
| [dotnet-trace collect](#dotnet-trace-collect)               |
| [dotnet-trace convert](#dotnet-trace-convert)               |
| [dotnet-trace ps](#dotnet-trace-ps) |
| [dotnet-trace list-profiles](#dotnet-trace-list-profiles)   |

## <a name="dotnet-trace-collect"></a>dotnet-trace collect

実行中のプロセスから診断トレースを収集します。

### <a name="synopsis"></a>構文

```console
dotnet-trace collect [-h|--help] [-p|--process-id] [--buffersize <size>] [-o|--output]
    [--providers] [--profile <profile-name>] [--format]
```

### <a name="options"></a>オプション

- **`-p|--process-id <PID>`**

  トレースを収集するプロセス。

- **`--buffersize <size>`**

  メモリ内の循環バッファーのサイズをメガバイトに設定します。 既定は 256 MB です。

- **`-o|--output <trace-file-path>`**

  収集されたトレース データの出力パス。 指定しない場合の既定は、`trace.nettrace` です。

- **`--providers <list-of-comma-separated-providers>`**

  有効にする `EventPipe` プロバイダーのコンマ区切りのリスト。 これらのプロバイダーは、`--profile <profile-name>` で示されている任意のプロバイダーを補完します。 特定のプロバイダーに不整合がある場合、この構成がプロファイルの暗黙的な構成に優先されます。

  プロバイダーの一覧の形式は、次のとおりです。

  - `Provider[,Provider]`
  - `Provider` は、`KnownProviderName[:Flags[:Level][:KeyValueArgs]]` という形式です。
  - `KeyValueArgs` は、`[key1=value1][;key2=value2]` という形式です。

- **`--profile <profile-name>`**

  共通のトレース シナリオを簡潔に指定できるようにする、事前定義されたプロバイダーの名前付き構成のセット。

- **`--format {NetTrace|Speedscope}`**

  トレース ファイルの出力の変換形式を設定します。 既定値は、`NetTrace` です。

## <a name="dotnet-trace-convert"></a>dotnet-trace convert

`nettrace` トレースを、別のトレース分析ツールで使用するために、別の形式に変換します。

### <a name="synopsis"></a>構文

```console
dotnet-trace convert [<input-filename>] [-h|--help] [--format] [-o|--output]
```

### <a name="arguments"></a>引数

- **`<input-filename>`**

  変換する入力トレース ファイル。 既定は *trace.nettrace* です。

### <a name="options"></a>オプション

- **`--format <NetTrace|Speedscope>`**

  トレース ファイルの出力の変換形式を設定します。

- **`-o|--output <output-filename>`**

  出力ファイルの名前。 ターゲットの形式の拡張子が追加されます。

## <a name="dotnet-trace-ps"></a>dotnet-trace ps

接続できる dotnet プロセスの一覧を示します。

### <a name="synopsis"></a>構文

```console
dotnet-trace ps [-h|--help]
```

## <a name="dotnet-trace-list-profiles"></a>dotnet-trace list-profiles

各プロファイルに含まれるプロバイダーとフィルターの記述と共に、事前に構築されているトレースのプロファイルを一覧表示します。

### <a name="synopsis"></a>構文

```console
dotnet-trace list-profiles [-h|--help]
```

## <a name="collect-a-trace-with-dotnet-trace"></a>dotnet-trace を使用してトレースを収集する

`dotnet-trace` を使用してトレースを収集するには:

- トレースを収集する .NET Core アプリケーションのプロセス識別子 (PID) を取得します。

  - Windows で、タスク マネージャーや、たとえば、`tasklist` コマンドを使用できます。
  - Linux の場合、たとえば、`ps` コマンドを使用できます。
  - [dotnet-trace ps](#dotnet-trace-ps)

- 次のコマンドを実行します。

  ```console
  dotnet-trace collect --process-id <PID>
  ```

  上のコマンドを実行すると、次のような出力が生成されます。

  ```console
  Press <Enter> to exit...
  Connecting to process: <Full-Path-To-Process-Being-Profiled>/dotnet.exe
  Collecting to file: <Full-Path-To-Trace>/trace.nettrace
  Session Id: <SessionId>
  Recording trace 721.025 (KB)
  ```

- `<Enter>` キーを押すと、コレクションが停止します。 `dotnet-trace` では、*trace.nettrace* ファイルにイベントをログ記録する作業が完了します。

## <a name="view-the-trace-captured-from-dotnet-trace"></a>dotnet-trace からキャプチャされたトレースを表示する

Windows の場合、 *.nettrace* ファイルを [PerfView](https://github.com/microsoft/perfview) で表示し、分析できます。他のプラットフォームで収集されたトレースについては、トレース ファイルを Windows コンピューターに移動し、PerfView で表示できます。

Linux の場合、`dotnet-trace` の出力形式を `speedscope` に変更することでトレースを表示できます。 出力ファイル形式は `-f|--format` オプションで変更できます。`-f speedscope` により、`dotnet-trace` で `speedscope` ファイルが生成されます。 `nettrace` (既定のオプション) か `speedscope` を選択できます。 `Speedscope` ファイルは <https://www.speedscope.app> で開くことができます。

> [!NOTE]
> .NET Core ランタイムにより、`nettrace` 形式でトレースが生成されます。 トレースの完了後、トレースは speedscope に変換されます (指定されている場合)。 変換によってはデータが失われる場合もあるため、元の `nettrace` ファイルが変換されたファイルの横に保持されます。

## <a name="use-dotnet-trace-to-collect-counter-values-over-time"></a>dotnet-trace を使用して経時的なカウンター値を収集する

`dotnet-trace` でできること:

* パフォーマンスで左右される環境で基本的な正常性監視を行うには `EventCounter` を使用します。 たとえば、運用環境です。
* リアルタイムで表示する必要がないようにトレースを収集します。

たとえば、実行時のパフォーマンス カウンター値を収集するには、次のコマンドを使用します。

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1
```

先のコマンドでは、正常性の簡易監視を 1 秒ごとに報告するようにランタイム カウンターに命令します。 `EventCounterIntervalSec=1` の値を (60 など) 大きい値に置き換えた場合、カウンター データの細分性の詳細度がより低いトレースをより少数収集できるようになります。

次のコマンドでは、先のコマンドよりオーバーヘッドとトレース サイズが少なくなります。

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1,Microsoft-Windows-DotNETRuntime:0:1,Microsoft-DotNETCore-SampleProfiler:0:1
```

上のコマンドでは、ランタイム イベントとマネージド スタック プロファイラーが無効になります。

## <a name="net-providers"></a>.NET プロバイダー

.NET Core ランタイムでは、次の .NET プロバイダーがサポートされています。 .NET Core では、`Event Tracing for Windows (ETW)` と `EventPipe` トレースの有効化に、いずれも同じキーワードを使用しています。

| プロバイダー名                            | 情報 |
|------------------------------------------|-------------|
| `Microsoft-Windows-DotNETRuntime`        | [ランタイム プロバイダー](../../framework/performance/clr-etw-providers.md#the-runtime-provider)<br>[CLR ランタイム キーワード](../../framework/performance/clr-etw-keywords-and-levels.md#runtime) |
| `Microsoft-Windows-DotNETRuntimeRundown` | [ランダウン プロバイダー](../../framework/performance/clr-etw-providers.md#the-rundown-provider)<br>[CLR ランダウン キーワード](../../framework/performance/clr-etw-keywords-and-levels.md#rundown) |
| `Microsoft-DotNETCore-SampleProfiler`    | サンプル プロファイラーを有効にします。 |
