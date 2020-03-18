---
title: 非同期ファイル I/O
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- streams, synchronous streams
- asynchronous I/O
- synchronous I/O
- streams, asynchronous streams
- I/O [.NET Framework], asynchronous I/O
- Stream class, synchronous I/O
- data streams, asynchronous streams
- Stream class, asynchronous I/O
- multiple I/O requests
- data streams, synchronous streams
ms.assetid: dbdd55e7-d6b9-4f9e-8abb-ab0edd4457f7
ms.openlocfilehash: 66e7d01f37a1119b9d2076a9131aa40f26d15625
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708192"
---
# <a name="asynchronous-file-io"></a>非同期ファイル I/O

非同期操作では、メイン スレッドをブロックすることなくリソース使用量の多い I/O 操作を実行できます。 このパフォーマンスに関する考慮事項は、時間のかかるストリーム操作によって UI スレッドがブロックされ、アプリが動作していないと見なされる可能性がある Windows 8.x Store アプリまたはデスクトップ アプリで特に重要です。

.NET Framework 4.5 以降の I/O 型には非同期操作を単純化する非同期メソッドが含まれています。 非同期のメソッドの名前には `Async` が含まれます ( <xref:System.IO.Stream.ReadAsync%2A>、 <xref:System.IO.Stream.WriteAsync%2A>、 <xref:System.IO.Stream.CopyToAsync%2A>、 <xref:System.IO.Stream.FlushAsync%2A>、 <xref:System.IO.TextReader.ReadLineAsync%2A>、 <xref:System.IO.TextReader.ReadToEndAsync%2A>など)。 これらの非同期のメソッドは、 <xref:System.IO.Stream>、 <xref:System.IO.FileStream>、 <xref:System.IO.MemoryStream>などのストリーム クラスと、 <xref:System.IO.TextReader> 、 <xref:System.IO.TextWriter>などのストリームとの間の読み取り/書き込みに使用されるクラスに実装されます。

.NET Framework 4 およびそれ以前のバージョンで非同期 I/O 操作を実装するには、 <xref:System.IO.Stream.BeginRead%2A> 、 <xref:System.IO.Stream.EndRead%2A> などのメソッドを使用する必要があります。 これらのメソッドは、レガシ コードをサポートするために .NET Framework 4.5 でも使用できますが、非同期 I/O 操作は非同期のメソッドを使用して実装する方が簡単です。

C# と Visual Basic には、非同期プログラミング用のキーワードがそれぞれ 2 つあります。

- `Async` (Visual Basic) 修飾子または `async` (C#) 修飾子。非同期操作を含むメソッドをマークするために使用されます。

- `Await` (Visual Basic) 演算子または `await` (C#) 演算子。非同期のメソッドの結果に適用されます。

非同期 I/O 操作を実装するには、これらのキーワードを非同期のメソッドと組み合わせて使用します。次に例を示します。 詳細については、「[Async および Await を使用した非同期プログラミング (C#)](../../csharp/programming-guide/concepts/async/index.md)」または「[Async および Await を使用した非同期プログラミング (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)」を参照してください。

次の例では、2 つの <xref:System.IO.FileStream> オブジェクトを使用して、ファイルをディレクトリ間で非同期にコピーする方法を示します。 非同期のメソッドを呼び出すので、 <xref:System.Web.UI.WebControls.Button.Click> コントロールの <xref:System.Windows.Controls.Button> イベント ハンドラーは `async` 修飾子でマークされていることに注意してください。

[!code-csharp[Asynchronous_File_IO_async#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Asynchronous_File_IO_async/cs/example.cs#1)]
[!code-vb[Asynchronous_File_IO_async#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Asynchronous_File_IO_async/vb/example.vb#1)]

次の例は前述の例と似ていますが、 <xref:System.IO.StreamReader> オブジェクトと <xref:System.IO.StreamWriter> オブジェクトを使用して、テキスト ファイルの内容の読み取り/書き込みを非同期で行います。

[!code-csharp[Asynchronous_File_IO_async#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Asynchronous_File_IO_async/cs/example2.cs#2)]
[!code-vb[Asynchronous_File_IO_async#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Asynchronous_File_IO_async/vb/example2.vb#2)]

次の例では、分離コード ファイルと XAML ファイルを使用して Windows 8.x Store アプリでファイルを <xref:System.IO.Stream> として開き、<xref:System.IO.StreamReader> クラスのインスタンスを使用してその内容を読み取る方法を示します。 ここでは、非同期のメソッドを使用して、ファイルをストリームとして開き、ファイルの内容を読み取ります。

[!code-csharp[System.IO.WindowsRuntimeStorageExtensions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/cs/blankpage.xaml.cs#2)]
[!code-vb[System.IO.WindowsRuntimeStorageExtensions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/vb/blankpage.xaml.vb#2)]

[!code-xaml[System.IO.WindowsRuntimeStorageExtensions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/cs/blankpage.xaml#1)]

## <a name="see-also"></a>参照

- <xref:System.IO.Stream>
- [ファイルおよびストリーム入出力](index.md)
- [Async および Await を使用した非同期プログラミング (C#)](../../csharp/programming-guide/concepts/async/index.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)
