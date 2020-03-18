---
title: stackalloc 演算子 - C# リファレンス
ms.date: 09/20/2019
f1_keywords:
- stackalloc_CSharpKeyword
helpviewer_keywords:
- stackalloc operator [C#]
ms.openlocfilehash: 9c9767e0c9945a9589d049fa7abba192cb928ad5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78846254"
---
# <a name="stackalloc-operator-c-reference"></a>stackalloc 演算子 (C# リファレンス)

`stackalloc` 演算子は、スタックにメモリ ブロックを割り当てます。 メソッドの実行中に作成された、スタックに割り当てられたメモリ ブロックは、そのメソッドが戻るときに自動的に破棄されます。 `stackalloc` 演算子を使用して割り当てられたメモリを明示的に開放することはできません。 スタックに割り当てられたメモリ ブロックは、[ガベージ コレクション](../../../standard/garbage-collection/index.md)の対象外であり、[`fixed` ステートメント](../keywords/fixed-statement.md)を使用してピン留めする必要はありません。

`stackalloc` 演算子の結果を、次のいずれかの型の変数に割り当てることができます。

- C# 7.2 以降では <xref:System.Span%601?displayProperty=nameWithType> または <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>。次に例を示します。

  [!code-csharp[stackalloc span](snippets/StackallocOperator.cs#AssignToSpan)]

  スタックに割り当てられたメモリ ブロックを [ 変数または ](../keywords/unsafe.md) 変数に割り当てるときに、<xref:System.Span%601>unsafe<xref:System.ReadOnlySpan%601> コンテキストを使用する必要はありません。

  これらの型を操作するときに、次の例に示すように、`stackalloc`条件[式または代入式の中で ](conditional-operator.md) 式を使用できます。

  [!code-csharp[stackalloc expression](snippets/StackallocOperator.cs#AsExpression)]

  C# 8.0 以降では、次の例のように、`stackalloc` または <xref:System.Span%601> の変数が許可されるところでは、他の式の中で <xref:System.ReadOnlySpan%601> 式を使用できます。

  [!code-csharp[stackalloc in nested expressions](snippets/StackallocOperator.cs#Nested)]

  > [!NOTE]
  > スタックに割り当てられたメモリを操作するときは、できるだけ <xref:System.Span%601> 型または <xref:System.ReadOnlySpan%601> 型を使用することをお勧めします。

- [ポインター型](../../programming-guide/unsafe-code-pointers/pointer-types.md)。次に例を示します。

  [!code-csharp[stackalloc pointer](snippets/StackallocOperator.cs#AssignToPointer)]

  前の例に示したように、ポインター型を操作するときは、`unsafe` コンテキストを使用する必要があります。

  ポインター型の場合、ローカル変数宣言でのみ `stackalloc` 式を使用し、変数を初期化できます。

新しく割り当てられたメモリの内容は未定義です。 C# 7.3 以降、配列初期化子構文を使用して、新しく割り当てられたメモリの内容を定義できます。 これを実行するさまざまな方法を次の例に示します。

[!code-csharp[stackalloc initialization](snippets/StackallocOperator.cs#StackallocInit)]

`stackalloc T[E]` の式では、`T` が[アンマネージド型](../builtin-types/unmanaged-types.md)であり、`E` は型 [int](../builtin-types/integral-numeric-types.md) の式である必要があります。

## <a name="security"></a>Security

`stackalloc` を使用すると、共通言語ランタイム (CLR) のバッファー オーバーラン検出機能が自動的に有効になります。 バッファー オーバーランが検出されると、悪意のあるコードが実行される可能性を最小限に抑えるために、プロセスはできる限り迅速に終了されます。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/unsafe-code.md#stack-allocation)に関するページの「[スタック割り当て](~/_csharplang/spec/introduction.md)」セクションと、[入れ子になった `stackalloc` の許可](~/_csharplang/proposals/csharp-8.0/nested-stackalloc.md)に関する機能提案メモをご覧ください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ポインターに関連する演算子](pointer-related-operators.md)
- [ポインター型](../../programming-guide/unsafe-code-pointers/pointer-types.md)
- [メモリおよびスパンに関連する型](../../../standard/memory-and-spans/index.md)
