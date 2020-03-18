---
title: I/O パイプライン - .NET
description: .NET で I/O パイプラインを効率的に使用し、コードの問題を回避する方法について学習します。
ms.date: 10/01/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- Pipelines
- Pipelines I/O
- I/O [.NET], Pipelines
author: rick-anderson
ms.author: riande
ms.openlocfilehash: b18b2bf31787fa58e614cd4f057fba9037fe8ad8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77627553"
---
# <a name="systemiopipelines-in-net"></a>.NET の System.IO.Pipelines

<xref:System.IO.Pipelines> は、.NET でハイ パフォーマンスの I/O をより簡単に行えるように設計された新しいライブラリです。 これは、すべての .NET 実装で動作する .NET Standard を対象とするライブラリです。

<a name="solve"></a>

## <a name="what-problem-does-systemiopipelines-solve"></a>System.IO.Pipelines によって解決される問題

<!-- corner case doesn't MT (machine translate)   -->
ストリーミング データを解析するアプリは、多くの特殊な通常とは異なるコード フローを持つ定型コードで構成されます。 定型および特殊なケースのコードは複雑で、保守が困難です。

`System.IO.Pipelines` は、以下のようになるように設計されています。

* ハイ パフォーマンスのストリーミング データ解析を実現する。
* コードの複雑さを軽減する。

次のコードは、クライアントから (`'\n'` で区切られた) 行区切りメッセージを受信する TCP サーバーでは一般的なものです。

```csharp
async Task ProcessLinesAsync(NetworkStream stream)
{
    var buffer = new byte[1024];
    await stream.ReadAsync(buffer, 0, buffer.Length);

    // Process a single line from the buffer
    ProcessLine(buffer);
}
```

上記のコードには、以下のようないくつかの問題があります。

* メッセージ全体 (行の終わり) が、`ReadAsync` への 1 回の呼び出しで受信されない場合がある。
* `stream.ReadAsync` の結果が無視される。 `stream.ReadAsync` で、読み取られたデータの量が返される。
* 複数の行が 1 回の `ReadAsync` 呼び出しで読み取られるケースが処理されない。
* 読み取りごとに `byte` 配列が割り当てられる。

上記の問題を解決するには、次の変更が必要です。

* 改行が検出されるまで、受信データをバッファーに格納します。
* バッファーで返されたすべての行を解析します。
* 行が 1 KB (1024 バイト) より大きい可能性があります。 コードでは、バッファー内の完全な行に収まるように、区切り記号が見つかるまで入力バッファーのサイズを変更する必要があります。

  * バッファーのサイズが変更された場合、入力により長い行が表示されると、より多くのバッファー コピーが作成されます。
  * 無駄な領域を減らすには、行の読み取りに使用されるバッファーを小さくします。

* メモリが繰り返し割り当てられないようにするために、バッファー プーリングの使用を検討します。
* 次のコードでは、これらの問題の一部に対処します。

[!code-csharp[](~/samples/snippets/csharp/pipelines/ProcessLinesAsync.cs?name=snippet)]

上記のコードは複雑で、特定されたすべての問題には対処していません。 ハイ パフォーマンス ネットワークは、通常、パフォーマンスを最大化するために非常に複雑なコードを記述することを意味します。 `System.IO.Pipelines` は、この種のコードをより簡単に記述できるように設計されています。

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="pipe"></a>パイプ

<xref:System.IO.Pipelines.Pipe> クラスを使用して、`PipeWriter/PipeReader` ペアを作成できます。 `PipeWriter` に書き込まれたすべてのデータは、`PipeReader` で利用できます。

[!code-csharp[](~/samples/snippets/csharp/pipelines/Pipe.cs?name=snippet2)]

<a name="pbu"></a>

### <a name="pipe-basic-usage"></a>パイプの基本的な使用方法

[!code-csharp[](~/samples/snippets/csharp/pipelines/Pipe.cs?name=snippet)]

