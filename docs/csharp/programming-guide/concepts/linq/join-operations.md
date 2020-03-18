---
title: Join 演算 (C#)
ms.date: 07/20/2015
ms.assetid: 5105e0da-1267-4c00-837a-f0e9602279b8
no-loc:
- Join
- GroupJoin
ms.openlocfilehash: 6e2ec1a0c8120f6869b7c0a196b77d118762a8dd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76868006"
---
# <a name="join-operations-c"></a>結合操作 (C#)

2 つのデータ ソースの "*結合*" とは、あるデータ ソースのオブジェクトを、共通の属性を共有する別のデータ ソースのオブジェクトと関連付けることです。  
  
 相互に直接の関連がない 2 つのデータ ソースを対象とするクエリにおいて、結合は重要な操作になります。 オブジェクト指向プログラミングでは、これは一方向の関係における逆の方向など、モデル化されていないオブジェクト間の相関関係を意味する場合があります。 一方向の関係の例として、City 型のプロパティを持つ Customer クラスがあるとします。ただし、City クラスには、Customer オブジェクトのコレクションを表すプロパティはありません。 City オブジェクトのリストから各都市のすべての顧客を取得する場合は、結合演算を使用して顧客を検索できます。  
  
 LINQ framework で用意された結合メソッドは <xref:System.Linq.Enumerable.Join%2A> と <xref:System.Linq.Enumerable.GroupJoin%2A> です。 この 2 つのメソッドは、等結合 (キーが等しいかどうかに基づいて 2 つのデータ ソースを対応させる結合) を実行します。 (比較に関して、Transact-SQL では、"小なり" 演算子などの "等値" 以外の結合演算子もサポートされます)。リレーショナル データベース用語で説明すると、<xref:System.Linq.Enumerable.Join%2A> は内部結合 (両方のデータ セットで一致するオブジェクトだけが返される結合) を実装します。 リレーショナル データベース用語で <xref:System.Linq.Enumerable.GroupJoin%2A> メソッドに直接相当するものはありませんが、このメソッドは内部結合と左外部結合のスーパーセットを実装します。 左外部結合とは、最初 (左側) のデータ ソースの各要素を返す結合です。これらの要素は、もう一方のデータ ソースの要素と相関関係がなくても返されます。  
  
 次の図は、2 つのセットと、内部結合または左外部結合としてこれらのセットに含まれている要素の概念図を示しています。  
  
 ![内側&#47;外側を示す 2 つの重なり合う円。](./media/join-operations/join-method-overlapping-circles.png)  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|説明|C# のクエリ式の構文|説明|  
|-----------------|-----------------|---------------------------------|----------------------|  
|Join|キー セレクター関数に基づいて 2 つのシーケンスを結合し、値のペアを抽出します。|`join … in … on … equals …`|<xref:System.Linq.Enumerable.Join%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Join%2A?displayProperty=nameWithType>|  
|GroupJoin|キー セレクター関数に基づいて 2 つのシーケンスを結合し、各要素について結果として得られる一致をグループ化します。|`join … in … on … equals … into …`|<xref:System.Linq.Enumerable.GroupJoin%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.GroupJoin%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>クエリ式の構文例
  
### Join  
  
次の例では、`join … in … on … equals …` 句を使用し、特定の値に基づいて 2 つのシーケンスを結合します。
  
[!code-csharp[Join](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csLINQJoinOperation/CS/JoinOperation.cs#Join)]  

### GroupJoin  

次の例では、`join … in … on … equals … into …` 句を使用し、特定の値に基づいて 2 つのシーケンスを結合し、要素ごとに結果として得られる一致をグループ化します。
  
[!code-csharp[GroupJoin](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csLINQJoinOperation/CS/JoinOperation.cs#GroupJoin)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Linq>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [匿名型](../../classes-and-structs/anonymous-types.md)
- [結合およびクロス積クエリの作成](../../../../framework/data/adonet/sql/linq/formulate-joins-and-cross-product-queries.md)
- [join 句](../../../language-reference/keywords/join-clause.md)
- [複合キーを使用した Join](../../../linq/join-by-using-composite-keys.md)
- [異種ファイルのコンテンツを結合する方法 (LINQ) (C#)](./how-to-join-content-from-dissimilar-files-linq.md)
- [join 句の結果の順序指定](../../../linq/order-the-results-of-a-join-clause.md)
- [カスタム結合操作の実行](../../../linq/perform-custom-join-operations.md)
- [グループ化結合の実行](../../../linq/perform-grouped-joins.md)
- [内部結合の実行](../../../linq/perform-inner-joins.md)
- [左外部結合の実行](../../../linq/perform-left-outer-joins.md)
- [複数のソースからオブジェクト コレクションにデータを設定する方法 (LINQ) (C#)](./how-to-populate-object-collections-from-multiple-sources-linq.md)
