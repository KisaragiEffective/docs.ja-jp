---
title: SQL Server 接続プール
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7e51d44e-7c4e-4040-9332-f0190fe36f07
ms.openlocfilehash: 3bf0ce98b9b16b8d698a814f3bf2c4f442f3bf06
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980042"
---
# <a name="sql-server-connection-pooling-adonet"></a>SQL Server の接続プール (ADO.NET)
通常、データベース サーバーへの接続は、時間のかかるいくつかの手順で構成されています。 ソケットまたは名前付きパイプなどの物理チャネルの確立、サーバーとの最初のハンドシェイクの実行、接続文字列の情報の解析、サーバーによる接続の認証、現在のトランザクションへ参加するための検証などの手順を行う必要があります。  
  
 実際には、ほとんどのアプリケーションで、1 つまたはいくつかの違いがあるだけの構成を接続に使用しています。 つまり、アプリケーションの実行時に、多数の同一の接続が繰り返し開かれ、閉じられているのです。 接続を開くコストを最小限に抑えるために、ADO.NET は*接続プール*と呼ばれる最適化手法を使用します。  
  
 接続プールは、新しく開く必要のある接続の数を減らします。 *プーラー*は、物理的な接続の所有権を保持します。 プーラーは、任意の接続構成それぞれのアクティブな接続のセットをそのまま保持して、接続を管理します。 この接続の `Open` を呼び出すと、プーラーは、プールに使用可能な接続があるかどうかを確認します。 プールされた接続が使用できる場合は、新しい接続を開く代わりに、プールされた接続を呼び出し元に返します。 アプリケーションが接続で `Close` を呼び出すと、プーラーは接続を閉じる代わりに、プールされたアクティブな接続のセットに接続を返します。 接続がプールに返されると、その接続は、次の `Open` 呼び出しで再度使用できる状態になります。  
  
 同じ構成を持つ接続のみをプールすることができます。 ADO.NET は、構成ごとに1つずつ、複数のプールを同時に保持します。 接続は接続文字列によってプール内で区別されます。また、統合セキュリティを使用している場合は、Windows ID によっても区別されます。 接続がプールされるかどうかは、その接続がトランザクションに参加しているかどうかにも依存します。 <xref:System.Data.SqlClient.SqlConnection.ChangePassword%2A> を使用すると、<xref:System.Data.SqlClient.SqlCredential> インスタンスは接続プールに影響します。 ユーザー ID とパスワードが同じでも、<xref:System.Data.SqlClient.SqlCredential> のインスタンスごとに異なる接続プールが使用されます。  
  
 接続をプールすると、アプリケーションのパフォーマンスとスケーラビリティを大幅に改善できます。 既定では、ADO.NET で接続プールが有効になっています。 明示的に無効にしない限り、プーラーは、アプリケーションで接続が開かれたり、閉じられたりする際に接続を最適化します。 また、接続プール機能の動作を制御する接続文字列修飾子を指定することもできます。 詳細については、このトピックで後述する「接続文字列キーワードによる接続プールの制御」を参照してください。  
  
> [!NOTE]
> 接続プールが有効になっている場合とタイムアウト エラーや他のログイン エラーが発生した場合は、例外がスローされ、それ以降に接続を試みると、5 秒間の "ブロック期間" 中失敗します。 ブロック期間内にアプリケーションが接続を試みると、最初の例外が再度スローされます。 ブロック期間の終了後も失敗が続く場合は、最大 1 分として、新しいブロック期間は前のブロック期間の 2 倍の長さになります。  
  
## <a name="pool-creation-and-assignment"></a>プールの作成と割り当て  
 接続が最初に開かれると、完全一致のアルゴリズムに基づいて接続プールが作成され、接続内の接続文字列に関連付けられます。 各接続プールが別の接続文字列に関連付けられます。 新しい接続が開かれたとき、接続文字列が既存のプールと完全に一致しない場合は、新しいプールが作成されます。 接続は、プロセス、アプリケーション ドメイン、接続文字列、Windows ID (統合セキュリティを使用している場合) ごとにプールされます。 また、接続文字列は完全に一致していることが必要です。同じ接続に対してキーワードを異なる順序で指定すると別々にプールされます。  
  
 3 つの新しい <xref:System.Data.SqlClient.SqlConnection> オブジェクトを作成し、2 つの接続プールだけでこの 3 つのオブジェクトを管理する C# の例を次に示します。 1 番目の接続文字列と 2 番目の接続文字列では、`Initial Catalog` に割り当てる値が異なります。  
  
```csharp
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=Northwind"))  
    {  
        connection.Open();        
        // Pool A is created.  
    }  
  
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=pubs"))  
    {  
        connection.Open();        
        // Pool B is created because the connection strings differ.  
    }  
  
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=Northwind"))  
    {  
        connection.Open();        
        // The connection string matches pool A.  
    }  
```  
  
 接続文字列で `MinPoolSize` が指定されていない、またはゼロに指定されている場合、プール内の接続は、アクティブではない一定の期間後に閉じられます。 ただし、`MinPoolSize` がゼロより大きい値に設定されている場合、接続プールは、`AppDomain` がアンロードされ、プロセスが終了するまで破棄されません。 アクティブでないプールまたは空のプールを維持するためには、最小限のシステム オーバーヘッドが発生します。  
  
