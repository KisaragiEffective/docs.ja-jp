---
title: データ型のトラブルシューティング
ms.date: 07/20/2015
helpviewer_keywords:
- Char data type [Visual Basic], converting
- Decimal data type [Visual Basic], conversions
- data types [Visual Basic], troubleshooting
- literals [Visual Basic], default types
- type characters [Visual Basic], literal
- Mod operator [Visual Basic], in floating-point operations
- troubleshooting Visual Basic, data types
- troubleshooting data types [Visual Basic]
- floating-point numbers [Visual Basic], precision
- Boolean data type [Visual Basic], converting
- literal types [Visual Basic]
- literal type characters [Visual Basic]
- floating-point numbers [Visual Basic], imprecision
- String data type [Visual Basic], converting
- floating-point numbers [Visual Basic], comparison
- floating-point numbers
ms.assetid: 90040d67-b630-4125-a6ae-37195b079042
ms.openlocfilehash: 63b2513e420320742bf7e25314743f08404f46a7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350513"
---
# <a name="troubleshooting-data-types-visual-basic"></a>データ型のトラブルシューティング (Visual Basic)

このページでは、組み込みデータ型に対して操作を実行するときに発生する可能性がある一般的な問題をいくつか示します。

## <a name="floating-point-expressions-do-not-compare-as-equal"></a>浮動小数点式は等しいと比較されません

浮動小数点数 ([単精度浮動小数点データ型(Single Data Type)](../../../../visual-basic/language-reference/data-types/single-data-type.md)と[倍精度浮動小数点データ型(Double Data Type)](../../../../visual-basic/language-reference/data-types/double-data-type.md)) を使用する場合は、それらがバイナリの分数として格納されていることに注意してください。 つまり、二進小数ではない（k /（2 ^ n）の形式で、k と n は整数）数量を正確に表すことはできません。 たとえば、0.5 (1/2 =) および 0.3125 (= 5/16) は正確な値として保持できますが、0.2（= 1/5）、0.3 （= 3/10）は 近似値にしかなりません。

このおける誤差により、浮動小数点値を操作するときに正確な結果を利用することはできません。 特に、理論的に等しい2つの値の表現は多少異なる場合があります。

| 浮動小数点の数量を比較するには |
|---|
|1. <xref:System> 名前空間の <xref:System.Math> クラスの <xref:System.Math.Abs%2A> メソッドを使用して、それらの差の絶対値を計算します。<br />2. 許容される最大の差を確認します。これにより、2つの数量を実際の目的のために均等にすることができます (差が大きくない場合)。<br />3. 差の絶対値と許容される差を比較します。|

次の例は、2つの `Double` 値の誤った比較と正しい比較を示しています。

