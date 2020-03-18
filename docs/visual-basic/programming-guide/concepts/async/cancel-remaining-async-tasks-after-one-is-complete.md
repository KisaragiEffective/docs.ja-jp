---
title: 完了後の残りの非同期タスクのキャンセル
ms.date: 07/20/2015
ms.assetid: c928b5a1-622f-4441-8baf-adca1dde197f
ms.openlocfilehash: e6747f35e665611ac7a48a87f955c8b893ee2b99
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347922"
---
# <a name="cancel-remaining-async-tasks-after-one-is-complete-visual-basic"></a>完了後の残りの非同期タスクのキャンセル (Visual Basic)

<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> メソッドを <xref:System.Threading.CancellationToken> と共に使用すると、1 つのタスクが完了したときに残りのすべてのタスクを取り消しできます。 `WhenAny` メソッドは、タスクのコレクションである引数を受け取ります。 このメソッドは、すべてのタスクを開始し、単一のタスクを返します。 単一のタスクは、コレクションのいずれかのタスクが完了すると完了します。

この例では、キャンセル トークンを `WhenAny` と共に使用して、タスクのコレクションから最初のタスクを終了まで保持し、残りのタスクを取り消す方法を示しています。 各タスクは、Web サイトのコンテンツをダウンロードします。 この例は最初のダウンロードが完了したコンテンツの長さを表示し、他のダウンロードを取り消します。

> [!NOTE]
> この例を実行するには、Visual Studio 2012 以降および .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。

## <a name="downloading-the-example"></a>例をダウンロードする

