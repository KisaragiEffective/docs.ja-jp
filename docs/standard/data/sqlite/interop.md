---
title: 相互運用性
ms.date: 12/13/2019
description: 他の SQLite ライブラリと相互運用する方法について説明します。
ms.openlocfilehash: 486e2a8e00999b8ebe9c85a5fcc1539c514676d3
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450460"
---
# <a name="interoperability"></a>相互運用性

SQLitePCLRaw は、ネイティブ SQLite ライブラリと対話するために使用されます。 SQLitePCLRaw は、ネイティブの SQLite API に対するシン .NET API を提供します。 SqliteConnection と SqliteDataReader は、これらの Api を直接呼び出すことができる、基になる SQLitePCLRaw オブジェクトへのアクセスを提供します。

次の例では、`sqlite3_trace` を呼び出して、実行された SQL ステートメントをコンソールに書き込む方法を示します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/InteropSample/Program.cs?name=snippet_Trace)]

次の例では、`sqlite3_stmt_status` を呼び出して、SQL ステートメントがコンパイルされた SQLite 仮想マシンのステップ数を確認します。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/InteropSample/Program.cs?name=snippet_StatementStatus)]

SQLitePCLRaw オブジェクトは、ネイティブオブジェクトへのポインターを公開しています。これにより、追加のネイティブ SQLite Api を呼び出すことができます。
