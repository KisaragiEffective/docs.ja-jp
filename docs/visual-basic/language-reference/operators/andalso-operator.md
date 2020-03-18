---
title: AndAlso 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.AndAlso
- AndAlso
helpviewer_keywords:
- short-circuiting
- AndAlso operator [Visual Basic]
- operators [Visual Basic], short-circuiting
- operators [Visual Basic], conjunction
- short-circuit evaluation
ms.assetid: bbc15191-b374-495b-9b8f-7b8c2f4388eb
ms.openlocfilehash: b3801c7e05142e1bc793e3c9d49a6f6991756f9d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350236"
---
# <a name="andalso-operator-visual-basic"></a>AndAlso 演算子 (Visual Basic)
2つの式の短絡論理積を実行します。  
  
## <a name="syntax"></a>構文  
  
```vb
result = expression1 AndAlso expression2  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`result`|必須。 任意のブール型 ( `Boolean` ) の式を指定します。 結果は、2つの式を比較した結果 `Boolean` になります。|  
|`expression1`|必須。 任意のブール型 ( `Boolean` ) の式を指定します。|  
|`expression2`|必須。 任意のブール型 ( `Boolean` ) の式を指定します。|  
  
## <a name="remarks"></a>コメント  
 コンパイルされたコードが別の式の結果に応じて1つの式の評価をバイパスできる場合、論理操作は*ショートサーキット*と呼ばれます。 最初に評価された式の結果が演算の最終結果を決定した場合、最終的な結果を変更できないため、2番目の式を評価する必要はありません。 ショートサーキットは、バイパスされた式が複雑な場合や、プロシージャ呼び出しが関係している場合に、パフォーマンスを向上させることができます。  
  
 両方の式が `True`に評価される場合、`result` は `True`ます。 次の表は、`result` がどのように決定されるかを示しています。  
  
|`expression1` の場合|`expression2`|`result` の値はです。|  
|---|---|---|  
|`True`|`True`|`True`|  
|`True`|`False`|`False`|  
|`False`|(評価なし)|`False`|  
  
## <a name="data-types"></a>データ型  
 `AndAlso` 演算子は、[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)に対してのみ定義されます。Visual Basic は、式を評価する前に、必要に応じて各オペランドを `Boolean` に変換します。結果を数値型に代入すると、Visual Basic によって `Boolean` からその型に変換され、`False` が `0` になり、`True` が `-1`になります。
詳細については、「[Boolean データ型](../data-types/boolean-data-type.md#type-conversions)」を参照してください。
  
## <a name="overloading"></a>オーバーロード  
 [And 演算子](../../../visual-basic/language-reference/operators/and-operator.md)と [IsFalse 演算子](../../../visual-basic/language-reference/operators/isfalse-operator.md)は*オーバーロード*することができます。つまり、クラスまたは構造体が、そのクラスまたは構造体の型を持つ場合に、その動作を再定義できます。`And` 演算子と `IsFalse` 演算子のオーバーロードは、`AndAlso` 演算子の動作に影響します。コードで `And` と `IsFalse`をオーバーロードするクラスまたは構造体の `AndAlso` を使用する場合は、再定義された動作を理解していることを確認してください。詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  

  
## <a name="example"></a>例  
 次の例では、`AndAlso` 演算子を使用して、2つの式の論理積を実行します。 結果は、conjoined 式全体が true であるかどうかを示す `Boolean` 値です。 最初の式が `False`場合、2番目の式は評価されません。  
  
 [!code-vb[VbVbalrOperators#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#24)]  
  
 前の例では、`True`、`False`、および `False`の結果がそれぞれ生成されます。 `secondCheck`の計算では、最初の式が既に `False`ているため、2番目の式は評価されません。 ただし、2番目の式は `thirdCheck`の計算で評価されます。  
  
## <a name="example"></a>例  
 次の例は、配列の要素の中から特定の値を検索する `Function` プロシージャを示しています。 配列が空の場合、または配列の長さを超えた場合、`While` のステートメントでは、検索値に対して配列要素がテストされません。  
  
 [!code-vb[VbVbalrOperators#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#25)]  
  
## <a name="see-also"></a>参照

- [論理/ビット演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [And 演算子](../../../visual-basic/language-reference/operators/and-operator.md)
- [IsFalse 演算子](../../../visual-basic/language-reference/operators/isfalse-operator.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
