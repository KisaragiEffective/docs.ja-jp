---
title: = 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.Assign
- vb.=
helpviewer_keywords:
- = operator [Visual Basic]
- = assignment statements [Visual Basic]
ms.assetid: 2dac2e49-86c8-42f8-80c1-458452fb5e29
ms.openlocfilehash: 75f303219b9bf32613989f65f90a9096ef70e02e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350202"
---
# <a name="-operator-visual-basic"></a>= 演算子 (Visual Basic)
変数またはプロパティに値を割り当てます。  
  
## <a name="syntax"></a>構文  
  
```vb  
variableorproperty = value  
```  
  
## <a name="parts"></a>指定項目  
 `variableorproperty`  
 任意の書き込み可能な変数または任意のプロパティ。  
  
 `value`  
 任意のリテラル、定数、または式。  
  
## <a name="remarks"></a>コメント  
 等号 (`=`) の左側の要素は、単純なスカラー変数、プロパティ、または配列の要素にすることができます。 変数またはプロパティを[ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)にすることはできません。 `=` 演算子は、右辺の値を左側の変数またはプロパティに代入します。  
  
> [!NOTE]
> `=` 演算子は、比較演算子としても使用されます。 詳細については、「[比較演算子](../../../visual-basic/language-reference/operators/comparison-operators.md)」を参照してください。  
  
## <a name="overloading"></a>オーバーロード  
 `=` 演算子は、代入演算子としてではなく、関係比較演算子としてのみオーバーロードできます。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例は、代入演算子を示しています。 右側の値は、左側の変数に割り当てられます。  
  
 [!code-vb[VbVbalrOperators#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>参照

- [& = 演算子](../../../visual-basic/language-reference/operators/and-assignment-operator.md)
- [*= 演算子](../../../visual-basic/language-reference/operators/multiplication-assignment-operator.md)
- [+= 演算子](../../../visual-basic/language-reference/operators/addition-assignment-operator.md)
- [-= 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/subtraction-assignment-operator.md)
- [/= 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/floating-point-division-assignment-operator.md)
- [\\= 演算子](../../../visual-basic/language-reference/operators/integer-division-assignment-operator.md)
- [^= 演算子](../../../visual-basic/language-reference/operators/exponentiation-assignment-operator.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
- [比較演算子](../../../visual-basic/language-reference/operators/comparison-operators.md)
- [ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)
- [ローカル型の推論](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
