---
title: データ型
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], declaring
- typing
- data types [Visual Basic]
- Visual Basic code, data types
- data types [Visual Basic], improving speed with
ms.assetid: 5e1b9aaf-c7ca-4b29-9b22-0e82ed8e85e2
ms.openlocfilehash: 01529b36a68b8db6febb80b69a7bd81486a47087
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346349"
---
# <a name="data-types-in-visual-basic"></a>Visual Basic におけるデータ型
プログラミング要素の*データ型*は、保持できるデータの種類やデータの格納方法を示します。 データ型は、コンピューターのメモリに格納できるすべての値に適用され、式の評価にも関与します。 変数、リテラル、定数、列挙値、プロパティ、プロシージャのパラメーター、プロシージャの引数、およびプロシージャの戻り値にはすべてデータ型があります。  
  
## <a name="declared-data-types"></a>宣言済みデータ型  
 プログラミング要素は、宣言ステートメントで定義し、`As` 句を使用してデータ型を指定します。 次の表は、さまざまな要素の宣言に使用するステートメントを示しています。  
  
|プログラミング要素|データ型の宣言|  
|-------------------------|---------------------------|  
|変数|[Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)<br /><br /> `Dim`   `amount As Double`<br /><br /> `Static`   `yourName As String`<br /><br /> `Public`   `billsPaid As Decimal = 0`|  
|リテラル|リテラルの型文字で指定する場合は、[型文字](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)」の「リテラルの型文字」を参照してください<br /><br /> `Dim searchChar As Char = "."`  `C`|  
|定数|[Const ステートメント](../../../../visual-basic/language-reference/statements/const-statement.md)<br /><br /> `Const`   `modulus As Single = 4.17825F`|  
|列挙型|[Enum ステートメント](../../../../visual-basic/language-reference/statements/enum-statement.md)<br /><br /> `Public`   `Enum`   `colors`|  
|プロパティ|[Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)<br /><br /> `Property`   `region() As String`|  
|プロシージャ パラメーター|[Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)、[Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)、または[Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)<br /><br /> `Sub addSale(ByVal`   `amount`   `As Double)`|  
|プロシージャ引数|呼び出しコードの各引数は、すでに宣言されているプログラミング要素、または宣言されている要素を含む式です<br /><br /> `subString = Left(`  `inputString`  `,`   `5`  `)`|  
|プロシージャの戻り値|[Function ステートメント](../../../../visual-basic/language-reference/statements/function-statement.md)、または[Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)<br /><br /> `Function convert(ByVal b As Byte)`   `As String`|  
  
 Visual Basic のデータ型の一覧については、[データ型](../../../../visual-basic/language-reference/data-types/index.md)に関するページをご覧ください。  
  
## <a name="see-also"></a>参照

- [型文字](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [Visual Basic におけるジェネリック型](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [Visual Basic における型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [タプル](tuples.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [データ型の有効な使用方法](../../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
