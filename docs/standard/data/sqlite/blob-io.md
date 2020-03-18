---
title: Blob i/o
ms.date: 12/13/2019
description: SQLite の BLOB i/o 機能の使用方法について説明します。
ms.openlocfilehash: 0c133deacdc19684eca3a6724fb398dc01fda558
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450304"
---
# <a name="blob-io"></a>Blob i/o

データベースとの間でデータをストリーミングすることによって、大きなオブジェクトの読み取りおよび書き込み中にメモリ使用量を削減できます。 これは、データを解析または変換するときに特に便利です。

最初に通常どおりに行を挿入します。 `zeroblob()` SQL 関数を使用して、ラージオブジェクトを保持するデータベースの領域を割り当てます。 `last_insert_rowid()` 関数は、rowid を取得する便利な方法を提供します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Insert)]

行を挿入した後、ストリームを開いて <xref:Microsoft.Data.Sqlite.SqliteBlob>を使用してラージオブジェクトを書き込みます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Write)]

大きなオブジェクトをデータベースからストリーミングするには、ラージオブジェクトの列に加えて、rowid またはそのエイリアスのいずれかを選択する必要があります。 Rowid を選択しない場合、オブジェクト全体がメモリに読み込まれます。 `GetStream()` によって返されるオブジェクトは、正常に完了すると `SqliteBlob` になります。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Read)]
