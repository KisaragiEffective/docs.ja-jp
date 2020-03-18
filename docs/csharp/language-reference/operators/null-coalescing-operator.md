---
title: ?? および ??= 演算子 - C# リファレンス
ms.date: 09/10/2019
f1_keywords:
- ??_CSharpKeyword
- ??=_CSharpKeyword
helpviewer_keywords:
- null-coalescing operator [C#]
- ?? operator [C#]
- null-coalescing assignment [C#]
- ??= operator [C#]
ms.assetid: 088b1f0d-c1af-4fe1-b4b8-196fd5ea9132
ms.openlocfilehash: 69294f0fb706868b48b8d7fe8b95fa345af169b1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847314"
---
# <a name="-and--operators-c-reference"></a>?? および ?? 演算子 (C# リファレンス)

null 合体演算子 `??` では、それが `null` ではない場合、その左側のオペランドの値が返されます。それ以外の場合は、右側のオペランドが評価され、その結果が返されます。 `??` 演算子では、左側のオペランドが null 値以外に評価された場合は、その右側のオペランドは評価されません。

C# 8.0 以降では、左側のオペランドが `??=` に評価された場合にのみ、右側のオペランドの値を左側のオペランドに割り当てる null 合体割り当て演算子 `null` を使用できます。 `??=` 演算子では、左側のオペランドが null 値以外に評価された場合は、その右側のオペランドは評価されません。

[!code-csharp[null-coalescing assignment](snippets/NullCoalescingOperator.cs#Assignment)]

`??=` 演算子の左側のオペランドは、変数、[プロパティ](../../programming-guide/classes-and-structs/properties.md)、または[インデクサー](../../programming-guide/indexers/index.md)要素である必要があります。

C# 7.3 以前では、`??` 演算子の左側のオペランドの型は、[参照型](../keywords/reference-types.md)または [null 許容値型](../builtin-types/nullable-value-types.md)である必要があります。 C# 8.0 以降では、その要件は次に置き換えられます。`??` 演算子と `??=` 演算子の左側のオペランドの型は、null 非許容値型にすることはできません。 特に、C# 8.0 以降では、null 合体演算子を制約のない型パラメーターと共に使用できます。

[!code-csharp[unconstrained type parameter](snippets/NullCoalescingOperator.cs#UnconstrainedType)]

null 合体演算子は、右結合です。 つまり、フォームの式

```csharp
a ?? b ?? c
d ??= e ??= f
```

は次のように評価されます。

```csharp
a ?? (b ?? c)
d ??= (e ??= f)
```

## <a name="examples"></a>例

`??` 演算子および `??=` 演算子は、次のシナリオで役立つことがあります。

- [null 条件演算子 ?. および ?[]](member-access-operators.md#null-conditional-operators--and-) を含む式は、null 条件演算の式の結果が `??` の場合に評価する代替の式を指定するために、`null` 演算子を使用することができます。

  [!code-csharp-interactive[with null-conditional](snippets/NullCoalescingOperator.cs#WithNullConditional)]

- [null 値許容型](../builtin-types/nullable-value-types.md)を使用して、基になる値の型の値を提供する必要がある場合、`??` 演算子を使用して、null 値許容型が `null` の場合に提供する値を指定します。

  [!code-csharp-interactive[with nullable types](snippets/NullCoalescingOperator.cs#WithNullableTypes)]

  null 値許容型が <xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType> の場合に使用される値を、基になる値型の既定値にする場合は、`null` メソッドを使用します。

- C# 7.0 以降では、[ 演算子の右側のオペランドとして `throw`](../keywords/throw.md#the-throw-expression) 式`??`を使用し、引数のチェック コードをより簡潔にすることができます。

  [!code-csharp[with throw expression](snippets/NullCoalescingOperator.cs#WithThrowExpression)]

  上記の例は、[式のようなメンバー](../../programming-guide/statements-expressions-operators/expression-bodied-members.md)を使用してプロパティを定義する方法も示しています。

- C# 8.0 以降では、`??=` 演算子を使用して、フォームのコード

  ```csharp
  if (variable is null)
  {
      variable = expression;
  }
  ```

  を、以下のコードに置き換えます。

  ```csharp
  variable ??= expression;
  ```

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

演算子 `??` および `??=` はオーバーロードできません。

## <a name="c-language-specification"></a>C# 言語仕様

`??` 演算子の詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#the-null-coalescing-operator)の [null 合体演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

`??=` リテラルの詳細については、[機能提案メモ](~/_csharplang/proposals/csharp-8.0/null-coalescing-assignment.md)を参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [?. および ?[] 演算子](member-access-operators.md#null-conditional-operators--and-)
- [?:演算子 (C# リファレンス)](conditional-operator.md)
