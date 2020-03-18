---
title: ファイル アクセスにおける非同期の使用 (C#)
ms.date: 07/20/2015
ms.assetid: bb018fea-5313-4c80-ab3f-7c24b2145bd9
ms.openlocfilehash: e6b0370049d9b9315de6a72d0e84c080aac12481
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69595536"
---
# <a name="using-async-for-file-access-c"></a>ファイル アクセスにおける非同期の使用 (C#)
ファイルにアクセスする際に非同期機能を使用できます。 非同期機能を使用すると、コールバックの使用や複数のメソッドまたはラムダ式へのコードの分割を行わずに、非同期メソッドを呼び出すことができます。 同期コードを非同期コードにするには、同期メソッドの代わりに非同期メソッドを呼び出して、コードにいくつかのキーワードを追加するだけで済みます。  
  
 ファイル アクセスの呼び出しに非同期性を適用する利点には、次のようなものがあります。  
  
- 非同期性により、UI アプリケーションの応答性が向上します。非同期処理を開始した UI スレッドが他の処理を実行できるためです。 UI スレッドが、時間のかかるコード、たとえば 50 ミリ秒を超えるコードを実行する必要がある場合、I/O が完了して、UI スレッドがキーボードやマウス入力などのイベントを再度処理できるようになるまで、UI が停止することがあります。  
  
- 非同期性を適用すると、スレッドの必要性が軽減され、ASP.NET などのサーバー ベースのアプリケーションのスケーラビリティが向上します。 アプリケーションが応答ごとに専用スレッドを使用している場合、1,000 個の要求を同時に処理するには、1,000 個のスレッドが必要です。 非同期処理では、待機中にスレッドを使用する必要がほとんどありません。 既存の I/O 完了スレッドが最後に少しだけ使用されます。  
  
- 現状ではファイル アクセス操作の待機時間が非常に短くても、将来に大幅に長くなる可能性があります。 たとえば、地球の裏側にあるサーバーにファイルが移動される場合があります。  
  
- 非同期機能の使用に伴うオーバーヘッドはわずかです。  
  
- 非同期タスクは簡単に並列実行できます。  
  
## <a name="running-the-examples"></a>例の実行  
 このトピックの例を実行するには、**WPF アプリケーション**または **Windows フォーム アプリケーション**を作成し、**ボタン**を追加します。 ボタンの `Click` イベントに、それぞれの例で最初のメソッドの呼び出しを追加してください。  
  
 以降の例には、次の `using` ステートメントを含めてください。  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Diagnostics;  
using System.IO;  
using System.Text;  
using System.Threading.Tasks;  
```  
  
## <a name="use-of-the-filestream-class"></a>FileStream クラスの使用  
 このトピックの例では、<xref:System.IO.FileStream> クラスを使用します。このクラスには、非同期 I/O をオペレーティング システム レベルで発生させるオプションが用意されています。 このオプションを使用すると、多くのケースで ThreadPool スレッドがブロックされるのを回避できます。 このオプションを有効にするには、コンストラクター呼び出しで `useAsync=true` または `options=FileOptions.Asynchronous` 引数を指定します。  
  
 ファイル パスを指定して <xref:System.IO.StreamReader> と <xref:System.IO.StreamWriter> を直接開いた場合、このオプションは使用できません。 一方、<xref:System.IO.Stream> クラスによって開かれた <xref:System.IO.FileStream> を使用する場合は、このオプションを使用できます。 UI アプリでは、ThreadPool スレッドがブロックされた場合でも、非同期呼び出しのほうが高速です。これは、UI スレッドは待機中にブロックされないためです。  
  
## <a name="writing-text"></a>テキストの書き込み  
 次の例では、ファイルにテキストを書き込みます。 各 await ステートメントに達すると、メソッドは直ちに終了します。 ファイル I/O が完了すると、メソッドは await ステートメントの後のステートメントから再開します。 await ステートメントを使用するメソッドの定義に async 修飾子が含まれていることに注意してください。  
  
```csharp  
public async Task ProcessWriteAsync()  
{  
    string filePath = @"temp2.txt";  
    string text = "Hello World\r\n";  
  
    await WriteTextAsync(filePath, text);  
}  
  
private async Task WriteTextAsync(string filePath, string text)  
{  
    byte[] encodedText = Encoding.Unicode.GetBytes(text);  
  
    using (FileStream sourceStream = new FileStream(filePath,  
        FileMode.Append, FileAccess.Write, FileShare.None,  
        bufferSize: 4096, useAsync: true))  
    {  
        await sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
    };  
}  
```  
  
 元の例には `await sourceStream.WriteAsync(encodedText, 0, encodedText.Length);` ステートメントがあります。これは、次の 2 つのステートメントの省略形です。  
  
```csharp  
Task theTask = sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
await theTask;  
```  
  
 最初のステートメントはタスクを返し、ファイル処理を開始します。 await が含まれた 2 番目のステートメントによって、メソッドが直ちに終了し、別のタスクを返します。 ファイル処理が完了すると、await の後のステートメントに実行が戻ります。 詳細については、「[非同期プログラムにおける制御フロー (C#)](./control-flow-in-async-programs.md)」を参照してください。  
  
## <a name="reading-text"></a>テキストの読み取り  
 次の例では、ファイルからテキストを読み取ります。 テキストはバッファーに格納されます。この例では <xref:System.Text.StringBuilder> に配置されます。 前の例と異なり、await の評価で値が生成されます。 <xref:System.IO.Stream.ReadAsync%2A> メソッドによって <xref:System.Threading.Tasks.Task>\<<xref:System.Int32> が返されます。処理の完了後、await の評価によって `Int32` 値 (`numRead`) が生成されます。 詳細については、「[非同期の戻り値の型 (C#)](./async-return-types.md)」を参照してください。  
  
```csharp  
public async Task ProcessReadAsync()  
{  
    string filePath = @"temp2.txt";  
  
    if (File.Exists(filePath) == false)  
    {  
        Debug.WriteLine("file not found: " + filePath);  
    }  
    else  
    {  
        try  
        {  
            string text = await ReadTextAsync(filePath);  
            Debug.WriteLine(text);  
        }  
        catch (Exception ex)  
        {  
            Debug.WriteLine(ex.Message);  
        }  
    }  
}  
  
private async Task<string> ReadTextAsync(string filePath)  
{  
    using (FileStream sourceStream = new FileStream(filePath,  
        FileMode.Open, FileAccess.Read, FileShare.Read,  
        bufferSize: 4096, useAsync: true))  
    {  
        StringBuilder sb = new StringBuilder();  
  
        byte[] buffer = new byte[0x1000];  
        int numRead;  
        while ((numRead = await sourceStream.ReadAsync(buffer, 0, buffer.Length)) != 0)  
        {  
            string text = Encoding.Unicode.GetString(buffer, 0, numRead);  
            sb.Append(text);  
        }  
  
        return sb.ToString();  
    }  
}  
```  
  
## <a name="parallel-asynchronous-io"></a>並列非同期 I/O  
 次の例では、10 個のテキスト ファイルを記述する並列処理を示します。 <xref:System.IO.Stream.WriteAsync%2A> メソッドは、ファイルごとにタスクを返します。タスクはタスクの一覧に追加されます。 `await Task.WhenAll(tasks);` ステートメントはメソッドを終了し、すべてのタスクのファイル処理が完了すると、メソッド内で再開します。  
  
 この例では、タスクの完了後、<xref:System.IO.FileStream> ブロックのすべての `finally` インスタンスを閉じます。 `FileStream` ステートメントで `using` が作成された場合は、タスクが完了する前に `FileStream` が破棄されることがあります。  
  
 パフォーマンスの向上のほとんどが、非同期処理ではなく並列処理によって実現していることに注意してください。 非同期性の利点は、複数のスレッドやユーザー インターフェイス スレッドが拘束されなくなる点にあります。  
  
```csharp  
public async Task ProcessWriteMultAsync()  
{  
    string folder = @"tempfolder\";  
    List<Task> tasks = new List<Task>();  
    List<FileStream> sourceStreams = new List<FileStream>();  
  
    try  
    {  
        for (int index = 1; index <= 10; index++)  
        {  
            string text = "In file " + index.ToString() + "\r\n";  
  
            string fileName = "thefile" + index.ToString("00") + ".txt";  
            string filePath = folder + fileName;  
  
            byte[] encodedText = Encoding.Unicode.GetBytes(text);  
  
            FileStream sourceStream = new FileStream(filePath,  
                FileMode.Append, FileAccess.Write, FileShare.None,  
                bufferSize: 4096, useAsync: true);  
  
            Task theTask = sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
            sourceStreams.Add(sourceStream);  
  
            tasks.Add(theTask);  
        }  
  
        await Task.WhenAll(tasks);  
    }  
  
    finally  
    {  
        foreach (FileStream sourceStream in sourceStreams)  
        {  
            sourceStream.Close();  
        }  
    }  
}  
```  
  
 <xref:System.IO.Stream.WriteAsync%2A> メソッドと <xref:System.IO.Stream.ReadAsync%2A> メソッドを使用すると、<xref:System.Threading.CancellationToken> を指定して、途中で処理をキャンセルすることができます。 詳細については、「[非同期アプリケーションの微調整 (C#)](./fine-tuning-your-async-application.md)」および「[マネージド スレッドのキャンセル](../../../../standard/threading/cancellation-in-managed-threads.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Async および Await を使用した非同期プログラミング (C#)](./index.md)
- [非同期の戻り値の型 (C#)](./async-return-types.md)
- [非同期プログラムにおける制御フロー (C#)](./control-flow-in-async-programs.md)
