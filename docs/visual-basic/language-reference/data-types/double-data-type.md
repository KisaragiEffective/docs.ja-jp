---
title: 倍精度浮動小数点数型 (Double)
ms.date: 07/20/2015
f1_keywords:
- vb.Double
helpviewer_keywords:
- 'identifier type characters [Visual Basic], #'
- trailing zeros
- real numbers
- trailing 0 characters [Visual Basic]
- 0 characters [Visual Basic], trailing
- literal type characters [Visual Basic], R
- data types [Visual Basic], assigning
- Double data type [Visual Basic]
- '# identifier type character'
- double-precision numbers
- floating-point numbers [Visual Basic], Double data type
- R literal type character [Visual Basic]
- zeros, trailing
- Double data type
ms.assetid: 0c5670f7-fcb1-453a-bef1-374730cd38fd
ms.openlocfilehash: 347b5c7b7af4c4aafec0f91aca46a8cf640236b9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344012"
---
# <a name="double-data-type-visual-basic"></a>倍精度浮動小数点数型 (Double) (Visual Basic)

負の値に対して-1.79769313486231570 E + 308 から-4.94065645841246544 E-324 までの値を範囲とする、符号付き IEEE 64 ビット (8 バイト) の倍精度浮動小数点数を保持します。 4.94065645841246544 E-324 から 1.79769313486231570 E + 308 まで正の値。 倍精度数値は、実数の概数を格納します。

## <a name="remarks"></a>コメント

`Double` データ型は、数値に対して可能な最大値と最小大きくなりを提供します。

`Double` の既定値は 0 です。

## <a name="programming-tips"></a>プログラミングのヒント

- **有効桁数。 ** 浮動小数点数を使用する場合は、メモリ内に常に正確な表現がないという点に注意してください。 これにより、値の比較や `Mod` 演算子など、特定の操作によって予期しない結果が生じる可能性があります。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」を参照してください。

- **後続のゼロ。** 浮動小数点データ型には、後続のゼロ文字の内部表現がありません。 たとえば、4.2000 と4.2 を区別しません。 そのため、浮動小数点値を表示または印刷するときに、後続のゼロ文字は表示されません。

- **文字を入力します。** あるリテラルにリテラルの型文字 `R` を付けると、そのリテラルは `Double` に変換されます。 たとえば、整数値の後に `R`がある場合、値は `Double`に変更されます。

  ```vb
  ' Visual Basic expands the 4 in the statement Dim dub As Double = 4R to 4.0:
  Dim dub As Double = 4.0R
  ```

  ある識別子に識別子の型文字 `#` を付けると、その識別子は整数型 (`Double`) に変換されます。 次の例では、変数 `num` が `Double`として型指定されています。

  ```vb
  Dim num# = 3
  ```

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.Double?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>関連項目

- <xref:System.Double?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [Decimal データ型](../../../visual-basic/language-reference/data-types/decimal-data-type.md)
- [Single データ型](../../../visual-basic/language-reference/data-types/single-data-type.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [トラブルシューティング (データ型)](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [型文字](../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
