---
title: 比較演算子 - C# リファレンス
description: 数値の順序を確認するために使用できる C# 比較演算子について説明します。
ms.date: 04/25/2019
author: pkulikov
f1_keywords:
- <_CSharpKeyword
- '>_CSharpKeyword'
- <=_CSharpKeyword
- '>=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- less than operator [C#]
- < operator [C#]
- greater than operator [C#]
- '> operator [C#]'
- less than or equal to operator [C#]
- <= operator [C#]
- greater than or equal to operator [C#]
- '>= operator [C#]'
ms.openlocfilehash: 68502205193a1fc8ab7410053e13274560ffffb0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398053"
---
# <a name="comparison-operators-c-reference"></a>比較演算子 (C# リファレンス)

[`<` (小なり)](#less-than-operator-)、[`>` (大なり)](#greater-than-operator-)、[`<=` (以下)](#less-than-or-equal-operator-)、および [`>=` (以上) ](#greater-than-or-equal-operator-) 比較演算子は、関係演算子とも呼ばれ、そのオペランドの比較に使用されます。 これらの演算子は、[整数](../builtin-types/integral-numeric-types.md)と[浮動小数点](../builtin-types/floating-point-numeric-types.md)のすべての数値型によってサポートされています。

> [!NOTE]
> `==`、`<`、`>`、`<=`、および `>=` 演算子の場合、いずれかのオペランドが数値 (<xref:System.Double.NaN?displayProperty=nameWithType> または <xref:System.Single.NaN?displayProperty=nameWithType>) でない場合、演算結果は `false` になります。 つまり、`NaN` の値は、`double` を含む他のどの `float` (または `NaN`) の値を上回ることも、下回ることも、等しいこともありません。 詳細およびサンプルについては、<xref:System.Double.NaN?displayProperty=nameWithType> または <xref:System.Single.NaN?displayProperty=nameWithType> の参照記事をご覧ください。

列挙型は比較演算子もサポートします。 同じ[列挙](../builtin-types/enum.md)型のオペランドで、基になる整数型の対応する値が比較されます。

[`==` および `!=` 演算子](equality-operators.md) では、そのオペランドが等しいかどうかが確認されます。

## <a name="less-than-operator-"></a>小なり演算子 \<

左側のオペランドが右側のオペランドより小さい場合、`<` 演算子から `true` が返され、それ以外の場合は `false` が返されます。

[!code-csharp-interactive[less than example](snippets/ComparisonOperators.cs#Less)]

## <a name="greater-than-operator-"></a>大なり演算子 >

左側のオペランドが右側のオペランドより大きい場合、`>` 演算子から `true` が返され、それ以外の場合は `false` が返されます。

[!code-csharp-interactive[greater than example](snippets/ComparisonOperators.cs#Greater)]

## <a name="less-than-or-equal-operator-"></a>以下演算子 \<=

左側のオペランドが右側のオペランド以下の場合、`<=` 演算子から `true` が返され、それ以外の場合は `false` が返されます。

[!code-csharp-interactive[less than or equal example](snippets/ComparisonOperators.cs#LessOrEqual)]

## <a name="greater-than-or-equal-operator-"></a>以上演算子 >=

左側のオペランドが右側のオペランド以上の場合、`>=` 演算子から `true` が返され、それ以外の場合は `false` が返されます。

[!code-csharp-interactive[greater than or equal example](snippets/ComparisonOperators.cs#GreaterOrEqual)]

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は、[、](operator-overloading.md)、`<`、および `>` 演算子を`<=`オーバーロード`>=`できます。

ある型で `<` または `>` 演算子のいずれかをオーバーロードする場合は、`<` と `>` の両方をオーバーロードする必要があります。 ある型で `<=` または `>=` 演算子のいずれかをオーバーロードする場合は、`<=` と `>=` の両方をオーバーロードする必要があります。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators)に関するページの「[関係演算子と型検査演算子](~/_csharplang/spec/introduction.md)」のセクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- <xref:System.IComparable%601?displayProperty=nameWithType>
- [等値演算子](equality-operators.md)
