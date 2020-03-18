---
title: And 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.And
helpviewer_keywords:
- operators [Visual Basic], bitwise
- logical conjunction
- bitwise AND operator [Visual Basic]
- conjunction operator [Visual Basic]
- And operator [Visual Basic]
- bitwise operators [Visual Basic], AND operator
- operators [Visual Basic], conjunction
- bitwise comparison [Visual Basic]
ms.assetid: 2ea711f3-439a-4c7c-9e3a-1ffe3b0d6046
ms.openlocfilehash: 78a65843a449bd15d5615710e1685f40d94c37f7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350254"
---
# <a name="and-operator-visual-basic"></a>And 演算子 (Visual Basic)
2つの `Boolean` 式、または2つの数値式のビットごとの組み合わせに対して論理積を実行します。  
  
## <a name="syntax"></a>構文  
  
```vb  
result = expression1 And expression2  
```  
  
## <a name="parts"></a>指定項目  
 `result`  
 必須。 任意の `Boolean` または数値式。 ブール型の比較の場合、`result` は2つの `Boolean` 値の論理積となります。 ビットごとの演算の場合、`result` は2つの数値ビットパターンのビットごとの組み合わせを表す数値です。  
  
 `expression1`  
 必須。 任意の `Boolean` または数値式。  
  
 `expression2`  
 必須。 任意の `Boolean` または数値式。  
  
## <a name="remarks"></a>コメント  
 ブール値の比較では、`expression1` と `expression2` の両方が `True`に評価される場合にのみ、`result` が `True` ます。 次の表は、`result` がどのように決定されるかを示しています。  
  
|`expression1` の場合|`expression2`|`result` の値はです。|  
|-------------------------|--------------------------|------------------------------|  
|`True`|`True`|`True`|  
|`True`|`False`|`False`|  
|`False`|`True`|`False`|  
|`False`|`False`|`False`|  
  
> [!NOTE]
> ブール値の比較では、`And` 演算子は常に両方の式を評価します。これには、プロシージャ呼び出しを含めることができます。 [AndAlso 演算子](../../../visual-basic/language-reference/operators/andalso-operator.md)は*ショートサーキット*を実行します。つまり、`expression1` が `False`場合、`expression2` は評価されません。  
  
 `And` 演算子は、数値に適用した場合、2つの数値式で同じ位置にあるビットのビットごとの比較を実行し、次の表に従って `result` に対応するビットを設定します。  
  
|`expression1` のビット|`expression2` のビット|`result` のビット|  
|--------------------------------|---------------------------------|----------------------------|  
|1|1|1|  
|1|0|0|  
|0|1|0|  
|0|0|0|  
  
> [!NOTE]
> 論理演算子とビット処理演算子は、他の算術演算子および関係演算子より優先順位が低いので、ビットごとの演算は、正確な結果を得るためにかっこで囲む必要があります。  
  
## <a name="data-types"></a>データ型  
 オペランドが1つの `Boolean` 式と1つの数値式で構成されている場合、Visual Basic は `Boolean` 式を数値に変換し (`True` の場合は-1、`False`の場合は 0)、ビットごとの演算を実行します。  
  
 ブール値の比較の場合、結果のデータ型は `Boolean`になります。 ビットごとの比較の場合、結果のデータ型は `expression1` および `expression2`のデータ型に適した数値型になります。 「[演算子の結果のデータ型](../../../visual-basic/language-reference/operators/data-types-of-operator-results.md)」の「リレーショナルおよびビットごとの比較」の表を参照してください。  
  
> [!NOTE]
> `And` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 コードでこのようなクラスまたは構造体に対してこの演算子を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`And` 演算子を使用して、2つの式の論理積を実行します。 結果は、両方の式が `True`かどうかを表す `Boolean` 値です。  
  
 [!code-vb[VbVbalrOperators#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#22)]  
  
 前の例では、`True` と `False`の結果がそれぞれ生成されます。  
  
## <a name="example"></a>例  
 次の例では、`And` 演算子を使用して、2つの数値式の個々のビットに対して論理積演算を実行します。 オペランドの対応するビットが両方とも1に設定されている場合、結果パターンのビットは設定されます。  
  
 [!code-vb[VbVbalrOperators#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#23)]  
  
 前の例では、それぞれ8、2、および0の結果が生成されます。  
  
## <a name="see-also"></a>関連項目

- [論理/ビット演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [AndAlso 演算子](../../../visual-basic/language-reference/operators/andalso-operator.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
