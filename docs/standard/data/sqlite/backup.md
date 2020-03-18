---
title: オンライン バックアップ。
ms.date: 12/13/2019
description: SQLite のオンラインバックアップ機能を使用する方法について説明します。
ms.openlocfilehash: d857dcb69f2b2d10b034a0abf222b30c2e20bb41
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901283"
---
# <a name="online-backup"></a>オンライン バックアップ。

SQLite は、アプリの実行中にデータベースファイルをバックアップできます。 この機能は、`SqliteConnection`の <xref:Microsoft.Data.Sqlite.SqliteConnection.BackupDatabase%2A> メソッドとして、Microsoft. Data. Sqlite で使用できます。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/BackupSample/Program.cs?name=snippet_Backup)]

現時点では、`BackupDatabase` はできるだけ早くデータベースをバックアップし、他の接続がデータベースへの書き込みをブロックします。 問題[#13834](https://github.com/dotnet/efcore/issues/13834)は、データベースをバックグラウンドでバックアップするための代替 API を提供し、他の接続がバックアップとデータベースへの書き込みを中断できるようにします。 関心がある場合は、問題に関するフィードバックをお寄せください。
