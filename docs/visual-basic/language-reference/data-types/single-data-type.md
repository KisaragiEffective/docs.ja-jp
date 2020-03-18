---
title: 単精度浮動小数点型 (Single)
ms.date: 07/20/2015
f1_keywords:
- vb.Single
helpviewer_keywords:
- Single data type
- F literal type character [Visual Basic]
- trailing zeros
- real numbers
- literal type characters [Visual Basic], F
- trailing 0 characters [Visual Basic]
- identifier type characters [Visual Basic], !
- single-precision numbers
- '! identifier type character'
- 0 characters [Visual Basic], trailing
- data types [Visual Basic], assigning
- floating-point numbers [Visual Basic], Single data type
- numbers [Visual Basic], real
- zeros, trailing
- numbers [Visual Basic], floating point
ms.assetid: 224a2795-4cd5-496c-8f7a-a4f05a06d45d
ms.openlocfilehash: 60a688c510f6e36dca5809566b37a388429e18c7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343918"
---
# <a name="single-data-type-visual-basic"></a>単精度浮動小数点型 (Single) (Visual Basic)

負の値の場合は-3.4028235 E + 38 から-1.401298 E-45 の値、正の値の場合は 1.401298 E-45 から 3.4028235 E + 38 までの範囲で、符号付き IEEE 32 ビット (4 バイト) の単精度浮動小数点数を保持します。 単精度数値は、実数の概数を格納します。  
  
## <a name="remarks"></a>コメント  

 `Double`の完全なデータ幅を必要としない浮動小数点値を格納するには、`Single` データ型を使用します。 場合によっては、共通言語ランタイムが `Single` 変数をまとめてパッケージ化し、メモリ消費量を節約できることがあります。  
  
 `Single` の既定値は 0 です。  
  
## <a name="programming-tips"></a>プログラミングのヒント  
  
- **精度.** 浮動小数点数を使用する場合は、メモリ内に常に正確な表現がないという点に注意してください。 これにより、値の比較や `Mod` 演算子など、特定の操作によって予期しない結果が生じる可能性があります。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」を参照してください。  
  
- **広げ.** `Single` データ型は、`Double`に拡大変換されます。 これは、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`Single` を `Double` に変換できることを意味します。  
  
- **後続のゼロ。** 浮動小数点データ型には、末尾の0文字の内部表現がありません。 たとえば、4.2000 と4.2 を区別しません。 そのため、浮動小数点値を表示または印刷するときに、後続の0文字は表示されません。  
  
- **文字を入力します。** あるリテラルにリテラルの型文字 `F` を付けると、そのリテラルは `Single` に変換されます。 ある識別子に識別子の型文字 `!` を付けると、その識別子は整数型 (`Single`) に変換されます。  
  
- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.Single?displayProperty=nameWithType> 構造体です。  
  
## <a name="see-also"></a>参照

- <xref:System.Single?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [Decimal データ型](../../../visual-basic/language-reference/data-types/decimal-data-type.md)
- [Double 型](../../../visual-basic/language-reference/data-types/double-data-type.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [トラブルシューティング (データ型)](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
