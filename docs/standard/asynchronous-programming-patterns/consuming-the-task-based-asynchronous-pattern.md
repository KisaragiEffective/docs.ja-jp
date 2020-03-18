---
title: タスク ベースの非同期パターンの利用
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- .NET Framework, and TAP
- asynchronous design patterns, .NET Framework
- TAP, .NET Framework support for
- Task-based Asynchronous Pattern, .NET Framework support for
- .NET Framework, asynchronous design patterns
ms.assetid: 033cf871-ae24-433d-8939-7a3793e547bf
ms.openlocfilehash: f80e6ae520ab03c0f5f4edc30c0b7102193ee6c5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139814"
---
# <a name="consuming-the-task-based-asynchronous-pattern"></a>タスク ベースの非同期パターンの利用

タスク ベースの非同期パターン (TAP) を使用して非同期操作を行うと、コールバックを使用して、ブロックすることなく待機できます。  タスクの場合、これは <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> などのメソッドによって行われます。 言語ベースの非同期サポートが、通常の制御フロー内での非同期操作の待機を許可することで、コールバックを隠し、コンパイラにより生成されたコードはこの同じ API レベルのサポートを提供します。

## <a name="suspending-execution-with-await"></a>Await による実行の中断
 .NET Framework 4.5 以降では、C# の [await](../../csharp/language-reference/operators/await.md) キーワードおよび Visual Basic の [Await 演算子](../../visual-basic/language-reference/operators/await-operator.md)を使用して <xref:System.Threading.Tasks.Task> と <xref:System.Threading.Tasks.Task%601> オブジェクトを非同期に待機できます。 <xref:System.Threading.Tasks.Task> を待っているとき、`await` 式は型 `void` になります。 <xref:System.Threading.Tasks.Task%601> を待っているとき、`await` 式は型 `TResult` になります。 `await` 式は、非同期メソッドの本体内に含める必要があります。 .NET Framework 4.5 における C# および Visual Basic の言語サポートの詳細については、C# および Visual Basic の言語仕様を参照してください。

 待機機能は、隠れた状態で継続を使用してタスクにコールバックをインストールします。  このコールバックは中断ポイントから非同期メソッドを再開します。 非同期メソッドが再開され、待機していた操作が正常に完了し、<xref:System.Threading.Tasks.Task%601> であった場合に、その `TResult` が返されます。  待っていた <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> が <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態で終わった場合、<xref:System.OperationCanceledException> 例外がスローされます。  待っていた <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> が <xref:System.Threading.Tasks.TaskStatus.Faulted> 状態で終わった場合、エラーの原因となった例外がスローされます。 `Task` は複数の例外の結果としてエラーになることがありますが、反映されるのはこれらの例外の中の 1 つのみです。 ただし、<xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> プロパティは、すべてのエラーが含まれる <xref:System.AggregateException> 例外を返します。

 中断時に非同期メソッドを実行したスレッドに同期コンテキスト (<xref:System.Threading.SynchronizationContext> オブジェクト) が関連付けられている場合 (<xref:System.Threading.SynchronizationContext.Current%2A?displayProperty=nameWithType> プロパティが `null` ではないなど)、非同期メソッドは、コンテキストの <xref:System.Threading.SynchronizationContext.Post%2A> メソッドを利用し、その同じ同期コンテキストで再開します。 そのように関連付けられていない場合、中断時に実行されていたタスク スケジューラ (<xref:System.Threading.Tasks.TaskScheduler> オブジェクト) に基づきます。 通常、これは既定のタスク スケジューラ (<xref:System.Threading.Tasks.TaskScheduler.Default%2A?displayProperty=nameWithType>) であり、スレッド プールをターゲットにします。 このタスク スケジューラによって、待機していた非同期操作が完了した場合に再開するかどうか、または再開のスケジュールを設定するかどうかが決定されます。 既定のスケジューラは、通常、待機していた操作が完了したスレッド上での実行の継続を許可します。

 非同期メソッドが呼び出された場合、まだ完了していない待機可能なインスタンスの最初の await 式まで、関数の本体を同期をとって実行し、この時点で呼び出しを呼び出し元に返します。 非同期メソッドが `void` を返さない場合、進行中の計算を表す <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> オブジェクトが返されます。 void 以外の非同期メソッドでは、return ステートメントが検出された場合、またはメソッド本体の末尾に到達した場合、タスクが <xref:System.Threading.Tasks.TaskStatus.RanToCompletion> の終了状態で完了します。 ハンドルされない例外により、非同期メソッドの本体から制御が離れた場合、タスクは <xref:System.Threading.Tasks.TaskStatus.Faulted> 状態になります。 その例外が <xref:System.OperationCanceledException> の場合、タスクは代わりに <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態で終わります。 この方法では、結果または例外が最終的に発行されます。

 この動作には、いくつかの重要なバリエーションがあります。  タスクが待機されるまでに既に完了していた場合は、パフォーマンス上の理由から、制御が明け渡されず、関数が実行を継続します。  また、元のコンテキストに戻ることが必ずしも望ましい動作ではない場合は変更されることがあります。これは、次のセクションで詳しく説明します。

