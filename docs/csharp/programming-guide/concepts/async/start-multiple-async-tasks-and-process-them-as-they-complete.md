---
title: 完了時の非同期タスクの処理
ms.date: 09/12/2018
ms.assetid: 25331850-35a7-43b3-ab76-3908e4346b9d
ms.openlocfilehash: b618fd6bf80551231d2b285fd0e8aef688d00d93
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "71736732"
---
# <a name="start-multiple-async-tasks-and-process-them-as-they-complete-c"></a>完了時での複数の非同期タスクとプロセスの実行 (C#)

<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> を使用すると、複数のタスクを、開始された順番に処理するのでなく、同時に開始して完了するごとに 1 つずつ処理できます。

クエリを使用して、タスクのコレクションを作成する例を次に示します。 各タスクは、指定された Web サイトのコンテンツをダウンロードします。 while ループの各反復で、待機されている `WhenAny` への呼び出しは、最初にダウンロードを終了するタスクのコレクションにあるタスクを返します。 タスクはコレクションから削除され、処理されます。 ループは、コレクションのタスクがなくなるまで繰り返されます。

> [!NOTE]
> この例を実行するには、Visual Studio (2012 以降) および .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。

## <a name="download-an-example-solution"></a>ソリューションの例をダウンロードする

完全な Windows Presentation Foundation (WPF) プロジェクトは、「[Async Sample: Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」(非同期のサンプル: アプリケーションの微調整) からダウンロードできます。その後、次の手順に従います。

> [!TIP]
> プロジェクトをダウンロードしない場合は、代わりに、このトピックの最後の *MainWindow.xaml.cs* ファイルをレビューしてもかまいません。

1. *.zip* ファイルからダウンロードしたファイルを抽出して、Visual Studio を開始します。

2. メニュー バーで、 **[ファイル]**  >  **[開く]**  >  **[プロジェクト/ソリューション]** を選択します。

3. **[プロジェクトを開く]** ダイアログ ボックスで、ダウンロードしたサンプル コードを含むフォルダーを開き、*AsyncFineTuningCS*/*AsyncFineTuningVB* のソリューション ( *.sln*) ファイルを開きます。

4. **ソリューション エクスプローラー**で、**ProcessTasksAsTheyFinish** プロジェクトのショートカット メニューを開き、 **[スタートアップ プロジェクトに設定]** をクリックします。

5. <kbd>F5</kbd> キーを押してデバッグありでプログラムを実行します。または、<kbd>Ctrl</kbd>+<kbd>F5</kbd> キーを押して、デバッグせずにプログラムを実行します。

6. ダウンロードの長さが常に同じ順序では表示されないことを確認するために、プロジェクトを複数回実行します。

## <a name="create-the-program-yourself"></a>プログラムを自分で作成する

この例では、「[完了後の残りの非同期タスクのキャンセル (C#)](cancel-remaining-async-tasks-after-one-is-complete.md)」で開発したコードを追加し、同じ UI を使用します。

この例を自分でビルドするには、「例をダウンロードする」のセクションの詳細な手順の指示に従いますが、 **[スタートアップ プロジェクト]** では [CancelAfterOneTask](cancel-remaining-async-tasks-after-one-is-complete.md#downloading-the-example) を設定します。 そのプロジェクトの `AccessTheWebAsync` メソッドに、このトピックでの変更を追加します。 変更部分にはアスタリスクが付いています。

**CancelAfterOneTask** プロジェクトには、実行時にタスクのコレクションを作成するクエリが含まれています。 次のコードの `ProcessURLAsync` への各呼び出しは、`TResult` が整数である <xref:System.Threading.Tasks.Task%601> を返します。

```csharp
IEnumerable<Task<int>> downloadTasksQuery = from url in urlList select ProcessURL(url, client, ct);
```

プロジェクトの *MainWindow.xaml.cs* ファイルで、`AccessTheWebAsync` メソッドに次の変更を行います。

- <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> の代わりに <xref:System.Linq.Enumerable.ToArray%2A> を適用して、クエリを実行します。

    ```csharp
    List<Task<int>> downloadTasks = downloadTasksQuery.ToList();
    ```

- コレクションの各タスクで次の手順を実行する `while` ループを追加します。

    1. `WhenAny` への呼び出しを待機し、ダウンロードを終了する、コレクションの最初のタスクを識別します。

        ```csharp
        Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);
        ```

    2. コレクションからそのタスクを削除します。

        ```csharp
        downloadTasks.Remove(firstFinishedTask);
        ```

    3. `firstFinishedTask` への呼び出しから返される、`ProcessURLAsync` を待機します。 `firstFinishedTask` 変数は <xref:System.Threading.Tasks.Task%601> が整数である `TReturn` です。 次の例に示すように、タスクは既に完了していますが、ダウンロードした Web サイトの長さの取得を待機します。 タスクが失敗した場合、`AggregateException` がスローされる `Result` プロパティの読み取りとは異なり、`await` からは `AggregateException` に格納されている最初の子の例外がスローされます。

        ```csharp
        int length = await firstFinishedTask;
        resultsTextBox.Text += $"\r\nLength of the download:  {length}";
        ```

ダウンロードされた長さが常に同じ順序では表示されないことを確認するために、プログラムを複数回実行します。

> [!CAUTION]
> ループで `WhenAny` を使って、例に示すように、いくつかのタスクを格納する問題を解決できます。 ただし、多数のタスクが処理する場合、他のアプローチがより効率的です。 詳細と例については、「[Processing Tasks as they complete](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)」 (完了したタスクを処理する) を参照してください。

## <a name="complete-example"></a>コード例全体

次のコードは、この例での *MainWindow.xaml.cs* ファイルのテキスト全体です。 アスタリスクはこの例のために追加された要素を示しています。 また、<xref:System.Net.Http> の参照を追加する必要があることに注意してください。

プロジェクトは、「[Async Sample: Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」(非同期のサンプル: アプリケーションの微調整) からダウンロードできます。

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

// Add a using directive and a reference for System.Net.Http.
using System.Net.Http;

// Add the following using directive.
using System.Threading;

namespace ProcessTasksAsTheyFinish
{
    public partial class MainWindow : Window
    {
        // Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;

        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // Instantiate the CancellationTokenSource.
            cts = new CancellationTokenSource();

            try
            {
                await AccessTheWebAsync(cts.Token);
                resultsTextBox.Text += "\r\nDownloads complete.";
            }
            catch (OperationCanceledException)
            {
                resultsTextBox.Text += "\r\nDownloads canceled.\r\n";
            }
            catch (Exception)
            {
                resultsTextBox.Text += "\r\nDownloads failed.\r\n";
            }

            cts = null;
        }

        private void cancelButton_Click(object sender, RoutedEventArgs e)
        {
            if (cts != null)
            {
                cts.Cancel();
            }
        }

        async Task AccessTheWebAsync(CancellationToken ct)
        {
            HttpClient client = new HttpClient();

            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // ***Create a query that, when executed, returns a collection of tasks.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURL(url, client, ct);

            // ***Use ToList to execute the query and start the tasks.
            List<Task<int>> downloadTasks = downloadTasksQuery.ToList();

            // ***Add a loop to process the tasks one at a time until none remain.
            while (downloadTasks.Count > 0)
            {
                    // Identify the first task that completes.
                    Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);

                    // ***Remove the selected task from the list so that you don't
                    // process it more than once.
                    downloadTasks.Remove(firstFinishedTask);

                    // Await the completed task.
                    int length = await firstFinishedTask;
                    resultsTextBox.Text += $"\r\nLength of the download:  {length}";
            }
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        async Task<int> ProcessURL(string url, HttpClient client, CancellationToken ct)
        {
            // GetAsync returns a Task<HttpResponseMessage>.
            HttpResponseMessage response = await client.GetAsync(url, ct);

            // Retrieve the website contents from the HttpResponseMessage.
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

            return urlContents.Length;
        }
    }
}

// Sample Output:

// Length of the download:  226093
// Length of the download:  412588
// Length of the download:  175490
// Length of the download:  204890
// Length of the download:  158855
// Length of the download:  145790
// Length of the download:  44908
// Downloads complete.
```

## <a name="see-also"></a>関連項目

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [非同期アプリケーションの微調整 (C#)](fine-tuning-your-async-application.md)
- [Async および Await を使用した非同期プログラミング (C#)](index.md)
- [Async Sample:Fine Tuning Your Application (非同期のサンプル: アプリケーションの微調整)](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
