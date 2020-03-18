---
title: ADO.NET の制限事項
ms.date: 12/13/2019
description: 発生する可能性がある ADO.NET の制限事項について説明します。
ms.openlocfilehash: 8664b73071fc859ed30080f549b05e7d6ed020f4
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901261"
---
# <a name="adonet-limitations"></a>ADO.NET の制限事項

ADO.NET には、多くの抽象化が実装されていますが、いくつかの制限があります。

## <a name="database-schema-information"></a>データベーススキーマ情報

クエリ結果に関するメタデータは、<xref:Microsoft.Data.Sqlite.SqliteDataReader.GetSchemaTable%2A> メソッドを使用して取得できます。

`DbConnection.GetSchema()` は実装されていません。 この API は明確に定義されていないため、 [sqlite_master](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema)テーブルや[table_info](https://www.sqlite.org/pragma.html#pragma_table_info)プラグマなどの標準の SQLite api を使用して、データベースのメタデータを直接取得することをお勧めします。

詳細については、「[メタデータ](metadata.md)」を参照してください。

## <a name="systemtransactions"></a>System.Transactions

Microsoft Data Sqlite では、まだ system.string はサポートされていません。 代わりに ADO.NET トランザクションを使用してください。 詳細については、「[トランザクション](transactions.md)」を参照してください。

問題[#13825](https://github.com/dotnet/efcore/issues/13825)でのシステムトランザクションのサポート不足に関するフィードバックを提供します。

## <a name="data-adapters"></a>データアダプター

`DbDataAdapter` は、まだ Microsoft. Sqlite によって実装されていません。 つまり、ADO.NET `DataTable` `DataSet` のみを使用してデータを読み込み、更新することはできません。

問題[#13838](https://github.com/dotnet/efcore/issues/13838)を使用して、`DbDataAdapter`の実装に関するフィードバックを提供します。

## <a name="output-parameters"></a>出力パラメーター

SQLite では、出力パラメーターはサポートされていません。

## <a name="positional-parameters"></a>位置指定パラメーター

Microsoft では、名前付き[パラメーター](parameters.md)のみをサポートしています。 位置指定パラメーターはサポートされていません。

## <a name="stored-procedures"></a>ストアド プロシージャ

SQLite はストアドプロシージャをサポートしていません。

## <a name="isolation-levels"></a>分離レベル

`Chaos` および `Snapshot` の分離レベルは、SQLite トランザクションではサポートされていません。

## <a name="see-also"></a>関連項目

* [非同期の制限事項](async.md)
* [データ型](types.md)
* [トランザクション](transactions.md)
