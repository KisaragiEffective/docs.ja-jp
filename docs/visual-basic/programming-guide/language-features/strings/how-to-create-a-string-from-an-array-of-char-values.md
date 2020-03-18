---
title: '方法: Char 値の配列から文字列を作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- examples [Visual Basic], arrays
- examples [Visual Basic], Char data type
ms.assetid: 69f94e85-d57c-4ccc-a62a-426e829f5c5e
ms.openlocfilehash: bf37ceba901e712df10ad4b39f9ad74194843136
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346770"
---
# <a name="how-to-create-a-string-from-an-array-of-char-values-visual-basic"></a>方法: Char 値の配列から文字列を作成する (Visual Basic)
この例では、個々の文字から文字列 "abcd" を作成します。  
  
## <a name="example"></a>使用例  
 [!code-vb[VbVbalrStrings#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#61)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 このメソッドには特別な要件はありません。  
  
 `"a"c`構文では、1つの `c` が引用符で囲まれた単一の文字に続く場合、文字リテラルの作成に使用されます。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 文字列の Null 文字 (`Chr(0)`に相当) は、文字列を使用すると予期しない結果になります。 Null 文字は文字列に含まれますが、一部の状況では、null 文字の後の文字は表示されません。  
  
## <a name="see-also"></a>関連項目

- <xref:System.String>
- [Char データ型](../../../../visual-basic/language-reference/data-types/char-data-type.md)
- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
