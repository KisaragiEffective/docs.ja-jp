---
title: 非同期の制限事項
ms.date: 12/13/2019
description: 非同期 Api の制限と代わりに使用できるいくつかの代替手段について説明します。
ms.openlocfilehash: 350237dc5c03023f60e9680e8b9c94aabb62606f
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450334"
---
# <a name="async-limitations"></a>非同期の制限事項

SQLite は、非同期 i/o をサポートしていません。 非同期 ADO.NET メソッドは、同期的に実行されます。 これらの呼び出しは避けてください。

代わりに、[先行書き込みログ](https://www.sqlite.org/wal.html)を使用して、パフォーマンスと同時実行性を向上させます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/AsyncSample/Program.cs?name=snippet_WAL)]

> [!TIP]
> [Entity Framework Core](/ef/core/)を使用して作成されたデータベースでは、先行書き込みログが既定で有効になっています。
