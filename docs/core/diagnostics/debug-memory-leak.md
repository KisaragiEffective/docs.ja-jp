---
title: メモリ リークのデバッグ チュートリアル
description: .NET Core でメモリ リークをデバッグする方法について説明します。
ms.topic: tutorial
ms.date: 12/17/2019
ms.openlocfilehash: 014945394f87edd02c94f7c3b28043bd07470d8b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76737737"
---
# <a name="tutorial-debug-a-memory-leak-in-net-core"></a>チュートリアル: .NET Core でメモリ リークをデバッグする

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

このチュートリアルでは、.NET Core のメモリ リークを分析するためのツールについて説明します。

このチュートリアルでは、意図的にメモリをリークするように設計されたサンプル アプリを使用します。 このサンプルは演習として提供されています。 意図せずにメモリをリークしているアプリも分析できます。

このチュートリアルでは、次の作業を行います。

> [!div class="checklist"]
>
> - [dotnet-counters](dotnet-counters.md) を使用してマネージド メモリの使用量を確認します。
> - ダンプ ファイルを生成します。
> - ダンプ ファイルを使用してメモリ使用量を分析します。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは次のものを使用します。

- [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core) 以降のバージョン。
- プロセスを一覧表示するための [dotnet-trace](dotnet-trace.md)。
- マネージド メモリ使用量を確認するための [dotnet-counters](dotnet-counters.md)。
- ダンプ ファイルを収集して分析するための [dotnet-dump](dotnet-dump.md)。
- 診断する[サンプル デバッグ ターゲット](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/) アプリ。

このチュートリアルでは、サンプルとツールがインストールされ、使用できる状態であることを前提としています。

## <a name="examine-managed-memory-usage"></a>マネージド メモリ使用量を確認する

このシナリオの根本原因を突き止めるのに役立つ診断データの収集を開始する前に、メモリ リーク (メモリの増加) が実際に発生していることを確認する必要があります。 それを確認するには、[dotnet-counters](dotnet-counters.md) ツールを使用できます。

コンソール ウィンドウを開き、[サンプル デバッグ ターゲット](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/)をダウンロードして解凍したディレクトリに移動します。 ターゲットを実行します。

```dotnetcli
dotnet run
```

別のコンソールから、[dotnet-trace](dotnet-trace.md) ツールを使用してプロセス ID を見つけます。

```console
dotnet-trace ps
```

出力は次のようになります。

```console
4807 DiagnosticScena /home/user/git/samples/core/diagnostics/DiagnosticScenarios/bin/Debug/netcoreapp3.0/DiagnosticScenarios
```

次に、[dotnet-counters](dotnet-counters.md) ツールを使用してマネージド メモリ使用量を確認します。 `--refresh-interval` は、更新間隔を秒数で指定します。

```console
dotnet-counters monitor --refresh-interval 1 -p 4807
```

ライブ出力は次のようになります。

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    # of Assemblies Loaded                           118
    % Time in GC (since last GC)                       0
    Allocation Rate (Bytes / sec)                 37,896
    CPU Usage (%)                                      0
    Exceptions / sec                                   0
    GC Heap Size (MB)                                  4
    Gen 0 GC / sec                                     0
    Gen 0 Size (B)                                     0
    Gen 1 GC / sec                                     0
    Gen 1 Size (B)                                     0
    Gen 2 GC / sec                                     0
    Gen 2 Size (B)                                     0
    LOH Size (B)                                       0
    Monitor Lock Contention Count / sec                0
    Number of Active Timers                            1
    ThreadPool Completed Work Items / sec             10
    ThreadPool Queue Length                            0
    ThreadPool Threads Count                           1
    Working Set (MB)                                  83
```

次の行に焦点を当てます。

```console
    GC Heap Size (MB)                                  4
```

スタートアップの直後、マネージド ヒープ メモリは 4 MB であることがわかります。

次に、URL `http://localhost:5000/api/diagscenario/memleak/20000` を指定します。

メモリ使用量が 30 MB に増加していることに注意してください。

```console
    GC Heap Size (MB)                                 30
```

メモリ使用量を監視することにより、メモリが増加していること、またはリークしていることを確実に知ることができます。 次の手順では、メモリ分析に適切なデータを収集します。

### <a name="generate-memory-dump"></a>メモリ ダンプを生成する

メモリ リークの可能性について分析するときは、アプリのメモリ ヒープにアクセスする必要があります。 その後、メモリーの内容を分析できます。 オブジェクト間の関係を確認して、メモリが解放されない理由についての理論を作成します。 一般的な診断データのソースは、Windows 上のメモリ ダンプ、または Linux 上の同等のコア ダンプです。 .NET Core アプリケーションのダンプを生成するには、[dotnet-dump](dotnet-dump.md) ツールを使用できます。

前に開始した[サンプル デバッグ ターゲット](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/)を使用して、次のコマンドを実行し、Linux コア ダンプを生成します。

```dotnetcli
dotnet-dump collect -p 4807
```

結果として、同じフォルダーにコア ダンプが生成されます。

```console
Writing minidump with heap to ./core_20190430_185145
Complete
```

### <a name="restart-the-failed-process"></a>失敗したプロセスを再起動する

ダンプが収集されると、失敗したプロセスを診断するのに十分な情報が得られます。 失敗したプロセスが運用サーバーで実行されている場合は、今が、そのプロセスを再起動して短期的な修復を行うのに最適なタイミングです。

このチュートリアルでは、[サンプル デバッグ ターゲット](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/)を終了したので、閉じることができます。 サーバーを起動したターミナルに移動して、`Control-C` を押します。

