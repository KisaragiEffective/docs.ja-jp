---
title: 演算子の結果のデータ型
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], operator result data types
- result data types [Visual Basic]
- operator result data types [Visual Basic]
- operators [Visual Basic], data types
- data types [Visual Basic], ranges
- operators [Visual Basic], result data types
ms.assetid: 9d524533-e1a1-4aa8-b1b8-622068173d06
ms.openlocfilehash: 3867d433ea5f9a6effe70db0ff4162390fb50b5c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331469"
---
# <a name="data-types-of-operator-results-visual-basic"></a>演算子の結果のデータ型 (Visual Basic)
Visual Basic は、オペランドのデータ型に基づいて、操作の結果のデータ型を決定します。 場合によっては、これがいずれかのオペランドよりも範囲の広いデータ型である可能性があります。  
  
## <a name="data-type-ranges"></a>データ型の範囲  
 関連するデータ型の範囲は、小さい方から順に、次のようになります。  
  
- [Boolean](../../../visual-basic/language-reference/data-types/boolean-data-type.md): 2 種類の値  
  
- [SByte](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)、 [Byte](../../../visual-basic/language-reference/data-types/byte-data-type.md) : 256 種類の整数値  
  
- [Short](../../../visual-basic/language-reference/data-types/short-data-type.md)、 [UShort](../../../visual-basic/language-reference/data-types/ushort-data-type.md) : 65,536 (6.5...E+4) 種類の整数値  
  
- [Integer](../../../visual-basic/language-reference/data-types/integer-data-type.md)、 [UInteger](../../../visual-basic/language-reference/data-types/uinteger-data-type.md) : 4,294,967,296 (4.2...E+9) 種類の整数値  
  
- [Long](../../../visual-basic/language-reference/data-types/long-data-type.md)、 [ULong](../../../visual-basic/language-reference/data-types/ulong-data-type.md) : 18,446,744,073,709,551,615 (1.8...E+19) 種類の整数値  
  
- [10 進](../../../visual-basic/language-reference/data-types/decimal-data-type.md) : 1.5...E+29 種類の整数値、最大範囲 7.9...E+28 (絶対値)  
  
- [Single](../../../visual-basic/language-reference/data-types/single-data-type.md) : 最大範囲 3.4...E+38 (絶対値)  
  
- [Double](../../../visual-basic/language-reference/data-types/double-data-type.md) : 最大範囲 1.7...E+308 (絶対値)  
  
 Visual Basic のデータ型の詳細については、「[データ型](../../../visual-basic/language-reference/data-types/index.md)」を参照してください。  
  
 オペランドが[Nothing](../../../visual-basic/language-reference/nothing.md)と評価された場合、Visual Basic 算術演算子は0として処理します。  
  
## <a name="decimal-arithmetic"></a>10進数の算術演算  
 [Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md)データ型は、浮動小数点と整数のどちらでもないことに注意してください。  
  
 `+`、`–`、`*`、`/`、または `Mod` 操作のオペランドのいずれかが `Decimal` で、もう一方が `Single` でも `Double`でもない場合は、他のオペランドを Visual Basic に拡大変換 `Decimal`ます。 `Decimal`で操作を実行し、結果のデータ型を `Decimal`します。  
  
## <a name="floating-point-arithmetic"></a>浮動小数点演算  
 Visual Basic は、ほとんどの浮動小数点演算を[Double](../../../visual-basic/language-reference/data-types/double-data-type.md)で実行します。これは、このような操作にとって最も効率的なデータ型です。 一方のオペランドが[1](../../../visual-basic/language-reference/data-types/single-data-type.md)つで、もう一方が `Double`ない場合、Visual Basic は `Single`で演算を実行します。 必要に応じて各オペランドを適切なデータ型に変換し、その結果をそのデータ型に変換します。  
  
### <a name="-and--operators"></a>/および ^ 演算子  
 `/` 演算子は、 [Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md)、 [Single](../../../visual-basic/language-reference/data-types/single-data-type.md)、および[Double](../../../visual-basic/language-reference/data-types/double-data-type.md)データ型に対してのみ定義されます。 演算の前に各オペランドを適切なデータ型に拡大変換すると、結果のデータ型は Visual Basic になります。  
  
 次の表は、`/` 演算子の結果のデータ型を示しています。 このテーブルは対称であることに注意してください。オペランドデータ型の特定の組み合わせについては、オペランドの順序に関係なく、結果のデータ型は同じになります。  
  
