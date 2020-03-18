---
title: GROUP BY (Entity SQL)
ms.date: 03/30/2017
ms.assetid: cf4f4972-4724-4945-ba44-943a08549139
ms.openlocfilehash: 711fbdc2d51177037cf349150c3431de14b11974
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71833777"
---
# <a name="group-by-entity-sql"></a>GROUP BY (Entity SQL)
クエリ ([SELECT](select-entity-sql.md)) 式によって返されるオブジェクトをグループ化するよう指定します。  
  
## <a name="syntax"></a>構文  
  
```sql  
[ GROUP BY aliasedExpression [ ,...n ] ]  
```  
  
## <a name="arguments"></a>引数  
 `aliasedExpression`  
 グループ化の実行対象となる有効なクエリ式。 `expression` には、プロパティを指定することも、FROM 句から返されたプロパティを参照する非集計式を指定することもできます。 GROUP BY 句内の各式は、等価かどうかを比較できる型に評価される必要があります。 通常、これらの型は数値、文字列、日付などのスカラー プリミティブです。 コレクション別にグループ化することはできません。  
  
## <a name="remarks"></a>コメント  
 Select 句に集計関数が含まれている場合 \<選択リスト > では、group BY によって各グループの集計値が計算されます。 GROUP BY を指定する場合は、選択リスト内の非集計式内の各プロパティ名が GROUP BY リストに含まれるか、GROUP BY 式が選択リスト式に正確に一致する必要があります。  
  
> [!NOTE]
> ORDER BY 句を指定しない場合、GROUP BY 句でグループが返される順序には特に決まりはありません。 データの特定の順序を指定するには、常に ORDER BY 句を使用することをお勧めします。  
  
 GROUP BY 句を明示的または暗黙的に指定した場合 (たとえばクエリ内の HAVING 句)、現在のスコープは非表示になり、新しいスコープが導入されます。  
  
 SELECT 句、HAVING 句、および ORDER BY 句は、FROM 句で指定された要素名を参照することはできなくなります。 ユーザーが参照できるのは、グループ化式自体だけです。 このためには、新しい名前 (別名) を各グループ化式に割り当てます。 グループ化式に別名が指定されていない場合、 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、次の例に示すように別名生成規則を使用することによって別名が生成されます。  
  
 `SELECT g1, g2, ...gn FROM c as c1`  
  
 `GROUP BY e1 as g1, e2 as g2, ...en as gn`  
  
 GROUP BY 句の式は、同じ GROUP BY 句で前に定義された名前を参照できません。  
  
 名前のグループ化に加え、SELECT 句、HAVING 句、および ORDER BY 句で集計を指定することもできます。 集計には、グループの各要素について評価される式が含まれます。 集計演算子により、これらのすべての式の値の数が減少します (ただし、必ずしも単一の値になるとは限りません)。 集計式は、現在のスコープに表示される基の要素名、または GROUP BY 句自体によって導入された新しい任意の名前を参照できます。 集計は SELECT 句、HAVING 句、および ORDER BY 句に含まれますが、次の例で示すとおり、実際にはグループ化式と同じスコープで評価されます。  
  
 `SELECT name, sum(o.Price * o.Quantity) as total`  
  
 `FROM orderLines as o`  
  
 `GROUP BY o.Product as name`  
  
 このクエリは GROUP BY 句を使用して、注文した全製品の価格のレポートを製品別に分類して生成します。 グループ化式の一部として、製品に `name` という名前を付け、SELECT リストでその名前を参照します。 さらに、注文明細の価格と数量を内部で参照する SELECT リスト内の集計 `sum` を指定します。  
  
 各 GROUP By キー式には、入力スコープへの参照が少なくとも 1 つ必要です。  
  
```sql  
SELECT FROM Persons as P  
GROUP BY Q + P   -- GOOD  
GROUP BY Q   -- BAD  
GROUP BY 1   -- BAD, a constant is not allowed  
```  
  
 GROUP BY を使用する場合の例については、「 [HAVING](having-entity-sql.md)」をご覧ください。  
  
## <a name="example"></a>例  
 次の Entity SQL クエリでは、GROUP BY 演算子を使用して、クエリによって返されるオブジェクトをグループ化するよう指定します。 このクエリは、AdventureWorks Sales Model に基づいています。 このクエリをコンパイルして実行するには、次の手順を実行します。  
  
1. [「方法: PrimitiveType の結果を返すクエリを実行](../how-to-execute-a-query-that-returns-primitivetype-results.md)する」の手順に従います。  
  
2. 次のクエリを引数として `ExecutePrimitiveTypeQuery` メソッドに渡します。  
  
 [!code-sql[DP EntityServices Concepts#GROUPBY](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#groupby)]  
  
## <a name="see-also"></a>参照

- [Entity SQL リファレンス](entity-sql-reference.md)
- [クエリ式](query-expressions-entity-sql.md)
