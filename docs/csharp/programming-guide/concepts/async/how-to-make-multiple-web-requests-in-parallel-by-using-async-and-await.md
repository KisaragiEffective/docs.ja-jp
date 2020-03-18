---
title: async と await を使用して複数の Web 要求を並列実行する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 19745899-f97a-4499-a7c7-e813d1447580
ms.openlocfilehash: 9f7420113d4af83d7d057b772af307bd8d4bcc00
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169950"
---
# <a name="how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await-c"></a>async と await を使用して複数の Web 要求を並列実行する方法 (C#)
非同期メソッドでは、タスクは作成されると開始されます。 [await](../../../language-reference/operators/await.md) 演算子は、メソッド内でタスクが終了するまで処理が続行できなくなった時点で、タスクに適用されます。 次の例に示すように、タスクは多くの場合、作成されるとすぐに待機します。  
  
```csharp  
var result = await someWebAccessMethodAsync(url);  
```  
  
 ただし、プログラムにタスクの完了に依存せずに実行する別の処理がある場合、タスクの作成とタスクの待機を分けることもできます。  
  
```csharp  
// The following line creates and starts the task.  
var myTask = someWebAccessMethodAsync(url);  
  
// While the task is running, you can do other work that doesn't depend  
// on the results of the task.  
// . . . . .  
  
// The application of await suspends the rest of this method until the task is complete.  
var result = await myTask;  
```  
  
 タスクを開始して待機する間に、他のタスクを開始できます。 追加のタスクは暗黙的に並列で実行されますが、追加のスレッドは作成されません。  
  
 次のプログラムは 3 つの非同期的な Web ダウンロードを開始し、次に呼び出した順にそれを待機します。 プログラムを実行する場合、タスクは必ずしも作成して待機した順には終了しないことに注意します。 タスクは作成されると実行され、メソッドが await 式に到達する前に 1 つまたは複数のタスクが終了する場合もあります。  
  
> [!NOTE]
> このプロジェクトを完成させるには、Visual Studio 2012 以降および .NET Framework 4.5 以降がコンピューターにインストールされている必要があります。  
  
 複数のタスクを同時に開始する別の例については、「[Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)」を参照してください。
  
 この例のコードは、[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e)のページからダウンロードできます。  
  
### <a name="to-set-up-the-project"></a>プロジェクトを設定するには  
  
1. WPF アプリケーションを設定するには、次の手順を実行します。 これらの手順の詳細については、「[チュートリアル: Async と Await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)」を参照してください。  
  
    - テキスト ボックスとボタンを含む WPF アプリケーションを作成します。 ボタンに `startButton` という名前を付け、テキスト ボックスに `resultsTextBox` という名前を付けます。  
  
    - <xref:System.Net.Http> への参照を追加します。  
  
    - MainWindow.xaml.cs ファイルで、`using` に `System.Net.Http` ディレクティブを追加します。  
  
### <a name="to-add-the-code"></a>コードを追加するには  
  
1. デザイン ウィンドウの MainWindow.xaml で、ボタンをダブルクリックして、MainWindow.xaml.cs に `startButton_Click` イベント ハンドラーを作成します。  
  
2. 次のコードをコピーし、MainWindow.xaml.cs の `startButton_Click` の本体に貼り付けます。  
  
    ```csharp  
    resultsTextBox.Clear();  
    await CreateMultipleTasksAsync();  
    resultsTextBox.Text += "\r\n\r\nControl returned to startButton_Click.\r\n";  
    ```  
  
     このコードは、アプリケーションを呼び出す非同期メソッドである `CreateMultipleTasksAsync` を呼び出します。  
  
