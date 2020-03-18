---
title: クエリ式の例外の処理 (C# での LINQ)
description: C# で LINQ クエリ式の例外を処理する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 2bf0c397-13fb-4f68-bc2b-531c6c88a167
ms.openlocfilehash: f900669412026e69598d3939c51ff8208b51b7ec
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688502"
---
# <a name="handle-exceptions-in-query-expressions"></a>クエリ式の例外の処理

クエリ式のコンテキストで任意のメソッドを呼び出すことができます。 ただし、データ ソースのコンテンツが変更されたり例外がスローされたりするなどの副作用が生じる可能性のあるクエリ式では、メソッドを呼び出さないようにすることをお勧めします。 この例では、クエリ式でメソッドを呼び出すときに、例外処理に関する .NET の全般的なガイドラインに違反することなく例外の発生を避ける方法を示します。 ガイドラインでは、特定のコンテキストで特定の例外がスローされる理由がわかっているときにその例外をキャッチすることは認められています。 詳細については、「[例外の推奨事項](../../standard/exceptions/best-practices-for-exceptions.md)」を参照してください。

最後の例では、クエリの実行中に例外をスローする必要がある場合の処理方法を示します。

## <a name="example"></a>例

次の例では、例外処理コードをクエリ式の外側に移動する方法を説明します。 この方法は、メソッドがクエリのローカル変数に依存しない場合にのみ可能です。

[!code-csharp[csProgGuideLINQ#10](~/samples/snippets/csharp/concepts/linq/how-to-handle-exceptions-in-query-expressions_1.cs)]

## <a name="example"></a>例

場合によっては、クエリ内からスローされる例外に対する最適な応答は、クエリの実行をすぐに停止することです。 次の例では、クエリ本体の内部からスローされる例外を処理する方法を示します。 `SomeMethodThatMightThrow` で、クエリの実行を停止することが必要な例外が発生する可能性があるとします。

`try` ブロックは `foreach` ループを囲み、クエリ自体を囲むのではありません。 その理由は、`foreach` ループがクエリの実際の実行ポイントであるためです。 詳細については、「[LINQ クエリの概要](../programming-guide/concepts/linq/introduction-to-linq-queries.md)」を参照してください。

[!code-csharp[csProgGuideLINQ#12](~/samples/snippets/csharp/concepts/linq/how-to-handle-exceptions-in-query-expressions_2.cs)]

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
