---
title: 照合順序
ms.date: 12/13/2019
description: カスタムの照合シーケンスを作成する方法について説明します。
ms.openlocfilehash: 9cc574a75c8f5347dd9bb44e36af72e50afa57b4
ms.sourcegitcommit: cbdc0f4fd39172b5191a35200c33d5030774463c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777382"
---
# <a name="collation"></a>照合順序

照合シーケンスは、テキスト値を比較して順序と等価性を決定するときに SQLite によって使用されます。 SQL クエリで列を作成するとき、または操作ごとに使用する照合順序を指定できます。 SQLite には、既定で3つの照合シーケンスが含まれています。

| 照合順序 | 説明                               |
| --------- | ----------------------------------------- |
| RTRIM     | 末尾の空白を無視します               |
| NOCASE    | ASCII 文字 A-z の大文字と小文字を区別しない |
| BINARY    | 大文字と小文字を区別します。 バイトを直接比較します。   |

## <a name="custom-collation"></a>カスタム照合順序

また、独自の照合シーケンスを定義したり、<xref:Microsoft.Data.Sqlite.SqliteConnection.CreateCollation%2A>を使用して組み込みのシーケンスをオーバーライドしたりすることもできます。 次の例では、NOCASE 照合順序をオーバーライドして Unicode 文字をサポートしています。 [完全なサンプルコード](https://github.com/dotnet/samples/blob/master/snippets/standard/data/sqlite/CollationSample/Program.cs)は、GitHub で入手できます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/CollationSample/Program.cs?name=snippet_Collation)]

## <a name="like-operator"></a>Like 演算子

SQLite の LIKE 演算子は照合順序を受け入れません。 既定の実装は、NOCASE 照合順序と同じセマンティクスを持ちます。 ASCII 文字 A から Z までは大文字と小文字が区別されません。

次のプラグマステートメントを使用すると、大文字と小文字の区別を簡単に行うことができます。

```sql
PRAGMA case_sensitive_like = 0
```

LIKE 演算子の実装のオーバーライドの詳細については、「[ユーザー定義関数](user-defined-functions.md)」を参照してください。

## <a name="see-also"></a>関連項目

* [ユーザー定義関数](user-defined-functions.md)
* [SQL 構文](https://www.sqlite.org/lang.html)