### <a name="configuring-suspension-and-resumption-with-yield-and-configureawait"></a>Yield と ConfigureAwait を使用して中断と再開を構成する
 一部のメソッドでは、非同期メソッドの実行をより詳細に制御できます。 たとえば、<xref:System.Threading.Tasks.Task.Yield%2A?displayProperty=nameWithType> メソッドを使用し、yield ポイントを非同期メソッドに伝えることができます。

```csharp
public class Task : …
{
    public static YieldAwaitable Yield();
    …
}
```

 これは、非同期にポストするか、現在のコンテキストにスケジュールを戻すことと同じです。

```csharp
Task.Run(async delegate
{
    for(int i=0; i<1000000; i++)
    {
        await Task.Yield(); // fork the continuation into a separate work item
        ...
    }
});
```

 <xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> メソッドを使用することもできます。非同期メソッドでの中断と再開でより優れた制御機能が与えられます。  再開時に非同期メソッドの継続を呼び出すために非同期メソッドが中断され、そのキャプチャされたコンテキストが使用されている場合、既定では、既に説明したように現在のコンテキストがキャプチャされます。  これは、多くの場合に求められる動作です。  その他の場合では、継続のコンテキストが重要でないこともあり、元のコンテキストへのポスト バックを回避することでパフォーマンスを向上できます。  そのためには、<xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> メソッドを使用して、待機操作にコンテキストをキャプチャして再開するのではなく、待機していた非同期操作がどこで完了したかを問わず、実行を継続するように指示できます。

```csharp
await someTask.ConfigureAwait(continueOnCapturedContext:false);
```

## <a name="canceling-an-asynchronous-operation"></a>非同期操作の取り消し
 .NET Framework 4 以降では、取り消しをサポートする TAP メソッドによって、取り消しトークン (<xref:System.Threading.CancellationToken> オブジェクト) を受け取るオーバーロードが少なくとも 1 つ提供されます。

 キャンセル トークンは、キャンセル トークンのソース (<xref:System.Threading.CancellationTokenSource> オブジェクト) によって作成されます。  ソースの <xref:System.Threading.CancellationTokenSource.Token%2A> プロパティがキャンセル トークンを返します。ソースの <xref:System.Threading.CancellationTokenSource.Cancel%2A> メソッドが呼び出されたとき、このトークンに信号が送られます。  たとえば、単一の Web ページをダウンロードする際に操作を取り消せるようにする場合は、<xref:System.Threading.CancellationTokenSource> オブジェクトを作成し、TAP メソッドにそのオブジェクトのトークンを渡し、操作を取り消す準備ができたらソースの <xref:System.Threading.CancellationTokenSource.Cancel%2A> メソッドを呼び出します。

```csharp
var cts = new CancellationTokenSource();
string result = await DownloadStringAsync(url, cts.Token);
… // at some point later, potentially on another thread
cts.Cancel();
```

 複数の非同期呼び出しを取り消す場合は、すべての呼び出しに同じトークンを渡すことができます。

```csharp
var cts = new CancellationTokenSource();
    IList<string> results = await Task.WhenAll(from url in urls select DownloadStringAsync(url, cts.Token));
    // at some point later, potentially on another thread
    …
    cts.Cancel();
```

 または、操作の一部を選択して同じトークンを渡すこともできます。

```csharp
var cts = new CancellationTokenSource();
    byte [] data = await DownloadDataAsync(url, cts.Token);
    await SaveToDiskAsync(outputPath, data, CancellationToken.None);
    … // at some point later, potentially on another thread
    cts.Cancel();
```

 取り消し要求は任意のスレッドから開始することができます。

 取り消しが要求されないことを示すために、キャンセル トークンを受け取るすべてのメソッドに <xref:System.Threading.CancellationToken.None%2A?displayProperty=nameWithType> 値を渡すことができます。  これで <xref:System.Threading.CancellationToken.CanBeCanceled%2A?displayProperty=nameWithType> プロパティが `false` を返します。呼び出されたメソッドは適宜最適化できます。  テストのためには、トークンが既に取り消された状態か取り消し不可能な状態で開始するかどうかを示すブール値を受け取るコンストラクターを使用してインスタンス化した、事前に取り消されたキャンセル トークンを渡すこともできます。

 この取り消し方法には、いくつか利点があります。

