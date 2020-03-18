---
title: '方法: 特定のパターンに一致するサブディレクトリを検索する'
ms.date: 07/20/2015
helpviewer_keywords:
- pattern matching
- folders, finding
ms.assetid: c9265fd1-7483-4150-8b7f-ff642caa939d
ms.openlocfilehash: c8e13598080139cafabffb2e17d0a3b99c37dc5d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348767"
---
# <a name="how-to-find-subdirectories-with-a-specific-pattern-in-visual-basic"></a>方法: 特定のパターンに一致するサブディレクトリを検索する (Visual Basic)

<xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A> メソッドは、ディレクトリのサブディレクトリのパス名を表す文字列の読み取り専用のコレクションを返します。 `wildCards` パラメーターを使用して、特定のパターンを指定できます。 サブディレクトリの内容を検索対象に含めるには、`searchType` パラメーターを `SearchOption.SearchAllSubDirectories` に設定します。

指定したパターンに一致するディレクトリが見つからなかった場合は、空のコレクションが返されます。

## <a name="to-find-subdirectories-with-a-specific-pattern"></a>特定のパターンに一致するサブディレクトリを検索するには

検索するディレクトリの名前およびパスを指定して、`GetDirectories` メソッドを使用します。 次の例では、ディレクトリ構造内で名前に "Logs" という単語を含むすべてのディレクトリが返され、`ListBox1` に追加されます。

[!code-vb[VbVbcnFileAccess#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnFileAccess/VB/Class1.vb#1)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

次の条件を満たす場合は、例外が発生する可能性があります。

- パスが正しくない。長さが 0 の文字列である、空白だけが含まれている、使用できない文字が含まれている、デバイス パスである (先頭が \\\\.\\)、のいずれかの理由が考えられる (<xref:System.ArgumentException>)。

- パスが `Nothing` であるため、有効でない (<xref:System.ArgumentNullException>)

- 指定したワイルドカード文字の中に 1 つ以上の `Nothing`、空の文字列が含まれている、または空白のみである (<xref:System.ArgumentNullException>)。

- `directory` が存在しない (<xref:System.IO.DirectoryNotFoundException>)。

- `directory` が既存のファイルを指している (<xref:System.IO.IOException>)。

- パスがシステムで定義されている最大長を超えている (<xref:System.IO.PathTooLongException>)。

- パス内のファイル名またはフォルダー名にコロン (:) が含まれている、または形式が無効である (<xref:System.NotSupportedException>)。

- ユーザーがパスを参照するのに必要なアクセス許可がない (<xref:System.Security.SecurityException>)

- ユーザーに必要な権限がない (<xref:System.UnauthorizedAccessException>)。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A>
- [方法: 特定のパターンに一致するファイルを検索する](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-find-files-with-a-specific-pattern.md)