||||||  
|---|---|---|---|---|  
||`Decimal`|`Single`|`Double`|任意の整数型|  
|`Decimal`|10 進数|Single|Double|10 進数|  
|`Single`|Single|Single|Double|Single|  
|`Double`|Double|Double|Double|Double|  
|任意の整数型|10 進数|Single|Double|Double|  
  
 `^` 演算子は、`Double` データ型に対してのみ定義されています。 演算の前に `Double` するために各オペランドを必要に応じて拡大 Visual Basic し、結果のデータ型は常に `Double`ます。  
  
## <a name="integer-arithmetic"></a>整数演算  
 整数演算の結果のデータ型は、オペランドのデータ型によって異なります。 一般に、Visual Basic では、次のポリシーを使用して結果のデータ型を決定します。  
  
- 二項演算子の両方のオペランドのデータ型が同じである場合、結果はそのデータ型になります。 例外は `Boolean`であり、`Short`に強制されます。  
  
- 符号なしのオペランドが符号付きのオペランドに含まれている場合、結果の符号付きの型は、少なくともいずれかのオペランドと同じ範囲になります。  
  
- それ以外の場合、結果は通常、2つのオペランドデータ型のうち、大きい方になります。  
  
 結果のデータ型は、どちらのオペランドデータ型とも異なる場合があることに注意してください。  
  
> [!NOTE]
> 結果のデータ型は、操作の結果として得られるすべての値を保持するのに十分な大きさではありません。 値が結果のデータ型に対して大きすぎる場合は、<xref:System.OverflowException> 例外が発生する可能性があります。  
  
### <a name="unary--and--operators"></a>単項 + 演算子と–演算子  
 次の表は、`+` と `–`の2つの単項演算子の結果のデータ型を示しています。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|単項 `+`|Short|SByte|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
|単項 `–`|Short|SByte|Short|Short|整数型|整数型|Long|Long|10 進数|  
  
### <a name="-and--operators"></a><\< と > > 演算子  
 次の表は、2つのビットシフト演算子、`<<` および `>>`の結果のデータ型を示しています。 Visual Basic は、各ビットシフト演算子を左オペランド (シフトされるビットパターン) で単項演算子として扱います。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`<<`, `>>`|Short|SByte|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
  
 左側のオペランドが `Decimal`、`Single`、`Double`、または `String`の場合、Visual Basic は演算の前に変換を試行し、結果のデータ型は `Long` になります。`Long` 右オペランド (シフトするビット位置の数) は `Integer` であるか、または `Integer`に拡大変換される型である必要があります。  
  
### <a name="binary----and-mod-operators"></a>二項演算子 +、–、\*、および Mod 演算子  
 次の表に、バイナリ `+` と `–` 演算子、および `*` および `Mod` 演算子の結果のデータ型を示します。 このテーブルは対称であることに注意してください。オペランドデータ型の特定の組み合わせについては、オペランドの順序に関係なく、結果のデータ型は同じになります。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|整数型|整数型|Long|Long|10 進数|  
|`SByte`|SByte|SByte|Short|Short|整数型|整数型|Long|Long|10 進数|  
|`Byte`|Short|Short|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|整数型|整数型|Long|Long|10 進数|  
|`UShort`|整数型|整数型|UShort|整数型|UShort|整数型|UInteger|Long|ULong|  
|`Integer`|整数型|整数型|整数型|整数型|整数型|整数型|Long|Long|10 進数|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|10 進数|  
|`ULong`|10 進数|10 進数|ULong|10 進数|ULong|10 進数|ULong|10 進数|ULong|  
  
