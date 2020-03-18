---
title: 'チュートリアル: Async と Await を使用した Web へのアクセス'
ms.date: 07/20/2015
ms.assetid: 84fd047f-fab8-4d89-8ced-104fb7310a91
ms.openlocfilehash: 7f649f1f16da545c4587f0ed76b8f1a443ee8744
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715849"
---
# <a name="walkthrough-accessing-the-web-by-using-async-and-await-visual-basic"></a>チュートリアル: Async と Await を使用した Web へのアクセス (Visual Basic)

async/await 機能を使用することで、非同期プログラムをより簡単かつ直感的に記述できます。 同期コードに似た非同期コードを記述し、通常の非同期コードが必要とする難しいコールバック関数や継続の処理をコンパイラに任せます。

非同期機能の詳細については、「[Async および Await を使用した非同期プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)」を参照してください。

このチュートリアルは、Web サイトの一覧でのバイト数の合計を計算する同期 Windows Presentation Foundation (WPF) アプリケーションから開始します。 その後、新しい機能を使用して、アプリケーションを非同期ソリューションに変換します。

自分でアプリケーションを作成しない場合は、[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)のページから、"Async Sample: Accessing the Web Walkthrough (C# and Visual Basic) (非同期サンプル: Web へのアクセスのチュートリアル (C# および Visual Basic))" をダウンロードできます。

このチュートリアルでは、次のタスクを行います。

