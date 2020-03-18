---
title: 接続文字列
ms.date: 12/13/2019
description: 接続文字列のサポートされているキーワードと値。
ms.openlocfilehash: bb54d152bac62a86c2a49192cf678a745159164e
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450274"
---
# <a name="connection-strings"></a>接続文字列

接続文字列は、データベースへの接続方法を指定するために使用されます。 ADO.NET の接続文字列は、キーワードと値をセミコロンで区切ったリストとして、標準の[構文](../../../framework/data/adonet/connection-strings.md)に従います。

## <a name="keywords"></a>キーワード

次の接続文字列キーワードは、Microsoft. Data. Sqlite と共に使用できます。

### <a name="data-source"></a>[データ ソース]

データベース ファイルのパス。 *DataSource* (スペースなし) と*Filename*は、このキーワードのエイリアスです。

SQLite は、現在の作業ディレクトリを基準とした相対パスを扱います。 絶対パスを指定することもできます。

**空**の場合、SQLite は接続が閉じられたときに削除される一時的なディスク上のデータベースを作成します。

`:memory:`すると、メモリ内データベースが使用されます。 詳細については、「[メモリ内データベース](in-memory-databases.md)」を参照してください。

`|DataDirectory|` 置換文字列で始まるパスは、相対パスと同じように扱われます。 設定すると、DataDirectory アプリケーションドメインプロパティ値を基準とした相対パスが作成されます。

このキーワードは[URI ファイル名](https://www.sqlite.org/uri.html)もサポートしています。

### <a name="mode"></a>モード

接続モード。

| Value           | 説明                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------- |
| ReadWriteCreate | 読み取りと書き込みのためにデータベースを開き、存在しない場合は作成します。 これは既定です。 |
| ReadWrite       | 読み取りと書き込みのためにデータベースを開きます。                                                        |
| ReadOnly        | データベースを読み取り専用モードで開きます。                                                              |
| メモリ          | メモリ内データベースを開きます。                                                                       |

### <a name="cache"></a>キャッシュ

接続で使用されるキャッシュモード。

| Value   | 説明                                                                                    |
| ------- | ---------------------------------------------------------------------------------------------- |
| [既定値] | は、基になる SQLite ライブラリの既定のモードを使用します。 これは既定です。                   |
| Private | 各接続は、プライベートキャッシュを使用します。                                                          |
| [共有]  | 接続はキャッシュを共有します。 このモードでは、トランザクションおよびテーブルロックの動作を変更できます。 |

### <a name="password"></a>Password

暗号化キー。 指定した場合、接続を開いた直後に `PRAGMA key` が送信されます。

> [!WARNING]
> ネイティブ SQLite ライブラリで暗号化がサポートされていない場合、パスワードは無効です。

### <a name="foreign-keys"></a>外部キー

Foreign key 制約を有効にするかどうかを示す値です。

| Value   | 説明
| ------- | --- |
| True    | 接続を開いた直後に `PRAGMA foreign_keys = 1` を送信します。
| [False]   | 接続を開いた直後に `PRAGMA foreign_keys = 0` を送信します。
| (空) | `PRAGMA foreign_keys`を送信しません。 これは既定です。 |

E_sqlite3 のように、ネイティブ SQLite ライブラリをコンパイルするために SQLITE_DEFAULT_FOREIGN_KEYS を使用した場合、外部キーを有効にする必要はありません。

### <a name="recursive-triggers"></a>再帰トリガー

再帰トリガーを有効にするかどうかを示す値です。

| Value | 説明                                                                 |
| ----- | --------------------------------------------------------------------------- |
| True  | 接続を開いた直後に `PRAGMA recursive_triggers` を送信します。 |
| [False] | `PRAGMA recursive_triggers`を送信しません。 これは既定です。              |

## <a name="connection-string-builder"></a>接続文字列ビルダー

<xref:Microsoft.Data.Sqlite.SqliteConnectionStringBuilder> は、厳密に型指定された接続文字列の作成方法として使用できます。 また、接続文字列のインジェクション攻撃を防ぐためにも使用できます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/EncryptionSample/Program.cs?name=snippet_ConnectionStringBuilder)]

## <a name="examples"></a>使用例

### <a name="basic"></a>Basic

同時実行性を向上させるための共有キャッシュを備えた基本的な接続文字列。

```ConnectionString
Data Source=Application.db;Cache=Shared
```

### <a name="encrypted"></a>暗号化

暗号化されたデータベース。

```ConnectionString
Data Source=Encrypted.db;Password=MyEncryptionKey
```

### <a name="read-only"></a>読み取り専用

アプリによって変更できない読み取り専用のデータベース。

```ConnectionString
Data Source=Reference.db;Mode=ReadOnly
```

### <a name="in-memory"></a>メモリ内

プライベートなメモリ内データベース。

```ConnectionString
Data Source=:memory:
```

### <a name="sharable-in-memory"></a>メモリ内で共有可能

*共有可能な名前で*識別される、共有可能なメモリ内のデータベース。

```ConnectionString
Data Source=Sharable;Mode=Memory;Cache=Shared
```

## <a name="see-also"></a>関連項目

* [ADO.NET の接続文字列](../../../framework/data/adonet/connection-strings.md)
* [インメモリデータベース](in-memory-databases.md)
* [トランザクション](transactions.md)