> [!NOTE]
> フェールオーバーなど、致命的なエラーが発生すると、プールは自動的にクリアされます。  
  
## <a name="adding-connections"></a>接続の追加  
 一意の接続文字列ごとに 1 つの接続プールが作成されます。 プールが作成された段階で、複数の接続オブジェクトが作成されてそのプールへ追加され、最小プール サイズ要件を満たします。 必要に応じて、指定された最大プール サイズ (既定は 100) に達するまで、接続がプールへ追加されます。 接続が終了または破棄されると、その接続は解放され、プールに戻されます。  
  
 <xref:System.Data.SqlClient.SqlConnection> オブジェクトが要求されると、プールに使用可能な接続がある場合はプールから取得されます。 使用可能な接続とは、サーバーへの有効なリンクを持つ接続のうち、使用中でないか、一致するトランザクション コンテキストを持っているか、またはどのトランザクション コンテキストにも関連付けられていない接続のことです。  
  
 接続プーラーは、接続がプールに解放されたときに接続の再割り当てを行って、接続に対する要求に応えます。 最大プール サイズに達すると、使用可能な接続を取得できなくなり、要求はキューに置かれます。 プーラーは、タイムアウト (既定は 15 秒) に達するまで接続の再利用を試みます。 接続がタイムアウトになる前に、プーラーが要求を満たすことができない場合は、例外がスローされます。  
  
> [!CAUTION]
> 接続がプールに返されるようにするために、接続を使い終えたら必ず接続を終了することを強くお勧めします。 これを行うには、`Connection` オブジェクトの `Close` または `Dispose` のいずれかのメソッドを使用するか、でC#`using` ステートメント内のすべての接続を開くか、`Using` の Visual Basic ステートメントを使用します。 明示的に終了されていない接続は、プールに追加したり返したりすることができないことがあります。 詳細については、「 [Using ステートメント](../../../csharp/language-reference/keywords/using-statement.md)」または「[方法: Visual Basic のシステムリソースを破棄](../../../visual-basic/programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource.md)する」を参照してください。  
  
> [!NOTE]
> クラスの `Close` メソッド内で `Dispose`、`Connection`、またはその他のマネージド オブジェクトの `DataReader` または `Finalize` を呼び出さないでください。 終了処理では、クラスに直接所有されているアンマネージ リソースだけを解放してください。 クラスがアンマネージ リソースを所有していない場合は、クラス定義に `Finalize` メソッドを含めないでください。 詳細については、「[ガベージコレクション](../../../standard/garbage-collection/index.md)」を参照してください。  
  
接続の開始と終了に関連するイベントの詳細については、SQL Server のドキュメントの「 [Audit Login イベントクラス](/sql/relational-databases/event-classes/audit-login-event-class)」および「 [Audit Logout イベントクラス](/sql/relational-databases/event-classes/audit-logout-event-class)」を参照してください。  
  
## <a name="removing-connections"></a>接続の削除  
 接続プール機能は、アイドル状態の時間が約 4-8 分になったか、サーバーとの接続が切断されたことをプール機能が検出した場合に、プールからの接続を削除します。 サーバーとの通信を試みた後にのみ、切断されたサーバー接続が検出可能になることに注意してください。 接続がサーバーに接続していないことがわかると、その接続は無効としてマークされます。 無効な接続は、閉じられるか、または再利用された場合のみ、接続プールから削除されます。  
  
 既に存在しないサーバーへの接続が存在する場合は、接続プーラーが、その接続が切断されていることをまだ検出せず、無効というマークを付けていない状況のときでも、プールからその接続を削除できます。 この機能は、接続がまだ有効であることを確認するオーバーヘッドによってサーバーへのラウンド トリップが実行されることにより、プーラーの利点が失われてしまうことを防ぐためにあります。 この状況が発生した場合は、接続の使用を最初に試みたときに接続が切断されていることが検出され、例外がスローされます。  
  
## <a name="clearing-the-pool"></a>プールのクリア  
 ADO.NET 2.0 では、<xref:System.Data.SqlClient.SqlConnection.ClearAllPools%2A> と <xref:System.Data.SqlClient.SqlConnection.ClearPool%2A>プールをクリアする2つの新しいメソッドが導入されました。 `ClearAllPools` は、指定されたプロバイダーの接続プールをクリアし、`ClearPool` は、特定の接続に関連付けられた接続プールをクリアします。 これらのメソッドが呼び出されたときに使用中の接続がある場合は、適切にマーク付けされ、 接続が閉じられるとプールに返されずに破棄されます。  
  