次の 2 つのループがあります。

* `FillPipeAsync` では `Socket` から読み取り、`PipeWriter` に書き込みます。
* `ReadPipeAsync` では `PipeReader` から読み取り、受信行を解析します。

明示的なバッファーは割り当てられていません。 すべてのバッファー管理は、`PipeReader` と `PipeWriter` の実装に委任されます。 バッファー管理を委任することで、ビジネス ロジックのみに焦点を当てるコードがより簡単に消費されるようになります。

最初のループでは、次のようになります。

* <xref:System.IO.Pipelines.PipeWriter.GetMemory(System.Int32)?displayProperty=nameWithType> は、基になるライターからメモリを取得するために呼び出されます。
* <xref:System.IO.Pipelines.PipeWriter.Advance(System.Int32)?displayProperty=nameWithType> は、バッファーに書き込まれたデータの量を `PipeWriter` に示すために呼び出されます。
* <xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A?displayProperty=nameWithType> は、データを `PipeReader` で使用できるようにするために呼び出されます。

2 番目のループでは、`PipeWriter` によって書き込まれたバッファーが `PipeReader` で消費されます。 バッファーはソケットから取得されます。 `PipeReader.ReadAsync` の呼び出しでは、次のようになります。

* 次の 2 つの重要な情報を含む、<xref:System.IO.Pipelines.ReadResult> を返します。

  * `ReadOnlySequence<byte>` の形式で読み取られたデータ。
  * データの終わり (EOF) に到達したかどうかを示すブール値 `IsCompleted`。

行の終わり (EOL) の区切り記号が検出され、行が解析された後は、次のようになります。

* ロジックでバッファーが処理され、既に処理されているものがスキップされます。
* `PipeReader.AdvanceTo` は、どのくらいの量のデータが使用され、検査されたかを `PipeReader` に示すために呼び出されます。

リーダーとライターのループは、`Complete` を呼び出すことによって終了します。 `Complete` は、基になるパイプで割り当てられたメモリを解放できるようにします。

### <a name="backpressure-and-flow-control"></a>バックプレッシャとフロー制御

読み取りと解析を連動させるのが理想的です。

* 書き込みスレッドでは、ネットワークからのデータを消費し、それをバッファーに格納します。
* 解析スレッドでは、適切なデータ構造の構築を担当します。

通常、解析には、ネットワークからデータのブロックをコピーするだけの場合より、時間がかかります。

* 読み取りスレッドは解析スレッドより前になります。
* 読み取りスレッドの速度を落とすか、解析スレッド用のデータを格納するためにより多くのメモリを割り当てる必要があります。

最適なパフォーマンスが得られるように、頻繁な一時停止間のバランスが保たれ、より多くのメモリが割り当てられます。

この問題を解決するために、`Pipe` には、データのフローを制御するための次の 2 つの設定があります。

* <xref:System.IO.Pipelines.PipeOptions.PauseWriterThreshold>:<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A> の呼び出しが一時停止する前に、バッファーに格納する必要があるデータ量を判別します。
* <xref:System.IO.Pipelines.PipeOptions.ResumeWriterThreshold>:`PipeWriter.FlushAsync` の呼び出しが再開される前に、リーダーが監視する必要があるデータの量を判別します。

![ResumeWriterThreshold と PauseWriterThreshold を含む図](./media/pipelines/resume-pause.png)

<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A?displayProperty=nameWithType>:

* `Pipe` のデータ量が `PauseWriterThreshold` を超えたときに、不完全な `ValueTask<FlushResult>` を返します。
* `ResumeWriterThreshold` より低くなったときに、`ValueTask<FlushResult>` を完了します。

2 つの値は、1 つの値が使用された場合に発生する可能性がある、急速な循環を防ぐために使用されます。

### <a name="examples"></a>使用例

