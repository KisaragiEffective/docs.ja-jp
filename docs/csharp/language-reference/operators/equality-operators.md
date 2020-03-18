---
title: 等値演算子 - C# リファレンス
description: C# の等値比較演算子と C# の型の等価性について学習します。
ms.date: 06/26/2019
author: pkulikov
f1_keywords:
- ==_CSharpKeyword
- '!=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- equality operator [C#]
- equals operator [C#]
- == operator [C#]
- inequality operator [C#]
- not equals operator [C#]
- '!= operator [C#]'
ms.openlocfilehash: 079522b18afdf86a942d502672174516d45d37fc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398257"
---
# <a name="equality-operators-c-reference"></a>等値演算子 (C# リファレンス)

[`==` (等価)](#equality-operator-) と [`!=` (非等値)](#inequality-operator-) 演算子は、そのオペランドが等しいかどうを確認します。

## <a name="equality-operator-"></a>等値演算子 ==

等値演算子 `==` は、そのオペランドが等しい場合には `true` を返し、それ以外の場合は `false` を返します。

### <a name="value-types-equality"></a>値の型の等価性

[組み込みの値の型](../builtin-types/value-types.md#built-in-value-types)のオペランドは、その値が等しい場合は等しくなります。

[!code-csharp-interactive[value types equality](snippets/EqualityOperators.cs#ValueTypesEquality)]

> [!NOTE]
> `==`、[`<`、`>`、`<=`、および `>=`](comparison-operators.md) 演算子の場合、いずれかのオペランドが数値 (<xref:System.Double.NaN?displayProperty=nameWithType> または <xref:System.Single.NaN?displayProperty=nameWithType>) でない場合、演算結果は `false` になります。 つまり、`NaN` の値は、`double` を含む他のどの `float` (または `NaN`) の値を上回ることも、下回ることも、等しいこともありません。 詳細およびサンプルについては、<xref:System.Double.NaN?displayProperty=nameWithType> または <xref:System.Single.NaN?displayProperty=nameWithType> の参照記事をご覧ください。

同じ[列挙](../builtin-types/enum.md)型の 2 つのオペランドは、基になる整数型の対応する値が等しい場合は等しくなります。

既定ではユーザー定義 [struct](../builtin-types/struct.md) 型は `==` 演算子をサポートしていません。 `==` 演算子をサポートするには、ユーザー定義 struct でそれを[オーバーロード](operator-overloading.md)する必要があります。

C# 7.3 より、`==` および `!=` 演算子は C# の[タプル](../../tuples.md)によってサポートされています。 詳細については、「[C# のタプル型](../../tuples.md#equality-and-tuples)」の記事の「[等値とタプル](../../tuples.md)」のセクションを参照してください。

### <a name="reference-types-equality"></a>参照型の等価性

既定では、2 つの参照型オペランドは、同じオブジェクトを参照しているときに等しくなります。

[!code-csharp[reference type equality](snippets/EqualityOperators.cs#ReferenceTypesEquality)]

次の例は、ユーザー定義の参照型が既定で `==` 演算子をサポートしていることを示しています。 ただし、参照型は `==` 演算子をオーバーロードできます。 参照型が `==` 演算子をオーバーロードする場合、その型の 2 つの参照が同じオブジェクトを参照しているかどうかを調べるには <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType> メソッドを使用します。

### <a name="string-equality"></a>文字列の等価性

2 つの [string](../builtin-types/reference-types.md#the-string-type) オペランドは、その両方が `null` であるか、両方の文字列インスタンスの長さが同じで、それぞれの文字列の位置に同じ文字が含まれている場合に等しくなります。

[!code-csharp-interactive[string equality](snippets/EqualityOperators.cs#StringEquality)]

序数の比較では大文字と小文字が区別されます。 文字列の比較に関する詳細については、「[C# で文字列を比較する方法](../../how-to/compare-strings.md)」を参照してください。

### <a name="delegate-equality"></a>デリゲートの等価性

同じランタイム型を持つ 2 つの[デリゲート](../../programming-guide/delegates/index.md) オペランドが等しくなるのは、それらの両方が `null` であるか、それらの呼び出しリストが同じ長さで、各位置に等しいエントリを含んでいる場合です。

[!code-csharp-interactive[delegate equality](snippets/EqualityOperators.cs#DelegateEquality)]

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#delegate-equality-operators)の「[Delegate equality operators (デリゲートの等値演算子)](~/_csharplang/spec/introduction.md)」セクションをご覧ください。

次の例に示すように、意味的に等しい[ラムダ式](../../programming-guide/statements-expressions-operators/lambda-expressions.md)を評価して生成されるデリゲートは、等しくありません。

[!code-csharp-interactive[from identical lambdas](snippets/EqualityOperators.cs#IdenticalLambdas)]

## <a name="inequality-operator-"></a>非等値演算子 !=

非等値演算子 `!=` は、そのオペランドが等しくない場合には `true` を返し、それ以外の場合は `false` を返します。 [組み込み型](../builtin-types/built-in-types.md)のオペランドの場合、式 `x != y` と式 `!(x == y)` では同じ結果が生成されます。 等価型の詳細については、「[等値演算子](#equality-operator-)」セクションを参照してください。

`!=` 演算子の使用例を次に示します。

[!code-csharp-interactive[non-equality examples](snippets/EqualityOperators.cs#NonEquality)]

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は [ 演算子と ](operator-overloading.md) 演算子を`==`オーバーロード`!=`できます。 ある型でこの 2 つの演算子の 1 つをオーバーロードする場合は、もう 1 つの演算子もオーバーロードする必要があります。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators)に関するページの「[関係演算子と型検査演算子](~/_csharplang/spec/introduction.md)」のセクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- <xref:System.IEquatable%601?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType>
- [等価比較](../../programming-guide/statements-expressions-operators/equality-comparisons.md)
- [比較演算子](comparison-operators.md)
