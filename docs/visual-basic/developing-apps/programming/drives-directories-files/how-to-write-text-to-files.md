---
title: '方法: ファイルにテキストを書き込む'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- writing to files [Visual Basic]
- examples [Visual Basic], text files
ms.assetid: 304956eb-530d-4df7-b48f-9b4d1f2581a0
ms.openlocfilehash: ce1ee59ba71af6bb13e05a5bce37a2f7eee37712
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334471"
---
# <a name="how-to-write-text-to-files-in-visual-basic"></a>方法: ファイルにテキストを書き込む (Visual Basic)

<xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> メソッドを利用し、テキストをファイルに書き込みます。 指定したファイルが存在しない場合は、作成されます。  
  
## <a name="procedure"></a>プロシージャ  
  
#### <a name="to-write-text-to-a-file"></a>テキストをファイルに書き込むには  
  
- ファイルにテキストを書き込むには `WriteAllText` メソッドを使い、ファイルと書き込むテキストを指定します。 この例では、`"This is new text."` という名前のファイルに既に存在するテキストの最後に、`test.txt` という行を書き込んで追加します。  
  
     [!code-vb[VbFileIOWrite#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#3)]  
  
#### <a name="to-write-a-series-of-strings-to-a-file"></a>一連の文字列をファイルに書き込むには  
  
- 文字列のコレクションをループ処理します。 `WriteAllText` メソッドを使って、テキストをファイルに書き込みます。書き込むファイルと追加する文字列を指定し、`append` を `True` に設定します。  
  
     この例では、`Documents and Settings` ディレクトリにあるファイルの名前を `FileList.txt` に書き込み、読みやすくするために各ファイル名の間に改行を挿入します。  
  
     [!code-vb[VbFileIOWrite#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#4)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- パスが正しくない。長さが 0 の文字列である、空白だけが含まれている、使用できない文字が含まれている、デバイス パスである (先頭が \\\\.\\)、のいずれかの理由が考えられる (<xref:System.ArgumentException>)。  
  
- パスが `Nothing` であるため、有効でない (<xref:System.ArgumentNullException>)  
  
- `File` が、存在しないパスを指している (<xref:System.IO.FileNotFoundException> または <xref:System.IO.DirectoryNotFoundException>)。  
  
- 他のプロセスがファイルを使用しているか、または I/O エラーが発生した (<xref:System.IO.IOException>)。  
  
- パスがシステムで定義されている最大長を超えている (<xref:System.IO.PathTooLongException>)。  
  
- パス内のファイル名またはディレクトリ名にコロン (:) が含まれているか、または形式が無効である (<xref:System.NotSupportedException>)  
  
- ユーザーがパスを参照するのに必要なアクセス許可がない (<xref:System.Security.SecurityException>)  
  
- ディスクがいっぱいになっており、`WriteAllText` の呼び出しが失敗する (<xref:System.IO.IOException>)。  
  
 部分的に信頼されているコンテキストで実行している場合は、特権不足のため例外がスローされることがあります。 詳しくは、「[コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- [方法: テキスト ファイルからデータを読み取る](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-read-from-text-files.md)
