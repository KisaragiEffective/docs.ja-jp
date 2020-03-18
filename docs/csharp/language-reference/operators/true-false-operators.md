---
title: true 演算子と false 演算子 - C# リファレンス
ms.date: 12/10/2018
helpviewer_keywords:
- false operator [C#]
- true operator [C#]
ms.assetid: 81a888fd-011e-4589-b242-6c261fea505e
ms.openlocfilehash: 5ccd08a348478902bbbac36e99acf7ffc1fc814b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78846220"
---
# <a name="true-and-false-operators-c-reference"></a>true 演算子と false 演算子 (C# リファレンス)

`true` 演算子は、オペランドが確実に true であることを示す[ブール](../builtin-types/bool.md)値 `true` を返します。 `false` 演算子は、オペランドが確実に false であることを示す `bool` 値 `true` を返します。 `true` および `false` 演算子が互いに補完することは保証されていません。 つまり、`true` と `false` 演算子の両方が同じオペランドに対して `bool` 値 `false` を返す場合があります。 ある型でこの 2 つの演算子の 1 つを定義する場合は、もう 1 つの演算子も定義する必要があります。

> [!TIP]
> 3 値ロジックをサポートする必要がある場合は、`bool?` 型を使用します。たとえば、3 値ブール型をサポートするデータベースを操作する場合などです。 C# には、`&` オペランドを使用して 3 値ロジックをサポートする `|` および `bool?` 演算子があります。 詳細については、「[Boolean logical operators (ブール論理演算子)](boolean-logical-operators.md#nullable-boolean-logical-operators)」記事の「[Nullable Boolean logical operators (null 許容論理演算子)](boolean-logical-operators.md)」セクションを参照してください。

## <a name="boolean-expressions"></a>ブール式

`true` 演算子が定義された型は、[if](../keywords/if-else.md)、[do](../keywords/do.md)、[while](../keywords/while.md)、および [for](../keywords/for.md) ステートメントと、[条件演算子 `?:`](conditional-operator.md) の制御条件式の結果の型にすることができます。 詳細については、「[C# 言語仕様](~/_csharplang/spec/expressions.md#boolean-expressions)」の[ブール型の式](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

## <a name="user-defined-conditional-logical-operators"></a>ユーザー定義の条件付き論理演算子

`true` と `false` 演算子が定義された型によって、[論理 OR 演算子](operator-overloading.md) [ または](boolean-logical-operators.md#logical-or-operator-)論理 AND 演算子`|` [ が特定の方法で](boolean-logical-operators.md#logical-and-operator-)オーバーロード`&`される場合、[条件付き論理 OR 演算子](boolean-logical-operators.md#conditional-logical-or-operator-) `||` または [条件付き論理 AND 演算子](boolean-logical-operators.md#conditional-logical-and-operator-) `&&` を、それぞれ、その型のオペランドに対して評価することができます。 詳細については、「[C# 言語仕様](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)」の[ユーザー定義型条件論理演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

## <a name="example"></a>例

次の例では、`true` と `false` 演算子の両方を定義する型を示します。 この型により、論理 AND 演算子 `&` もオーバーロードされ、演算子 `&&` もその型のオペランドで評価できるようになります。

[!code-csharp[true and false operators example](snippets/TrueFalseOperators.cs)]

`&&` 演算子のショートサーキット動作に注意してください。 `GetFuelLaunchStatus` メソッドから `LaunchStatus.Red` が返されると、`&&` 演算子の右側のオペランドは評価されません。 これは、`LaunchStatus.Red` が確実に false であるためです。 したがって、論理 AND の結果は右側のオペランドの値に依存しません。 この例の出力は次のとおりです。

```console
Getting fuel launch status...
Wait!
```

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
