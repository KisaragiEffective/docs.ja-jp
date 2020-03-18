---
title: 'チュートリアル: Async と Await を使用した Web へのアクセス (C#)'
ms.date: 07/20/2015
ms.assetid: c95d8d71-5a98-4bf0-aaf4-45fed2ebbacd
ms.openlocfilehash: 42b09dab26fd514e184163eaf41aff117d3a463f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74281791"
---
# <a name="walkthrough-accessing-the-web-by-using-async-and-await-c"></a>チュートリアル: Async と Await を使用した Web へのアクセス (C#)

async/await 機能を使用することで、非同期プログラムをより簡単かつ直感的に記述できます。 同期コードに似た非同期コードを記述し、通常の非同期コードが必要とする難しいコールバック関数や継続の処理をコンパイラに任せます。

非同期機能について詳しくは、「[Async および Await を使用した非同期プログラミング (C# および Visual Basic)](./index.md)」をご覧ください。

このチュートリアルは、Web サイトの一覧でのバイト数の合計を計算する同期 Windows Presentation Foundation (WPF) アプリケーションから開始します。 その後、新しい機能を使用して、アプリケーションを非同期ソリューションに変換します。

自分でアプリケーションを作成しない場合は、[非同期サンプル:Web へのアクセスのチュートリアル (C# および Visual Basic)](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f) をダウンロードできます。

> [!NOTE]
> この例を実行するには、コンピューターに Visual Studio 2012 以降および .NET Framework 4.5 以降がインストールされている必要があります。

## <a name="create-a-wpf-application"></a>WPF アプリケーションを作成する

1. Visual Studio を起動します。

2. メニュー バーで、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** を選択します。

     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストールされたテンプレート]** ウィンドウで、[Visual C#] をクリックし、プロジェクトの種類の一覧で **[WPF アプリケーション]** をクリックします。

4. **[名前]** ボックスに「`AsyncExampleWPF`」と入力して、 **[OK]** を選択します。

     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

## <a name="design-a-simple-wpf-mainwindow"></a>単純な WPF MainWindow をデザインする

1. Visual Studio コード エディターで、 **[MainWindow.xaml]** タブをクリックします。

2. **[ツールボックス]** ウィンドウが表示されていない場合は、 **[表示]** メニューを開き、 **[ツールボックス]** をクリックします。

3. **[Button]** コントロールと **[TextBox]** コントロールを **[MainWindow]** ウィンドウに追加します。

4. **[TextBox]** コントロールを強調表示し、 **[プロパティ]** ウィンドウで次の値を設定します。

    - **[Name]** プロパティを `resultsTextBox` に設定します。

    - **[Height]** プロパティを 250 に設定します。

    - **[Width]** プロパティを 500 に設定します。

    - **[テキスト]** タブで、Lucida Console や Global Monospace などの等幅フォントを指定します。

5. **[Button]** コントロールを強調表示し、 **[プロパティ]** ウィンドウで次の値を設定します。

    - **[Name]** プロパティを `startButton` に設定します。

    - **[Content]** プロパティの値を **[Button]** から **[Start]** に変更します。

6. テキスト ボックスとボタンの位置を調整し、両方が **[MainWindow]** ウィンドウ内に表示されるようにします。

     WPF XAML デザイナーについて詳しくは、「[XAML デザイナーを使用した UI の作成](/visualstudio/xaml-tools/creating-a-ui-by-using-xaml-designer-in-visual-studio)」をご覧ください。

## <a name="add-a-reference"></a>参照を追加する

1. **ソリューション エクスプローラー**で、プロジェクトの名前を強調表示します。

2. メニュー バーで、 **[プロジェクト]**  >  **[参照の追加]** の順に選択します。

     **[参照マネージャー]** ダイアログ ボックスが表示されます。

3. ダイアログ ボックスの上部で、プロジェクトのターゲットが .NET Framework 4.5 以上であることを確認します。

4. **[アセンブリ]** カテゴリで、 **[フレームワーク]** を選択します (選択されていない場合)。

5. 名前の一覧で、 **[System.Net.Http]** のチェック ボックスをオンにします。

6. **[OK]** をクリックしてダイアログ ボックスを閉じます。

## <a name="add-necessary-using-directives"></a>ディレクティブを使用して必要なものを追加する

1. **ソリューション エクスプローラー**で MainWindow.xaml.cs のショートカット メニューを開き、 **[コードの表示]** を選択します。

2. 次の `using` ディレクティブが含まれていない場合は、コード ファイルの先頭に追加します。

    ```csharp
    using System.Net.Http;
    using System.Net;
    using System.IO;
    ```

## <a name="create-a-synchronous-app"></a>同期アプリを作成する

1. デザイン ウィンドウの MainWindow.xaml で、 **[Start]** ボタンをダブルクリックして、MainWindow.xaml.cs に `startButton_Click` イベント ハンドラーを作成します。

2. MainWindow.xaml.cs で、次のコードを `startButton_Click` の本文にコピーします。

    ```csharp
    resultsTextBox.Clear();
    SumPageSizes();
    resultsTextBox.Text += "\r\nControl returned to startButton_Click.";
    ```

    このコードは、`SumPageSizes` アプリケーションを実行するメソッドを呼び出し、`startButton_Click` に制御が戻るとメッセージを表示します。

3. 同期ソリューションのコードには、次の 4 つのメソッドが含まれています。

    - `SumPageSizes` は、`SetUpURLList` から Web ページ URL のリストを取得し、`GetURLContents` と `DisplayResults` を呼び出して各 URL を処理します。

    - `SetUpURLList` は、Web アドレスのリストを作成して返します。

    - `GetURLContents` は、各 Web サイトのコンテンツをダウンロードし、バイト配列としてそのコンテンツを返します。

    - `DisplayResults` は、各 URL のバイト配列内のバイト数を表示します。

    次の 4 つのメソッドをコピーし、それを MainWindow.xaml.cs の `startButton_Click` イベント ハンドラーの下に貼り付けます。

    ```csharp
    private void SumPageSizes()
    {
        // Make a list of web addresses.
        List<string> urlList = SetUpURLList();

        var total = 0;
        foreach (var url in urlList)
        {
            // GetURLContents returns the contents of url as a byte array.
            byte[] urlContents = GetURLContents(url);

            DisplayResults(url, urlContents);

            // Update the total.
            total += urlContents.Length;
        }

        // Display the total count for all of the web addresses.
        resultsTextBox.Text += $"\r\n\r\nTotal bytes returned:  {total}\r\n";
    }

    private List<string> SetUpURLList()
    {
        var urls = new List<string>
        {
            "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
            "https://msdn.microsoft.com",
            "https://msdn.microsoft.com/library/hh290136.aspx",
            "https://msdn.microsoft.com/library/ee256749.aspx",
            "https://msdn.microsoft.com/library/hh290138.aspx",
            "https://msdn.microsoft.com/library/hh290140.aspx",
            "https://msdn.microsoft.com/library/dd470362.aspx",
            "https://msdn.microsoft.com/library/aa578028.aspx",
            "https://msdn.microsoft.com/library/ms404677.aspx",
            "https://msdn.microsoft.com/library/ff730837.aspx"
        };
        return urls;
    }

    private byte[] GetURLContents(string url)
    {
        // The downloaded resource ends up in the variable named content.
        var content = new MemoryStream();

        // Initialize an HttpWebRequest for the current URL.
        var webReq = (HttpWebRequest)WebRequest.Create(url);

        // Send the request to the Internet resource and wait for
        // the response.
        // Note: you can't use HttpWebRequest.GetResponse in a Windows Store app.
        using (WebResponse response = webReq.GetResponse())
        {
            // Get the data stream that is associated with the specified URL.
            using (Stream responseStream = response.GetResponseStream())
            {
                // Read the bytes in responseStream and copy them to content.
                responseStream.CopyTo(content);
            }
        }

        // Return the result as a byte array.
        return content.ToArray();
    }

    private void DisplayResults(string url, byte[] content)
    {
        // Display the length of each website. The string format
        // is designed to be used with a monospaced font, such as
        // Lucida Console or Global Monospace.
        var bytes = content.Length;
        // Strip off the "https://".
        var displayURL = url.Replace("https://", "");
        resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
    }
    ```

## <a name="test-the-synchronous-solution"></a>同期ソリューションをテストする

**F5** キーを押してプログラムを実行し、 **[スタート]** を複数回クリックします。

次の一覧のような出力が表示されます。

```text
msdn.microsoft.com/library/windows/apps/br211380.aspx        383832
msdn.microsoft.com                                            33964
msdn.microsoft.com/library/hh290136.aspx               225793
msdn.microsoft.com/library/ee256749.aspx               143577
msdn.microsoft.com/library/hh290138.aspx               237372
msdn.microsoft.com/library/hh290140.aspx               128279
msdn.microsoft.com/library/dd470362.aspx               157649
msdn.microsoft.com/library/aa578028.aspx               204457
msdn.microsoft.com/library/ms404677.aspx               176405
msdn.microsoft.com/library/ff730837.aspx               143474

Total bytes returned:  1834802

Control returned to startButton_Click.
```

カウントの表示には数秒かかる点に注意してください。 その間、要求されたリソースのダウンロードが完了するまで UI スレッドがブロックされます。 このため、 **[Start]** ボタンのクリック後は、表示ウィンドウの移動、最大化、最小化のほか、閉じることさえできなくなります。 バイト カウントの表示が開始するまでは、これらの操作を実行しても失敗します。 Web サイトが応答していない場合、どのサイトに問題があるのかを示す情報は表示されません。 待つのをやめて、プログラムを閉じることさえ難しい状態になります。

## <a name="convert-geturlcontents-to-an-asynchronous-method"></a>GetURLContents を非同期メソッドに変換する

1. 同期ソリューションを非同期ソリューションに変換する際に、最初に取りかかるのに最適な場所は、`GetURLContents` 内です。その理由は、<xref:System.Net.HttpWebRequest> の <xref:System.Net.HttpWebRequest.GetResponse%2A> メソッドおよび <xref:System.IO.Stream> の <xref:System.IO.Stream.CopyTo%2A> メソッドへの呼び出しで、アプリケーションが Web にアクセスするためです。 .NET Framework には両方のメソッドの非同期バージョンが用意されているため、変換は簡単です。

     `GetURLContents` で使用されているメソッドの詳細については、「<xref:System.Net.WebRequest>」を参照してください。

    > [!NOTE]
    > このチュートリアルの手順に従っていると、いくつかのコンパイラ エラーが表示されます。 これらのエラーは無視することで、チュートリアルを続行できます。

     `GetURLContents` の 3 行目で呼び出されるメソッドを、`GetResponse` から、非同期でタスク ベースの <xref:System.Net.WebRequest.GetResponseAsync%2A> メソッドに変更します。

    ```csharp
    using (WebResponse response = webReq.GetResponseAsync())
    ```

2. `GetResponseAsync` は、<xref:System.Threading.Tasks.Task%601> を返します。 この場合、*タスク戻り変数*の `TResult` の型は <xref:System.Net.WebResponse> です。 このタスクは、要求されたデータのダウンロードが完了し、タスクが最後まで実行された後に、実際の `WebResponse` オブジェクトを生成するという約束です。

     タスクから `WebResponse` 値を取得するには、次のコードに示すように、[await](../../../language-reference/operators/await.md) (C#) 演算子を `GetResponseAsync` への呼び出しに適用します。

    ```csharp
    using (WebResponse response = await webReq.GetResponseAsync())
    ```

     `await` 演算子は、現在のメソッド、`GetURLContents` の実行を、待機しているタスクが完了するまで中断します。 その間、現在のメソッドの呼び出し元に制御が戻されます。 この例では、現在のメソッドが `GetURLContents` で、呼び出し元が `SumPageSizes` です。 タスクが完了すると、約束されていた `WebResponse` オブジェクトが完了したタスクの値として生成され、変数 `response` に割り当てられます。

     上記のステートメントは、動作を明確にするため、次の 2 つのステートメントに分割できます。

    ```csharp
    //Task<WebResponse> responseTask = webReq.GetResponseAsync();
    //using (WebResponse response = await responseTask)
    ```

     `webReq.GetResponseAsync` への呼び出しによって、`Task(Of WebResponse)` または `Task<WebResponse>` が返されます。 その後、`WebResponse` 値を取得するため、タスクに await 演算子が適用されます。

     非同期メソッドにタスクの完了に依存しない処理がある場合、メソッドはこれら 2 つのステートメントの間、つまり非同期メソッドへの呼び出しから、`await` 演算子の適用までの間にその処理を続行することができます。 この例については、「[async と await を使用して複数の Web 要求を並列実行する方法 (C#)](./how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)」および「[Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)」を参照してください。

3. 前の手順で `await` 演算子を追加したため、コンパイラ エラーが発生します。 この演算子は、[async](../../../language-reference/keywords/async.md) 修飾子でマークされているメソッドでのみ使用できます。 `CopyTo` への呼び出しを `CopyToAsync` への呼び出しに置き換える変換手順を繰り返す間は、エラーを無視してください。

    - 呼び出されるメソッドの名前を <xref:System.IO.Stream.CopyToAsync%2A> に変更します。

    - `CopyTo` または `CopyToAsync` メソッドは、その引数 `content` にバイトをコピーし、意味のある値は返しません。 同期バージョンでは、`CopyTo` への呼び出しは値を返さない単純なステートメントです。 非同期バージョンでは、`CopyToAsync` は <xref:System.Threading.Tasks.Task> を返します。 タスクは "Task(void)" のように機能し、メソッドを待機できるようにします。 次のコードに示すように、`Await` または `await` を、`CopyToAsync` への呼び出しに適用します。

        ```csharp
        await responseStream.CopyToAsync(content);
        ```

         上記のステートメントでは、次の 2 行のコードを省略しています。

        ```csharp
        // CopyToAsync returns a Task, not a Task<T>.
        //Task copyTask = responseStream.CopyToAsync(content);

        // When copyTask is completed, content contains a copy of
        // responseStream.
        //await copyTask;
        ```

4. `GetURLContents` 内で必要な作業として残っているのは、メソッド シグネチャの調整のみです。 `await` 演算子は、[async](../../../language-reference/keywords/async.md) 修飾子でマークされているメソッドでのみ使用できます。 次のコードに示すように、修飾子を追加し、メソッドを*非同期メソッド*としてマークします。

    ```csharp
    private async byte[] GetURLContents(string url)
    ```

5. C# での非同期メソッドの戻り値の型は、<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、または `void` のみを指定できます。 通常、`void` の戻り値の型は、`void` を必要とする非同期イベント ハンドラーでのみ使用します。 それ以外のケースでは、完成したメソッドに、T 型の値を返す [return](../../../language-reference/keywords/return.md) ステートメントが含まれる場合は `Task(T)` を使用し、完成したメソッドが意味のある値を返さない場合は `Task` を使用します。 戻り値の型 `Task` は、"Task(void)" を意味するものと考えることができます。

     詳しくは、「[非同期の戻り値の型 (C#)](./async-return-types.md)」をご覧ください。

     メソッド `GetURLContents` には return ステートメントがあり、このステートメントはバイト配列を返します。 そのため、非同期バージョンの戻り値の型は Task(T) であり、T はバイト配列です。 メソッド シグネチャに、次の変更を加えます。

    - 戻り値の型を `Task<byte[]>` に変更します。

    - 規則により、非同期メソッドは "Async" で終わる名前を持つことになっているため、メソッドの名前を `GetURLContentsAsync` に変更します。

     これらの変更を次のコードに示します。

    ```csharp
    private async Task<byte[]> GetURLContentsAsync(string url)
    ```

     このいくつかの変更によって、`GetURLContents` の非同期メソッドへの変換が完了しました。

## <a name="convert-sumpagesizes-to-an-asynchronous-method"></a>SumPageSizes を非同期メソッドに変換する

1. `SumPageSizes` に対して、前述した手順を繰り返します。 まずは、`GetURLContents` への呼び出しを非同期呼び出しに変更します。

    - 呼び出されるメソッドの名前を `GetURLContents` から `GetURLContentsAsync` に変更します (まだ変更していない場合)。

    - バイト配列値を取得するために、`await` を、`GetURLContentsAsync` が返すタスクに適用します。

     これらの変更を次のコードに示します。

    ```csharp
    byte[] urlContents = await GetURLContentsAsync(url);
    ```

     上記の割り当てでは、次の 2 行のコードを省略しています。

    ```csharp
    // GetURLContentsAsync returns a Task<T>. At completion, the task
    // produces a byte array.
    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
    //byte[] urlContents = await getContentsTask;
    ```

2. メソッドのシグネチャに、次の変更を加えます。

    - メソッドを `async` 修飾子でマークします。

    - メソッド名に "Async" を追加します。

    - 今回、タスク戻り変数の T がない理由は、`SumPageSizesAsync` が T のための値を返さないからです (メソッドに `return` ステートメントがありません)。ただし、メソッドは待機可能になるために `Task` を返す必要があります。 そのため、メソッドの戻り値の型を `void` から `Task` に変更します。

    これらの変更を次のコードに示します。

    ```csharp
    private async Task SumPageSizesAsync()
    ```

     `SumPageSizes` から `SumPageSizesAsync` への変換が完了しました。

## <a name="convert-startbutton_click-to-an-asynchronous-method"></a>startButton_Click を非同期メソッドに変換する

1. イベント ハンドラーで、呼び出されるメソッドの名前を `SumPageSizes` から `SumPageSizesAsync` に変更します (まだ変更していない場合)。

2. `SumPageSizesAsync` は非同期メソッドであるため、結果を待機するイベント ハンドラーのコードを変更します。

     `SumPageSizesAsync` への呼び出しは、`GetURLContentsAsync` の `CopyToAsync` への呼び出しに似ています。 この呼び出しによって、`Task(T)` ではなく `Task` が返されます。

     前述した手順と同様に、1 つまたは 2 つのステートメントを使用して、呼び出しを変換できます。 これらの変更を次のコードに示します。

    ```csharp
    // One-step async call.
    await SumPageSizesAsync();

    // Two-step async call.
    //Task sumTask = SumPageSizesAsync();
    //await sumTask;
    ```

3. 誤って操作が再入することを避けるために、次のステートメントを `startButton_Click` の先頭に追加して **[Start]** ボタンを無効にします。

    ```csharp
    // Disable the button until the operation is complete.
    startButton.IsEnabled = false;
    ```

     イベント ハンドラーの末尾で、ボタンを再び有効にできます。

    ```csharp
    // Reenable the button in case you want to run the operation again.
    startButton.IsEnabled = true;
    ```

     再入について詳しくは、「[非同期アプリにおける再入の処理 (C#)](./handling-reentrancy-in-async-apps.md)」をご覧ください。

4. 最後に、`async` 修飾子を宣言に追加し、イベント ハンドラーが `SumPagSizesAsync` を待機できるようにします。

    ```csharp
    private async void startButton_Click(object sender, RoutedEventArgs e)
    ```

     通常、イベント ハンドラーの名前は変更されません。 戻り値の型が `Task` に変更されていない理由は、イベント ハンドラーが `void` を返す必要があるためです。

     同期処理から非同期処理へのプロジェクトの変換が完了しました。

## <a name="test-the-asynchronous-solution"></a>非同期ソリューションをテストする

1. **F5** キーを押してプログラムを実行し、 **[スタート]** を複数回クリックします。

2. 同期ソリューションの出力に似た出力が表示されます。 ただし、次の相違点に注意してください。

    - 処理の完了後に、すべての結果が同時に表示されることはありません。 たとえば、両方のプログラムの `startButton_Click` には、テキスト ボックスをクリアする行が含まれています。 この目的は、実行ごとにテキスト ボックスをクリアすることです。1 つの結果セットが表示された後に、もう一度 **[Start]** ボタンをクリックすると、テキスト ボックスがクリアされます。 同期バージョンでは、2 回目のカウントが表示される直前、ダウンロードが完了して UI スレッドが他の処理を実行できる状態になったときにテキスト ボックスがクリアされます。 非同期バージョンでは、 **[Start]** ボタンをクリックした直後にテキスト ボックスがクリアされます。

    - 最も重要な点は、ダウンロード中に UI スレッドがブロックされないことです。 Web リソースをダウンロード、カウント、および表示している間に、ウィンドウの移動やサイズ変更を行うことができます。 いずれかの Web サイトの処理が遅い、または応答しない場合、**閉じる**ボタン (右上隅の赤色のフィールドにある [x]) をクリックすることで、操作を取り消すことができます。

## <a name="replace-method-geturlcontentsasync-with-a-net-framework-method"></a>GetURLContentsAsync メソッドを .NET Framework メソッドに置き換える

1. .NET Framework 4.5 では、使用できる非同期メソッドが数多く用意されています。 その 1 つである、<xref:System.Net.Http.HttpClient> の <xref:System.Net.Http.HttpClient.GetByteArrayAsync%28System.String%29> メソッドは、このチュートリアルに必要な処理だけを実行します。 これを、前述の手順で作成した `GetURLContentsAsync` メソッドの代わりに使用できます。

     まずは、`SumPageSizesAsync` メソッドに `HttpClient` オブジェクトを作成します。 次の宣言をメソッドの先頭に追加します。

    ```csharp
    // Declare an HttpClient object and increase the buffer size. The
    // default buffer size is 65,536.
    HttpClient client =
        new HttpClient() { MaxResponseContentBufferSize = 1000000 };
    ```

2. `SumPageSizesAsync,` で、`GetURLContentsAsync` メソッドへの呼び出しを `HttpClient` メソッドへの呼び出しに置き換えます。

    ```csharp
    byte[] urlContents = await client.GetByteArrayAsync(url);
    ```

3. 記述した `GetURLContentsAsync` メソッドを削除するかコメント アウトします。

4. **F5** キーを押してプログラムを実行し、 **[スタート]** を複数回クリックします。

     このバージョンのプロジェクトの動作は、「非同期ソリューションをテストするには」の手順で説明している動作と同じですが、さらに少ない手間で作成できます。

## <a name="example-code"></a>コード例

次のコードには、記述した非同期 `GetURLContentsAsync` メソッドを使用する、同期ソリューションから非同期ソリューションへの変換例のすべてが含まれています。 この例は、元の同期ソリューションと非常によく似ています。

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
using System.IO;
using System.Net;

namespace AsyncExampleWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            // Disable the button until the operation is complete.
            startButton.IsEnabled = false;

            resultsTextBox.Clear();

            // One-step async call.
            await SumPageSizesAsync();

            // Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";

            // Reenable the button in case you want to run the operation again.
            startButton.IsEnabled = true;
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            var total = 0;

            foreach (var url in urlList)
            {
                byte[] urlContents = await GetURLContentsAsync(url);

                // The previous line abbreviates the following two assignment statements.

                // GetURLContentsAsync returns a Task<T>. At completion, the task
                // produces a byte array.
                //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
                //byte[] urlContents = await getContentsTask;

                DisplayResults(url, urlContents);

                // Update the total.
                total += urlContents.Length;
            }
            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        private async Task<byte[]> GetURLContentsAsync(string url)
        {
            // The downloaded resource ends up in the variable named content.
            var content = new MemoryStream();

            // Initialize an HttpWebRequest for the current URL.
            var webReq = (HttpWebRequest)WebRequest.Create(url);

            // Send the request to the Internet resource and wait for
            // the response.
            using (WebResponse response = await webReq.GetResponseAsync())

            // The previous statement abbreviates the following two statements.

            //Task<WebResponse> responseTask = webReq.GetResponseAsync();
            //using (WebResponse response = await responseTask)
            {
                // Get the data stream that is associated with the specified url.
                using (Stream responseStream = response.GetResponseStream())
                {
                    // Read the bytes in responseStream and copy them to content.
                    await responseStream.CopyToAsync(content);

                    // The previous statement abbreviates the following two statements.

                    // CopyToAsync returns a Task, not a Task<T>.
                    //Task copyTask = responseStream.CopyToAsync(content);

                    // When copyTask is completed, content contains a copy of
                    // responseStream.
                    //await copyTask;
                }
            }
            // Return the result as a byte array.
            return content.ToArray();
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each website. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

次のコードには、`HttpClient` の `GetByteArrayAsync` メソッドを使用するソリューション例のすべてが含まれています。

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
using System.IO;
using System.Net;

namespace AsyncExampleWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // Disable the button until the operation is complete.
            startButton.IsEnabled = false;

            // One-step async call.
            await SumPageSizesAsync();

            //// Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";

            // Reenable the button in case you want to run the operation again.
            startButton.IsEnabled = true;
        }

        private async Task SumPageSizesAsync()
        {
            // Declare an HttpClient object and increase the buffer size. The
            // default buffer size is 65,536.
            HttpClient client =
                new HttpClient() { MaxResponseContentBufferSize = 1000000 };

            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            var total = 0;

            foreach (var url in urlList)
            {
                // GetByteArrayAsync returns a task. At completion, the task
                // produces a byte array.
                byte[] urlContents = await client.GetByteArrayAsync(url);

                // The following two lines can replace the previous assignment statement.
                //Task<byte[]> getContentsTask = client.GetByteArrayAsync(url);
                //byte[] urlContents = await getContentsTask;

                DisplayResults(url, urlContents);

                // Update the total.
                total += urlContents.Length;
            }

            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each website. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

## <a name="see-also"></a>関連項目

- [Async Sample:Web へのアクセスのチュートリアル (C# および Visual Basic)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/hh300224(v=vs.110))
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
- [Async および Await を使用した非同期プログラミング (C#)](./index.md)
- [非同期の戻り値の型 (C#)](./async-return-types.md)
- [タスク ベースの非同期プログラミング (TAP)](https://www.microsoft.com/download/details.aspx?id=19957)
- [Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
- [async と await を使用して複数の Web 要求を並列実行する方法 (C#)](./how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)
