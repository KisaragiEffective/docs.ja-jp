---
title: .NET Core でアセンブリのアンローダビリティを使用およびデバッグする方法
description: マネージド アセンブリのロードとアンロードに収集可能な AssemblyLoadContext を使用する方法とアンロードの成功を妨げる問題をデバッグする方法について説明します。
author: janvorli
ms.author: janvorli
ms.date: 02/05/2019
ms.openlocfilehash: 267c2209556b66ab3541c9c79c99d7eceb2024da
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159742"
---
# <a name="how-to-use-and-debug-assembly-unloadability-in-net-core"></a>.NET Core でアセンブリのアンローダビリティを使用およびデバッグする方法

.NET Core 3.0 以降では、一連のアセンブリをロードし、後でアンロードする機能がサポートされます。 .NET Framework では、この目的でカスタム アプリ ドメインが使用されていましたが、.NET Core では、既定のアプリ ドメインは 1 つしかサポートされません。

このため、.NET Core 3.0 以降のバージョンでは、アンロードをサポートするために <xref:System.Runtime.Loader.AssemblyLoadContext> が用意されています。 一連のアセンブリを収集可能な `AssemblyLoadContext` にロードし、その中のメソッドを実行するか、またはリフレクションを使用して調べた後、最後に `AssemblyLoadContext` をアンロードすることができます。 これにより、`AssemblyLoadContext` にロードされたアセンブリがアンロードされます。

`AssemblyLoadContext` を使用したアンロードと AppDomain を使用したアンロードには、1 つの注目すべき違いがあります。 AppDomain では、アンロードが強制されます。 アンロード時には、ターゲット AppDomain で実行中のすべてのスレッドが中止されたり、ターゲット AppDomain で作成されたマネージド COM オブジェクトが破棄されたりといったことが行われます。 `AssemblyLoadContext` では、アンロードは "協調的" です。 <xref:System.Runtime.Loader.AssemblyLoadContext.Unload%2A?displayProperty=nameWithType> メソッドを呼び出すと、アンロードが単に開始されるだけです。 アンロードは次の場合に完了します。

- コール スタックに、`AssemblyLoadContext` にロードされたアセンブリ内のメソッドを含むスレッドがなくなった。
- `AssemblyLoadContext` にロードされたアセンブリ内の型、それらの型のインスタンス、およびアセンブリ自体が、次の方法により参照されなくなった。
  - 弱い参照 (`AssemblyLoadContext` または <xref:System.WeakReference>) を除く、<xref:System.WeakReference%601> の外部での参照。
  - [ の内部と外部の両方からの厳密なガベージ コレクター (GC) ハンドル (](xref:System.Runtime.InteropServices.GCHandleType.Normal)GCHandleType.Normal[ または ](xref:System.Runtime.InteropServices.GCHandleType.Pinned)GCHandleType.Pinned`AssemblyLoadContext`)。

## <a name="use-collectible-assemblyloadcontext"></a>収集可能な AssemblyLoadContext の使用

