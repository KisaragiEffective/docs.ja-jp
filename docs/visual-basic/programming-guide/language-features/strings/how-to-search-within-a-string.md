---
title: '方法: 文字列内を検索する'
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], finding
- strings [Visual Basic], searching
- examples [Visual Basic], strings
ms.assetid: ae4c79e0-08ea-489f-bdb2-5eb6d355f284
ms.openlocfilehash: 655f746e4e496e1935afcd2a9f9fe36d9e3a2564
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348419"
---
# <a name="how-to-search-within-a-string-visual-basic"></a>方法: 文字列内を検索する (Visual Basic)

この記事では、Visual Basic 内の文字列内で検索する方法の例を示します。

## <a name="example"></a>使用例

この例では、<xref:System.String> オブジェクトの <xref:System.String.IndexOf%2A> メソッドを呼び出して、部分文字列が最初に見つかった位置のインデックスを報告します。

 [!code-vb[VbVbalrStrings#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#71)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

<xref:System.String.IndexOf%2A> メソッドは、最初に見つかった部分文字列の最初の文字の位置を返します。 インデックスは0から始まる値です。つまり、文字列の最初の文字のインデックスは0になります。

<xref:System.String.IndexOf%2A> が部分文字列を見つけられなかった場合は、-1 を返します。

<xref:System.String.IndexOf%2A> メソッドでは、大文字と小文字が区別され、現在のカルチャが使用されます。

最適なエラーに制御するために文字列の検索をする可能性があります、`Try`のブロックを[Try...Catch...Finally ステートメント](../../../language-reference/statements/try-catch-finally-statement.md)構築します。

## <a name="see-also"></a>関連項目

- <xref:System.String.IndexOf%2A>
- [Try...Catch...Finally ステートメント](../../../language-reference/statements/try-catch-finally-statement.md)
- [Visual Basic の文字列の概要](introduction-to-strings.md)
- [文字列](index.md)
