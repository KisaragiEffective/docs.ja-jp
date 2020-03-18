---
title: CString
ms.date: 10/24/2018
f1_keywords:
- vb.CUShort
- vb.csng
- vb.CDate
- CByte
- CSng
- vb.CDec
- CBool
- CStr
- vb.CULng
- CDec
- CVErr
- CDbl
- CShort
- vb.CObj
- vb.CVErr
- CULng
- vb.cdbl
- vb.cbool
- CObj
- CDate
- CLng
- vb.cstr
- vb.cbyte
- vb.clng
- vb.CChar
- CUShort
- vb.CUInt
- vb.cint
- vb.CShort
- CInt
- CUInt
- CChar
helpviewer_keywords:
- CDate function
- CByte function
- Integer data type [Visual Basic], converting
- string conversion [Visual Basic], conversion functions
- fractions
- data types [Visual Basic], converting
- text, converting
- CDec function
- Char data type [Visual Basic], converting
- type conversion [Visual Basic], functions for
- Single data type [Visual Basic], converting
- numbers [Visual Basic], rounding
- rounding numbers [Visual Basic], type conversion
- CUShort function
- Long data type [Visual Basic], converting
- return values [Visual Basic], data types
- single-precision numbers [Visual Basic], converting
- data type conversion [Visual Basic], functions for
- CStr function
- times [Visual Basic], converting
- CSng function
- conversions [Visual Basic], type conversion functions
- CBool function
- CDbl function
- CUInt function
- Currency data type [Visual Basic], conversion functions
- numbers [Visual Basic], converting
- Double data type [Visual Basic], converting
- CLng function
- CSByte function
- double-precision numbers
- Decimal data type [Visual Basic], converting
- Boolean data type [Visual Basic], converting
- integers [Visual Basic], type conversion functions
- dates [Visual Basic], converting
- CULng function
- CInt function
- Date data type [Visual Basic], converting
- Byte data type [Visual Basic], converting
- String data type [Visual Basic], converting
- CChar function
- banker's rounding
- Short data type [Visual Basic], converting
- rounding numbers [Visual Basic], banker's rounding
- type conversion [Visual Basic], Visual Basic vs. .NET Framework
ms.assetid: d9d8d165-f967-44ff-a6cd-598e4740a99e
ms.openlocfilehash: 3924da6ccbfea00668370f2fbcf4baf289be80db
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349972"
---
# <a name="type-conversion-functions-visual-basic"></a>データ型変換関数 (Visual Basic)

これらの関数はインラインでコンパイルされます。つまり、変換コードは、式を評価するコードの一部です。 変換を実行するためのプロシージャの呼び出しがないことがあります。これにより、パフォーマンスが向上します。 各関数は、式を特定のデータ型に変換します。

## <a name="syntax"></a>構文

```vb
CBool(expression)
CByte(expression)
CChar(expression)
CDate(expression)
CDbl(expression)
CDec(expression)
CInt(expression)
CLng(expression)
CObj(expression)
CSByte(expression)
CShort(expression)
CSng(expression)
CStr(expression)
CUInt(expression)
CULng(expression)
CUShort(expression)
```

## <a name="part"></a>要素

`expression`  
必須。 変換元のデータ型の任意の式。

## <a name="return-value-data-type"></a>戻り値のデータ型

関数名は、次の表に示すように、返される値のデータ型を決定します。