```csharp
// The Pipe will start returning incomplete tasks from FlushAsync until
// the reader examines at least 5 bytes.
var options = new PipeOptions(pauseWriterThreshold: 10, resumeWriterThreshold: 5);
var pipe = new Pipe(options);
```

### <a name="pipescheduler"></a>PipeScheduler

通常、`async` および `await` を使用する場合、非同期コードは <xref:System.Threading.Tasks.TaskScheduler> または現在の <xref:System.Threading.SynchronizationContext> で再開されます。

I/O を行う場合は、I/O が実行される場所をきめ細かく制御することが重要です。 この制御により、CPU キャッシュを効果的に利用できます。 効率的なキャッシュは、Web サーバーなどのハイ パフォーマンス アプリに不可欠です。 <xref:System.IO.Pipelines.PipeScheduler> では、非同期コールバックが実行される場所を制御できます。 既定では:

* 現在の <xref:System.Threading.SynchronizationContext> が使用されます。
* `SynchronizationContext` がない場合は、スレッド プールを使用してコールバックを実行します。

[!code-csharp[](~/samples/snippets/csharp/pipelines/Program.cs?name=snippet)]

[PipeScheduler.ThreadPool](xref:System.IO.Pipelines.PipeScheduler.ThreadPool) は、スレッド プールへのコールバックをキューに登録する <xref:System.IO.Pipelines.PipeScheduler> の実装です。 `PipeScheduler.ThreadPool` は既定値であり、一般的に最適な選択肢です。 [PipeScheduler.Inline](xref:System.IO.Pipelines.PipeScheduler.Inline) を使用すると、デッドロックなどの意図しない結果となる可能性があります。

### <a name="pipe-reset"></a>パイプのリセット

多くの場合、`Pipe` オブジェクトを再利用するのが効率的です。 パイプをリセットするには、`PipeReader` と `PipeWriter` の両方が完了したときに <xref:System.IO.Pipelines.PipeReader> <xref:System.IO.Pipelines.Pipe.Reset%2A> を呼び出します。

## <a name="pipereader"></a>PipeReader

<xref:System.IO.Pipelines.PipeReader> では、呼び出し元の代わりにメモリを管理します。 <xref:System.IO.Pipelines.PipeReader.ReadAsync%2A?displayProperty=nameWithType> を呼び出した後に、**常に**<xref:System.IO.Pipelines.PipeReader.AdvanceTo%2A?displayProperty=nameWithType> を呼び出します。 これにより、`PipeReader` では、呼び出し元がメモリを使用して実行されるタイミングを把握し、追跡できるようになります。 `PipeReader.ReadAsync` から返された `ReadOnlySequence<byte>` が有効なのは、`PipeReader.AdvanceTo` が呼び出されるまでのみとなります。 `PipeReader.AdvanceTo` を呼び出した後、`ReadOnlySequence<byte>` を使用することはできません。

`PipeReader.AdvanceTo` では、次の 2 つの <xref:System.SequencePosition> 引数を受け取ります。

* 最初の引数では、消費されたメモリの量を判別します。
* 2 番目の引数では、監視されたバッファーの量を判別します。

データを消費済みとしてマークすることは、パイプでメモリを基になるバッファープ ールに返すことができることを意味します。 データを監視済みとしてマークすると、`PipeReader.ReadAsync` の次の呼び出しで行われる内容が制御されます。 すべてを監視済みとしてマークすることは、パイプにデータがさらに書き込まれるまで、`PipeReader.ReadAsync` の次の呼び出しでは何も返されないことを意味します。 それ以外の値を指定すると、`PipeReader.ReadAsync` の次の呼び出しで、監視されたデータ*と* 監視されていないデータがすぐに返されますが、既に消費されているデータは除きます。

### <a name="read-streaming-data-scenarios"></a>ストリーミング データの読み取りシナリオ

ストリーミング データを読み取ろうとすると発生する、一般的なパターンがいくつかあります。

* 指定したデータのストリームで、1 つのメッセージを解析する。
* 指定したデータのストリームで、使用可能なメッセージをすべて解析する。

