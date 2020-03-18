---
title: MULTISET (Entity SQL)
ms.date: 03/30/2017
ms.assetid: eb90a377-e47a-43a5-b308-e993b6d611e6
ms.openlocfilehash: 222be86db434b5d41c7b0536d271a3750b6afbe8
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319583"
---
# <a name="multiset-entity-sql"></a>MULTISET (Entity SQL)
値のリストからマルチセットのインスタンスを作成します。 MULTISET コンストラクターの値はすべて、互換性のある型 `T`である必要があります。 空のマルチセット コンストラクターは使用できません。  
  
## <a name="syntax"></a>構文  
  
```sql  
MULTISET ( expression [{, expression }] )  
-- or  
{ expression [{, expression }] }  
```  
  
## <a name="arguments"></a>引数  
 `expression`  
 任意の有効な値のリスト。  
  
## <a name="return-value"></a>戻り値  
 \<T > 型のマルチセットのコレクション。  
  
## <a name="remarks"></a>コメント  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、行コンストラクター、オブジェクト コンストラクター、およびマルチセット (またはコレクション) コンストラクターの 3 種類のコンストラクターが用意されています。 詳細については、「[型の構築](constructing-types-entity-sql.md)」を参照してください。  
  
 マルチセット コンストラクターは、値のリストからマルチセットのインスタンスを作成します。 このコンストラクターの値はすべて、互換性のある型である必要があります。  
  
 たとえば、次の式は整数のマルチセットを作成します。  
  
 `MULTISET(1, 2, 3)`  
  
 `{1, 2, 3}`  
  
> [!NOTE]
> 入れ子になったマルチセットリテラルは、複数のマルチセット要素がある場合にのみサポートされます。たとえば、`{{1, 2, 3}}`のようにします。 複数のマルチセット要素が外側のマルチセットに含まれている場合 ( `{{1, 2}, {3, 4}}`など)、入れ子になったマルチセット リテラルはサポートされません。  
  
## <a name="example"></a>例  
 次の Entity SQL クエリでは、MULTISET 演算子を使用して、値のリストからマルチセットのインスタンスを作成します。 このクエリは、AdventureWorks Sales Model に基づいています。 このクエリをコンパイルして実行するには、次の手順を実行します。  
  
1. 「 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)」の手順に従います。  
  
2. 次のクエリを引数として `ExecuteStructuralTypeQuery` メソッドに渡します。  
  
 [!code-sql[DP EntityServices Concepts#MULTISET](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#multiset)]  
  
## <a name="see-also"></a>参照

- [コンストラクター](constructing-types-entity-sql.md)
- [Entity SQL リファレンス](entity-sql-reference.md)
