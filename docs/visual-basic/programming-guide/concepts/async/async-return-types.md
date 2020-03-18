---
title: 非同期の戻り値の型
ms.date: 07/20/2015
ms.assetid: 07890291-ee72-42d3-932a-fa4d312f2c60
ms.openlocfilehash: 96d3a945a49a12f7c2d5d60e8ee59ce047a0bae6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347977"
---
# <a name="async-return-types-visual-basic"></a>非同期の戻り値の型 (Visual Basic)

非同期メソッドには、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.Task>、および void の 3 とおりの戻り値の型があります。 Visual Basic では、void の戻り値の型は [Sub](../../../../visual-basic/programming-guide/language-features/procedures/sub-procedures.md) プロシージャとして作成されます。 非同期メソッドの詳細については、「 [async および Await を使用した非同期プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)」を参照してください。

それぞれの戻り値の型は、次のセクションの 1 つで確認でき、トピックの最後で 3 種類のすべてを使用する例を参照できます。

> [!NOTE]
> この例を実行するには、Visual Studio 2012 以降と .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。

## <a name="BKMK_TaskTReturnType"></a>Task(T) 型

<xref:System.Threading.Tasks.Task%601> 戻り値の型は、オペランドの型が `TResult`である[return](../../../../visual-basic/language-reference/statements/return-statement.md)ステートメントを含む非同期メソッドに使用されます。

次の例では、`TaskOfT_MethodAsync` 非同期メソッドには整数を返す return ステートメントが含まれます。 そのため、メソッド宣言では、戻り値の型を `Task(Of Integer)` と指定する必要があります。

```vb
' TASK(OF T) EXAMPLE
Async Function TaskOfT_MethodAsync() As Task(Of Integer)

    ' The body of an async method is expected to contain an awaited
    ' asynchronous call.
    ' Task.FromResult is a placeholder for actual work that returns a string.
    Dim today As String = Await Task.FromResult(Of String)(DateTime.Now.DayOfWeek.ToString())

    ' The method then can process the result in some way.
    Dim leisureHours As Integer
    If today.First() = "S" Then
        leisureHours = 16
    Else
        leisureHours = 5
    End If

    ' Because the return statement specifies an operand of type Integer, the
    ' method must have a return type of Task(Of Integer).
    Return leisureHours
End Function
```

`TaskOfT_MethodAsync` が await 式の中から呼び出されると、await 式は `leisureHours` から返されるタスクに格納されている整数値 (`TaskOfT_MethodAsync` の値) を取得します。 Await 式の詳細については、「 [Await 演算子](../../../../visual-basic/language-reference/operators/await-operator.md)」を参照してください。

次のコードは、`TaskOfT_MethodAsync` メソッドを呼び出して待機します。 結果は `result1` 変数に割り当てられます。

```vb
' Call and await the Task(Of T)-returning async method in the same statement.
Dim result1 As Integer = Await TaskOfT_MethodAsync()
```

次のコードに示すように、`TaskOfT_MethodAsync` の呼び出しと、`Await` の適用を分離すると、この仕組みをよく理解できます。 メソッドの宣言から予想されるように、直ちに待機しない `TaskOfT_MethodAsync` メソッドの呼び出しは、`Task(Of Integer)` を返します。 タスクは、この例の `integerTask` 変数に割り当てられます。 `integerTask` は <xref:System.Threading.Tasks.Task%601> であるため、<xref:System.Threading.Tasks.Task%601.Result> 型の `TResult` プロパティが含まれています。 この場合、TResult が整数型を表します。 `Await` が `integerTask` に適用されると、<xref:System.Threading.Tasks.Task%601.Result%2A> の `integerTask` プロパティの内容が await 式の評価となります。 この値は `result2` 変数に割り当てられます。

> [!WARNING]
> <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティは Blocking プロパティです。 タスクが終了する前にアクセスしようとすると、現在アクティブなスレッドは、タスクが完了して値が使用可能になるまで、ブロックされます。 多くの場合、プロパティに直接アクセスする代わりに、`Await` を使用して値にアクセスする必要があります。

```vb
' Call and await in separate statements.
Dim integerTask As Task(Of Integer) = TaskOfT_MethodAsync()

' You can do other work that does not rely on resultTask before awaiting.
textBox1.Text &= "Application can continue working while the Task(Of T) runs. . . . " & vbCrLf

Dim result2 As Integer = Await integerTask
```

次のコードの表示ステートメントは、`result1` 変数、`result2` 変数、および `Result` プロパティの値が同じであることを確認します。 `Result` プロパティは Blocking プロパティであり、タスクが待機される前にアクセスしないように注意してください。

```vb
' Display the values of the result1 variable, the result2 variable, and
' the resultTask.Result property.
textBox1.Text &= vbCrLf & $"Value of result1 variable:   {result1}" & vbCrLf
textBox1.Text &= $"Value of result2 variable:   {result2}" & vbCrLf
textBox1.Text &= $"Value of resultTask.Result:  {integerTask.Result}" & vbCrLf
```