- 同じキャンセル トークンを、任意の数の同期操作および非同期操作に渡すことができます。

- 同じ取り消し要求を、任意の数のリスナーに渡すことができます。

- 取り消しが要求されるかどうか、およびそれがいつ実行されるかは、非同期 API の開発者が完全に制御します。

- API を使用するコードにより、取り消し要求が反映される非同期呼び出しが選択され、決定されます。

## <a name="monitoring-progress"></a>進行状況の監視
 一部の非同期メソッドは、非同期メソッドに渡される進行状況インターフェイスを通じて進行状況を公開します。  たとえば、テキスト文字列を非同期的にダウンロードし、その過程で完了したダウンロードの割合を含む進行状況の更新を発生させる関数があるとします。  このようなメソッドは、Windows Presentation Foundation (WPF) アプリケーションでは次のように使用できます。

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtResult.Text = await DownloadStringAsync(txtUrl.Text,
            new Progress<int>(p => pbDownloadProgress.Value = p));
    }
    finally { btnDownload.IsEnabled = true; }
}
```

<a name="combinators"></a>
## <a name="using-the-built-in-task-based-combinators"></a>タスク ベースの組み込み連結子の使用
 <xref:System.Threading.Tasks> 名前空間には、タスクを構成および使用するための複数のメソッドがあります。

### <a name="taskrun"></a>Task.Run
 <xref:System.Threading.Tasks.Task> クラスには、<xref:System.Threading.Tasks.Task.Run%2A> メソッドがいくつか含まれています。このメソッドでは、次のように、<xref:System.Threading.Tasks.Task> や <xref:System.Threading.Tasks.Task%601> のような作業の負荷をスレッド プールに簡単に移すことができます。

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    textBox1.Text = await Task.Run(() =>
    {
        // … do compute-bound work here
        return answer;
    });
}
```

 <xref:System.Threading.Tasks.Task.Run%2A> オーバーロードなど、これらの <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> メソッドの一部は <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> メソッドの短縮形として存在します。  <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> など、他のオーバーロードでは、次のように、負荷分散される作業内で await を使用できます。

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    pictureBox1.Image = await Task.Run(async() =>
    {
        using(Bitmap bmp1 = await DownloadFirstImageAsync())
        using(Bitmap bmp2 = await DownloadSecondImageAsync())
        return Mashup(bmp1, bmp2);
    });
}
```

 このようなオーバーロードは論理的に、タスク並列ライブラリの <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 拡張メソッドとの連動で <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> メソッドを使用することと等しくなります。

### <a name="taskfromresult"></a>Task.FromResult
 データが既に利用でき、単に <xref:System.Threading.Tasks.Task.FromResult%2A> にリフトされたタスクを返すメソッドから返す必要があるシナリオでは、<xref:System.Threading.Tasks.Task%601> メソッドを使用します。

```csharp
public Task<int> GetValueAsync(string key)
{
    int cachedValue;
    return TryGetCachedValue(out cachedValue) ?
        Task.FromResult(cachedValue) :
        GetValueAsyncInternal();
}

