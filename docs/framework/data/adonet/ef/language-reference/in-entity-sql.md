---
title: IN (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 51662950-ee01-4857-b7b9-311dd8515966
ms.openlocfilehash: e46db63600b6baa03697615a2f5eb9240f55d15e
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71833692"
---
# <a name="in-entity-sql"></a>IN (Entity SQL)
コレクション内に一致する値があるかどうかを調べます。  
  
## <a name="syntax"></a>構文  
  
```sql  
value [ NOT ] IN expression  
```  
  
## <a name="arguments"></a>引数  
 `value`  
 照合する値を返す任意の有効な式。  
  
 [ NOT ]  
 IN の `Boolean` 型の結果を否定することを指定します。  
  
 `expression`  
 一致の判定対象のコレクションを返す任意の有効な式。 すべての式は、 `value`と同じ型であるか、共通の基本型または派生型である必要があります。  
  
## <a name="return-value"></a>戻り値  
 値がコレクション内で見つかった場合は `true` します。値が null の場合、またはコレクションが null の場合は null です。それ以外の場合は、`false`ます。 NOT IN を使用すると、IN の結果が否定されます。  
  
## <a name="example"></a>例  
 次の Entity SQL クエリでは、IN 演算子を使用して、コレクション内に一致する値があるかどうかを調べます。 このクエリは、AdventureWorks Sales Model に基づいています。 このクエリをコンパイルして実行するには、次の手順を実行します。  
  
1. 「 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)」の手順に従います。  
  
2. 次のクエリを引数として `ExecuteStructuralTypeQuery` メソッドに渡します。  
  
 [!code-sql[DP EntityServices Concepts#IN](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#in)]  
  
## <a name="see-also"></a>参照

- [Entity SQL リファレンス](entity-sql-reference.md)