|関数名|戻り値のデータ型|`expression` 引数の範囲|
|-------------------|----------------------|-------------------------------------|
|`CBool`|[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)|任意の有効な `Char` または `String` または数値式。|
|`CByte`|[Byte データ型](../../../visual-basic/language-reference/data-types/byte-data-type.md)|<xref:System.Byte.MinValue?displayProperty=nameWithType> (0) から <xref:System.Byte.MaxValue?displayProperty=nameWithType> (255) (符号なし);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CByte` 関数を使用した浮動小数点からバイトへの変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CChar`|[Char データ型](../../../visual-basic/language-reference/data-types/char-data-type.md)|任意の有効な `Char` または `String` 式。`String` の最初の文字のみが変換されます。値には 0 ~ 65535 (符号なし) を指定できます。|
|`CDate`|[Date データ型](../../../visual-basic/language-reference/data-types/date-data-type.md)|任意の有効な日付と時刻の表現。|
|`CDbl`|[Double 型](../../../visual-basic/language-reference/data-types/double-data-type.md)|-1.79769313486231570 e + 308 から-4.94065645841246544 E-324 (負の値の場合)正の値の場合は、e-324 ~ 1.79769313486231570 E + 308 4.94065645841246544。|
|`CDec`|[Decimal データ型](../../../visual-basic/language-reference/data-types/decimal-data-type.md)|0から始まる数値の場合は +/-79228162514264337593543950335、小数点以下の場合は数値。 小数点以下を28桁の数値の場合、範囲は +/-7.9228162514264337593543950335 です。 0以外の最小値は 0.0000000000000000000000000001 (+/-1E-28) です。|
|`CInt`|[Integer データ型](../../../visual-basic/language-reference/data-types/integer-data-type.md)|<xref:System.Int32.MinValue?displayProperty=nameWithType> (-2147483648) から <xref:System.Int32.MaxValue?displayProperty=nameWithType> (2147483647);小数部は丸められます。<sup>1</sup> <br/><br/>Visual Basic 15.8 以降では、Visual Basic `CInt` 関数を使用した浮動小数点から整数への変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。 |
|`CLng`|[Long データ型](../../../visual-basic/language-reference/data-types/long-data-type.md)|<xref:System.Int64.MinValue?displayProperty=nameWithType> (-9223372036854775808) から <xref:System.Int64.MaxValue?displayProperty=nameWithType> (9223372036854775807);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CLng` 関数を使用した浮動小数点から64ビットへの整数変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CObj`|[Object Data Type](../../../visual-basic/language-reference/data-types/object-data-type.md)|任意の有効な式。|
|`CSByte`|[SByte データ型](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)|<xref:System.SByte.MinValue?displayProperty=nameWithType> (-128) から <xref:System.SByte.MaxValue?displayProperty=nameWithType> (127);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CSByte` 関数を使用した浮動小数点から符号付きバイト変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CShort`|[Short データ型](../../../visual-basic/language-reference/data-types/short-data-type.md)|<xref:System.Int16.MinValue?displayProperty=nameWithType> (-32768) から <xref:System.Int16.MaxValue?displayProperty=nameWithType> (32767);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CShort` 関数を使用した浮動小数点から16ビット整数への変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CSng`|[Single データ型](../../../visual-basic/language-reference/data-types/single-data-type.md)|-3.402823 e + 38 ~-1.401298 E-45 (負の値の場合)正の値の場合は、1.401298 e-45 ~ 3.402823 E + 38 です。|
|`CStr`|[String データ型](../../../visual-basic/language-reference/data-types/string-data-type.md)|`expression` 引数に依存する `CStr` の場合はを返します。 [CStr 関数の戻り値を](../../../visual-basic/language-reference/functions/return-values-for-the-cstr-function.md)参照してください。|
|`CUInt`|[UInteger データ型](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)|<xref:System.UInt32.MinValue?displayProperty=nameWithType> (0) から <xref:System.UInt32.MaxValue?displayProperty=nameWithType> (4294967295) (符号なし);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CUInt` 関数を使用した浮動小数点から符号なし整数への変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CULng`|[ULong データ型](../../../visual-basic/language-reference/data-types/ulong-data-type.md)|<xref:System.UInt64.MinValue?displayProperty=nameWithType> (0) から <xref:System.UInt64.MaxValue?displayProperty=nameWithType> (18446744073709551615) (符号なし) です。小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CULng` 関数を使用した浮動小数点から符号なし長整数への変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|
|`CUShort`|[UShort データ型](../../../visual-basic/language-reference/data-types/ushort-data-type.md)|<xref:System.UInt16.MinValue?displayProperty=nameWithType> (0) から <xref:System.UInt16.MaxValue?displayProperty=nameWithType> (65535) (符号なし);小数部は丸められます。<sup>1</sup><br/><br/>Visual Basic 15.8 以降では、Visual Basic `CUShort` 関数を使用して、浮動小数点から符号なしの16ビット整数型への変換のパフォーマンスを最適化します。詳細については、「[解説](#remarks)」を参照してください。 例については、「 [CInt の例](#cint-example)」を参照してください。|

<sup>1 個</sup>の小数部は、*銀行型丸め*と呼ばれる特殊な丸め処理に従うことができます。 詳細については、「解説」を参照してください。

## <a name="remarks"></a>コメント

原則として、Visual Basic 型変換関数は、<xref:System.Convert> クラスまたは個々の型構造体またはクラスの `ToString()`などの .NET Framework メソッドに対して優先的に使用する必要があります。 Visual Basic 関数は、Visual Basic コードとの最適な対話を目的として設計されています。また、ソースコードを短くして読みやすくします。 また、.NET Framework の変換メソッドでは、Visual Basic 関数と同じ結果が生成されるとは限りません。たとえば、`Boolean` を `Integer`に変換する場合などです。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」を参照してください。

Visual Basic 15.8 以降では、次のメソッドによって返される <xref:System.Single> または <xref:System.Double> 値を整数変換関数 (`CByte`、`CShort`、`CInt`、`CLng`、`CSByte`、`CUShort`、`CUInt`、`CULng`) のいずれかに渡すと、浮動小数点から整数への変換のパフォーマンスが最適化されます。

- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Double)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Object)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Fix(System.Single)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Double)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Object)?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Conversion.Int(System.Single)?displayProperty=nameWithType>
- <xref:System.Math.Ceiling(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Floor(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Round(System.Double)?displayProperty=nameWithType>
- <xref:System.Math.Truncate(System.Double)?displayProperty=nameWithType>

この最適化により、多数の整数変換を実行するコードは、最大2倍の速度で実行できます。 次の例は、これらの最適化された浮動小数点から整数への変換を示しています。

```vb
Dim s As Single = 173.7619
Dim d As Double = s

