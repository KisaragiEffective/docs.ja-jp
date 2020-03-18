---
title: データのフィルター処理 (C#)
ms.date: 07/20/2015
ms.assetid: fbaece0d-0f23-47f7-89c5-f3ea8db692b6
ms.openlocfilehash: 74399244990f8ff2deaa1d10576ea94a57c16bee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75346991"
---
# <a name="filtering-data-c"></a>データのフィルター処理 (C#)
フィルター処理とは、特定の条件を満たす要素のみが含まれるように結果セットを限定する操作のことです。 選択とも呼ばれます。  
  
 次の図は、文字のシーケンスをフィルター処理した結果を示したものです。 フィルター処理操作の述語では、文字が "A" でなければならないことが指定されています。  
  
 ![LINQ のフィルター操作を示す図。](./media/filtering-data/linq-filter-operation.png)  
  
 次のセクションでは、選択を実行する標準クエリ演算子メソッドの一覧を示します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|[説明]|C# のクエリ式の構文|説明|  
|-----------------|-----------------|---------------------------------|----------------------|  
|OfType|指定した型にキャストできるかどうかにより、値を選択します。|該当しない。|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|  
|Where|述語関数に基づいて値を選択します。|`where`|<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Where%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-example"></a>クエリ式の構文例  
 次の例では、`where` 句を使って、配列から特定の長さを持つ文字列をフィルター処理します。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            where word.Length == 3  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
*/  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [where 句](../../../language-reference/keywords/where-clause.md)
- [実行時における述語フィルターの動的指定](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
- [リフレクションを使用してアセンブリのメタデータにクエリを実行する方法 (LINQ) (C#)](./how-to-query-an-assembly-s-metadata-with-reflection-linq.md)
- [指定された属性または名前のファイルを照会する方法 (C#)](./how-to-query-for-files-with-a-specified-attribute-or-name.md)
- [任意のワードまたはフィールドを基準にテキスト データの並べ替えまたはフィルター処理を実行する方法 (LINQ) (C#)](./how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
