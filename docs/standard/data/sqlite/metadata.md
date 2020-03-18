---
title: メタデータ
ms.date: 12/13/2019
description: データベースに関するメタデータを取得する方法について説明します。
ms.openlocfilehash: b2f2704a748627d9943943fa2fa7b1b7e9f3007f
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450430"
---
# <a name="metadata"></a>メタデータ

ADO.NET のメタデータを取得するための Api は2つあります。 1つは、クエリ結果に関するメタデータを取得します。 もう1つは、データベーススキーマに関するメタデータを取得します。

## <a name="query-result-metadata"></a>クエリ結果のメタデータ

`SqliteDataReader`の <xref:Microsoft.Data.Sqlite.SqliteDataReader.GetSchemaTable%2A> メソッドを使用して、クエリの結果に関するメタデータを取得できます。 返される <xref:System.Data.DataTable> には、次の列が含まれます。

| [列 ]             | の型    | 説明                                                               |
| ------------------ | ------- | ------------------------------------------------------------------------- |
| `AllowDBNull`      | Boolean | Origin 列が NULL である場合は True を指定します。                                    |
| `BaseCatalogName`  | 文字列型  | 元の列のデータベースの名前。 式の場合は常に NULL です。    |
| `BaseColumnName`   | 文字列型  | 元の列のエイリアス解除された名前。 式の場合は常に NULL です。    |
| `BaseSchemaName`   | 文字列型  | 常に NULL です。 SQLite はスキーマをサポートしていません。                              |
| `BaseServerName`   | 文字列型  | 接続文字列で指定されたデータベースファイルへのパスです。         |
| `BaseTableName`    | 文字列型  | 元の列のテーブルの名前。 式の場合は常に NULL です。       |
| `ColumnName`       | 文字列型  | 結果セット内の列の名前または別名。                        |
| `ColumnOrdinal`    | Int32   | 結果セット内の列の序数。                              |
| `ColumnSize`       | Int32   | 常に-1 です。 これは、`Microsoft.Data.Sqlite`の将来のバージョンで変更される可能性があります。   |
| `DataType`         | の型    | 列の既定の .NET データ型。                                 |
| `DataTypeName`     | 文字列型  | 列の SQLite データ型。                                       |
| `IsAliased`        | Boolean | 列名が結果セットに含まれる場合は True を指定します。                     |
| `IsAutoIncrement`  | Boolean | Origin 列が AUTOINCREMENT キーワードを使用して作成された場合は True。     |
| `IsExpression`     | Boolean | 列がクエリ内の式から生成される場合は True を指定します。            |
| `IsKey`            | Boolean | Origin 列が主キーの一部である場合は True。                     |
| `IsUnique`         | Boolean | Origin 列が一意である場合は True を示します。                                      |
| `NumericPrecision` | Int16   | 常に NULL です。 これは、`Microsoft.Data.Sqlite`の将来のバージョンで変更される可能性があります。 |
| `NumericScale`     | Int16   | 常に NULL です。 これは、`Microsoft.Data.Sqlite`の将来のバージョンで変更される可能性があります。 |

次の例は、`GetSchemaTable` を使用して、結果に関するメタデータを示すデバッグ文字列を作成する方法を示しています。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ResultMetadataSample/Program.cs?name=snippet_ResultMetadata)]

たとえば、次のクエリでは、次のデバッグ文字列が生成されます。

```sql
SELECT id AS post_id,
       title,
       body,
       random() AS random
FROM post
```

```output
post.id AS post_id INTEGER PRIMARY KEY AUTOINCREMENT
post.title TEXT NOT NULL UNIQUE
post.body TEXT
(expression) AS random BLOB
```

## <a name="schema-metadata"></a>スキーマメタデータ

DbConnection には、GetSchema メソッドが実装されていません。 代わりに、 [sqlite_master](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema)テーブルと、 [table_info](https://www.sqlite.org/pragma.html#pragma_table_info)や[foreign_key_list](https://www.sqlite.org/pragma.html#pragma_foreign_key_list)などのプラグマステートメントを使用して、スキーマ情報を直接照会することができます。

たとえば、次のクエリでは、データベース内のすべての列に関するメタデータが取得されます。

```sql
SELECT t.name AS tbl_name, c.name, c.type, c.notnull, c.dflt_value, c.pk
FROM sqlite_master AS t,
     pragma_table_info(t.name) AS c
WHERE t.type = 'table';
```

## <a name="see-also"></a>関連項目

* [SQL Database スキーマの格納](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema)
* [PRAGMA ステートメント](https://www.sqlite.org/pragma.html)
* [データ型](types.md)
