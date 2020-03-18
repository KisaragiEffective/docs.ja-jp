---
title: 非同期プログラミング
description: がコアF#関数型プログラミングの概念から派生した言語レベルのプログラミングモデルに基づいて、非同期性のクリーンなサポートを提供する方法について説明します。
ms.date: 12/17/2018
ms.openlocfilehash: 7021d7936d10f9ea6fceb4aa56db3285d21624ad
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628853"
---
# <a name="async-programming-in-f"></a>F\# での非同期プログラミング

非同期プログラミングは、さまざまな理由で、最新のアプリケーションに不可欠なメカニズムです。 ほとんどの開発者が直面する主なユースケースには、次の2つがあります。

- 多数の同時受信要求を処理できるサーバープロセスを提供する一方で、要求の処理中に使用されるシステムリソースを最小限に抑えながら、そのプロセスの外部のシステムまたはサービスからの入力を待機する
- バックグラウンド作業の同時進行中に応答性の高い UI またはメインスレッドを維持する

多くの場合、バックグラウンド作業には複数のスレッドの使用が含まれますが、非同期性とマルチスレッドの概念を個別に考慮することが重要です。 実際、別々の検討事項であり、互いに他者を意味する訳ではありません。 この記事では、これについてさらに詳しく説明します。

## <a name="asynchrony-defined"></a>非同期性の定義

非同期性はマルチスレッドの利用と独立である、という点に関してもう少し説明しておいた方が良いでしょう。 関連することがある 3 つの概念がありますが、これらは厳密には独立した概念です。

- 並行処理: 複数の計算がオーバーラップする時間帯に実行される場合
- 並列処理: 複数の計算または 1 つの計算の複数の部分が厳密に同じ時間に実行される場合
- 非同期処理: メインのプログラムとは別に、1 つ以上の計算が実行される場合

これら 3 つは直交概念ですが、一緒に用いてしまうと混同する場合があります。 たとえば、複数の非同期計算を並行して実行することが必要になる場合があります。 これは、並列処理または非同期処理が互いを意味するという意味ではありません。

"非同期" という語の語源を考慮すると、次の2つの部分が関係します。

- 「ない」を意味する"a"
- 「同時に」を意味する"synchronous"

これらの2つの用語をまとめると、"非同期" とは "同時に実行しない" ことを意味します。 以上で終わりです。 この定義には、並行処理も並列処理もありません。 このことは実際にも当てはまるのです。

実際には、のF#非同期計算は、メインプログラムフローとは別に実行するようにスケジュールされています。 これは、同時実行または並列処理を意味しません。また、計算が常にバックグラウンドで発生することも意味しません。 実際、非同期計算は、計算の性質と計算が実行されている環境に応じて、同期的に実行することもできます。

重要なことは、非同期計算はメインプログラムフローから独立していることです。 非同期計算がいつまたはどのように実行されるかに関する保証はほとんどありませんが、それらを調整しスケジューリングするためのアプローチがいくつかあります。 この記事の残りの部分では、 F# の非同期処理に関する主要な概念と、F# に組み込まれている型、関数、および式の使用方法について説明します。

## <a name="core-concepts"></a>主要な概念

F# では、非同期プログラミングは次の 3 つの主要な概念を中心にしています。

- `Async<'T>` 型。これは、コンポーザブルな非同期計算を表します。
- `Async` モジュール関数を使用すると、非同期処理のスケジュール、非同期計算の作成、および非同期結果の変換を実行できます。
- `async { }`[コンピュテーション式](../../language-reference/computation-expressions.md)。非同期計算を構築および制御するための便利な構文を提供します。

これら 3 つの概念を、次の例で確認できます。

```fsharp
open System
open System.IO

let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn "File %s has %d bytes" fileName bytes.Length
    }

[<EntryPoint>]
let main argv =
    printTotalFileBytes "path-to-file.txt"
    |> Async.RunSynchronously

    Console.Read() |> ignore
    0
```

この例では、`printTotalFileBytes` 関数は `string -> Async<unit>`型です。 関数を呼び出すと、実際には非同期計算が実行されません。 代わりに、非同期に実行する作業の*仕様*として機能する `Async<unit>` が返されます。 その本体で `Async.AwaitTask` を呼び出し、<xref:System.IO.File.ReadAllBytesAsync%2A> の結果を適切な型に変換します。

もう1つの重要な行は、`Async.RunSynchronously`の呼び出しです。 これは、 F# の非同期計算を実際に実行する際に呼び出す必要のある Async モジュールの開始関数の 1 つです。

これは、 C#`async` プログラミングの Visual Basic スタイルとの基本的な違いです。 でF#は、非同期計算は**コールドタスク**と考えることができます。 これらを実際に実行するには、明示的に開始する必要があります。 これにはいくつかのC#利点があります。これにより、または Visual Basic よりもはるかに簡単に非同期作業を組み合わせることができます。

