---
title: '方法: テキストのファイルへの書き込み'
ms.date: 01/04/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- writing text to files
- I/O [.NET Framework], writing text to files
- streams, writing text to files
- data streams, writing text to files
ms.assetid: 060cbe06-2adf-4337-9e7b-961a5c840208
ms.openlocfilehash: ba1c1815f0e49c02d1f0ee3c48ba01b7c2f5e727
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160249"
---
# <a name="how-to-write-text-to-a-file"></a>方法: テキストのファイルへの書き込み
このトピックでは、.NET アプリ用のファイルにテキストを書き込むさまざまな方法を示します。

テキストをファイルに書き込むには、一般に次のクラスおよびメソッドを使用します。  
  
- <xref:System.IO.StreamWriter> には、同期的にファイルに書き込むメソッド (<xref:System.IO.StreamWriter.Write%2A> と <xref:System.IO.TextWriter.WriteLine%2A>) または非同期的に書き込むメソッド (<xref:System.IO.StreamWriter.WriteAsync%2A> と <xref:System.IO.StreamWriter.WriteLineAsync%2A>) が含まれています。  
  
- <xref:System.IO.File> では、ファイルにテキストを書き込む静的メソッド (<xref:System.IO.File.WriteAllLines%2A>、<xref:System.IO.File.WriteAllText%2A> など)、またはファイルにテキストを追加する静的メソッド (<xref:System.IO.File.AppendAllLines%2A>、<xref:System.IO.File.AppendAllText%2A>、<xref:System.IO.File.AppendText%2A> など) が提供されています。  
  
- <xref:System.IO.Path> は、ファイルまたはディレクトリのパスの情報を含む文字列用です。 これには、<xref:System.IO.Path.Combine%2A> メソッドと、.NET Core 2.1 以降においてファイルまたはディレクトリのパスを作成するために文字列を連結できる <xref:System.IO.Path.Join%2A> および <xref:System.IO.Path.TryJoin%2A> メソッドが含まれます。

> [!NOTE]
> 次の例では、最小限必要なコードのみを示します。 通常、実際のアプリではこれよりも信頼性の高いエラー チェックと例外処理を行います。  
  
## <a name="example-synchronously-write-text-with-streamwriter"></a>例:StreamWriter で同期的にテキストを書き込む

次の例では、<xref:System.IO.StreamWriter> クラスを使用して、新しいファイルにテキストを一度に 1 行ずつ同期的に書き込む方法を示します。 <xref:System.IO.StreamWriter> オブジェクトが宣言されていて、`using` ステートメントでインスタンス化されるため、<xref:System.IO.StreamWriter.Dispose%2A> メソッドが呼び出され、それによってストリームが自動的にフラッシュされて閉じられます。  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/write.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/write.vb)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="example-synchronously-append-text-with-streamwriter"></a>例:StreamWriter で同期的にテキストを追加する

次の例では、<xref:System.IO.StreamWriter> クラスを使用して、最初の例で作成したテキスト ファイルにテキストを同期的に追加する方法を示します。

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/append.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/append.vb)]  

## <a name="example-asynchronously-write-text-with-streamwriter"></a>例:StreamWriter で非同期的にテキストを書き込む

次の例に、 <xref:System.IO.StreamWriter> クラスを使用して、新しいファイルにテキストを非同期的に書き込む方法を示します。 <xref:System.IO.StreamWriter.WriteAsync%2A> メソッドを呼び出すには、`async` メソッド内で呼び出す必要があります。 C# の例では、プログラム エントリ ポイントでの `async` 修飾子のサポートが追加されている C# 7.1 以降が必要です。

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/async.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/async.vb)]  

## <a name="example-write-and-append-text-with-the-file-class"></a>例:File クラスを使用してテキストの書き込みと追加を行う

次の例に、 <xref:System.IO.File> クラスを使用して、新しいファイルにテキストを書き込み、この同じファイルに新しいテキスト行を追加する方法を示します。 <xref:System.IO.File.WriteAllText%2A> および <xref:System.IO.File.AppendAllLines%2A> メソッドは、ファイルを自動的に開き、閉じます。 <xref:System.IO.File.WriteAllText%2A> メソッドに指定したパスが既に存在する場合、ファイルは上書きされます。  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/file.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/file.vb)]  

## <a name="see-also"></a>関連項目

- <xref:System.IO.StreamWriter>
- <xref:System.IO.Path>
- <xref:System.IO.File.CreateText%2A?displayProperty=nameWithType>
- [方法: ディレクトリとファイルを列挙する](../../../docs/standard/io/how-to-enumerate-directories-and-files.md)
- [方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)
- [方法: ログ ファイルを開いて情報を追加する](../../../docs/standard/io/how-to-open-and-append-to-a-log-file.md)
- [方法: ファイルからテキストを読み取る](../../../docs/standard/io/how-to-read-text-from-a-file.md)
- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
