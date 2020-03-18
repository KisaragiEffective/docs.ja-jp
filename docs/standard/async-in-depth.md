---
title: 非同期の詳細
description: .NET のタスクベース非同期モデルを使用して、I/O バインドおよび CPU バインドの非同期コードを簡単に記述する方法について説明します。
author: cartermp
ms.author: wiwagn
ms.date: 06/20/2016
ms.technology: dotnet-standard
ms.assetid: 1e38f9d9-8f84-46ee-a15f-199aec4f2e34
ms.openlocfilehash: 91fd37ce329c03b43b5472e4579be7f5ef961738
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70169109"
---
# <a name="async-in-depth"></a>非同期の詳細

I/O および CPU バインドの非同期コードは、.NET のタスクベース非同期モデルを使用して簡単に記述できます。 C# と Visual Basic では、このモデルは `Task` および `Task<T>` 型と、`async` および `await` キーワードによって公開されます (言語固有のリソースについては、「[関連項目](#see-also)」セクションを参照してください)。この記事では、.NET 非同期を使用する方法について説明し、背後で使用される非同期フレームワークを把握するための情報を示します。

## <a name="task-and-taskt"></a>Task と Task\<T>

Task は、[Promise Model of Concurrency](https://en.wikipedia.org/wiki/Futures_and_promises) (並行性の Promise 型モデル) として知られるモデルを実装するために使用される構成体です。  つまり、今後のある時点で作業が完了することを "約束" し、クリーン API を使用した約束の調整を可能にします。

- `Task` は、値を返さない 1 回の操作を表します。
- `Task<T>` は、`T` 型の値を返す 1 回の操作を表します。

作業の抽象化は非同期に発生し、スレッド処理の抽象化では*ない*ため、タスクについて判断することが重要です。 既定では、タスクは現在のスレッドで実行され、必要に応じてオペレーティング システムに作業を委任します。 必要に応じて、`Task.Run` API を使用して、タスクを別のスレッドで実行することを明示的に要求できます。

タスクは、タスクの監視、待機、結果値へのアクセス (`Task<T>` の場合) のための API プロトコルを公開しています。 `await` キーワードによる言語統合は、タスクを使用するための高度なレベルの抽象化を提供します。

`await` を使用すると、アプリケーションまたはサービスで、タスク完了まで呼び出し元に制御を渡すことによって、タスクの実行中に有用な作業を実行できます。 コードは、タスク完了後に実行を続けるために、コールバックまたはイベントに依存する必要はありません。 言語とタスクの API 統合によって処理されます。 `Task<T>` を使用する場合、`await` キーワードはさらに、タスク完了時に返された値を "ラップ解除" します。  この動作の詳細について次に詳しく説明します。

タスクと、タスクを処理するさまざまな方法については、[タスクベースの非同期パターン (TAP) に関するトピック](./asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)を参照してください。

## <a name="deeper-dive-into-tasks-for-an-io-bound-operation"></a>I/O バインド操作に関するタスクの詳細

次のセクションでは、一般的な非同期 I/O 呼び出しで実行される処理の概要について説明します。 はじめに例を 2 つ示します。

最初の例では、非同期メソッドを呼び出し、未完了のアクティブなタスクを返します。

```csharp
public Task<string> GetHtmlAsync()
{
    // Execution is synchronous here
    var client = new HttpClient();

    return client.GetStringAsync("https://www.dotnetfoundation.org");
}
```

2 番目の例では、タスクに対する処理に `async` キーワードと `await` キーワードを追加で使用しています。

```csharp
public async Task<string> GetFirstCharactersCountAsync(string url, int count)
{
    // Execution is synchronous here
    var client = new HttpClient();

    // Execution of GetFirstCharactersCountAsync() is yielded to the caller here
    // GetStringAsync returns a Task<string>, which is *awaited*
    var page = await client.GetStringAsync("https://www.dotnetfoundation.org");

    // Execution resumes when the client.GetStringAsync task completes,
    // becoming synchronous again.

    if (count > page.Length)
    {
        return page;
    }
    else
    {
        return page.Substring(0, count);
    }
}
```

`GetStringAsync()` を呼び出すと、ネイティブ ネットワーク ライブラリへの P/Invoke interop 呼び出しに到達するまで、下位レベルの .NET ライブラリ (おそらく他の非同期メソッドを呼び出す) を介して呼び出しが行われます。 ネイティブ ライブラリはその後、システム API 呼び出し (Linux のソケットへの `write()` など) を実行することがあります。 ネイティブまたはマネージ境界にタスク オブジェクトが作成されます。作成には [TaskCompletionSource](xref:System.Threading.Tasks.TaskCompletionSource%601.SetResult(%600)) が使用される可能性があります。 タスク オブジェクトは上位レイヤーに渡され、操作が実行される場合もあれば直接返される場合もありますが、最終的に最初の呼び出し元に戻されます。

上記の 2 番目の例で、`Task<T>` オブジェクトが `GetStringAsync` から返されます。 `await` キーワードを使用すると、メソッドは新しく作成したタスク オブジェクトを返します。 `GetFirstCharactersCountAsync` メソッドのこの場所から呼び出し元に制御が戻ります。 [Task&lt;T&gt;](xref:System.Threading.Tasks.Task%601) オブジェクトのメソッドとプロパティを使用すると、呼び出し元はタスクの進行状況を監視できます。タスクは、GetFirstCharactersCountAsync の残りのコードが実行されれば完了します。

システム API 呼び出しの後、要求はカーネル領域に入り、OS のネットワーク サブシステム (Linux カーネルの `/net` など) に移ります。  ここで、OS はネットワーク要求を*非同期的に*処理します。  詳細は使用する OS によって異なる場合がありますが (デバイス ドライバーの呼び出しが、ランタイムに返送されるシグナルとしてスケジュールされている場合や、デバイス ドライバー呼び出しが行われた*後*にシグナルが返送される場合があります)、最終的にはネットワーク要求が進行中であることがランタイムに通知されます。  この時点で、デバイス ドライバーに関する処理はスケジュール済み、進行中、または既に完了済み (要求が既に "接続" されていない状態) のいずれかですが、すべてが非同期に発生するため、デバイス ドライバーはすぐに別の処理を実行できます。

たとえば、Windows で OS スレッドからネットワーク デバイス ドライバーに呼び出しを行い、操作を表す割り込み要求パケット (IRP) を通じてネットワーク操作の実行を依頼します。  デバイス ドライバーは IRP を受信し、ネットワークへの呼び出しを行い、IRP を "保留中" とマークして OS に戻ります。  OS スレッドは IRP が "保留中" であることがわかっているため、このジョブに対してそれ以上行う処理はなく、"戻る" ことによって他の作業を実行できるようになります。

要求が満たされ、データがデバイス ドライバーに戻ると、新しいデータの受信が割り込みを介して CPU に通知されます。  この割り込みが処理される方法は OS によって異なりますが、最終的にデータは OS に渡されてシステム interop 呼び出しに到達します (たとえば、Linux では割り込みハンドラーが IRQ の残り半分をスケジュールし、データを OS に非同期に渡します)。  これも*非同期に*行われることにご注意ください。  結果は、次の使用可能なスレッドが非同期メソッドを実行して完了したタスクの結果を "ラップ解除" できるようになるまで、キューに置かれます。

このプロセス全体を通じて、重要なことは、**タスクを実行する専用のスレッドは存在しない**ということです。  作業は何らかのコンテキストで実行されますが (つまり、OS はデバイス ドライバーにデータを渡し、割り込みに応答する必要があります)、要求からデータが戻るのを*待機*する専用のスレッドは存在しません。  そのため、システムは I/O 呼び出しの完了を待機するのではなく、より大量の作業を処理できます。

実測時間で考えると、上記の処理には実行する作業が多いように見えるかもしれませんが、実際の I/O 作業の実行にかかる時間に比較すればわずかです。 まったくの目安ですが、このような呼び出しの潜在的なタイムラインは次のようなります。

0-1————————————————————————————————————————————————–2-3

- `0` の時点から `1` の時点までは、非同期メソッドが呼び出し元に制御を渡すまでにかかった時間です。
- `1` の時点から `2` の時点までは、CPU に負荷をかけずに I/O に費やされた時間です。
- 最後に、`2` の時点から `3` の時点までは、非同期メソッドに制御 (および場合によっては値) を戻すまでの時間です。この時点でそのメソッドは再度実行されます。

### <a name="what-does-this-mean-for-a-server-scenario"></a>サーバー側のシナリオ

このモデルは、標準的なサーバー シナリオのワークロードで問題なく動作します。  未完了のタスクでのブロッキング専用のスレッドがないため、サーバー スレッドプールでより大量の Web 要求の処理ができます。

2 種類のサーバーについて考えます。非同期コードを実行しているサーバーと、実行していないサーバーです。  この例では、各サーバーには、サービス要求に使用できるスレッドが 5 つだけあります。  これは非現実的な少ない数であり、デモの目的に限って使用していることに注意してください。

どちらのサーバーも 6 件の同時要求を受信したとします。 各要求で I/O 操作を 1 回実行します。  非同期コードを*使用しない*サーバーは、5 つのスレッドのいずれかが I/O バインドの作業を完了して応答を書き込むまで、6 番目の要求をキューに入れておく必要があります。 サーバーに 20 番目の要求が到着すると、キューが長くなりすぎるため、サーバー速度が低下し始める可能性があります。

非同期コードを*実行している*サーバーでも、6 番目の要求はキューに入れられますが、`async` と `await` を使用しているため、各スレッドは I/O バインドの作業が完了した時点ではなく、開始した時点で解放されます。  20 番目の要求が到着する時点までに受信要求のキューは大幅に短くなり (キューに何らかの要求が格納されている場合)、サーバーの速度低下は発生しません。

これは架空の例ですが、実際の動作にとても似ています。  実際には、`async` と `await` を使用すると、受信する要求ごとにスレッドを専用にした場合に比べ、1 桁以上多い数の要求をサーバーで処理できると期待できます。

### <a name="what-does-this-mean-for-client-scenario"></a>クライアント側のシナリオ

クライアント アプリに `async` と `await` を使用する最大のメリットは、応答性の向上です。  スレッドを手動で作成することによって、アプリの応答性を高めることができます。スレッドを作成することは、単に `async` と `await` を使用する場合に比べ、相対的に負荷の高い操作です。  モバイル ゲームなどの場合は特に、I/O に関して UI スレッドへの影響をできるだけ少なくすることが重要です。

さらに重要なことに、I/O バインドの作業に費やされる CPU 時間が実質的にゼロであるため、ほとんど有用性のない作業の実行に CPU スレッド全体を独占的に使用することは、リソースの使用方法として不適切です。

さらに、UI スレッドへの作業の処理依頼 (UI の更新など) は、`async` メソッドを使用すればとても簡単で、余分な作業 (スレッドセーフ デリゲートの呼び出しなど) は必要ありません。

## <a name="deeper-dive-into-task-and-taskt-for-a-cpu-bound-operation"></a>CPU バインド操作の Task と Task\<T> の詳細

CPU バインドの `async` コードは、I/O バインドの `async` コードとは少し異なります。  作業は CPU で実行されるため、スレッドを計算専用にすることを避ける方法はありません。  `async` と `await` を使用すると、バックグラウンドのスレッドをクリーンな方法で使用でき、非同期メソッドの呼び出し元の応答性を維持できます。  これにより共有データに対する保護は提供されないことに注意してください。  共有データを使用している場合は、適切な同期戦略を適用する必要があります。

CPU バインドの非同期呼び出しの概要を次に示します。

```csharp
public async Task<int> CalculateResult(InputData data)
{
    // This queues up the work on the threadpool.
    var expensiveResultTask = Task.Run(() => DoExpensiveCalculation(data));

    // Note that at this point, you can do some other work concurrently,
    // as CalculateResult() is still executing!

    // Execution of CalculateResult is yielded here!
    var result = await expensiveResultTask;

    return result;
}
```

`CalculateResult()` が、呼び出されたスレッドで実行されます。  `Task.Run` の呼び出し時に、負荷の高い CPU バインド操作 `DoExpensiveCalculation()` をスレッドプールでキューに入れ、`Task<int>` ハンドルを受け取ります。  `DoExpensiveCalculation()` は最終的に次の使用可能なスレッドで (通常は別の CPU コアで) 同時に実行されます。  `CalculateResult()` を呼び出したスレッドがまだ実行中であるために、`DoExpensiveCalculation()` が別のスレッドでビジー状態のときに、同時処理を行うことができます。

`await` が検出されると、`CalculateResult()` の実行は呼び出し元に任され、`DoExpensiveCalculation()` が結果を生成している間に、現在のスレッドで他の作業を実行できます。  その作業が完了すると、結果はキューに入れられ、メイン スレッドで実行されます。  最終的に、メイン スレッドが `CalculateResult()` の実行に戻り、その時点で `DoExpensiveCalculation()` の結果が得られます。

### <a name="why-does-async-help-here"></a>async が役に立つ理由

`async` と `await` は、応答性を必要とする場合に CPU バインドの作業を管理する際のベスト プラクティスです。 CPU バインドの作業で async を使用するには、複数のパターンがあります。 async を使用すると小さいながらも負荷がかかるため、厳密なループ処理にはお勧めしません。  この新しい機能を利用してコードを記述するかどうかは、ユーザーの判断に任されます。

## <a name="see-also"></a>関連項目

- [C# の非同期プログラミング](../csharp/async.md)
- [Async および Await を使用した非同期プログラミング (C#)](../csharp/programming-guide/concepts/async/index.md)
- [F# の非同期プログラミング](../fsharp/tutorials/asynchronous-and-concurrent-programming/async.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../visual-basic/programming-guide/concepts/async/index.md)
