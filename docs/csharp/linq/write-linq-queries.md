---
title: C# での LINQ クエリの作成
description: C# で LINQ クエリを作成する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 30703f79-cf3a-4d02-b892-c95d58a1d9ed
ms.openlocfilehash: ed32543b0422e0664a8577f2c27f7c7c00a719a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "65632873"
---
# <a name="write-linq-queries-in-c"></a>C\# での LINQ クエリの作成

この記事では、C# で LINQ クエリを作成できる 3 つの方法を示しています。

1. クエリ構文を使用する。

2. メソッド構文を使用する。

3. クエリ構文とメソッド構文を組み合わせて使用する。

以下の例では、上記の各アプローチを使用したシンプルな LINQ クエリを示します。 一般に、可能であれば常に (1) を使用し、(2) と (3) は必要に応じて使用するというのがルールになっています。

> [!NOTE]
> これらのクエリでは、シンプルなメモリ内コレクションに対して操作を実行します。ただし、基本的な構文は、LINQ to Entities および LINQ to XML で使用されるのと同じものです。

## <a name="example---query-syntax"></a>例 - クエリ構文

ほとんどのクエリ記述については、*クエリの構文*を使用して*クエリ式*を作成することをお勧めします。 次の例は、3 つのクエリ式を示しています。 1 つ目のクエリ式は、`where` 句を使用した条件を適用することで、結果をフィルター処理または制限する方法を示しています。 このクエリは、値が 7 より大きいか、または 3 より小さいソース シーケンス内のすべての要素を返します。 2 つ目の式は、返された結果を並べ替える方法を示しています。 3 つ目の式は、キーに基づいて結果をグループ化する方法を示しています。 このクエリは、単語の頭文字に基づいて 2 つのグループを返します。

[!code-csharp[csProgGuideLINQ#5](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_1.cs)]

クエリの型が <xref:System.Collections.Generic.IEnumerable%601> であることに注意してください。 これらのクエリはすべて、次の例で示すように、`var` を使用しても記述できます。

`var query = from num in numbers...`

上記の各例では、`foreach` ステートメントまたはその他のステートメントでクエリ変数を反復処理するまで、クエリは実際には実行されません。 詳しくは、「[LINQ クエリの概要](../programming-guide/concepts/linq/introduction-to-linq-queries.md)」をご覧ください。

## <a name="example---method-syntax"></a>例 - メソッド構文

一部のクエリ操作は、メソッド呼び出しとして表す必要があります。 このようなメソッドで最も一般的なものは、<xref:System.Linq.Enumerable.Sum%2A>、<xref:System.Linq.Enumerable.Max%2A>、<xref:System.Linq.Enumerable.Min%2A>、<xref:System.Linq.Enumerable.Average%2A> のような単一の数値を返すメソッドです。 これらのメソッドは、単一の値のみを表し、追加のクエリ操作のソースとしては使用できないため、常に、どのクエリよりも後に呼び出す必要があります。 次の例は、クエリ式でのメソッド呼び出しを示したものです。

[!code-csharp[csProgGuideLINQ#6](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_2.cs)]

メソッドに Action または Func パラメーターがある場合、それらは[ラムダ](../programming-guide/statements-expressions-operators/lambda-expressions.md)式の形式で提供されます。次に例を示します。

[!code-csharp[csProgGuideLINQ#7](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_3.cs)]

上記のクエリでは Query #4 だけが直ちに実行されます。 これは、それがジェネリック <xref:System.Collections.Generic.IEnumerable%601> コレクションではなく、単一値を返すためです。 メソッド自体は、値を計算するために `foreach` を使用する必要があります。

上記の各クエリは、[var](../language-reference/keywords/var.md) による暗黙的型指定を使用しても記述できます。次に例を示します。

[!code-csharp[csProgGuideLINQ#8](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_4.cs)]

## <a name="example---mixed-query-and-method-syntax"></a>例 - クエリとメソッド構文の併用

この例では、クエリ句の結果に対してメソッド構文を使用する方法を示します。 方法は、クエリ式をかっこで囲み、ドット演算子を適用して、メソッドを呼び出すだけです。 次の例では、Query #7 は、値が 3 ～ 7 である数値のカウントを返します。 ただし一般的には、メソッド呼び出しの結果の格納には、2 番目の変数を使うほうが賢明です。 この方法のほうが、クエリをクエリ結果と混同する恐れがありません。

[!code-csharp[csProgGuideLINQ#9](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_5.cs)]

Query #7 はコレクションではなく単一の値を返すので、クエリはすぐに実行されます。

上記のクエリは、`var` による暗黙的型指定を使用しても記述できます。次に例を示します。

```csharp
var numCount = (from num in numbers...
```

次のように、メソッド構文で記述することもできます。

```csharp
var numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

次のように、明示的な型指定を使用して記述することもできます。

```csharp
int numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

## <a name="see-also"></a>参照

- [チュートリアル: C# でのクエリの作成](../programming-guide/concepts/linq/walkthrough-writing-queries-linq.md)
- [統合言語クエリ (LINQ)](index.md)
- [where 句](../language-reference/keywords/where-clause.md)
