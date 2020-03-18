---
title: 数値データ型
ms.date: 07/20/2015
helpviewer_keywords:
- integral types [Visual Basic], Visual Basic
- Short data type [Visual Basic], numeric data types
- Double data type [Visual Basic], numeric data types
- Long data type [Visual Basic], Visual Basic numeric data types
- numbers [Visual Basic], whole
- fractions
- numbers
- whole numbers
- integer numbers
- numbers [Visual Basic], integer
- fractional data types [Visual Basic]
- mantissas, of fractional numbers
- mantissas
- data types [Visual Basic], numeric
- Integer data type [Visual Basic], numeric data types
- exponent, of fractional numbers
- integers [Visual Basic]
- numeric data types [Visual Basic], Visual Basic
- Single data type [Visual Basic], numeric types
- Decimal data type [Visual Basic], numeric data types
ms.assetid: a27bd4d0-7e14-43eb-9cc4-b42eaab323c9
ms.openlocfilehash: dc8b630eebc48e5733344a00664b453360769c0b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346310"
---
# <a name="numeric-data-types-visual-basic"></a>数値データ型 (Visual Basic)
Visual Basic には、さまざまな表現で数値を処理するための*数値データ型*がいくつか用意されています。 *整数型は*整数 (正、負、0) のみを*表し、非整数型は*整数と小数部の両方を持つ数値を表します。  
  
 Visual Basic のデータ型の並列比較を示す表については、「[データ型](../../../../visual-basic/language-reference/data-types/index.md)」を参照してください。  
  
## <a name="integral-numeric-types"></a>整数の数値型  
 *整数データ型*は、小数部を含まない数値のみを表す型です。  
  
 *符号付き*整数データ型は、 [SByte データ型](../../../../visual-basic/language-reference/data-types/sbyte-data-type.md)(8 ビット)、 [Short データ型](../../../../visual-basic/language-reference/data-types/short-data-type.md)(16 ビット)、[整数データ型](../../../../visual-basic/language-reference/data-types/integer-data-type.md)(32 ビット)、および[Long データ型](../../../../visual-basic/language-reference/data-types/long-data-type.md)(64 ビット) です。 変数が小数点数ではなく整数を常に格納する場合は、次のいずれかの型として宣言します。  
  
 *符号なし*整数型は、 [Byte データ型](../../../../visual-basic/language-reference/data-types/byte-data-type.md)(8 ビット)、 [UShort データ型](../../../../visual-basic/language-reference/data-types/ushort-data-type.md)(16 ビット)、 [UInteger データ型](../../../../visual-basic/language-reference/data-types/uinteger-data-type.md)(32 ビット)、および[ULong データ型](../../../../visual-basic/language-reference/data-types/ulong-data-type.md)(64 ビット) です。 変数にバイナリデータまたは不明な性質のデータが含まれている場合は、次のいずれかの型として宣言します。  
  
### <a name="performance"></a>パフォーマンス  
 算術演算は、他のデータ型よりも整数型の方が高速です。 これらは Visual Basic の `Integer` と `UInteger` の種類によって最速になります。  
  
### <a name="large-integers"></a>大きい整数  
 データ型が保持できる `Integer` よりも大きい整数を保持する必要がある場合は、代わりに `Long` データ型を使用できます。 `Long` 変数は、-9223372036854775808 ~ 9223372036854775807 の数値を保持できます。 `Long` を使用した操作は、`Integer`よりも少し遅くなります。  
  
 さらに大きな値が必要な場合は、 [Decimal データ型](../../../../visual-basic/language-reference/data-types/decimal-data-type.md)を使用できます。 小数点以下の桁数を使用しない場合は、`Decimal` 変数に-79228162514264337593543950335 ~ 79228162514264337593543950335 の数値を保持できます。 ただし、`Decimal` 数を持つ操作は、他の数値データ型よりもかなり低速です。  
  
