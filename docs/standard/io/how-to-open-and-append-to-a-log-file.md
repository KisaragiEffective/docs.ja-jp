---
title: '方法: ログ ファイルを開いて情報を追加する'
ms.date: 01/21/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- log files, opening
- streams, opening and appending to log file
- log files, appending to
- I/O [.NET Framework], log files
ms.assetid: 74423362-1721-49cb-aa0a-e04005f72a06
ms.openlocfilehash: a549aba3a763bcfc5a3889efd65e2495eca7622c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78155712"
---
# <a name="how-to-open-and-append-to-a-log-file"></a>方法: ログ ファイルを開いて情報を追加する
<xref:System.IO.StreamWriter> および <xref:System.IO.StreamReader> は、ストリームから文字の書き込んだり、読み取りを行います。 次のコード例は、入力用に *log.txt* ファイルを開くか、まだファイルがない場合、ファイルを作成し、ファイルの末尾に情報を追加します。 次に、ファイルの内容が表示用に標準出力に書き込まれます。

この例の代わりとして、情報を 1 つの文字列または文字列配列として格納し、<xref:System.IO.File.WriteAllText%2A?displayProperty=nameWithType> または <xref:System.IO.File.WriteAllLines%2A?displayProperty=nameWithType> メソッドを使用して同じ機能を実現できます。  
  
> [!NOTE]
> Visual Basic を使用するユーザーは、ログ ファイルの作成またはログ ファイルへの書き込みのために、<xref:Microsoft.VisualBasic.Logging.Log> クラスまたは <xref:Microsoft.VisualBasic.FileIO.FileSystem> クラスによって提供されるメソッドまたはプロパティを使用することを選択できます。  
  
## <a name="example"></a>例  
 [!code-csharp[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/source2.cs#2)]
 [!code-vb[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/source2.vb#2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.StreamWriter>  
- <xref:System.IO.StreamReader>  
- <xref:System.IO.File.AppendText%2A?displayProperty=nameWithType>  
- <xref:System.IO.File.OpenText%2A?displayProperty=nameWithType>  
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>  
- [方法: ディレクトリとファイルを列挙する](../../../docs/standard/io/how-to-enumerate-directories-and-files.md)  
- [方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)  
- [方法: ファイルからテキストを読み取る](../../../docs/standard/io/how-to-read-text-from-a-file.md)  
- [方法: テキストのファイルへの書き込み](../../../docs/standard/io/how-to-write-text-to-a-file.md)  
- [方法: 文字列からの文字の読み取り](../../../docs/standard/io/how-to-read-characters-from-a-string.md)  
- [方法: 文字列への文字の書き込み](../../../docs/standard/io/how-to-write-characters-to-a-string.md)  
- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