3. プロジェクトに次のサポート メソッドを追加します。  
  
    - `ProcessURLAsync` は <xref:System.Net.Http.HttpClient> メソッドを使用して、Web サイトのコンテンツをバイト配列としてダウンロードします。 次に `ProcessURLAsync` サポート メソッドは、配列の長さを表示して返します。  
  
    - `DisplayResults` は各 URL のバイト配列内のバイトの数を表示します。 この表示は、各タスクがいつダウンロードを完了したかを示します。  
  
     次のメソッドをコピーし、MainWindow.xaml.cs の `startButton_Click` イベント ハンドラーの後に貼り付けます。  
  
    ```csharp  
    async Task<int> ProcessURLAsync(string url, HttpClient client)  
    {  
        var byteArray = await client.GetByteArrayAsync(url);  
        DisplayResults(url, byteArray);  
        return byteArray.Length;  
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
  
4. 最後に、次の手順を実行するメソッド `CreateMultipleTasksAsync` を定義します。  
  
    - このメソッドは、`HttpClient` の <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> メソッドにアクセスするために必要な `ProcessURLAsync` オブジェクトを宣言します。  
  
    - このメソッドは <xref:System.Threading.Tasks.Task%601> が整数である `TResult` 型の 3 つのタスクを作成して開始します。 各タスクが終了すると、`DisplayResults` はタスクの URL とダウンロードしたコンテンツの長さを表示します。 タスクは非同期的に実行されるため、結果が表示される順序は、宣言された順序と異なる場合があります。  
  
    - メソッドは、各タスクの完了を待機します。 各 `await` 演算子は、待機したタスクが終了するまで `CreateMultipleTasksAsync` の実行を中断します。 さらに演算子は、完了した各タスクから `ProcessURLAsync` への呼び出しからの戻り値を取得します。  
  
    - タスクが完了して整数値が取得されると、メソッドは Web サイトの長さの合計し、その結果を表示します。  
  
     次のメソッドをコピーしてソリューションに貼り付けます。  
  
    ```csharp  
    private async Task CreateMultipleTasksAsync()  
    {  
        // Declare an HttpClient object, and increase the buffer size. The  
        // default buffer size is 65,536.  
        HttpClient client =  
            new HttpClient() { MaxResponseContentBufferSize = 1000000 };  
  
        // Create and start the tasks. As each task finishes, DisplayResults
        // displays its length.  
        Task<int> download1 =
            ProcessURLAsync("https://msdn.microsoft.com", client);  
        Task<int> download2 =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client);  
        Task<int> download3 =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client);  
  
        // Await each task.  
        int length1 = await download1;  
        int length2 = await download2;  
        int length3 = await download3;  
  
        int total = length1 + length2 + length3;  
  
        // Display the total count for the downloaded websites.  
        resultsTextBox.Text += $"\r\n\r\nTotal bytes returned:  {total}\r\n";
    }  
    ```  
  
5. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。  
  
     プログラムを複数回実行して、3 つのタスクが必ずしも同じ順序では完了しないこと、また完了の順序は必ずしも作成され待機した順序と同じではないことを確認します。  
  
## <a name="example"></a>例  
 ここまでの例をすべて含んだコードを次に示します。  
  
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
  
// Add the following using directive, and add a reference for System.Net.Http.  
using System.Net.Http;  
  
namespace AsyncExample_MultipleTasks  
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
            await CreateMultipleTasksAsync();  
            resultsTextBox.Text += "\r\n\r\nControl returned to startButton_Click.\r\n";  
        }  
  
        private async Task CreateMultipleTasksAsync()  
        {  
            // Declare an HttpClient object, and increase the buffer size. The  
            // default buffer size is 65,536.  
            HttpClient client =  
                new HttpClient() { MaxResponseContentBufferSize = 1000000 };  
  
            // Create and start the tasks. As each task finishes, DisplayResults
            // displays its length.  
            Task<int> download1 =
                ProcessURLAsync("https://msdn.microsoft.com", client);  
            Task<int> download2 =
                ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client);  
            Task<int> download3 =
                ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client);  
  
            // Await each task.  
            int length1 = await download1;  
            int length2 = await download2;  
            int length3 = await download3;  
  
            int total = length1 + length2 + length3;  
  
            // Display the total count for the downloaded websites.  
            resultsTextBox.Text += $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }  
  
        async Task<int> ProcessURLAsync(string url, HttpClient client)  
        {  
            var byteArray = await client.GetByteArrayAsync(url);  
            DisplayResults(url, byteArray);  
            return byteArray.Length;  
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
  
## <a name="see-also"></a>参照

- [チュートリアル: async と await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Async および Await を使用した非同期プログラミング (C#)](./index.md)
- [Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
