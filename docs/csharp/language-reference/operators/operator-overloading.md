---
title: 演算子のオーバーロード - C# リファレンス
description: C# 演算子をオーバーロードする方法と、どの C# 演算子がオーバーロード可能かについて説明します。
ms.date: 07/05/2019
f1_keywords:
- operator_CSharpKeyword
helpviewer_keywords:
- operator keyword [C#]
- operator overloading [C#]
ms.openlocfilehash: cdb35b212d5bfc4cc685fbfd6c294066983709df
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847301"
---
# <a name="operator-overloading-c-reference"></a>演算子のオーバーロード (C# リファレンス)

ユーザー定義型は定義済みの C# 演算子をオーバーロードできます。 つまり、1 つまたは両方のオペランドに該当する型では、演算のカスタム実装を提供できます。 オーバーロード可能な C# 演算子は、「[オーバーロード可能な演算子](#overloadable-operators)」のセクションで示します。

演算子の宣言には `operator` キーワードを使用します。 演算子の宣言では、次の規則を満たす必要があります。

- これには、`public` と `static` 修飾子の両方が含まれています。
- 単項演算子には、1 つの入力パラメーターがあります。 2 項演算子には、2 つの入力パラメーターがあります。 どちらの場合も、少なくとも 1 つのパラメーターの型が `T` または `T?` でなければなりません。`T` は演算子の宣言が含まれる型です。

次の例は、有理数を表す簡略化された構造を定義しています。 構造体がいくつかの[算術演算子](arithmetic-operators.md)をオーバーロードします。

[!code-csharp[fraction example](snippets/OperatorOverloading.cs)]

[ から ](user-defined-conversion-operators.md) への`int`暗黙的な変換を定義する`Fraction`ことで、前の例を拡張できます。 その場合、オーバーロードされた演算子はこれら 2 つの型の引数をサポートします。 つまり、整数を分数に足し、結果として分数を取得できるようになります。

また、`operator` キーワードを使用してカスタムの型変換を定義することもできます。 詳細については、[ユーザー定義の変換演算子](user-defined-conversion-operators.md) に関するページを参照してください。

## <a name="overloadable-operators"></a>オーバーロード可能な演算子

次の表は、C# 演算子のオーバーロード可/不可に関する情報を示します。

| オペレーター | オーバーロード可/不可 |
| --------- | --------------- |
|[+x](arithmetic-operators.md#unary-plus-and-minus-operators)、[-x](arithmetic-operators.md#unary-plus-and-minus-operators)、[!x](boolean-logical-operators.md#logical-negation-operator-)、[~x](bitwise-and-shift-operators.md#bitwise-complement-operator-)、[++](arithmetic-operators.md#increment-operator-)、[--](arithmetic-operators.md#decrement-operator---)、[true](true-false-operators.md)、[false](true-false-operators.md)|これらの単項演算子はオーバーロードできます。|
|[x + y](addition-operator.md)、[x - y](subtraction-operator.md)、[x \* y](arithmetic-operators.md#multiplication-operator-)、[x / y](arithmetic-operators.md#division-operator-)、[x % y](arithmetic-operators.md#remainder-operator-)、[x & y](boolean-logical-operators.md#logical-and-operator-)、[x &#124; y](boolean-logical-operators.md#logical-or-operator-)、[x ^ y](boolean-logical-operators.md#logical-exclusive-or-operator-)、[x \<\< y](bitwise-and-shift-operators.md#left-shift-operator-)、[x >> y](bitwise-and-shift-operators.md#right-shift-operator-)、[x == y](equality-operators.md#equality-operator-)、[x != y](equality-operators.md#inequality-operator-)、[x \< y](comparison-operators.md#less-than-operator-)、[x > y](comparison-operators.md#greater-than-operator-)、[x \<= y](comparison-operators.md#less-than-or-equal-operator-)、[x >= y](comparison-operators.md#greater-than-or-equal-operator-)|これらの 2 項演算子はオーバーロードできます。 特定の演算子はペアでオーバーロードする必要があります。詳細については、この表の後にある注を参照してください。|
|[x && y](boolean-logical-operators.md#conditional-logical-and-operator-)、[x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-)|条件付き論理演算子は、オーバーロードできません。 ただし、オーバーロードされた [`true` および `false` 演算子](true-false-operators.md)を含む型が特定の方法で `&` または <code>&#124;</code> もオーバーロードしている場合は、その型のオペランドに対してそれぞれ `&&` または <code>&#124;&#124;</code> 演算子の評価が可能になります。 詳細については、「[C# 言語仕様](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)」の[ユーザー定義型条件論理演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。|
|[a&#91;i&#93;](member-access-operators.md#indexer-operator-)|要素へのアクセスはオーバーロード可能な演算子とは見なされていませんが、[インデクサー](../../programming-guide/indexers/index.md)を定義することができます。|
|[(T)x](type-testing-and-cast.md#cast-operator-)|キャスト演算子はオーバーロードできませんが、新しい変換演算子を定義できます。 詳細については、[ユーザー定義の変換演算子](user-defined-conversion-operators.md) に関するページを参照してください。|
|[+=](arithmetic-operators.md#compound-assignment), [-=](arithmetic-operators.md#compound-assignment), [\*=](arithmetic-operators.md#compound-assignment), [/=](arithmetic-operators.md#compound-assignment), [%=](arithmetic-operators.md#compound-assignment), [&=](boolean-logical-operators.md#compound-assignment), [&#124;=](boolean-logical-operators.md#compound-assignment), [^=](boolean-logical-operators.md#compound-assignment), [\<\<=](bitwise-and-shift-operators.md#compound-assignment), [>>=](bitwise-and-shift-operators.md#compound-assignment)|複合代入演算子を明示的にオーバーロードすることはできません。 ただし、二項演算子をオーバーロードするとき、対応する複合代入演算子がある場合は、それも暗黙的にオーバーロードされます。 たとえば、`+=` は、オーバーロード可能な `+` を使用して評価されます。|
|[^x](member-access-operators.md#index-from-end-operator-)、[x = y](assignment-operator.md)、[x.y](member-access-operators.md#member-access-operator-)、[c ? t : f](conditional-operator.md)、[x ?? y](null-coalescing-operator.md)、[x ??= y](null-coalescing-operator.md)、[x..y](member-access-operators.md#range-operator-)、[x->y](pointer-related-operators.md#pointer-member-access-operator--)、[=>](lambda-operator.md)、[f(x)](member-access-operators.md#invocation-operator-)、[as](type-testing-and-cast.md#as-operator)、[await](await.md)、[checked](../keywords/checked.md)、[unchecked](../keywords/unchecked.md)、[default](default.md)、[delegate](delegate-operator.md)、[is](type-testing-and-cast.md#is-operator)、[nameof](nameof.md)、[new](new-operator.md)、[sizeof](sizeof.md)、[stackalloc](stackalloc.md)、[typeof](type-testing-and-cast.md#typeof-operator)|これらの演算子はオーバーロードできません。|

> [!NOTE]
> 比較演算子は、ペアでオーバーロードする必要があります。 つまり、ペアのどちらかの演算子をオーバーロードする場合、もう一方の演算子もオーバーロードする必要があります。 次のようなペアがこれに該当します。
>
> - `==` 演算子と `!=` 演算子
> - `<` 演算子と `>` 演算子
> - `<=` 演算子と `>=` 演算子

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [演算子のオーバーロード](~/_csharplang/spec/expressions.md#operator-overloading)
- [オペレーター](~/_csharplang/spec/classes.md#operators)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ユーザー定義の変換演算子](user-defined-conversion-operators.md)
- [設計ガイドライン - 演算子のオーバーロード](../../../standard/design-guidelines/operator-overloads.md)
- [設計ガイドライン - 等値演算子](../../../standard/design-guidelines/equality-operators.md)
- [C# のオーバーロードされた演算子が常に静的である理由](https://docs.microsoft.com/archive/blogs/ericlippert/why-are-overloaded-operators-always-static-in-c)
