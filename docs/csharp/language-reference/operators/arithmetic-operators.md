---
title: 算術演算子 - C# リファレンス
description: 数値型を使用して乗算、除算、剰余、加算、減算の演算を実行する C# 演算子について学習します。
ms.date: 03/27/2019
author: pkulikov
f1_keywords:
- ++_CSharpKeyword
- --_CSharpKeyword
- '*_CSharpKeyword'
- /_CSharpKeyword
- '%_CSharpKeyword'
- +_CSharpKeyword
- -_CSharpKeyword
helpviewer_keywords:
- arithmetic operators [C#]
- increment operator [C#]
- ++ operator [C#]
- decrement operator [C#]
- -- operator [C#]
- multiplication operator [C#]
- '* operator [C#]'
- division operator [C#]
- / operator [C#]
- remainder operator [C#]
- '% operator [C#]'
- addition operator [C#]
- + operator [C#]
- subtraction operator [C#]
- '- operator [C#]'
ms.openlocfilehash: f03084fa611c35c5504190b28fab79563d560d03
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398065"
---
# <a name="arithmetic-operators-c-reference"></a>算術演算子 (C# リファレンス)

次の演算子は、数値型のオペランドを使用して算術演算を実行します。

- 単項演算子: [`++` (インクリメント)](#increment-operator-)、[`--` (デクリメント)](#decrement-operator---)、[`+` (プラス)](#unary-plus-and-minus-operators)、[`-` (マイナス)](#unary-plus-and-minus-operators)。
- 2 項演算子: [`*` (乗算)](#multiplication-operator-)、[`/` (除算)](#division-operator-)、[`%` (剰余)](#remainder-operator-)、[`+` (加算)](#addition-operator-)、[`-` (減算)](#subtraction-operator--)。

これらの演算子は、[整数](../builtin-types/integral-numeric-types.md)と[浮動小数点](../builtin-types/floating-point-numeric-types.md)のすべての数値型によってサポートされています。

## <a name="increment-operator-"></a>インクリメント演算子 ++

単項インクリメント演算子 `++` は、オペランドを 1 ずつインクリメントします。 このオペランドは、変数、[プロパティ](../../programming-guide/classes-and-structs/properties.md)のアクセス、または[インデクサー](../../programming-guide/indexers/index.md)のアクセスである必要があります。

インクリメント演算子は、後置インクリメント演算子である `x++` と、前置インクリメント演算子である`++x` という 2 つの形式でサポートされます。

### <a name="postfix-increment-operator"></a>後置インクリメント演算子

次の例に示すように、`x++` の結果は、演算子の "`x`前の *"*  値です。

[!code-csharp-interactive[postfix increment](snippets/ArithmeticOperators.cs#PostfixIncrement)]

### <a name="prefix-increment-operator"></a>前置インクリメント演算子

次の例に示すように、`++x` の結果は、演算子の "`x`後ろの *"*  値です。

[!code-csharp-interactive[prefix increment](snippets/ArithmeticOperators.cs#PrefixIncrement)]

## <a name="decrement-operator---"></a>デクリメント演算子 --

単項デクリメント演算子 `--` は、オペランドを 1 ずつデクリメントします。 このオペランドは、変数、[プロパティ](../../programming-guide/classes-and-structs/properties.md)のアクセス、または[インデクサー](../../programming-guide/indexers/index.md)のアクセスである必要があります。

デクリメント演算子は、後置デクリメント演算子である `x--` と、前置デクリメント演算子である `--x` という 2 つの形式でサポートされます。

### <a name="postfix-decrement-operator"></a>後置デクリメント演算子

次の例に示すように、`x--` の結果は、演算子の "`x`前の *"*  値です。

[!code-csharp-interactive[postfix decrement](snippets/ArithmeticOperators.cs#PostfixDecrement)]

### <a name="prefix-decrement-operator"></a>前置デクリメント演算子

次の例に示すように、`--x` の結果は、演算子の "`x`後ろの *"*  値です。

[!code-csharp-interactive[prefix decrement](snippets/ArithmeticOperators.cs#PrefixDecrement)]

## <a name="unary-plus-and-minus-operators"></a>単項プラス演算子と単項マイナス演算子

単項 `+` 演算子によって、そのオペランドの値が返されます。 単項 `-` 演算子は、そのオペランドの数値の否定を計算します。

[!code-csharp-interactive[unary plus and minus](snippets/ArithmeticOperators.cs#UnaryPlusAndMinus)]

[ulong](../builtin-types/integral-numeric-types.md) 型は、単項 `-` 演算子をサポートしません。

## <a name="multiplication-operator-"></a>乗算演算子 *

乗算演算子 (`*`) は、そのオペランドの積を計算します。

[!code-csharp-interactive[multiplication operator](snippets/ArithmeticOperators.cs#Multiplication)]

単項 `*` 演算子は、[ポインター間接参照演算子](pointer-related-operators.md#pointer-indirection-operator-)です。

## <a name="division-operator-"></a>除算演算子 /

除算演算子 `/` は、左側のオペランドを右側のオペランドで除算します。

### <a name="integer-division"></a>整数の除算

整数型のオペランドに対する `/` 演算子の結果は、整数型で、2 つのオペランドの商を 0 方向に丸めたものと等しくなります。

[!code-csharp-interactive[integer division](snippets/ArithmeticOperators.cs#IntegerDivision)]

2 つのオペランドの商を浮動小数点数として取得するには、`float`、`double`、または `decimal` 型を使います。

[!code-csharp-interactive[integer as floating-point division](snippets/ArithmeticOperators.cs#IntegerAsFloatingPointDivision)]

### <a name="floating-point-division"></a>浮動小数点の除算

`float`、`double`、`decimal` 型に対する `/` 演算子の結果は、2 つのオペランドの商となります。

[!code-csharp-interactive[floating-point division](snippets/ArithmeticOperators.cs#FloatingPointDivision)]

オペランドの 1 つが `decimal` であった場合、もう 1 つのオペランドを `float` や `double` にすることはできません。`float` と `double` は暗黙的に `decimal` に変換できないためです。 `float` または `double` オペランドは明示的に `decimal` 型に変換する必要があります。 数値型間の変換について詳しくは、[組み込みの数値変換](../builtin-types/numeric-conversions.md)に関するページをご覧ください。

## <a name="remainder-operator-"></a>剰余演算子 %

剰余演算子 `%` は、左側のオペランドを右側のオペランドで除算した後の剰余を計算します。

### <a name="integer-remainder"></a>整数の剰余

整数型のオペランドの場合、`a % b` の結果は `a - (a / b) * b` で生成される値になります。 0 以外の剰余の符号は、次の例で示されるように、左側のオペランドの符号と同じになります。

[!code-csharp-interactive[integer remainder](snippets/ArithmeticOperators.cs#IntegerRemainder)]

<xref:System.Math.DivRem%2A?displayProperty=nameWithType> メソッドを使用して、整数除算と剰余の結果の両方を計算します。

### <a name="floating-point-remainder"></a>浮動小数点の剰余

`float` オペランドと `double` オペランドの場合、有限の `x % y` と `x` の `y` の結果は、次のような値 `z` となります。

- `z` の符号は、0 以外の場合、`x` の符号と同じになります。
- `z` の絶対値は、`|x| - n * |y|` で生成される値となります。`n` は、`|x| / |y|` 以下で最も大きい整数であり、`|x|` と `|y|` はそれぞれ、`x` と `y` の絶対値です。

> [!NOTE]
> 剰余を計算するこの手法は、整数オペランドに使用される手法に類似していますが、IEEE 754 の仕様とは異なります。 IEEE 754 の仕様に準拠する剰余演算が必要な場合、<xref:System.Math.IEEERemainder%2A?displayProperty=nameWithType> メソッドを使用してください。

無限オペランドがある `%` 演算子の動作については、[C# 言語仕様](~/_csharplang/spec/expressions.md#remainder-operator)に関するページの「[剰余演算](~/_csharplang/spec/introduction.md)」セクションを参照してください。

`decimal` オペランドの場合、剰余演算子 `%` は [ 型の](<xref:System.Decimal.op_Modulus(System.Decimal,System.Decimal)>)剰余演算子<xref:System.Decimal?displayProperty=nameWithType>に等しくなります。

次の例では、浮動小数点オペランドを使用した剰余演算子の動作を示しています。

[!code-csharp-interactive[floating-point remainder](snippets/ArithmeticOperators.cs#FloatingPointRemainder)]

## <a name="addition-operator-"></a>加算演算子 +

加算演算子 `+` は、そのオペランドの合計を計算します。

[!code-csharp-interactive[addition operator](snippets/ArithmeticOperators.cs#Addition)]

文字列連結とデリゲートの組み合わせにも、`+` 演算子が使用できます。 詳細については、「[`+` および `+=` 演算子](addition-operator.md)」の記事を参照してください。

## <a name="subtraction-operator--"></a>減算演算子 -

減算演算子 `-` は、その左側のオペランドから右側のオペランドを減算します。

[!code-csharp-interactive[subtraction operator](snippets/ArithmeticOperators.cs#Subtraction)]

デリゲートの削除には、`-` 演算子を使用することもできます。 詳細については、「[`-` および `-=` 演算子](subtraction-operator.md)」の記事を参照してください。

## <a name="compound-assignment"></a>複合代入。

2 項演算子 `op` の場合、フォームの複合代入式

```csharp
x op= y
```

上記の式は、次の式と同じです。

```csharp
x = x op y
```

ただし、`x` が評価されるのは 1 回だけです。

次の例は、算術演算子を使用した複合代入の使用方法を示しています。

[!code-csharp-interactive[compound assignment](snippets/ArithmeticOperators.cs#CompoundAssignment)]

[数値の上位変換](~/_csharplang/spec/expressions.md#numeric-promotions)のため、`op` 演算の結果は、`T` の型 `x` に暗黙的に変換できない可能性があります。 そのような場合、`op` が定義済みの演算子であり、演算の結果が `T` の型 `x` に明示的に変換できる場合、`x op= y` の形式の複合代入式は、`x = (T)(x op y)` が 1 回だけ評価される点を除き、`x` と等価です。 次の例は、その動作を示します。

[!code-csharp-interactive[compound assignment with cast](snippets/ArithmeticOperators.cs#CompoundAssignmentWithCast)]

`+=`イベント`-=`のサブスクリプションとサブスクリプションの解除には、[ 演算子と ](../keywords/event.md) 演算子もそれぞれ使用できます。 詳細については、「[イベントのサブスクリプションとサブスクリプション解除を行う方法](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)」を参照してください。

## <a name="operator-precedence-and-associativity"></a>演算子の優先順位と結合規則

次の算術演算子の一覧は、優先度が高い順に並べられています。

- 後置インクリメント演算子 `x++` と後置デクリメント演算子 `x--`
- 前置インクリメント演算子 `++x` とデクリメント演算子 `--x`および単項演算子 `+` と `-`
- 乗算演算子 `*`、`/`、`%`
- 加法 `+` および `-` 演算子

2 項算術演算子は左結合です。 つまり、優先度が同じ演算子は、左から右に評価されます。

演算子の優先順位と結合規則によって定められた評価の順序を変更するには、かっこ `()` を使用します。

[!code-csharp-interactive[precedence and associativity](snippets/ArithmeticOperators.cs#PrecedenceAndAssociativity)]

優先度順に並べられた C# 演算子の完全な一覧については、「[C# 演算子](index.md#operator-precedence)」の記事の「[演算子の優先順位](index.md)」のセクションを参照してください。

## <a name="arithmetic-overflow-and-division-by-zero"></a>算術オーバーフローと 0 による除算

算術演算の結果が関係する数値型の有限値の範囲外にある場合は、算術演算子の動作は、そのオペランドの型に依存します。

### <a name="integer-arithmetic-overflow"></a>整数の算術オーバーフロー

0 による整数除算では、常に <xref:System.DivideByZeroException> がスローされます。

整数の算術オーバーフローの場合、オーバーフロー チェック コンテキスト ([checked または unchecked](../keywords/checked-and-unchecked.md)) が結果の動作を制御します。

- checked コンテキストでは、定数式でオーバーフローが発生すると、コンパイル時エラーが発生します。 それ以外の場合は、実行時に演算が実行されると <xref:System.OverflowException> がスローされます。
- unchecked コンテキストでは、結果の格納先の型に収まらない上位ビットが破棄されて、結果が切り詰められます。

[checked と unchecked](../keywords/checked-and-unchecked.md) のステートメントとともに、`checked` 演算子と `unchecked` 演算子を使用して、式が評価されるオーバーフロー チェック コンテキストを制御することができます。

[!code-csharp-interactive[checked and unchecked](snippets/ArithmeticOperators.cs#CheckedUnchecked)]

既定では、算術演算は *unchecked* コンテキストで発生します。

### <a name="floating-point-arithmetic-overflow"></a>浮動小数点演算のオーバーフロー

`float` 型と`double` 型を使用した算術演算では、例外はスローされません。 これらの型を使用した算術演算の結果は、無限および数値ではないことを表す特殊な値のいずれかになる可能性があります。

[!code-csharp-interactive[double non-finite values](snippets/ArithmeticOperators.cs#FloatingPointOverflow)]

`decimal` 型のオペランドの場合、算術オーバーフローは常に <xref:System.OverflowException> をスローし、0 による除算は常に <xref:System.DivideByZeroException> をスローします。

## <a name="round-off-errors"></a>丸め誤差

実数の浮動小数点表記と浮動小数点演算の一般的な制限事項により、浮動小数点型を使った計算で丸め誤差が生じる可能性があります。 つまり、式の生成された結果が、予期した数学的結果と異なる場合があります。 次の例は、こうしたいくつかのケースを示しています。

[!code-csharp-interactive[round-off errors](snippets/ArithmeticOperators.cs#RoundOffErrors)]

詳細については、[System.Double](/dotnet/api/system.double#remarks)、[System.Single](/dotnet/api/system.single#remarks)、または [System.Decimal](/dotnet/api/system.decimal#remarks) の参照ページの解説を参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は、単項算術演算子 ([、](operator-overloading.md)、`++`、`--`) と 2 項算術演算子 (`+`、`-`、`*`、`/`、`%`) を`+`オーバーロード`-`できます。 2 項演算子をオーバーロードすると、対応する複合代入演算子も暗黙的にオーバーロードされます。 ユーザー定義型は、複合代入演算子を明示的にオーバーロードすることはできません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [後置インクリメント演算子と後置デクリメント演算子](~/_csharplang/spec/expressions.md#postfix-increment-and-decrement-operators)
- [前置インクリメント演算子と前置デクリメント演算子](~/_csharplang/spec/expressions.md#prefix-increment-and-decrement-operators)
- [単項プラス演算子](~/_csharplang/spec/expressions.md#unary-plus-operator)
- [単項マイナス演算子](~/_csharplang/spec/expressions.md#unary-minus-operator)
- [乗算演算子](~/_csharplang/spec/expressions.md#multiplication-operator)
- [除算演算子](~/_csharplang/spec/expressions.md#division-operator)
- [剰余演算子](~/_csharplang/spec/expressions.md#remainder-operator)
- [加算演算子](~/_csharplang/spec/expressions.md#addition-operator)
- [減算演算子](~/_csharplang/spec/expressions.md#subtraction-operator)
- [複合代入](~/_csharplang/spec/expressions.md#compound-assignment)
- [checked 演算子と unchecked 演算子](~/_csharplang/spec/expressions.md#the-checked-and-unchecked-operators)
- [数値の上位変換](~/_csharplang/spec/expressions.md#numeric-promotions)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- <xref:System.Math?displayProperty=nameWithType>
- <xref:System.MathF?displayProperty=nameWithType>
- [.NET における数値](../../../standard/numerics.md)