完全な Windows Presentation Foundation (WPF) プロジェクトは「[Async Sample: Fine Tuning Your Application (非同期のサンプル: アプリケーションの微調整)](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」からダウンロードできます。ダウンロード後、次の手順に従います。

1. ダウンロードしたファイルを圧縮解除し、Visual Studio を起動します。

2. メニュー バーで **[ファイル]** 、 **[開く]** 、 **[プロジェクト/ソリューション]** の順に選択します。

3. **[プロジェクトを開く]** ダイアログ ボックスで、圧縮解除したサンプル コードを含むフォルダーを開き、AsyncFineTuningVB 用のソリューション (.sln) ファイルを開きます。

4. **ソリューション エクスプローラー**で、**CancelAfterOneTask** プロジェクトのショートカット メニューを開き、 **[スタートアップ プロジェクトに設定]** をクリックします。

5. F5 キーを押してプロジェクトを実行します。

    Ctrl + F5 キーを押して、デバッグを行わずにプロジェクトを実行します。

6. プログラムを複数回実行し、最初に終了するダウンロードが異なることを確認します。

プロジェクトをダウンロードしない場合は、このトピックの最後の MainWindow.xaml.vb ファイルをレビューできます。

## <a name="building-the-example"></a>例のビルド

このトピックの例では、「[非同期タスクのキャンセル](../../../../visual-basic/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md)」で開発したプロジェクトに追加し、タスクの一覧をキャンセルするタスクの一覧を追加します。 この例では、 **[キャンセル]** ボタンは明示的に使用していませんが、同じ UI を使用します。

この例を自分で 1 つずつビルドするには、"例をダウンロードする" セクションの手順に従います。ただし、 **[スタートアップ プロジェクト]** として **CancelAListOfTasks** を選択します。 そのプロジェクトに、このトピックでの変更を追加します。

**Cancelmainwindow.xaml Stoftasks**プロジェクトのファイルで、各 web サイトの処理手順を `AccessTheWebAsync` のループから次の非同期メソッドに移動して、移行を開始します。

```vb
' ***Bundle the processing steps for a website into one async method.
Async Function ProcessURLAsync(url As String, client As HttpClient, ct As CancellationToken) As Task(Of Integer)

    ' GetAsync returns a Task(Of HttpResponseMessage).
    Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

    ' Retrieve the website contents from the HttpResponseMessage.
    Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

    Return urlContents.Length
End Function
```

この例では、`AccessTheWebAsync` で、<xref:System.Linq.Enumerable.ToArray%2A> メソッドのクエリと `WhenAny` メソッドを使用して、タスクの配列を作成して開始します。 配列への `WhenAny` のアプリケーションは、待機したときに、タスクの配列で完了に到達する最初のタスクを評価する 1 つのタスクを返します。

`AccessTheWebAsync` で次の変更を行います。 アスタリスクはコード ファイルの変更点を示しています。

1. ループをコメント アウトするか、削除します。

2. 実行されると、一般的なタスクのコレクションを生成するクエリを作成します。 `ProcessURLAsync` に対する各呼び出しは、<xref:System.Threading.Tasks.Task%601> が整数である `TResult` を返します。

    ```vb
    ' ***Create a query that, when executed, returns a collection of tasks.
    Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
        From url In urlList Select ProcessURLAsync(url, client, ct)
    ```

3. `ToArray` を呼び出してクエリを実行し、タスクを開始します。 次の手順で `WhenAny` メソッドのアプリケーションは、`ToArray` を使用せずにクエリを実行してタスクを開始しますが、他のメソッドはそうでない場合があります。 最も安全な方法は、クエリの実行を明示的に強制することです。

    ```vb
    ' ***Use ToArray to execute the query and start the download tasks.
    Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()
    ```

4. タスクのコレクションで `WhenAny` を呼び出します。 `WhenAny` は `Task(Of Task(Of Integer))` または `Task<Task<int>>` を返します。  つまり、`WhenAny` は、待機すると、単一の `Task(Of Integer)` または `Task<int>` に評価するタスクを返します。 その単一のタスクが、コレクションで最初に終了するタスクです。 最初に終了したタスクは `firstFinishedTask` に割り当てられます。 `firstFinishedTask` の型は、<xref:System.Threading.Tasks.Task%601> が整数である `TResult` です。それは `ProcessURLAsync` の戻り値の型であるためです。

    ```vb
    ' ***Call WhenAny and then await the result. The task that finishes
    ' first is assigned to firstFinishedTask.
    Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)
    ```

5. この例では、最初に終了したタスクにのみ焦点を当てています。 したがって、<xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> を使用して、残りのタスクを取り消します。

    ```vb
    ' ***Cancel the rest of the downloads. You just want the first one.
    cts.Cancel()
    ```

6. 最後に、`firstFinishedTask` を待機して、ダウンロードされたコンテンツの長さを取得します。

    ```vb
    Dim length = Await firstFinishedTask
    resultsTextBox.Text &= vbCrLf & $"Length of the downloaded website:  {length}" & vbCrLf
    ```

プログラムを複数回実行し、最初に終了するダウンロードが異なることを確認します。

## <a name="complete-example"></a>コード例全体

次のコードは、この例での MainWindow.xaml.vb または MainWindow.xaml.cs ファイルの全体です。 アスタリスクはこの例のために追加された要素を示しています。

<xref:System.Net.Http> の参照を追加する必要があることに注意してください。

このプロジェクトは「[Async Sample: Fine Tuning Your Application (非同期のサンプル: アプリケーションの微調整)](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」からダウンロードできます。

```vb
' Add an Imports directive and a reference for System.Net.Http.
Imports System.Net.Http

' Add the following Imports directive for System.Threading.
Imports System.Threading

Class MainWindow

    ' Declare a System.Threading.CancellationTokenSource.
    Dim cts As CancellationTokenSource

    Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)

        ' Instantiate the CancellationTokenSource.
        cts = New CancellationTokenSource()

        resultsTextBox.Clear()

        Try
            Await AccessTheWebAsync(cts.Token)
            resultsTextBox.Text &= vbCrLf & "Download complete."

        Catch ex As OperationCanceledException
            resultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

        Catch ex As Exception
            resultsTextBox.Text &= vbCrLf & "Download failed." & vbCrLf
        End Try

        ' Set the CancellationTokenSource to Nothing when the download is complete.
        cts = Nothing
    End Sub

    ' You can still include a Cancel button if you want to.
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)

        If cts IsNot Nothing Then
            cts.Cancel()
        End If
    End Sub

    ' Provide a parameter for the CancellationToken.
    ' Change the return type to Task because the method has no return statement.
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task

        Dim client As HttpClient = New HttpClient()

        ' Call SetUpURLList to make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        '' Comment out or delete the loop.
        ''For Each url In urlList
        ''    ' GetAsync returns a Task(Of HttpResponseMessage).
        ''    ' Argument ct carries the message if the Cancel button is chosen.
        ''    ' Note that the Cancel button can cancel all remaining downloads.
        ''    Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ''    ' Retrieve the website contents from the HttpResponseMessage.
        ''    Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ''    resultsTextBox.Text &=
        ''        vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
        ''Next

        ' ***Create a query that, when executed, returns a collection of tasks.
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
            From url In urlList Select ProcessURLAsync(url, client, ct)

        ' ***Use ToArray to execute the query and start the download tasks.
        Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()

        ' ***Call WhenAny and then await the result. The task that finishes
        ' first is assigned to firstFinishedTask.
        Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)

        ' ***Cancel the rest of the downloads. You just want the first one.
        cts.Cancel()

        ' ***Await the first completed task and display the results
        ' Run the program several times to demonstrate that different
        ' websites can finish first.
        Dim length = Await firstFinishedTask
        resultsTextBox.Text &= vbCrLf & $"Length of the downloaded website:  {length}" & vbCrLf
    End Function

    ' ***Bundle the processing steps for a website into one async method.
    Async Function ProcessURLAsync(url As String, client As HttpClient, ct As CancellationToken) As Task(Of Integer)

        ' GetAsync returns a Task(Of HttpResponseMessage).
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ' Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        Return urlContents.Length
    End Function

    ' Add a method that creates a list of web addresses.
    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
        Return urls
    End Function

End Class

' Sample output:

' Length of the downloaded website:  158856

' Download complete.
```

## <a name="see-also"></a>参照

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [非同期アプリケーションの微調整 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/fine-tuning-your-async-application.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)
- [非同期のサンプル: アプリケーションの微調整](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
