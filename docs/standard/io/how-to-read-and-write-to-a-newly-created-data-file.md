---
title: '方法: 新しく作成されたデータ ファイルに対して読み書きする'
ms.date: 01/21/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- streams, reading and writing data
- BinaryReader class, examples
- I/O [.NET Framework], reading data
- I/O [.NET Framework], writing data
- BinaryWriter class, examples
ms.assetid: e209d949-31e8-44ea-8e38-87f9093f3093
ms.openlocfilehash: 3772aeeb25d1251a13fde2a0e51a913e5e39eabc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78155751"
---
# <a name="how-to-read-and-write-to-a-newly-created-data-file"></a>方法: 新しく作成されたデータ ファイルに対して読み書きする
<xref:System.IO.BinaryWriter?displayProperty=nameWithType> クラスおよび <xref:System.IO.BinaryReader?displayProperty=nameWithType> クラスは、文字列ではない形式でデータを書き込んだり読み取ったりするために使用します。 次の例では、空のファイル ストリームを作成し、それにデータを書き込み、それからデータを読み取る方法を示します。

この例では、現在のディレクトリに *Test.data* という名前のデータ ファイルが作成され、関連する <xref:System.IO.BinaryWriter> オブジェクトと <xref:System.IO.BinaryReader> オブジェクトが作成され、<xref:System.IO.BinaryWriter> オブジェクトを使用し、0 から 10 までの整数が *Test.data* に書き込まれます。ファイル ポインターがファイルの末尾に残ります。 次に、<xref:System.IO.BinaryReader> オブジェクトによってファイル ポインターが起点に戻され、指定された内容が読み出されます。  
  
> [!NOTE]
> *Test.data* が既に現在のディレクトリに存在する場合は、<xref:System.IO.IOException> 例外がスローされます。 例外をスローせず、常に新しいファイルを作成するには、ファイル モード オプションとして <xref:System.IO.FileMode.CreateNew?displayProperty=nameWithType> ではなく <xref:System.IO.FileMode.Create?displayProperty=nameWithType> を使用します。  
  
## <a name="example"></a>例  
 [!code-csharp[System.IO.BinaryReaderWriter#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.BinaryReaderWriter/CS/source6.cs#7)]
 [!code-vb[System.IO.BinaryReaderWriter#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.BinaryReaderWriter/VB/source6.vb#7)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.BinaryReader>  
- <xref:System.IO.BinaryWriter>  
- <xref:System.IO.FileStream>  
- <xref:System.IO.FileStream.Seek%2A?displayProperty=nameWithType>  
- <xref:System.IO.SeekOrigin>  
- [方法: ディレクトリとファイルを列挙する](../../../docs/standard/io/how-to-enumerate-directories-and-files.md)  
- [方法: ログ ファイルを開いて情報を追加する](../../../docs/standard/io/how-to-open-and-append-to-a-log-file.md)  
- [方法: ファイルからテキストを読み取る](../../../docs/standard/io/how-to-read-text-from-a-file.md)  
- [方法: テキストのファイルへの書き込み](../../../docs/standard/io/how-to-write-text-to-a-file.md)  
- [方法: 文字列からの文字の読み取り](../../../docs/standard/io/how-to-read-characters-from-a-string.md)  
- [方法: 文字列への文字の書き込み](../../../docs/standard/io/how-to-write-characters-to-a-string.md)  
- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
