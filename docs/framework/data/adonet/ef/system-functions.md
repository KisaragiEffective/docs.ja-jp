---
title: システム関数
ms.date: 03/30/2017
ms.assetid: b7c71b58-09e6-44ce-a3e5-a0fdb892fb86
ms.openlocfilehash: 9b5455d63dca40834515b14bae2f35d3b54d2aee
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452436"
---
# <a name="system-functions"></a>システム関数
.NET Framework Data Provider for SQL Server (SqlClient) には、次のシステム関数が用意されています。  
  
|関数|説明|  
|--------------|-----------------|  
|`CHECKSUM (` `value`、[`value`、[`value`]]`)`|チェックサム値を返します。 `CHECKSUM` は、ハッシュ インデックスの作成に使用します。<br /><br /> **引数**<br /><br /> `value`: `Boolean`、`Byte`、`Int16`、`Int32`、`Int64`、`Single`、`Decimal`、`Double`、`DateTime`、`String`、`Binary`、`Guid`。 1 つ、2 つ、または 3 つの値を指定できます。<br /><br /> **戻り値**<br /><br /> 指定された式の絶対値。<br /><br /> **例**<br /><br /> `SqlServer.CHECKSUM(10,100,1000.0)`|  
|`CURRENT_TIMESTAMP ()`|有効桁数が 7 (SQL Server 2008) または 3 (SQL Server 2005) の `DateTime` 値に使用する現在の日付と時刻を SQL Server の内部形式で生成します。<br /><br /> **戻り値**<br /><br /> 現在のシステム日時を `DateTime` として表現した値。<br /><br /> **例**<br /><br /> `SqlServer.CURRENT_TIMESTAMP()`|  
|`CURRENT_ USER` `()`|現在のユーザーの名前を返します。<br /><br /> **戻り値**<br /><br /> ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.CURRENT_USER()`|  
|`DATALENGTH` `(` `expression` `)`|式を表すために必要なバイト数を返します。<br /><br /> **引数**<br /><br /> `expression`: `Boolean`、`Byte`、`Int16`、`Int32`、`Int64`、`Single`、`Decimal`、`Double`、`DateTime`、`Time`、`DateTimeOffset`、`String`です。`Binary``Guid`<br /><br /> **戻り値**<br /><br /> プロパティのサイズ (`Int32`)。<br /><br /> **例**<br /><br /> `SELECT VALUE SqlServer.DATALENGTH(P.Name)FROM`<br /><br /> `AdventureWorksEntities.Product AS P`|  
|`HOST_NAME()`|ワークステーション名を返します。<br /><br /> **戻り値**<br /><br /> Unicode の `String`。<br /><br /> **例**<br /><br /> `SqlServer.HOST_NAME()`|  
|`ISDATE(` `expression` `)`|入力式が有効な日付かどうかを調べます。<br /><br /> **引数**<br /><br /> `expression`: `Boolean`、`Byte`、`Int16`、`Int32`、`Int64`、`Single`、`Decimal`、`Double`、`DateTime`、`Time`、`DateTimeOffset`、`String`です。`Binary``Guid`<br /><br /> **戻り値**<br /><br /> `Int32`。 入力式が有効な日付である場合は 1 です。 それ以外の場合は 0 です。<br /><br /> **例**<br /><br /> `SqlServer.ISDATE('1/1/2006')`|  
|`ISNUMERIC(` `expression` `)`|式が数値型として有効かどうかを調べます。<br /><br /> **引数**<br /><br /> `expression`: `Boolean`、`Byte`、`Int16`、`Int32`、`Int64`、`Single`、`Decimal`、`Double`、`DateTime`、`Time`、`DateTimeOffset`、`String`です。`Binary``Guid`<br /><br /> **戻り値**<br /><br /> `Int32`。 入力式が有効な日付である場合は 1 です。 それ以外の場合は 0 です。<br /><br /> **例**<br /><br /> `SqlServer.ISNUMERIC('21')`|  
|`NEWID()`|Guid 型の一意な値を作成します。<br /><br /> **戻り値**<br /><br /> `Guid` です。<br /><br /> **例**<br /><br /> `SqlServer.NEWID()`|  
|`USER_NAME(` `id` `)`|指定した ID 番号から、データベース ユーザー名を返します。<br /><br /> **引数**<br /><br /> `expression`: データベース ユーザーに関連付けられている `Int32` 型の識別番号を指定します。<br /><br /> **戻り値**<br /><br /> Unicode の `String`。<br /><br /> **例**<br /><br /> `SqlServer.USER_NAME(0)`|  
  
 SqlClient でサポートされる `String` 関数の詳細については、「[文字列関数 (transact-sql)](/sql/t-sql/functions/string-functions-transact-sql)」を参照してください。
  
## <a name="see-also"></a>参照

- [Entity SQL 言語](./language-reference/entity-sql-language.md)
- [Entity Framework 用 SqlClient 関数](sqlclient-for-ef-functions.md)