このセクションは、収集可能な `AssemblyLoadContext` に NET Core アプリケーションをロードして、そのエントリ ポイントを実行した後、NET Core アプリケーションをアンロードするための簡単な方法を詳細に説明したステップバイステップのチュートリアルです。 完全なサンプルについては、[https://github.com/dotnet/samples/tree/master/core/tutorials/Unloading](https://github.com/dotnet/samples/tree/master/core/tutorials/Unloading) を参照してください。

### <a name="create-a-collectible-assemblyloadcontext"></a>収集可能な AssemblyLoadContext を作成する

<xref:System.Runtime.Loader.AssemblyLoadContext> からクラスを派生させ、その <xref:System.Runtime.Loader.AssemblyLoadContext.Load%2A?displayProperty=nameWithType> メソッドをオーバーロードする必要があります。 このメソッドは、その `AssemblyLoadContext` に読み込まれたアセンブリと依存関係にあるすべてのアセンブリへの参照を解決します。

次のコードは、最も簡単なカスタム `AssemblyLoadContext` の例を示します。

[!code-csharp[Simple custom AssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/simple_example.cs#1)]

ご覧のとおり、`Load` メソッドから `null` が返されます。 つまり、すべての依存関係アセンブリが既定のコンテキストに読み込まれ、新しいコンテキストには、そこに明示的に読み込まれたアセンブリのみが含まれます。

一部またはすべての依存関係も `AssemblyLoadContext` に読み込む必要がある場合は、`AssemblyDependencyResolver` メソッドで `Load` を使用することができます。 `AssemblyDependencyResolver` によりアセンブリ名が絶対アセンブリ ファイル パスに解決されます。 リゾルバーでは、コンテキストに読み込まれたメイン アセンブリのディレクトリ内の *.deps.json* ファイルとアセンブリ ファイルが使用されます。

[!code-csharp[Advanced custom AssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/complex_assemblyloadcontext.cs)]

### <a name="use-a-custom-collectible-assemblyloadcontext"></a>カスタムの収集可能な AssemblyLoadContext を使用する

このセクションでは、より単純な `TestAssemblyLoadContext` が使用されていることを前提とします。

次のように、カスタム `AssemblyLoadContext` のインスタンスを作成して、アセンブリをそこに読み込むことができます。

[!code-csharp[Part 1](~/samples/snippets/standard/assembly/unloading/simple_example.cs#3)]

読み込まれたアセンブリによって参照される各アセンブリについて、`TestAssemblyLoadContext.Load` メソッドを呼び出して、`TestAssemblyLoadContext` がどこからアセンブリを取得すればよいかを判断できるようにします。 この例では、`null` が返されます。これは、ランタイムでアセンブリを読み込むために既定で使用される場所から既定のコンテキストに読み込む必要があることを示します。

アセンブリが読み込まれたので、そこからメソッドを実行できます。 `Main` メソッドを実行します。

[!code-csharp[Part 2](~/samples/snippets/standard/assembly/unloading/simple_example.cs#4)]

`Main` メソッドが返ると、カスタム `Unload` で `AssemblyLoadContext` メソッドを呼び出すか、`AssemblyLoadContext` に対する参照を削除することにより、アンロードを開始できます。

[!code-csharp[Part 3](~/samples/snippets/standard/assembly/unloading/simple_example.cs#5)]

これだけでテスト アセンブリをアンロードできます。 では、実際に、これらすべてをインライン化できない個別のメソッドに配置して、`TestAssemblyLoadContext`、`Assembly`、および `MethodInfo` (`Assembly.EntryPoint`) が、スタック スロット参照 (実数または JIT 対応のローカル) でキープ アライブできないようにしましょう。 これにより、`TestAssemblyLoadContext` をキープ アライブし、アンロードを妨ぐことができます。

また、`AssemblyLoadContext` への弱い参照が返されるので、後でこれを使用して、アンロードの完了を検出できます。

[!code-csharp[Part 4](~/samples/snippets/standard/assembly/unloading/simple_example.cs#2)]

これで、この関数を実行して、アセンブリのロード、実行、アンロードを行うことができるようになりました。

[!code-csharp[Part 5](~/samples/snippets/standard/assembly/unloading/simple_example.cs#6)]

ただし、アンロードは即座には完了しません。 前述のとおり、これはガベージ コレクターに依存して、テスト アセンブリからすべてのオブジェクトを収集します。 多くの場合、アンロードは完了するまで待機する必要はありません。 しかしながら、アンロードが完了したことを認識できると便利な場合があります。 たとえば、ディスクからカスタム `AssemblyLoadContext` にロードされたアセンブリ ファイルを削除したい場合などです。 このような場合、以下のコード スニペットを使用できます。 これは、ガベージ コレクションをトリガーし、カスタム `AssemblyLoadContext` への弱い参照が、ターゲット オブジェクトが収集されたことを示す `null` に設定されるまで、ループ内の保留中のファイナライザーを待機します。 ほとんどの場合、ループは 1 回だけ通る必要があります。 しかしながら、より複雑な場合、つまり `AssemblyLoadContext` で実行されるコードによって作成されるオブジェクトにファイナライザーが含まれている場合は、ループを複数回通ることが必要になる可能性があります。

[!code-csharp[Part 6](~/samples/snippets/standard/assembly/unloading/simple_example.cs#7)]

### <a name="the-unloading-event"></a>アンロード イベント

アンロードの開始時に何らかのクリーンアップを実行するために、カスタム `AssemblyLoadContext` にロードされたコードが必要になることがあります。 たとえば、スレッドの停止や厳密な GC ハンドルのクリーンアップが必要になる場合があります。 このような場合、`Unloading` イベントを使用できます。 必要なクリーンアップを実行するハンドラーをこのイベントにフックすることができます。

### <a name="troubleshoot-unloadability-issues"></a>アンローダビリティ問題のトラブルシューティング

アンロードの協調的な性質のために、収集可能な `AssemblyLoadContext` の内容をキープ アライブしてアンロードを妨げている可能性がある参照のことを忘れてしまいがちです。 以下に、参照を保持できるエンティティの概要を示します (一部は明確ではありません)。

- 収集可能な `AssemblyLoadContext` の外部から保持される標準参照は、スタック スロットまたはプロセッサ レジスタ (ユーザー コードで明示的に、または Just-In-Time (JIT) コンパイラで暗黙的に作成されるメソッド ローカル)、静的変数、または厳密な (固定の) GC ハンドルに格納され、次のものを推移的に指します。
  - 収集可能な `AssemblyLoadContext` に読み込まれたアセンブリ。
  - このようなアセンブリ内の型。
  - このようなアセンブリ内の型のインスタンス。
- 収集可能な `AssemblyLoadContext` に読み込まれたアセンブリからコードを実行するスレッド。
- 収集可能な `AssemblyLoadContext` 内部で作成されたカスタムの収集不可能な `AssemblyLoadContext` 型のインスタンス。
- コールバックがカスタムの <xref:System.Threading.RegisteredWaitHandle> 内のメソッドに設定された保留中の `AssemblyLoadContext` インスタンス。

> [!TIP]
> スタック スロットまたはプロセッサ レジスタに格納されていて、`AssemblyLoadContext` のアンロードを妨げる可能性があるオブジェクト参照は、次の状況で発生する可能性があります。
>
> - ユーザーが作成したローカル変数がなくても、関数呼び出しの結果が別の関数に直接渡される場合。
> - JIT コンパイラが、メソッドのある時点で使用可能だったオブジェクトへの参照を保持している場合。

## <a name="debug-unloading-issues"></a>アンロードに関する問題のデバッグ

アンロードに関する問題のデバッグは、面倒な場合があります。 何が `AssemblyLoadContext` を保持しているかはわからないが、アンロードが失敗する状況に陥ることがあります。 最も役立つツールは、WinDbg (Unix では LLDB) と SOS プラグインです。 特定の `LoaderAllocator` に属する `AssemblyLoadContext` をキープ アライブしているのが何かを突き止める必要があります。 SOS プラグインを使用すると、GC ヒープ オブジェクト、その階層、およびルートを調べることができます。

プラグインをデバッガーに読み込むには、次のコマンドをデバッガーのコマンド ラインに入力します。

WinDbg の場合 (WinDbg では、.NET Core アプリケーションを中断すると、自動的に実行されるようです):

```console
.loadby sos coreclr
```

LLDB の場合:

```console
plugin load /path/to/libsosplugin.so
```

アンロードの問題が発生するプログラム例をデバッグしましょう。 ソース コードの内容は次のとおりです。 WinDbg でこのプログラムを実行すると、このデバッガーでは、アンロードが成功したかどうかを確認しようとした直後で中断されます。 この後、原因の究明を開始できます。

> [!TIP]
> UNIX で LLDB を使用してデバッグする場合は、次の例の SOS コマンドの前に `!` がないようにします。

```console
!dumpheap -type LoaderAllocator
```

このコマンドは、GC ヒープ内にあり、型名に `LoaderAllocator` を含むすべてのオブジェクトをダンプします。 たとえば次のようになります。

```console
         Address               MT     Size
000002b78000ce40 00007ffadc93a288       48
000002b78000ceb0 00007ffadc93a218       24

Statistics:
              MT    Count    TotalSize Class Name
00007ffadc93a218        1           24 System.Reflection.LoaderAllocatorScout
00007ffadc93a288        1           48 System.Reflection.LoaderAllocator
Total 2 objects
```

次の例の "Statistics:" パーツで、`MT` に属する `MethodTable` (`System.Reflection.LoaderAllocator`) を確認します。これが、注目すべきオブジェクトです。 次に、最初にリストでこのエントリと `MT` が一致するエントリを見つけて、オブジェクト自体のアドレスを取得します。 この例では、"000002b78000ce40" です。

`LoaderAllocator` オブジェクトのアドレスがわかったので、別のコマンドを使用して GC ルートを見つけることができます。

```console
!gcroot -all 0x000002b78000ce40
```

このコマンドは、`LoaderAllocator` インスタンスが発生するオブジェクト参照のチェーンをダンプします。 リストはルートから始まります。ルートは、`LoaderAllocator` をキープ アライブしているエンティティであるため、問題の核心になります。 ルートは、スタック スロット、プロセッサ レジスタ、GC ハンドル、または静的変数のいずれかです。

`gcroot` コマンドの出力例を次に示します。

```console
Thread 4ac:
    000000cf9499dd20 00007ffa7d0236bc example.Program.Main(System.String[]) [E:\unloadability\example\Program.cs @ 70]
        rbp-20: 000000cf9499dd90
            ->  000002b78000d328 System.Reflection.RuntimeMethodInfo
            ->  000002b78000d1f8 System.RuntimeType+RuntimeTypeCache
            ->  000002b78000d1d0 System.RuntimeType
            ->  000002b78000ce40 System.Reflection.LoaderAllocator

HandleTable:
    000002b7f8a81198 (strong handle)
    -> 000002b78000d948 test.Test
    -> 000002b78000ce40 System.Reflection.LoaderAllocator

    000002b7f8a815f8 (pinned handle)
    -> 000002b790001038 System.Object[]
    -> 000002b78000d390 example.TestInfo
    -> 000002b78000d328 System.Reflection.RuntimeMethodInfo
    -> 000002b78000d1f8 System.RuntimeType+RuntimeTypeCache
    -> 000002b78000d1d0 System.RuntimeType
    -> 000002b78000ce40 System.Reflection.LoaderAllocator

Found 3 roots.
```

次の手順は、修正するために、ルートがどこにあるかを特定することです。 最も簡単なケースは、ルートがスタック スロットまたはプロセッサ レジスタである場合です。 この場合、`gcroot` には、フレームにルートとその関数を実行するスレッドを含む関数の名前が表示されます。 難解なケースは、ルートが静的変数または GC ハンドルの場合です。

前述の例では、1 番目のルートは、アドレス `System.Reflection.RuntimeMethodInfo` にある関数 `example.Program.Main(System.String[])` のフレームに格納されている `rbp-20` 型のローカルです (`rbp` はプロセッサ レジスタ `rbp` で、-20 は、そのレジスタからの 16 進のオフセットです)。

2 番目のルートは通常の (強い) `GCHandle` で、`test.Test` クラスのインスタンスへの参照を保持します。

3 番目のルートは、固定の `GCHandle` です。 これは実際には静的変数ですが、残念ながら、これを確認する方法はありません。 参照型の静的変数は、内部のランタイム構造体内のマネージド オブジェクト配列に格納されます。

`AssemblyLoadContext` のアンロードが妨げられる可能性のあるもう 1 つのケースは、スタック上の `AssemblyLoadContext` に読み込まれたアセンブリ内のメソッドのフレームがスレッドに含まれている場合です。 すべてのスレッドのマネージド コール スタックをダンプすると、これを確認できます。

```console
~*e !clrstack
```

コマンドは、"すべてのスレッドに `!clrstack` コマンドを適用する" ことを意味します。 このコマンドの出力例を次に示します。 残念ながら、UNIX の LLDB には、すべてのスレッドにコマンドを適用する方法がないため、手動でスレッドを切り替えて、`clrstack` コマンドを繰り返す必要があります。 デバッガーで "マネージド スタックを調べることができません" と表示されたすべてのスレッドを無視します。

```console
OS Thread Id: 0x6ba8 (0)
        Child SP               IP Call Site
0000001fc697d5c8 00007ffb50d9de12 [HelperMethodFrame: 0000001fc697d5c8] System.Diagnostics.Debugger.BreakInternal()
0000001fc697d6d0 00007ffa864765fa System.Diagnostics.Debugger.Break()
0000001fc697d700 00007ffa864736bc example.Program.Main(System.String[]) [E:\unloadability\example\Program.cs @ 70]
0000001fc697d998 00007ffae5fdc1e3 [GCFrame: 0000001fc697d998]
0000001fc697df28 00007ffae5fdc1e3 [GCFrame: 0000001fc697df28]
OS Thread Id: 0x2ae4 (1)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x61a4 (2)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x7fdc (3)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x5390 (4)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x5ec8 (5)
        Child SP               IP Call Site
0000001fc70ff6e0 00007ffb5437f6e4 [DebuggerU2MCatchHandlerFrame: 0000001fc70ff6e0]
OS Thread Id: 0x4624 (6)
        Child SP               IP Call Site
GetFrameContext failed: 1
0000000000000000 0000000000000000
OS Thread Id: 0x60bc (7)
        Child SP               IP Call Site
0000001fc727f158 00007ffb5437fce4 [HelperMethodFrame: 0000001fc727f158] System.Threading.Thread.SleepInternal(Int32)
0000001fc727f260 00007ffb37ea7c2b System.Threading.Thread.Sleep(Int32)
0000001fc727f290 00007ffa865005b3 test.Program.ThreadProc() [E:\unloadability\test\Program.cs @ 17]
0000001fc727f2c0 00007ffb37ea6a5b System.Threading.Thread.ThreadMain_ThreadStart()
0000001fc727f2f0 00007ffadbc4cbe3 System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
0000001fc727f568 00007ffae5fdc1e3 [GCFrame: 0000001fc727f568]
0000001fc727f7f0 00007ffae5fdc1e3 [DebuggerU2MCatchHandlerFrame: 0000001fc727f7f0]

```

ご覧のとおり、最後のスレッドには、`test.Program.ThreadProc()` が含まれています。 これは、`AssemblyLoadContext` に読み込まれたアセンブリ内の関数であるため、これが `AssemblyLoadContext` をキープ アライブしています。

## <a name="example-source-with-unloadability-issues"></a>アンローダビリティの問題を発生するソース例

前述のデバッグ例では、次のコードが使用されています。

### <a name="main-testing-program"></a>メインのテスト プログラム

[!code-csharp[Main testing program](~/samples/snippets/standard/assembly/unloading/unloadability_issues_example_main.cs)]

## <a name="program-loaded-into-the-testassemblyloadcontext"></a>TestAssemblyLoadContext に読み込まれたプログラム

次のコードは、メインのテスト プログラムで *メソッドに渡された*test.dll`ExecuteAndUnload` を表しています。

[!code-csharp[Program loaded into the TestAssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/unloadability_issues_example_test.cs)]