### <a name="-operator"></a>\\ 演算子  
 次の表は、`\` 演算子の結果のデータ型を示しています。 このテーブルは対称であることに注意してください。オペランドデータ型の特定の組み合わせについては、オペランドの順序に関係なく、結果のデータ型は同じになります。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|整数型|整数型|Long|Long|Long|  
|`SByte`|SByte|SByte|Short|Short|整数型|整数型|Long|Long|Long|  
|`Byte`|Short|Short|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|整数型|整数型|Long|Long|Long|  
|`UShort`|整数型|整数型|UShort|整数型|UShort|整数型|UInteger|Long|ULong|  
|`Integer`|整数型|整数型|整数型|整数型|整数型|整数型|Long|Long|Long|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|Long|  
|`ULong`|Long|Long|ULong|Long|ULong|Long|ULong|Long|ULong|  
  
 `\` 演算子のいずれかのオペランドが[Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md)、 [Single](../../../visual-basic/language-reference/data-types/single-data-type.md)、または[Double](../../../visual-basic/language-reference/data-types/double-data-type.md)の場合、Visual Basic は演算の前の[Long](../../../visual-basic/language-reference/data-types/long-data-type.md)への変換を試み、結果のデータ型は `Long`になります。  
  
## <a name="relational-and-bitwise-comparisons"></a>関係とビットごとの比較  
 リレーショナル操作 (`=`、`<>`、`<`、`>`、`<=`、`>=`) の結果のデータ型は、常に`Boolean` [Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)です。 これは、`OrElse`オペランドの論理演算 (`And`、`AndAlso`、`Not`、`Or`、`Xor`、`Boolean`)にも当てはまります。  
  
 ビットごとの論理演算の結果のデータ型は、オペランドのデータ型によって異なります。 `AndAlso` と `OrElse` は `Boolean`に対してのみ定義され、Visual Basic 操作を実行する前に、必要に応じて各オペランドを `Boolean` に変換します。  
  
### <a name="-----and--operators"></a>=、< >、\<、>、\<=、および > = 演算子  
 両方のオペランドが `Boolean`場合、Visual Basic は `True` を `False`未満と見なします。 数値型と `String`が比較される場合、Visual Basic は、操作の前に `String` を `Double` に変換しようとします。 `Char` または `Date` のオペランドは、同じデータ型の別のオペランドとのみ比較できます。 結果のデータ型は常に `Boolean`です。  
  
### <a name="bitwise-not-operator"></a>ビットごとの Not 演算子  
 次の表は、ビットごとの `Not` 演算子の結果のデータ型を示しています。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Not`|ブール値|SByte|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
  
 オペランドが `Decimal`、`Single`、`Double`、または `String`の場合、Visual Basic は演算の前の `Long` に変換しようとし、結果のデータ型は `Long`になります。  
  
### <a name="bitwise-and-or-and-xor-operators"></a>ビットごとの And、Or、および Xor 演算子  
 次の表は、ビットごとの `And`、`Or`、および `Xor` 演算子の結果のデータ型を示しています。 このテーブルは対称であることに注意してください。オペランドデータ型の特定の組み合わせについては、オペランドの順序に関係なく、結果のデータ型は同じになります。  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|ブール値|SByte|Short|Short|整数型|整数型|Long|Long|Long|  
|`SByte`|SByte|SByte|Short|Short|整数型|整数型|Long|Long|Long|  
|`Byte`|Short|Short|バイト|Short|UShort|整数型|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|整数型|整数型|Long|Long|Long|  
|`UShort`|整数型|整数型|UShort|整数型|UShort|整数型|UInteger|Long|ULong|  
|`Integer`|整数型|整数型|整数型|整数型|整数型|整数型|Long|Long|Long|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|Long|  
|`ULong`|Long|Long|ULong|Long|ULong|Long|ULong|Long|ULong|  
  
 オペランドが `Decimal`、`Single`、`Double`、または `String`の場合、そのオペランドは演算の前に Visual Basic に変換され、結果のデータ型は、そのオペランドが既に `Long` ている場合と同じになります。`Long`  
  
## <a name="miscellaneous-operators"></a>その他の演算子  
 `&` 演算子は、`String` オペランドを連結する場合にのみ定義されます。 Visual Basic は演算の前に各オペランドを `String` に変換し、結果のデータ型は常に `String`ます。 `&` 演算子の目的では、`Option Strict` が `On`場合でも、`String` へのすべての変換は拡大されていると見なされます。  
  
 `Is` 演算子と `IsNot` 演算子では、両方のオペランドが参照型である必要があります。 `TypeOf`...`Is` 式では、最初のオペランドが参照型で、2番目のオペランドがデータ型の名前である必要があります。 このような場合、結果のデータ型は `Boolean`になります。  
  
 `Like` 演算子は、`String` オペランドのパターン一致に対してのみ定義されます。 Visual Basic は、必要に応じて各オペランドを変換しようとして、操作の前に `String` します。 結果のデータ型は常に `Boolean`です。  
  
## <a name="see-also"></a>参照

- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [Visual Basic の算術演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Visual Basic の比較演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [演算子](../../../visual-basic/language-reference/operators/index.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [算術演算子](../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [比較演算子](../../../visual-basic/language-reference/operators/comparison-operators.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