Dim i1 As Integer = CInt(Fix(s))               ' Result: 173
Dim b1 As Byte = CByte(Int(d))                 ' Result: 173
Dim s1 AS Short = CShort(Math.Truncate(s))     ' Result: 173
Dim i2 As Integer = CInt(Math.Ceiling(d))      ' Result: 174
Dim i3 As Integer = CInt(Math.Round(s))        ' Result: 174
```

## <a name="behavior"></a>動作

- **強制変換.** 一般に、データ型変換関数を使用すると、既定のデータ型ではなく、特定のデータ型に対して操作の結果を強制することができます。 たとえば、単精度、倍精度、または整数の算術演算が通常発生する場合は、`CDec` を使用して10進数の算術演算を実行します。

- **変換に失敗しました。** 関数に渡された `expression` が変換されるデータ型の範囲外の場合、<xref:System.OverflowException> が発生します。

- **小数部。** 整数型の値を整数型に変換する場合、整数変換関数 (`CByte`、`CInt`、`CLng`、`CSByte`、`CShort`、`CUInt`、`CULng`、および `CUShort`) は、小数部分を削除し、値を最も近い整数に丸めます。

     小数部分が完全に0.5 の場合、整数変換関数はそれを最も近い偶数の整数に丸めます。 たとえば、0.5 は0に丸められ、1.5 と2.5 は両方とも2に丸められます。 これは、*銀行型丸め*と呼ばれることもあります。その目的は、多数の数値を加算するときに蓄積される可能性があるバイアスを補正することです。

     `CInt` と `CLng` は、数値の小数部を丸めるのではなく、<xref:Microsoft.VisualBasic.Conversion.Int%2A> と <xref:Microsoft.VisualBasic.Conversion.Fix%2A> の関数とは異なります。 また、`Fix` と `Int` は、渡されたときと同じデータ型の値を常に返します。

- **日付/時刻の変換。** 値を日付と時刻に変換できるかどうかを判断するには、<xref:Microsoft.VisualBasic.Information.IsDate%2A> 関数を使用します。 `CDate` は、日付リテラルと時刻リテラルを認識しますが、数値は認識しません。 Visual Basic 6.0 `Date` 値を Visual Basic 2005 以降のバージョンの `Date` 値に変換するには、<xref:System.DateTime.FromOADate%2A?displayProperty=nameWithType> メソッドを使用します。

- **ニュートラルな日付/時刻値。** [日付データ型](../../../visual-basic/language-reference/data-types/date-data-type.md)には、常に日付と時刻の両方の情報が含まれています。 型変換の場合、Visual Basic は 1/1/0001 (1 年1月1日) を日付の*ニュートラル値*と見なし、00:00:00 (午前0時) はその時刻のニュートラル値と見なされます。 `Date` 値を文字列に変換する場合、`CStr` には、結果の文字列にニュートラル値は含まれません。 たとえば、`#January 1, 0001 9:30:00#` を文字列に変換した場合、結果は "9:30:00 AM" になります。日付情報は表示されません。 ただし、日付情報は依然として元の `Date` 値に存在し、<xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A> 関数などの関数を使用して回復できます。