次の例では、`ReadOnlySequence<byte>` からのメッセージを解析するために `TryParseMessage` メソッドを使用しています。 `TryParseMessage`では 1 つのメッセージを解析し、入力バッファーを更新して、解析されたメッセージをバッファーからトリミングします。 `TryParseMessage` は .NET の一部ではありません。これは、次のセクションで使用されるユーザーが記述するメソッドです。

```csharp
bool TryParseMessage(ref ReadOnlySequence<byte> buffer, out Message message);
```

### <a name="read-a-single-message"></a>1 つのメッセージを読み取る

次のコードでは、`PipeReader` からの 1 つのメッセージを読み取り、呼び出し元に返します。

[!code-csharp[ReadSingleMsg](~/samples/snippets/csharp/pipelines/ReadSingleMsg.cs?name=snippet)]

上記のコードでは次の操作が行われます。

* 1 つのメッセージを解析します。
* 消費された `SequencePosition` と検査された `SequencePosition` を更新し、トリミングされた入力バッファーの先頭を指すようにします。

2 つの `SequencePosition` 引数が更新されます。これは、`TryParseMessage` で解析されたメッセージが入力バッファーから削除されるためです。 一般に、バッファーからの 1 つのメッセージを解析する場合、検査位置は次のいずれかである必要があります。

* メッセージの終わり。
* メッセージが検出されなかった場合は、受信されたバッファーの終わり。

