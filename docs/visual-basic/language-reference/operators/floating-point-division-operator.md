---
title: / 演算子
ms.date: 07/20/2015
f1_keywords:
- vb./
helpviewer_keywords:
- division operator [Visual Basic], floating point
- floating-point numbers [Visual Basic], division operator
- slash (/) operator
- zero, division by zero
- operators [Visual Basic], arithmetic
- arithmetic operators [Visual Basic], division
- division [Visual Basic], by zero
- operators [Visual Basic], division
- division operator [Visual Basic], syntax
- / operator [Visual Basic]
- math operators [Visual Basic]
ms.assetid: 335e97f2-c434-439e-9064-76973a051101
ms.openlocfilehash: 537d8b0c703b59743f1a7c531448118058707645
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331056"
---
# <a name="-operator-visual-basic"></a>/ 演算子 (Visual Basic)
2つの数値を除算して、浮動小数点の結果を返します。  
  
## <a name="syntax"></a>構文  
  
```vb  
expression1 / expression2  
```  
  
## <a name="parts"></a>指定項目  
 `expression1`  
 必須。 任意の数式。  
  
 `expression2`  
 必須。 任意の数式。  
  
## <a name="supported-types"></a>サポートされている型  
 符号なしおよび浮動小数点型および `Decimal`を含むすべての数値型。  
  
## <a name="result"></a>結果  
 結果は、剰余を含め、`expression2`で割った `expression1` の完全な商です。  
  
 [\ 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)は、剰余を削除する整数の商を返します。  
  
## <a name="remarks"></a>コメント  
 結果のデータ型は、オペランドの型によって異なります。 次の表は、結果のデータ型がどのように決定されるかを示しています。  
  
|オペランドのデータ型|結果のデータ型|  
|------------------------|----------------------|  
|両方の式は整数データ型 ([SByte](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)、 [Byte](../../../visual-basic/language-reference/data-types/byte-data-type.md)、 [Short](../../../visual-basic/language-reference/data-types/short-data-type.md)、 [UShort](../../../visual-basic/language-reference/data-types/ushort-data-type.md)、 [Integer](../../../visual-basic/language-reference/data-types/integer-data-type.md)、 [UInteger](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)、 [Long](../../../visual-basic/language-reference/data-types/long-data-type.md)、 [ULong](../../../visual-basic/language-reference/data-types/ulong-data-type.md)) です。|`Double`|  
|1つの式が [単精度浮動小数点型(Single)](../../../visual-basic/language-reference/data-types/single-data-type.md)データ型で、もう一方が [倍精度浮動小数点数型(Double)](../../../visual-basic/language-reference/data-types/double-data-type.md)ではない場合|`Single`|  
|一方の式は Decimal データ型で、もう一方は[単](../../../visual-basic/language-reference/data-types/single-data-type.md)[精度浮動](../../../visual-basic/language-reference/data-types/double-data-type.md)[小数点](../../../visual-basic/language-reference/data-types/decimal-data-type.md)型ではありません。|`Decimal`|  
|どちらの式も [倍精度浮動小数点数型 (Double)](../../../visual-basic/language-reference/data-types/double-data-type.md)データ型の場合|`Double`|  
  
 除算を実行する前に、整数の数値式はすべて `Double`に拡張されます。 結果を整数データ型に代入すると、Visual Basic は `Double` の結果をその型に変換しようとします。 結果がその型に合わない場合、例外がスローされる可能性があります。 特に、このヘルプページの「ゼロによる除算」を参照してください。  
  
 `expression1` または `expression2` が[Nothing](../../../visual-basic/language-reference/nothing.md)と評価された場合、0として扱われます。  
  
## <a name="attempted-division-by-zero"></a>0による除算を試行しました  
 `expression2` が0に評価された場合、オペランドのデータ型によって `/` 演算子の動作が異なります。 次の表に、考えられる動作を示します。  
  
|オペランドのデータ型|`expression2` が0の場合の動作|  
|------------------------|---------------------------------------|  
|浮動小数点 (`Single` または `Double`)|無限大 (<xref:System.Double.PositiveInfinity> または <xref:System.Double.NegativeInfinity>)、または `expression1` が0の場合は <xref:System.Double.NaN> (非数) を返します。|  
|`Decimal`|スロー <xref:System.DivideByZeroException>|  
|整数 (符号付きまたは符号なし)|整数型への変換 <xref:System.Double.NaN> は、<xref:System.Double.PositiveInfinity>、<xref:System.Double.NegativeInfinity>、またはを受け入れることができないため、<xref:System.OverflowException> をスローします|  
  
> [!NOTE]
> `/` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 コードでこのようなクラスまたは構造体に対してこの演算子を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 この例では、`/` 演算子を使用して、浮動小数点除算を実行します。 結果は、2つのオペランドの商になります。  
  
 [!code-vb[VbVbalrOperators#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#16)]  
  
 前の例の式では、2.5 と3.333333 の値が返されます。 両方のオペランドが整数定数の場合でも、結果は常に浮動小数点 (`Double`) になることに注意してください。  
  
## <a name="see-also"></a>参照

- [/= 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/floating-point-division-assignment-operator.md)
- [\ 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)
- [演算子の結果のデータ型](../../../visual-basic/language-reference/operators/data-types-of-operator-results.md)
- [算術演算子](../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Visual Basic の算術演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