## <a name="BKMK_TaskReturnType"></a>Task 型

return ステートメントを含まない非同期メソッド、またはオペランドを返さない return ステートメントを含む非同期メソッドは、通常は <xref:System.Threading.Tasks.Task> 戻り値の型を指定します。 このようなメソッドは、同期的に実行するように記述されている場合、[サブ](../../../../visual-basic/programming-guide/language-features/procedures/sub-procedures.md)プロシージャになります。 非同期メソッドに戻り値の型 `Task` を使用した場合、呼び出し元のメソッドは `Await` 演算子を使って、呼び出された async のメソッドが終了するまで、呼び出し元の完了を中断します。

次の例では、非同期メソッド `Task_MethodAsync` には、return ステートメントが含まれていません。 したがって、`Task` を待機させるメソッドに、戻り値の型 `Task_MethodAsync` を指定します。 `Task` 型の定義は、戻り値を格納する `Result` プロパティを含みません。

```vb
' TASK EXAMPLE
Async Function Task_MethodAsync() As Task

    ' The body of an async method is expected to contain an awaited
    ' asynchronous call.
    ' Task.Delay is a placeholder for actual work.
    Await Task.Delay(2000)
    textBox1.Text &= vbCrLf & "Sorry for the delay. . . ." & vbCrLf

    ' This method has no return statement, so its return type is Task.
End Function
```

`Task_MethodAsync` は、同期 `Sub` または void を返すメソッドの呼び出しステートメントと同様に、await 式の代わりに await ステートメントを使用して呼び出され、待機されます。 この場合、`Await` 演算子を適用しても値は生成されません。

次のコードは、`Task_MethodAsync` メソッドを呼び出して待機します。

```vb
' Call and await the Task-returning async method in the same statement.
Await Task_MethodAsync()
```

前の <xref:System.Threading.Tasks.Task%601> 例と同様に、次のコードに示すように、`Task_MethodAsync` への呼び出しを `Await` 演算子のアプリケーションから分離することができます。 ただし `Task` は `Result` プロパティを持たないこと、また await 演算子が `Task` に適用されるときに値は生成されないことに注意します。

次のコードは `Task_MethodAsync` の呼び出しを `Task_MethodAsync` が返すタスクの待機から分離します。

```vb
' Call and await in separate statements.
Dim simpleTask As Task = Task_MethodAsync()

' You can do other work that does not rely on simpleTask before awaiting.
textBox1.Text &= vbCrLf & "Application can continue working while the Task runs. . . ." & vbCrLf

Await simpleTask
```

## <a name="BKMK_VoidReturnType"></a> Void 型

`Sub` プロシージャの主な用途は、(他の言語では void の戻り値の型と呼ばれる) 戻り値の型がないイベントハンドラーです。 void である戻り値は、void を返すメソッドをオーバーライドするためにも使われます。または「ファイア アンド フォーゲット (撃ち放し)」と分類されるアクティビティを実行するメソッドに対して使われます。 ただし、void を返す非同期メソッドを待機することはできないため、できる限り `Task` を返す必要があります。 このようなメソッドの呼び出し元は、呼び出した非同期メソッドが完了するのを待たずに、完了まで継続できる必要があります。また呼び出し元は、非同期メソッドが生成する値または例外とは無関係である必要があります。

void を返す非同期メソッドの呼び出し元は、メソッドがスローする例外をキャッチすることはできません。そのようなハンドルされない例外によって、アプリケーションが失敗する可能性が高くなります。 <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> を返す非同期メソッドで例外が発生すると、例外は返されたタスクに格納され、タスクが待機するときに再スローされます。 したがって、例外を生成する場合がある非同期メソッドは <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> の戻り値の型を持つこと、またメソッドの呼び出しが待機することを確認します。

非同期のメソッドで例外をキャッチする方法の詳細については、「[Try...Catch...Finally Statement (Try...Catch...Finally ステートメント)](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)」を参照してください。

次のコードは非同期のイベント ハンドラーを定義します。

```vb
' SUB EXAMPLE
Async Sub button1_Click(sender As Object, e As RoutedEventArgs) Handles button1.Click

    textBox1.Clear()

    ' Start the process and await its completion. DriverAsync is a
    ' Task-returning async method.
    Await DriverAsync()

    ' Say goodbye.
    textBox1.Text &= vbCrLf & "All done, exiting button-click event handler."
End Sub
```

## <a name="BKMK_Example"></a>コード例全体

次の Windows Presentation Foundation (WPF) プロジェクトには、このトピックのコード例が含まれています。

 このプロジェクトを実行するには、次の手順を実行します。

1. Visual Studio を起動します。

2. メニュー バーで、 **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。

     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストール済み]** の **[テンプレート]** カテゴリで、 **[Visual Basic]** を選択し、 **[Windows]** を選択します。 プロジェクトの種類の一覧から **[WPF アプリケーション]** をクリックします。

4. プロジェクトの名前として「`AsyncReturnTypes`」と入力し、 **[OK]** をクリックします。

     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

