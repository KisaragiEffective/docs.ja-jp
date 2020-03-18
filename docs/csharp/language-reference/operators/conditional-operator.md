---
title: '?: 演算子 - C# リファレンス'
ms.date: 11/20/2018
f1_keywords:
- ?:_CSharpKeyword
- ?_CSharpKeyword
- :_CSharpKeyword
helpviewer_keywords:
- '?: operator [C#]'
- conditional operator (?:) [C#]
ms.assetid: e83a17f1-7500-48ba-8bee-2fbc4c847af4
ms.openlocfilehash: 60156585dd21d5d2f9c9f3916452bb8574ddd4e4
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75712729"
---
# <a name="-operator-c-reference"></a>?: 演算子 (C# リファレンス)

条件演算子 `?:` は、三項条件演算子とも呼ばれ、ブール式を評価し、ブール式の評価結果 (`true` または `false`) に応じて、2 つの式のいずれかの結果を返します。 C# 7.2 以降、[ref 条件式](#conditional-ref-expression)は、2 つの式のいずれかの結果への参照を返します。

この条件演算子の構文は次のとおりです。

```csharp
condition ? consequent : alternative
```

`condition` 式は `true` または `false` と評価する必要があります。 `condition` が `true` と評価された場合は、`consequent` 式が評価され、その結果が演算の結果になります。 `condition` が `false` と評価された場合は、`alternative` 式が評価され、その結果が演算の結果になります。 `consequent` または `alternative` のみが評価されます。

`consequent` の型と `alternative` の型は同じ型であるか、一方の型から他方の型への暗黙の型変換が存在している必要があります。

条件演算子は右結合です。つまり、次の形式の式があるとします。

```csharp
a ? b : c ? d : e
```

これが次のように評価されます。

```csharp
a ? b : (c ? d : e)
```

> [!TIP]
> 次のニーモニック デバイスを使用して、条件演算子の評価方法を思い出すことができます。
>
> ```text
> is this condition true ? yes : no
> ```

条件演算子の使用例を次に示します。

[!code-csharp-interactive[non ref conditional](~/samples/csharp/language-reference/operators/ConditionalOperator.cs#ConditionalValue)]

## <a name="conditional-ref-expression"></a>ref 条件式

C# 7.2 以降、ref 条件式を使用して、2 つの式のいずれかの結果に対する参照を返すことができます。 この参照を [ref ローカル](../keywords/ref.md#ref-locals)または [ref readonly ローカル](../keywords/ref.md#ref-readonly-locals)変数に代入できます。または、[参照の戻り値](../keywords/ref.md#reference-return-values)または [`ref` メソッドのパラメーター](../keywords/ref.md#passing-an-argument-by-reference)として使用できます。

ref 条件式の構文は次のとおりです。

```csharp
condition ? ref consequent : ref alternative
```

元の条件演算子と同じように、ref 条件式は、2 つの式 (`consequent` または `alternative`) のいずれかのみを評価します。

ref 条件式の場合、`consequent` と`alternative` の型は同じである必要があります。

ref 条件演算子の使用例を次に示します。

[!code-csharp-interactive[conditional ref](~/samples/csharp/language-reference/operators/ConditionalOperator.cs#ConditionalRef)]

## <a name="conditional-operator-and-an-ifelse-statement"></a>条件演算子と `if..else` ステートメント

[if-else](../keywords/if-else.md) ステートメントではなく条件演算子を使用すると、値の計算を条件付きで実行する必要がある場合に、コードをもっと簡潔にできる可能性があります。 次の例では、整数を負の値または負以外の値に分類するための 2 つの方法を示しています。

[!code-csharp[conditional and if-else](~/samples/csharp/language-reference/operators/ConditionalOperator.cs#CompareWithIf)]

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は条件演算子をオーバーロードできません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の「[条件演算子](~/_csharplang/spec/expressions.md#conditional-operator)」セクションを参照してください。

ref 条件式について詳しくは、[機能提案メモ](~/_csharplang/proposals/csharp-7.2/conditional-ref.md)をご覧ください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [if-else ステートメント](../keywords/if-else.md)
- [?. および ?[] 演算子](member-access-operators.md#null-conditional-operators--and-)
- [?? および ??= 演算子](null-coalescing-operator.md)
- [ref キーワード](../keywords/ref.md)
