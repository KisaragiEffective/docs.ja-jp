---
title: 集計関数 (Entity Framework 用 SqlClient)
ms.date: 03/30/2017
ms.assetid: 03303f01-b591-4efc-9875-f9c608edff0b
ms.openlocfilehash: 3dbd4c0a24a5fc41153ea16747325e824669b0e5
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71700054"
---
# <a name="aggregate-functions-sqlclient-for-entity-framework"></a>集計関数 (Entity Framework 用 SqlClient)
.NET Framework Data Provider for SQL Server (SqlClient) には、集計関数が用意されています。 集計関数は、一連の入力値に対して計算を実行し、値を返します。 これらの関数は、SqlClient の SqlServer 名前空間に存在します。 Entity Framework は、プロバイダーの名前空間プロパティを使用することにより、型や関数など、特定のコンストラクターに対してこのプロバイダーによってどのプレフィックスが使用されているかを特定できます。  
  
 次に、SqlClient 集計関数を示します。  

## <a name="avgexpression"></a>AVG (式)

コレクション内の値の平均値を返します。 Null 値は無視されます。

**Arguments**

`Int32`、`Int64`、`Double`、および `Decimal`。

**戻り値**

`expression` の型。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_AVG](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_avg)]

## <a name="checksum_aggcollection"></a>CHECKSUM_AGG (コレクション)
 
 コレクション内にある値のチェックサムを返します。 Null 値は無視されます。
 
 **Arguments**
 
 コレクション (`Int32`)。
 
 **戻り値**
 
 `Int32`。
 
 **例**
 
[!code-sql[DP EntityServices Concepts#SQLSERVER_CHECKSUM](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_checksum)]
   
## <a name="countexpression"></a>COUNT (式)

コレクション内のアイテムの数を `Int32` 型の値として返します。

**Arguments**

T >\<のコレクション。ここで、T は次のいずれかの型になります。

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`|`Guid` (SQL Server 2000 では返されません)|

**戻り値**

`Int32`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_COUNT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_count)]
 
## <a name="count_bigexpression"></a>COUNT_BIG (式)
 
コレクション内のアイテムの数を `bigint` 型の値として返します。
 
 **Arguments**
 
 Collection (T)。ここで、T は次のいずれかの型になります。
 
 |   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`|`Guid` (SQL Server 2000 では返されません)|

**戻り値**

`Int64`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_COUNTBIG](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_countbig)]

## <a name="maxexpression"></a>MAX (式)

コレクション内の最大値を返します。

**Arguments**

Collection (T)。ここで、T は次のいずれかの型になります。 

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`||

**戻り値**

`expression` の型。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_MAX](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_max)]

## <a name="minexpression"></a>MIN (式)

コレクション内の最小値を返します。

**Arguments**

Collection (T)。ここで、T は次のいずれかの型になります。 

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`||

**戻り値**

`expression` の型。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_MIN](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_min)]

## <a name="stdevexpression"></a>STDEV (式)

指定された式のすべての値の統計的標準偏差を返します。

**Arguments**

コレクション (`Double`)。

**戻り値**

`Double`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_STDEV](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_stdev)]

## <a name="stdevpexpression"></a>STDEVP (式)

指定された式のすべての値を母集団として統計的標準偏差を返します。

**Arguments**

コレクション (`Double`)。

**戻り値**

`Double`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_STDEVP](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_stdevp)]

## <a name="sumexpression"></a>SUM (式)

コレクション内のすべての値の合計を返します。

**Arguments**

Collection (T)。ここで、T は `Int32`、`Int64`、`Double`、`Decimal`のいずれかの型になります。

**戻り値**

`expression` の型。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_SUM](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_sum)]

## <a name="varexpression"></a>VAR(expression)

指定された式のすべての値の統計的分散を返します。

**Arguments**

コレクション (`Double`)。

**戻り値**

`Double`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_VAR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_var)]

## <a name="varpexpression"></a>VARP (式)

指定した式のすべての値について、母集団に対する統計的変位を返します。

**Arguments**

コレクション (`Double`)。

**戻り値**

`Double`。

**例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_VARP](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_varp)] 
  
## <a name="see-also"></a>参照

- [集計関数 (Transact-sql)](/sql/t-sql/functions/aggregate-functions-transact-sql)
- [Entity SQL 言語](./language-reference/entity-sql-language.md)
- [集計正規関数](./language-reference/aggregate-canonical-functions.md)
