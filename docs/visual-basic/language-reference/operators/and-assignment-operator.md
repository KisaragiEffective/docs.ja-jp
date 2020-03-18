---
title: '&amp;= 演算子'
ms.date: 07/20/2015
f1_keywords:
- vb.&=
helpviewer_keywords:
- operator &=
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- '&= operator [Visual Basic]'
- compound assignment statements [Visual Basic]
ms.assetid: 0cf262fc-1a05-419a-a503-60013f111c8a
ms.openlocfilehash: 8668bfcbf32bb34b422efe8116bbd12a2d80b1d4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350260"
---
# <a name="amp-operator-visual-basic"></a>&amp;= 演算子 (Visual Basic)
`String` 式を `String` 変数またはプロパティに連結し、その結果を変数またはプロパティに代入します。  
  
## <a name="syntax"></a>構文  
  
```vb  
variableorproperty &= expression  
```  
  
## <a name="parts"></a>指定項目  
 `variableorproperty`  
 必須。 任意の `String` 変数またはプロパティ。  
  
 `expression`  
 必須。 任意のブール型 ( `String` ) の式を指定します。  
  
## <a name="remarks"></a>コメント  
 `&=` 演算子の左側の要素は、単純なスカラー変数、プロパティ、または配列の要素にすることができます。 変数またはプロパティを[ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)にすることはできません。 `&=` 演算子は、右側にある `String` 式を左側の `String` 変数またはプロパティに連結し、その結果を左側の変数またはプロパティに代入します。  
  
## <a name="overloading"></a>オーバーロード  
 [& 演算子](../../../visual-basic/language-reference/operators/concatenation-operator.md)は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 `&` 演算子のオーバーロードは、`&=` 演算子の動作に影響します。 コードで `&`をオーバーロードするクラスまたは構造体の `&=` を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`&=` 演算子を使用して2つの `String` 変数を連結し、その結果を最初の変数に代入します。  
  
 [!code-vb[VbVbalrOperators#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>関連項目

- [& 演算子](../../../visual-basic/language-reference/operators/concatenation-operator.md)
- [+= 演算子](../../../visual-basic/language-reference/operators/addition-assignment-operator.md)
- [代入演算子](../../../visual-basic/language-reference/operators/assignment-operators.md)
- [連結演算子](../../../visual-basic/language-reference/operators/concatenation-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