### <a name="small-integers"></a>小整数  
 `Integer` データ型の範囲全体が不要な場合は、-32768 ~ 32767 の整数を保持できる `Short` データ型を使用できます。 最小の整数範囲の場合、`SByte` データ型には-128 ~ 127 の整数が格納されます。 少数の整数を保持する変数が非常に多い場合は、共通言語ランタイムによって `Short` と `SByte` 変数がより効率的に格納され、メモリ消費が節約されることがあります。 ただし、`Short` と `SByte` を使用した操作は、`Integer`よりも多少遅くなります。  
  
### <a name="unsigned-integers"></a>符号なし整数  
 変数が負の数を保持する必要がないことがわかっている場合は、`Byte`、`UShort`、`UInteger`、および `ULong`の*符号なしの型*を使用できます。 これらの各データ型は、対応する符号付きの型 (`SByte`、`Short`、`Integer`、および `Long`) に対して、正の整数を2倍にすることができます。 パフォーマンス面では、符号なしの各型は、対応する符号付きの型とまったく同じように効率的です。 特に、すべての基本数値データ型を使用するのが最も効率的であるということを `Integer` と共有 `UInteger` ます。  
  
## <a name="nonintegral-numeric-types"></a>非整数数値型  
 非整数*データ型*は、整数と小数部の両方を持つ数値を表すものです。  
  
 整数以外の数値データ型は、`Decimal` (128 ビット固定ポイント)、[単一データ型](../../../../visual-basic/language-reference/data-types/single-data-type.md)(32 ビット浮動小数点)、および[倍精度](../../../../visual-basic/language-reference/data-types/double-data-type.md)浮動小数点 (64 ビット浮動小数点) です。 これらはすべて、署名された型です。 変数に分数を含めることができる場合は、次のいずれかの型として宣言します。  
  
 `Decimal` は、浮動小数点データ型ではありません。 `Decimal` 数値には、2進数の整数値と、値の小数点以下の部分を指定する整数の拡大率を指定します。  
  
 Money 値には `Decimal` 変数を使用できます。 この利点は、値の有効桁数です。 `Double` データ型の方が高速で、必要なメモリが少なくなりますが、丸めエラーが発生する可能性があります。 `Decimal` のデータ型では、完全な精度は28桁まで保持されます。  
  
 浮動小数点 (`Single` および `Double`) の数値の範囲は `Decimal` の数値よりも大きくなりますが、丸め誤差の影響を受ける可能性があります。 浮動小数点型では、`Decimal` よりも有効桁数が少ないものの、より大きい値を表すことができます。  
  
 非整数の数値は mmmEeee として表すことができます。ここで、mmm は*仮数*(有効桁数)、eee は*指数*(10 の累乗) です。 非整数型の正の最大値は、`Decimal`の場合は 7.9228162514264337593543950335 E + 28、`Single`の場合は 3.4028235 E + 28、`Double`の場合は 1.79769313486231570 E + 308 です。  
  
### <a name="performance"></a>パフォーマンス  
 現在のプラットフォームのプロセッサは倍精度で浮動小数点演算を実行するため、`Double` は小数データ型の中で最も効率的です。 ただし、`Double` の操作は、`Integer`などの整数型ほど高速ではありません。  
  
### <a name="small-magnitudes"></a>大きくなり s  
 可能な最小の絶対値を持つ数値 (0 に最も近い) では、`Double` の変数は負の値の場合は-4.94065645841246544 E-324、正の値の場合は 4.94065645841246544 E-324 のように小さい数値を保持できます。  
  
### <a name="small-fractional-numbers"></a>小数点以下の桁数  
 `Double` データ型の範囲全体が不要な場合は、-3.4028235 E + 38 から 3.4028235 E + 38 までの浮動小数点数を保持できるデータ型 `Single` を使用できます。 `Single` 変数の最小の大きくなりは、負の値の場合は-1.401298 E-45、正の値の場合は 1.401298 E-45 です。 小さい浮動小数点数を保持する変数が非常に多い場合は、共通言語ランタイムによって `Single` 変数がより効率的に格納され、メモリ使用量が節約されることがあります。  
  
## <a name="see-also"></a>関連項目

- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [文字データ型](../../../../visual-basic/programming-guide/language-features/data-types/character-data-types.md)
- [その他のデータ型](../../../../visual-basic/programming-guide/language-features/data-types/miscellaneous-data-types.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
