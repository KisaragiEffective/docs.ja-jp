---
title: ブール論理演算子 - C# リファレンス
description: ブール オペランドを使用した論理否定演算、論理積演算 (AND)、および包含的および排他的論理和演算 (OR) を実行する C# 演算子について説明します。
ms.date: 09/27/2019
author: pkulikov
f1_keywords:
- '!_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
- '&&_CSharpKeyword'
- '||_CSharpKeyword'
helpviewer_keywords:
- logical operators [C#]
- short-circuiting operators [C#]
- logical negation operator [C#]
- NOT operator [C#]
- '! operator [C#]'
- AND operator [C#]
- ampersand operator [C#]
- conjunction operator [C#]
- '& operator [C#]'
- exclusive OR operator [C#]
- XOR operator [C#]
- ^ operator [C#]
- OR operator [C#]
- disjunction operator [C#]
- '| operator [C#]'
- conditional AND operator [C#]
- short-circuiting AND operator [C#]
- '&& operator [C#]'
- conditional OR operator [C#]
- short-circuiting OR operator [C#]
- '|| operator [C#]'
ms.openlocfilehash: e4efb283c835a703ec64b6ec5995b821c995dc60
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2019
ms.locfileid: "74552491"
---
# <a name="boolean-logical-operators-c-reference"></a>ブール論理演算子 (C# リファレンス)

次の演算子は、[bool](../builtin-types/bool.md) オペランドを使用して論理演算を実行します。

- 単項 [`!` (論理否定)](#logical-negation-operator-) 演算子。
- 二項 [`&` (論理 AND)](#logical-and-operator-)、[`|` (論理 OR)](#logical-or-operator-)、および [`^` (論理排他的 OR)](#logical-exclusive-or-operator-) 演算子。 これらの演算子は常に両方のオペランドを評価します。
- 二項 [`&&` (条件付き論理 AND)](#conditional-logical-and-operator-) および [`||` (条件付き論理 OR)](#conditional-logical-or-operator-) 演算子。 これらの演算子では、必要な場合にのみ右側のオペランドが評価されます。

[整数の数値型](../builtin-types/integral-numeric-types.md)のオペランドの場合、`&`、`|`、`^` 演算子でビットごとの論理演算を実行します。 詳しくは、「[ビットごとの演算子とシフト演算子](bitwise-and-shift-operators.md)」をご覧ください。

## <a name="logical-negation-operator-"></a>論理否定演算子 !

単項の接頭辞 `!` 演算子では、そのオペランドの論理否定が計算されます。 つまり、オペランドが `false` と評価された場合は `true`、オペランドが `true` と評価された場合は `false` が生成されます。

[!code-csharp-interactive[logical negation](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Negation)]

C# 8.0 以降では、単項後置の `!` 演算子は [null 免除演算子](null-forgiving.md)です。

## <a name="logical-and-operator-"></a> 論理 AND 演算子 &amp;

`&` 演算子がそのオペランドの論理 AND を計算します。 `x` と `y` の両方が `true` と評価されれば、`x & y` の結果は `true` です。 それ以外の場合、結果は `false` です。

`&` 演算子は、左側のオペランドが `false` と評価されても両方のオペランドを評価するため、操作結果は右側のオペランドの値に関係なく、`false` になります。

次の例では、`&` 演算子の右側のオペランドはメソッド呼び出しで、左側のオペランドの値に関係なく実行されます。

[!code-csharp-interactive[logical AND](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#And)]

[条件付き論理 AND 演算子](#conditional-logical-and-operator-) `&&` もそのオペランドの論理 AND を計算しますが、左側のオペランドが `false` と評価された場合、右側のオペランドは評価されません。

[整数の数値型](../builtin-types/integral-numeric-types.md)のオペランドの場合、`&` 演算子でそのオペランドの[ビットごとの論理 AND](bitwise-and-shift-operators.md#logical-and-operator-) を計算します。 単項 `&` 演算子は[アドレス演算子](pointer-related-operators.md#address-of-operator-)です。

## <a name="logical-exclusive-or-operator-"></a>論理排他的 OR 演算子: ^

`^` 演算子は、そのオペランドの論理排他的 OR (論理 XOR とも呼ばれます) を計算します。 `x` が `true` に評価され、`y` が `false` に評価された場合、または `x` が `false` に評価され、`y` が `true` に評価された場合、`x ^ y` の結果は `true` です。 それ以外の場合、結果は `false` です。 つまり、`bool` オペランドの場合、`^` 演算子は[非等値演算子](equality-operators.md#inequality-operator-) `!=` と同じ結果を計算します。

[!code-csharp-interactive[logical exclusive OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Xor)]

[整数の数値型](../builtin-types/integral-numeric-types.md)のオペランドの場合、`^` 演算子でそのオペランドの[ビットごとの論理排他的 OR](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) を計算します。

## <a name="logical-or-operator-"></a>論理 OR 演算子 |

`|` 演算子がそのオペランドの論理 OR を計算します。 `x` または `y` のどちらかが `true` と評価された場合、`x | y` の結果は `true` になります。 それ以外の場合、結果は `false` です。

`|` 演算子は、左側のオペランドが `true` と評価されても両方のオペランドを評価するため、操作結果は右側のオペランドの値に関係なく、`true` になります。

次の例では、`|` 演算子の右側のオペランドはメソッド呼び出しで、左側のオペランドの値に関係なく実行されます。

[!code-csharp-interactive[logical OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Or)]

[条件付き論理 OR 演算子](#conditional-logical-or-operator-) `||` もそのオペランドの論理 OR を計算しますが、左側のオペランドが `true` と評価された場合、右側のオペランドは評価されません。

[整数の数値型](../builtin-types/integral-numeric-types.md)のオペランドの場合、`|` 演算子でそのオペランドの[ビットごとの論理 OR](bitwise-and-shift-operators.md#logical-or-operator-) を計算します。

## <a name="conditional-logical-and-operator-"></a> 条件付き論理 AND 演算子 &amp;&amp;

条件論理 AND 演算子 `&&` は、"短絡" 論理 AND 演算子とも呼ばれ、そのオペランドの論理 AND を計算します。 `x` と `y` の両方が `true` と評価されれば、`x && y` の結果は `true` です。 それ以外の場合、結果は `false` です。 `x` が `false` に評価される場合、`y` は評価されません。

次の例では、`&&` 演算子の右側のオペランドはメソッド呼び出しで、左側のオペランドが `false` と評価されると実行されません。

[!code-csharp-interactive[conditional logical AND](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#ConditionalAnd)]

[論理 AND 演算子](#logical-and-operator-) `&` もそのオペランドの論理 AND を計算しますが、常に両方のオペランドを評価します。

## <a name="conditional-logical-or-operator-"></a>条件付き論理 OR 演算子 ||

条件論理 OR 演算子 `||` は、"短絡" 論理 OR 演算子とも呼ばれ、そのオペランドの論理 OR を計算します。 `x` または `y` のどちらかが `true` と評価された場合、`x || y` の結果は `true` になります。 それ以外の場合、結果は `false` です。 `x` が `true` に評価される場合、`y` は評価されません。

次の例では、`||` 演算子の右側のオペランドはメソッド呼び出しで、左側のオペランドが `true` と評価されると実行されません。

[!code-csharp-interactive[conditional logical OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#ConditionalOr)]

[論理 OR 演算子](#logical-or-operator-) `|` もそのオペランドの論理 OR を計算しますが、常に両方のオペランドを評価します。

## <a name="nullable-boolean-logical-operators"></a>null 許容ブール論理演算子

`bool?` オペランドの場合、`&` 演算子と `|` 演算子では 3 値ロジックをサポートします。 次の表に、これらの演算子のセマンティクスが定義されています。  
  
|x|Y|x&y|x&#124;y|  
|----|----|----|----|  
|true|true|true|true|  
|TRUE|False|false|true|  
|true|null|null|true|  
|False|true|False|true|  
|False|False|False|False|  
|False|null|False|null|  
|null|true|null|true|  
|null|False|False|null|  
|null|null|null|null|  

これらの演算子の動作は、null 許容値型の一般的な演算子の動作とは異なります。 通常、値型のオペランドに定義されている演算子も、対応する null 値型のオペランドと共に使用できます。 このような演算子では、そのオペランドのいずれかが `null` として評価される場合に `null` を生成します。 しかし、`&` および `|` 演算子は、オペランドの 1 つが `null` として評価される場合でも、null 以外の値を生成する可能性があります。 null 許容値型の演算子の動作については、[null 許容値型](../builtin-types/nullable-value-types.md)に関する記事の「[リフト演算子](../builtin-types/nullable-value-types.md#lifted-operators)」セクションを参照してください。

次の例に示すように、`!` 演算子と `^` 演算子を `bool?` オペランドと共に使用することもできます。

[!code-csharp-interactive[lifted negation and xor](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#WithNullableBoolean)]

条件付き論理演算子 `&&` と `||` では、`bool?` オペランドをサポートしません。

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

次の例に示すように、`&`、`|`、および `^` 演算子は複合代入をサポートします。

[!code-csharp-interactive[compound assignment](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#CompoundAssignment)]

条件付き論理演算子 `&&` と `||` は複合代入をサポートしません。

## <a name="operator-precedence"></a>演算子の優先順位

次の論理演算子の一覧は、優先度が高い順に並べられています。

- 論理否定演算子 `!`
- 論理 AND 演算子 `&`
- 論理排他的 OR 演算子 `^`
- 論理 OR 演算子 `|`
- 条件付き論理 AND 演算子 `&&`
- 条件付き論理 OR 演算子 `||`

演算子の優先順位によって定められた評価の順序を変更するには、かっこ `()` を使用します。

[!code-csharp-interactive[operator precedence](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Precedence)]

優先度順に並べられた C# 演算子の完全な一覧については、[C# 演算子](index.md)に関する記事の「[演算子の優先順位](index.md#operator-precedence)」セクションを参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は、`!`、`&`、`|`、および `^` 演算子を[オーバーロード](operator-overloading.md)できます。 2 項演算子をオーバーロードすると、対応する複合代入演算子も暗黙的にオーバーロードされます。 ユーザー定義型は、複合代入演算子を明示的にオーバーロードすることはできません。

ユーザー定義型は条件付き論理演算子 `&&` と `||` をオーバーロードできません。 ただし、ユーザー定義型が [true および false 演算子](true-false-operators.md)と `&` または `|` 演算子を特定の方法でオーバーロードしている場合は、その型のオペランドに対してそれぞれ `&&` または `||` 演算の評価が可能になります。 詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の[ユーザー定義型条件論理演算子](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)に関するセクションを参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [論理否定演算子](~/_csharplang/spec/expressions.md#logical-negation-operator)
- [論理演算子](~/_csharplang/spec/expressions.md#logical-operators)
- [条件論理演算子](~/_csharplang/spec/expressions.md#conditional-logical-operators)
- [複合代入](~/_csharplang/spec/expressions.md#compound-assignment)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ビットごとの演算子とシフト演算子](bitwise-and-shift-operators.md)
