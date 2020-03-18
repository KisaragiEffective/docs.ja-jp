---
title: OrElse 演算子
ms.date: 07/20/2015
f1_keywords:
- OrElse
- vb.OrElse
helpviewer_keywords:
- short-circuiting
- operators [Visual Basic], short-circuiting
- operators [Visual Basic], disjunction
- short-circuit evaluation
- OrElse operator [Visual Basic]
ms.assetid: 253803d8-05b0-47d7-b213-abd222847779
ms.openlocfilehash: 361de44711c3b41411f2fa1dd81a3dd8db6b01e6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348242"
---
# <a name="orelse-operator-visual-basic"></a>OrElse 演算子 (Visual Basic)
2つの式のショートサーキット包括論理和を実行します。  
  
## <a name="syntax"></a>構文  
  
```vb
result = expression1 OrElse expression2  
```  
  
## <a name="parts"></a>指定項目  
 `result`  
 必須。 任意のブール型 ( `Boolean` ) の式を指定します。  
  
 `expression1`  
 必須。 任意のブール型 ( `Boolean` ) の式を指定します。  
  
 `expression2`  
 必須。 任意のブール型 ( `Boolean` ) の式を指定します。  
  
## <a name="remarks"></a>コメント  
 コンパイルされたコードが別の式の結果に応じて1つの式の評価をバイパスできる場合、論理操作は*ショートサーキット*と呼ばれます。 最初に評価された式の結果が演算の最終結果を決定した場合、最終的な結果を変更できないため、2番目の式を評価する必要はありません。 ショートサーキットは、バイパスされた式が複雑な場合や、プロシージャ呼び出しが関係している場合に、パフォーマンスを向上させることができます。  
  
 いずれかまたは両方の式が `True`に評価される場合、`result` は `True`です。 次の表は、`result` がどのように決定されるかを示しています。  
  
|`expression1` の場合|`expression2`|`result` の値はです。|  
|-------------------------|--------------------------|------------------------------|  
|`True`|(評価なし)|`True`|  
|`False`|`True`|`True`|  
|`False`|`False`|`False`|  
  
## <a name="data-types"></a>データ型  
 `OrElse` 演算子は、[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)に対してのみ定義されます。 Visual Basic は、式を評価する前に、必要に応じて各オペランドを `Boolean` に変換します。 結果を数値型に代入すると、Visual Basic によって `Boolean` からその型に変換され、`False` が `0` になり、`True` が `-1`になります。
詳細については、「[ブール型変換](../data-types/boolean-data-type.md#type-conversions)」を参照してください。
  
## <a name="overloading"></a>オーバーロード  
 [Or 演算子](../../../visual-basic/language-reference/operators/or-operator.md)と[IsTrue 演算子](../../../visual-basic/language-reference/operators/istrue-operator.md)を*オーバーロード*することができます。つまり、クラスまたは構造体が、そのクラスまたは構造体の型を持つ場合に、その動作を再定義できます。 `Or` 演算子と `IsTrue` 演算子のオーバーロードは、`OrElse` 演算子の動作に影響します。 コードで `Or` と `IsTrue`をオーバーロードするクラスまたは構造体の `OrElse` を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「 [Operator Procedures](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`OrElse` 演算子を使用して、2つの式の論理和を実行します。 結果は、2つの式のいずれかが true であるかどうかを示す `Boolean` 値になります。 最初の式が `True`場合、2番目の式は評価されません。  
  
 [!code-vb[VbVbalrOperators#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#37)]  
  
 前の例では、`True`、`True`、および `False` の結果がそれぞれ生成されます。 `firstCheck`の計算では、最初の式が既に `True`ているため、2番目の式は評価されません。 ただし、2番目の式は `secondCheck`の計算で評価されます。  
  
## <a name="example"></a>例  
 次の例は、2つのプロシージャ呼び出しを含む `If`...`Then` ステートメントを示しています。 最初の呼び出しで `True`が返された場合、2番目のプロシージャは呼び出されません。 この場合、2番目のプロシージャが、コードのこのセクションの実行時に常に実行する必要がある重要なタスクを実行すると、予期しない結果が生じる可能性があります。  
  
 [!code-vb[VbVbalrOperators#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#38)]  
  
## <a name="see-also"></a>関連項目

- [論理/ビット演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Or 演算子](../../../visual-basic/language-reference/operators/or-operator.md)
- [IsTrue 演算子](../../../visual-basic/language-reference/operators/istrue-operator.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
