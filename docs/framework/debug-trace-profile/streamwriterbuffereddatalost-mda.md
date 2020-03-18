---
title: streamWriterBufferedDataLost MDA
ms.date: 03/30/2017
helpviewer_keywords:
- StreamWriter class, data buffering problems
- managed debugging assistants (MDAs), StreamWriter data buffering
- buffers, StreamWriter problems
- MDAs (managed debugging assistants), StreamWriter data buffering
- StreamWriter buffered data lost
- data buffering problems
- streamWriterBufferedDataLost MDA
ms.assetid: 6e5c07be-bc5b-437a-8398-8779e23126ab
ms.openlocfilehash: 82940b40b302f4a928547f2e6a0c285727e13934
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216096"
---
# <a name="streamwriterbuffereddatalost-mda"></a>streamWriterBufferedDataLost MDA
`streamWriterBufferedDataLost` マネージド デバッグ アシスタント (MDA) は <xref:System.IO.StreamWriter> が書き込まれたときに起動しますが、その後、<xref:System.IO.StreamWriter.Flush%2A> のインスタンスが破棄される前に <xref:System.IO.StreamWriter.Close%2A> または <xref:System.IO.StreamWriter> メソッドが呼び出されません。 この MDA が有効になると、バッファーに入れられたデータが <xref:System.IO.StreamWriter> 内に残っているか、ランタイムにより判断されます。 バッファーに入れられたデータが残っている場合、MDA が起動します。 <xref:System.GC.Collect%2A> メソッドと <xref:System.GC.WaitForPendingFinalizers%2A> メソッドを呼び出すことで、ファイナライザーを強制的に実行できます。 それ以外の場合、ファイナライザーは任意のタイミングで実行されます。プロセス終了時に実行されることは、ほぼありません。 この MDA が有効になっている状態でファイナライザーを明示的に実行すると、この種類の問題をより確実に再現できます。  
  
## <a name="symptoms"></a>現象  
 <xref:System.IO.StreamWriter> では、最後の 1 – 4 KB のデータがファイルに書き込まれません。  
  
## <a name="cause"></a>原因  
 <xref:System.IO.StreamWriter> はデータを内部でバッファーに入れます。このとき、<xref:System.IO.StreamWriter.Close%2A> または <xref:System.IO.StreamWriter.Flush%2A> メソッドを呼び出し、バッファーに入れたデータを基礎となるデータ ストアに書き込む必要があります。 <xref:System.IO.StreamWriter.Close%2A> または <xref:System.IO.StreamWriter.Flush%2A> が正しく呼び出されない場合、<xref:System.IO.StreamWriter> インスタンスのバッファーに入れられたデータは予想どおりに書き込まれないことがあります。  
  
 次は、この MDA がキャッチする、書き込みが十分ではないコードの例です。  
  
```csharp  
// Poorly written code.  
void Write()   
{  
    StreamWriter sw = new StreamWriter("file.txt");  
    sw.WriteLine("Data");  
    // Problem: forgot to close the StreamWriter.  
}  
```  
  
 開始したガベージ コレクションがファイナライザーの完了まで保留となる場合、先行のコードがこの MDA をより確実に起動します。 この種類の問題を追跡するために、デバッグ ビルドで、先行メソッドの終わりに次のコードを追加できます。 これで MDA が起動する確率が高くなりますが、もちろん、問題の根本原因が解消されるわけではありません。  
  
```csharp
GC.Collect();  
GC.WaitForPendingFinalizers();  
```  
  
## <a name="resolution"></a>解決策  
 アプリケーションを閉じる前に、あるいは、<xref:System.IO.StreamWriter.Close%2A> のインスタンスが含まれるコード ブロックを終了する前に、<xref:System.IO.StreamWriter.Flush%2A> で <xref:System.IO.StreamWriter> または <xref:System.IO.StreamWriter> を呼び出します。 これを最も効率的に行う方法は、C# `using` ブロック (Visual Basic の場合、`Using`) でインスタンスを作成することです。ライターの <xref:System.IO.StreamWriter.Dispose%2A> メソッドが呼び出され、インスタンスが正しく終了します。  
  
```csharp
using(StreamWriter sw = new StreamWriter("file.txt"))   
{  
    sw.WriteLine("Data");  
}  
```  
  
 次のコードは同じ解決策ですが、`try/finally` の代わりに `using` が使用されています。  
  
```csharp
StreamWriter sw;  
try   
{  
    sw = new StreamWriter("file.txt"));  
    sw.WriteLine("Data");  
}  
finally   
{  
    if (sw != null)  
        sw.Close();  
}  
```  
  
 いずれの解決策も利用できない場合 (<xref:System.IO.StreamWriter> が静的変数に保存されており、その有効期間の終わりにコードを実行することが簡単でない場合など)、最後に使用した後で <xref:System.IO.StreamWriter.Flush%2A> で <xref:System.IO.StreamWriter> を呼び出すか、最初に使用する前に <xref:System.IO.StreamWriter.AutoFlush%2A> プロパティを `true` に設定すると、この問題を解決できるはずです。  
  
```csharp
private static StreamWriter log;  
// static class constructor.  
static WriteToFile()   
{  
    StreamWriter sw = new StreamWriter("log.txt");  
    sw.AutoFlush = true;  
  
    // Publish the StreamWriter for other threads.  
    log = sw;  
}  
```  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は、ランタイムに影響しません。  
  
## <a name="output"></a>出力  
 この違反が発生したことを示すメッセージ  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <streamWriterBufferedDataLost />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.IO.StreamWriter>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
