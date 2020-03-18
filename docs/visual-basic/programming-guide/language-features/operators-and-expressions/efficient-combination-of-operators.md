---
title: 演算子の効率のよい組み合わせ
ms.date: 07/20/2015
helpviewer_keywords:
- expressions [Visual Basic], parentheses
- operators [Visual Basic], associativity
- expressions [Visual Basic], operators
- operators [Visual Basic], precedence
- Visual Basic code, operators
- Visual Basic code, expressions
- operators [Visual Basic], complex expressions
- expressions [Visual Basic], complex
- parentheses [Visual Basic], complex expressions
- numeric expressions
ms.assetid: bd22340e-b5be-458b-8772-3916c02309a4
ms.openlocfilehash: 83ad53e4c75490a75eba0f80a6bf0f078aa4d426
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348996"
---
# <a name="efficient-combination-of-operators-visual-basic"></a>演算子の効率のよい組み合わせ (Visual Basic)
複合式には、さまざまな演算子を含めることができます。 次に例を示します。  
  
 `x = (45 * (y + z)) ^ (2 / 85) * 5 + z`  
  
 前の例のような複雑な式を作成するには、演算子の優先順位の規則を十分に理解している必要があります。 詳細については、「 [Visual Basic での演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)」を参照してください。  
  
## <a name="parenthetical-expressions"></a>かっこで囲まれる式  
 多くの場合、演算子の優先順位によって決定される順序とは異なる順序で操作を行う必要があります。 例を次に示します。  
  
 `x = z * y + 4`  
  
 前の例では、`z` を `y`で乗算し、その結果を `4`に追加します。 ただし、`z`によって結果を乗算する前に `y` と `4` を追加する場合は、かっこを使用して通常の演算子の優先順位をオーバーライドできます。 式をかっこで囲むことで、演算子の優先順位に関係なく、最初に式を評価するように強制します。 前の例を強制的に追加するには、次の例のように書き直します。  
  
 `x = z * (y + 4)`  
  
 前の例では、`y` と `4`を追加し、その合計を `z`で乗算しています。  
  
### <a name="nested-parenthetical-expressions"></a>かっこで囲まれた入れ子式  
 式を複数のレベルのかっこで入れ子にして、優先順位をさらにオーバーライドすることができます。 かっこで最も深く入れ子になっている式は、最初に評価され、次に最も深い入れ子になっています。さらに、最も深い入れ子になっていない式になります。 次に例を示します。  
  
 `x = (z * 4) ^ (y * (z + 2))`  
  
 前の例では、最初に `z + 2` が評価され、その後、その他のかっこで囲まれた式が評価されます。 通常、加算または乗算よりも高い優先順位を持つ指数演算は、この例の最後に評価されます。これは、他の式がかっこで囲まれているためです。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic の算術演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Visual Basic の比較演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
- [論理/ビット演算子 (Visual Basic)](../../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [ブール式](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)
- [値の比較](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/value-comparisons.md)
- [方法 : 数値を計算する](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/how-to-calculate-numeric-values.md)
- [Visual Basic における演算子の優先順位](../../../../visual-basic/language-reference/operators/operator-precedence.md)