- **カルチャの感度。** 文字列に関連する型変換関数は、アプリケーションの現在のカルチャ設定に基づいて変換を実行します。 たとえば、`CDate` は、システムのロケール設定に従って日付形式を認識します。 ロケールの正しい順序で日、月、年を指定する必要があります。指定しないと、日付が正しく解釈されない可能性があります。 "水曜日" など、曜日の文字列が含まれている場合、長い日付形式は認識されません。

     ロケールによって指定された形式以外の形式で、値の文字列形式との間で変換を行う必要がある場合は、Visual Basic 型変換関数を使用できません。 これを行うには、その値の型の `ToString(IFormatProvider)` および `Parse(String, IFormatProvider)` メソッドを使用します。 たとえば、文字列を `Double`に変換する場合は <xref:System.Double.Parse%2A?displayProperty=nameWithType> を使用し、`Double` 型の値を文字列に変換する場合は <xref:System.Double.ToString%2A?displayProperty=nameWithType> を使用します。

## <a name="ctype-function"></a>CType Function

[CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)は、2番目の引数 `typename`を受け取り、`expression` を強制的に `typename`に強制します。ここで、`typename` には、有効な変換が存在する任意のデータ型、構造体、クラス、またはインターフェイスを指定できます。

他の型変換キーワードと `CType` の比較については、「 [DirectCast operator](../../../visual-basic/language-reference/operators/directcast-operator.md) And [TryCast operator](../../../visual-basic/language-reference/operators/trycast-operator.md)」を参照してください。

## <a name="cbool-example"></a>CBool の例

次の例では、`CBool` 関数を使用して、式を `Boolean` の値に変換します。 式が0以外の値に評価された場合、`CBool` は `True`を返します。それ以外の場合は `False`を返します。

