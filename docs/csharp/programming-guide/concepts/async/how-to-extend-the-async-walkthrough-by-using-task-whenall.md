---
title: Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)
ms.date: 07/20/2015
ms.assetid: f6927ef2-dc6c-43f8-bc82-bbeac42de423
ms.openlocfilehash: afd7dda4e876b7faa54ae4a8e62d640d2b9aaf07
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73970034"
---
# <a name="how-to-extend-the-async-walkthrough-by-using-taskwhenall-c"></a>Task.WhenAll を使用して AsyncWalkthrough を拡張する方法 (C#)

[ メソッドを使用すると、「](./walkthrough-accessing-the-web-by-using-async-and-await.md)チュートリアル: Async と Await を使用した Web へのアクセス (C#)<xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>」の非同期ソリューションのパフォーマンスを向上できます。 このメソッドは、タスクのコレクションとして表される、複数の非同期操作を非同期に待機します。

このチュートリアルでは、Web サイトが異なる速度でダウンロードされることに気付きます。 Web サイトの 1 つが非常に低速な場合があります。これは残りのすべてのダウンロードを遅延させます。 このチュートリアルで構築した非同期ソリューションを実行すると、プログラムを待たない場合には簡単に終了することができますが、よりよい方法は、すべてのダウンロードを同時に開始して、遅延したダウンロードを待たずに早いダウンロードが継続できるようにすることです。

タスクのコレクションに `Task.WhenAll` メソッドを適用します。 `WhenAll` を適用すると、コレクション内のすべてのタスクが完了するまで完了しない 1 つのタスクを返します。 タスクは並列で実行されるように見えますが、追加のスレッドは作成されません。 タスクは任意の順序で完了します。

> [!IMPORTANT]
> 次の手順では「[チュートリアル: Async と Await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)」で開発した非同期アプリケーションの拡張機能について説明します。 チュートリアルを完了するか、[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)からコードをダウンロードして、アプリケーションを開発できます。
>
> 例を実行するには、Visual Studio 2012 以降がコンピューターにインストールされている必要があります。

### <a name="to-add-taskwhenall-to-your-geturlcontentsasync-solution"></a>GetURLContentsAsync ソリューションに Task.WhenAll を追加するには

1. 「`ProcessURLAsync`チュートリアル: Async と Await を使用した Web へのアクセス (C#)[」で開発した最初のアプリケーションに ](./walkthrough-accessing-the-web-by-using-async-and-await.md) メソッドを追加します。

    - コードを[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)からダウンロードした場合は、AsyncWalkthrough プロジェクトを開き、`ProcessURLAsync` を MainWindow.xaml.cs ファイルに追加します。

    - チュートリアルを実行してコードを開発する場合は、`ProcessURLAsync` メソッドを含むアプリケーションに `GetURLContentsAsync` を追加します。 このアプリケーションの MainWindow.xaml.cs ファイルは、「チュートリアルの完全なコード例」のセクションにある 1 つ目の例です。

    `ProcessURLAsync` メソッドは、元のチュートリアルの `foreach` の `SumPageSizesAsync` ループの本体にあるアクションを統合します。 このメソッドは、指定した Web サイトのコンテンツをバイト配列として非同期的にダウンロードし、バイト配列の長さを表示して返します。

    ```csharp
    private async Task<int> ProcessURLAsync(string url)
    {
        var byteArray = await GetURLContentsAsync(url);
        DisplayResults(url, byteArray);
        return byteArray.Length;
    }
    ```

2. 次のコードに示すように、`foreach` の `SumPageSizesAsync` ループをコメント アウトするか削除します。

    ```csharp
    //var total = 0;
    //foreach (var url in urlList)
    //{
    //    byte[] urlContents = await GetURLContentsAsync(url);

    //    // The previous line abbreviates the following two assignment statements.
    //    // GetURLContentsAsync returns a Task<T>. At completion, the task
    //    // produces a byte array.
    //    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
    //    //byte[] urlContents = await getContentsTask;

    //    DisplayResults(url, urlContents);

    //    // Update the total.
    //    total += urlContents.Length;
    //}
    ```

3. タスクのコレクションを作成します。 次のコードは、[ メソッドによって実行されるときに、各 Web サイトのコンテンツをダウンロードするタスクのコレクションを作成する](../linq/index.md)クエリ<xref:System.Linq.Enumerable.ToArray%2A>を定義します。 タスクは、クエリが評価されるときに開始されます。

    `SumPageSizesAsync` の宣言の後の `urlList` メソッドに、次のコードを追加します。

    ```csharp
    // Create a query.
    IEnumerable<Task<int>> downloadTasksQuery =
        from url in urlList select ProcessURLAsync(url);

    // Use ToArray to execute the query and start the download tasks.
    Task<int>[] downloadTasks = downloadTasksQuery.ToArray();
    ```

4. `Task.WhenAll` をタスクのコレクション `downloadTasks` に適用します。 `Task.WhenAll` は、タスクのコレクションのすべてのタスクが完了すると完了する、単一のタスクを返します。

    次の例では、`await` 式は、`WhenAll` によって返される単一のタスクが完了するのを待機します。 式は、各整数がダウンロードされたサイトの長さである、整数の配列に評価します。 `SumPageSizesAsync` の、前の手順で追加したコードの直後に、次のコードを追加します。

    ```csharp
    // Await the completion of all the running tasks.
    int[] lengths = await Task.WhenAll(downloadTasks);

    //// The previous line is equivalent to the following two statements.
    //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
    //int[] lengths = await whenAllTask;
    ```

5. 最後に <xref:System.Linq.Enumerable.Sum%2A> メソッドを使って、すべての Web サイトの長さの合計を計算します。 `SumPageSizesAsync` に次の行を追加します。

    ```csharp
    int total = lengths.Sum();
    ```

### <a name="to-add-taskwhenall-to-the-httpclientgetbytearrayasync-solution"></a>HttpClient.GetByteArrayAsync ソリューションに Task.WhenAll を追加するには

1. 「`ProcessURLAsync`チュートリアル: Async と Await を使用した Web へのアクセス (C#)[」で開発した 2 つ目のアプリケーションに ](./walkthrough-accessing-the-web-by-using-async-and-await.md) の下記のバージョンを追加します。

    - コードを[開発者コード サンプル](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)からダウンロードした場合は、AsyncWalkthrough_HttpClient プロジェクトを開き、`ProcessURLAsync` を MainWindow.xaml.cs ファイルに追加します。

    - チュートリアルを実行してコードを開発する場合は、`ProcessURLAsync` メソッドを使うアプリケーションに `HttpClient.GetByteArrayAsync` を追加します。 このアプリケーションの MainWindow.xaml.cs ファイルは、「チュートリアルの完全なコード例」のセクションにある 2 つ目の例です。

    `ProcessURLAsync` メソッドは、元のチュートリアルの `foreach` の `SumPageSizesAsync` ループの本体にあるアクションを統合します。 このメソッドは、指定した Web サイトのコンテンツをバイト配列として非同期的にダウンロードし、バイト配列の長さを表示して返します。

    前の手順の `ProcessURLAsync` メソッドとの唯一の違いは、<xref:System.Net.Http.HttpClient> インスタンス `client` の使用です。

    ```csharp
    async Task<int> ProcessURLAsync(string url, HttpClient client)
    {
        byte[] byteArray = await client.GetByteArrayAsync(url);
        DisplayResults(url, byteArray);
        return byteArray.Length;
    }
    ```

2. 次のコードに示すように、`For Each` の `foreach` または `SumPageSizesAsync` のループをコメント アウトするか削除します。

    ```csharp
    //var total = 0;
    //foreach (var url in urlList)
    //{
    //    // GetByteArrayAsync returns a Task<T>. At completion, the task
    //    // produces a byte array.
    //    byte[] urlContent = await client.GetByteArrayAsync(url);

    //    // The previous line abbreviates the following two assignment
    //    // statements.
    //    Task<byte[]> getContentTask = client.GetByteArrayAsync(url);
    //    byte[] urlContent = await getContentTask;

    //    DisplayResults(url, urlContent);

    //    // Update the total.
    //    total += urlContent.Length;
    //}
    ```

3. [ メソッドによって実行されるときに、各 Web サイトのコンテンツをダウンロードするタスクのコレクションを作成する](../linq/index.md)クエリ<xref:System.Linq.Enumerable.ToArray%2A>を定義します。 タスクは、クエリが評価されるときに開始されます。

    `SumPageSizesAsync` および `client` の宣言の後の `urlList` メソッドに、次のコードを追加します。

    ```csharp
    // Create a query.
    IEnumerable<Task<int>> downloadTasksQuery =
        from url in urlList select ProcessURLAsync(url, client);

    // Use ToArray to execute the query and start the download tasks.
    Task<int>[] downloadTasks = downloadTasksQuery.ToArray();
    ```

4. 次に、`Task.WhenAll` をタスクのコレクション `downloadTasks` に適用します。 `Task.WhenAll` は、タスクのコレクションのすべてのタスクが完了すると完了する、単一のタスクを返します。

    次の例では、`await` 式は、`WhenAll` によって返される単一のタスクが完了するのを待機します。 完了すると、`await` 式は、各整数がダウンロードされたサイトの長さである、整数の配列に評価します。 `SumPageSizesAsync` の、前の手順で追加したコードの直後に、次のコードを追加します。

    ```csharp
    // Await the completion of all the running tasks.
    int[] lengths = await Task.WhenAll(downloadTasks);

    //// The previous line is equivalent to the following two statements.
    //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
    //int[] lengths = await whenAllTask;
    ```

5. 最後に <xref:System.Linq.Enumerable.Sum%2A> メソッドを使って、すべての Web サイトの長さの合計を計算します。 `SumPageSizesAsync` に次の行を追加します。

    ```csharp
    int total = lengths.Sum();
    ```

### <a name="to-test-the-taskwhenall-solutions"></a>Task.WhenAll ソリューションをテストするには

- 各ソリューションで、F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。 出力は「[チュートリアル: Async と Await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)」の非同期のソリューションからの出力に似たものになります。 ただし、Web サイトは毎回、異なる順序で表示されることに注意してください。

## <a name="example"></a>例

次のコードは、`GetURLContentsAsync` メソッドを使用して Web からコンテンツをダウンロードするプロジェクトの拡張機能を示します。

```csharp
// Add the following using directives, and add a reference for System.Net.Http.
using System.Net.Http;
using System.IO;
using System.Net;

namespace AsyncExampleWPF_WhenAll
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

            // Two-step async call.
            Task sumTask = SumPageSizesAsync();
            await sumTask;

            // One-step async call.
            //await SumPageSizesAsync();

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // Create a query.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURLAsync(url);

            // Use ToArray to execute the query and start the download tasks.
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();

            // You can do other work here before awaiting.

            // Await the completion of all the running tasks.
            int[] lengths = await Task.WhenAll(downloadTasks);

            //// The previous line is equivalent to the following two statements.
            //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
            //int[] lengths = await whenAllTask;

            int total = lengths.Sum();

            //var total = 0;
            //foreach (var url in urlList)
            //{
            //    byte[] urlContents = await GetURLContentsAsync(url);

            //    // The previous line abbreviates the following two assignment statements.
            //    // GetURLContentsAsync returns a Task<T>. At completion, the task
            //    // produces a byte array.
            //    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
            //    //byte[] urlContents = await getContentsTask;

            //    DisplayResults(url, urlContents);

            //    // Update the total.
            //    total += urlContents.Length;
            //}

            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
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

        // The actions from the foreach loop are moved to this async method.
        private async Task<int> ProcessURLAsync(string url)
        {
            var byteArray = await GetURLContentsAsync(url);
            DisplayResults(url, byteArray);
            return byteArray.Length;
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
            {
                // Get the data stream that is associated with the specified url.
                using (Stream responseStream = response.GetResponseStream())
                {
                    await responseStream.CopyToAsync(content);
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

## <a name="example"></a>例

次のコードは、`HttpClient.GetByteArrayAsync` メソッドを使用して Web からコンテンツをダウンロードするプロジェクトの拡張機能を示します。

```csharp
// Add the following using directives, and add a reference for System.Net.Http.
using System.Net.Http;
using System.IO;
using System.Net;

namespace AsyncExampleWPF_HttpClient_WhenAll
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

            // One-step async call.
            await SumPageSizesAsync();

            // Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // Declare an HttpClient object and increase the buffer size. The
            // default buffer size is 65,536.
            HttpClient client = new HttpClient() { MaxResponseContentBufferSize = 1000000 };

            // Create a query.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURLAsync(url, client);

            // Use ToArray to execute the query and start the download tasks.
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();

            // You can do other work here before awaiting.

            // Await the completion of all the running tasks.
            int[] lengths = await Task.WhenAll(downloadTasks);

            //// The previous line is equivalent to the following two statements.
            //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
            //int[] lengths = await whenAllTask;

            int total = lengths.Sum();

            //var total = 0;
            //foreach (var url in urlList)
            //{
            //    // GetByteArrayAsync returns a Task<T>. At completion, the task
            //    // produces a byte array.
            //    byte[] urlContent = await client.GetByteArrayAsync(url);

            //    // The previous line abbreviates the following two assignment
            //    // statements.
            //    Task<byte[]> getContentTask = client.GetByteArrayAsync(url);
            //    byte[] urlContent = await getContentTask;

            //    DisplayResults(url, urlContent);

            //    // Update the total.
            //    total += urlContent.Length;
            //}

            // Display the total count for all of the web addresses.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
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

        // The actions from the foreach loop are moved to this async method.
        async Task<int> ProcessURLAsync(string url, HttpClient client)
        {
            byte[] byteArray = await client.GetByteArrayAsync(url);
            DisplayResults(url, byteArray);
            return byteArray.Length;
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each web site. The string format
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

- <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>
- [チュートリアル: async と await を使用した Web へのアクセス (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
