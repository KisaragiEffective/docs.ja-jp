---
title: '方法 : 数値を計算する'
ms.date: 07/20/2015
helpviewer_keywords:
- operator precedence
- operators [Visual Basic]
- expressions [Visual Basic], numeric
- calculations [Visual Basic], numeric expressions
- precedence [Visual Basic], of operators
- Visual Basic code, operators
- Visual Basic code, expressions
- numeric expressions
ms.assetid: ba6bf43d-bd96-49b8-b1de-4a7797551372
ms.openlocfilehash: d213f6b5a4abf8c52d8872ae36e89796183ff27c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348969"
---
# <a name="how-to-calculate-numeric-values-visual-basic"></a>方法: 数値を計算する (Visual Basic)
数値式を使用して数値を計算できます。 *数値式*は、リテラル、定数、および数値を表す変数、およびそれらの値に対して作用する演算子を含む式です。  
  
## <a name="calculating-numeric-values"></a>数値の計算  
  
#### <a name="to-calculate-a-numeric-value"></a>数値を計算するには  
  
- 1つ以上の数値リテラル、定数、および変数を数値式に結合します。 有効な数値式の例を次に示します。  
  
     `93.217`  
  
     `System.Math.PI`  
  
     `counter`  
  
     `4 * (67 + i)`  
  
     最初の3行は、リテラル、定数、および変数を示しています。 それぞれが有効な数値式を形成します。 最後の行は、2つのリテラルを持つ変数の組み合わせを示しています。  
  
     数値式では、完全な Visual Basic ステートメントが単独で形成されるわけではないことに注意してください。 式は、完全なステートメントの一部として使用する必要があります。  
  
#### <a name="to-store-a-numeric-value"></a>数値を格納するには  
  
- 次の例に示すように、代入ステートメントを使用して、数値式で表される値を変数に割り当てることができます。  
  
     [!code-vb[VbVbalrOperators#82](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#82)]  
  
     前の例では、等号演算子 (`=`) の右側にある式の値が演算子の左辺の `j` 変数に代入されているので、`j` は276に評価されます。  
  
     詳細については、「[ステートメント](../../../../visual-basic/language-reference/statements/index.md)」を参照してください。  
  
## <a name="multiple-operators"></a>複数の演算子  
 数値式に複数の演算子が含まれている場合、評価される順序は、演算子の優先順位の規則によって決まります。 演算子の優先順位の規則をオーバーライドするには、上の例のように、式をかっこで囲みます。囲まれた式は、最初に評価されます。  
  
#### <a name="to-override-normal-operator-precedence"></a>通常の演算子の優先順位をオーバーライドするには  
  
- 最初に実行する操作を囲むには、かっこを使用します。 次の例は、同じオペランドと演算子を持つ2つの異なる結果を示しています。  
  
     [!code-vb[VbVbalrOperators#83](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#83)]  
  
     前の例では、`j` の計算で加算演算子 (`+`) が最初に実行されます。これは、の `(67 + i)` 前後のかっこによって通常の優先順位がオーバーライドされ、`j` に割り当てられた値が 276 (4 倍 69) になるためです。 `k` の計算では、演算子を通常の優先順位 (`+`前の`*`) で実行し、`k` に割り当てられた値は 270 (268 + 2) になります。  
  
     詳細については、「 [Visual Basic での演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [演算子および式](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [値の比較](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)
- [ステートメント](../../../../visual-basic/language-reference/statements/index.md)
- [Visual Basic における演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)
- [算術演算子](../../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [演算子の効率のよい組み合わせ](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/efficient-combination-of-operators.md)