> [!div class="checklist"]
>
> - [WPF アプリケーションの作成](#create-a-wpf-application)
> - [単純な WPF Mainwindow.xaml を設計する](#design-a-simple-wpf-mainwindow)
> - [参照を追加する](#add-a-reference)
> - [必要な Imports ステートメントを追加する](#add-necessary-imports-statements)
> - [同期アプリケーションを作成する](#create-a-synchronous-application)
> - [同期ソリューションをテストする](#test-the-synchronous-solution)
> - [GetURLContents を非同期メソッドに変換する](#convert-geturlcontents-to-an-asynchronous-method)
> - [SumPageSizes を非同期メソッドに変換する](#convert-sumpagesizes-to-an-asynchronous-method)
> - [StartButton_Click を非同期メソッドに変換する](#convert-startbutton_click-to-an-asynchronous-method)
> - [非同期ソリューションをテストする](#test-the-asynchronous-solution)
> - [Geturlの Async メソッドを .NET Framework メソッドに置き換えます。](#replace-the-geturlcontentsasync-method-with-a-net-framework-method)

完全な非同期の例については、「[例](#example)」を参照してください。

## <a name="prerequisites"></a>必要条件

お使いのコンピューターに、Visual Studio 2012 以降がインストールされている必要があります。 詳細については、Visual Studio の[ダウンロード](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)ページを参照してください。

## <a name="create-a-wpf-application"></a>WPF アプリケーションを作成する

1. Visual Studio を起動します。

2. メニュー バーで **[ファイル]** 、 **[新規作成]** 、 **[プロジェクト]** の順にクリックします。

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストールされたテンプレート]** ペインで、Visual Basic を選択し、プロジェクトの種類の一覧から  **[WPF アプリケーション]** を選択します。

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

2. メニュー バーで、 **[プロジェクト]** 、 **[参照の追加]** の順に選択します。

    **[参照マネージャー]** ダイアログ ボックスが表示されます。

3. ダイアログ ボックスの上部で、プロジェクトのターゲットが .NET Framework 4.5 以上であることを確認します。

4. **[アセンブリ]** で、 **[フレームワーク]** を選択します (選択されていない場合)。

5. 名前の一覧で、 **[System.Net.Http]** のチェック ボックスをオンにします。

6. **[OK]** をクリックしてダイアログ ボックスを閉じます。

## <a name="add-necessary-imports-statements"></a>必要な Imports ステートメントを追加する

1. **ソリューションエクスプローラー**で、mainwindow.xaml のショートカットメニューを開き、 **[コードの表示]** を選択します。

2. 次の `Imports` ステートメントをコードファイルの先頭に追加します (まだ存在していない場合)。

    ```vb
    Imports System.Net.Http
    Imports System.Net
    Imports System.IO
    ```

## <a name="create-a-synchronous-application"></a>同期アプリケーションを作成する

1. デザインウィンドウの Mainwindow.xaml で、 **[Start]** ボタンをダブルクリックして、mainwindow.xaml に `startButton_Click` イベントハンドラーを作成します。

2. Mainwindow.xaml で、次のコードを `startButton_Click`の本文にコピーします。

    ```vb
    resultsTextBox.Clear()
    SumPageSizes()
    resultsTextBox.Text &= vbCrLf & "Control returned to startButton_Click."
    ```

    このコードは、`SumPageSizes` アプリケーションを実行するメソッドを呼び出し、`startButton_Click` に制御が戻るとメッセージを表示します。

3. 同期ソリューションのコードには、次の 4 つのメソッドが含まれています。

    - `SumPageSizes` は、`SetUpURLList` から Web ページ URL のリストを取得し、`GetURLContents` と `DisplayResults` を呼び出して各 URL を処理します。

    - `SetUpURLList` は、Web アドレスのリストを作成して返します。

    - `GetURLContents` は、各 Web サイトのコンテンツをダウンロードし、バイト配列としてそのコンテンツを返します。

    - `DisplayResults` は、各 URL のバイト配列内のバイト数を表示します。

    次の4つのメソッドをコピーし、Mainwindow.xaml の `startButton_Click` イベントハンドラーの下に貼り付けます。

    ```vb
    Private Sub SumPageSizes()

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            ' GetURLContents returns the contents of url as a byte array.
            Dim urlContents As Byte() = GetURLContents(url)

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the web addresses.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "Total bytes returned:  {0}" & vbCrLf, total)
    End Sub

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
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
            }
        Return urls
    End Function

    Private Function GetURLContents(url As String) As Byte()

        ' The downloaded resource ends up in the variable named content.
        Dim content = New MemoryStream()

        ' Initialize an HttpWebRequest for the current URL.
        Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

        ' Send the request to the Internet resource and wait for
        ' the response.
        ' Note: you can't use HttpWebRequest.GetResponse in a Windows Store app.
        Using response As WebResponse = webReq.GetResponse()
            ' Get the data stream that is associated with the specified URL.
            Using responseStream As Stream = response.GetResponseStream()
                ' Read the bytes in responseStream and copy them to content.
                responseStream.CopyTo(content)
            End Using
        End Using

        ' Return the result as a byte array.
        Return content.ToArray()
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub
    ```

## <a name="test-the-synchronous-solution"></a>同期ソリューションをテストする

1. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。

    次の一覧のような出力が表示されます。

    ```console
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

1. 同期ソリューションを非同期ソリューションに変換する場合、開始するのが最適な場所は `GetURLContents` です。これは、<xref:System.Net.HttpWebRequest.GetResponse%2A?displayProperty=nameWithType> メソッドと <xref:System.IO.Stream.CopyTo%2A?displayProperty=nameWithType> メソッドの呼び出しがアプリケーションによって web にアクセスするためです。 .NET Framework には両方のメソッドの非同期バージョンが用意されているため、変換は簡単です。

    `GetURLContents` で使用されているメソッドの詳細については、「<xref:System.Net.WebRequest>」を参照してください。

    > [!NOTE]
    > このチュートリアルの手順に従っていると、いくつかのコンパイラ エラーが表示されます。 これらのエラーは無視することで、チュートリアルを続行できます。

    `GetURLContents` の 3 行目で呼び出されるメソッドを、`GetResponse` から、非同期でタスク ベースの <xref:System.Net.WebRequest.GetResponseAsync%2A> メソッドに変更します。

    ```vb
    Using response As WebResponse = webReq.GetResponseAsync()
    ```

2. `GetResponseAsync` は、<xref:System.Threading.Tasks.Task%601> を返します。 この場合、*タスク戻り変数*の `TResult` の型は <xref:System.Net.WebResponse> です。 このタスクは、要求されたデータのダウンロードが完了し、タスクが最後まで実行された後に、実際の `WebResponse` オブジェクトを生成するという約束です。

    タスクから `WebResponse` 値を取得するには、次のコードに示すように、`GetResponseAsync`への呼び出しに[Await](../../../../visual-basic/language-reference/operators/await-operator.md)演算子を適用します。

    ```vb
    Using response As WebResponse = Await webReq.GetResponseAsync()
    ```

    `Await` 演算子は、現在のメソッド、`GetURLContents` の実行を、待機しているタスクが完了するまで中断します。 その間、現在のメソッドの呼び出し元に制御が戻されます。 この例では、現在のメソッドが `GetURLContents` で、呼び出し元が `SumPageSizes` です。 タスクが完了すると、約束されていた `WebResponse` オブジェクトが完了したタスクの値として生成され、変数 `response` に割り当てられます。

    上記のステートメントは、動作を明確にするため、次の 2 つのステートメントに分割できます。

    ```vb
    Dim responseTask As Task(Of WebResponse) = webReq.GetResponseAsync()
    Using response As WebResponse = Await responseTask
    ```

    `webReq.GetResponseAsync` への呼び出しによって、`Task(Of WebResponse)` または `Task<WebResponse>` が返されます。 次に、`WebResponse` 値を取得するために、タスクに `Await` 演算子が適用されます。

    非同期メソッドにタスクの完了に依存しない処理がある場合、メソッドはこれら 2 つのステートメントの間、つまり非同期メソッドへの呼び出しから、await 演算子の適用までの間にその処理を続行することができます。 例については、「[方法: async と Await を使用して複数の Web 要求を並列実行する方法 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md) 」および「[方法: task.whenall (Visual Basic) を使用して非同期のチュートリアルを拡張する](../../../../visual-basic/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)」を参照してください。

3. 前の手順で `Await` 演算子を追加したため、コンパイラ エラーが発生します。 演算子は、 [Async](../../../../visual-basic/language-reference/modifiers/async.md)修飾子でマークされているメソッドでのみ使用できます。 `CopyTo` への呼び出しを `CopyToAsync` への呼び出しに置き換える変換手順を繰り返す間は、エラーを無視してください。

    - 呼び出されるメソッドの名前を <xref:System.IO.Stream.CopyToAsync%2A> に変更します。

    - `CopyTo` または `CopyToAsync` メソッドは、その引数 `content` にバイトをコピーし、意味のある値は返しません。 同期バージョンでは、`CopyTo` への呼び出しは値を返さない単純なステートメントです。 非同期バージョンでは、`CopyToAsync` は <xref:System.Threading.Tasks.Task> を返します。 タスクは "Task(void)" のように機能し、メソッドを待機できるようにします。 次のコードに示すように、`Await` または `await` を、`CopyToAsync` への呼び出しに適用します。

        ```vb
        Await responseStream.CopyToAsync(content)
        ```

         上記のステートメントでは、次の 2 行のコードを省略しています。

        ```vb
        ' CopyToAsync returns a Task, not a Task<T>.
        Dim copyTask As Task = responseStream.CopyToAsync(content)

        ' When copyTask is completed, content contains a copy of
        ' responseStream.
        Await copyTask
        ```

4. `GetURLContents` 内で必要な作業として残っているのは、メソッド シグネチャの調整のみです。 `Await` 演算子は、 [Async](../../../../visual-basic/language-reference/modifiers/async.md)修飾子でマークされているメソッドでのみ使用できます。 次のコードに示すように、修飾子を追加し、メソッドを*非同期メソッド*としてマークします。

    ```vb
    Private Async Function GetURLContents(url As String) As Byte()
    ```

5. 非同期メソッドの戻り値の型には、<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>のみを指定できます。 Visual Basic でのメソッドは、`Task` または `Task(Of T)` を返す `Function` にするか、`Sub` にする必要があります。 通常、`Sub` メソッドは、`Sub` が必要な非同期イベントハンドラーでのみ使用されます。 `Task` また、完了したメソッドに T 型の値を返す[Return](../../../../visual-basic/language-reference/statements/return-statement.md)ステートメントがあり、完成したメソッドが意味のある値を返さない場合は、`Task(T)` を使用します。

    詳細については、「[非同期の戻り値の型 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/async-return-types.md)」を参照してください。

    メソッド `GetURLContents` には return ステートメントがあり、このステートメントはバイト配列を返します。 そのため、非同期バージョンの戻り値の型は Task(T) であり、T はバイト配列です。 メソッド シグネチャに、次の変更を加えます。

    - 戻り値の型を `Task(Of Byte())` に変更します。

    - 規則により、非同期メソッドは "Async" で終わる名前を持つことになっているため、メソッドの名前を `GetURLContentsAsync` に変更します。

    これらの変更を次のコードに示します。

    ```vb
    Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())
    ```

    このいくつかの変更によって、`GetURLContents` の非同期メソッドへの変換が完了しました。

## <a name="convert-sumpagesizes-to-an-asynchronous-method"></a>SumPageSizes を非同期メソッドに変換する

1. `SumPageSizes` に対して、前述した手順を繰り返します。 まずは、`GetURLContents` への呼び出しを非同期呼び出しに変更します。

    - 呼び出されるメソッドの名前を `GetURLContents` から `GetURLContentsAsync` に変更します (まだ変更していない場合)。

    - バイト配列値を取得するために、`Await` を、`GetURLContentsAsync` が返すタスクに適用します。

    これらの変更を次のコードに示します。

    ```vb
    Dim urlContents As Byte() = Await GetURLContentsAsync(url)
    ```

    上記の割り当てでは、次の 2 行のコードを省略しています。

    ```vb
    ' GetURLContentsAsync returns a task. At completion, the task
    ' produces a byte array.
    Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
    Dim urlContents As Byte() = Await getContentsTask
    ```

2. メソッドのシグネチャに、次の変更を加えます。

    - メソッドを `Async` 修飾子でマークします。

    - メソッド名に "Async" を追加します。

    - 現時点では、タスクの戻り変数 T がありません。これは `SumPageSizesAsync` が T の値を返さないためです (このメソッドには `Return` ステートメントがありません)。ただし、メソッドは、待機可能になる `Task` を返す必要があります。 そのため、メソッドの種類を `Sub` から `Function`に変更します。 関数の戻り値の型は、`Task` です。

    これらの変更を次のコードに示します。

    ```vb
    Private Async Function SumPageSizesAsync() As Task
    ```

    `SumPageSizes` から `SumPageSizesAsync` への変換が完了しました。

## <a name="convert-startbutton_click-to-an-asynchronous-method"></a>startButton_Click を非同期メソッドに変換する

1. イベント ハンドラーで、呼び出されるメソッドの名前を `SumPageSizes` から `SumPageSizesAsync` に変更します (まだ変更していない場合)。

2. `SumPageSizesAsync` は非同期メソッドであるため、結果を待機するイベント ハンドラーのコードを変更します。

    `SumPageSizesAsync` への呼び出しは、`GetURLContentsAsync` の `CopyToAsync` への呼び出しに似ています。 この呼び出しによって、`Task(T)` ではなく `Task` が返されます。

    前述した手順と同様に、1 つまたは 2 つのステートメントを使用して、呼び出しを変換できます。 これらの変更を次のコードに示します。

    ```vb
    ' One-step async call.
    Await SumPageSizesAsync()

    ' Two-step async call.
    Dim sumTask As Task = SumPageSizesAsync()
    Await sumTask
    ```

3. 誤って操作が再入することを避けるために、次のステートメントを `startButton_Click` の先頭に追加して **[Start]** ボタンを無効にします。

    ```vb
    ' Disable the button until the operation is complete.
    startButton.IsEnabled = False
    ```

    イベント ハンドラーの末尾で、ボタンを再び有効にできます。

    ```vb
    ' Reenable the button in case you want to run the operation again.
    startButton.IsEnabled = True
    ```

    再入の詳細については、「[非同期アプリでの再入の処理 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/handling-reentrancy-in-async-apps.md)」を参照してください。

4. 最後に、`Async` 修飾子を宣言に追加し、イベント ハンドラーが `SumPagSizesAsync` を待機できるようにします。

    ```vb
    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click
    ```

    通常、イベント ハンドラーの名前は変更されません。 イベントハンドラーは Visual Basic 内の `Sub` プロシージャである必要があるため、戻り値の型を `Task` に変更することはできません。

    同期処理から非同期処理へのプロジェクトの変換が完了しました。

## <a name="test-the-asynchronous-solution"></a>非同期ソリューションをテストする

1. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。

2. 同期ソリューションの出力に似た出力が表示されます。 ただし、次の相違点に注意してください。

    - 処理の完了後に、すべての結果が同時に表示されることはありません。 たとえば、両方のプログラムの `startButton_Click` には、テキスト ボックスをクリアする行が含まれています。 この目的は、実行ごとにテキスト ボックスをクリアすることです。1 つの結果セットが表示された後に、もう一度 **[Start]** ボタンをクリックすると、テキスト ボックスがクリアされます。 同期バージョンでは、2 回目のカウントが表示される直前、ダウンロードが完了して UI スレッドが他の処理を実行できる状態になったときにテキスト ボックスがクリアされます。 非同期バージョンでは、 **[Start]** ボタンをクリックした直後にテキスト ボックスがクリアされます。

    - 最も重要な点は、ダウンロード中に UI スレッドがブロックされないことです。 Web リソースをダウンロード、カウント、および表示している間に、ウィンドウの移動やサイズ変更を行うことができます。 いずれかの Web サイトの処理が遅い、または応答しない場合、**閉じる**ボタン (右上隅の赤色のフィールドにある [x]) をクリックすることで、操作を取り消すことができます。

## <a name="replace-the-geturlcontentsasync-method-with-a-net-framework-method"></a>Geturlの Async メソッドを .NET Framework メソッドに置き換えます。

1. .NET Framework には、使用できる非同期メソッドが数多く用意されています。 そのうちの1つは、このチュートリアルで必要なのは、<xref:System.Net.Http.HttpClient.GetByteArrayAsync%28System.String%29?displayProperty=nameWithType> メソッドです。 これを、前述の手順で作成した `GetURLContentsAsync` メソッドの代わりに使用できます。

    最初の手順では、`SumPageSizesAsync` メソッドに <xref:System.Net.Http.HttpClient> オブジェクトを作成します。 次の宣言をメソッドの先頭に追加します。

    ```vb
    ' Declare an HttpClient object and increase the buffer size. The
    ' default buffer size is 65,536.
    Dim client As HttpClient =
        New HttpClient() With {.MaxResponseContentBufferSize = 1000000}
    ```

2. `SumPageSizesAsync,` で、`GetURLContentsAsync` メソッドへの呼び出しを `HttpClient` メソッドへの呼び出しに置き換えます。

    ```vb
    Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)
    ```

3. 記述した `GetURLContentsAsync` メソッドを削除するかコメント アウトします。

4. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。

    このバージョンのプロジェクトの動作は、「非同期ソリューションをテストするには」の手順で説明している動作と同じですが、さらに少ない手間で作成できます。

## <a name="example"></a>使用例

非同期 `GetURLContentsAsync` メソッドを使用する変換された非同期ソリューションの完全な例を次に示します。 この例は、元の同期ソリューションと非常によく似ています。

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        ' Disable the button until the operation is complete.
        startButton.IsEnabled = False

        resultsTextBox.Clear()

        '' One-step async call.
        Await SumPageSizesAsync()

        ' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."

        ' Reenable the button in case you want to run the operation again.
        startButton.IsEnabled = True
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            Dim urlContents As Byte() = Await GetURLContentsAsync(url)

            ' The previous line abbreviates the following two assignment statements.

            '//<snippet21>
            ' GetURLContentsAsync returns a task. At completion, the task
            ' produces a byte array.
            'Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
            'Dim urlContents As Byte() = Await getContentsTask

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
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
            }
        Return urls
    End Function

    Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())

        ' The downloaded resource ends up in the variable named content.
        Dim content = New MemoryStream()

        ' Initialize an HttpWebRequest for the current URL.
        Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

        ' Send the request to the Internet resource and wait for
        ' the response.
        Using response As WebResponse = Await webReq.GetResponseAsync()

            ' The previous statement abbreviates the following two statements.

            'Dim responseTask As Task(Of WebResponse) = webReq.GetResponseAsync()
            'Using response As WebResponse = Await responseTask

            ' Get the data stream that is associated with the specified URL.
            Using responseStream As Stream = response.GetResponseStream()
                ' Read the bytes in responseStream and copy them to content.
                Await responseStream.CopyToAsync(content)

                ' The previous statement abbreviates the following two statements.

                ' CopyToAsync returns a Task, not a Task<T>.
                'Dim copyTask As Task = responseStream.CopyToAsync(content)

                ' When copyTask is completed, content contains a copy of
                ' responseStream.
                'Await copyTask
            End Using
        End Using

        ' Return the result as a byte array.
        Return content.ToArray()
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub

End Class
```

次のコードには、`HttpClient` の `GetByteArrayAsync` メソッドを使用するソリューション例のすべてが含まれています。

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        resultsTextBox.Clear()

        ' Disable the button until the operation is complete.
        startButton.IsEnabled = False

        ' One-step async call.
        Await SumPageSizesAsync()

        ' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."

        ' Reenable the button in case you want to run the operation again.
        startButton.IsEnabled = True
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Declare an HttpClient object and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            ' GetByteArrayAsync returns a task. At completion, the task
            ' produces a byte array.
            Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

            ' The following two lines can replace the previous assignment statement.
            'Dim getContentsTask As Task(Of Byte()) = client.GetByteArrayAsync(url)
            'Dim urlContents As Byte() = Await getContentsTask

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
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
            }
        Return urls
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub

End Class
```

## <a name="see-also"></a>参照

- [非同期サンプル: Web へのアクセスのチュートリアル (C# および Visual Basic)](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)
- [Await 演算子](../../../language-reference/operators/await-operator.md)
- [Async](../../../language-reference/modifiers/async.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](index.md)
- [非同期の戻り値の型 (Visual Basic)](async-return-types.md)
- [タスク ベースの非同期プログラミング (TAP)](https://www.microsoft.com/download/details.aspx?id=19957)
- [方法: Task.WhenAll を使用して AsyncWalkthrough を拡張する (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
- [方法: Async と Await を使用して複数の Web 要求を並列実行する (Visual Basic)](how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)
