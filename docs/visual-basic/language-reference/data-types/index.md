---
title: データ型の概要
ms.date: 07/20/2015
helpviewer_keywords:
- Boolean data type [Visual Basic], supported types in Visual Basic
- storage [Visual Basic], order of storage
- data types [Visual Basic], Visual Basic
- Single data type [Visual Basic], supported types in Visual Basic
- notation [Visual Basic], scientific
- memory requirements, data types
- user-defined data types [Visual Basic], Visual Basic
- Date data type [Visual Basic], Visual Basic
- Visual Basic, data types
- storage [Visual Basic], allocation
- Integer data type [Visual Basic], Visual Basic data types
- storage [Visual Basic], space
- Variant data types [Visual Basic], supported types in Visual Basic
- Char data type [Visual Basic], Visual Basic data types
- intrinsic data types [Visual Basic]
- memory consumption [Visual Basic], data types
- single-precision numbers
- data types [Visual Basic], order of storage
- Long data type [Visual Basic], supported types in Visual Basic
- String data type [Visual Basic], Visual Basic data types
- storage order, data types
- StructLayoutAttribute class, Visual Basic data type storage
- scientific notation
- Double data type [Visual Basic], Visual Basic data types
- Byte data type [Visual Basic], Visual Basic data types
- Object data type [Visual Basic], supported types in Visual Basic
- data types [Visual Basic], storage allocation
- double-precision numbers
- data types [Visual Basic], summary
- dates [Visual Basic], data types
- strings [Visual Basic], data types
- memory consumption
- storage order, controlling in Visual Basic
- data types [Visual Basic], memory requirements
ms.assetid: e975cdb6-64d8-4a4a-ae27-f3b3ed198ae0
ms.openlocfilehash: 72bd8158880c602fb2cde92a3851c4e8a702c70b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343996"
---
# <a name="data-type-summary-visual-basic"></a>データ型の概要 (Visual Basic)

次の表に、Visual Basic のデータ型、サポートする共通言語ランタイム型、その公称ストレージ割り当て、およびそれらの値範囲を示します。  
  
|Visual Basic の種類|共通言語ランタイム型の構造体|公称ストレージ割り当て|値の範囲|  
|-----------------------|--------------------------------------------|--------------------------------|-----------------|  
|[Boolean](../../../visual-basic/language-reference/data-types/boolean-data-type.md)|<xref:System.Boolean>|実装するプラットフォームに依存|`True` または `False`|  
|[Byte](../../../visual-basic/language-reference/data-types/byte-data-type.md)|<xref:System.Byte>|1 バイト|0 ~ 255 (符号なし)|  
|[Char](../../../visual-basic/language-reference/data-types/char-data-type.md) (単一文字)|<xref:System.Char>|2 バイト|0 ~ 65535 (符号なし)|  
|[Date](../../../visual-basic/language-reference/data-types/date-data-type.md)|<xref:System.DateTime>|8 バイト|0:00:00 (午前0時)、0001年1月1日から 11:59:59 PM (9999 年12月31日)|  
|[Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md)|<xref:System.Decimal>|16 バイト|0 ~ +/-79228162514264337593543950335 (+/-7.9...E + 28) <sup>†</sup> (小数点なし)0 ~ +/-7.9228162514264337593543950335 (10 進数の右側に28桁)<br /><br /> 0以外の最小値は +/-0.0000000000000000000000000001 (+/-1E-28) <sup>†</sup>です。|  
|[Double](../../../visual-basic/language-reference/data-types/double-data-type.md) (倍精度浮動小数点)|<xref:System.Double>|8 バイト|-1.79769313486231570 e + 308 から-4.94065645841246544 E-324 <sup>†</sup> (負の値の場合)<br /><br /> 4.94065645841246544 e-324 ~ 1.79769313486231570 E + 308 <sup>†</sup> (正の値)|  
|[Integer](../../../visual-basic/language-reference/data-types/integer-data-type.md)|<xref:System.Int32>|4 バイト|-2,147, 483,648 ~ 2,147, 483,647 (符号あり)|  
|[Long](../../../visual-basic/language-reference/data-types/long-data-type.md) (長整数)|<xref:System.Int64>|8 バイト|-9223372036854775808 ~ 9223372036854775807 (9.2... E + 18 <sup>†</sup>) (符号付き)|  
|[Object](../../../visual-basic/language-reference/data-types/object-data-type.md)|<xref:System.Object> (クラス)|32ビットプラットフォームで4バイト<br /><br /> 64ビットプラットフォームで8バイト|任意の型を型の変数に格納することができ `Object`|  
|[SByte](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)|<xref:System.SByte>|1 バイト|-128 ~ 127 (符号あり)|  
|[Short](../../../visual-basic/language-reference/data-types/short-data-type.md) (short 整数)|<xref:System.Int16>|2 バイト|-32,768 ~ 32,767 (符号あり)|  
|[Single](../../../visual-basic/language-reference/data-types/single-data-type.md) (単精度浮動小数点)|<xref:System.Single>|4 バイト|-3.4028235 e + 38 ~-1.401298 E-45 <sup>†</sup> (負の値の場合)<br /><br /> 1.401298 e-45 ~ 3.4028235 E + 38 <sup>†</sup> (正の値)|  
|[String](../../../visual-basic/language-reference/data-types/string-data-type.md) (可変長)|<xref:System.String> (クラス)|実装するプラットフォームに依存|0 ~ 約20億の Unicode 文字|  
|[UInteger](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)|<xref:System.UInt32>|4 バイト|0 ~ 4294967295 (符号なし)|  
|[ULong](../../../visual-basic/language-reference/data-types/ulong-data-type.md)|<xref:System.UInt64>|8 バイト|0 ~ 18446744073709551615 (1.8... E + 19 <sup>†</sup>) (符号なし)|  
|[ユーザー定義](../../../visual-basic/language-reference/data-types/user-defined-data-type.md)(構造体)|(<xref:System.ValueType>から継承)|実装するプラットフォームに依存|構造体の各メンバーには、そのデータ型によって決定され、他のメンバーの範囲とは独立した範囲があります。|  
|[UShort](../../../visual-basic/language-reference/data-types/ushort-data-type.md)|<xref:System.UInt16>|2 バイト|0 ~ 65535 (符号なし)|  
  
 <sup>†</sup>*科学的表記法*では、"E" は10の累乗を意味します。 したがって、3.56 E + 2 は 3.56 x 10<sup>2</sup>または356を示し、3.56 は 3.56/10<sup>2</sup>または0.0356 を意味します。  
  
