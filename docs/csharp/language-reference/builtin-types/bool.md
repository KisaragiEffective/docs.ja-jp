---
title: bool 型 - C# リファレンス
ms.date: 11/26/2019
f1_keywords:
- bool
- bool_CSharpKeyword
helpviewer_keywords:
- bool data type [C#]
- Boolean [C#]
ms.assetid: 551cfe35-2632-4343-af49-33ad12da08e2
ms.openlocfilehash: 2ba2e54a6b0f24402fc3728dfe19b548a2368830
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78846446"
---
# <a name="bool-c-reference"></a>bool (C# リファレンス)

`bool` 型キーワードは、ブール値 (<xref:System.Boolean?displayProperty=nameWithType> または `true` のいずれか) を表す .NET `false` 構造体型のエイリアスです。

`bool` 型の値を使って論理演算を実行するには、[ブール論理](../operators/boolean-logical-operators.md)演算子を使用します。 `bool` 型は、[比較](../operators/comparison-operators.md)および[等値](../operators/equality-operators.md)演算子の結果の型です。 `bool` 式は、[if](../keywords/if-else.md)、[do](../keywords/do.md)、[while](../keywords/while.md)、および [for](../keywords/for.md) ステートメントおよび[条件演算子 `?:`](../operators/conditional-operator.md) で制御条件式にすることができます。

`bool` 型の既定値は `false` です。

## <a name="literals"></a>リテラル

`true` および `false` リテラルを使用して、`bool` 変数を初期化したり、`bool` 値を渡したりすることができます。

[!code-csharp-interactive[bool literals](snippets/BoolType.cs#Literals)]

## <a name="three-valued-boolean-logic"></a>3 値ブール型ロジック

3 値ロジックをサポートする必要がある場合は、null 許容型の `bool?` を使用します。たとえば、3 値ブール型をサポートするデータベースを操作する場合などです。 `bool?` オペランドの場合、定義済みの `&` 演算子と `|` 演算子は 3 値ロジックをサポートします。 詳細については、「[Boolean logical operators (ブール論理演算子)](../operators/boolean-logical-operators.md#nullable-boolean-logical-operators)」記事の「[Nullable Boolean logical operators (null 許容論理演算子)](../operators/boolean-logical-operators.md)」セクションを参照してください。

null 許容値型の詳細については、「[null 許容値型](nullable-value-types.md)」を参照してください。

## <a name="conversions"></a>コンバージョン

C# には、`bool` 型が関係する変換が 2 つのみ用意されています。 対応する null 許容型の `bool?` への暗黙的な変換と、`bool?` 型からの明示的な変換です。 ただし、.NET には、`bool` 型との間の変換に使用できる追加のメソッドが用意されています。 詳細については、[ API リファレンス ページの「](/dotnet/api/system.boolean#converting-to-and-from-boolean-values)ブール値との間の変換<xref:System.Boolean?displayProperty=nameWithType>」セクションを参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/types.md#the-bool-type)の「[Bool 型](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [値型](value-types.md)
- [True および False 演算子](../operators/true-false-operators.md)
