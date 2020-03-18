---
title: 代入演算子 - C# リファレンス
ms.date: 09/10/2019
f1_keywords:
- =_CSharpKeyword
helpviewer_keywords:
- = operator [C#]
ms.assetid: d802a6d5-32f0-42b8-b180-12f5a081bfc1
ms.openlocfilehash: 420b41f586a6980d40cf1171eef00dad37bf5abf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398059"
---
# <a name="assignment-operators-c-reference"></a>代入演算子 (C# リファレンス)

代入演算子 `=` は、右辺オペランドの値を、左辺オペランドに指定された変数、[プロパティ](../../programming-guide/classes-and-structs/properties.md)、または[インデクサー](../../programming-guide/indexers/index.md)要素に割り当てます。 代入式の結果は、左辺のオペランドに割り当てられる値です。 右辺のオペランドの型は、左辺のオペランドの型と同じであるか、暗黙に変換できる必要があります。

代入演算子 `=` は右結合です。つまり、次の形式の式があるとします

```csharp
a = b = c
```

これが次のように評価されます。

```csharp
a = (b = c)
```

次の例では、左側のオペランドとしてローカル変数、プロパティ、およびインデクサー要素を使用する代入演算子の使用方法を示します。

[!code-csharp-interactive[simple assignment](snippets/AssignmentOperator.cs#Simple)]

## <a name="ref-assignment-operator"></a>ref 代入演算子

C# 7.3 以降では、ref 代入演算子 `= ref` を使用して、[ref ローカル](../keywords/ref.md#ref-locals)変数または [ref 読み取り専用ローカル](../keywords/ref.md#ref-readonly-locals)変数を割り当てることができます。 次の例は、ref 代入演算子の使用方法を示しています。

[!code-csharp[ref assignment operator](snippets/AssignmentOperator.cs#RefAssignment)]

ref 代入演算子の場合、その両方のオペランドの型が同じである必要があります。

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

複合代入は、[算術](arithmetic-operators.md#compound-assignment)、[ブール論理](boolean-logical-operators.md#compound-assignment)、[ビット単位論理およびシフト](bitwise-and-shift-operators.md#compound-assignment)の各演算子でサポートされています。

## <a name="null-coalescing-assignment"></a>null 合体割り当て

C# 8.0 以降では、null 合体割り当て演算子 `??=` を使用して、左側のオペランドが `null` に評価された場合にのみ、右側のオペランドの値を左側のオペランドに割り当てることができます。 詳細については、「[?? and ??= 演算子](null-coalescing-operator.md)」の記事を参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は、代入演算子を[オーバーロード](operator-overloading.md)できません。 ただし、ユーザー定義型は、別の型への暗黙的な変換を定義できます。 この方法により、ユーザー定義型の値を、別の型の変数、プロパティ、またはインデクサー要素に割り当てることができます。 詳細については、[ユーザー定義の変換演算子](user-defined-conversion-operators.md) に関するページを参照してください。

ユーザー定義型は、複合代入演算子を明示的にオーバーロードすることはできません。 ただし、ユーザー定義型が二項演算子 `op` をオーバーロードし、`op=` 演算子も存在する場合は、それも暗黙的にオーバーロードされます。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#assignment-operators)の「[Assignment operators (代入演算子)](~/_csharplang/spec/introduction.md)」セクションを参照してください。

ref 代入演算子 `= ref` の詳細については、[機能提案メモ](~/_csharplang/proposals/csharp-7.3/ref-local-reassignment.md)を参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ref キーワード](../keywords/ref.md)