private async Task<int> GetValueAsyncInternal(string key)
{
    …
}
```

### <a name="taskwhenall"></a>Task.WhenAll
 <xref:System.Threading.Tasks.Task.WhenAll%2A> メソッドは、タスクとして表される複数の非同期操作で非同期に待機するために使用されます。  メソッドには、一連の非ジェネリック タスクまたは不均一な一連のジェネリック タスク (複数の void を返す操作を非同期に待機したり、各値の型が異なる複数の値を返すメソッドを非同期に待機するなど) に加えて、均一な一連のジェネリック タスク (複数の `TResult` を返すメソッドを非同期に待機するなど) をサポートする複数のオーバーロードが含まれます。

 複数の顧客に電子メール メッセージを送信するとします。 このような場合、1 つのメッセージの送信完了を待たずに次のメッセージを送信するような、メッセージのオーバーラップ送信が可能です。 送信操作がいつ完了したか、またエラーが発生したかを確認することもできます。

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
await Task.WhenAll(asyncOps);
```

 このコードは発生する可能性がある例外を明示的に処理せず、`await` の結果のタスクの <xref:System.Threading.Tasks.Task.WhenAll%2A> から例外を反映します。  例外を処理するには、次のようなコードを使用できます。

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    ...
}
```

 この例では、非同期の操作が失敗した場合、すべての例外が <xref:System.AggregateException> 例外で連結され、<xref:System.Threading.Tasks.Task> メソッドから返される <xref:System.Threading.Tasks.Task.WhenAll%2A> に格納されます。  ただし、これらの例外の 1 つだけが `await` キーワードによって反映されます。  すべての例外を調べる場合は、上記のコードを次のように書き直します。

```csharp
Task [] asyncOps = (from addr in addrs select SendMailAsync(addr)).ToArray();
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    foreach(Task faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

 Web から複数のファイルを非同期にダウンロードする例を考えてみます。  この場合、すべての非同期操作の結果の型が同種で、結果に簡単にアクセスできます。

```csharp
string [] pages = await Task.WhenAll(
    from url in urls select DownloadStringAsync(url));
```

 前の void を返す場合で説明したのと同じ例外処理手法を使用できます。

```csharp
Task [] asyncOps =
    (from url in urls select DownloadStringAsync(url)).ToArray();
try
{
    string [] pages = await Task.WhenAll(asyncOps);
    ...
}
catch(Exception exc)
{
    foreach(Task<string> faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

### <a name="taskwhenany"></a>Task.WhenAny
 <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使用して、完了するタスクとして表される複数の非同期操作の 1 つのみを非同期に待機できます。  このメソッドは、次の 4 つの主なユース ケースで役立ちます。

- 冗長性:  1 つの操作を複数回実行し、最初に完了したものを選択する (たとえば、1 つの結果を生成する複数の株価情報 Web サービスに問い合わせて、最も速く完了したものを選択する)。

- インターリーブ: 複数の操作を起動してそのすべてが完了するのを待機するが、それらの操作を完了時に処理する。

- 調整:  他の操作が完了したら追加の操作を開始できるようにする。  これは、インターリーブのシナリオを拡張したものです。

- 初期のエスケープ: たとえば、タスク t1 が表す操作を別のタスク t2 と共に <xref:System.Threading.Tasks.Task.WhenAny%2A> タスクでグループにまとめ、<xref:System.Threading.Tasks.Task.WhenAny%2A> タスクで待機できます。 タスク t2 は、タイムアウトか取り消し、または <xref:System.Threading.Tasks.Task.WhenAny%2A> タスクを t1 の前に完了するようなその他のシグナルを表す場合があります。

#### <a name="redundancy"></a>冗長性
 株式を購入するかどうかを決定するとします。  信頼できるいくつかの株式推薦 Web サービスがありますが、日常的な負荷によっては、時に応じて各サービスがかなり遅くなることがあります。  <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使用すると、いずれかの操作が完了したときに通知を受け取ることができます。

```csharp
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol),
    GetBuyRecommendation2Async(symbol),
    GetBuyRecommendation3Async(symbol)
};
Task<bool> recommendation = await Task.WhenAny(recommendations);
if (await recommendation) BuyStock(symbol);
```

 正常に完了したすべてのタスクの結果をラップせずに返す <xref:System.Threading.Tasks.Task.WhenAll%2A> とは異なり、<xref:System.Threading.Tasks.Task.WhenAny%2A> は完了したタスクを返します。 タスクが失敗した場合は、それを把握することが重要です。タスクが成功した場合は、戻り値が関連付けられているタスクを把握することが重要です。  そのため、この例で示すように、返されるタスクの結果にアクセスするか、引き続き待機する必要があります。

 <xref:System.Threading.Tasks.Task.WhenAll%2A> と同様に、例外に対応できる必要があります。  完了したタスクを受け取るので、適切に `try/catch` で囲んで、エラーが反映されて返されるタスクを待機できます。次に例を示します。

```csharp
Task<bool> [] recommendations = …;
while(recommendations.Count > 0)
{
    Task<bool> recommendation = await Task.WhenAny(recommendations);
    try
    {
        if (await recommendation) BuyStock(symbol);
        break;
    }
    catch(WebException exc)
    {
        recommendations.Remove(recommendation);
    }
}
```

 さらに、最初のタスクが正常に完了しても、以降のタスクが失敗する可能性があります。  この時点で、例外を処理する方法はいくつかあります。起動したすべてのタスクが完了するまで待機できます。その場合は、<xref:System.Threading.Tasks.Task.WhenAll%2A> メソッドを使用するか、すべての例外が重要であり、ログに記録する必要があると指定できます。  そのためには、継続を使用してタスクが非同期に完了した時点で通知を受け取ることができます。

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => { if (t.IsFaulted) Log(t.Exception); });
}
```

 または

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => Log(t.Exception), TaskContinuationOptions.OnlyOnFaulted);
}
```

 または、以下のように指定できます。

```csharp
private static async void LogCompletionIfFailed(IEnumerable<Task> tasks)
{
    foreach(var task in tasks)
    {
        try { await task; }
        catch(Exception exc) { Log(exc); }
    }
}
…
LogCompletionIfFailed(recommendations);
```

 最後に、残りのすべての操作を取り消すこともできます。

```csharp
var cts = new CancellationTokenSource();
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol, cts.Token),
    GetBuyRecommendation2Async(symbol, cts.Token),
    GetBuyRecommendation3Async(symbol, cts.Token)
};

