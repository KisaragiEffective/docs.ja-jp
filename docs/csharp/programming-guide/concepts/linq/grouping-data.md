---
title: データのグループ化 (C#)
ms.date: 07/20/2015
ms.assetid: e414e9e4-343a-4e6e-858f-4a30c5e64492
ms.openlocfilehash: 7ef3d3c9097d7a9478605565518ac8975feb9fe2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635744"
---
# <a name="grouping-data-c"></a>データのグループ化 (C#)
グループ化とは、各グループの要素が共通の属性を持つようにデータをグループに分ける操作を指します。  
  
 次の図は、文字のシーケンスをグループ化した結果を示しています。 各グループのキーは文字です。  
  
 ![LINQ グループ化操作を示す図。](./media/grouping-data/linq-group-operation.png)  
  
 次のセクションでは、データ要素をグループ化する標準クエリ演算子メソッドの一覧を示します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|[説明]|C# のクエリ式の構文|説明|  
|-----------------|-----------------|---------------------------------|----------------------|  
|GroupBy|共通の属性を共有する要素をグループ化します。 各グループは <xref:System.Linq.IGrouping%602> オブジェクトによって表されます。|`group … by`<br /><br /> または<br /><br /> `group … by … into …`|<xref:System.Linq.Enumerable.GroupBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.GroupBy%2A?displayProperty=nameWithType>|  
|ToLookup|キー セレクター関数に基づいて、<xref:System.Linq.Lookup%602> (一対多の辞書) に要素を挿入します。|該当しない。|<xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-example"></a>クエリ式の構文例  
 次のコード例では、`group by` 句を使用して、偶数か奇数かによってリスト内の整数をグループ化します。  
  
```csharp  
List<int> numbers = new List<int>() { 35, 44, 200, 84, 3987, 4, 199, 329, 446, 208 };  
  
IEnumerable<IGrouping<int, int>> query = from number in numbers  
                                         group number by number % 2;  
  
foreach (var group in query)  
{  
    Console.WriteLine(group.Key == 0 ? "\nEven numbers:" : "\nOdd numbers:");  
    foreach (int i in group)  
        Console.WriteLine(i);  
}  
  
/* This code produces the following output:  
  
    Odd numbers:  
    35  
    3987  
    199  
    329  
  
    Even numbers:  
    44  
    200  
    84  
    4  
    446  
    208  
*/  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [group 句](../../../language-reference/keywords/group-clause.md)
- [入れ子になったグループの作成](../../../linq/create-a-nested-group.md)
- [拡張子別にファイルをグループ化する方法 (LINQ) (C#)](./how-to-group-files-by-extension-linq.md)
- [クエリ結果のグループ化](../../../linq/group-query-results.md)
- [グループ化操作でのサブクエリの実行](../../../linq/perform-a-subquery-on-a-grouping-operation.md)
- [グループを使用して 1 つのファイルを複数のファイルに分割する方法 (LINQ) (C#)](./how-to-split-a-file-into-many-files-by-using-groups-linq.md)