1 つのメッセージ ケースでは、エラーが発生する可能性が最も高くなります。 *examined* に間違った値を渡すと、メモリ不足の例外または無限ループが発生する可能性があります。 詳細については、この記事の「[PipeReader の一般的な問題](#gotchas)」セクションを参照してください。

### <a name="reading-multiple-messages"></a>複数のメッセージの読み取り

次のコードでは、`PipeReader` からのすべてのメッセージを読み取り、それぞれに対して `ProcessMessageAsync` を呼び出します。

[!code-csharp[MyConnection1](~/samples/snippets/csharp/pipelines/MyConnection1.cs?name=snippet)]

### <a name="cancellation"></a>キャンセル

`PipeReader.ReadAsync`:

* <xref:System.Threading.CancellationToken> を渡すことをサポートします。
* 読み取りが保留中のときに `CancellationToken` が取り消された場合は、<xref:System.OperationCanceledException> をスローします。
* <xref:System.IO.Pipelines.PipeReader.CancelPendingRead%2A?displayProperty=nameWithType> を使用して現在の読み取り操作を取り消す方法をサポートします。これにより、例外の発生を回避できます。 `PipeReader.CancelPendingRead` を呼び出すと、`PipeReader.ReadAsync` の現在の呼び出しまたは次の呼び出しで、`IsCanceled` が `true` に設定された <xref:System.IO.Pipelines.ReadResult> が返されます。 これは、既存の読み取りループを非破壊的で非例外的な方法で停止する際に役立つ場合があります。

[!code-csharp[MyConnection](~/samples/snippets/csharp/pipelines/MyConnection.cs?name=snippet)]

<a name="gotchas"></a>

### <a name="pipereader-common-problems"></a>PipeReader の一般的な問題

* `consumed` または `examined` に間違った値を渡すと、既に読み取られているデータが読み取られる場合があります。
* `buffer.End` を検査済みとして渡すと、次のようになる場合があります。

  * データが停止する
  * データが消費されていない場合、最終的にメモリ不足 (OOM) 例外が発生する可能性があります。 たとえば、バッファーから一度に 1 つのメッセージを処理する場合は、`PipeReader.AdvanceTo(position, buffer.End)` です。

* `consumed` または `examined` に間違った値を渡すと、無限ループが発生する可能性があります。 たとえば、`buffer.Start` が変更されていない場合、`PipeReader.AdvanceTo(buffer.Start)` では、新しいデータが到着する直前に `PipeReader.ReadAsync` への次の呼び出しで制御が返されます。
* `consumed` または `examined` に間違った値を渡すと、無限バッファーリング (最終的には OOM) が発生する可能性があります。
* `PipeReader.AdvanceTo` を呼び出した後に `ReadOnlySequence<byte>` を使用すると、メモリが破損する可能性があります (解放後使用)。
* `PipeReader.Complete/CompleteAsync` を呼び出せないと、メモリ リークが発生する可能性があります。
* バッファーを処理する前に <xref:System.IO.Pipelines.ReadResult.IsCompleted?displayProperty=nameWithType> を確認し、読み取りロジックを終了すると、データが失われます。 ループの終了条件は、`ReadResult.Buffer.IsEmpty` および `ReadResult.IsCompleted` に基づいている必要があります。 これを誤って実行すると、無限ループになる可能性があります。

#### <a name="problematic-code"></a>問題のあるコード

❌ **データ損失**

`IsCompleted` が `true` に設定されている場合、`ReadResult` でデータの最終セグメントを返すことができます。 読み取りループを終了する前にデータを読み取らないと、データが失われます。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#1](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **無限ループ**

次のロジックでは、`Result.IsCompleted` が `true` でも、バッファー内に完全なメッセージがない場合は、無限ループが発生する可能性があります。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#2](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet2)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

同じ問題があるもう 1 つのコードを次に示します。 `ReadResult.IsCompleted` を確認する前に、空でないバッファーがあるかどうかが確認されます。 これは `else if` にあるため、バッファー内に完全なメッセージがない場合は、無限にループされます。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#3](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet3)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **予期しないハング**

`examined` の位置で `buffer.End` を使用して `PipeReader.AdvanceTo` を無条件に呼び出すと、1 つのメッセージを解析するときにハングが発生する可能性があります。 `PipeReader.AdvanceTo` の次の呼び出しでは、以下のようになるまで何も返されません。

* パイプにさらにデータが書き込まれている。
* また、新しいデータが以前に検査されていない。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#4](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet4)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **メモリ不足 (OOM)**

次のような状況の場合、以下のコードでは、<xref:System.OutOfMemoryException> が発生するまでバッファーが保持されます。

* 最大メッセージ サイズがない。
* `PipeReader` から返されたデータで、完全なメッセージが作成されない。 たとえば、他の側で大きなメッセージ (4 GB のメッセージなど) を書き込んでいるため、完全なメッセージが作成されていない。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#5](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet5)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **メモリの破損**

バッファーを読み取るヘルパーを記述するときは、返されたすべてのペイロードをコピーしてから `Advance` を呼び出す必要があります。 次の例では、`Pipe` で破棄されたメモリを返し、次の操作 (読み取り/書き込み) でそれを再利用する可能性があります。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

[!code-csharp[DoNotUse#Message](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippetMessage)]

[!code-csharp[DoNotUse#6](~/samples/snippets/csharp/pipelines/DoNotUse.cs?name=snippet6)]

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

## <a name="pipewriter"></a>PipeWriter

<xref:System.IO.Pipelines.PipeWriter> では、呼び出し元の代わりに書き込むバッファーを管理します。 `PipeWriter` では、[`IBufferWriter<byte>`](xref:System.Buffers.IBufferWriter%601) を実装します。 `IBufferWriter<byte>` は、バッファーのコピーを追加せずに、書き込みを実行するためにバッファーにアクセスできるようにします。

[!code-csharp[MyPipeWriter](~/samples/snippets/csharp/pipelines/MyPipeWriter.cs?name=snippet)]

上記のコードでは、次のようになります。

* <xref:System.IO.Pipelines.PipeWriter.GetMemory%2A> を使用して、`PipeWriter` から少なくとも 5 バイトのバッファーを要求します。
* ASCII 文字列である `"Hello"` のバイトを、返された `Memory<byte>` に書き込みます。
* <xref:System.IO.Pipelines.PipeWriter.Advance%2A> を呼び出して、バッファーに書き込まれたバイト数を示します。
* 基になるデバイスにバイトを送信する、`PipeWriter` をフラッシュします。

上記の書き込みメソッドでは、`PipeWriter` によって提供されるバッファーが使用されています。 あるいは、<xref:System.IO.Pipelines.PipeWriter.WriteAsync%2A?displayProperty=nameWithType> では次のようになります。

* 既存のバッファーを `PipeWriter` にコピーします。
* `GetSpan` を呼び出し、必要に応じて `Advance` を呼び出してから、<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A> を呼び出します。

[!code-csharp[MyPipeWriter#2](~/samples/snippets/csharp/pipelines/MyPipeWriter.cs?name=snippet2)]

### <a name="cancellation"></a>キャンセル

<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A> では、<xref:System.Threading.CancellationToken> を渡すことがサポートされます。 `CancellationToken` を渡すと、フラッシュの保留中にトークンが取り消された場合に、`OperationCanceledException` となります。 `PipeWriter.FlushAsync` では、例外を発生させることなく、<xref:System.IO.Pipelines.PipeWriter.CancelPendingFlush%2A?displayProperty=nameWithType> を使用して、現在のフラッシュ操作を取り消す方法がサポートされます。 `PipeWriter.CancelPendingFlush` を呼び出すと、`PipeWriter.FlushAsync` または `PipeWriter.WriteAsync` の現在の呼び出しあるいは次の呼び出しで、`IsCanceled` が `true` に設定された <xref:System.IO.Pipelines.FlushResult> が返されます。 これは、非破壊的で非例外的な方法で生成されるフラッシュを停止する際に役立つ場合があります。

<a name="pwcp"></a>

### <a name="pipewriter-common-problems"></a>PipeWriter の一般的な問題

* <xref:System.IO.Pipelines.PipeWriter.GetSpan%2A> および <xref:System.IO.Pipelines.PipeWriter.GetMemory%2A> では、少なくとも要求された量のメモリを持つバッファーを返します。 正確なバッファー サイズを想定**しないでください**。
* 連続する呼び出しで同じバッファーまたは同じサイズのバッファーが返される保証はありません。
* さらにデータの書き込みを続行するには、<xref:System.IO.Pipelines.PipeWriter.Advance%2A> を呼び出した後に新しいバッファーを要求する必要があります。 以前に取得したバッファーに書き込むことはできません。
* `FlushAsync` の呼び出しが不完全な場合に `GetMemory` または `GetSpan` を呼び出すのは安全ではありません。
* フラッシュされていないデータがある場合に `Complete` または `CompleteAsync` を呼び出すと、メモリが破損する可能性があります。

## <a name="iduplexpipe"></a>IDuplexPipe

<xref:System.IO.Pipelines.IDuplexPipe> は、読み取りと書き込みの両方をサポートする種類のコントラクトです。 たとえば、ネットワーク接続は `IDuplexPipe` によって表されます。

 `PipeReader` と `PipeWriter` を含む `Pipe` とは異なり、`IDuplexPipe` は全二重接続の片側を表します。 つまり、`PipeWriter` に書き込まれるものは、`PipeReader` からは読み取られません。

## <a name="streams"></a>ストリーム

ストリーム データの読み取りまたは書き込みを行う場合、通常は、デシリアライザーを使用してデータを読み取り、シリアライザーを使用してデータを書き込みます。 これらの読み取りおよび書き込みストリーム API のほとんどに、`Stream` パラメーターがあります。 これらの既存の API との統合をより容易にするために、`PipeReader` および `PipeWriter` で <xref:System.IO.Pipelines.PipeReader.AsStream%2A> が公開されます。  <xref:System.IO.Pipelines.PipeWriter.AsStream%2A> では、`PipeReader` または `PipeWriter` に関する `Stream` 実装を返します。
