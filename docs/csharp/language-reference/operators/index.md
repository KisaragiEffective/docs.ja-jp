---
title: C# 演算子 - C# リファレンス
ms.date: 08/20/2019
f1_keywords:
- cs.operators
helpviewer_keywords:
- operators [C#]
- operator precedence [C#]
- operator associativity [C#]
- expressions [C#]
ms.assetid: 0301e31f-22ad-49af-ac3c-d5eae7f0ac43
ms.openlocfilehash: 11c544e7fc923b0820141fb2e096ef7707f0a95f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74552473"
---
# <a name="c-operators-c-reference"></a>C# 演算子 (C# リファレンス)

C# では、組み込み型でサポートされた演算子が多数提供されています。 たとえば、[算術演算子](arithmetic-operators.md)は数値オペランドで算術演算を実行し、[ブール論理演算子](boolean-logical-operators.md)は [bool](../builtin-types/bool.md) オペランドで論理演算を実行します。 特定の演算子は[オーバーロード](operator-overloading.md)できます。 演算子のオーバーロードを利用すると、ユーザー定義型のオペランドに対して演算子の動作を指定できます。

[式](../../programming-guide/statements-expressions-operators/expressions.md)では、演算子の優先順位と結合規則によって、操作の実行順序が決まります。 かっこを使用すれば、演算子の優先順位と結合規則によって定められた評価の順序を変更することができます。

## <a name="operator-precedence"></a>演算子の優先順位

複数の演算子を含む式では、優先順位の高い方の演算子が優先順位の低い方の演算子よりも先に評価されます。 次の例では、乗算は加算より優先順位が高いため、最初に乗算が実行されます。

```csharp-interactive
var a = 2 + 2 * 2;
Console.WriteLine(a); //  output: 6
```

演算子の優先順位によって定められた評価の順序を変更するには、かっこを使用します。

```csharp-interactive
var a = (2 + 2) * 2;
Console.WriteLine(a); //  output: 8
```

次の表は、C# の演算子を優先順位の高い順にまとめたものです。 各行内の演算子の優先順位は同じです。

| オペレーター | カテゴリまたは名前 |
| --------- | ---------------- |
| [x.y](member-access-operators.md#member-access-operator-)、[x?.y](member-access-operators.md#null-conditional-operators--and-)、[x?[y]](member-access-operators.md#null-conditional-operators--and-)、[f(x)](member-access-operators.md#invocation-operator-)、[a&#91;i&#93;](member-access-operators.md#indexer-operator-)、[x++](arithmetic-operators.md#increment-operator-)、[x--](arithmetic-operators.md#decrement-operator---)、[new](new-operator.md)、[typeof](type-testing-and-cast.md#typeof-operator)、[checked](../keywords/checked.md)、[unchecked](../keywords/unchecked.md)、[default](default.md)、[nameof](nameof.md)、[delegate](delegate-operator.md)、[sizeof](sizeof.md)、[stackalloc](stackalloc.md)、[x->y](pointer-related-operators.md#pointer-member-access-operator--) | 1 次式 |
| [+x](arithmetic-operators.md#unary-plus-and-minus-operators)、[-x](arithmetic-operators.md#unary-plus-and-minus-operators)、[\!x](boolean-logical-operators.md#logical-negation-operator-)、[~x](bitwise-and-shift-operators.md#bitwise-complement-operator-)、[++x](arithmetic-operators.md#increment-operator-)、[--x](arithmetic-operators.md#decrement-operator---)、[^x](member-access-operators.md#index-from-end-operator-)、[(T)x](type-testing-and-cast.md#cast-operator-)、[await](await.md)、[&x](pointer-related-operators.md#address-of-operator-)、[*x](pointer-related-operators.md#pointer-indirection-operator-)、[true and false](true-false-operators.md) | 単項演算子 |
| [x..y](member-access-operators.md#range-operator-) | 範囲 |
| [x * y](arithmetic-operators.md#multiplication-operator-)、[x / y](arithmetic-operators.md#division-operator-)、[x % y](arithmetic-operators.md#remainder-operator-) | 乗算|
| [x + y](arithmetic-operators.md#addition-operator-)、[x – y](arithmetic-operators.md#subtraction-operator--) | 加法 |
| [x \<\<  y](bitwise-and-shift-operators.md#left-shift-operator-)、[x >> y](bitwise-and-shift-operators.md#right-shift-operator-) | Shift |
| [x \< y](comparison-operators.md#less-than-operator-)、[x > y](comparison-operators.md#greater-than-operator-)、[x \<= y](comparison-operators.md#less-than-or-equal-operator-)、[x >= y](comparison-operators.md#greater-than-or-equal-operator-)、[is](type-testing-and-cast.md#is-operator)、[as](type-testing-and-cast.md#as-operator) | 関係式と型検査 |
| [x == y](equality-operators.md#equality-operator-), [x != y](equality-operators.md#inequality-operator-) | 等価比較 |
| `x & y` | [ブール演算の論理 AND](boolean-logical-operators.md#logical-and-operator-) または[ビット演算の論理 AND](bitwise-and-shift-operators.md#logical-and-operator-) |
| `x ^ y` | [ブール演算の論理 XOR](boolean-logical-operators.md#logical-exclusive-or-operator-) または[ビット演算の論理 XOR](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) |
| <code>x &#124; y</code> | [ブール演算の論理 OR](boolean-logical-operators.md#logical-or-operator-) または[ビット演算の論理 OR](bitwise-and-shift-operators.md#logical-or-operator-) |
| [x && y](boolean-logical-operators.md#conditional-logical-and-operator-) | 条件 AND |
| [x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-) | 条件 OR |
| [x ?? y](null-coalescing-operator.md) | Null 合体演算子 |
| [c ? t : f](conditional-operator.md) | 条件演算子 |
| [x = y](assignment-operator.md)、[x += y](arithmetic-operators.md#compound-assignment)、[x -= y](arithmetic-operators.md#compound-assignment)、[x *= y](arithmetic-operators.md#compound-assignment)、[x /= y](arithmetic-operators.md#compound-assignment)、[x %= y](arithmetic-operators.md#compound-assignment)、[x &= y](boolean-logical-operators.md#compound-assignment)、[x &#124;= y](boolean-logical-operators.md#compound-assignment)、[x ^= y](boolean-logical-operators.md#compound-assignment)、[x <<= y](bitwise-and-shift-operators.md#compound-assignment)、[x >>= y](bitwise-and-shift-operators.md#compound-assignment)、[x ??= y](null-coalescing-operator.md)、[=>](lambda-operator.md) | 代入とラムダ宣言 |

## <a name="operator-associativity"></a>演算子の結合規則

演算子の優先順位が同じ場合は、演算子の結合規則によって、操作の実行順序が決まります。

- *結合規則が左から右*の演算子は、左から右に順番に評価されます。 [代入演算子](assignment-operator.md)と [null 合体演算子](null-coalescing-operator.md)を除き、2 項演算子はすべて左からの結合です。 たとえば、 `a + b - c` は `(a + b) - c`と評価されます。
- *結合規則が右から左*の演算子は、右から左に評価されます。 代入演算子、null 合体演算子、および[条件演算子`?:`](conditional-operator.md)は、右からの結合です。 たとえば、 `x = y = z` は `x = (y = z)`と評価されます。

演算子の結合規則によって定められた評価の順序を変更するには、かっこを使用します。

```csharp-interactive
int a = 13 / 5 / 2;
int b = 13 / (5 / 2);
Console.WriteLine($"a = {a}, b = {b}");  // output: a = 1, b = 6
```

## <a name="operand-evaluation"></a>オペランドの評価

式内のオペランドは、演算子の優先順位と結合規則に関係なく、左から右に評価されます。 次の例では、演算子とオペランドが評価される順序を示しています。

| 正規表現 | 評価の順序 |
| ---------- | ------------------- |
|`a + b`|a、b、+|
|`a + b * c`|a、b、c、*、+|
|`a / b + c * d`|a、b、/、c、d、*、+|
|`a / (b + c) * d`|a、b、c、+、/、d、*|

通常、演算子のオペランドはすべて評価されます。 ただし、一部の演算子では、条件付きでオペランドが評価されます。 つまり、このような演算子では、左端のオペランドの値によって、他の (または、どの) オペランドを評価するかどうかが定義されます。 これらの演算子は、条件付き論理 [AND (`&&`)](boolean-logical-operators.md#conditional-logical-and-operator-) および [OR (`||`)](boolean-logical-operators.md#conditional-logical-or-operator-) 演算子、[null 合体演算子 `??` と `??=`](null-coalescing-operator.md)、[null 条件演算子 `?.` と `?[]`](member-access-operators.md#null-conditional-operators--and-)、および[条件演算子 `?:`](conditional-operator.md) です。 詳細については、演算子ごとの説明を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#operators)に関するページの「[演算子](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [式](../../programming-guide/statements-expressions-operators/expressions.md)
