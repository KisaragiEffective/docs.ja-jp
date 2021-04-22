---
title: dotnet-dump 診断ツール - .NET CLI
description: dotnet-dump CLI ツールをインストールして使用し、ネイティブ デバッガーなしで Windows と Linux のダンプを収集して分析する方法について学習します。
ms.date: 11/17/2020
ms.topic: reference
ms.openlocfilehash: 57149c9330d6057ea57a32bfa6582a73ed6b6dee
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740606"
---
# <a name="dump-collection-and-analysis-utility-dotnet-dump"></a>ダンプの収集と分析のユーティリティ (dotnet-dump)

**この記事の対象:** ✔️ .NET Core 3.0 SDK 以降のバージョン

> [!NOTE]
> macOS の `dotnet-dump` は、.NET 5.0 以降のバージョンでのみサポートされています。

## <a name="install"></a>インストール

`dotnet-dump` をダウンロードしてインストールするには、次の 2 つの方法があります。

- **dotnet グローバル ツール:**

  `dotnet-dump` [NuGet パッケージ](https://www.nuget.org/packages/dotnet-dump)の最新のリリース バージョンをインストールするには、次のように [dotnet tool install](../tools/dotnet-tool-install.md) コマンドを使用します。

  ```dotnetcli
  dotnet tool install --global dotnet-dump
  ```

- **直接ダウンロード:**

  ご利用のプラットフォームに適したツールの実行可能ファイルをダウンロードします。

  | OS  | プラットフォーム |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-dump/win-x86) \| [x64](https://aka.ms/dotnet-dump/win-x64) \| [arm](https://aka.ms/dotnet-dump/win-arm) \| [arm-x64](https://aka.ms/dotnet-dump/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-dump/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-dump/linux-x64) \| [arm](https://aka.ms/dotnet-dump/linux-arm) \| [arm64](https://aka.ms/dotnet-dump/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-dump/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-dump/linux-musl-arm64) |

> [!NOTE]
> x86 アプリ上で `dotnet-dump` を使用するには、対応する x86 バージョンのツールが必要です。

## <a name="synopsis"></a>構文

```console
dotnet-dump [-h|--help] [--version] <command>
```

## <a name="description"></a>説明

`dotnet-dump` グローバル ツールを使用すると、Linux の `lldb` などの任意のネイティブ デバッガーを使用せずに、Windows と Linux のダンプを収集して分析できます。 このツールは、`lldb` が完全に動作しない Alpine Linux などのプラットフォームで重要です。 `dotnet-dump` ツールでは、SOS コマンドを実行してクラッシュとガベージ コレクター (GC) を分析できますが、これはネイティブのデバッガーではないため、ネイティブ スタック フレームの表示などの操作はサポートされていません。

## <a name="options"></a>オプション

- **`--version`**

  dotnet-dump ユーティリティのバージョンを表示します。

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

## <a name="commands"></a>コマンド

| コマンド                                     |
| ------------------------------------------- |
| [dotnet-dump collect](#dotnet-dump-collect) |
| [dotnet-dump analyze](#dotnet-dump-analyze) |

## <a name="dotnet-dump-collect"></a>dotnet-dump collect

プロセスからダンプをキャプチャします。

### <a name="synopsis"></a>構文

```console
dotnet-dump collect [-h|--help] [-p|--process-id] [-n|--name] [--type] [-o|--output] [--diag]
```

### <a name="options"></a>オプション

- **`-h|--help`**

  コマンド ライン ヘルプを表示します。

- **`-p|--process-id <PID>`**

  ダンプを収集するプロセスの ID 番号を指定します。

- **`-n|--name <name>`**

  ダンプを収集するプロセスの名前を指定します。

- **`--type <Full|Heap|Mini>`**

  プロセスから収集する情報の種類を決定する、ダンプの種類を指定します。 次の 3 種類があります。

  - `Full` - モジュール イメージを含むメモリをすべて含む最大のダンプ。
  - `Heap`: モジュールの一覧、スレッドの一覧、すべてのスタック、例外情報、ハンドル情報、およびマップされたイメージを除くすべてのメモリを含む、大規模で比較的包括的なダンプです。
  - `Mini`: モジュールの一覧、スレッドの一覧、例外情報、およびすべてのスタックを含む小さいダンプです。

  指定しない場合の既定は、`Full` です。

- **`-o|--output <output_dump_path>`**

  収集したダンプを書き込む完全なパスとファイル名。

  指定していない場合は、次になります。

  - Windows での既定は、 *.\dump_YYYYMMDD_HHMMSS.dmp* です。
  - Linux での既定は、 *./core_YYYYMMDD_HHMMSS* です。

  YYYYMMDD は、年/月/日で、HHMMSS は時間/分/秒です。

- **`--diag`**

  ダンプの収集の診断ログを有効にします。

> [!NOTE]
> Linux と macOS 上でこのコマンドを使用するには、ターゲット アプリケーションと `dotnet-dump` で同じ `TMPDIR` 環境変数が共有されることが前提とされています。 それ以外の場合、このコマンドはタイムアウトします。

> [!NOTE]
> `dotnet-dump` を使用してダンプを収集するには、ターゲット プロセスを実行しているユーザーと同じユーザーとして、またはルートとしてそれを実行する必要があります。 それ以外の場合、このツールでターゲット プロセスとの接続を確立することはできません。

## <a name="dotnet-dump-analyze"></a>dotnet-dump analyze

ダンプを検索する対話型シェルを開始します。 このシェルでは、さまざまな [SOS コマンド](#analyze-sos-commands)を受け入れます。

### <a name="synopsis"></a>構文

```console
dotnet-dump analyze <dump_path> [-h|--help] [-c|--command]
```

### <a name="arguments"></a>引数

- **`<dump_path>`**

  分析のためにファイルをダンプするパスを指定します。

### <a name="options"></a>オプション

- **`-c|--command <debug_command>`**

  起動時にシェルで実行する[コマンド](#analyze-sos-commands)を指定します。

### <a name="analyze-sos-commands"></a>SOS コマンドを分析する

| コマンド                             | 関数                                                                                      |
| ----------------------------------- | --------------------------------------------------------------------------------------------- |
| `soshelp`                           | 使用可能なコマンドをすべて表示します。                                                               |
| `soshelp|help <command>`            | 指定したコマンドを表示します。                                                               |
| `exit|quit`                         | 対話モードを終了します。                                                                       |
| `clrstack <arguments>`              | マネージド コードのみのスタック トレースを提供します。                                                  |
| `clrthreads <arguments>`            | 実行中のマネージド スレッドを一覧表示します。                                                            |
| `dumpasync <arguments>`             | ガベージ コレクトされたヒープ上の非同期状態のマシンに関する情報を表示します。                |
| `dumpassembly <arguments>`          | 指定されたアドレスにあるアセンブリに関する詳細を表示します。                                 |
| `dumpclass <arguments>`             | 指定されたアドレスにある `EEClass` 構造体に関する情報を表示します。                  |
| `dumpdelegate <arguments>`          | 指定されたアドレスにあるデリゲートに関する情報を表示します。                             |
| `dumpdomain <arguments>`            | すべての AppDomain と、指定されたドメイン内のすべてのアセンブリに関する情報を表示します。       |
| `dumpheap <arguments>`              | ガベージ コレクトされたヒープと、オブジェクトの収集統計に関する情報を表示します。       |
| `dumpil <arguments>`                | マネージド メソッドに関連付けられている Microsoft Intermediate Language (MSIL) を表示します。 |
| `dumplog <arguments>`               | メモリ内ストレス ログの内容を、指定したファイルに書き込みます。                         |
| `dumpmd <arguments>`                | 指定されたアドレスにある `MethodDesc` 構造体に関する情報を表示します。               |
| `dumpmodule <arguments>`            | 指定されたアドレスにあるモジュールに関する情報を表示します。                               |
| `dumpmt <arguments>`                | 指定されたアドレスにある `MethodTable` に関する情報を表示します。                        |
| `dumpobj <arguments>`               | 指定したアドレスにあるオブジェクトに関する情報を表示します。                                      |
| `dso|dumpstackobjects <arguments>`  | 現在のスタックの範囲内で見つかったすべてのマネージド オブジェクトを表示します。                    |
| `eeheap <arguments>`                | 内部のランタイム データ構造体によって消費されたプロセス メモリに関する情報を表示します。              |
| `finalizequeue <arguments>`         | 完了の目的で登録されているすべてのオブジェクトを表示します。                                             |
| `gcroot <arguments>`                | 指定されたアドレスにあるオブジェクトへの参照 (またはルート) に関する情報を表示します。             |
| `gcwhere <arguments>`               | 渡された引数の GC ヒープ内の場所を表示します。                               |
| `ip2md <arguments>`                 | 指定したアドレスにある JIT コード内の `MethodDesc` 構造体に関する情報を表示します。                     |
| `histclear <arguments>`             | `hist*` コマンドのファミリによって使用されているすべてのリソースを解放します。                                |
| `histinit <arguments>`              | デバッグ対象に保存されているストレス ログから SOS 構造体を初期化します。                     |
| `histobj <arguments>`               | `<arguments>` に関連するガベージ コレクションのストレス ログの再配置を表示します。              |
| `histobjfind <arguments>`           | 指定されたアドレスにあるオブジェクトを参照するすべてのログ エントリを表示します。              |
| `histroot <arguments>`              | 指定したルートの上位変換と再配置の両方に関係する情報を表示します。        |
| `lm|modules`                        | プロセス内のネイティブ モジュールを表示します。                                                   |
| `name2ee <arguments>`               | `<argument>` の `MethodTable` 構造体と `EEClass` 構造体を表示します。                     |
| `pe|printexception <arguments>`     | `<argument>` の <xref:System.Exception> クラスから派生したオブジェクトを表示します。      |
| `setsymbolserver <arguments>`       | シンボル サーバーのサポートを有効にします。                                                             |
| `syncblk <arguments>`               | SyncBlock の所有者の情報を表示します。                                                           |
| `threads|setthread <threadid>`      | SOS コマンドの現在のスレッド ID を設定するか表示します。                                  |

> [!NOTE]
> その他の詳細は、「[.NET 用 SOS デバッガー拡張](sos-debugging-extension.md)」に記載されています。

## <a name="using-dotnet-dump"></a>`dotnet-dump` を使用する

まずはダンプを収集します。 コア ダンプを既に生成している場合、この手順をスキップできます。 コア ダンプは、オペレーティング システムまたは .NET Core ランタイムに組み込まれているそれぞれの[ダンプ生成機能](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md)で作成できます。

```console
$ dotnet-dump collect --process-id 1902
Writing minidump to file ./core_20190226_135837
Written 98983936 bytes (24166 pages) to core file
Complete
```

ここで、次のように `analyze` コマンドを使用して、コア ダンプを分析します。

```console
$ dotnet-dump analyze ./core_20190226_135850
Loading core dump: ./core_20190226_135850
Ready to process analysis commands. Type 'help' to list available commands or 'help [command]' to get detailed help on a command.
Type 'quit' or 'exit' to exit the session.
>
```

この操作により、次のようなコマンドを受け取る対話型セッションが開始されます。

```console
> clrstack
OS Thread Id: 0x573d (0)
    Child SP               IP Call Site
00007FFD28B42C58 00007fb22c1a8ed9 [HelperMethodFrame_PROTECTOBJ: 00007ffd28b42c58] System.RuntimeMethodHandle.InvokeMethod(System.Object, System.Object[], System.Signature, Boolean, Boolean)
00007FFD28B42DD0 00007FB1B1334F67 System.Reflection.RuntimeMethodInfo.Invoke(System.Object, System.Reflection.BindingFlags, System.Reflection.Binder, System.Object[], System.Globalization.CultureInfo) [/root/coreclr/src/mscorlib/src/System/Reflection/RuntimeMethodInfo.cs @ 472]
00007FFD28B42E20 00007FB1B18D33ED SymbolTestApp.Program.Foo4(System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 54]
00007FFD28B42ED0 00007FB1B18D2FC4 SymbolTestApp.Program.Foo2(Int32, System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 29]
00007FFD28B42F00 00007FB1B18D2F5A SymbolTestApp.Program.Foo1(Int32, System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 24]
00007FFD28B42F30 00007FB1B18D168E SymbolTestApp.Program.Main(System.String[]) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 19]
00007FFD28B43210 00007fb22aa9cedf [GCFrame: 00007ffd28b43210]
00007FFD28B43610 00007fb22aa9cedf [GCFrame: 00007ffd28b43610]
```

アプリを強制終了させた未処理の例外を表示するには、次のようにします。

```console
> pe -lines
Exception object: 00007fb18c038590
Exception type:   System.Reflection.TargetInvocationException
Message:          Exception has been thrown by the target of an invocation.
InnerException:   System.Exception, Use !PrintException 00007FB18C038368 to see more.
StackTrace (generated):
SP               IP               Function
00007FFD28B42DD0 0000000000000000 System.Private.CoreLib.dll!System.RuntimeMethodHandle.InvokeMethod(System.Object, System.Object[], System.Signature, Boolean, Boolean)
00007FFD28B42DD0 00007FB1B1334F67 System.Private.CoreLib.dll!System.Reflection.RuntimeMethodInfo.Invoke(System.Object, System.Reflection.BindingFlags, System.Reflection.Binder, System.Object[], System.Globalization.CultureInfo)+0xa7 [/root/coreclr/src/mscorlib/src/System/Reflection/RuntimeMethodInfo.cs @ 472]
00007FFD28B42E20 00007FB1B18D33ED SymbolTestApp.dll!SymbolTestApp.Program.Foo4(System.String)+0x15d [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 54]
00007FFD28B42ED0 00007FB1B18D2FC4 SymbolTestApp.dll!SymbolTestApp.Program.Foo2(Int32, System.String)+0x34 [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 29]
00007FFD28B42F00 00007FB1B18D2F5A SymbolTestApp.dll!SymbolTestApp.Program.Foo1(Int32, System.String)+0x3a [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 24]
00007FFD28B42F30 00007FB1B18D168E SymbolTestApp.dll!SymbolTestApp.Program.Main(System.String[])+0x6e [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 19]

StackTraceString: <none>
HResult: 80131604
```

## <a name="special-instructions-for-docker"></a>Docker 用の特別な手順

ダンプの収集を Docker で実行している場合、`SYS_PTRACE` 機能 (`--cap-add=SYS_PTRACE` または `--privileged`) が必要です。

Microsoft .NET Core SDK Linux Docker イメージでは、一部の `dotnet-dump` コマンドで次の例外がスローされる場合があります。

> 未処理の例外:System.DllNotFoundException:共有ライブラリ 'libdl.so' またはその依存関係の 1 つを読み込めませんでした。

"libc6-dev" パッケージをインストールすると、この問題を回避できます。

## <a name="see-also"></a>関連項目

- [メモリ ダンプの収集と分析に関するブログ](https://devblogs.microsoft.com/dotnet/collecting-and-analyzing-memory-dumps/)
- [ヒープ分析ツール (dotnet-gcdump)](dotnet-gcdump.md)
