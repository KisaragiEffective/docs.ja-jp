---
title: '方法: テキスト ファイルからデータを読み取る'
ms.date: 07/20/2015
helpviewer_keywords:
- extended characters [Visual Basic], reading
- reading text files [Visual Basic]
- reading data, text files
- examples [Visual Basic], reading text files
- text files [Visual Basic], reading
ms.assetid: 735fe9d7-0f7a-4185-ba02-f35e580ec4b8
ms.openlocfilehash: 8af088ad269cc77bc5c83aedb86bde9af2e37a15
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334586"
---
# <a name="how-to-read-from-text-files-in-visual-basic"></a>方法: テキスト ファイルからデータを読み取る (Visual Basic)

<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.ReadAllText%2A> オブジェクトの `My.Computer.FileSystem` メソッドを使用すると、テキスト ファイルを読み取ることができます。 ファイルの内容が ASCII や UTF-8 などのエンコーディングを使用している場合、ファイル エンコーディングを指定できます。

読み取るファイルで拡張文字が使用されている場合、ファイル エンコーディングを指定する必要があります。

> [!NOTE]
> ファイルから 1 回に 1 行のテキストを読み取るには、<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.OpenTextFileReader%2A> オブジェクトの `My.Computer.FileSystem` メソッドを使用します。 `OpenTextFileReader` メソッドは <xref:System.IO.StreamReader> オブジェクトを返します。 <xref:System.IO.StreamReader.ReadLine%2A> オブジェクトの `StreamReader` メソッドを使用して、ファイルから 1 回に 1 行を読み取ることができます。 <xref:System.IO.StreamReader.EndOfStream%2A> オブジェクトの `StreamReader` メソッドを使用して、ファイルの最後かどうかをテストできます。

## <a name="to-read-from-a-text-file"></a>テキスト ファイルを読み取るには

`ReadAllText` オブジェクトの `My.Computer.FileSystem` メソッドを使用して、ファイルのパスを指定し、テキスト ファイルの内容を文字列に読み取ります。 次の例は、test.txt の内容を文字列に読み取り、メッセージ ボックスに表示します。

[!code-vb[VbFileIORead#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#2)]

### <a name="to-read-from-a-text-file-that-is-encoded"></a>エンコードされているテキスト ファイルを読み取るには

`ReadAllText` オブジェクトの `My.Computer.FileSystem` メソッドを使用して、ファイルのパスとファイル エンコードの種類を指定し、テキスト ファイルの内容を文字列に読み取ります。 次の例は、UTF32 ファイルである test.txt の内容を文字列に読み取り、メッセージ ボックスに表示します。

[!code-vb[VbFileIORead#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#3)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

次の条件を満たす場合は、例外が発生する可能性があります。

- パスが無効である場合。1) 長さが 0 の文字列である、2) 空白だけが含まれている、3) 無効な文字が含まれている、4) デバイス パスである、のいずれかの理由が考えられる (<xref:System.ArgumentException>)。

- パスが `Nothing` であるため、有効でない (<xref:System.ArgumentNullException>)

- ファイルが存在しない (<xref:System.IO.FileNotFoundException>)。

- 他のプロセスがファイルを使用しているか、または I/O エラーが発生した (<xref:System.IO.IOException>)。

- パスがシステムで定義されている最大長を超えている (<xref:System.IO.PathTooLongException>)。

- パス内のファイル名またはディレクトリ名にコロン (:) が含まれているか、または形式が無効である (<xref:System.NotSupportedException>)

- 文字列をバッファーに書き込むための十分なメモリがない (<xref:System.OutOfMemoryException>)

- ユーザーがパスを参照するのに必要なアクセス許可がない (<xref:System.Security.SecurityException>)

ファイル名からファイルの内容を判断しないでください。 たとえば、Form1.vb というファイルは Visual Basic のソース ファイルではない可能性もあります。

アプリケーションでデータを使用する前に、入力をすべて検証してください。 ファイルの内容が予想どおりでないことがあり、ファイルの内容を読み取るメソッドが失敗する可能性があります。

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllText%2A>
- [ファイルの読み取り](../../../../visual-basic/developing-apps/programming/drives-directories-files/reading-from-files.md)
- [方法: コンマ区切りのテキスト ファイルを読み取る](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-read-from-comma-delimited-text-files.md)
- [方法: 固定幅のテキスト ファイルを読み取る](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-read-from-fixed-width-text-files.md)
- [方法: 複数の書式を持つテキスト ファイルを読み取る](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-read-from-text-files-with-multiple-formats.md)
- [トラブルシューティング : テキスト ファイルの読み取りと書き込み](../../../../visual-basic/developing-apps/programming/drives-directories-files/troubleshooting-reading-from-and-writing-to-text-files.md)
- [チュートリアル: Visual Basic によるファイルとディレクトリの操作](../../../../visual-basic/developing-apps/programming/drives-directories-files/walkthrough-manipulating-files-and-directories.md)
- [ファイル エンコーディング](../../../../visual-basic/developing-apps/programming/drives-directories-files/file-encodings.md)
