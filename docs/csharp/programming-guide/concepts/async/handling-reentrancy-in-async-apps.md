---
title: 非同期アプリにおける再入の処理 (C#)
ms.date: 07/20/2015
ms.assetid: 47c5075e-c448-45ce-9155-ed4e7e98c677
ms.openlocfilehash: 67fbbd294ffe6219b58065f974543b2dd483a92c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77451864"
---
# <a name="handling-reentrancy-in-async-apps-c"></a>非同期アプリにおける再入の処理 (C#)

非同期コードをアプリに含める場合は、再入を考慮し、場合によっては回避することをお勧めします。これは、完了前に非同期操作の再入力を参照します。 再入の可能性を特定して処理しないと、予期しない結果が発生する可能性があります。

**このトピックの内容**

- [再入を認識する](#BKMK_RecognizingReentrancy)

- [再入を処理する](#BKMK_HandlingReentrancy)

  - [[Start] ボタンを無効にする](#BKMK_DisableTheStartButton)

  - [操作を取り消して再開する](#BKMK_CancelAndRestart)

  - [複数の操作を実行して出力をキューに登録する](#BKMK_RunMultipleOperations)

- [例のアプリをレビューして実行する](#BKMD_SettingUpTheExample)

> [!NOTE]
> この例を実行するには、Visual Studio 2012 以降と .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。

> [!NOTE]
> 現在、アプリ開発で使用する最小バージョンは、トランスポート層セキュリティ (TLS) バージョン 1.2 です。 ご利用のアプリがバージョン 4.7 より前の .NET Framework を対象としている場合は、「[.NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス](../../../../framework/network-programming/tls.md)」の記事を参照してください。

## <a name="BKMK_RecognizingReentrancy"></a>再入を認識する

このトピックの例では、ユーザーが **[Start]** をクリックして非同期アプリを開始します。このアプリは、一連の Web サイトをダウンロードし、ダウンロードされた合計バイト数を計算します。 同期バージョンの例では、ユーザーが何回ボタンをクリックしても同じように応答します。2 回目以降はアプリが実行を完了するまで、UI スレッドはこれらのイベントを無視するからです。 ただし、非同期アプリでは、UI スレッドは応答し続けるので、完了前に非同期操作を再入力することがあります。

次の例は、ユーザーが 1 度だけ **[Start]** をクリックした場合の出力を示しています。 ダウンロードされた Web サイトの一覧には、各サイトのサイズがバイト単位で表示されます。 合計バイト数は最後に表示されます。

```output
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

ただし、ユーザーがボタンを複数回クリックすると、イベント ハンドラーは繰り返し呼び出され、ダウンロード プロセスはそのたびに再入力されます。 その結果、複数の非同期操作が同時に実行され、出力は結果をインターリーブするので、合計バイト数がややこしくなります。

```output
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
7. msdn.microsoft.com                                            42972
4. msdn.microsoft.com/library/hh290140.aspx               117152
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
7. msdn.microsoft.com                                            42972
5. msdn.microsoft.com/library/hh524395.aspx                68959
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

このトピックの最後にスクロールすると、この出力を生成するコードをレビューできます。 コードを試してみるには、ソリューションをローカル コンピューターにダウンロードしてから WebsiteDownload プロジェクトを実行するか、このトピックの最後にあるコードを使用して独自のプロジェクトを作成します。 詳細と手順については、「[例のアプリをレビューして実行する](#BKMD_SettingUpTheExample)」を参照してください。

## <a name="BKMK_HandlingReentrancy"></a>再入を処理する

再入の処理は、アプリで何を行うかに応じてさまざまな方法で実行できます。 このトピックでは、次の例を紹介します。

- [[Start] ボタンを無効にする](#BKMK_DisableTheStartButton)

  処理の実行中、ユーザーが中断できないように **[Start]** ボタンを無効にします。

- [操作を取り消して再開する](#BKMK_CancelAndRestart)

  ユーザーが **[Start]** を再度クリックしたときに実行されている処理を取り消し、最後に要求された処理を続行できるようにします。

- [複数の操作を実行して出力をキューに登録する](#BKMK_RunMultipleOperations)

  要求されたすべての処理を非同期的に実行できるようにします。ただし、各処理の結果が順番にまとめて表示されるように出力を調整します。

### <a name="BKMK_DisableTheStartButton"></a>[Start] ボタンを無効にする

処理の実行中に **[Start]** ボタンを利用できないようにするには、`StartButton_Click` イベント ハンドラーの上部にあるボタンを無効にします。 処理が完了しユーザーが再度アプリを実行できるようになったら、`finally` ブロック内からこのボタンを再度有効にできます。

このシナリオを設定するには、「[例のアプリをレビューして実行する](#BKMD_SettingUpTheExample)」に用意されている基本コードを次のように変更します。 また、完成したアプリを「[Async Samples: Reentrancy in .NET Desktop Apps (非同期の例: .NET デスクトップ アプリでの再入)](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)」からダウンロードすることもできます。 プロジェクトの名前は DisableStartButton です。

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // This line is commented out to make the results clearer in the output.
    //ResultsTextBox.Text = "";

    // ***Disable the Start button until the downloads are complete.
    StartButton.IsEnabled = false;

    try
    {
        await AccessTheWebAsync();
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.";
    }
    // ***Enable the Start button in case you want to run the program again.
    finally
    {
        StartButton.IsEnabled = true;
    }
}
```

変更の結果、`AccessTheWebAsync` が Web サイトをダウンロードしている間は、ボタンが応答しないので、プロセスを再入力できません。

### <a name="BKMK_CancelAndRestart"></a>操作を取り消して再開する

**[Start]** ボタンを無効にせず、有効の状態を保持できますが、ユーザーがボタンを再度クリックしたときに、実行中の処理を取り消し、最後に開始された処理を続行できます。

取り消しの詳細については、「[Fine Tuning Your Async Application (C#) (非同期アプリケーションの微調整 (C#))](./fine-tuning-your-async-application.md)」を参照してください。

このシナリオを設定するには、「[例のアプリをレビューして実行する](#BKMD_SettingUpTheExample)」に用意されている基本コードを次のように変更します。 また、完成したアプリを「[Async Samples: Reentrancy in .NET Desktop Apps (非同期の例: .NET デスクトップ アプリでの再入)](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)」からダウンロードすることもできます。 プロジェクトの名前は CancelAndRestart です。

1. すべてのメソッドのスコープである <xref:System.Threading.CancellationTokenSource> 変数、`cts` を宣言します。

    ```csharp
    public partial class MainWindow : Window   // Or class MainPage
    {
        // *** Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;
    ```

2. `StartButton_Click` で、処理が既に実行されているかどうかを確認します。 `cts` の値が null の場合、アクティブな操作はまだありません。 値が null 以外の場合は、実行中の処理が取り消されます。

    ```csharp
    // *** If a download process is already underway, cancel it.
    if (cts != null)
    {
        cts.Cancel();
    }
    ```

3. `cts` に、現在のプロセスを表す別の値を設定します。

    ```csharp
    // *** Now set cts to a new value that you can use to cancel the current process
    // if the button is chosen again.
    CancellationTokenSource newCTS = new CancellationTokenSource();
    cts = newCTS;
    ```

4. `StartButton_Click` の最後で現在のプロセスが完了します。したがって、`cts` の値を null に戻します。

    ```csharp
    // *** When the process is complete, signal that another process can begin.
    if (cts == newCTS)
        cts = null;
    ```

次のコードは、`StartButton_Click` のすべての変更を示しています。 追加部分はアスタリスクが付いています。

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // This line is commented out to make the results clearer in the output.
    //ResultsTextBox.Clear();

    // *** If a download process is already underway, cancel it.
    if (cts != null)
    {
        cts.Cancel();
    }

    // *** Now set cts to cancel the current process if the button is chosen again.
    CancellationTokenSource newCTS = new CancellationTokenSource();
    cts = newCTS;

    try
    {
        // ***Send cts.Token to carry the message if there is a cancellation request.
        await AccessTheWebAsync(cts.Token);

    }
    // *** Catch cancellations separately.
    catch (OperationCanceledException)
    {
        ResultsTextBox.Text += "\r\nDownloads canceled.\r\n";
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.\r\n";
    }
    // *** When the process is complete, signal that another process can proceed.
    if (cts == newCTS)
        cts = null;
}
```

`AccessTheWebAsync` で、次の変更を行います。

- `StartButton_Click` からキャンセル トークンを受け取るためのパラメーターを追加します。

- <xref:System.Net.Http.HttpClient.GetAsync%2A> メソッドを使用して Web サイトをダウンロードします。これは `GetAsync` が <xref:System.Threading.CancellationToken> 引数を受け取るからです。

- `DisplayResults` を呼び出してダウンロードした各 Web サイトの結果を表示する前に、`ct` で、現在の処理が取り消されていないことを確認します。

次のコードはこの変更を示しています。変更の部分にはアスタリスクが付いています。

```csharp
// *** Provide a parameter for the CancellationToken from StartButton_Click.
async Task AccessTheWebAsync(CancellationToken ct)
{
    // Declare an HttpClient object.
    HttpClient client = new HttpClient();

    // Make a list of web addresses.
    List<string> urlList = SetUpURLList();

    var total = 0;
    var position = 0;

    foreach (var url in urlList)
    {
        // *** Use the HttpClient.GetAsync method because it accepts a
        // cancellation token.
        HttpResponseMessage response = await client.GetAsync(url, ct);

        // *** Retrieve the website contents from the HttpResponseMessage.
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

        // *** Check for cancellations before displaying information about the
        // latest site.
        ct.ThrowIfCancellationRequested();

        DisplayResults(url, urlContents, ++position);

        // Update the total.
        total += urlContents.Length;
    }

    // Display the total count for all of the websites.
    ResultsTextBox.Text +=
        $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
}
```

このアプリの実行中に複数回 **[Start]** ボタンをクリックすると、次の出力のような結果が生成されます。

```output
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               122505
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

部分的なリストを削除するには、`StartButton_Click` コードの先頭行のコメントを解除して、ユーザーが操作を再開するたびに、テキスト ボックスをクリアします。

### <a name="BKMK_RunMultipleOperations"></a>複数の操作を実行して出力をキューに登録する

この 3 番目の例は、ユーザーが **[Start]** ボタンをクリックするたびに非同期操作が開始され、すべての操作が完了まで実行されるという点で最も複雑です。 要求されたすべての操作によって Web サイトがリストから非同期的にダウンロードされますが、操作からの出力は順次表示されます。 つまり、「[再入を認識する](#BKMK_RecognizingReentrancy)」の出力に示されているように、実際のダウンロード アクティビティはインターリーブされますが、各グループの結果のリストは個別に表示されます。

操作は、表示プロセスのゲートキーパーとして機能するグローバル <xref:System.Threading.Tasks.Task>、`pendingWork` を共有します。

このシナリオを設定するには、「[例のアプリをレビューして実行する](#BKMD_SettingUpTheExample)」に用意されている基本コードを次のように変更します。 また、完成したアプリを「[Async Samples: Reentrancy in .NET Desktop Apps (非同期の例: .NET デスクトップ アプリでの再入)](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)」からダウンロードすることもできます。 プロジェクトの名前は QueueResults です。

次の出力は、ユーザーが 1 度だけ **[Start]** ボタンをクリックした場合の結果を示しています。 文字ラベル A は、 **[Start]** ボタンが最初にクリックされた結果であることを示しています。 数字は、ダウンロード対象の一覧における URL の順序を示しています。

```output
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               209858
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71260
A-6. msdn.microsoft.com/library/ms404677.aspx               199186
A-7. msdn.microsoft.com                                            53266
A-8. msdn.microsoft.com/library/ff730837.aspx               148020

TOTAL bytes returned:  918876

#Group A is complete.
```

ユーザーが **[Start]** ボタンを 3 回クリックすると、アプリでは次のような出力が生成されます。 先頭にシャープ記号 (#) が付いている情報行は、アプリケーションの進行状況を追跡します。

```output
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               207089
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71259
A-6. msdn.microsoft.com/library/ms404677.aspx               199185

#Starting group B.
#Task assigned for group B.

A-7. msdn.microsoft.com                                            53266

#Starting group C.
#Task assigned for group C.

A-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916095

B-1. msdn.microsoft.com/library/hh191443.aspx                87389
B-2. msdn.microsoft.com/library/aa578028.aspx               207089
B-3. msdn.microsoft.com/library/jj155761.aspx                30870
B-4. msdn.microsoft.com/library/hh290140.aspx               119027
B-5. msdn.microsoft.com/library/hh524395.aspx                71260
B-6. msdn.microsoft.com/library/ms404677.aspx               199186

#Group A is complete.

B-7. msdn.microsoft.com                                            53266
B-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916097

C-1. msdn.microsoft.com/library/hh191443.aspx                87389
C-2. msdn.microsoft.com/library/aa578028.aspx               207089

#Group B is complete.

C-3. msdn.microsoft.com/library/jj155761.aspx                30870
C-4. msdn.microsoft.com/library/hh290140.aspx               119027
C-5. msdn.microsoft.com/library/hh524395.aspx                72765
C-6. msdn.microsoft.com/library/ms404677.aspx               199186
C-7. msdn.microsoft.com                                            56190
C-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  920526

#Group C is complete.
```

グループ B とグループ C は、グループ A が終了する前に開始します。ただし、各グループの出力は個別に表示されます。 グループ A のすべての出力が最初に表示され、その後にグループ B のすべての出力が続き、さらにグループ C のすべての出力が表示されます。グループは必ず順番に表示され、グループごとに、個別の Web サイトに関する情報が URL 一覧の URL の順番で表示されます。

ただし、ダウンロードが実際に行われる順序は予測できません。 複数のグループが開始されたら、そのグループが生成するダウンロード タスクはすべてのアクティブです。 B-1 の前に A-1 がダウンロードされる、また、A-2 の前に A-1 がダウンロードされると仮定することはできません。

#### <a name="global-definitions"></a>グローバル定義

サンプル コードには、すべてのメソッドから参照できる次の 2 つのグローバル宣言が含まれます。

```csharp
public partial class MainWindow : Window  // Class MainPage in Windows Store app.
{
    // ***Declare the following variables where all methods can access them.
    private Task pendingWork = null;
    private char group = (char)('A' - 1);
```

`Task` 変数、`pendingWork` は、表示プロセスを監視し、あるグループが別のグループの表示操作を中断しないようにします。 文字変数 `group` は、異なるグループからの出力にラベルを付け、予想された順序で結果が表示されていることを確認します。

#### <a name="the-click-event-handler"></a>Click イベント ハンドラー

イベント ハンドラー `StartButton_Click` は、ユーザーが **[Start]** ボタンを選択するたびに、グループ文字をインクリメントします。 ハンドラーは `AccessTheWebAsync` を呼び出して、ダウンロード操作を実行します。

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // ***Verify that each group's results are displayed together, and that
    // the groups display in order, by marking each group with a letter.
    group = (char)(group + 1);
    ResultsTextBox.Text += $"\r\n\r\n#Starting group {group}.";

    try
    {
        // *** Pass the group value to AccessTheWebAsync.
        char finishedGroup = await AccessTheWebAsync(group);

        // The following line verifies a successful return from the download and
        // display procedures.
        ResultsTextBox.Text += $"\r\n\r\n#Group {finishedGroup} is complete.\r\n";
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.";
    }
}
```

#### <a name="the-accessthewebasync-method"></a>AccessTheWebAsync メソッド

この例では、`AccessTheWebAsync` を 2 つのメソッドに分割します。 最初のメソッド、`AccessTheWebAsync` は、グループのすべてのダウンロード タスクを開始し、`pendingWork` を設定して表示プロセスを制御します。 このメソッドは、統合言語クエリ (LINQ クエリ) と <xref:System.Linq.Enumerable.ToArray%2A> を使用して、すべてのダウンロードを同時に開始します。

その後、`AccessTheWebAsync` は、`FinishOneGroupAsync` を呼び出して各ダウンロードが完了するまで待機し、その長さを表示します。

`FinishOneGroupAsync` は、`pendingWork` の `AccessTheWebAsync` に割り当てられたタスクを返します。 この値は、タスクが完了する前に、別の操作によって操作が中断されないようにします。

```csharp
private async Task<char> AccessTheWebAsync(char grp)
{
    HttpClient client = new HttpClient();

    // Make a list of the web addresses to download.
    List<string> urlList = SetUpURLList();

    // ***Kick off the downloads. The application of ToArray activates all the download tasks.
    Task<byte[]>[] getContentTasks = urlList.Select(url => client.GetByteArrayAsync(url)).ToArray();

    // ***Call the method that awaits the downloads and displays the results.
    // Assign the Task that FinishOneGroupAsync returns to the gatekeeper task, pendingWork.
    pendingWork = FinishOneGroupAsync(urlList, getContentTasks, grp);

    ResultsTextBox.Text += $"\r\n#Task assigned for group {grp}. Download tasks are active.\r\n";

    // ***This task is complete when a group has finished downloading and displaying.
    await pendingWork;

    // You can do other work here or just return.
    return grp;
}
```

#### <a name="the-finishonegroupasync-method"></a>FinishOneGroupAsync メソッド

このメソッドは、グループのダウンロード タスクを循環参照し、それぞれを待機してから、ダウンロードされた Web サイトの長さを表示して、その長さを合計に追加します。

`FinishOneGroupAsync` の最初のステートメントは、`pendingWork` を使用して、メソッドを入力することが、表示プロセスの操作または待機中の操作の妨げにならないようにします。 これらの操作が進行中の場合、入力操作は順番を待つ必要があります。

```csharp
private async Task FinishOneGroupAsync(List<string> urls, Task<byte[]>[] contentTasks, char grp)
{
    // ***Wait for the previous group to finish displaying results.
    if (pendingWork != null) await pendingWork;

    int total = 0;

    // contentTasks is the array of Tasks that was created in AccessTheWebAsync.
    for (int i = 0; i < contentTasks.Length; i++)
    {
        // Await the download of a particular URL, and then display the URL and
        // its length.
        byte[] content = await contentTasks[i];
        DisplayResults(urls[i], content, i, grp);
        total += content.Length;
    }

    // Display the total count for all of the websites.
    ResultsTextBox.Text +=
        $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
}
```

#### <a name="points-of-interest"></a>目的のポイント

出力で先頭にシャープ記号 (#) が付いている情報行は、この例の動作を明確に示しています。

出力のパターンを次に示します。

- 前のグループが出力を表示していても、グループを開始できます。その際、前のグループの出力の表示は中断されません。

    ```output
    #Starting group A.
    #Task assigned for group A. Download tasks are active.

    A-1. msdn.microsoft.com/library/hh191443.aspx                87389
    A-2. msdn.microsoft.com/library/aa578028.aspx               207089
    A-3. msdn.microsoft.com/library/jj155761.aspx                30870
    A-4. msdn.microsoft.com/library/hh290140.aspx               119037
    A-5. msdn.microsoft.com/library/hh524395.aspx                71260

    #Starting group B.
    #Task assigned for group B. Download tasks are active.

    A-6. msdn.microsoft.com/library/ms404677.aspx               199186
    A-7. msdn.microsoft.com                                            53078
    A-8. msdn.microsoft.com/library/ff730837.aspx               148010

    TOTAL bytes returned:  915919

    B-1. msdn.microsoft.com/library/hh191443.aspx                87388
    B-2. msdn.microsoft.com/library/aa578028.aspx               207089
    B-3. msdn.microsoft.com/library/jj155761.aspx                30870

    #Group A is complete.

    B-4. msdn.microsoft.com/library/hh290140.aspx               119027
    B-5. msdn.microsoft.com/library/hh524395.aspx                71260
    B-6. msdn.microsoft.com/library/ms404677.aspx               199186
    B-7. msdn.microsoft.com                                            53078
    B-8. msdn.microsoft.com/library/ff730837.aspx               148010

    TOTAL bytes returned:  915908
    ```

- 最初に開始したグループ A についてのみ、`pendingWork` 開始時の、`FinishOneGroupAsync` タスクは null になります。 `FinishOneGroupAsync` に達したとき、グループ A はまだ await 式を完了していません。 したがって、コントロールは `AccessTheWebAsync` に戻っておらず、`pendingWork` への最初の割り当ては発生していません。

- 次の 2 行は、出力に必ず同時に表示されます。 `StartButton_Click` のグループ操作が開始してから、グループのタスクが `pendingWork` に割り当てられるまでの間、コードが中断されることは決してありません。

    ```output
    #Starting group B.
    #Task assigned for group B. Download tasks are active.
    ```

    グループが `StartButton_Click` に移行したら、`FinishOneGroupAsync` に移行するまでは、await 式は完了しません。 したがって、コード セグメントの途中で、他の操作がコントロールを得ることはありません。

## <a name="BKMD_SettingUpTheExample"></a>例のアプリをレビューして実行する

サンプル アプリをさらに詳しく理解するには、そのアプリをダウンロードし、ご自身でビルドしてみてください。また、このトピックの最後にあるコードをレビューすることもできます。アプリを実装する必要はありません。

> [!NOTE]
> Windows Presentation Foundation (WPF) デスクトップ アプリとして例を実行するには、Visual Studio 2012 以降と .NET Framework 4.5 以降がコンピューターにインストールされている必要があります。

### <a name="BKMK_DownloadingTheApp"></a>アプリをダウンロードする

1. 圧縮ファイルを「[Async Samples: Reentrancy in .NET Desktop Apps (非同期の例: .NET デスクトップ アプリでの再入)](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)」からダウンロードします。

2. ダウンロードしたファイルを圧縮解除し、Visual Studio を起動します。

3. メニュー バーで **[ファイル]** 、 **[開く]** 、 **[プロジェクト/ソリューション]** の順に選択します。

4. 圧縮解除したサンプル コードが含まれるフォルダーに移動し、ソリューション (.sln) ファイルを開きます。

5. **ソリューション エクスプローラー**で、実行するプロジェクトのショートカット メニューを開き、 **[スタートアップ プロジェクトに設定]** を選択します。

6. Ctrl キーを押しながら F5 キーを押してプロジェクトをビルドし、実行します。

### <a name="BKMK_BuildingTheApp"></a>アプリケーションをビルドする

次のセクションでは、WPF アプリとして例をビルドするコードを示します。

##### <a name="to-build-a-wpf-app"></a>WPF アプリをビルドするには

1. Visual Studio を起動します。

2. メニュー バーで、 **[ファイル]** 、 **[新規作成]** 、 **[プロジェクト]** の順に選択します。

     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストールされたテンプレート]** ペインで、 **[Visual C#]** を展開し、 **[Windows]** を展開します。

4. プロジェクトの種類の一覧の **[WPF アプリケーション]** をクリックします。

5. プロジェクトに `WebsiteDownloadWPF` という名前を指定し、4.6 以降の .NET Framework のバージョンを選択して、 **[OK]** ボタンをクリックします。

     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

6. Visual Studio コード エディターで、 **[MainWindow.xaml]** タブをクリックします。

     タブが表示されない場合は、**ソリューション エクスプローラー**で MainWindow.xaml のショートカット メニューを開き、 **[コードの表示]** を選択します。

7. MainWindow.xaml の **XAML** ビューで、コードを次のコードに置き換えます。

    ```csharp
    <Window x:Class="WebsiteDownloadWPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:WebsiteDownloadWPF"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Width="517" Height="360">
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="-1,0,0,0" VerticalAlignment="Top" Click="StartButton_Click" Height="53" Background="#FFA89B9B" FontSize="36" Width="518"  />
            <TextBox x:Name="ResultsTextBox" HorizontalAlignment="Left" Margin="-1,53,0,-36" TextWrapping="Wrap" VerticalAlignment="Top" Height="343" FontSize="10" ScrollViewer.VerticalScrollBarVisibility="Visible" Width="518" FontFamily="Lucida Console" />
        </Grid>
    </Window>
    ```

     テキスト ボックスとボタンを含む簡単なウィンドウが、MainWindow.xaml の**デザイン** ビューに表示されます。

8. **ソリューション エクスプローラー**で **[参照]** を右クリックし、 **[参照の追加]** を選択します。

     まだ選択されていない場合は、<xref:System.Net.Http> の参照を追加します。

9. **ソリューション エクスプローラー**で MainWindow.xaml.cs のショートカット メニューを開き、 **[コードの表示]** を選択します。

10. MainWindow.xaml.cs のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows;
    using System.Windows.Controls;
    using System.Windows.Data;
    using System.Windows.Documents;
    using System.Windows.Input;
    using System.Windows.Media;
    using System.Windows.Media.Imaging;
    using System.Windows.Navigation;
    using System.Windows.Shapes;

    // Add the following using directives, and add a reference for System.Net.Http.
    using System.Net.Http;
    using System.Threading;

    namespace WebsiteDownloadWPF
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                System.Net.ServicePointManager.SecurityProtocol |= System.Net.SecurityProtocolType.Tls12;
                InitializeComponent();
            }

            private async void StartButton_Click(object sender, RoutedEventArgs e)
            {
                // This line is commented out to make the results clearer in the output.
                //ResultsTextBox.Text = "";

                try
                {
                    await AccessTheWebAsync();
                }
                catch (Exception)
                {
                    ResultsTextBox.Text += "\r\nDownloads failed.";
                }
            }

            private async Task AccessTheWebAsync()
            {
                // Declare an HttpClient object.
                HttpClient client = new HttpClient();

                // Make a list of web addresses.
                List<string> urlList = SetUpURLList();

                var total = 0;
                var position = 0;

                foreach (var url in urlList)
                {
                    // GetByteArrayAsync returns a task. At completion, the task
                    // produces a byte array.
                    byte[] urlContents = await client.GetByteArrayAsync(url);

                    DisplayResults(url, urlContents, ++position);

                    // Update the total.
                    total += urlContents.Length;
                }

                // Display the total count for all of the websites.
                ResultsTextBox.Text +=
                    $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
            }

            private List<string> SetUpURLList()
            {
                List<string> urls = new List<string>
                {
                    "https://msdn.microsoft.com/library/hh191443.aspx",
                    "https://msdn.microsoft.com/library/aa578028.aspx",
                    "https://msdn.microsoft.com/library/jj155761.aspx",
                    "https://msdn.microsoft.com/library/hh290140.aspx",
                    "https://msdn.microsoft.com/library/hh524395.aspx",
                    "https://msdn.microsoft.com/library/ms404677.aspx",
                    "https://msdn.microsoft.com",
                    "https://msdn.microsoft.com/library/ff730837.aspx"
                };
                return urls;
            }

            private void DisplayResults(string url, byte[] content, int pos)
            {
                // Display the length of each website. The string format is designed
                // to be used with a monospaced font, such as Lucida Console or
                // Global Monospace.

                // Strip off the "https://".
                var displayURL = url.Replace("https://", "");
                // Display position in the URL list, the URL, and the number of bytes.
                ResultsTextBox.Text += $"\n{pos}. {displayURL,-58} {content.Length,8}";
            }
        }
    }
    ```

11. Ctrl キーを押しながら F5 キーを押してプログラムを実行し、 **[Start]** ボタンを複数回クリックします。

12. 「[[Start] ボタンを無効にする](#BKMK_DisableTheStartButton)」、「[操作を取り消して再開する](#BKMK_CancelAndRestart)」、または「[複数の操作を実行して出力をキューに登録する](#BKMK_RunMultipleOperations)」の変更を行って再入を処理します。

## <a name="see-also"></a>参照

- [チュートリアル: async と await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Async および Await を使用した非同期プログラミング (C#)](./index.md)