Task<bool> recommendation = await Task.WhenAny(recommendations);
cts.Cancel();
if (await recommendation) BuyStock(symbol);
```

#### <a name="interleaving"></a>インターリーブ
 Web からイメージをダウンロードして、各イメージを処理するとします (イメージを UI コントロールに追加するなど)。  UI スレッドで順次処理する必要があるとしても、イメージはできるだけ同時にダウンロードすることを考えています。 また、すべてダウンロードされるまで UI へのイメージの追加を保留するのは望ましくないので、ダウンロードが完了した順に追加します。

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

 次のように、ダウンロードしたイメージの <xref:System.Threading.ThreadPool> でコンピューター処理を集中して行うことが必要になるシナリオでも、インターリーブを適用できます。

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)
         .ContinueWith(t => ConvertImage(t.Result)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

#### <a name="throttling"></a>Throttling
 インターリーブの例ではユーザーが多くのイメージをダウンロードするため、このダウンロード数を絞り込み、たとえば、特定の数のダウンロードだけを同時実行する必要がある場合を考えます。 これを行うには、非同期操作のサブセットを開始します。  操作が完了したら、追加操作を開始して、完了した操作に代えて実行します。

```csharp
const int CONCURRENCY_LEVEL = 15;
Uri [] urls = …;
int nextIndex = 0;
var imageTasks = new List<Task<Bitmap>>();
while(nextIndex < CONCURRENCY_LEVEL && nextIndex < urls.Length)
{
    imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
    nextIndex++;
}

while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch(Exception exc) { Log(exc); }

    if (nextIndex < urls.Length)
    {
        imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
        nextIndex++;
    }
}
```

#### <a name="early-bailout"></a>初期のエスケープ
 ユーザーの取り消し要求 (キャンセル ボタンがクリックされたなど) に同時に対応できるようにしながら、1 つの操作の完了を非同期に待機するとします。 このシナリオのコードを次に示します。

```csharp
private CancellationTokenSource m_cts;

public void btnCancel_Click(object sender, EventArgs e)
{
    if (m_cts != null) m_cts.Cancel();
}

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();
    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        if (imageDownload.IsCompleted)
        {
            Bitmap image = await imageDownload;
            panel.AddImage(image);
        }
        else imageDownload.ContinueWith(t => Log(t));
    }
    finally { btnRun.Enabled = true; }
}