## <a name="combine-asynchronous-computations"></a>非同期計算の結合

次に、上記の例をもとに計算処理を組み合わせる例を示します。

```fsharp
open System
open System.IO

let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn "File %s has %d bytes" fileName bytes.Length
    }

[<EntryPoint>]
let main argv =
    argv
    |> Array.map printTotalFileBytes
    |> Async.Parallel
    |> Async.Ignore
    |> Async.RunSynchronously

    0
```

ご覧のように、`main` 関数には多くの呼び出しが行われています。 概念的には、次のことが行われます。

1. コマンドライン引数を、`Array.map`を使用して `Async<unit>` の計算に変換します。
2. `printTotalFileBytes` 計算の実行時に並列実行をスケジュールして実行する `Async<'T[]>` を作成します。
3. 並列計算を実行し、その結果を無視する `Async<unit>` を作成します。
4. `Async.RunSynchronously` で最後の計算を明示的に実行し、完了するまでブロックします。

このプログラムを実行すると、コマンドライン引数ごとに `printTotalFileBytes` が並列実行されます。 非同期計算はプログラムフローとは無関係に実行されるため、それぞれ情報を出力して終了する順序は決まりません。 計算処理は並列にスケジューリングされますが、その実行順序は保証されません。

## <a name="sequence-asynchronous-computations"></a>シーケンスの非同期計算

`Async<'T>` は既に実行されているタスクではなく作業の仕様であるため、より複雑な変換を簡単に実行できます。 非同期計算のセットをシーケンス処理して、1つずつ実行する例を次に示します。

```fsharp
let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn "File %s has %d bytes" fileName bytes.Length
    }

[<EntryPoint>]
let main argv =
    argv
    |> Array.map printTotalFileBytes
    |> Async.Sequential
    |> Async.Ignore
    |> Async.RunSynchronously
    |> ignore
```

これにより、`printTotalFileBytes` を並行してスケジュールするのではなく、`argv` の要素の順序で実行するようにスケジュールが設定されます。 次の項目は、最後の計算の実行が完了するまではスケジュールされないため、実行中に重複が発生しないように計算がシーケンス処理されます。

## <a name="important-async-module-functions"></a>重要な Async モジュール関数

非同期コードF#を記述する場合は、通常、計算のスケジューリングを処理するフレームワークと対話します。 ただし、これは常にであるとは限りません。そのため、非同期処理をスケジュールするためのさまざまな開始関数について学習することをお勧めします。

非同期F#計算は、既に実行されている作業の表現ではなく、作業の_仕様_であるため、開始関数を使用して明示的に開始する必要があります。 さまざまなコンテキストで役に立つ[非同期開始関数](https://msdn.microsoft.com/library/ee370232.aspx)が多数あります。 次のセクションでは、より一般的な開始関数のいくつかについて説明します。

### <a name="asyncstartchild"></a>Async.StartChild

非同期計算内で子計算を開始します。 これを用いることで、複数の非同期計算を同時に実行できます。 子の計算では、キャンセルトークンを親計算と共有しています。 親の計算が取り消された場合は、子の計算も取り消されます。

署名:

```fsharp
computation: Async<'T> - timeout: ?int -> Async<Async<'T>>
```

使用すべき状況:

- 一度に 1 つずつではなく、複数の非同期計算を同時に実行するが、並列でスケジューリングされない場合。
- 子計算の有効期間を親計算の有効期間に関連付ける場合。

注意事項:

- `Async.StartChild` を使用した複数の計算の開始は、並列でのスケジュール設定と同じではありません。 計算を並列でスケジュールする場合は、`Async.Parallel`を使用します。
- 親計算を取り消すと、開始したすべての子計算の取り消しをトリガーします。

### <a name="asyncstartimmediate"></a>Async.StartImmediate

非同期計算を実行し、現在のオペレーティング システムのスレッドですぐに開始します。 これは、計算中に呼び出し元のスレッドで何かを更新する必要がある場合に便利です。 たとえば、非同期計算で UI を更新する必要がある場合 (進行状況バーを更新する場合など) は、`Async.StartImmediate` を使用する必要があります。

署名:

```fsharp
computation: Async<unit> - cancellationToken: ?CancellationToken -> unit
```

使用すべき状況:

- 非同期計算の途中で、呼び出し元のスレッドで何かを更新する必要がある場合。

注意事項:

- 非同期計算のコードは、スケジュールされている任意のスレッドで実行されます。 これは、そのスレッドが UI スレッドなど、何らかの方法で重要な場合に問題になることがあります。 このような場合、`Async.StartImmediate` の使用は不適切である可能性があります。

### <a name="asyncstartastask"></a>Async.StartAsTask

