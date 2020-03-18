---
title: SQL Server のクエリ通知
ms.date: 03/30/2017
ms.assetid: 0f0ba1a1-3180-4af8-87f7-c795dc8f8f55
ms.openlocfilehash: 11d9a1a800bea4224853a57b128ca89c9f2cf781
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452371"
---
# <a name="query-notifications-in-sql-server"></a>SQL Server のクエリ通知
クエリ通知は Service Broker インフラストラクチャに基づいて構築されており、データが変更されたときにクエリ通知を使用してアプリケーションに通知できます。 Web アプリケーションのように、データベースからの情報のキャッシュを用意し、データベースのデータが変更されたときに通知する必要があるアプリケーションでは、この機能が特に有用です。  
  
 ADO.NET を使用してクエリ通知機能を実装する方法は 3 つあります。  
  
1. 基本レベルの実装は、`SqlNotificationRequest` クラスによって行われます。これによってサーバー側の機能が公開され、通知要求を伴うコマンドを実行できます。  
  
2. 高レベルの実装は、`SqlDependency` クラスによって行われます。このクラスでは、ソース アプリケーションと SQL Server 間の通知機能が高度に抽象化され、その依存関係を使用してサーバー内の変更を検出することができます。 マネージド クライアント アプリケーションが .NET Framework Data Provider for SQL Server を使用して SQL Server の通知機能を活用するには、ほとんどの場合、これが最も簡単かつ効果的な方法です。  
  
3. さらに、ASP.NET 2.0 以降を使用して構築された Web アプリケーションは、`SqlCacheDependency` ヘルパー クラスを使用できます。  
  
 クエリ通知は、基になるデータの変更に応じて表示またはキャッシュを更新する必要があるアプリケーションで使用されます。 Microsoft SQL Server では、最初に取得したものと異なる結果セットが同じコマンドで得られた場合には、.NET Framework アプリケーションから SQL Server にコマンドを送信して通知を要求することができます。 サーバーで生成された通知は、後で処理するためにキュー経由で送信されます。  
  
 通知は SELECT ステートメントおよび EXECUTE ステートメントに対して設定できます。 EXECUTE ステートメントを使用した場合、SQL Server では、EXECUTE ステートメント自体ではなく、EXECUTE ステートメントで実行されたコマンドに対する通知が登録されます。 コマンドは、SELECT ステートメントの要件と制限を満たしている必要があります。 通知を登録するコマンドに複数のステートメントが含まれている場合、Database Engine によりバッチ内のステートメントごとに通知が作成されます。  
  
 データの変更時に信頼できるサブ秒の通知が必要なアプリケーションを開発している場合は、「[通知の計画](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms187528(v=sql.105))」記事の「**効率的なクエリ通知方法の計画**」および「**クエリ通知の代替手段**」セクションを参照してください。 クエリ通知と SQL Server Service Broker の詳細については、SQL Server のドキュメントの記事への次のリンクを参照してください。  
  
 **SQL Server のドキュメント**  
  
- [クエリ通知の使用](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms175110(v=sql.105))  
  
- [通知のためのクエリの作成](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms181122(v=sql.105))  
  
- [開発 (Service Broker)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522889(v=sql.105))  
  
- [Service Broker 開発者向けの情報](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms166100(v=sql.105))  
  
- [開発者ガイド (Service Broker)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522908(v=sql.105))  
  
## <a name="in-this-section"></a>このセクションの内容  
 [クエリ通知の有効化](enabling-query-notifications.md)  
 クエリ通知を有効にするための要件を含め、クエリ通知の使用方法について説明します。  
  
 [ASP.NET アプリケーションでの SqlDependency](sqldependency-in-an-aspnet-app.md)  
 ASP.NET アプリケーションからクエリ通知を使用する方法について説明します。  
  
 [SqlDependency を使用した変更の検出](detecting-changes-with-sqldependency.md)  
 最初に取得された結果と異なるクエリ結果を検出する方法について説明します。  
  
 [SqlCommand の実行と SqlNotificationRequest](sqlcommand-execution-with-a-sqlnotificationrequest.md)  
 クエリ通知を使用する <xref:System.Data.SqlClient.SqlCommand> オブジェクトの構成例を示します。  
  
## <a name="reference"></a>参照  
 <xref:System.Data.Sql.SqlNotificationRequest>  
 <xref:System.Data.Sql.SqlNotificationRequest> クラスおよびそのすべてのメンバーについて説明します。  
  
 <xref:System.Data.SqlClient.SqlDependency>  
 <xref:System.Data.SqlClient.SqlDependency> クラスおよびそのすべてのメンバーについて説明します。  
  
 <xref:System.Web.Caching.SqlCacheDependency>  
 <xref:System.Web.Caching.SqlCacheDependency> クラスおよびそのすべてのメンバーについて説明します。  
  
## <a name="see-also"></a>参照

- [SQL Server と ADO.NET](index.md)
- [ADO.NET の概要](../ado-net-overview.md)
