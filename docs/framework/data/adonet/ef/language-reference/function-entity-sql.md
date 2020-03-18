---
title: FUNCTION (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 0bb88992-37ed-4991-ace5-55be612a2c4d
ms.openlocfilehash: bacc773351812a5db60f493f3025c8e4b07dbaa2
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71833798"
---
# <a name="function-entity-sql"></a>FUNCTION (Entity SQL)
Entity SQL クエリ コマンドのスコープに関数を定義します。  
  
## <a name="syntax"></a>構文  
  
```sql  
FUNCTION function-name  
( [ { parameter_name <type_definition>   
        [ ,...n ]  
  ]  
) AS ( function_expression )   
  
<type_definition>::=  
    { data_type | COLLECTION ( <type_definition> )   
                | REF ( data_type )   
                | ROW ( row_expression )   
        }   
```  
  
## <a name="arguments"></a>引数  
 `function-name`  
 関数名。  
  
 `parameter-name`  
 関数のパラメーター名。  
  
 `function_expression`  
 関数である有効な Entity SQL 式。 関数内のコマンドは、関数に渡される `parameter_name` パラメーターに基づいて実行されます。  
  
 `data_type`  
 サポートされる型の名前。  
  
 コレクション (< type_definition`>`)  
 サポートされる型、行、または参照のコレクションを返す式。  
  
 REF **(** `data_type` **)**  
 エンティティ型への参照を返す式。  
  
 ROW **(** `row_expression` **)**  
 1 つまたは複数の値から構造的に型指定された匿名レコードを返す式。 詳細については、「 [ROW](row-entity-sql.md)」を参照してください。  
  
## <a name="remarks"></a>コメント  
 関数のシグネチャが異なっていれば、名前が同じ複数の関数をインラインで宣言することは可能です。 詳細については、「 [Function Overload Resolution](function-overload-resolution-entity-sql.md)」を参照してください。  
  
 関数がインラインである場合、Entity SQL コマンドに呼び出せるのは、そのコマンド内で定義された後のみです。 ただし、インライン関数を別のインライン関数内で呼び出す場合には、呼び出される関数が定義される前でも後でもかまいません。 次の例では、関数 B が定義される前に、関数 A が関数 B を呼び出しています。  
  
 `Function A() as ('A calls B. ' + B())`  
  
 `Function B() as ('B was called.')`  
  
 `A()`  
  
 詳しくは、「 [ユーザー定義関数を呼び出す方法](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd490951(v=vs.100))」をご覧ください。  
  
 関数をモデル自体で宣言することもできます。 モデルで宣言された関数は、コマンドでインラインで宣言された関数と同じように実行されます。 詳細については、「[ユーザー定義関数](user-defined-functions-entity-sql.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の Entity SQL コマンドは、関数 `Products` を定義します。この関数は、整数値を受け取って、返された製品をフィルター処理します。  
  
 [!code-sql[DP EntityServices Concepts#FUNCTION1](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#function1)]  
  
## <a name="example"></a>例  
 次の Entity SQL コマンドは、関数 `StringReturnsCollection` を定義します。この関数は、文字列のコレクションを受け取って、返された連絡先をフィルター処理します。  
  
 [!code-sql[DP EntityServices Concepts#FUNCTION2](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#function2)]  
  
## <a name="see-also"></a>参照

- [Entity SQL リファレンス](entity-sql-reference.md)
- [Entity SQL 言語](entity-sql-language.md)