スレッド プールで計算を実行します。 計算が終了した後、対応する状態で完了する <xref:System.Threading.Tasks.Task%601> を返します (結果を生成するか、例外をスローするか、または取り消されます)。 キャンセルトークンが指定されていない場合は、既定のキャンセルトークンが使用されます。

署名:

```fsharp
computation: Async<'T> - taskCreationOptions: ?TaskCreationOptions - cancellationToken: ?CancellationToken -> Task<'T>
```

使用すべき状況:

- 非同期計算の結果を表すために <xref:System.Threading.Tasks.Task%601> を想定している .NET API を呼び出す必要がある場合。

注意事項:

- この呼び出しでは、追加の `Task` オブジェクトが割り当てられます。これにより、頻繁に使用される場合にオーバーヘッドが増加する可能性があります。

### <a name="asyncparallel"></a>Async.Parallel

並列で実行される一連の非同期計算をスケジューリングします。 並列処理の次数は、`maxDegreesOfParallelism` パラメーターを指定することによって、必要に応じてチューニング/調整できます。

署名:

```fsharp
computations: seq<Async<'T>> - ?maxDegreesOfParallelism: int -> Async<'T[]>
```

使用するタイミング:

- 一連の計算を同時に実行する必要があるが、実行の順序に依存しない場合。
- すべてが完了するまで並列でスケジューリングされた計算結果を必要としない場合。

注意事項:

- すべての計算が完了するまで、結果として得られる値の配列にアクセスできません。
- 計算は実行されますが、スケジュールが設定されます。 つまり、実行の順序に依存することはできません。

### <a name="asyncsequential"></a>Async.Sequential

一連の非同期計算を渡された順序で実行されるようにスケジューリングします。 最初の計算を実行し、その後、その次という具合です。 計算は並列実行されません。

署名:

```fsharp
computations: seq<Async<'T>> -> Async<'T[]>
```

使用するタイミング:

- 複数の計算を順番に実行する必要がある場合。

注意事項:

- すべての計算が完了するまで、結果として得られる値の配列にアクセスできません。
- 計算は、この関数に渡される順序で実行されます。そのため、結果が返されるまでに時間がかかることがあります。

### <a name="asyncawaittask"></a>Async.AwaitTask

指定された <xref:System.Threading.Tasks.Task%601> が完了するまで待機し、その結果をとして返す非同期計算を返し `Async<'T>`

署名:

```fsharp
task: Task<'T>  -> Async<'T>
```

使用すべき状況:

- F#非同期計算内で <xref:System.Threading.Tasks.Task%601> を返す .net API を使用する場合。

注意事項:

- 例外は、タスク並列ライブラリの規則に従って <xref:System.AggregateException> にラップされます。これはF# 、async が例外を一般に示す方法とは異なります。

### <a name="asynccatch"></a>Async.Catch

指定された `Async<'T>`を実行し、`Async<Choice<'T, exn>>`を返す非同期計算を作成します。 指定された `Async<'T>` が正常に完了した場合、結果の値と共に `Choice1Of2` が返されます。 完了前に例外がスローされた場合は、発生した例外と共に `Choice2of2` が返されます。 多くの計算で構成され、その計算の1つが例外をスローする非同期計算で使用される場合、外側の計算は完全に停止されます。

署名:

```fsharp
computation: Async<'T> -> Async<Choice<'T, exn>>
```

使用すべき状況:

- 例外によって失敗する可能性があり、呼び出し元でその例外を処理する必要がある非同期処理を実行する場合。

注意事項:

- 結合またはシーケンスされた非同期計算を使用する場合、その "内部" 計算の1つが例外をスローすると、外側の計算が完全に停止します。

### <a name="asyncignore"></a>Async.Ignore

指定された計算を実行し、その結果を無視する非同期計算を作成します。

署名:

```fsharp
computation: Async<'T> -> Async<unit>
```

使用すべき状況:

- 結果が不要な非同期計算がある場合。 これは、非非同期コードの `ignore` コードに似ています。

注意事項:

- `Async.Start` または `Async<unit>`を必要とする別の関数を使用するためにこれを使用する必要がある場合は、結果を破棄することができるかどうかを検討してください。 型シグネチャに一致するように結果を破棄することは、通常はできません。

### <a name="asyncrunsynchronously"></a>Async.RunSynchronously

非同期計算を実行し、呼び出し元のスレッドでその結果を待機します。 これはブロッキングする呼び出しです。

署名:

```fsharp
computation: Async<'T> - timeout: ?int - cancellationToken: ?CancellationToken -> 'T
```

使用するタイミング:

- 必要な場合、実行可能ファイルのエントリポイントでアプリケーション内で 1 回だけ使用します。
- パフォーマンスを気にせずに、他の一連の非同期操作を一度に実行する場合。

注意事項:

