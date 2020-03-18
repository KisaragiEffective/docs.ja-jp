---
title: 完了時での複数の同期タスクとプロセスの実行
ms.date: 07/20/2015
ms.assetid: 57ffb748-af40-4794-bedd-bdb7fea062de
ms.openlocfilehash: 5293c2f6e1a17d3645fd1ce5a4ba61eac4d8b3a2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346673"
---
# <a name="start-multiple-async-tasks-and-process-them-as-they-complete-visual-basic"></a>複数の非同期タスクを開始し、完了時に処理する (Visual Basic)
<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> を使用すると、複数のタスクを、開始された順番に処理するのでなく、同時に開始して完了するごとに 1 つずつ処理できます。  
  
 クエリを使用して、タスクのコレクションを作成する例を次に示します。 各タスクは、指定された Web サイトのコンテンツをダウンロードします。 while ループの各反復で、待機されている `WhenAny` への呼び出しは、最初にダウンロードを終了するタスクのコレクションにあるタスクを返します。 タスクはコレクションから削除され、処理されます。 ループは、コレクションのタスクがなくなるまで繰り返されます。  
  
> [!NOTE]
> この例を実行するには、Visual Studio 2012 以降および .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。  
  
## <a name="downloading-the-example"></a>例をダウンロードする  
 完全な Windows Presentation Foundation (WPF) プロジェクトは「[Async Sample: Fine Tuning Your Application (非同期のサンプル: アプリケーションの微調整)](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」からダウンロードできます。ダウンロード後、次の手順に従います。  
  
1. ダウンロードしたファイルを圧縮解除し、Visual Studio を起動します。  
  
2. メニュー バーで **[ファイル]** 、 **[開く]** 、 **[プロジェクト/ソリューション]** の順に選択します。  
  
3. **[プロジェクトを開く]** ダイアログ ボックスで、圧縮解除したサンプル コードを含むフォルダーを開き、AsyncFineTuningVB 用のソリューション (.sln) ファイルを開きます。  
  
4. **ソリューション エクスプローラー**で、**ProcessTasksAsTheyFinish** プロジェクトのショートカット メニューを開き、 **[スタートアップ プロジェクトに設定]** をクリックします。  
  
5. F5 キーを押してプロジェクトを実行します。  
  
     Ctrl + F5 キーを押して、デバッグを行わずにプロジェクトを実行します。  
  
6. ダウンロードの長さが常に同じ順序では表示されないことを確認するために、プロジェクトを複数回実行します。  
  
 プロジェクトをダウンロードしない場合は、このトピックの最後の MainWindow.xaml.vb ファイルをレビューできます。  
  
## <a name="building-the-example"></a>例のビルド  
 この例で[は、「完了後の残りの非同期タスクのキャンセル (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md) 」で開発したコードにを追加し、同じ UI を使用します。  
  
 この例を自分でビルドするには、「例をダウンロードする」のセクションの詳細な手順の指示に従いますが、 **[スタートアップ プロジェクト]** では **CancelAfterOneTask** を選択します。 そのプロジェクトの `AccessTheWebAsync` メソッドに、このトピックでの変更を追加します。 変更部分にはアスタリスクが付いています。  
  
 **CancelAfterOneTask** プロジェクトには、実行時にタスクのコレクションを作成するクエリが含まれています。 次のコードの `ProcessURLAsync` への各呼び出しは、<xref:System.Threading.Tasks.Task%601> が整数である `TResult` を返します。  
  
```vb  
Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
    From url In urlList Select ProcessURLAsync(url, client, ct)  
```  
  
 プロジェクトの Mainwindow.xaml ファイルで、`AccessTheWebAsync` メソッドに次の変更を加えます。  
  
- <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> の代わりに <xref:System.Linq.Enumerable.ToArray%2A> を適用して、クエリを実行します。  
  
    ```vb  
    Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
    ```  
  
- コレクションの各タスクで次の手順を実行する while ループを追加します。  
  
    1. `WhenAny` への呼び出しを待機し、ダウンロードを終了する、コレクションの最初のタスクを識別します。  
  
        ```vb  
        Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
        ```  
  
    2. コレクションからそのタスクを削除します。  
  
        ```vb  
        downloadTasks.Remove(firstFinishedTask)  
        ```  
  
    3. `firstFinishedTask` への呼び出しから返される、`ProcessURLAsync` を待機します。 `firstFinishedTask` 変数は <xref:System.Threading.Tasks.Task%601> が整数である `TReturn` です。 次の例に示すように、タスクは既に完了していますが、ダウンロードした Web サイトの長さの取得を待機します。  
  
        ```vb  
        Dim length = Await firstFinishedTask  
        resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        ```  
  
 ダウンロードの長さが常に同じ順序では表示されないことを確認するために、プロジェクトを複数回実行します。  
  
> [!CAUTION]
> ループで `WhenAny` を使って、例に示すように、いくつかのタスクを格納する問題を解決できます。 ただし、多数のタスクが処理する場合、他のアプローチがより効率的です。 使用例を含む詳細については、「[Processing Tasks as they complete (完了したタスクを処理する)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)」を参照してください。  
  
## <a name="complete-example"></a>コード例全体  
 次のコードは、この例の MainWindow.xaml.vb ファイルのテキスト全体です。 アスタリスクはこの例のために追加された要素を示しています。  
  
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
            resultsTextBox.Text &= vbCrLf & "Downloads complete."  
  
        Catch ex As OperationCanceledException  
            resultsTextBox.Text &= vbCrLf & "Downloads canceled." & vbCrLf  
  
        Catch ex As Exception  
            resultsTextBox.Text &= vbCrLf & "Downloads failed." & vbCrLf  
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
  
        ' ***Create a query that, when executed, returns a collection of tasks.  
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
            From url In urlList Select ProcessURLAsync(url, client, ct)  
  
        ' ***Use ToList to execute the query and start the download tasks.   
        Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
  
        ' ***Add a loop to process the tasks one at a time until none remain.  
        While downloadTasks.Count > 0  
            ' ***Identify the first task that completes.  
            Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
  
            ' ***Remove the selected task from the list so that you don't  
            ' process it more than once.  
            downloadTasks.Remove(firstFinishedTask)  
  
            ' ***Await the first completed task and display the results.  
            Dim length = Await firstFinishedTask  
            resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        End While  
  
    End Function  
  
    ' Bundle the processing steps for a website into one async method.  
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
  
' Length of the download:  226093  
' Length of the download:  412588  
' Length of the download:  175490  
' Length of the download:  204890  
' Length of the download:  158855  
' Length of the download:  145790  
' Length of the download:  44908  
' Downloads complete.  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [非同期アプリケーションの微調整 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/fine-tuning-your-async-application.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)
- [非同期のサンプル: アプリケーションの微調整](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
