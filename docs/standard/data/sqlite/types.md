---
title: データの種類
ms.date: 12/13/2019
description: サポートされているデータ型と、それらに関連する制限事項について説明します。
ms.openlocfilehash: ae70cb91a5a6d9cfed45a5a47dda25a70362871e
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450418"
---
# <a name="data-types"></a>データの種類

SQLite には、整数、実数、テキスト、BLOB の4つのプリミティブデータ型しかありません。 `object` としてデータベース値を返す Api は、次の4つの型のいずれか1つだけを返します。 その他の .NET 型は、Microsoft でサポートされていますが、最終的には、これらの型と4つのプリミティブ型の1つの間で値が強制されます。

| .NET           | SQLite  | コメント                                                       |
| -------------- | ------- | ------------------------------------------------------------- |
| Boolean        | INTEGER | `0` または `1`                                                    |
| Byte           | INTEGER |                                                               |
| Byte[]         | BLOB    |                                                               |
| Char           | TEXT    | UTF-8                                                         |
| DateTime       | TEXT    | yyyy-MM-dd HH: MM: ss.FFFFFFF                                   |
| DateTimeOffset | TEXT    | yyyy-MM-dd HH: MM: ss.Ss'.'fffffffzzz                                |
| Decimal        | TEXT    | `0.0###########################` 形式。 実際には、損失はありません。 |
| 倍精度浮動小数点型         | real    |                                                               |
| GUID           | TEXT    | 00000000-0000-0000-0000-000000000000                          |
| Int16          | INTEGER |                                                               |
| Int32          | INTEGER |                                                               |
| Int64          | INTEGER |                                                               |
| SByte          | INTEGER |                                                               |
| Single         | real    |                                                               |
| 文字列型         | TEXT    | UTF-8                                                         |
| TimeSpan       | TEXT    | d. hh: mm: fffffff                                            |
| UInt16         | INTEGER |                                                               |
| UInt64         | INTEGER | 大きな値のオーバーフロー                                         |

## <a name="alternative-types"></a>代替型

一部の .NET 型は、代替の SQLite 型から読み取ることができます。 これらの代替の型を使用するようにパラメーターを構成することもできます。 詳しくは、「[パラメーター](parameters.md#alternative-types)」をご覧ください。

| .NET           | SQLite  | コメント          |
| -------------- | ------- | ---------------- |
| Char           | INTEGER | UTF-16           |
| DateTime       | real    | ユリウス日の値 |
| DateTimeOffset | real    | ユリウス日の値 |
| GUID           | BLOB    |                  |
| TimeSpan       | real    | 日数          |

たとえば、次のクエリでは、結果セット内の実際の列から TimeSpan 値が読み取られます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DateAndTimeSample/Program.cs?name=snippet_AlternativeType)]

## <a name="column-types"></a>列の型

SQLite は、値の型が格納されている列ではなく、値自体に関連付けられている動的な型システムを使用します。 任意の列の型名を自由に使用できます。 これらの名前には、追加のセマンティクスは適用されません。

列の型名は、[型のアフィニティ](https://www.sqlite.org/datatype3.html#type_affinity)に影響します。 1つの一般的な注意事項は、文字列の列型を使用すると、値を整数または実数に変換しようとし、予期しない結果になる可能性があるということです。 4つのプリミティブな SQLite 型名 (INTEGER、REAL、TEXT、BLOB) のみを使用することをお勧めします。

SQLite では、長さ、有効桁数、小数点以下桁数などの型のファセットを指定できますが、データベースエンジンによって適用されるわけではありません。 アプリは、これらの適用を担当します。

## <a name="see-also"></a>関連項目

- [SQLite のデータ型](https://www.sqlite.org/datatype3.html)
