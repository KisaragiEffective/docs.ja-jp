---
title: '方法 : 変換演算子を定義する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: 54203dfa-c24b-463f-9942-d5153e89e762
ms.openlocfilehash: 0ff95390206947e5a28f7a5b85547b496746a9cc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344901"
---
# <a name="how-to-define-a-conversion-operator-visual-basic"></a>方法: 変換演算子を定義する (Visual Basic)
クラスまたは構造体を定義している場合は、クラスまたは構造体の型と、別のデータ型 (`Integer`、`Double`、`String`など) の間の型変換演算子を定義できます。  
  
 クラスまたは構造体内で、型変換を [CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)プロシージャとして定義します。 すべての変換プロシージャは `Public Shared`である必要があり、それぞれが[拡大](../../../../visual-basic/language-reference/modifiers/widening.md)または[縮小](../../../../visual-basic/language-reference/modifiers/narrowing.md)のどちらかを指定する必要があります。  
  
 クラスまたは構造体に対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。  
  
## <a name="example"></a>例  
 次の例では、`digit` という構造体と `Byte`の間の変換演算子を定義します。  
  
 [!code-vb[VbVbcnProcedures#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#27)]  
  
 構造 `digit` をテストするには、次のコードを使用します。  
  
 [!code-vb[VbVbcnProcedures#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>参照

- [演算子プロシージャ](./operator-procedures.md)
- [方法 : 演算子を定義する](./how-to-define-an-operator.md)
- [方法 : 演算子プロシージャを呼び出す](./how-to-call-an-operator-procedure.md)
- [方法: 演算子を定義するクラスを使用する](./how-to-use-a-class-that-defines-operators.md)
- [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [暗黙の型変換と明示的な型変換](../../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
