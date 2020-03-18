---
title: データベース エラー
ms.date: 12/13/2019
description: データベースのエラーと廃止がライブラリによってどのように処理されるかについて説明します。
ms.openlocfilehash: 9a613b6f94a89dc40e627c14f46836be77080035
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450490"
---
# <a name="database-errors"></a>データベース エラー

SQLite エラーが発生すると <xref:Microsoft.Data.Sqlite.SqliteException> がスローされます。 メッセージは SQLite によって提供されます。 `SqliteErrorCode` プロパティと `SqliteExtendedErrorCode` プロパティには、エラーの SQLite[結果コード](https://www.sqlite.org/rescode.html)が含まれています。

エラーが発生する可能性があるのは、Microsoft データ Sqlite がネイティブ SQLite ライブラリとやり取りしているときです。 次の一覧は、エラーが発生する可能性がある一般的なシナリオを示しています。

* 接続を開いています。
* トランザクションを開始しています。
* コマンドの実行。
* <xref:Microsoft.Data.Sqlite.SqliteDataReader.NextResult%2A> の呼び出し

アプリがこれらのエラーをどのように処理するかを慎重に検討してください。

## <a name="locking-retries-and-timeouts"></a>ロック、再試行、タイムアウト

SQLite は、テーブルおよびデータベースファイルのロックに関して積極的に行われます。 アプリケーションがデータベースへの同時アクセスを有効にすると、ビジー状態とロックエラーが発生する可能性があります。 [共有キャッシュ](connection-strings.md#cache)と[先行書き込みログ](async.md)を使用すると、多くのエラーを軽減できます。

Microsoft では、ビジー状態またはロックされたエラーが発生するたびに、成功またはコマンドのタイムアウトに達するまで自動的に再試行します。

<xref:Microsoft.Data.Sqlite.SqliteCommand.CommandTimeout%2A>を設定して、コマンドのタイムアウトを増やすことができます。 既定のタイムアウトは30秒です。 `0` の値は、タイムアウトがないことを意味します。

```csharp
// Retry for 60 seconds while locked
command.CommandTimeout = 60;
```

場合によっては、暗黙的なコマンドオブジェクトの作成が必要になることがあります。 たとえば、BeginTransaction 中です。 これらのコマンドのタイムアウトを設定するには、<xref:Microsoft.Data.Sqlite.SqliteConnection.DefaultTimeout%2A>を使用します。

```csharp
// Set the default timeout of all commands on this connection
connection.DefaultTimeout = 60;
```

## <a name="see-also"></a>関連項目

* [SQLite エラーコード](https://www.sqlite.org/rescode.html)
* [SQLite でのファイルロック](https://www.sqlite.org/lockingv3.html)
* [共有キャッシュモード](https://www.sqlite.org/sharedcache.html)
* [先行書き込みログ](https://www.sqlite.org/wal.html)
