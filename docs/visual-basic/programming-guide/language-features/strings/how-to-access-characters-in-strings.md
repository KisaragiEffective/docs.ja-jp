---
title: '方法 : 文字列の文字にアクセスする'
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], accessing characters
- characters [Visual Basic], accessing in strings
ms.assetid: 02c5206c-ffab-494d-b648-3b2ea358dc34
ms.openlocfilehash: 44a021ed3ce1d10613cf6ab7c959c62feec6046c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352456"
---
# <a name="how-to-access-characters-in-strings-in-visual-basic"></a>方法 : Visual Basic で文字列の文字にアクセスする
この例では、<xref:System.String.Chars%2A> プロパティを使用して、文字列内の指定した位置にある文字にアクセスする方法を示します。  
  
## <a name="example"></a>例  
 文字列内の文字に関するデータと文字列内の文字の位置を取得すると便利な場合があります。 文字列は、文字の配列 (`Char` インスタンス) と考えることができます。特定の文字を取得するには、<xref:System.String.Chars%2A> プロパティを使用してその文字のインデックスを参照します。  
  
 [!code-vb[VbVbalrStrings#49](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#49)]  
  
 <xref:System.String.Chars%2A> プロパティの `index` パラメーターは0から始まります。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 <xref:System.String.Chars%2A> プロパティは、指定された位置にある文字を返します。 ただし、一部の Unicode 文字は複数の文字で表すことができます。 Unicode 文字の使用方法の詳細については、「[方法: 文字列を文字配列に変換](../../../../visual-basic/programming-guide/language-features/strings/how-to-convert-a-string-to-an-array-of-characters.md)する」を参照してください。  
  
 `index` パラメーターが文字列の長さ以上である場合、または0未満の場合、<xref:System.String.Chars%2A> プロパティは <xref:System.IndexOutOfRangeException> の例外をスローします。  
  
## <a name="see-also"></a>関連項目

- <xref:System.String.Chars%2A>
- [方法 : 文字列を文字の配列に変換する](../../../../visual-basic/programming-guide/language-features/strings/how-to-convert-a-string-to-an-array-of-characters.md)
- [Visual Basic で、文字列型とその他のデータ型との変換を行う](../../../../visual-basic/programming-guide/language-features/strings/converting-between-strings-and-other-data-types.md)
- [文字列](../../../../visual-basic/programming-guide/language-features/strings/index.md)