private static async Task UntilCompletionOrCancellation(
    Task asyncOp, CancellationToken ct)
{
    var tcs = new TaskCompletionSource<bool>();
    using(ct.Register(() => tcs.TrySetResult(true)))
        await Task.WhenAny(asyncOp, tcs.Task);
    return asyncOp;
}
```

 この実装は、エスケープを決めた直後にユーザー インターフェイスを再度使用できるようにしますが、基になる非同期操作は取り消しません。  別の方法として、エスケープを決めたときに保留中の操作を取り消すことがありますが、取り消し要求のために途中で終了する場合など、操作が実際に完了するまでユーザー インターフェイスを再確立しません。

```csharp
private CancellationTokenSource m_cts;

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();

    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text, m_cts.Token);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        Bitmap image = await imageDownload;
        panel.AddImage(image);
    }
    catch(OperationCanceledException) {}
    finally { btnRun.Enabled = true; }
}
```

 初期のエスケープのもう 1 つの例では、次のセクションで説明する <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドと連動で <xref:System.Threading.Tasks.Task.Delay%2A> メソッドを使用します。

### <a name="taskdelay"></a>Task.Delay
 <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> メソッドを使用し、非同期メソッドの実行を一時停止できます。  これは、ポーリング ループのビルド、あらかじめ指定された期間にわたるユーザー入力処理の遅延など、さまざまな機能に役立ちます。  <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> メソッドは、<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> との連動で使用し、待機にタイムアウトを実装する場合にも便利です。

 大規模な非同期操作 (ASP.NET Web サービスなど) の一部を構成するタスクが完了するまで時間がかかる場合 (特に操作が完了しない場合) には、操作全般に影響が及びます。  そのため、非同期操作の待機をタイムアウトできるようにすることが重要になります。  同期の <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=nameWithType> メソッドはタイムアウト値を受け取りますが、対応する <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> と前述の <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> メソッドは受け取りません。  代わりに、<xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> と <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> を併用し、タイムアウトを実装できます。

 たとえば、UI アプリケーションで、イメージをダウンロードし、そのダウンロード中は UI を無効にするとします。 ただし、ダウンロードに時間がかかりすぎる場合には、UI を再び有効にしてダウンロードを破棄します。

```csharp
public async void btnDownload_Click(object sender, EventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap> download = GetBitmapAsync(url);
        if (download == await Task.WhenAny(download, Task.Delay(3000)))
        {
            Bitmap bmp = await download;
            pictureBox.Image = bmp;
            status.Text = "Downloaded";
        }
        else
        {
            pictureBox.Image = null;
            status.Text = "Timed out";
            var ignored = download.ContinueWith(
                t => Trace("Task finally completed"));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

 複数のダウンロードにも同じことが当てはまります。次のように、<xref:System.Threading.Tasks.Task.WhenAll%2A> がタスクを返すためです。

```csharp
public async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap[]> downloads =
            Task.WhenAll(from url in urls select GetBitmapAsync(url));
        if (downloads == await Task.WhenAny(downloads, Task.Delay(3000)))
        {
            foreach(var bmp in downloads) panel.AddImage(bmp);
            status.Text = "Downloaded";
        }
        else
        {
            status.Text = "Timed out";
            downloads.ContinueWith(t => Log(t));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

## <a name="building-task-based-combinators"></a>タスク ベースの連結子のビルド
 タスクは、非同期操作を完全に表現し、操作の結合、その結果の取得などの同期機能と非同期機能を提供することができるため、大きなパターンをビルドするためのタスクを構成する有益な連結子ライブラリを構築できるようになります。  前のセクションで説明したように、.NET Framework には、いくつかの組み込み連結子が用意されていますが、独自にビルドすることもできます。 以下のセクションでは、有効な連結子メソッドと型の例をいくつか示します。

### <a name="retryonfault"></a>RetryOnFault
 多くの状況では、前の操作が失敗した場合に再試行することが望まれます。  同期コードの場合は、次の例のように `RetryOnFault` などのヘルパー メソッドをビルドしてこれを行うことができます。

```csharp
public static T RetryOnFault<T>(
    Func<T> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return function(); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 非同期操作の場合も、ほとんど同じヘルパー メソッドをビルドでき、TAP を使用して非同期操作を実装し、タスクを返します。

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 次のように、この連結子を使用してアプリケーションのロジックに再試行をエンコードできます。

```csharp
// Download the URL, trying up to three times in case of failure
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3);
```

 `RetryOnFault` 関数をさらに拡張できます。 たとえば、この関数は別の `Func<Task>` を受け取り、次のように再試行のタイミングを判断するために、再試行の間に呼び出されます。

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries, Func<Task> retryWhen)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
        await retryWhen().ConfigureAwait(false);
    }
    return default(T);
}
```

 次の関数を使用して、操作を再試行する前に 1 秒待機するようにできます。

```csharp
// Download the URL, trying up to three times in case of failure,
// and delaying for a second between retries
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3, () => Task.Delay(1000));
```

### <a name="needonlyone"></a>NeedOnlyOne
 操作の待機時間と成功の確率を改善するために、冗長性を活用できる場合があります。  株価情報を提供する複数の Web サービスがあるとします。しかし、時間によって、各サービスの品質レベルと応答時間が変化します。  この変動に対応するには、すべての Web サービスに要求を出し、最初の応答を取得した時点で残りの要求を取り消します。  複数の操作を起動し、応答を待機し、残りの操作を取り消すというこの一般的なパターンの実装は、ヘルパー関数を実装して簡略化できます。 次の例の `NeedOnlyOne` 関数は、このシナリオを示します。

```csharp
public static async Task<T> NeedOnlyOne(
    params Func<CancellationToken,Task<T>> [] functions)
{
    var cts = new CancellationTokenSource();
    var tasks = (from function in functions
                 select function(cts.Token)).ToArray();
    var completed = await Task.WhenAny(tasks).ConfigureAwait(false);
    cts.Cancel();
    foreach(var task in tasks)
    {
        var ignored = task.ContinueWith(
            t => Log(t), TaskContinuationOptions.OnlyOnFaulted);
    }
    return completed;
}
```

 この関数は次のように使用できます。

```csharp
double currentPrice = await NeedOnlyOne(
    ct => GetCurrentPriceFromServer1Async("msft", ct),
    ct => GetCurrentPriceFromServer2Async("msft", ct),
    ct => GetCurrentPriceFromServer3Async("msft", ct));
