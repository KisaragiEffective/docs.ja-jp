---
title: 型テストとキャスト演算子 - C# リファレンス
description: 式の結果の型を確認し、必要に応じて別の型に変換することができる、C# の演算子について説明します。
ms.date: 06/21/2019
author: pkulikov
f1_keywords:
- is_CSharpKeyword
- as_CSharpKeyword
- ()_CSharpKeyword
- typeof_CSharpKeyword
helpviewer_keywords:
- type-testing operators [C#]
- conversion operators [C#]
- type conversion [C#]
- is operator [C#]
- as operator [C#]
- cast operator [C#]
- cast expression [C#]
- () operator [C#]
- typeof operator [C#]
ms.openlocfilehash: 037ddc8eeda418b2e4858ab98be6cd362ca0e1e2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398173"
---
# <a name="type-testing-and-cast-operators-c-reference"></a>型テストとキャスト演算子 (C# リファレンス)

次の演算子を使って、型のチェックまたは型の変換を実行できます。

- [is 演算子](#is-operator): 式のランタイム型と指定された型の間に互換性があるかどうかを確認します
- [as 演算子](#as-operator): 式のランタイム型と指定された型の間に互換性がある場合、式を指定された型に明示的に変換します
- [キャスト演算子 ()](#cast-operator-): 明示的な変換を実行します
- [typeof 演算子](#typeof-operator): 型の <xref:System.Type?displayProperty=nameWithType> インスタンスを取得します

## <a name="is-operator"></a>is 演算子

`is` 演算子では、式の結果のランタイム型と指定された型の間に互換性があるかどうかが調べられます。 C# 7.0 以降の `is` 演算子では、パターンに対する式の結果のテストも行われます。

`is` 型テスト演算子を使用する式の形式は次のとおりです

```csharp
E is T
```

`E` は値を返す式であり、`T` は型または型パラメーターの名前です。 `E` を匿名メソッドまたはラムダ式にすることはできません。

式 `E is T` では、`true` の結果が null ではなく、参照変換、ボックス化変換、またはボックス化解除変換によって型 `E` に変換できる場合は `T` が返され、それ以外の場合は `false` が返されます。 `is` 演算子では、ユーザー定義変換は考慮されません。

次の例で示す `is` 演算子では、式の結果のランタイム型が指定された型から派生する場合、つまり型の間に参照変換が存在する場合は、`true` が返されます。

[!code-csharp[is with reference conversion](snippets/TypeTestingAndConversionOperators.cs#IsWithReferenceConversion)]

次の例で示す `is` 演算子では、ボックス化変換とボックス化解除変換は考慮されますが、[数値変換](../builtin-types/numeric-conversions.md)は考慮されません。

[!code-csharp-interactive[is with int](snippets/TypeTestingAndConversionOperators.cs#IsWithInt)]

C# の変換については、[C# 言語仕様](~/_csharplang/spec/conversions.md)の「[Conversions (変換)](~/_csharplang/spec/introduction.md)」の章をご覧ください。

### <a name="type-testing-with-pattern-matching"></a>パターン マッチングを使用する型テスト

C# 7.0 以降の `is` 演算子では、パターンに対する式の結果のテストも行われます。 具体的には、次の形式の型パターンがサポートされます。

```csharp
E is T v
```

`E` は値を返す式、`T` は型または型パラメーターの名前、`v` は `T` 型の新しいローカル変数です。 `E` の結果が null ではなく、参照変換、ボックス化変換、またはボックス化解除変換によって `T` に変換できる場合、式 `E is T v` から `true` が返され、`E` の結果の変換された値が変数 `v` に代入されます。

次の例では、型パターンによる `is` 演算子の使用方法を示します。

[!code-csharp-interactive[is with type pattern](snippets/TypeTestingAndConversionOperators.cs#IsTypePattern)]

型パターンおよび他のサポートされるパターンについて詳しくは、「[is を使用したパターン マッチング](../keywords/is.md#pattern-matching-with-is)」をご覧ください。

## <a name="as-operator"></a>as 演算子

`as` 演算子では、式の結果が、指定された参照型または null 許容値型に明示的に変換されます。 変換できない場合、`as` 演算子からは `null` が返されます。 [キャスト演算子 ()](#cast-operator-) とは異なり、`as` 演算子では例外はスローされません。

式の形式は次のとおりです

```csharp
E as T
```

`E` は値を返す式であり、`T` は型または型パラメーターの名前です。次の式と同じ結果が生成されます

```csharp
E is T ? (T)(E) : (T)null
```

ただし、`E` が評価されるのは 1 回だけです。

`as` 演算子では、参照、null 許容、ボックス化、およびボックス化解除の各変換だけが考慮されます。 `as` 演算子を使って、ユーザー定義の変換を実行することはできません。 それを行うには、[キャスト演算子 ()](#cast-operator-) を使います。

`as` 演算子の使用例を次に示します。

[!code-csharp-interactive[as operator](snippets/TypeTestingAndConversionOperators.cs#AsOperator)]

> [!NOTE]
> 上の例のように、変換が成功したかどうかを確認するには、`as` 式の結果を `null` と比較する必要があります。 C# 7.0 以降では、変換が成功するかどうかのテストと、成功する場合の新しい変数への結果の代入の両方を、[is 演算子](#type-testing-with-pattern-matching)を使って行うことができます。

## <a name="cast-operator-"></a>キャスト演算子 ()

`(T)E` という形式のキャスト式では、式 `E` の結果が、型 `T` に明示的に変換されます。 型 `E` から型 `T` への明示的な変換が存在しない場合は、コンパイル時エラーが発生します。 実行時に、明示的な変換が成功せず、キャスト式で例外がスローされる可能性があります。

次の例では、数値と参照の明示的な変換を示します。

[!code-csharp-interactive[cast expression](snippets/TypeTestingAndConversionOperators.cs#Cast)]

サポートされる明示的な変換については、[C# 言語仕様](~/_csharplang/spec/conversions.md#explicit-conversions)の「[Explicit conversions (明示的な変換)](~/_csharplang/spec/introduction.md)」セクションをご覧ください。 カスタムの明示的または暗黙的な型変換を定義する方法については、「[User-defined conversion operators](user-defined-conversion-operators.md)」(ユーザー定義の変換演算子) を参照してください。

### <a name="other-usages-of-"></a>() の他の使用方法

かっこは、[メソッドまたはデリゲートを呼び出すとき](member-access-operators.md#invocation-operator-)にも使います。

他には、式に含まれる演算を評価する順序を調整する場合にもかっこを使います。 詳細については、[C# 演算子](index.md)に関するページを参照してください。

## <a name="typeof-operator"></a>typeof 演算子

`typeof` 演算子では、型の <xref:System.Type?displayProperty=nameWithType> インスタンスが取得されます。 `typeof` 演算子への引数では、次の例で示すように、型または型パラメーターの名前を指定する必要があります。

[!code-csharp-interactive[typeof operator](snippets/TypeTestingAndConversionOperators.cs#TypeOf)]

バインドされていないジェネリック型で `typeof` 演算子を使うこともできます。 バインドされていないジェネリック型の名前には、適切な数のコンマが含まれる必要があります。これは、型パラメーターの数より 1 だけ少ない数です。 次の例では、バインドされていないジェネリック型での `typeof` 演算子の使用方法を示します。

[!code-csharp-interactive[typeof unbound generic](snippets/TypeTestingAndConversionOperators.cs#TypeOfUnboundGeneric)]

式を `typeof` 演算子の引数にすることはできません。 式の結果のランタイム型に対する <xref:System.Type?displayProperty=nameWithType> インスタンスを取得するには、<xref:System.Object.GetType%2A?displayProperty=nameWithType> メソッドを使います。

### <a name="type-testing-with-the-typeof-operator"></a>`typeof` 演算子での型テスト

`typeof` 演算子を使って、式の結果のランタイム型が指定された型と完全に一致するかどうかを調べます。 次の例では、`typeof` 演算子と [is 演算子](#is-operator)で実行される型チェックの違いを示します。

[!code-csharp[typeof vs is](snippets/TypeTestingAndConversionOperators.cs#TypeCheckWithTypeOf)]

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

`is`、`as`、および `typeof` の各演算子はオーバーロードできません。

ユーザー定義型で `()` 演算子をオーバーロードすることはできませんが、キャスト式で実行できるカスタム型変換を定義することはできます。 詳細については、[ユーザー定義の変換演算子](user-defined-conversion-operators.md) に関するページを参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [is 演算子](~/_csharplang/spec/expressions.md#the-is-operator)
- [as 演算子](~/_csharplang/spec/expressions.md#the-as-operator)
- [キャスト式](~/_csharplang/spec/expressions.md#cast-expressions)
- [typeof 演算子](~/_csharplang/spec/expressions.md#the-typeof-operator)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [パターン マッチング、is 演算子、as 演算子を使用して安全にキャストする方法](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)
- [.NET のジェネリック](../../../standard/generics/index.md)