5. Visual Studio コード エディターで、 **[MainWindow.xaml]** タブをクリックします。

     タブが表示されない場合は、**ソリューション エクスプローラー**で MainWindow.xaml のショートカット メニューを開き、 **[開く]** をクリックします。

6. MainWindow.xaml の **[XAML]** ウィンドウで、コードを次のコードに置き換えます。

    ```vb
    <Window x:Class="MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            Title="MainWindow" Height="350" Width="525">
        <Grid>
            <Button x:Name="button1" Content="Start" HorizontalAlignment="Left" Margin="214,28,0,0" VerticalAlignment="Top" Width="75" HorizontalContentAlignment="Center" FontWeight="Bold" FontFamily="Aharoni" Click="button1_Click"/>
            <TextBox x:Name="textBox1" Margin="0,80,0,0" TextWrapping="Wrap" FontFamily="Lucida Console"/>

        </Grid>
    </Window>
    ```

     テキスト ボックスとボタンを含むシンプルなウィンドウが、MainWindow.xaml の **[デザイン]** ウィンドウに表示されます。

7. **ソリューションエクスプローラー**で、mainwindow.xaml のショートカットメニューを開き、 **[コードの表示]** を選択します。

8. MainWindow.xaml.vb のコードを次のコードに置き換えます。

    ```vb
    Class MainWindow

        ' SUB EXAMPLE
        Async Sub button1_Click(sender As Object, e As RoutedEventArgs) Handles button1.Click

            textBox1.Clear()

            ' Start the process and await its completion. DriverAsync is a
            ' Task-returning async method.
            Await DriverAsync()

            ' Say goodbye.
            textBox1.Text &= vbCrLf & "All done, exiting button-click event handler."
        End Sub

        Async Function DriverAsync() As Task

            ' Task(Of T)
            ' Call and await the Task(Of T)-returning async method in the same statement.
            Dim result1 As Integer = Await TaskOfT_MethodAsync()

            ' Call and await in separate statements.
            Dim integerTask As Task(Of Integer) = TaskOfT_MethodAsync()

            ' You can do other work that does not rely on resultTask before awaiting.
            textBox1.Text &= "Application can continue working while the Task(Of T) runs. . . . " & vbCrLf

            Dim result2 As Integer = Await integerTask

            ' Display the values of the result1 variable, the result2 variable, and
            ' the resultTask.Result property.
            textBox1.Text &= vbCrLf & $"Value of result1 variable:   {result1}" & vbCrLf
            textBox1.Text &= $"Value of result2 variable:   {result2}" & vbCrLf
            textBox1.Text &= $"Value of resultTask.Result:  {integerTask.Result}" & vbCrLf

            ' Task
            ' Call and await the Task-returning async method in the same statement.
            Await Task_MethodAsync()

            ' Call and await in separate statements.
            Dim simpleTask As Task = Task_MethodAsync()

            ' You can do other work that does not rely on simpleTask before awaiting.
            textBox1.Text &= vbCrLf & "Application can continue working while the Task runs. . . ." & vbCrLf

            Await simpleTask
        End Function

        ' TASK(OF T) EXAMPLE
        Async Function TaskOfT_MethodAsync() As Task(Of Integer)

            ' The body of an async method is expected to contain an awaited
            ' asynchronous call.
            ' Task.FromResult is a placeholder for actual work that returns a string.
            Dim today As String = Await Task.FromResult(Of String)(DateTime.Now.DayOfWeek.ToString())

            ' The method then can process the result in some way.
            Dim leisureHours As Integer
            If today.First() = "S" Then
                leisureHours = 16
            Else
                leisureHours = 5
            End If

            ' Because the return statement specifies an operand of type Integer, the
            ' method must have a return type of Task(Of Integer).
            Return leisureHours
        End Function

        ' TASK EXAMPLE
        Async Function Task_MethodAsync() As Task

            ' The body of an async method is expected to contain an awaited
            ' asynchronous call.
            ' Task.Delay is a placeholder for actual work.
            Await Task.Delay(2000)
            textBox1.Text &= vbCrLf & "Sorry for the delay. . . ." & vbCrLf

            ' This method has no return statement, so its return type is Task.
        End Function

    End Class
    ```

9. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。

     次の出力が表示されます。

    ```console
    Application can continue working while the Task<T> runs. . . .

    Value of result1 variable:   5
    Value of result2 variable:   5
    Value of integerTask.Result: 5

    Sorry for the delay. . . .

    Application can continue working while the Task runs. . . .

    Sorry for the delay. . . .

    All done, exiting button-click event handler.
    ```

## <a name="see-also"></a>関連項目

- <xref:System.Threading.Tasks.Task.FromResult%2A>
- [チュートリアル: Async と Await を使用した Web へのアクセス (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
- [非同期プログラムにおける制御フロー (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/control-flow-in-async-programs.md)
- [Async](../../../../visual-basic/language-reference/modifiers/async.md)
- [Await 演算子](../../../../visual-basic/language-reference/operators/await-operator.md)
