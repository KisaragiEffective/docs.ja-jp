---
title: 要素操作 (C#)
ms.date: 10/03/2018
ms.assetid: 283206c9-3246-4c48-b01a-d9de409a7231
ms.openlocfilehash: e9ec41793afffe60a7184622f91b5fc023e0958f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75345159"
---
# <a name="element-operations-c"></a>要素操作 (C#)

要素操作では、シーケンスから単一の特定の要素が返されます。  
  
 次のセクションでは、要素操作を実行する標準クエリ演算子のメソッドの一覧を示します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|[説明]|C# のクエリ式の構文|説明|  
|-----------------|-----------------|---------------------------------|----------------------|  
|ElementAt|コレクション内の指定されたインデックス位置にある要素を返します。|該当しない。|<xref:System.Linq.Enumerable.ElementAt%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAt%2A?displayProperty=nameWithType>|  
|ElementAtOrDefault|コレクション内の指定したインデックス位置にある要素を返します。インデックスが範囲外の場合は既定値を返します。|該当しない。|<xref:System.Linq.Enumerable.ElementAtOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAtOrDefault%2A?displayProperty=nameWithType>|  
|First|コレクションの最初の要素、または条件を満たす最初の要素を返します。|該当しない。|<xref:System.Linq.Enumerable.First%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.First%2A?displayProperty=nameWithType>|  
|FirstOrDefault|コレクションの最初の要素、または条件を満たす最初の要素を返します。 そのような要素が存在しない場合は、既定値を返します。|該当しない。|<xref:System.Linq.Enumerable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%60%601%28System.Linq.IQueryable%7B%60%600%7D%29?displayProperty=nameWithType>|  
|末尾|コレクションの最後の要素、または条件を満たす最後の要素を返します。|該当しない。|<xref:System.Linq.Enumerable.Last%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Last%2A?displayProperty=nameWithType>|  
|LastOrDefault|コレクションの最後の要素、または条件を満たす最後の要素を返します。 そのような要素が存在しない場合は、既定値を返します。|該当しない。|<xref:System.Linq.Enumerable.LastOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LastOrDefault%2A?displayProperty=nameWithType>|  
|Single|コレクションの唯一の要素、または条件を満たす唯一の要素を返します。 要素がない場合、または複数の要素が返される場合は、<xref:System.InvalidOperationException> をスローします。 |該当しない。|<xref:System.Linq.Enumerable.Single%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Single%2A?displayProperty=nameWithType>|  
|SingleOrDefault|コレクションの唯一の要素、または条件を満たす唯一の要素を返します。 返す要素がない場合は、既定値を返します。 複数の要素が返される場合は、<xref:System.InvalidOperationException> をスローします。 |該当しない。|<xref:System.Linq.Enumerable.SingleOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SingleOrDefault%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [ディレクトリ ツリー内で最もサイズの大きいファイルを照会する方法 (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)
