---
title: '方法: Async と Await を使用して複数の Web 要求を並列実行する'
ms.date: 07/20/2015
ms.assetid: a894b99b-7cfd-4a38-adfb-20d24f986730
ms.openlocfilehash: 616efca79312883f17ba837d17a5ee9c97d15b34
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346149"
---
# <a name="how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await-visual-basic"></a>Async と Await を使用して複数の Web 要求を並列実行する方法 (Visual Basic)

非同期メソッドでは、タスクは作成されると開始されます。 [Await](../../../../visual-basic/language-reference/operators/await-operator.md)演算子は、タスクが終了するまで処理を続行できないメソッドのポイントでタスクに適用されます。 次の例に示すように、タスクは多くの場合、作成されるとすぐに待機します。

```vb
Dim result = Await someWebAccessMethodAsync(url)
```

ただし、プログラムにタスクの完了に依存せずに実行する別の処理がある場合、タスクの作成とタスクの待機を分けることもできます。

```vb
' The following line creates and starts the task.
Dim myTask = someWebAccessMethodAsync(url)

' While the task is running, you can do other work that does not depend
' on the results of the task.
' . . . . .

' The application of Await suspends the rest of this method until the task is
' complete.
Dim result = Await myTask
```

タスクを開始して待機する間に、他のタスクを開始できます。 追加のタスクは暗黙的に並列で実行されますが、追加のスレッドは作成されません。

次のプログラムは 3 つの非同期的な Web ダウンロードを開始し、次に呼び出した順にそれを待機します。 プログラムを実行する場合、タスクは必ずしも作成して待機した順には終了しないことに注意します。 タスクは作成されると実行され、メソッドが await 式に到達する前に 1 つまたは複数のタスクが終了する場合もあります。

> [!NOTE]
> このプロジェクトを完成させるには、Visual Studio 2012 以降および .NET Framework 4.5 以降がコンピューターにインストールされている必要があります。

複数のタスクを同時に開始する別の例については、「[方法: task.whenall (Visual Basic) を使用して非同期のチュートリアルを拡張](../../../../visual-basic/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)する」を参照してください。

この例のコードは、[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e)のページからダウンロードできます。

### <a name="to-set-up-the-project"></a>プロジェクトを設定するには

1. WPF アプリケーションを設定するには、次の手順を実行します。 これらの手順の詳細については[、「チュートリアル: Async と Await を使用した Web へのアクセス (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)」を参照してください。

    - テキスト ボックスとボタンを含む WPF アプリケーションを作成します。 ボタンに `startButton` という名前を付け、テキスト ボックスに `resultsTextBox` という名前を付けます。

    - <xref:System.Net.Http> への参照を追加します。

    - Mainwindow.xaml ファイルで、`System.Net.Http`の `Imports` ステートメントを追加します。

### <a name="to-add-the-code"></a>コードを追加するには

1. デザインウィンドウの Mainwindow.xaml で、ボタンをダブルクリックして、Mainwindow.xaml に `startButton_Click` イベントハンドラーを作成します。

2. 次のコードをコピーし、Mainwindow.xaml の `startButton_Click` の本文に貼り付けます。

    ```vb
    resultsTextBox.Clear()
    Await CreateMultipleTasksAsync()
    resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    ```

     このコードは、アプリケーションを呼び出す非同期メソッドである `CreateMultipleTasksAsync` を呼び出します。

3. プロジェクトに次のサポート メソッドを追加します。

    - `ProcessURLAsync` は <xref:System.Net.Http.HttpClient> メソッドを使用して、Web サイトのコンテンツをバイト配列としてダウンロードします。 次に `ProcessURLAsync` サポート メソッドは、配列の長さを表示して返します。

    - `DisplayResults` は各 URL のバイト配列内のバイトの数を表示します。 この表示は、各タスクがいつダウンロードを完了したかを示します。

     次のメソッドをコピーし、Mainwindow.xaml の `startButton_Click` イベントハンドラーの後に貼り付けます。

    ```vb
    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
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

4. 最後に、次の手順を実行するメソッド `CreateMultipleTasksAsync` を定義します。

    - このメソッドは、`HttpClient` の <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> メソッドにアクセスするために必要な `ProcessURLAsync` オブジェクトを宣言します。

    - このメソッドは <xref:System.Threading.Tasks.Task%601> が整数である `TResult` 型の 3 つのタスクを作成して開始します。 各タスクが終了すると、`DisplayResults` はタスクの URL とダウンロードしたコンテンツの長さを表示します。 タスクは非同期的に実行されるため、結果が表示される順序は、宣言された順序と異なる場合があります。

    - メソッドは、各タスクの完了を待機します。 各 `Await` 演算子は、待機したタスクが終了するまで `CreateMultipleTasksAsync` の実行を中断します。 さらに演算子は、完了した各タスクから `ProcessURLAsync` への呼び出しからの戻り値を取得します。

    - タスクが完了して整数値が取得されると、メソッドは Web サイトの長さの合計し、その結果を表示します。

     次のメソッドをコピーしてソリューションに貼り付けます。

    ```vb
    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function
    ```

5. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。

     プログラムを複数回実行して、3 つのタスクが必ずしも同じ順序では完了しないこと、また完了の順序は必ずしも作成され待機した順序と同じではないことを確認します。

## <a name="example"></a>例

ここまでの例をすべて含んだコードを次に示します。

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click
        resultsTextBox.Clear()
        Await CreateMultipleTasksAsync()
        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    End Sub

    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
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

- [チュートリアル: Async と Await を使用した Web へのアクセス (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)
- [方法: Task.WhenAll を使用して AsyncWalkthrough を拡張する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