[!code-vb[VbVbalrDataTypes#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#10)]

前の例では、<xref:System.Double> 構造の <xref:System.Double.ToString%2A> メソッドを使用して、`CStr` キーワードが使用するよりも高い精度を指定できるようにしています。 既定値は15桁ですが、"G17" の形式は17桁に拡張されます。

## <a name="mod-operator-does-not-return-accurate-result"></a>Mod 演算子が正確な結果を返しません

浮動小数点型ストレージのおける誤差のため、少なくとも1つのオペランドが浮動小数点である場合、 [Mod 演算子](../../../../visual-basic/language-reference/operators/mod-operator.md)は予期しない結果を返すことがあります。

[Decimal データ型](../../../../visual-basic/language-reference/data-types/decimal-data-type.md)では、浮動小数点表現は使用されません。 `Single` と `Double` では正確ではない多くの数値は `Decimal` では正確です (たとえば、0.2 と 0.3)。 浮動小数点型より `Decimal` では算術が遅くなりますが、精度を高めるためにパフォーマンスが低下する可能性があります。

|浮動小数点数値の剰余を求めるには|
|---|
|1. `Decimal`として変数を宣言します。<br />2. リテラルの型文字 `D` を使用して、`Long` データ型に対して値が大きすぎる場合に、リテラルを強制的に `Decimal`にします。|

次の例は、浮動小数点オペランドのおける誤差の可能性を示しています。

[!code-vb[VbVbalrDataTypes#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#11)]

前の例では、<xref:System.Double> 構造の <xref:System.Double.ToString%2A> メソッドを使用して、`CStr` キーワードが使用するよりも高い精度を指定できるようにしています。 既定値は15桁ですが、"G17" の形式は17桁に拡張されます。

`zeroPointTwo` が `Double`ので、0.2 の値は、0.20000000000000001 の値が格納されている無限の連続するバイナリ部分です。 2\.0 をこの数量で割ると、9.9999999999999995 が0.19999999999999991 になります。

`decimalRemainder`の式では、リテラルの型文字 `D` によって両方のオペランドの `Decimal`が強制され、0.2 の正確な表現が使用されます。 したがって、`Mod` 演算子により、0.0 の予想される剰余が得られます。

`decimalRemainder` を `Decimal`として宣言するのは十分ではないことに注意してください。 また、リテラルを強制的に `Decimal`するか、`Double` を既定で使用し、`decimalRemainder` は `doubleRemainder`と同じ不正確な値を受け取る必要があります。

## <a name="boolean-type-does-not-convert-to-numeric-type-accurately"></a>Boolean データで正確な数値型に変換されません。

[Boolean データ型](../../../../visual-basic/language-reference/data-types/boolean-data-type.md)の値は数値として格納されず、格納されている値は数値と等価であるとは見なされません。 以前のバージョンとの互換性のために、Visual Basic では、`Boolean` と数値型の間で変換を行うための変換キーワード ([CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)、`CBool`、`CInt`など) が用意されています。 ただし、その他の言語では、.NET Framework メソッドと同様に、これらの変換が異なる方法で実行されることがあります。

`True` と `False`の等価の数値に依存するコードを記述することは避けてください。 可能な限り、`Boolean` 変数の使用は、設計対象の論理値に制限する必要があります。 `Boolean` 値と数値を混在させる必要がある場合は、選択した変換方法を理解していることを確認してください。

### <a name="conversion-in-visual-basic"></a>Visual Basic での変換

`CType` または `CBool` の変換キーワードを使用して数値データ型を `Boolean`に変換すると、0が `False` になり、その他のすべての値が `True`になります。 変換キーワードを使用して `Boolean` 値を数値型に変換すると、`False` は0になり `True` は-1 になります。

### <a name="conversion-in-the-framework"></a>フレームワークでの変換

<xref:System> 名前空間の <xref:System.Convert> クラスの <xref:System.Convert.ToInt32%2A> メソッドは、`True` を + 1 に変換します。

`Boolean` 値を数値データ型に変換する必要がある場合は、使用する変換方法に注意してください。

## <a name="character-literal-generates-compiler-error"></a>文字リテラルによってコンパイラエラーが生成される

型文字が存在しない場合、Visual Basic はリテラルの既定のデータ型を想定します。 引用符 (`" "`) で囲まれた文字リテラルの既定の型は `String`です。

`String` データ型が[Char データ型](../../../../visual-basic/language-reference/data-types/char-data-type.md)に拡大変換されません。 つまり、`Char` 変数にリテラルを割り当てる場合は、縮小変換を行うか、リテラルを `Char` 型に強制する必要があります。

|変数または定数に代入する Char リテラルを作成するには|
|---|
|1. 変数または定数を `Char`として宣言します。<br />2. 文字の値を引用符で囲みます (`" "`)。<br />3. リテラルを強制的に `Char`するには、リテラルの型文字 `C` で終わりの二重引用符に従います。 これは、型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) が `On`であり、どのような場合でも望ましい場合に必要です。|

次の例では、`Char` 変数へのリテラルの正常な割り当てと成功した代入の両方を示します。

[!code-vb[VbVbalrDataTypes#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#12)]

実行時に失敗する可能性があるため、縮小変換の使用には常にリスクがあります。 たとえば、`String` 値に複数の文字が含まれている場合、`String` から `Char` への変換は失敗する可能性があります。 そのため、`C` 型文字を使用する方が適切なプログラミングです。

## <a name="string-conversion-fails-at-run-time"></a>String の変換は実行時に失敗します。

[文字列データ型](../../../../visual-basic/language-reference/data-types/string-data-type.md)は、非常に多くの拡大変換に関与します。 `String` 自体と `Object`にのみ拡大変換され、`Char` と `Char()` (`Char` 配列) のみが `String`に拡大変換されます。 これは、`String` の変数と定数に、他のデータ型に含めることができない値が含まれている可能性があるためです。

型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) が `On`場合、コンパイラは暗黙的な縮小変換をすべて許可しません。 これには、`String`に関係するものも含まれます。 コードでは、`CStr` や[CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)などの変換キーワードを使用して、変換を試行するように .NET Framework に指示することもできます。

> [!NOTE]
> `For Each…Next` コレクション内の要素から loop 制御変数への変換では、縮小変換エラーが抑制されます。 詳細と例については、 [For Each ... Next ステートメント](../../../../visual-basic/language-reference/statements/for-each-next-statement.md) の「縮小変換」セクションを参照してください。

### <a name="narrowing-conversion-protection"></a>縮小変換保護

縮小変換の欠点は、実行時にエラーが発生する可能性があることです。 たとえば、`String` 変数に "True" または "False" 以外のものが含まれている場合、その変数を `Boolean`に変換することはできません。 区切り文字が含まれている場合、任意の数値型への変換は失敗します。 `String` 変数が、変換先の型で受け入れ可能な値を常に保持していることがわかっている場合を除き、変換を試行しないでください。

`String` から別のデータ型に変換する必要がある場合、最も安全な手順は、試行された変換を [Try...Catch...Finally ステートメント](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md) で囲むことです。 これにより、実行時エラーに対処できます。

### <a name="character-arrays"></a>文字配列

1つの `Char` と `Char` 要素の配列の両方が `String`に拡大されます。 ただし、`String` は `Char()`に拡大されません。 `String` 値を `Char` 配列に変換するには、<xref:System.String?displayProperty=nameWithType> クラスの <xref:System.String.ToCharArray%2A> メソッドを使用します。

### <a name="meaningless-values"></a>意味のない値

一般に、`String` の値は他のデータ型では意味がなく、変換は非常に人為的で危険です。 可能な限り、`String` 変数の使用を、デザインされた文字シーケンスに制限する必要があります。 他の型の同等の値に依存するコードを記述することは避けてください。

## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [型文字](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [Visual Basic での型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [データ型の有効な使用方法](../../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