> [!NOTE]
> テキストを含む文字列の場合は、<xref:Microsoft.VisualBasic.Strings.StrConv%2A> 関数を使用してテキスト形式を別の形式に変換します。  
  
 宣言ステートメントでデータ型を指定するだけでなく、型文字を使用して一部のプログラミング要素のデータ型を強制的に指定することもできます。 「[型文字](../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)」を参照してください。  
  
## <a name="memory-consumption"></a>メモリの使用量  

 基本データ型を宣言する場合、メモリ消費量が公称ストレージ割り当てと同じであると想定するのは安全ではありません。 これは、次の点に注意してください。  
  
- **ストレージの割り当て。** 共通言語ランタイムは、アプリケーションが実行されているプラットフォームの現在の特性に基づいてストレージを割り当てることができます。 メモリがほぼいっぱいの場合は、宣言された要素を可能な限り厳密にまとめることができます。 それ以外の場合は、パフォーマンスを最適化するために、メモリアドレスを自然ハードウェアの境界に合わせることができます。  
  
- **プラットフォームの幅。** 64ビットプラットフォームでのストレージ割り当ては、32ビットプラットフォームでの割り当てとは異なります。  
  
### <a name="composite-data-types"></a>複合データ型  

 構造体や配列など、複合データ型の各メンバーにも同じ考慮事項が適用されます。 型のメンバーの公称ストレージ割り当てを簡単に追加することはできません。 さらに、次のような考慮事項があります。  
   
- **オーバーヘッド。** 複合型によっては、追加のメモリ要件があります。 たとえば、配列は配列自体に追加のメモリを使用し、次元ごとにも使用します。 32 ビットプラットフォームでは、このオーバーヘッドは現在 12 バイト、各ディメンションに 8 バイトを加えたものです。 64 ビットプラットフォームでは、この要件は 2 倍になります。  
  
- **ストレージのレイアウト。** メモリ内のストレージの順序は、宣言の順序と同じであると想定することはできません。 2バイトまたは4バイトの境界など、バイトのアラインメントについても想定することはできません。 クラスまたは構造体を定義していて、そのメンバーのストレージレイアウトを制御する必要がある場合は、クラスまたは構造体に <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を適用できます。  
  
### <a name="object-overhead"></a>オブジェクトのオーバーヘッド  

 任意の基本データ型または複合データ型を参照する `Object` は、データ型に含まれるデータに加えて4バイトを使用します。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Strings.StrConv%2A>
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [型文字](../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
