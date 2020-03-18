---
title: '方法 : バイナリ ファイルに書き込む'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], binary access
- WriteAllBytes method [Visual Basic]
- binary files [Visual Basic], writing in Visual Basic
ms.assetid: 59fae125-de5b-4c96-883c-209f4a55112c
ms.openlocfilehash: 72d019f5f49868bd84d0507535e8ebc547b50e25
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334431"
---
# <a name="how-to-write-to-binary-files-in-visual-basic"></a>方法 : Visual Basic でバイナリ ファイルに書き込む

<xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllBytes%2A> メソッドは、バイナリ ファイルにデータを書き込みます。 `append` パラメーターが `True` の場合、データはファイルに追加されます。それ以外の場合は、ファイル内のデータが上書きされます。

指定されたパスのファイル名を除く部分が有効でない場合は、<xref:System.IO.DirectoryNotFoundException> 例外がスローされます。 パスが有効でもファイルが存在しない場合は、ファイルが作成されます。

## <a name="to-write-to-a-binary-file"></a>バイナリ ファイルに書き込むには

`WriteAllBytes` メソッドを使い、ファイルのパス、ファイルの名前、書き込むバイトを指定します。 この例では、データ配列 `CustomerData` を `CollectedData.dat` という名前のファイルに追加します。

[!code-vb[VbVbcnMyFileSystem#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#27)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

次の条件を満たす場合は、例外が発生する可能性があります。

- 文字列の長さが 0 である、空白文字だけが含まれる、または無効な文字が含まれるために、パスが無効である (<xref:System.ArgumentException>).

- パスが `Nothing` であるため、有効でない (<xref:System.ArgumentNullException>)

- `File` が、存在しないパスを指している (<xref:System.IO.FileNotFoundException> または <xref:System.IO.DirectoryNotFoundException>)。

- 他のプロセスがファイルを使用しているか、または I/O エラーが発生した (<xref:System.IO.IOException>)。

- パスがシステムで定義されている最大長を超えている (<xref:System.IO.PathTooLongException>)。

- パス内のファイル名またはディレクトリ名にコロン (:) が含まれているか、または形式が無効である (<xref:System.NotSupportedException>)

- ユーザーがパスを参照するのに必要なアクセス許可がない (<xref:System.Security.SecurityException>)

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllBytes%2A>
- [方法: ファイルにテキストを書き込む](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-write-text-to-files.md)