[!code-vb[VbVbalrFunctions#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#1)]

## <a name="cbyte-example"></a>CByte の例

次の例では、`CByte` 関数を使用して、式を `Byte`に変換します。

[!code-vb[VbVbalrFunctions#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#2)]

## <a name="cchar-example"></a>CChar の例

次の例では、`CChar` 関数を使用して、`String` 式の最初の文字を `Char` 型に変換します。

[!code-vb[VbVbalrFunctions#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#3)]

`CChar` の入力引数は、データ型 `Char` または `String`である必要があります。 `CChar` を使用して数値を文字に変換することはできません。これは `CChar` で数値データ型を使用できないためです。 次の例では、コードポイント (文字コード) を表す数値を取得し、それを対応する文字に変換します。 <xref:Microsoft.VisualBasic.Interaction.InputBox%2A> 関数を使用して、数字の文字列を取得し `CInt`、文字列を `Integer`型に変換し、`ChrW` して数値を `Char`型に変換します。

[!code-vb[VbVbalrFunctions#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#4)]

## <a name="cdate-example"></a>CDate の例

次の例では、`CDate` 関数を使用して、文字列を `Date` の値に変換します。 一般に、日付と時刻を文字列としてハードコーディングする (この例で示す) ことはお勧めしません。 代わりに、#Feb 12、1969年 #、#4: 45:23 PM # などの日付リテラルと時刻リテラルを使用します。

[!code-vb[VbVbalrFunctions#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#5)]

## <a name="cdbl-example"></a>CDbl の例

[!code-vb[VbVbalrFunctions#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#6)]

## <a name="cdec-example"></a>CDec の例

次の例では、`CDec` 関数を使用して、数値を `Decimal`に変換します。

[!code-vb[VbVbalrFunctions#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#7)]

## <a name="cint-example"></a>CInt の例

次の例では、`CInt` 関数を使用して、値を `Integer`に変換します。

[!code-vb[VbVbalrFunctions#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#8)]

## <a name="clng-example"></a>CLng の例

次の例では、`CLng` 関数を使用して、値を `Long`に変換します。

[!code-vb[VbVbalrFunctions#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#9)]

## <a name="cobj-example"></a>CObj の例

次の例では、`CObj` 関数を使用して、数値を `Object`に変換します。 `Object` 変数自体には、割り当てられた `Double` 値を指す4バイトのポインターのみが含まれています。

[!code-vb[VbVbalrFunctions#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#10)]

## <a name="csbyte-example"></a>CSByte の例

次の例では、`CSByte` 関数を使用して、数値を `SByte`に変換します。

[!code-vb[VbVbalrFunctions#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#11)]

## <a name="cshort-example"></a>CShort の例

次の例では、`CShort` 関数を使用して、数値を `Short`に変換します。

[!code-vb[VbVbalrFunctions#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#12)]

## <a name="csng-example"></a>CSng の例

次の例では、`CSng` 関数を使用して、値を `Single`に変換します。

[!code-vb[VbVbalrFunctions#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#13)]

## <a name="cstr-example"></a>CStr の例

次の例では、`CStr` 関数を使用して、数値を `String`に変換します。

[!code-vb[VbVbalrFunctions#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#14)]

次の例では、`CStr` 関数を使用して、`Date` 値を `String` 値に変換します。

[!code-vb[VbVbalrFunctions#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#15)]

`CStr` は常に、現在のロケールの標準の短い形式 ("6/15/2003 4:35:47 PM" など) で `Date` 値を表示します。 ただし、`CStr` では、日付と00:00:00 の時刻に対して1/1/0001 の*ニュートラル値*が抑制されます。

`CStr`によって返される値の詳細については、「 [CStr 関数の戻り値](../../../visual-basic/language-reference/functions/return-values-for-the-cstr-function.md)」を参照してください。

## <a name="cuint-example"></a>CUInt の例

次の例では、`CUInt` 関数を使用して、数値を `UInteger`に変換します。

[!code-vb[VbVbalrFunctions#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#16)]

## <a name="culng-example"></a>選別 Ng の例

次の例では、`CULng` 関数を使用して、数値を `ULong`に変換します。

[!code-vb[VbVbalrFunctions#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#17)]

## <a name="cushort-example"></a>CUShort の例

次の例では、`CUShort` 関数を使用して、数値を `UShort`に変換します。

[!code-vb[VbVbalrFunctions#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#18)]

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- <xref:Microsoft.VisualBasic.Strings.Chr%2A>
- <xref:Microsoft.VisualBasic.Strings.ChrW%2A>
- <xref:Microsoft.VisualBasic.Conversion.Int%2A>
- <xref:Microsoft.VisualBasic.Conversion.Fix%2A>
- <xref:Microsoft.VisualBasic.Strings.Format%2A>
- <xref:Microsoft.VisualBasic.Conversion.Hex%2A>
- <xref:Microsoft.VisualBasic.Conversion.Oct%2A>
- <xref:Microsoft.VisualBasic.Conversion.Str%2A>
- <xref:Microsoft.VisualBasic.Conversion.Val%2A>
- [変換関数](../../../visual-basic/language-reference/functions/conversion-functions.md)
- [Visual Basic における型変換](../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
