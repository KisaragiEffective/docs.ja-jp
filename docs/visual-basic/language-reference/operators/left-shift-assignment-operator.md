---
title: <<= 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.<<=
helpviewer_keywords:
- operator <<=
- assignment statements [Visual Basic], compound
- <<= operator [Visual Basic]
- statements [Visual Basic], compound assignment
- operator<<=
- compound assignment statements [Visual Basic]
ms.assetid: 8ad26613-faff-4e2f-89ee-63feee33bfda
ms.openlocfilehash: cc89e0dadc7148b21e695a53a2e746a00ed66441
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350956"
---
# <a name="-operator-visual-basic"></a>\<\<= 演算子 (Visual Basic)
変数またはプロパティの値に対して算術左シフトを実行し、結果を変数またはプロパティに代入します。  
  
## <a name="syntax"></a>構文  
  
```vb  
variableorproperty <<= amount  
```  
  
## <a name="parts"></a>指定項目  
 `variableorproperty`  
 必須。 整数型の変数またはプロパティ (`SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`)。  
  
 `amount`  
 必須。 `Integer`に拡大変換されるデータ型の数値式。  
  
## <a name="remarks"></a>コメント  
 `<<=` 演算子の左側の要素は、単純なスカラー変数、プロパティ、または配列の要素にすることができます。 変数またはプロパティを [ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md) にすることはできません。  
  
 `<<=` 演算子は、まず変数またはプロパティの値に対して算術左シフトを実行します。 次に、演算子は、その操作の結果をその変数またはプロパティに戻します。  
  
 算術シフトは循環していません。つまり、結果の一方の端からシフトされたビットはもう一方の端には再入されません。 算術左シフトでは、結果のデータ型の範囲を超えてシフトされたビットは破棄され、右側に空いているビット位置は0に設定されます。  
  
## <a name="overloading"></a>オーバーロード  
 [< < 演算子](../../../visual-basic/language-reference/operators/left-shift-operator.md)は*オーバーロード*できます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 `<<` 演算子のオーバーロードは、`<<=` 演算子の動作に影響します。 コードで `<<`をオーバーロードするクラスまたは構造体の `<<=` を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`<<=` 演算子を使用して、`Integer` 変数のビットパターンを指定された量だけ左にシフトし、その結果を変数に代入します。  
  
 [!code-vb[VbVbalrOperators#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#13)]  
  
## <a name="see-also"></a>参照

- [<< 演算子](../../../visual-basic/language-reference/operators/left-shift-operator.md)
- [代入演算子](../../../visual-basic/language-reference/operators/assignment-operators.md)
- [ビット シフト演算子](../../../visual-basic/language-reference/operators/bit-shift-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
