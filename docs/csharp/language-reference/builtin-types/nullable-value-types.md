---
title: null 許容値型 - C# リファレンス
description: C# の Null 許容値型とその使用方法について説明します
ms.date: 11/04/2019
helpviewer_keywords:
- nullable value types [C#]
ms.openlocfilehash: a84b3d60269491846b783e5046a84a1d14e258a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398275"
---
# <a name="nullable-value-types-c-reference"></a>null 許容値型 (C# リファレンス)

"*null 許容値型*" `T?` は、基になる[値型](value-types.md) `T` のすべての値と、追加の [null](../keywords/null.md) 値を表します。 たとえば、`bool?` 変数には、`true`、`false`、`null` の 3 つの値のいずれかを割り当てることができます。 基になる値型 `T` を null 許容値型にすることはできません。

> [!NOTE]
> C# 8.0 で、Null 許容参照型機能が導入されました。 詳細については、「[null 許容参照型](../../nullable-references.md)」を参照してください。 null 許容値型は、C# 2 から使用できます。

null 許容値型は、ジェネリック <xref:System.Nullable%601?displayProperty=nameWithType> 構造体のインスタンスです。 `T` または `Nullable<T>` の代替可能な形式のいずれかで基になる型 `T?` を持つ null 許容値型を参照できます。

null 許容値型は通常、基になる値型の未定義の値を表す必要があるときに使用します。 たとえば、ブール型 (`bool`) 変数で可能なのは、`true` または `false` のいずれかです。 ただし、一部のアプリケーションでは、変数の値が未定義または存在しない場合があります。 たとえば、データベース フィールドに、`true` または `false` が含まれている場合や、値がまったく含まれていない場合 (`NULL`) があります。 このシナリオでは、`bool?` 型を使用できます。

## <a name="declaration-and-assignment"></a>宣言と代入

値型は、対応する null 許容値型に暗黙的に変換できるため、基になる値型の場合と同様に、null 許容値型の変数に値を割り当てることができます。 `null` 値を代入することもできます。 次に例を示します。

[!code-csharp[declare and assign](snippets/NullableValueTypes.cs#Declaration)]

null 許容値型の既定値は `null` を表します。つまり、<xref:System.Nullable%601.HasValue%2A?displayProperty=nameWithType> プロパティが `false` を返すインスタンスです。

## <a name="examination-of-an-instance-of-a-nullable-value-type"></a>null 許容値型のインスタンスの検査

C# 7.0 以降では、[型パターンで `is` 演算子](../operators/type-testing-and-cast.md#type-testing-with-pattern-matching)を使用して、`null` の null 許容値型のインスタンスを調べ、基になる型の値を取得することができます。

[!code-csharp-interactive[use pattern matching](snippets/NullableValueTypes.cs#PatternMatching)]

null 許容値型の変数の値を確認して取得するには、常に次の読み取り専用プロパティを使用できます。

- <xref:System.Nullable%601.HasValue%2A?displayProperty=nameWithType> は、null 許容値型のインスタンスに、基になる型の値が含まれるかどうかを示します。

- <xref:System.Nullable%601.Value%2A?displayProperty=nameWithType> が <xref:System.Nullable%601.HasValue%2A> の場合、`true` は基になる型の値を取得します。 <xref:System.Nullable%601.HasValue%2A> が `false` の場合、<xref:System.Nullable%601.Value%2A> プロパティは <xref:System.InvalidOperationException> をスローします。

次の例では、`HasValue` プロパティを使用して、値を表示する前に変数に値が格納されているかどうかをテストします。

[!code-csharp-interactive[use HasValue](snippets/NullableValueTypes.cs#HasValue)]

次の例に示すように、`null` プロパティを使用する代わりに、null 許容値型の変数を `HasValue` と比較することもできます。

[!code-csharp-interactive[use comparison with null](snippets/NullableValueTypes.cs#CompareWithNull)]

## <a name="conversion-from-a-nullable-value-type-to-an-underlying-type"></a>null 許容値型から基になる型への変換

null 許容値型の値を null 非許容値型の変数に割り当てる場合は、`null` の代わりに割り当てる値を指定する必要がある場合があります。 これを行うには、[null 合体演算子 `??`](../operators/null-coalescing-operator.md) を使用します (<xref:System.Nullable%601.GetValueOrDefault(%600)?displayProperty=nameWithType> メソッドも同じ目的で使用することができます)。

[!code-csharp-interactive[?? operator](snippets/NullableValueTypes.cs#NullCoalescing)]

[ の代わりに基になる値の型の](default-values.md)既定`null`値を使用する場合は、<xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType> メソッドを使用します。

次の例に示すように、null 許容値型を null 非許容型に明示的にキャストすることもできます。

[!code-csharp[explicit cast](snippets/NullableValueTypes.cs#Cast)]

実行時に null 許容値型の値が `null` の場合は、明示的なキャストによって <xref:System.InvalidOperationException> がスローされます。

null 非許容値型 `T` は、対応する null 許容値型 `T?` に暗黙的に変換されます。

## <a name="lifted-operators"></a>リフト演算子

定義済みの単項[演算子](../operators/index.md)および 2 項演算子、または値型 `T` によってサポートされるオーバーロードされた任意の演算子は、対応する null 許容値型 `T?` でもサポートされます。 "*リフト演算子*" とも呼ばれるこれらの演算子では、一方または両方のオペランドが `null` の場合に `null` が生成されます。それ以外の場合は、そのオペランドに含まれている値を使用して結果が算出されます。 次に例を示します。

[!code-csharp[lifted operators](snippets/NullableValueTypes.cs#LiftedOperator)]

> [!NOTE]
> `bool?` 型の場合、定義済みの `&` および `|` 演算子は、このセクションで説明されている規則に従わないことに注意してください。オペランドの 1 つが `null` の場合も、演算子の評価の結果は null 以外である可能性があります。 詳細については、「[Boolean logical operators (ブール論理演算子)](../operators/boolean-logical-operators.md#nullable-boolean-logical-operators)」記事の「[Nullable Boolean logical operators (null 許容論理演算子)](../operators/boolean-logical-operators.md)」セクションを参照してください。

[比較演算子](../operators/comparison-operators.md) `<`、`>`、`<=`、`>=` では、一方または両方のオペランドが `null` の場合、結果は `false` になります。それ以外の場合は、オペランドに含まれる値が比較されます。 ある比較 (たとえば、`<=`) から返される結果が `false` であっても、逆の比較 (`>`) から返される結果が `true` であるとは限りません。 次の例は、10 が

- `null` 以上ではなく
- `null` 未満でもないことを示します

[!code-csharp-interactive[relational and equality operators](snippets/NullableValueTypes.cs#ComparisonOperators)]

[等値演算子](../operators/equality-operators.md#equality-operator-) `==` では、両方のオペランドが `null` の場合、結果は `true` になります。一方のオペランドだけが `null` の場合、結果は `false` です。それ以外の場合は、オペランドに含まれる値が比較されます。

[非等値演算子](../operators/equality-operators.md#inequality-operator-) `!=` では、両方のオペランドが `null` の場合、結果は `false` になります。一方のオペランドだけが `null` の場合、結果は `true` です。それ以外の場合は、オペランドに含まれる値が比較されます。

2 つの値型の間に[ユーザー定義の変換](../operators/user-defined-conversion-operators.md)が存在する場合は、それに対応する null 許容値型間でも同じ変換を使用することができます。

## <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除

null 許容値型のインスタンス `T?` は、次のように[ボックス化](../../programming-guide/types/boxing-and-unboxing.md)されます。

- <xref:System.Nullable%601.HasValue%2A> が `false` を返した場合は、null 参照が生成されます。
- <xref:System.Nullable%601.HasValue%2A> が `true`を返した場合は、`T` のインスタンスではなく、基になる値型 <xref:System.Nullable%601> の対応する値がボックス化されます。

次の例に示すように、値型 `T` のボックス化された値を、対応する null 許容値型 `T?` にボックス化解除できます。

[!code-csharp-interactive[boxing and unboxing](snippets/NullableValueTypes.cs#Boxing)]

## <a name="how-to-identify-a-nullable-value-type"></a>方法: null 許容値型を識別する

次の例は、<xref:System.Type?displayProperty=nameWithType> インスタンスが構築された null 許容値型 (つまり、指定された型パラメーター <xref:System.Nullable%601?displayProperty=nameWithType> を使用する `T` 型) を表すかどうかを判断する方法を示しています。

[!code-csharp-interactive[whether Type is nullable](snippets/NullableValueTypes.cs#IsTypeNullable)]

例で示されているとおり、[ インスタンスの作成には、](../operators/type-testing-and-cast.md#typeof-operator)typeof<xref:System.Type?displayProperty=nameWithType> 演算子を使用します。

インスタンスが null 許容値型かどうかを判断したい場合は、<xref:System.Object.GetType%2A?displayProperty=nameWithType> インスタンスが前述のコードでテストされるように、<xref:System.Type> メソッドは使用しないでください。 null 許容値型のインスタンスで <xref:System.Object.GetType%2A?displayProperty=nameWithType> メソッドを呼び出した場合、そのインスタンスは [ に](#boxing-and-unboxing)ボクシング<xref:System.Object>されます。 null 許容値型の null 以外のインスタンスのボックス化は、基になる型の値のボックス化と等しいので、<xref:System.Object.GetType%2A> は、null 許容値型の基になる型を表す <xref:System.Type> インスタンスを返します。

[!code-csharp-interactive[GetType example](snippets/NullableValueTypes.cs#GetType)]

また、インスタンスが null 許容値型であるかどうかを判断するために、[is](../operators/type-testing-and-cast.md#is-operator) 演算子を使用しないでください。 次の例に示すように、`is` 演算子を使用して null 許容値型のインスタンスとその基になる型のインスタンスの型を区別することはできません。

[!code-csharp-interactive[is operator example](snippets/NullableValueTypes.cs#IsOperator)]

次の例のコードを使用すると、インスタンスが null 許容値型であるかどうかを判別することができます。

[!code-csharp-interactive[whether an instance is of a nullable type](snippets/NullableValueTypes.cs#IsInstanceNullable)]

> [!NOTE]
> このセクションで説明されているメソッドは、[null 許容参照型](../../nullable-references.md)の場合には適用されません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [Null 許容型](~/_csharplang/spec/types.md#nullable-types)
- [リフト演算子](~/_csharplang/spec/expressions.md#lifted-operators)
- [暗黙の null 許容変換](~/_csharplang/spec/conversions.md#implicit-nullable-conversions)
- [明示的な null 許容変換](~/_csharplang/spec/conversions.md#explicit-nullable-conversions)
- [リフト変換演算子](~/_csharplang/spec/conversions.md#lifted-conversion-operators)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [What Exactly Does 'Lifted' mean? ('Lifted' の正確な意味)](https://docs.microsoft.com/archive/blogs/ericlippert/what-exactly-does-lifted-mean)
- <xref:System.Nullable%601?displayProperty=nameWithType>
- <xref:System.Nullable?displayProperty=nameWithType>
- <xref:System.Nullable.GetUnderlyingType%2A?displayProperty=nameWithType>
- [Null 許容参照型](../../nullable-references.md)
