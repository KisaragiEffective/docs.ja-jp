---
title: ビットごとの演算子とシフト演算子 - C# リファレンス
description: 整数型のオペランドに対してビットごとの論理演算またはシフト演算を実行する C# の演算子について説明します。
ms.date: 04/18/2019
author: pkulikov
f1_keywords:
- ~_CSharpKeyword
- <<_CSharpKeyword
- '>>_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
helpviewer_keywords:
- bitwise logical operators [C#]
- shift operators [C#]
- tilde operator [C#]
- one's complement operator [C#]
- bitwise complement operator [C#]
- ~ operator [C#]
- left shift operator [C#]
- << operator [C#]
- right shift operator [C#]
- '>> operator [C#]'
- bitwise logical AND operator [C#]
- ampersand operator [C#]
- '& operator [C#]'
- bitwise logical exclusive OR operator [C#]
- bitwise logical XOR operator [C#]
- ^ operator [C#]
- bitwise logical OR operator [C#]
- '| operator [C#]'
ms.openlocfilehash: 54198368672e0c9324210a232c7851b5a90402cb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398071"
---
# <a name="bitwise-and-shift-operators-c-reference"></a>ビットごとの演算子とシフト演算子 (C# リファレンス)

以下の演算子では、[整数型](../builtin-types/integral-numeric-types.md)または [char](../builtin-types/char.md) 型のオペランドに対してビットごとの演算またはシフト演算が実行されます。

- 単項 [`~` (ビットごとの補数)](#bitwise-complement-operator-) 演算子
- 2 項 [`<<` (左シフト)](#left-shift-operator-) および [`>>` (右シフト)](#right-shift-operator-) シフト演算子
- 2 項 [`&` (論理 AND)](#logical-and-operator-)、[`|` (論理 OR)](#logical-or-operator-)、および [`^` (論理排他的 OR)](#logical-exclusive-or-operator-) 演算子

これらの演算子は、`int`、`uint`、`long`、`ulong` 型に対して定義されています。 両方のオペランドが他の整数型 (`sbyte`、`byte`、`short`、`ushort`、`char`) の場合、それらの値は `int` 型に変換され、演算の結果もその型になります。 オペランドが異なる整数型の場合、それらの値は最も近い含んでいる整数型に変換されます。 詳しくは、「[C# 言語仕様](~/_csharplang/spec/expressions.md#numeric-promotions)」の「[数値の上位変換](~/_csharplang/spec/introduction.md)」セクションをご覧ください。

`&`、`|`、`^` の各演算子は、`bool` 型のオペランドに対しても定義されています。 詳しくは、「[ブール論理演算子](boolean-logical-operators.md)」をご覧ください。

ビットごとの演算子およびシフト演算が原因でオーバーフローが発生することはなく、[checked と unchecked](../keywords/checked-and-unchecked.md) のコンテキストで同じ結果が生成されることはありません。

## <a name="bitwise-complement-operator-"></a>ビットごとの補数演算子 ~

`~` 演算子では、各ビットを反転させることにより、オペランドのビットごとの補数が生成されます。

[!code-csharp-interactive[bitwise NOT](snippets/BitwiseAndShiftOperators.cs#BitwiseComplement)]

`~` シンボルはファイナライザーの宣言にも使用できます。 詳細については、「[Finalizers](../../programming-guide/classes-and-structs/destructors.md)」 (ファイナライザー) を参照してください。

## <a name="left-shift-operator-"></a>左シフト演算子 \<\<

`<<` 演算子では、左側のオペランドが、[右側のオペランドで定義されたビット数](#shift-count-of-the-shift-operators)だけ左にシフトされます。

次の例に示すように、左シフト演算子では、結果の型の範囲外にある上位ビットは破棄され、空の下位ビット位置は、ゼロに設定されます。

[!code-csharp-interactive[left shift](snippets/BitwiseAndShiftOperators.cs#LeftShift)]

シフト演算子は `int`、`uint`、`long`、`ulong` 型に対してのみ定義されるので、演算の結果には常に少なくとも 32 ビットが含まれます。 左側のオペランドが別の整数型 (`sbyte`、`byte`、`short`、`ushort`、`char`) の場合、次の例で示すように、その値は `int` 型に変換されます。

[!code-csharp-interactive[left shift with promotion](snippets/BitwiseAndShiftOperators.cs#LeftShiftPromoted)]

`<<` 演算子の右側のオペランドでのシフト数の定義方法については、「[シフト演算子のシフト数](#shift-count-of-the-shift-operators)」セクションをご覧ください。

## <a name="right-shift-operator-"></a>右シフト演算子 >>

`>>` 演算子では、左側のオペランドが、[右側のオペランドで定義されたビット数](#shift-count-of-the-shift-operators)だけ右にシフトされます。

次の例で示すように、右シフト演算では、下位ビットが破棄されます。

[!code-csharp-interactive[right shift](snippets/BitwiseAndShiftOperators.cs#RightShift)]

空の上位ビット位置は、左側のオペランドの型に基づいて次のように設定されます。

- 左側のオペランドの型が `int` または `long` である場合、右シフト演算子では、"*算術*" シフトが実行されます: 左側のオペランドの最上位ビット (符号ビット) の値が空の上位ビット位置に反映されます。 つまり、左側のオペランドが負でない場合は空の上位ビット位置が 0 に設定され、負の場合は 1 に設定されます。

  [!code-csharp-interactive[arithmetic right shift](snippets/BitwiseAndShiftOperators.cs#ArithmeticRightShift)]

- 左側のオペランドの型が `uint` または `ulong` である場合、右シフト演算子では、"*論理*" シフトが実行されます: 空の上位ビット位置は常に 0 に設定されます。

  [!code-csharp-interactive[logical right shift](snippets/BitwiseAndShiftOperators.cs#LogicalRightShift)]

`>>` 演算子の右側のオペランドでのシフト数の定義方法については、「[シフト演算子のシフト数](#shift-count-of-the-shift-operators)」セクションをご覧ください。

## <a name="logical-and-operator-"></a> 論理 AND 演算子 &amp;

`&` 演算子では、そのオペランドのビットごとの論理 AND が計算されます。

[!code-csharp-interactive[bitwise AND](snippets/BitwiseAndShiftOperators.cs#BitwiseAnd)]

`bool` オペランドの場合、`&` 演算子がそのオペランドの[論理 AND](boolean-logical-operators.md#logical-and-operator-) を計算します。 単項 `&` 演算子は[アドレス演算子](pointer-related-operators.md#address-of-operator-)です。

## <a name="logical-exclusive-or-operator-"></a>論理排他的 OR 演算子: ^

`^` 演算子では、そのオペランドのビットごとの論理排他的 OR (ビットごとの論理 XOR とも呼ばれます) が計算されます。

[!code-csharp-interactive[bitwise XOR](snippets/BitwiseAndShiftOperators.cs#BitwiseXor)]

`bool` オペランドの場合、`^` 演算子がそのオペランドの[論理排他的 OR](boolean-logical-operators.md#logical-exclusive-or-operator-) を計算します。

## <a name="logical-or-operator-"></a>論理 OR 演算子 |

`|` 演算子では、そのオペランドのビットごとの論理 OR が計算されます。

[!code-csharp-interactive[bitwise OR](snippets/BitwiseAndShiftOperators.cs#BitwiseOr)]

`bool` オペランドの場合、`|` 演算子がそのオペランドの[論理 OR](boolean-logical-operators.md#logical-or-operator-) を計算します。

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

次の例では、ビットごとの演算子およびシフト演算子を使った複合代入の使用方法を示します。

[!code-csharp-interactive[compound assignment](snippets/BitwiseAndShiftOperators.cs#CompoundAssignment)]

[数値の上位変換](~/_csharplang/spec/expressions.md#numeric-promotions)のため、`op` 演算の結果は、`T` の型 `x` に暗黙的に変換できない可能性があります。 そのような場合、`op` が定義済みの演算子であり、演算の結果が `T` の型 `x` に明示的に変換できる場合、`x op= y` の形式の複合代入式は、`x = (T)(x op y)` が 1 回だけ評価される点を除き、`x` と等価です。 次の例は、その動作を示します。

[!code-csharp-interactive[compound assignment with cast](snippets/BitwiseAndShiftOperators.cs#CompoundAssignmentWithCast)]

## <a name="operator-precedence"></a>演算子の優先順位

次のビットごとの演算子およびシフト演算子の一覧は、優先度が高い順に並べられています。

- ビットごとの補数演算子 `~`
- シフト演算子 `<<` および `>>`
- 論理 AND 演算子 `&`
- 論理排他的 OR 演算子 `^`
- 論理 OR 演算子 `|`

演算子の優先順位によって定められた評価の順序を変更するには、かっこ `()` を使用します。

[!code-csharp-interactive[operator precedence](snippets/BitwiseAndShiftOperators.cs#Precedence)]

優先度順に並べられた C# 演算子の完全な一覧については、「[C# 演算子](index.md#operator-precedence)」の記事の「[演算子の優先順位](index.md)」のセクションを参照してください。

## <a name="shift-count-of-the-shift-operators"></a>シフト演算子のシフト数

シフト演算子 `<<` および `>>` の場合、右側のオペランドの型は、`int` であるか、または [ への](../builtin-types/numeric-conversions.md#implicit-numeric-conversions)事前に定義された暗黙的な数値変換`int`を持つ型にする必要があります。

`x << count` および `x >> count` の式では、実際のシフト数は次のように `x` の型によって異なります。

- `x` の型が `int` または `uint` である場合、シフト数は、右側のオペランドの下位 *5* ビットで定義されます。 つまり、シフト数は `count & 0x1F` (または `count & 0b_1_1111`) から計算されます。

- `x` の型が `long` または `ulong` である場合、シフト数は、右側のオペランドの下位 *6* ビットで定義されます。 つまり、シフト数は `count & 0x3F` (または `count & 0b_11_1111`) から計算されます。

次の例は、その動作を示します。

[!code-csharp-interactive[shift count example](snippets/BitwiseAndShiftOperators.cs#ShiftCount)]

> [!NOTE]
> 前の例で示したように、右側のオペランドの値が左側のオペランドのビット数よりも大きい場合でも、シフト演算の結果が 0 以外になることがあります。

## <a name="enumeration-logical-operators"></a>列挙論理演算子

`~`、`&`、`|`、`^` の演算子は、任意の[列挙](../builtin-types/enum.md)型でもサポートされます。 オペランドが同じ列挙型の場合、基になっている整数型の対応する値に対して、論理演算が実行されます。 たとえば、基になる型が `x` である列挙型 `y` の任意の `T` と `U` に対して、式 `x & y` では式 `(T)((U)x & (U)y)` と同じ結果が生成されます。

通常、ビットごとの論理演算子は、[Flags](xref:System.FlagsAttribute) 属性で定義されている列挙型で使います。 詳しくは、「[列挙型](../builtin-types/enum.md#enumeration-types-as-bit-flags)」記事の「[ビット フラグとしての列挙型](../builtin-types/enum.md)」セクションをご覧ください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型では、[、](operator-overloading.md)、`~`、`<<`、`>>`、`&` の各演算子を`|`オーバーロード`^`できます。 2 項演算子をオーバーロードすると、対応する複合代入演算子も暗黙的にオーバーロードされます。 ユーザー定義型は、複合代入演算子を明示的にオーバーロードすることはできません。

ユーザー定義型 `T` で `<<` または `>>` 演算子をオーバーロードする場合、左側のオペランドの型は `T` である必要があり、右側のオペランドの型は `int` である必要があります。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [ビットごとの補数演算子](~/_csharplang/spec/expressions.md#bitwise-complement-operator)
- [シフト演算子](~/_csharplang/spec/expressions.md#shift-operators)
- [論理演算子](~/_csharplang/spec/expressions.md#logical-operators)
- [複合代入](~/_csharplang/spec/expressions.md#compound-assignment)
- [数値の上位変換](~/_csharplang/spec/expressions.md#numeric-promotions)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ブール論理演算子](boolean-logical-operators.md)
