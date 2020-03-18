---
title: NULL 比較
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ef88af8c-8dfe-4556-8b56-81df960a900b
ms.openlocfilehash: 8eca2ee1afec5662e40d4f43347c469bd538c066
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319493"
---
# <a name="null-comparisons"></a>NULL 比較
データ ソースの `null` 値は不明な値を表します。 LINQ to Entities のクエリでは、null 値をチェックして、特定の計算または比較が、有効な、または null 以外のデータを含む行に対してのみ実行されるようにすることができます。 ただし、CLR の NULL セマンティクスは、データ ソースの NULL セマンティクスとは異なる場合があります。 ほとんどのデータベースでは、3 値論理を使用して NULL 比較を処理します。 つまり、null 値に対する比較は `true` または `false`には評価されず、`unknown`に評価されます。 これは、多くの場合は ANSI NULL の実装ですが、そうでない場合もあります。  
  
 SQL Server の既定では、NULL = NULL の比較は NULL 値を返します。 次の例では、`ShipDate` が null である行が結果セットから除外され、Transact-sql ステートメントが0行を返します。  
  
```sql  
-- Find order details and orders with no ship date.  
SELECT h.SalesOrderID  
FROM Sales.SalesOrderHeader h  
JOIN Sales.SalesOrderDetail o ON o.SalesOrderID = h.SalesOrderID  
WHERE h.ShipDate IS Null  
```  
  
 これは、NULL = NULL の比較が true を返す CLR の NULL セマンティクスとは大きく異なります。  
  
 次の LINQ クエリは CLR で表されますが、データ ソースで実行されます。 CLR セマンティクスがデータ ソースで使用される保証はないため、動作は予測できません。  
  
 [!code-csharp[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#joinonnull)]
 [!code-vb[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#joinonnull)]  
  
## <a name="key-selectors"></a>キー セレクター  
 *キーセレクター*は、要素からキーを抽出するために標準クエリ演算子で使用される関数です。 キー セレクター関数では、式を定数と比較できます。 式が NULL 定数と比較される場合、または 2 つの NULL 定数が比較される場合、CLR の NULL セマンティクスが使用されます。 データ ソースの NULL 値を持つ 2 つの列が比較される場合は、ストア NULL セマンティクスが使用されます。 キー セレクターは、<xref:System.Linq.Queryable.GroupBy%2A> など、グループ化や並べ替えの標準クエリ演算子で使用される場合が多く、クエリ結果の並べ替えやグループ化に使用するキーを選択できます。  
  
## <a name="null-property-on-a-null-object"></a>NULL オブジェクトの NULL プロパティ  
 Entity Framework では、null オブジェクトのプロパティは null です。 CLR で NULL オブジェクトのプロパティを参照しようとすると、<xref:System.NullReferenceException> が返されます。 LINQ クエリに NULL オブジェクトのプロパティが含まれている場合、動作の一貫性が失われることがあります。  
  
 たとえば、次のクエリでは、`NewProduct` へのキャストはコマンド ツリー レイヤーで行われ、`Introduced` プロパティが NULL になる可能性があります。 <xref:System.DateTime> の比較が true に評価されるように NULL 比較がデータベースで定義されている場合に、その行が含められます。  
  
 [!code-csharp[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#castresultsisnull)]
 [!code-vb[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#castresultsisnull)]  
  
## <a name="passing-null-collections-to-aggregate-functions"></a>集計関数に NULL コレクションを渡す  
 LINQ to Entities では、`IQueryable` をサポートするコレクションを集計関数に渡すと、集計操作がデータベースで実行されます。 メモリ内で実行されたクエリの結果と、データベースで実行されたクエリの結果が異なる場合があります。 メモリ内クエリで一致するものがない場合、クエリは0を返します。 データベースでは、これと同じクエリから `null` が返されます。 `null` 値が LINQ 集計関数に渡されると、例外がスローされます。 使用可能な `null` 値を受け入れるには、クエリ結果を受け取る型の型とプロパティを null 許容型にキャストします。  
  
## <a name="see-also"></a>参照

- [LINQ to Entities クエリ内の式](expressions-in-linq-to-entities-queries.md)
