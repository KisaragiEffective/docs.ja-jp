---
title: dotnet-trace 診断ツール - .NET CLI
description: dotnet-trace CLI ツールをインストールして使用し、.NET EventPipe を使って、ネイティブ プロファイラーなしで実行中のプロセスの .NET トレースを収集する方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: ae6621631a916f1d9cb5f8982c9e313f1ca2cc09
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740567"
---
# <a name="dotnet-trace-performance-analysis-utility"></a>dotnet-trace パフォーマンス分析ユーティリティ

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

## <a name="install"></a>インストール

`dotnet-trace` をダウンロードしてインストールするには、次の 2 つの方法があります。

- **dotnet グローバル ツール:**

  `dotnet-trace` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-trace)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

  ```dotnetcli
  dotnet tool install --global dotnet-trace
  ```

- **直接ダウンロード:**

  ご利用のプラットフォームに適したツールの実行可能ファイルをダウンロードします。

  | OS  | プラットフォーム |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-trace/win-x86) \| [x64](https://aka.ms/dotnet-trace/win-x64) \| [arm](https://aka.ms/dotnet-trace/win-arm) \| [arm-x64](https://aka.ms/dotnet-trace/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-trace/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-trace/linux-x64) \| [arm](https://aka.ms/dotnet-trace/linux-arm) \| [arm64](https://aka.ms/dotnet-trace/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-trace/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-trace/linux-musl-arm64) |

> [!NOTE]
> x86 アプリ上で `dotnet-trace` を使用するには、対応する x86 バージョンのツールが必要です。

## <a name="synopsis"></a>構文

```console
dotnet-trace [-h, --help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-trace` ツール:

* クロスプラットフォーム .NET Core ツールです。
* ネイティブ プロファイラーなしで実行中のプロセスの .NET Core トレースを回収できます。
* .NET Core ランタイムの [`EventPipe`](./eventpipe.md) に基づいています。
* Windows、Linux または macOS でも同じエクスペリエンスを提供します。

## <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`--version`**

  dotnet-trace ユーティリティのバージョンを表示します。

## <a name="commands"></a>コマンド

| コマンド                                                   |
|-----------------------------------------------------------|
| [dotnet-trace collect](#dotnet-trace-collect)             |
| [dotnet-trace convert](#dotnet-trace-convert)             |
| [dotnet-trace ps](#dotnet-trace-ps)                       |
| [dotnet-trace list-profiles](#dotnet-trace-list-profiles) |

## <a name="dotnet-trace-collect"></a>dotnet-trace collect

実行中のプロセスから診断トレースを収集します。

### <a name="synopsis"></a>構文

```console
dotnet-trace collect [--buffersize <size>] [--clreventlevel <clreventlevel>] [--clrevents <clrevents>]
    [--format <Chromium|NetTrace|Speedscope>] [-h|--help]
    [-n, --name <name>] [--diagnostic-port] [-o|--output <trace-file-path>] [-p|--process-id <pid>]
    [--profile <profile-name>] [--providers <list-of-comma-separated-providers>]
    [-- <command>] (for target applications running .NET 5.0 or later)
```

### <a name="options"></a>オプション

- **`--buffersize <size>`**

  メモリ内の循環バッファーのサイズをメガバイトに設定します。 既定は 256 MB です。

  > [!NOTE]
  > ターゲット プロセスによるイベントの書き込みが頻繁すぎる場合、このバッファーがオーバーフローして、一部のイベントが削除される可能性があります。 削除されるイベントが多すぎる場合は、バッファー サイズを増やし、削除されるイベントの数が減少するかどうかを確認します。 バッファー サイズを大きくしても削除されるイベントの数が減らない場合は、リーダーが遅いために、ターゲット プロセスのバッファーのフラッシュが妨げられている可能性があります。

- **`--clreventlevel <clreventlevel>`**

  生成される CLR イベントの詳細度。

- **`--clrevents <clrevents>`**

  有効にする CLR ランタイム プロバイダーのキーワードの、`+` 記号で区切られたリスト。 これは、16 進値ではなく文字列の別名を使用してイベント キーワードを指定できるようにする、単純なマッピングです。 たとえば、`dotnet-trace collect --providers Microsoft-Windows-DotNETRuntime:3:4` では、`dotnet-trace collect --clrevents gc+gchandle --clreventlevel informational` と同じイベントのセットが要求されます。 次の表に、使用できるキーワードの一覧を示します。

  | キーワードの文字列別名 | キーワードの 16 進値 |
  | ------------ | ------------------- |
  | `gc` | `0x1` |
  | `gchandle` | `0x2` |
  | `fusion` | `0x4` |
  | `loader` | `0x8` |
  | `jit` | `0x10` |
  | `ngen` | `0x20` |
  | `startenumeration` | `0x40` |
  | `endenumeration` | `0x80` |
  | `security` | `0x400` |
  | `appdomainresourcemanagement` | `0x800` |
  | `jittracing` | `0x1000` |
  | `interop` | `0x2000` |
  | `contention` | `0x4000` |
  | `exception` | `0x8000` |
  | `threading` | `0x10000` |
  | `jittedmethodiltonativemap` | `0x20000` |
  | `overrideandsuppressngenevents` | `0x40000` |
  | `type` | `0x80000` |
  | `gcheapdump` | `0x100000` |
  | `gcsampledobjectallocationhigh` | `0x200000` |
  | `gcheapsurvivalandmovement` | `0x400000` |
  | `gcheapcollect` | `0x800000` |
  | `gcheapandtypenames` | `0x1000000` |
  | `gcsampledobjectallocationlow` | `0x2000000` |
  | `perftrack` | `0x20000000` |
  | `stack` | `0x40000000` |
  | `threadtransfer` | `0x80000000` |
  | `debugger` | `0x100000000` |
  | `monitoring` | `0x200000000` |
  | `codesymbols` | `0x400000000` |
  | `eventsource` | `0x800000000` |
  | `compilation` | `0x1000000000` |
  | `compilationdiagnostic` | `0x2000000000` |
  | `methoddiagnostic` | `0x4000000000` |
  | `typediagnostic` | `0x8000000000` |

  CLR プロバイダーの詳細については、[.NET ランタイム プロバイダーのリファレンス ドキュメント](../../fundamentals/diagnostics/runtime-events.md)を参照してください。

- **`--format {Chromium|NetTrace|Speedscope}`**

  トレース ファイルの出力の変換形式を設定します。 既定値は、`NetTrace` です。

- **`-n, --name <name>`**

  トレースを収集するプロセスの名前。

- **`--diagnostic-port <path-to-port>`**

  作成する診断ポートの名前。 このオプションを使用してアプリの起動からトレースを収集する方法については、「[診断ポートを使用してアプリの起動からトレースを収集する](#use-diagnostic-port-to-collect-a-trace-from-app-startup)」を参照してください。

- **`-o|--output <trace-file-path>`**

  収集されたトレース データの出力パス。 指定しない場合の既定値は `trace.nettrace` です。

- **`-p|--process-id <PID>`**

  トレースを収集するプロセス ID。

- **`--profile <profile-name>`**

  共通のトレース シナリオを簡潔に指定できるようにする、事前定義されたプロバイダーの名前付き構成のセット。 次のプロファイルを利用できます。

 | [プロファイル] | 説明 |
 |---------|-------------|
 |`cpu-sampling`|CPU 使用率および一般的な .NET ランタイム情報の追跡に役立ちます。 プロファイルまたはプロバイダーが指定されていない場合は、これが既定のオプションです。|
 |`gc-verbose`|GC コレクションを追跡し、オブジェクトの割り当てをサンプリングします。|
 |`gc-collect`|オーバーヘッドが非常に低い場合にのみ GC コレクションを追跡します。|

- **`--providers <list-of-comma-separated-providers>`**

  有効にする `EventPipe` プロバイダーのコンマ区切りのリスト。 これらのプロバイダーは、`--profile <profile-name>` で示されている任意のプロバイダーを補完します。 特定のプロバイダーに不整合がある場合、この構成がプロファイルの暗黙的な構成に優先されます。

  プロバイダーの一覧の形式は、次のとおりです。

  - `Provider[,Provider]`
  - `Provider` は、`KnownProviderName[:Flags[:Level][:KeyValueArgs]]` という形式です。
  - `KeyValueArgs` は、`[key1=value1][;key2=value2]` という形式です。

  .NET でのいくつかの既知のプロバイダーの詳細については、[既知のイベント プロバイダー](./well-known-event-providers.md)に関するページを参照してください。

- **`-- <command>` (.NET 5.0 のみを実行しているターゲット アプリケーションの場合)**

  ユーザーは、コレクション構成パラメーターの後にまず `--`、次にコマンドを追加すれば、5.0 以降のランタイムで .NET アプリケーションを起動することができます。 これは、起動時のパフォーマンスの問題やアセンブリ ローダーおよびバインダーのエラーなど、プロセスの早い段階で発生する問題を診断するのに役立つ場合があります。

  > [!NOTE]
  > このオプションを使用すると、ツールに返信する最初の .NET 5.0 プロセスが監視されます。つまり、ご利用のコマンドで複数の .NET アプリケーションが起動される場合、収集されるのは最初のアプリのみとなります。 このため、このオプションについては、自己完結型アプリケーションに対して使用するか、または `dotnet exec <app.dll>` オプションを使用することをお勧めします。

> [!NOTE]
> 大きなアプリケーションの場合、トレースの停止に時間がかかることがあります (最大数分)。 ランタイムでは、トレースにキャプチャされたすべてのマネージド コードを対象に、型キャッシュを送信する必要があります。

> [!NOTE]
> Linux と macOS 上でこのコマンドを使用するには、ターゲット アプリケーションと `dotnet-trace` で同じ `TMPDIR` 環境変数が共有されることが前提とされています。 それ以外の場合、このコマンドはタイムアウトします。

> [!NOTE]
> `dotnet-trace` を使用してトレースを収集するには、ターゲット プロセスを実行しているユーザーと同じユーザーとして、またはルートとしてそれを実行する必要があります。 それ以外の場合、このツールでターゲット プロセスとの接続を確立することはできません。

> [!NOTE]
> `[ERROR] System.ComponentModel.Win32Exception (299): A 32 bit processes cannot access modules of a 64 bit process.` のようなエラー メッセージが表示される場合は、ターゲット プロセスとビットが一致しない `dotnet-trace` を使用しようとしています。 「[インストール](#install)」のリンクで、正しいビットのツールをダウンロードしてください。

## <a name="dotnet-trace-convert"></a>dotnet-trace convert

`nettrace` トレースを、別のトレース分析ツールで使用するために、別の形式に変換します。

### <a name="synopsis"></a>構文

```console
dotnet-trace convert [<input-filename>] [--format <Chromium|NetTrace|Speedscope>] [-h|--help] [-o|--output <output-filename>]
```

### <a name="arguments"></a>引数

- **`<input-filename>`**

  変換する入力トレース ファイル。 既定は *trace.nettrace* です。

### <a name="options"></a>オプション

- **`--format <Chromium|NetTrace|Speedscope>`**

  トレース ファイルの出力の変換形式を設定します。

- **`-o|--output <output-filename>`**

  出力ファイルの名前。 ターゲットの形式の拡張子が追加されます。

> [!NOTE]
> `nettrace` ファイルを `chromium` または `speedscope` ファイルに変換した場合、元に戻せません。 `speedscope` ファイルと `chromium` ファイルには、`nettrace` ファイルを再構築するために必要な情報が一部含まれていません。 ただし、`convert` コマンドによって元の `nettrace` ファイルが保持されます。今後開くことがある場合、このファイルは削除しないでください。

## <a name="dotnet-trace-ps"></a>dotnet-trace ps

 トレースを収集できる dotnet プロセスを一覧表示します。

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

## <a name="launch-a-child-application-and-collect-a-trace-from-its-startup-using-dotnet-trace"></a>子アプリケーションを起動し、そのスタートアップからトレースを dotnet-trace を使用して収集します

> [!IMPORTANT]
> これは、.NET 5.0 以降を実行しているアプリに対してのみ機能します。

場合によっては、プロセスのトレースをそのスタートアップから収集すると便利なことがあります。 .NET 5.0 以降を実行しているアプリでは、dotnet-trace を使用してこれを行うことができます。

これを使用すると、コマンドライン引数として `arg1` と `arg2` を含む `hello.exe` が起動され、そのランタイム スタートアップからトレースが収集されます。

```console
dotnet-trace collect -- hello.exe arg1 arg2
```

上のコマンドを実行すると、次のような出力が生成されます。

```console
No profile or providers specified, defaulting to trace profile 'cpu-sampling'

Provider Name                           Keywords            Level               Enabled By
Microsoft-DotNETCore-SampleProfiler     0x0000F00000000000  Informational(4)    --profile
Microsoft-Windows-DotNETRuntime         0x00000014C14FCCBD  Informational(4)    --profile

Process        : E:\temp\gcperfsim\bin\Debug\net5.0\gcperfsim.exe
Output File    : E:\temp\gcperfsim\trace.nettrace


[00:00:00:05]   Recording trace 122.244  (KB)
Press <Enter> or <Ctrl+C> to exit...
```

トレースの収集を停止するには、`<Enter>` または `<Ctrl + C>` キーを押します。 この操作を行うと、`hello.exe` も終了します。

> [!NOTE]
> dotnet-trace を介して `hello.exe` を起動すると、その入力/出力がリダイレクトされ、その stdin/stdout とやりとりができなくなります。
> CTRL + C または SIGTERM を介してツールを終了すると、ツールと子プロセスの両方が安全に終了します。
> ツールの前に子プロセスが終了すると、ツールも終了し、トレースを安全に表示できるようになります。

## <a name="use-diagnostic-port-to-collect-a-trace-from-app-startup"></a>診断ポートを使用してアプリの起動からトレースを収集する

  > [!IMPORTANT]
  > これは、.NET 5.0 以降を実行しているアプリに対してのみ機能します。

診断ポートは、.NET 5 で追加された新しいランタイム機能で、アプリの起動からトレースを開始することができます。 `dotnet-trace` を使用してこれを行うには、上記の例の説明に従って `dotnet-trace collect -- <command>` を使用するか、`--diagnostic-port` オプションを使用します。

起動からすばやくトレースする方法としては、アプリケーションを子プロセスとして起動するために `dotnet-trace <collect|monitor> -- <command>` を使用するのが最も簡単です。

ただし、(アプリを最初の 10 分だけ監視し、実行を継続するなど) トレースしているアプリの有効期間をより細かく制御する場合、または CLI を使用してアプリと対話する必要がある場合は、`--diagnostic-port` オプションを使用すると、監視対象アプリと `dotnet-trace` の両方を制御できます。

1. 次のコマンドにより、`dotnet-trace` で `myport.sock` という名前の診断ソケットを作成し、接続を待機します。

    > ```dotnet-cli
    > dotnet-trace collect --diagnostic-port myport.sock
    > ```

    出力:

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ```

2. 別のコンソールで、`dotnet-trace` の出力値に設定された環境変数 `DOTNET_DiagnosticPorts` を使用して対象アプリケーションを起動します。

    > ```bash
    > export DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ./my-dotnet-app arg1 arg2
    > ```

    これにより、`dotnet-trace` による `my-dotnet-app` のトレースが開始します。

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=myport.sock
    > Starting a counter session. Press Q to quit.
    > ```

    > [!IMPORTANT]
    > `dotnet run` を使用してアプリを起動すると、dotnet CLI によってお使いのアプリのものではない子プロセスが多数生成され、お使いのアプリよりも先にそれらが `dotnet-trace` に接続されてしまい、実行時にお使いのアプリが中断されることで、問題が発生する場合があります。 アプリケーションの起動には、アプリの自己完結型バージョンを直接使用するか、`dotnet exec` を使用することをお勧めします。

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

## <a name="use-rsp-file-to-avoid-typing-long-commands"></a>.rsp ファイルを使用し、長いコマンドの入力を避ける

渡す引数が含まれる `.rsp` ファイルで `dotnet-trace` を起動できます。 これは、文字を取り除くシェル環境を使用しているとき、長い引数を求めるプロバイダーを有効にするときに便利です。

たとえば、次のプロバイダーの場合、トレースのたびに入力するのが面倒です。

```cmd
dotnet-trace collect --providers Microsoft-Diagnostics-DiagnosticSource:0x3:5:FilterAndPayloadSpecs="SqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandBefore@Activity1Start:-Command;Command.CommandText;ConnectionId;Operation;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nSqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandAfter@Activity1Stop:\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuting@Activity2Start:-Command;Command.CommandText;ConnectionId;IsAsync;Command.Connection.ClientConnectionId;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuted@Activity2Stop:",OtherProvider,AnotherProvider
```

また、前の例には、引数の一部として `"` が含まれています。 引用符の処理がシェルごとに異なるため、異なるシェルを使用するとき、さまざまな問題が発生するおそれがあります。 たとえば、`zsh` に入力するコマンドは、`cmd` のコマンドとは異なります。

これを毎回入力する代わりに、`myprofile.rsp` という名称のファイルに次のテキストを保存できます。

```txt
--providers
Microsoft-Diagnostics-DiagnosticSource:0x3:5:FilterAndPayloadSpecs="SqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandBefore@Activity1Start:-Command;Command.CommandText;ConnectionId;Operation;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nSqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandAfter@Activity1Stop:\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuting@Activity2Start:-Command;Command.CommandText;ConnectionId;IsAsync;Command.Connection.ClientConnectionId;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuted@Activity2Stop:",OtherProvider,AnotherProvider
```

`myprofile.rsp` を保存したら、次のコマンドを使用し、この構成で `dotnet-trace` を起動できます。

```bash
dotnet-trace @myprofile.rsp
```

## <a name="see-also"></a>関連項目

- [.NET の既知のイベント プロバイダー](well-known-event-providers.md)
