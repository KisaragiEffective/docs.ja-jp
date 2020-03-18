---
title: トランザクション
ms.date: 12/13/2019
description: トランザクションの使用方法について説明します。
ms.openlocfilehash: 4b72a1573a560ffd1bfd0f54d46ab3b135280976
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450382"
---
# <a name="transactions"></a>トランザクション

トランザクションを使用すると、複数の SQL ステートメントをグループ化し、1つのアトミックユニットとしてデータベースにコミットされる1つの作業単位にすることができます。 トランザクション内のいずれかのステートメントが失敗した場合は、前のステートメントによって行われた変更をロールバックできます。 トランザクションが開始されたときのデータベースの初期状態は保持されます。 トランザクションを使用すると、一度にデータベースに多数の変更を加えたときに SQLite のパフォーマンスを向上させることもできます。

## <a name="concurrency"></a>コンカレンシー

SQLite では、一度に1つのトランザクションだけがデータベースで保留中の変更を保持できます。 このため、別のトランザクションの完了に時間がかかりすぎると、<xref:Microsoft.Data.Sqlite.SqliteConnection.BeginTransaction%2A> と <xref:Microsoft.Data.Sqlite.SqliteCommand> の `Execute` メソッドの呼び出しがタイムアウトする可能性があります。

ロック、再試行、タイムアウトの詳細については、「[データベースエラー](database-errors.md)」を参照してください。

## <a name="isolation-levels"></a>分離レベル

SQLite では、トランザクションは既定で**シリアル化**可能です。 この分離レベルは、トランザクション内で行われたすべての変更が完全に分離されることを保証します。 トランザクションの外部で実行される他のステートメントは、トランザクションの変更の影響を受けません。

SQLite は、共有キャッシュを使用しているときに、 **read 未確定**をサポートしています。 このレベルでは、ダーティリード、反復不能読み取り、およびファントムが許可されます。

- *ダーティリード*は、あるトランザクションで保留中の変更がトランザクションの外部のクエリによって返されたが、トランザクションの変更がロールバックされた場合に発生します。 結果には、実際にはデータベースにコミットされていないデータが含まれます。

- *反復不能読み取り*は、トランザクションが同じ行を2回クエリする場合に発生しますが、結果が異なるのは、2つのクエリの間で別のトランザクションによって変更されたためです。

- *ファントム*は、トランザクション中にクエリの where 句を満たすために変更または追加される行です。 許可されている場合、同じクエリで同じトランザクション内で2回実行すると、異なる行が返される可能性があります。

<xref:Microsoft.Data.Sqlite.SqliteConnection.BeginTransaction%2A> に渡される IsolationLevel は、最小レベルとして扱われます。 実際の分離レベルは、read 未確定または serializable に昇格されます。

次のコードは、ダーティリードをシミュレートします。 接続文字列には `Cache=Shared`を含める必要があることに注意してください。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DirtyReadSample/Program.cs?name=snippet_DirtyRead)]