- `Async.RunSynchronously` を呼び出すと、実行が完了するまで呼び出し元のスレッドがブロックされます。

### <a name="asyncstart"></a>Async.Start

`unit`を返すスレッドプール内の非同期計算を開始します。 は結果を待機しません。 `Async.Start` で開始された入れ子になった計算は、それを呼び出した親計算とは無関係に完全に開始されます。 有効期間は、どの親計算にも関連付けられていません。 親計算が取り消された場合、子計算は取り消されません。

署名:

```fsharp
computation: Async<unit> - cancellationToken: ?CancellationToken -> unit
```

次の場合にのみ使用します。

- 非同期計算による結果が得られない、かつ/またはその結果を処理する必要がない。
- 非同期計算がいつ完了するかを知る必要がない。
- 非同期計算が実行されるスレッドを認識する必要がない。
- タスクの結果として発生した例外を認識したり、報告したりする必要がない。

注意事項:

- `Async.Start` で開始された計算によって発生した例外は、呼び出し元に反映されません。 呼び出し履歴は完全にアンワインドされます。
- すべての effectful 作業 (`printfn`の呼び出しなど) が `Async.Start` で開始されても、プログラムの実行のメインスレッドで効果が発生することはありません。

## <a name="interoperate-with-net"></a>.NET との相互運用

非同期[/await](../../../standard/async.md)スタイルの非同期プログラミングを使用C#する .net ライブラリまたはコードベースを使用する場合があります。 ほとんどC#の .net ライブラリでは、`Async<'T>`ではなく、<xref:System.Threading.Tasks.Task%601> と <xref:System.Threading.Tasks.Task> の種類を中核となる抽象化として使用するため、これらの2つの方法の間には、非同期性にまたがる境界を越える必要があります。

### <a name="how-to-work-with-net-async-and-taskt"></a>.NET async と `Task<T>` の使用方法

<xref:System.Threading.Tasks.Task%601> を使用する .NET 非同期ライブラリおよびコードベースの操作 (つまり、戻り値を持つ非同期計算) は簡単であり、でF#のサポートが組み込まれています。

`Async.AwaitTask` 関数を使用すると、.NET の非同期計算を待機できます。

```fsharp
let getValueFromLibrary param =
    async {
        let! value = DotNetLibrary.GetValueAsync param |> Async.AwaitTask
        return value
    }
```

`Async.StartAsTask` 関数を使用すると、非同期計算を .NET 呼び出し元に渡すことができます。

```fsharp
let computationForCaller param =
    async {
        let! result = getAsyncResult param
        return result
    } |> Async.StartAsTask
```

### <a name="how-to-work-with-net-async-and-task"></a>.NET async と `Task` の使用方法

<xref:System.Threading.Tasks.Task> を使用する Api (つまり、値を返さない .NET 非同期計算) を操作するには、`Async<'T>` を <xref:System.Threading.Tasks.Task>に変換する関数を追加する必要があります。

```fsharp
module Async =
    // Async<unit> -> Task
    let startTaskFromAsyncUnit (comp: Async<unit>) =
        Async.StartAsTask comp :> Task
```

入力として <xref:System.Threading.Tasks.Task> を受け入れる `Async.AwaitTask` が既に存在します。 これと以前に定義した `startTaskFromAsyncUnit` 関数を使用すると、 F#非同期計算から <xref:System.Threading.Tasks.Task> 型を開始および待機できます。

## <a name="relationship-to-multi-threading"></a>マルチスレッドとの関係

この記事ではスレッド処理について説明していますが、次の2つの重要な点に注意してください。

1. 現在のスレッドで明示的に開始されている場合を除き、非同期計算とスレッド間の関係はありません。
1. F# での非同期プログラミングは、マルチスレッドを抽象的にしたものではありません。

たとえば、計算処理は作業の性質によっては実際には呼び出し元のスレッドで実行される場合があります。 また、計算処理がスレッド間を "ジャンプ" して、"待機中" の少しの時間 (ネットワーク呼び出しが転送中の場合など) の間に有用な作業を行うこともあります。

F# には現在のスレッド (または現在のスレッド以外) で 非同期計算を開始する機能を提供していますが、一般的には非同期処理は特定のスレッド処理方法に関連付けられません。

## <a name="see-also"></a>参照

- [F#非同期プログラミングモデル](https://www.microsoft.com/research/publication/the-f-asynchronous-programming-model)
- [Jet com のF#非同期ガイド](https://medium.com/jettech/f-async-guide-eb3c8a2d180a)
- [F#楽しいと利益の非同期プログラミングガイド](https://fsharpforfunandprofit.com/posts/concurrency-async-and-parallel/)
- [とF#でC#の非同期: 非同期の注意事項C#](http://tomasp.net/blog/csharp-async-gotchas.aspx/)
