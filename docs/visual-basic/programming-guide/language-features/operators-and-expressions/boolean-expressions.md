---
title: Boolean 式
ms.date: 07/20/2015
helpviewer_keywords:
- short-circuiting
- Boolean expressions
- logical operators [Visual Basic], Boolean expressions
- expressions [Visual Basic], Boolean
- expression evaluation [Visual Basic], Boolean expressions
- operators [Visual Basic], short-circuiting
- Visual Basic code, operators
- short-circuit evaluation
- logical operators [Visual Basic], short-circuiting
- operators [Visual Basic], Boolean
- Visual Basic code, expressions
ms.assetid: d3d90406-55c8-4404-8143-50fd7f0d0d1a
ms.openlocfilehash: 1000ec6e4b35d0cb2c6232b50f9a9551cb0dfdcd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350810"
---
# <a name="boolean-expressions-visual-basic"></a>Boolean 式 (Visual Basic)
*ブール式*は、`True` または `False`[Boolean データ型](../../../../visual-basic/language-reference/data-types/boolean-data-type.md)の値に評価される式です。 `Boolean` 式には、複数の形式を使用できます。 最も単純なのは、次の例に示すように、`Boolean` 変数の値を `Boolean` リテラルに直接比較することです。  
  
 [!code-vb[VbVbalrOperators#87](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#87)]  
  
## <a name="two-meanings-of-the--operator"></a>= 演算子の2つの意味  
 代入ステートメント `newCustomer = True` は、前の例の式と同じように見えますが、別の関数を実行し、異なる方法で使用します。 前の例では、式 `newCustomer = True` はブール値を表し、`=` 符号は比較演算子として解釈されます。 スタンドアロンのステートメントでは、`=` 記号は代入演算子として解釈され、右側の値が左側の変数に代入されます。 これを次の例に示します。  
  
 [!code-vb[VbVbalrOperators#88](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#88)]  
  
 詳細については、「[値の比較](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)と[ステートメント](../../../../visual-basic/language-reference/statements/index.md)」を参照してください。  
  
## <a name="comparison-operators"></a>比較演算子  
 `=`、`<`、`>`、`<>`、`<=`、`>=` などの比較演算子は、演算子の左辺の式を演算子の右辺の式と比較し、結果を `True` または `False`として評価することによって、ブール式を生成します。 これを次の例に示します。  
  
 `42 < 81`  
  
 42は81未満であるため、前の例のブール式は `True`に評価されます。 この種類の式の詳細については、「[値の比較](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)」を参照してください。  
  
### <a name="comparison-operators-combined-with-logical-operators"></a>論理演算子と組み合わせた比較演算子  
 論理演算子を使用して比較式を組み合わせると、より複雑なブール式を生成できます。 次の例では、論理演算子と共に比較演算子を使用する方法を示します。  
  
 `x > y And x < 1000`  
  
 前の例では、式全体の値は、`And` 演算子の各辺の式の値によって決まります。 両方の式が `True`場合、全体の式が `True`に評価されます。 いずれかの式が `False`場合、式全体が `False`に評価されます。  
  
## <a name="short-circuiting-operators"></a>ショートサーキット演算子  
 論理演算子 `AndAlso` と `OrElse`*ショートサーキット*と呼ばれる動作を示すことができます。 ショートサーキット演算子は、左オペランドを最初に評価します。 左側のオペランドで式全体の値が決定された場合、プログラムの実行は正しい式を評価せずに続行されます。 これを次の例に示します。  
  
 [!code-vb[VbVbalrOperators#89](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#89)]  
  
 前の例では、演算子は左の式の `45 < 12`を評価します。 左の式は `False`と評価されるため、論理式全体が `False`に評価される必要があります。 このため、プログラムを実行すると、正しい式 `testFunction(3)`を評価せずに `If` ブロック内のコードの実行がスキップされます。 この例では `testFunction()` は呼び出されません。左の式が式全体を falsifies ためです。  
  
 同様に、`OrElse` を使用する論理式の左式が `True`に評価される場合、左の式では式全体が検証済みであるため、右の式を評価せずに次のコード行に実行が進みます。  
  
### <a name="comparison-with-non-short-circuiting-operators"></a>非ショートサーキット演算子との比較  
 一方、論理演算子の両側は、論理演算子 `And` と `Or` が使用されるときに評価されます。 これを次の例に示します。  
  
 [!code-vb[VbVbalrOperators#90](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#90)]  
  
 前の例では、左の式が `False`に評価された場合でも `testFunction()` が呼び出されます。  
  
## <a name="parenthetical-expressions"></a>かっこで囲まれる式  
 かっこを使用して、ブール式の評価順序を制御できます。 かっこで囲まれた式は最初に評価されます。 入れ子のレベルが複数ある場合は、最も深い入れ子になった式に優先順位が与えられます。 かっこ内で、評価は演算子の優先順位の規則に従って行われます。 詳細については、「 [Visual Basic での演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Visual Basic の論理演算子とビット処理演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
- [値の比較](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)
- [ステートメント](../../../../visual-basic/programming-guide/language-features/statements.md)
- [比較演算子](../../../../visual-basic/language-reference/operators/comparison-operators.md)
- [演算子の効率のよい組み合わせ](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/efficient-combination-of-operators.md)
- [Visual Basic における演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)
- [Boolean データ型](../../../../visual-basic/language-reference/data-types/boolean-data-type.md)
