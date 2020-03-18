---
title: Xor 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.Xor
helpviewer_keywords:
- exclusive OR operator [Visual Basic]
- logical exclusion
- operators [Visual Basic], exclusive or
- exclusion operator [Visual Basic]
- operators [Visual Basic], bitwise
- bitwise operators [Visual Basic], XOR operator
- Xor operator [Visual Basic]
- Xor keyword [Visual Basic]
- bitwise comparison [Visual Basic]
ms.assetid: 036000a9-3934-4e7f-a9d0-a816de3d84a6
ms.openlocfilehash: c5c7ec87bc173f724f8c670395bc3b0458444df6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74335395"
---
# <a name="xor-operator-visual-basic"></a>Xor 演算子 (Visual Basic)
2つの `Boolean` 式、または2つの数値式のビットごとの除外に対して、論理的な除外を実行します。  
  
## <a name="syntax"></a>構文  
  
```vb  
result = expression1 Xor expression2  
```  
  
## <a name="parts"></a>指定項目  
 `result`  
 必須。 任意の `Boolean` または数値変数。 ブール型の比較の場合、`result` は2つの `Boolean` 値の論理的な排他的論理和です。 ビットごとの演算の場合、`result` は、2つの数値ビットパターンのビットごとの排他的論理 (ビットごとの和) を表す数値です。  
  
 `expression1`  
 必須。 任意の `Boolean` または数値式。  
  
 `expression2`  
 必須。 任意の `Boolean` または数値式。  
  
## <a name="remarks"></a>コメント  
 ブール値の比較の場合、`result` は、`expression1` と `expression2` の1つだけが `True`に評価される場合にのみ `True` ます。 つまり、`expression1` と `expression2` が逆の `Boolean` 値に評価される場合に限ります。 次の表は、`result` がどのように決定されるかを示しています。  
  
|`expression1` の場合|`expression2`|`result` の値はです。|  
|-------------------------|--------------------------|------------------------------|  
|`True`|`True`|`False`|  
|`True`|`False`|`True`|  
|`False`|`True`|`True`|  
|`False`|`False`|`False`|  
  
> [!NOTE]
> ブール値の比較では、`Xor` 演算子は常に両方の式を評価します。これには、プロシージャ呼び出しを含めることができます。 `Xor`に対応するショートサーキットはありません。結果は常に両方のオペランドに依存しているためです。 *ショートサーキット*論理演算子については、「[AndAlso 演算子](../../../visual-basic/language-reference/operators/andalso-operator.md)と[OrElse 演算子](../../../visual-basic/language-reference/operators/orelse-operator.md)」を参照してください。  
  
 ビットごとの演算の場合、`Xor` 演算子は2つの数値式で同一の位置ビットのビットごとの比較を実行し、次の表に従って `result` に対応するビットを設定します。  
  
|`expression1` のビットがの場合|`expression2` のビットは|`result` のビットはです。|  
|--------------------------------|---------------------------------|----------------------------|  
|1|1|0|  
|1|0|1|  
|0|1|1|  
|0|0|0|  
  
> [!NOTE]
> 論理演算子とビット処理演算子は、他の算術演算子および関係演算子より優先順位が低いので、ビットごとの演算は、正確な実行を保証するためにかっこで囲む必要があります。  
  
 たとえば、5 `Xor` 3 は6です。 その理由を確認するには、5と3をバイナリ表現101および011に変換します。 次に、前の表を使用して、101 Xor 011 が110であることを確認します。これは、10進数6のバイナリ表現です。  
  
## <a name="data-types"></a>データ型  
 オペランドが1つの `Boolean` 式と1つの数値式で構成されている場合、Visual Basic は `Boolean` 式を数値に変換し (`True` の場合は-1、`False`の場合は 0)、ビットごとの演算を実行します。  
  
 `Boolean` の比較では、結果のデータ型は `Boolean`です。 ビットごとの比較の場合、結果のデータ型は `expression1` および `expression2`のデータ型に適した数値型になります。 「[演算子の結果のデータ型](../../../visual-basic/language-reference/operators/data-types-of-operator-results.md)」の「リレーショナルおよびビットごとの比較」の表を参照してください。  
  
## <a name="overloading"></a>オーバーロード  
 `Xor` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 コードでこのようなクラスまたは構造体に対してこの演算子を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`Xor` 演算子を使用して、2つの式に対して論理的な排他的論理和を実行します。 結果は、式の1つだけが `True`かどうかを示す `Boolean` 値です。  
  
 [!code-vb[VbVbalrOperators#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#40)]  
  
 前の例では、`False`、`True`、および `False`の結果がそれぞれ生成されます。  
  
## <a name="example"></a>例  
 次の例では、`Xor` 演算子を使用して、2つの数値式の個々のビットに対して論理的な除外 (排他的論理和) を実行します。 オペランドの対応するビットの1つだけが1に設定されている場合、結果パターンのビットは設定されます。  
  
 [!code-vb[VbVbalrOperators#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#41)]  
  
 前の例では、それぞれ2、12、14の結果が生成されます。  
  
## <a name="see-also"></a>参照

- [論理/ビット演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
