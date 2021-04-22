---
title: dotnet-counters 診断ツール - .NET CLI
description: dotnet-counter CLI ツールをインストールし、アドホックな正常性監視と最初のレベルのパフォーマンス調査に使用する方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: 436db72677186b5fcc8b560451b69e616870f66e
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740801"
---
# <a name="investigate-performance-counters-dotnet-counters"></a>パフォーマンス カウンターを調べる (dotnet-counters)

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

## <a name="install"></a>インストール

`dotnet-counters` をダウンロードしてインストールするには、次の 2 つの方法があります。

- **dotnet グローバル ツール:**

  `dotnet-counters` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-counters)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

  ```dotnetcli
  dotnet tool install --global dotnet-counters
  ```

- **直接ダウンロード:**

  ご利用のプラットフォームに適したツールの実行可能ファイルをダウンロードします。

  | OS  | プラットフォーム |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-counters/win-x86) \| [x64](https://aka.ms/dotnet-counters/win-x64) \| [arm](https://aka.ms/dotnet-counters/win-arm) \| [arm-x64](https://aka.ms/dotnet-counters/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-counters/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-counters/linux-x64) \| [arm](https://aka.ms/dotnet-counters/linux-arm) \| [arm64](https://aka.ms/dotnet-counters/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-counters/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-counters/linux-musl-arm64) |

> [!NOTE]
> x86 アプリ上で `dotnet-counters` を使用するには、対応する x86 バージョンのツールが必要です。

## <a name="synopsis"></a>構文

```console
dotnet-counters [-h|--help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-counters` は、アドホックな正常性監視と第 1 レベルのパフォーマンス調査のためのパフォーマンス監視ツールです。 <xref:System.Diagnostics.Tracing.EventCounter> API を使用して公開されたパフォーマンス カウンターの値を監視できます。 たとえば、CPU 使用率や .NET Core アプリケーションでスローされる例外の発生率などを迅速に監視して、`PerfView` または `dotnet-trace` を使用した、より重大なパフォーマンス調査を開始する前に疑わしいものがあるかどうかを確認できます。

## <a name="options"></a>オプション

- **`--version`**

  dotnet-counters ユーティリティのバージョンを表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## <a name="commands"></a>コマンド

| コマンド                                             |
|-----------------------------------------------------|
| [dotnet-counters collect](#dotnet-counters-collect) |
| [dotnet-counters list](#dotnet-counters-list)       |
| [dotnet-counters monitor](#dotnet-counters-monitor) |
| [dotnet-counters ps](#dotnet-counters-ps)           |

## <a name="dotnet-counters-collect"></a>dotnet-counters collect

選択されたカウンター値を定期的に収集し、後処理用に指定されたファイル形式にエクスポートします。

### <a name="synopsis"></a>構文

```console
dotnet-counters collect [-h|--help] [-p|--process-id] [-n|--name] [--diagnostic-port] [--refresh-interval] [--counters <COUNTERS>] [--format] [-o|--output] [-- <command>]
```

### <a name="options"></a>オプション

- **`-p|--process-id <PID>`**

  カウンター データを収集するプロセスの ID。

- **`-n|--name <name>`**

  カウンター データを収集するプロセスの名前。

- **`--diagnostic-port`**

  作成する診断ポートの名前。 このオプションを使用し、アプリの起動からカウンターの監視を開始する方法については、「[診断ポートの使用](#using-diagnostic-port)」を参照してください。

- **`--refresh-interval <SECONDS>`**

  表示されているカウンターを更新するまでの遅延時間 (秒数)

- **`--counters <COUNTERS>`**

  カウンターのコンマ区切りのリスト。 カウンターは `provider_name[:counter_name]` で指定できます。 `provider_name` をカウンターの限定リストを指定せずに使用した場合、そのプロバイダーのすべてのカウンターが表示されます。 プロバイダーとカウンターの名前を検出するには、[dotnet-counters list](#dotnet-counters-list) コマンドを使用します。

- **`--format <csv|json>`**

  エクスポートされる形式。 現在使用可能: csv、json。

- **`-o|--output <output>`**

  出力ファイルの名前。

- **`-- <command>` (対象アプリケーションが .NET 5.0 以降を実行している場合のみ)**

  ユーザーは、コレクション構成パラメーターの後に `--` を付加し、5.0 以降のランタイムで .NET アプリケーションを起動するコマンドを続けて指定できます。 `dotnet-counters` を使用して、指定したコマンドでプロセスを起動し、要求したメトリックを収集します。 これは、アプリケーションの起動パスのメトリックを収集するのに役立つことが多く、メイン エントリポイントの前または直後に発生する問題を診断または監視するために使用できます。

  > [!NOTE]
  > このオプションを使用して、ツールに応答する最初の .NET 5.0 プロセスを監視します。つまり、コマンドから複数の .NET アプリケーションを起動する場合、最初のアプリのみ収集されます。 このため、このオプションについては、自己完結型アプリケーションに対して使用するか、または `dotnet exec <app.dll>` オプションを使用することをお勧めします。

  > [!NOTE]
  > dotnet-counters を使用して .NET 実行可能ファイルを起動すると、その入力/出力がリダイレクトされ、その stdin/stdout とやりとりができなくなります。 CTRL + C または SIGTERM を介してツールを終了すると、ツールと子プロセスの両方が安全に終了します。 ツールの前に子プロセスが終了すると、ツールも終了し、トレースを安全に表示できるようになります。 stdin/stdout を使用する必要がある場合は、`--diagnostic-port` オプションを使用します。 詳細については、「[診断ポートの使用](#using-diagnostic-port)」を参照してください。

> [!NOTE]
> Linux と macOS 上でこのコマンドを使用するには、ターゲット アプリケーションと `dotnet-counters` で同じ `TMPDIR` 環境変数が共有されることが前提とされています。 それ以外の場合、このコマンドはタイムアウトします。

> [!NOTE]
> `dotnet-counters` を使用してメトリックを収集するには、ターゲット プロセスを実行しているユーザーと同じユーザーとして、またはルートとしてそれを実行する必要があります。 それ以外の場合、このツールでターゲット プロセスとの接続を確立することはできません。

### <a name="examples"></a>使用例

- すべてのカウンターを更新間隔 3 秒で収集し、csv を出力として生成します。

  ```console
  > dotnet-counters collect --process-id 1902 --refresh-interval 3 --format csv

  counter_list is unspecified. Monitoring all counters by default.
  Starting a counter session. Press Q to quit.
  ```

- 子プロセスとして `dotnet mvc.dll` を開始し、起動時からランタイム カウンターと ASP.NET Core ホスティング カウンターの収集を開始し、JSON 出力として保存します。

  ```console
  > dotnet-counters collect --format json --counters System.Runtime,Microsoft.AspNetCore.Hosting -- dotnet mvc.dll
  Starting a counter session. Press Q to quit.
  File saved to counter.json
  ```

## <a name="dotnet-counters-list"></a>dotnet-counters list

カウンターの名前と説明の一覧をプロバイダー別にグループ化して表示します。

### <a name="synopsis"></a>構文

```console
dotnet-counters list [-h|--help]
```

### <a name="example"></a>例

```console
> dotnet-counters list
Showing well-known counters only. Specific processes may support additional counters.

System.Runtime
    cpu-usage                                    Amount of time the process has utilized the CPU (ms)
    working-set                                  Amount of working set used by the process (MB)
    gc-heap-size                                 Total heap size reported by the GC (MB)
    gen-0-gc-count                               Number of Gen 0 GCs per interval
    gen-1-gc-count                               Number of Gen 1 GCs per interval
    gen-2-gc-count                               Number of Gen 2 GCs per interval
    time-in-gc                                   % time in GC since the last GC
    gen-0-size                                   Gen 0 Heap Size
    gen-1-size                                   Gen 1 Heap Size
    gen-2-size                                   Gen 2 Heap Size
    loh-size                                     LOH Heap Size
    alloc-rate                                   Allocation Rate
    assembly-count                               Number of Assemblies Loaded
    exception-count                              Number of Exceptions per interval
    threadpool-thread-count                      Number of ThreadPool Threads
    monitor-lock-contention-count                Monitor Lock Contention Count
    threadpool-queue-length                      ThreadPool Work Items Queue Length
    threadpool-completed-items-count             ThreadPool Completed Work Items Count
    active-timer-count                           Active Timers Count

Microsoft.AspNetCore.Hosting
    requests-per-second                  Request rate
    total-requests                       Total number of requests
    current-requests                     Current number of requests
    failed-requests                      Failed number of requests
```

> [!NOTE]
> `Microsoft.AspNetCore.Hosting` カウンターは、そのようなカウンターをサポートするプロセスが特定されたときに表示されます。たとえば、ホスト マシンで ASP.NET Core アプリケーションが実行されているときです。

## <a name="dotnet-counters-monitor"></a>dotnet-counters monitor

選択したカウンターの定期的に更新される値を表示します。

### <a name="synopsis"></a>構文

```console
dotnet-counters monitor [-h|--help] [-p|--process-id] [-n|--name] [--diagnostic-port] [--refresh-interval] [--counters] [-- <command>]
```

### <a name="options"></a>オプション

- **`-p|--process-id <PID>`**

  監視するプロセスの ID。

- **`-n|--name <name>`**

  監視するプロセスの名前。

- **`--diagnostic-port`**

  作成する診断ポートの名前。 このオプションを使用し、アプリの起動からカウンターの監視を開始する方法については、「[診断ポートの使用](#using-diagnostic-port)」を参照してください。

- **`--refresh-interval <SECONDS>`**

  表示されているカウンターを更新するまでの遅延時間 (秒数)

- **`--counters <COUNTERS>`**

  カウンターのコンマ区切りのリスト。 カウンターは `provider_name[:counter_name]` で指定できます。 `provider_name` をカウンターの限定リストを指定せずに使用した場合、そのプロバイダーのすべてのカウンターが表示されます。 プロバイダーとカウンターの名前を検出するには、[dotnet-counters list](#dotnet-counters-list) コマンドを使用します。

 **`-- <command>` (対象アプリケーションが .NET 5.0 以降を実行している場合のみ)**

  ユーザーは、コレクション構成パラメーターの後に `--` を付加し、5.0 以降のランタイムで .NET アプリケーションを起動するコマンドを続けて指定できます。 `dotnet-counters` を使用して、指定したコマンドでプロセスを起動し、要求したメトリックを監視します。 これは、アプリケーションの起動パスのメトリックを収集するのに役立つことが多く、メイン エントリポイントの前または直後に発生する問題を診断または監視するために使用できます。

  > [!NOTE]
  > このオプションを使用して、ツールに応答する最初の .NET 5.0 プロセスを監視します。つまり、コマンドから複数の .NET アプリケーションを起動する場合、最初のアプリのみ収集されます。 このため、このオプションについては、自己完結型アプリケーションに対して使用するか、または `dotnet exec <app.dll>` オプションを使用することをお勧めします。

  > [!NOTE]
  > dotnet-counters を使用して .NET 実行可能ファイルを起動すると、その入力/出力がリダイレクトされ、その stdin/stdout とやりとりができなくなります。 CTRL + C または SIGTERM を介してツールを終了すると、ツールと子プロセスの両方が安全に終了します。 ツールの前に子プロセスが終了すると、ツールも終了します。 stdin/stdout を使用する必要がある場合は、`--diagnostic-port` オプションを使用します。 詳細については、「[診断ポートの使用](#using-diagnostic-port)」を参照してください。

> [!NOTE]
> Linux と macOS 上でこのコマンドを使用するには、ターゲット アプリケーションと `dotnet-counters` で同じ `TMPDIR` 環境変数が共有されることが前提とされています。

> [!NOTE]
> `dotnet-counters` を使用してメトリックを監視するには、ターゲット プロセスを実行しているユーザーと同じユーザーとして、またはルートとしてそれを実行する必要があります。

> [!NOTE]
> `[ERROR] System.ComponentModel.Win32Exception (299): A 32 bit processes cannot access modules of a 64 bit process.` のようなエラー メッセージが表示される場合は、ターゲット プロセスとビットが一致しない `dotnet-counters` を使用しようとしています。 「[インストール](#install)」のリンクで、正しいビットのツールをダウンロードしてください。

### <a name="examples"></a>例

- 3 秒の更新間隔で `System.Runtime` のすべてのカウンターを監視します。

  ```console
  > dotnet-counters monitor --process-id 1902  --refresh-interval 3 --counters System.Runtime
  Press p to pause, r to resume, q to quit.
      Status: Running

  [System.Runtime]
      % Time in GC since last GC (%)                                 0
      Allocation Rate (B / 1 sec)                                5,376
      CPU Usage (%)                                                  0
      Exception Count (Count / 1 sec)                                0
      GC Fragmentation (%)                                          48.467
      GC Heap Size (MB)                                              0
      Gen 0 GC Count (Count / 1 sec)                                 1
      Gen 0 Size (B)                                                24
      Gen 1 GC Count (Count / 1 sec)                                 1
      Gen 1 Size (B)                                                24
      Gen 2 GC Count (Count / 1 sec)                                 1
      Gen 2 Size (B)                                           272,000
      IL Bytes Jitted (B)                                       19,449
      LOH Size (B)                                              19,640
      Monitor Lock Contention Count (Count / 1 sec)                  0
      Number of Active Timers                                        0
      Number of Assemblies Loaded                                    7
      Number of Methods Jitted                                     166
      POH (Pinned Object Heap) Size (B)                             24
      ThreadPool Completed Work Item Count (Count / 1 sec)           0
      ThreadPool Queue Length                                        0
      ThreadPool Thread Count                                        2
      Working Set (MB)                                              19
  ```

- `System.Runtime` から CPU 使用率と GC ヒープ サイズのみを監視します。

  ```console
  > dotnet-counters monitor --process-id 1902 --counters System.Runtime[cpu-usage,gc-heap-size]

  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      CPU Usage (%)                                 24
      GC Heap Size (MB)                            811
  ```

- ユーザー定義の `EventSource` の `EventCounter` 値を監視します。 詳しくは、「[チュートリアル: .NET Core で EventCounters を使用してパフォーマンスを測定する](event-counter-perf.md)」を参照してください。

  ```console
  > dotnet-counters monitor --process-id 1902 --counters Samples-EventCounterDemos-Minimal

  Press p to pause, r to resume, q to quit.
      request                                      100
  ```

- `dotnet-counters` で利用できるすべての既知のカウンターを表示する:

  ```console
  > dotnet-counters list

  Showing well-known counters for .NET (Core) version 3.1 only. Specific processes may support additional counters.
  System.Runtime
      cpu-usage                          The percent of process' CPU usage relative to all of the system CPU resources [0-100]
      working-set                        Amount of working set used by the process (MB)
      gc-heap-size                       Total heap size reported by the GC (MB)
      gen-0-gc-count                     Number of Gen 0 GCs between update intervals
      gen-1-gc-count                     Number of Gen 1 GCs between update intervals
      gen-2-gc-count                     Number of Gen 2 GCs between update intervals
      time-in-gc                         % time in GC since the last GC
      gen-0-size                         Gen 0 Heap Size
      gen-1-size                         Gen 1 Heap Size
      gen-2-size                         Gen 2 Heap Size
      loh-size                           LOH Size
      alloc-rate                         Number of bytes allocated in the managed heap between update intervals
      assembly-count                     Number of Assemblies Loaded
      exception-count                    Number of Exceptions / sec
      threadpool-thread-count            Number of ThreadPool Threads
      monitor-lock-contention-count      Number of times there were contention when trying to take the monitor lock between update intervals
      threadpool-queue-length            ThreadPool Work Items Queue Length
      threadpool-completed-items-count   ThreadPool Completed Work Items Count
      active-timer-count                 Number of timers that are currently active

  Microsoft.AspNetCore.Hosting
      requests-per-second                Number of requests between update intervals
      total-requests                     Total number of requests
      current-requests                   Current number of requests
      failed-requests                    Failed number of requests
  ```

- .NET 5 アプリに対して `dotnet-counters` で利用できるすべての既知のカウンターを表示する:

  ```console
  > dotnet-counters list --runtime-version 5.0

  Showing well-known counters for .NET (Core) version 5.0 only. Specific processes may support additional counters.
  System.Runtime
      cpu-usage                          The percent of process' CPU usage relative to all of the system CPU resources [0-100]
      working-set                        Amount of working set used by the process (MB)
      gc-heap-size                       Total heap size reported by the GC (MB)
      gen-0-gc-count                     Number of Gen 0 GCs between update intervals
      gen-1-gc-count                     Number of Gen 1 GCs between update intervals
      gen-2-gc-count                     Number of Gen 2 GCs between update intervals
      time-in-gc                         % time in GC since the last GC
      gen-0-size                         Gen 0 Heap Size
      gen-1-size                         Gen 1 Heap Size
      gen-2-size                         Gen 2 Heap Size
      loh-size                           LOH Size
      poh-size                           POH (Pinned Object Heap) Size
      alloc-rate                         Number of bytes allocated in the managed heap between update intervals
      gc-fragmentation                   GC Heap Fragmentation
      assembly-count                     Number of Assemblies Loaded
      exception-count                    Number of Exceptions / sec
      threadpool-thread-count            Number of ThreadPool Threads
      monitor-lock-contention-count      Number of times there were contention when trying to take the monitor lock between update intervals
      threadpool-queue-length            ThreadPool Work Items Queue Length
      threadpool-completed-items-count   ThreadPool Completed Work Items Count
      active-timer-count                 Number of timers that are currently active
      il-bytes-jitted                    Total IL bytes jitted
      methods-jitted-count               Number of methods jitted

  Microsoft.AspNetCore.Hosting
      requests-per-second   Number of requests between update intervals
      total-requests        Total number of requests
      current-requests      Current number of requests
      failed-requests       Failed number of requests

  Microsoft-AspNetCore-Server-Kestrel
      connections-per-second      Number of connections between update intervals
      total-connections           Total Connections
      tls-handshakes-per-second   Number of TLS Handshakes made between update intervals
      total-tls-handshakes        Total number of TLS handshakes made
      current-tls-handshakes      Number of currently active TLS handshakes
      failed-tls-handshakes       Total number of failed TLS handshakes
      current-connections         Number of current connections
      connection-queue-length     Length of Kestrel Connection Queue
      request-queue-length        Length total HTTP request queue

  System.Net.Http
      requests-started        Total Requests Started
      requests-started-rate   Number of Requests Started between update intervals
      requests-aborted        Total Requests Aborted
      requests-aborted-rate   Number of Requests Aborted between update intervals
      current-requests        Current Requests
  ```

- `my-aspnet-server.exe` を起動し、起動時から読み込まれるアセンブリの数を監視します (.NET 5.0 以降のみ)。

  > [!IMPORTANT]
  > これは、.NET 5.0 以降を実行しているアプリに対してのみ機能します。

  ```console
  > dotnet-counters monitor --counters System.Runtime[assembly-count] -- my-aspnet-server.exe

  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      Number of Assemblies Loaded                   24
  ```
  
- コマンドライン引数に `arg1` と `arg2` を指定して `my-aspnet-server.exe` を起動し、その動作セットと起動時の GC ヒープ サイズを監視します (.NET 5.0 以降のみ)。

  > [!IMPORTANT]
  > これは、.NET 5.0 以降を実行しているアプリに対してのみ機能します。

  ```console
  > dotnet-counters monitor --counters System.Runtime[working-set,gc-heap-size] -- my-aspnet-server.exe arg1 arg2
  ```

  ```console
  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      GC Heap Size (MB)                                 39
      Working Set (MB)                                  59
  ```

## <a name="dotnet-counters-ps"></a>dotnet-counters ps

監視できる dotnet プロセスの一覧を表示します。

### <a name="synopsis"></a>概要

```console
dotnet-counters ps [-h|--help]
```

### <a name="example"></a>例

```console
> dotnet-counters ps
  
  15683 WebApi     /home/user/repos/WebApi/WebApi
  16324 dotnet     /usr/local/share/dotnet/dotnet
```

## <a name="using-diagnostic-port"></a>診断ポートの使用

  > [!IMPORTANT]
  > これは、.NET 5.0 以降を実行しているアプリに対してのみ機能します。

診断ポートは、監視やカウンターの収集をアプリの起動から開始できる、.NET 5 で追加された新しいランタイム機能です。 `dotnet-counters` を使用してこれを行うには、上記の例の説明に従って `dotnet-counters <collect|monitor> -- <command>` を使用するか、`--diagnostic-port` オプションを使用します。

起動からすぐにアプリケーションを監視するには、`dotnet-counters <collect|monitor> -- <command>` を使用して子プロセスとしてアプリケーションを起動するのが最も簡単です。

ただし、(アプリを最初の 10 分だけ監視し、実行を継続するなど) 監視しているアプリの有効期間をより細かく制御する場合、または CLI を使用してアプリと対話する必要がある場合は、`--diagnostic-port` オプションを使用すると、監視対象アプリと `dotnet-counters` の両方を制御できます。

1. 次のコマンドにより、dotnet-counters は `myport.sock` という名前の診断ソケットを作成し、接続を待機します。

    > ```dotnet-cli
    > dotnet-counters collect --diagnostic-port myport.sock
    > ```

    出力:

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ```

2. 別のコンソールで、`dotnet-counters` の出力値に設定された環境変数 `DOTNET_DiagnosticPorts` を使用して対象アプリケーションを起動します。

    > ```bash
    > export DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ./my-dotnet-app arg1 arg2
    > ```

    これにより、`dotnet-counters` が `my-dotnet-app` でカウンターを収集し始めます。

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=myport.sock
    > Starting a counter session. Press Q to quit.
    > ```

    > [!IMPORTANT]
    > `dotnet run` を使用してアプリを起動すると、dotnet CLI によってお使いのアプリのものではない子プロセスが多数生成され、お使いのアプリに先立ってそれらが `dotnet-counters` に接続されてしまい、実行時にお使いのアプリが中断されることで、問題が発生する場合があります。 アプリケーションの起動には、アプリの自己完結型バージョンを直接使用するか、`dotnet exec` を使用することをお勧めします。
