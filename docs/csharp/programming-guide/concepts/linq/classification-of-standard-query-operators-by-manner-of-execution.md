---
title: 実行方法による標準クエリ演算子の分類 (C#)
ms.date: 07/20/2015
ms.assetid: b9435ce5-a7cf-4182-9f01-f3468a5533dc
ms.openlocfilehash: ccf8fced5c92ceaaf84f9240e235da0e2b56ac1e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69924289"
---
# <a name="classification-of-standard-query-operators-by-manner-of-execution-c"></a>実行方法による標準クエリ演算子の分類 (C#)
標準クエリ演算子メソッドの LINQ to Objects 実装は、主に 2 とおりの方法 (即時と遅延) で実行されます。 遅延実行を使用するクエリ演算子は、さらに 2 つのカテゴリ (ストリーミングと非ストリーミング) に分けることができます。 それぞれのクエリ演算子がどのように動作するかを把握しておくと、指定したクエリの結果を理解するうえで役立ちます。 これは、データ ソースが変更される場合や、別のクエリに基づいてさらにクエリを作成する場合に特に便利です。 このトピックでは、標準クエリ演算子を、その実行方法に基づいて分類します。  
  
## <a name="manners-of-execution"></a>実行方法  
  
### <a name="immediate"></a>即時  
 即時実行とは、クエリが宣言されたコード内の位置で、データ ソースが読み取られ、演算が実行されることを意味します。 列挙できない単一の結果を返す標準クエリ演算子は、すべて即時に実行されます。  
  
### <a name="deferred"></a>遅延  
 遅延実行とは、クエリが宣言されたコード内の位置では演算が実行されないことを意味します。 演算は、`foreach` ステートメントを使用するなどの方法により、クエリ変数が列挙されたときにのみ実行されます。 つまり、クエリの実行結果は、クエリの定義時ではなくクエリの実行時のデータ ソースの内容に依存します。 クエリ変数が複数回列挙される場合は、そのたびに結果が変わる可能性があります。 戻り値の型が <xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Linq.IOrderedEnumerable%601> の標準クエリ演算子は、ほとんどが遅延実行されます。  
  
 遅延実行を使用するクエリ演算子は、さらにストリーミングか非ストリーミングに分類できます。  
  
#### <a name="streaming"></a>ストリーミング  
 ストリーミング演算子では、要素を生成する前にすべてのソース データを読み取る必要はありません。 実行時に、ストリーミング演算子は読み取ったソース要素ごとに演算を実行し、必要に応じて要素を生成します。 ストリーミング演算子は、結果の要素を生成できるまでソース要素の読み取りを続行します。 つまり、結果の要素を 1 つ生成するために複数のソース要素が読み取られる場合があります。  
  
#### <a name="non-streaming"></a>非ストリーミング  
 非ストリーミング演算子では、結果の要素を生成する前にすべてのソース データを読み取る必要があります。 並べ替えやグループ化などの演算はこのカテゴリに分類されます。 実行時に、非ストリーミング クエリ演算子はすべてのソース データを読み取ってデータ構造に格納し、演算を実行して結果の要素を生成します。  
  
## <a name="classification-table"></a>分類表  
 次の表では、各標準クエリ演算子メソッドを、その実行方法に基づいて分類しています。  
  
> [!NOTE]
> 2 つの列にマークが付けられている演算子では、2 つの入力シーケンスが演算に使用され、各シーケンスの評価は異なります。 この場合、遅延実行のストリーミングで評価されるのは、常にパラメーター リストの最初のシーケンスになります。  
  
|標準クエリ演算子|戻り値の型|即時実行|遅延実行 (ストリーミング)|遅延実行 (非ストリーミング)|  
|-----------------------------|-----------------|-------------------------|----------------------------------|---------------------------------------|  
|<xref:System.Linq.Enumerable.Aggregate%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.All%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Any%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.AsEnumerable%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Average%2A>|1 つの数値|X|||  
|<xref:System.Linq.Enumerable.Cast%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Concat%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Contains%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Count%2A>|<xref:System.Int32>|X|||  
|<xref:System.Linq.Enumerable.DefaultIfEmpty%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Distinct%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.ElementAt%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.ElementAtOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.Empty%2A>|<xref:System.Collections.Generic.IEnumerable%601>|X|||  
|<xref:System.Linq.Enumerable.Except%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.First%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.FirstOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.GroupBy%2A>|<xref:System.Collections.Generic.IEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.GroupJoin%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
<xref:System.Linq.Enumerable.Intersect%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.Join%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.Last%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.LastOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.LongCount%2A>|<xref:System.Int64>|X|||  
|<xref:System.Linq.Enumerable.Max%2A>|1 つの数値、TSource、または TResult|X|||  
|<xref:System.Linq.Enumerable.Min%2A>|1 つの数値、TSource、または TResult|X|||  
|<xref:System.Linq.Enumerable.OfType%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.OrderBy%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.OrderByDescending%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.Range%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Repeat%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Reverse%2A>|<xref:System.Collections.Generic.IEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.Select%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SelectMany%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SequenceEqual%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Single%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.SingleOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.Skip%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SkipWhile%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Sum%2A>|1 つの数値|X|||  
|<xref:System.Linq.Enumerable.Take%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
<xref:System.Linq.Enumerable.TakeWhile%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.ThenBy%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.ThenByDescending%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.ToArray%2A>|TSource 配列|X|||  
|<xref:System.Linq.Enumerable.ToDictionary%2A>|<xref:System.Collections.Generic.Dictionary%602>|X|||  
|<xref:System.Linq.Enumerable.ToList%2A>|<xref:System.Collections.Generic.IList%601>|X|||  
|<xref:System.Linq.Enumerable.ToLookup%2A>|<xref:System.Linq.ILookup%602>|X|||  
|<xref:System.Linq.Enumerable.Union%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Where%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
  
## <a name="see-also"></a>参照

- <xref:System.Linq.Enumerable>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [標準クエリ演算子のクエリ式構文 (C#)](./query-expression-syntax-for-standard-query-operators.md)
- [LINQ to Objects (C#)](./linq-to-objects.md)