```

### <a name="interleaved-operations"></a>インターリーブされた操作
 多数のタスクを使用する際に、インターリーブのシナリオをサポートするために <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使用すると、パフォーマンスの問題が発生することがあります。 <xref:System.Threading.Tasks.Task.WhenAny%2A> を呼び出すたびに、継続が各タスクに登録されます。 タスクの数を N とすると、インターリーブ操作の有効期間に作成された O (N<sup>2</sup>) の継続になります。 多数のタスクを使用する場合は、連結子 (次の例の `Interleaved`) を使用してパフォーマンスの問題に対処できます。

```csharp
static IEnumerable<Task<T>> Interleaved<T>(IEnumerable<Task<T>> tasks)
{
    var inputTasks = tasks.ToList();
    var sources = (from _ in Enumerable.Range(0, inputTasks.Count)
                   select new TaskCompletionSource<T>()).ToList();
    int nextTaskIndex = -1;
    foreach (var inputTask in inputTasks)
    {
        inputTask.ContinueWith(completed =>
        {
            var source = sources[Interlocked.Increment(ref nextTaskIndex)];
            if (completed.IsFaulted)
                source.TrySetException(completed.Exception.InnerExceptions);
            else if (completed.IsCanceled)
                source.TrySetCanceled();
            else
                source.TrySetResult(completed.Result);
        }, CancellationToken.None,
           TaskContinuationOptions.ExecuteSynchronously,
           TaskScheduler.Default);
    }
    return from source in sources
           select source.Task;
}
```

 タスクが完了すると、たとえば次のように、連結子を使用してタスクの結果を処理できます。

```csharp
IEnumerable<Task<int>> tasks = ...;
foreach(var task in Interleaved(tasks))
{
    int result = await task;
    …
}
```

### <a name="whenallorfirstexception"></a>WhenAllOrFirstException
 特定のスキャッター/ギャザー シナリオでは、すべてのタスクの完了を待機することが考えられますが、そのうちの 1 つがエラーになった場合には、例外の発生時点で待機を停止することが望まれます。  これは、次の例の `WhenAllOrFirstException` などの連結子メソッドを使用して実行できます。

```csharp
public static Task<T[]> WhenAllOrFirstException<T>(IEnumerable<Task<T>> tasks)
{
    var inputs = tasks.ToList();
    var ce = new CountdownEvent(inputs.Count);
    var tcs = new TaskCompletionSource<T[]>();

    Action<Task> onCompleted = (Task completed) =>
    {
        if (completed.IsFaulted)
            tcs.TrySetException(completed.Exception.InnerExceptions);
        if (ce.Signal() && !tcs.Task.IsCompleted)
            tcs.TrySetResult(inputs.Select(t => t.Result).ToArray());
    };

    foreach (var t in inputs) t.ContinueWith(onCompleted);
    return tcs.Task;
}
```

## <a name="building-task-based-data-structures"></a>タスク ベースのデータ構造のビルド
 カスタム タスク ベースの連結子をビルドする機能に加えて、非同期操作の結果と、結合する必要な同期の両方を表す <xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> 内にデータ構造を配置することで、非同期シナリオに使用するカスタム データ構造をビルドするための非常に強力な型になります。

### <a name="asynccache"></a>AsyncCache
 タスクの重要な側面の 1 つに、タスクを待機する複数のコンシューマーに渡し、タスクに継続を登録し、その結果または例外を取得できることなどがあります (<xref:System.Threading.Tasks.Task%601> の場合)。  これで <xref:System.Threading.Tasks.Task> と <xref:System.Threading.Tasks.Task%601> が非同期キャッシュ インフラストラクチャで使用できるように調整されます。  次に、<xref:System.Threading.Tasks.Task%601> 上にビルドした小さいながらも強力な非同期キャッシュの例を示します。

```csharp
public class AsyncCache<TKey, TValue>
{
    private readonly Func<TKey, Task<TValue>> _valueFactory;
    private readonly ConcurrentDictionary<TKey, Lazy<Task<TValue>>> _map;

    public AsyncCache(Func<TKey, Task<TValue>> valueFactory)
    {
        if (valueFactory == null) throw new ArgumentNullException("loader");
        _valueFactory = valueFactory;
        _map = new ConcurrentDictionary<TKey, Lazy<Task<TValue>>>();
    }

