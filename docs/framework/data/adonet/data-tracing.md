---
title: データトレース
ms.date: 03/30/2017
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.openlocfilehash: 2b2a33739619ba09e56d4cc9ce99c51423678918
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980224"
---
# <a name="data-tracing-in-adonet"></a>ADO.NET のデータ追跡

ADO.NET、Oracle、OLE DB、および <xref:System.Data.DataSet>ODBC 用 SQL Server の .NET データプロバイダー、および SQL Server ネットワークプロトコルによってサポートされている組み込みのデータトレース機能が組み込まれています。

データ アクセス API 呼び出しのトレースは、次の問題を診断する際に役立ちます。

- クライアント プログラムとデータベース間のスキーマの不一致

- データベースの使用不可またはネットワーク ライブラリの問題

- 誤った SQL がアプリケーションによりハードコーディングまたは生成された

- プログラミング ロジックが不適切

- 複数の ADO.NET コンポーネント間または ADO.NET と独自コンポーネント間の対話に起因する問題

トレースを拡張することにより異なるトレース技術をサポートできます。このため、開発者はアプリケーション スタックのあらゆるレベルで問題をトレースできます。 トレースは ADO.NET のみで使用できる機能ではありませんが、Microsoft プロバイダーでは、汎用トレース機能および instrumentation API を活用しています。

ADO.NET でのマネージトレースの設定と構成の詳細については、「[データアクセスのトレース](https://docs.microsoft.com/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))」を参照してください。

## <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>拡張イベント ログの診断情報へのアクセス

SQL Server の .NET Framework Data Provider では、データアクセスのトレース ([データアクセスのトレース](https://docs.microsoft.com/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))) が更新され、クライアントイベントと接続エラーなどの診断情報を、拡張イベントログのサーバーの接続リングバッファーとアプリケーションのパフォーマンス情報から簡単に関連付けられるようになりました。 拡張イベントログの読み取りの詳細については、「[イベントセッションデータの表示](https://docs.microsoft.com/previous-versions/sql/sql-server-2012/hh710068(v=sql.110))」を参照してください。

接続操作では、ADO.NET はクライアント接続 ID を送信します。 接続に失敗した場合は、接続リングバッファー (接続[リングバッファーを使用した SQL Server 2008 の接続のトラブルシューティング](https://docs.microsoft.com/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) にアクセスし、`ClientConnectionID` フィールドを見つけて、接続エラーに関する診断情報を取得することができます。 クライアント接続 ID は、エラーが発生した場合にのみリング バッファーに記録されます。 (ログイン前パケットを送信する前に接続が失敗した場合、クライアント接続 ID は生成されません)。クライアント接続 ID は16バイトの GUID です。 拡張イベント セッション内のイベントに `client_connection_id` アクションが追加された場合にも、拡張イベントのターゲット出力のクライアント接続 ID を見つけることができます。 それ以上にクライアントのドライバーの診断について支援が必要な場合は、データ アクセスのトレースを有効にし、接続コマンドを再実行して、データ アクセスのトレースの `ClientConnectionID` フィールドを確認することができます。

`SqlConnection.ClientConnectionID` プロパティを使用して、クライアント接続 ID をプログラムによって取得できます。

`ClientConnectionID` は、正常に接続を確立する <xref:System.Data.SqlClient.SqlConnection> オブジェクトで使用できます。 接続試行が失敗すると、`ClientConnectionID` は `SqlException.ToString` を通じて利用可能になることがあります。

ADO.NET は、スレッド固有のアクティビティ ID も送信します。 アクティビティ ID は、TRACK_CAUSALITY オプションを有効にしてセッションが開始された場合に、拡張イベントセッションでキャプチャされます。 アクティブな接続のパフォーマンスの問題については、クライアントのデータ アクセスのトレース (`ActivityID` フィールド) からアクティビティ ID を取得した後、その拡張イベントの出力のアクティビティ ID を検索できます。 拡張イベント内のアクティビティ ID とは、16 バイトの GUID (クライアント接続 ID の GUID とは異なる) の後に 4 バイトのシーケンス番号が付いたものです。 シーケンス番号は、スレッド内で要求の順序を表し、スレッドのバッチと RPC ステートメントの相対的順序を示します。 `ActivityID` は、現在、データ アクセスのトレースが有効な場合、データ アクセスのトレースの構成のワードの 18 番目のビットが有効にされると、オプションとして SQL のバッチ ステートメントと RPC の要求に送信されるようになっています。

次に示すのは、Transact-sql を使用して拡張イベントセッションを開始するサンプルです。このセッションでは、リングバッファーに格納され、RPC およびバッチ操作でクライアントから送信されたアクティビティ ID が記録されます。

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>関連項目

- [.NET Framework のネットワークのトレース](../../network-programming/network-tracing.md)
- [アプリケーションのトレースとインストルメント](../../debug-trace-profile/tracing-and-instrumenting-applications.md)
- [ADO.NET の概要](ado-net-overview.md)
