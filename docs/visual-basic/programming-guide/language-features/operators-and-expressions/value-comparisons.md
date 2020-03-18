---
title: 値の比較
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], comparing values
- Visual Basic code, operators
- Visual Basic code, expressions
- comparison operators [Visual Basic], comparing expressions
- numeric expressions
- operators [Visual Basic], comparison
- expressions [Visual Basic], comparing
ms.assetid: 60da0c76-9458-4afc-97e9-44a7939c064c
ms.openlocfilehash: f766eaaada486a0f70838bafb754d25070ff4174
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346286"
---
# <a name="value-comparisons-visual-basic"></a>値の比較 (Visual Basic)
比較演算子を使用すると、数値変数の値を比較する式を作成できます。 これらの式は、比較が true であるか false であるかに基づいて、`Boolean` 値を返します。 このような式の例を次に示します。  
  
 `45 > 26`  
  
 `26 > 45`  
  
 最初の式は `True`に評価されます。45が26を超えるためです。 2番目の例は `False`に評価されます。これは、26が45を超えていないためです。  
  
 この方法では、数値式を比較することもできます。 比較する式は、次の例のように、複雑な式にすることができます。  
  
 `x / 45 * (y +17) >= System.Math.Sqrt(z) / (p - (x * 16))`  
  
 前の複合式には、リテラル、変数、および関数呼び出しが含まれています。 比較演算子の両側の式が評価され、結果の値が `>=` 比較演算子を使用して比較されます。 左辺の式の値が、右側の式の値以上の場合、式全体が `True`に評価されますが、それ以外の場合は、`False`として評価されます。  
  
 値を比較する式は、次の例に示すように、`If...Then` の構造で最もよく使用されます。  
  
 [!code-vb[VbVbalrOperators#84](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#84)]  
  
 `=` 記号は、代入演算子でもあり、比較演算子でもあります。 次の例に示すように、比較演算子として使用すると、左側の値が右側の値と等しいかどうかが評価されます。  
  
 [!code-vb[VbVbalrOperators#85](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#85)]  
  
 また、`If`、`While`、`Loop`、または `ElseIf` のステートメントなどの `Boolean` 値が必要な場合や、`Boolean` 変数に値を割り当てたり渡したりする場合は、比較式を使用することもできます。 次の例では、比較式によって返される値が `Boolean` 変数に代入されます。  
  
 [!code-vb[VbVbalrOperators#86](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#86)]  
  
## <a name="see-also"></a>関連項目

- [ブール式](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)
- [演算子および式](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [Visual Basic の比較演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [方法 : 数値を計算する](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/how-to-calculate-numeric-values.md)
- [Visual Basic における演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)
