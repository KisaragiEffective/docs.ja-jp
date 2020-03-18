---
title: パラメーター
ms.date: 12/13/2019
description: SQL パラメーターの使用方法について説明します。
ms.openlocfilehash: 1d2f818ad392a919faedd785394de28a9c6f56c3
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450424"
---
# <a name="parameters"></a>パラメーター

パラメーターは、SQL インジェクション攻撃から保護するために使用されます。 ユーザー入力を SQL ステートメントと連結するのではなく、パラメーターを使用して入力がリテラル値として扱われ、実行されないようにします。 SQLite では、パラメーターは通常、リテラルが SQL ステートメントで許可されているすべての場所で使用できます。

パラメーターには、`:`、`@`、`$`のいずれかのプレフィックスを付けることができます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/HelloWorldSample/Program.cs?name=snippet_Parameter)]

.NET 値を SQLite 値にマップする方法の詳細については、「[データ型](types.md)」を参照してください。

## <a name="truncation"></a>[切り捨て]

テキストと BLOB の値を切り捨てるには、<xref:Microsoft.Data.Sqlite.SqliteParameter.Size> プロパティを使用します。

```csharp
// Truncate name to 30 characters
command.Parameters.AddWithValue("$name", name).Length = 30;
```

## <a name="alternative-types"></a>代替型

場合によっては、代替の SQLite 型を使用することもできます。 これを行うには、<xref:Microsoft.Data.Sqlite.SqliteParameter.SqliteType> プロパティを設定します。

次の代替の型マッピングを使用できます。 既定のマッピングについては、「[データ型](types.md)」を参照してください。

| Value          | SqliteType | コメント          |
| -------------- | ---------- | ---------------- |
| Char           | 整数型    | UTF-16           |
| DateTime       | Real       | ユリウス日の値 |
| DateTimeOffset | Real       | ユリウス日の値 |
| GUID           | Blob       |                  |
| TimeSpan       | Real       | 日数          |

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DateAndTimeSample/Program.cs?name=snippet_SqliteType)]

## <a name="output-parameters"></a>出力パラメーター

SQLite では、出力パラメーターはサポートされていません。 代わりに、クエリ結果の値を返します。

## <a name="see-also"></a>関連項目

* [データ型](types.md)
