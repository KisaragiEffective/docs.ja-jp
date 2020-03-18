---
title: '方法: 複数の書式を持つテキスト ファイルを読み取る'
ms.date: 07/20/2015
helpviewer_keywords:
- TextFieldParser object, reading from a file
- TextFieldType enumeration
- My.Computer.FileSystem.WriteAllText method, parsing structured text files
- WriteAllText method [Visual Basic], parsing structured text files
- PeekChars method [Visual Basic], determining format of text
- reading text files [Visual Basic], multiple formats
- I/O [Visual Basic], reading text files
- text files [Visual Basic], reading
ms.assetid: 8d185eb2-79ca-42cd-95a7-d3ff44a5a0f8
ms.openlocfilehash: b36c781d5f8333749d346bb8f19540f0d1bd1692
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334573"
---
# <a name="how-to-read-from-fext-files-with-multiple-formats-in-visual-basic"></a>方法: Visual Basic で複数の書式を持つテキスト ファイルを読み取る

<xref:Microsoft.VisualBasic.FileIO.TextFieldParser> オブジェクトには、構造化されたテキスト ファイル (ログなど) を簡単にかつ効率的に解析する方法が備わっています。 `PeekChars` メソッドを使用して、ファイルを解析するときに各行の書式を判断すると、複数の書式を持つファイルを処理できます。
  
### <a name="to-parse-a-text-file-with-multiple-formats"></a>複数の書式を持つテキスト ファイルを解析するには

1. *testfile.txt* という名前のテキスト ファイルをプロジェクトに追加します。 次の内容をテキスト ファイルに追加します。

    ```text
    Err  1001 Cannot access resource.
    Err  2014 Resource not found.
    Acc  10/03/2009User1      Administrator.
    Err  0323 Warning: Invalid access attempt.
    Acc  10/03/2009User2      Standard user.
    Acc  10/04/2009User2      Standard user.
    ```

2. 予期される形式と、エラーが報告されたときに使用される形式を定義します。 各配列の最後のエントリは -1 です。そのため、最後のフィールドは可変幅であると見なされます。 これは、配列の最後のエントリが 0 以下の場合に発生します。

     [!code-vb[VbFileIORead#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#4)]

3. 新しい <xref:Microsoft.VisualBasic.FileIO.TextFieldParser> オブジェクトを作成し、幅と形式を定義します。

     [!code-vb[VbFileIORead#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#5)]

4. 各行をループし、読み取り前に書式を調べます。

     [!code-vb[VbFileIORead#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#6)]

5. エラーをコンソールに書き込みます。

     [!code-vb[VbFileIORead#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#7)]

## <a name="example"></a>例

この例では、`testfile.txt` ファイルを読み取ります。

 [!code-vb[VbFileIORead#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#8)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

次の条件を満たす場合は、例外が発生する可能性があります。  
  
- 指定された形式で行を解析できない (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>)。 例外の原因となった行が例外メッセージで報告され、その行に含まれているテキストには <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> プロパティが代入されます。
- 指定されたファイルが存在しない (<xref:System.IO.FileNotFoundException>)。
- 部分信頼の状況下で、ファイルにアクセスするために必要なアクセス許可がユーザーにない。 (<xref:System.Security.SecurityException>).
- パスが長すぎる (<xref:System.IO.PathTooLongException>)。
- ファイルにアクセスする十分なアクセス許可がユーザーにない (<xref:System.UnauthorizedAccessException>)。

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.PeekChars%2A>
- <xref:Microsoft.VisualBasic.FileIO.MalformedLineException>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.EndOfData%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TextFieldType%2A>
- [方法: コンマ区切りのテキスト ファイルを読み取る](how-to-read-from-comma-delimited-text-files.md)
- [方法: 固定幅のテキスト ファイルを読み取る](how-to-read-from-fixed-width-text-files.md)
- [TextFieldParser オブジェクトによるテキスト ファイルの解析](parsing-text-files-with-the-textfieldparser-object.md)