### <a name="analyze-the-core-dump"></a>コア ダンプを分析する

コア ダンプが生成されたので、[dotnet-dump](dotnet-dump.md) ツールを使用してダンプを分析します。

```dotnetcli
dotnet-dump analyze core_20190430_185145
```

ここで、`core_20190430_185145` は、分析するコア ダンプの名前です。

> [!NOTE]
> *libdl.so* が見つからないことを示すエラーが表示された場合は、*libc6-dev* パッケージのインストールが必要な可能性があります。 詳細については、「[Linux における .NET Core の前提条件](../linux-prerequisites.md)」を参照してください。

SOS コマンドを入力できるプロンプトが表示されます。 一般的に、最初に調べる必要があるのは、マネージド ヒープの全体的な状態です。

```console
> dumpheap -stat

Statistics:
              MT    Count    TotalSize Class Name
...
00007f6c1eeefba8      576        59904 System.Reflection.RuntimeMethodInfo
00007f6c1dc021c8     1749        95696 System.SByte[]
00000000008c9db0     3847       116080      Free
00007f6c1e784a18      175       128640 System.Char[]
00007f6c1dbf5510      217       133504 System.Object[]
00007f6c1dc014c0      467       416464 System.Byte[]
00007f6c21625038        6      4063376 testwebapi.Controllers.Customer[]
00007f6c20a67498   200000      4800000 testwebapi.Controllers.Customer
00007f6c1dc00f90   206770     19494060 System.String
Total 428516 objects
```

ここで、ほとんどのオブジェクトが `String` または `Customer` オブジェクトであることがわかります。

メソッド テーブル (MT) を指定して `dumpheap` コマンドを再び使用すると、すべての `String` インスタンスの一覧を取得できます。

```console
> dumpheap -mt 00007faddaa50f90

         Address               MT     Size
...
00007f6ad09421f8 00007faddaa50f90       94
...
00007f6ad0965b20 00007f6c1dc00f90       80
00007f6ad0965c10 00007f6c1dc00f90       80
00007f6ad0965d00 00007f6c1dc00f90       80
00007f6ad0965df0 00007f6c1dc00f90       80
00007f6ad0965ee0 00007f6c1dc00f90       80

Statistics:
              MT    Count    TotalSize Class Name
00007f6c1dc00f90   206770     19494060 System.String
Total 206770 objects
```

次に、`System.String` インスタンスで `gcroot` コマンドを使用して、このオブジェクトがルート指定されている方法と理由を確認します。 このコマンドは 30 MB のヒープで数分かかるため、しばらくお待ちください。

```console
> gcroot -all 00007f6ad09421f8

Thread 3f68:
    00007F6795BB58A0 00007F6C1D7D0745 System.Diagnostics.Tracing.CounterGroup.PollForValues() [/_/src/System.Private.CoreLib/shared/System/Diagnostics/Tracing/CounterGroup.cs @ 260]
        rbx:  (interior)
            ->  00007F6BDFFFF038 System.Object[]
            ->  00007F69D0033570 testwebapi.Controllers.Processor
            ->  00007F69D0033588 testwebapi.Controllers.CustomerCache
            ->  00007F69D00335A0 System.Collections.Generic.List`1[[testwebapi.Controllers.Customer, DiagnosticScenarios]]
            ->  00007F6C000148A0 testwebapi.Controllers.Customer[]
            ->  00007F6AD0942258 testwebapi.Controllers.Customer
            ->  00007F6AD09421F8 System.String

HandleTable:
    00007F6C98BB15F8 (pinned handle)
    -> 00007F6BDFFFF038 System.Object[]
    -> 00007F69D0033570 testwebapi.Controllers.Processor
    -> 00007F69D0033588 testwebapi.Controllers.CustomerCache
    -> 00007F69D00335A0 System.Collections.Generic.List`1[[testwebapi.Controllers.Customer, DiagnosticScenarios]]
    -> 00007F6C000148A0 testwebapi.Controllers.Customer[]
    -> 00007F6AD0942258 testwebapi.Controllers.Customer
    -> 00007F6AD09421F8 System.String

Found 2 roots.
```

`String` が `Customer` オブジェクトによって直接保持され、`CustomerCache` オブジェクトによって間接的に保持されていることがわかります。

オブジェクトのダンプを続けて、ほとんどの `String` オブジェクトが同様のパターンに従っていることを確認できます。 この時点で、コードの根本原因を特定するのに十分な情報が調査によって提供されています。

この一般的な手順では、主要なメモリ リークの原因を特定できます。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このチュートリアルでは、サンプル Web サーバーを開始しました。 このサーバーは、「[失敗したプロセスを再起動する](#restart-the-failed-process)」セクションの説明に従ってシャットダウンされている必要があります。

また、作成されたダンプ ファイルを削除することもできます。

## <a name="next-steps"></a>次の手順

これでこのチュートリアルは完了です。

引き続き診断チュートリアルを公開します。 ドラフト バージョンは、[dotnet/diagnostics](https://github.com/dotnet/diagnostics/tree/master/documentation/tutorial) リポジトリで読むことができます。

このチュートリアルでは、主要な .NET 診断ツールの基本について説明しました。 高度な使用方法については、次の参照ドキュメントをご覧ください。

* プロセスを一覧表示するための [dotnet-trace](dotnet-trace.md)。
* マネージド メモリ使用量を確認するための [dotnet-counters](dotnet-counters.md)。
* ダンプ ファイルを収集して分析するための [dotnet-dump](dotnet-dump.md)。