## <a name="transaction-support"></a>トランザクションのサポート  
 接続はプールから取り出され、トランザクション コンテキストに基づいて割り当てられます。 接続文字列で `Enlist=false` が指定されていない限り、接続プールは、接続を <xref:System.Transactions.Transaction.Current%2A> コンテキストの中に参加させます。 接続が閉じられ、参加した `System.Transactions` トランザクションと共にプールに返されると、同じ `System.Transactions` トランザクションを持つ接続プールに対する次の要求でも、使用可能であれば同じ接続を返すように接続が保持されます。 このような要求が発行された場合で、なおかつ、プールされた接続が使用できない場合は、プールの非トランザクション部分から接続を取り出して参加させます。 プールのどちらの領域にも使用できる接続がない場合は、新しい接続を作成して参加させます。  
  
 接続が終了すると、その接続は解放されてプールへ返り、さらに、そのトランザクション コンテキストに基づいて特定のサブプールへ返ります。 そのため、分散トランザクションが保留状態である場合を含め、エラーを発生させることなく、開発者が接続を終了させることは可能です。 これにより、分散トランザクションを後でコミットまたは中止できます。  
  
## <a name="controlling-connection-pooling-with-connection-string-keywords"></a>接続文字列キーワードによる接続プールの制御  
 `ConnectionString` オブジェクトの <xref:System.Data.SqlClient.SqlConnection> プロパティは、接続プール ロジックの動作を調整するために使用できる接続文字列キーおよび値のペアをサポートします。 詳細については、「 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>」を参照してください。  
  
## <a name="pool-fragmentation"></a>プールの断片化  
 プールの断片化は、プロセスが終了するまで解放されないプールを多数作成できる Web アプリケーションの多くで一般的な問題です。 これにより、多数の接続が開いたままになってメモリを消費し、パフォーマンスが低下します。  
  
### <a name="pool-fragmentation-due-to-integrated-security"></a>統合セキュリティによるプールの断片化  
 接続は、接続文字列とユーザー ID に基づいてプールされます。 したがって、Web サイトで基本認証または Windows 認証を使用していて、統合セキュリティ ログインを使用している場合は、1 ユーザーにつき 1 つのプールが作成されます。 これによって、単一のユーザーによる後続のデータベース要求のパフォーマンスが向上しますが、他のユーザーによって作成された接続は使用できません。 また、1 ユーザーにつき少なくとも 1 つの接続がデータベース サーバーに存在することになります。 これは、開発者がセキュリティおよび監査要件に対して、特定の Web アプリケーションのアーキテクチャに重点を置く必要があるために起こる副作用です。  
  
### <a name="pool-fragmentation-due-to-many-databases"></a>多数のデータベースによるプールの断片化  
 インターネット サービス プロバイダーの多くは、1 つのサーバー上で複数の Web サイトをホストしています。 インターネット サービス プロバイダーは、1 つのデータベースを使用してフォーム認証ログインを確認し、そのユーザーまたはユーザー グループの特定のデータベースへの接続を開きます。 認証データベースへの接続はプールされ、すべてのユーザーが使用できるようになります。 ただし、各データベースに対して個別の接続のプールが存在するため、サーバーへの接続数が増加します。  
  
 これもまた、アプリケーションのデザインの副作用です。 SQL Server に接続するときにセキュリティを損なうことなく、この副作用を回避する比較的簡単な方法があります。 各ユーザーまたはグループの個別のデータベースに接続する代わりに、サーバー上の同じデータベースに接続してから、Transact-SQL USE ステートメントを実行して目的のデータベースに変更します。 `master` データベースへの最初の接続を作成し、`databaseName` 文字列変数で指定した目的のデータベースに切り替えるコードを次に示します。  
  
```vb  
' Assumes that command is a valid SqlCommand object and that  
' connectionString connects to master.  
    command.Text = "USE DatabaseName"  
Using connection As New SqlConnection(connectionString)  
    connection.Open()  
    command.ExecuteNonQuery()  
End Using  
```  
  
```csharp  
// Assumes that command is a SqlCommand object and that  
// connectionString connects to master.  
command.Text = "USE DatabaseName";  
using (SqlConnection connection = new SqlConnection(  
  connectionString))  
  {  
    connection.Open();  
    command.ExecuteNonQuery();  
  }  
```  
  
## <a name="application-roles-and-connection-pooling"></a>アプリケーション ロールおよび接続プール  
 `sp_setapprole` システム ストアド プロシージャの呼び出しにより SQL Server のアプリケーション ロールが起動された後は、その接続のセキュリティ コンテキストをリセットすることはできません。 ただし、プールを有効した場合は、プールに接続が返され、プール接続が再利用されると、エラーが発生します。 詳細については、サポート技術情報の記事「[OLE DB リソースプーリングを使用した SQL アプリケーションロールエラー](https://support.microsoft.com/default.aspx?scid=KB;EN-US;Q229564)」を参照してください。  
  
### <a name="application-role-alternatives"></a>アプリケーション ロールに代わる方法  
 アプリケーション ロールに代わるセキュリティ メカニズムの使用をお勧めします。 詳細については、「 [SQL Server でのアプリケーションロールの作成](./sql/creating-application-roles-in-sql-server.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [接続プール](connection-pooling.md)
- [SQL Server と ADO.NET](./sql/index.md)
- [Performance Counters](performance-counters.md)
- [ADO.NET の概要](ado-net-overview.md)
