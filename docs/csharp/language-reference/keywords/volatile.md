---
title: volatile - C# リファレンス
ms.date: 10/24/2018
f1_keywords:
- volatile_CSharpKeyword
- volatile
helpviewer_keywords:
- volatile keyword [C#]
ms.assetid: 78089bc7-7b38-4cfd-9e49-87ac036af009
ms.openlocfilehash: c7a6c442c33ac2b41f652805837f455a957819de
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712846"
---
# <a name="volatile-c-reference"></a>volatile (C# リファレンス)

`volatile` キーワードは、同時に実行されている複数のスレッドによって、フィールドが変更される可能性があることを示します。 コンパイラ、ランタイム システム、さらにはハードウェアで、パフォーマンスを上げる目的でメモリの読み書き場所を再配置するかもしれません。 `volatile` が宣言されているフィールドはこのような最適化の対象になりません。 `volatile` 修飾子を追加することで、すべてのスレッドによって、他のスレッドにより実行される volatile 書き込みがその実行順序どおりに観察されます。 全ての実行スレッドから見たvolatile 書き込みの全体的順序が単一である保証はありません。

`volatile` キーワードは次の型のフィールドに使用できます。

- 参照型。
- ポインター型 (unsafe コンテキスト内)。 ポインター自体は volatile にできますが、ポインターが指しているオブジェクトは volatile にできません。 つまり、"volatile を指すポインター" は宣言できません。
- `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool` など、単純型。
- 基本型が `enum`、`byte`、`sbyte`、`short`、`ushort`、`int` のいずれかの `uint` 型。
- 参照型であることが判明しているジェネリック型パラメーター。
- <xref:System.IntPtr> および <xref:System.UIntPtr>。

`double` や `long` など、その他の型には `volatile` を指定できません。そのような型のフィールドに対する読み書きはアトミックになるとは限らないためです。 そのような型のフィールドへのマルチスレッド アクセスを保護するために、<xref:System.Threading.Interlocked> クラス メンバーを使用するか、[`lock`](lock-statement.md) ステートメントでアクセスを保護します。

`volatile` キーワードは `class` または `struct` のフィールドにのみ適用できます。 ローカル変数を `volatile` として宣言することはできません。

## <a name="example"></a>例

次の例は、public のフィールド変数を `volatile` として宣言する方法を示しています。

[!code-csharp[declareVolatile](~/samples/snippets/csharp/language-reference/keywords/volatile/Program.cs#Declaration)]

次の例は、補助スレッドつまりワーカー スレッドを作成および使用して、プライマリ スレッドとの並行処理を実行する方法を示しています。 マルチスレッドについて詳しくは、「[マネージド スレッド処理](../../../standard/threading/index.md)」をご覧ください。

[!code-csharp[declareVolatile](~/samples/snippets/csharp/language-reference/keywords/volatile/Program.cs#Volatile)]

`volatile` 修飾子を `_shouldStop` の宣言に追加することで、(前述のコードにある抜粋に似た) 同じ結果が常に得られます。 しかしながら、この修飾子が `_shouldStop` メンバーになければ、動作は予測できません。 `DoWork` メソッドでメンバー アクセスが最適化されることがありますが、古いデータが読み取られます。 マルチスレッド プログラミングの性質上、古い読み取りの数は予測できません。 プログラムを実行するたびに若干異なる結果が得られます。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# 言語仕様: volatile キーワード](../../../../_csharplang/spec/classes.md#volatile-fields)
- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [修飾子](index.md)
- [lock ステートメント](lock-statement.md)
- <xref:System.Threading.Interlocked>