    public Task<TValue> this[TKey key]
    {
        get
        {
            if (key == null) throw new ArgumentNullException("key");
            return _map.GetOrAdd(key, toAdd =>
                new Lazy<Task<TValue>>(() => _valueFactory(toAdd))).Value;
        }
    }
}
```

 [AsyncCache\<TKey,TValue>](https://devblogs.microsoft.com/pfxteam/parallelextensionsextras-tour-12-asynccache/) クラスは、コンストラクターへのデリゲートとして `TKey` を受け取る関数を受け入れ、<xref:System.Threading.Tasks.Task%601> を返します。  以前にキャッシュからアクセスした値は内部ディクショナリに格納され、`AsyncCache` によって、キャッシュに同時にアクセスしてもキーごとにタスクが 1 つしか生成されないようにします。

 たとえば、次のようにダウンロードされた Web ページのキャッシュをビルドできます。

```csharp
private AsyncCache<string,string> m_webPages =
    new AsyncCache<string,string>(DownloadStringAsync);
```

 Web ページのコンテンツが必要なときはいつでも、非同期メソッドでこのキャッシュを使用できます。 `AsyncCache` クラスは、できるだけ少ない数のページがダウンロードされるようにして、結果をキャッシュします。

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtContents.Text = await m_webPages["https://www.microsoft.com"];
    }
    finally { btnDownload.IsEnabled = true; }
}
```

### <a name="asyncproducerconsumercollection"></a>AsyncProducerConsumerCollection
 タスクは、非同期アクティビティを調整するためのデータ構造のビルドにも使用できます。  プロデューサー/コンシューマーというクラシックな並列設計パターンの 1 つについて考えてみます。  このパターンでは、コンシューマーによって使用されるデータをプロデューサーが生成し、プロデューサーおよびコンシューマーは並列に実行できます。 たとえば、コンシューマーは、項目 2 を生成中のプロデューサーが以前に生成した項目 1 を処理します。  プロデューサー/コンシューマー パターンでは、新しいデータについてコンシューマーに通知され、そのデータが使用可能な場合にコンシューマーが見つけられるように、プロデューサーが作成した作業を格納するデータ構造が必ず必要です。

 プロデューサーおよびコンシューマーとして使用する非同期メソッドを有効にするタスク上にビルドされた単純なデータ構造を次に示します。

```csharp
public class AsyncProducerConsumerCollection<T>
{
    private readonly Queue<T> m_collection = new Queue<T>();
    private readonly Queue<TaskCompletionSource<T>> m_waiting =
        new Queue<TaskCompletionSource<T>>();

    public void Add(T item)
    {
        TaskCompletionSource<T> tcs = null;
        lock (m_collection)
        {
            if (m_waiting.Count > 0) tcs = m_waiting.Dequeue();
            else m_collection.Enqueue(item);
        }
        if (tcs != null) tcs.TrySetResult(item);
    }

    public Task<T> Take()
    {
        lock (m_collection)
        {
            if (m_collection.Count > 0)
            {
                return Task.FromResult(m_collection.Dequeue());
            }
            else
            {
                var tcs = new TaskCompletionSource<T>();
                m_waiting.Enqueue(tcs);
                return tcs.Task;
            }
        }
    }
}
```

 このデータ構造を使用すると、次のようなコードを作成できます。

```csharp
private static AsyncProducerConsumerCollection<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.Take();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Add(data);
}
```

<xref:System.Threading.Tasks.Dataflow> 名前空間には <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 型が含まれます。これは同様に使用できますが、カスタムのコレクション型をビルドする必要はありません。

```csharp
private static BufferBlock<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.ReceiveAsync();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Post(data);
}
```

> [!NOTE]
> <xref:System.Threading.Tasks.Dataflow> 名前空間は .NET Framework 4.5 にあります。**NuGet** を利用してください。 <xref:System.Threading.Tasks.Dataflow> 名前空間が含まれるアセンブリをインストールするには、Visual Studio でプロジェクトを開き、 **[プロジェクト] メニューの [NuGet パッケージの管理]** を選択し、Microsoft.Tpl.Dataflow パッケージをオンライン検索します。

## <a name="see-also"></a>参照

- [タスク ベースの非同期パターン (TAP)](../../../docs/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)
- [タスク ベースの非同期パターンの実装](../../../docs/standard/asynchronous-programming-patterns/implementing-the-task-based-asynchronous-pattern.md)
- [他の非同期パターンと型との相互運用](../../../docs/standard/asynchronous-programming-patterns/interop-with-other-asynchronous-patterns-and-types.md)
