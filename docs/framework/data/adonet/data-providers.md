---
title: .NET Framework データ プロバイダー
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 03a9fc62-2d24-491a-9fe6-d6bdb6dcb131
ms.openlocfilehash: 2c986aab33f2c4dcefb5924ea61e8b9f6b3c50a3
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347809"
---
# <a name="net-framework-data-providers"></a>.NET Framework データ プロバイダー
.NET Framework データプロバイダーは、データベースへの接続、コマンドの実行、および結果の取得に使用されます。 その結果は、直接処理されるか、必要に応じてユーザーに公開されるように <xref:System.Data.DataSet> に格納されるか、取得したデータセットを複数のソースからのデータと組み合わせるか、または、層間でリモート処理されます。 .NET Framework データプロバイダーは軽量であり、データソースとコードの間に最小限のレイヤーを作成することにより、機能を損なうことなくパフォーマンスを向上させることができます。  
  
 次の表に、.NET Framework に含まれているデータプロバイダーを示します。  
  
|.NET Framework データ プロバイダー|説明|  
|-------------------------------------------------------------------------------|-----------------|  
|.NET Framework Data Provider for SQL Server|Microsoft SQL Server へのデータ アクセスを提供します。 <xref:System.Data.SqlClient> 名前空間を使用してください。|  
|.NET Framework Data Provider for OLE DB|OLE DB を使用して公開されるデータ ソースに対応。 <xref:System.Data.OleDb> 名前空間を使用してください。|  
|.NET Framework Data Provider for ODBC|ODBC を使用して公開されるデータ ソースに対応。 <xref:System.Data.Odbc> 名前空間を使用してください。|  
|.NET Framework Data Provider for Oracle|Oracle データ ソースに対応。 Oracle 用の .NET Framework Data Provider は、Oracle クライアントソフトウェアバージョン8.1.7 以降以降をサポートし、<xref:System.Data.OracleClient> 名前空間を使用します。|  
|EntityClient プロバイダー|エンティティ データ モデル (EDM) アプリケーションにデータ アクセスを提供します。 <xref:System.Data.EntityClient> 名前空間を使用してください。|  
|SQL Server Compact 4.0 の .NET Framework Data Provider。|Microsoft SQL Server Compact 4.0 へのデータアクセスを提供します。 [System.Data.SqlServerCe](https://docs.microsoft.com/previous-versions/sql/compact/sql-server-compact-4.0/ec4st0e3(v=vs.100)) 名前空間を使用します。|  
  
## <a name="core-objects-of-net-framework-data-providers"></a>.NET Framework Data Providers の核となるオブジェクト  
 次の表は、.NET Framework データプロバイダーを構成する4つの主要なオブジェクトの概要を示しています。  
  
|オブジェクト|説明|  
|------------|-----------------|  
|`Connection`|特定のデータ ソースへの接続を確立します。 すべての `Connection` オブジェクトの基本クラスは <xref:System.Data.Common.DbConnection> クラスです。|  
|`Command`|データ ソースに対してコマンドを実行します。 `Parameters` を公開し、 `Transaction` から `Connection`のスコープ内で実行できます。 すべての `Command` オブジェクトの基本クラスは <xref:System.Data.Common.DbCommand> クラスです。|  
|`DataReader`|データ ソースから、前方参照専用で読み取り専用のデータ ストリームを読み取ります。 すべての `DataReader` オブジェクトの基本クラスは <xref:System.Data.Common.DbDataReader> クラスです。|  
|`DataAdapter`|`DataSet` にデータ ソースのデータを読み込んだり、データ ソースの更新内容を解決したりします。 すべての `DataAdapter` オブジェクトの基本クラスは <xref:System.Data.Common.DbDataAdapter> クラスです。|  
  
 このドキュメントの前の表に挙げたコアクラスに加えて、.NET Framework データプロバイダーには、次の表に示すクラスも含まれています。  
  
|オブジェクト|説明|  
|------------|-----------------|  
|`Transaction`|データ ソースでトランザクション内にコマンドを追加します。 すべての `Transaction` オブジェクトの基本クラスは <xref:System.Data.Common.DbTransaction> クラスです。 ADO.NET は、 <xref:System.Transactions> 名前空間のクラスを使ったトランザクションもサポートします。|  
|`CommandBuilder`|`DataAdapter` のコマンド プロパティを自動的に生成したり、ストアド プロシージャからパラメーター情報を取得したり、 `Parameters` オブジェクトの `Command` コレクションにパラメーターを設定したりするためのヘルパー オブジェクトです。 すべての `CommandBuilder` オブジェクトの基本クラスは <xref:System.Data.Common.DbCommandBuilder> クラスです。|  
|`ConnectionStringBuilder`|`Connection` オブジェクトが使用する接続文字列を簡単に作成および管理するためのヘルパー オブジェクトです。 すべての `ConnectionStringBuilder` オブジェクトの基本クラスは <xref:System.Data.Common.DbConnectionStringBuilder> クラスです。|  
|`Parameter`|コマンドやストアド プロシージャの入力パラメーター、出力パラメーター、および戻り値パラメーターを定義します。 すべての `Parameter` オブジェクトの基本クラスは <xref:System.Data.Common.DbParameter> クラスです。|  
|`Exception`|データ ソースでエラーが検出されたときに返されます。 クライアントでエラーが発生した場合、.NET Framework データプロバイダーは .NET Framework 例外をスローします。 すべての `Exception` オブジェクトの基本クラスは <xref:System.Data.Common.DbException> クラスです。|  
|`Error`|データ ソースから返された警告またはエラー情報を公開します。|  
|`ClientPermission`|.NET Framework データプロバイダーのコードアクセスセキュリティ属性用に用意されています。 すべての `ClientPermission` オブジェクトの基本クラスは <xref:System.Data.Common.DBDataPermission> クラスです。|  
  
## <a name="net-framework-data-provider-for-sql-server-sqlclient"></a>SQL Server の .NET Framework Data Provider (SqlClient)  
 SQL Server 用の .NET Framework Data Provider (SqlClient) は、独自のプロトコルを使用して SQL Server と通信します。 これは軽量であり、OLE DB または Open Database Connectivity (ODBC) レイヤーを追加せずに SQL Server に直接アクセスできるように最適化されているため、適切に動作します。 次の図は、Data Provider の .NET Framework OLE DB を使用して SQL Server の .NET Framework Data Provider を比較しています。 OLE DB の .NET Framework Data Provider は、接続プールとトランザクションサービスを提供する OLE DB サービスコンポーネントと、データソースの OLE DB プロバイダーの両方を介して、OLE DB データソースと通信します。  
  
> [!NOTE]
> ODBC 用の .NET Framework Data Provider は、OLE DB の .NET Framework Data Provider に似たアーキテクチャを備えています。たとえば、ODBC サービスコンポーネントを呼び出します。  
  
 ![データ プロバイダー](./media/netdataproviders-bpuedev11.gif "NETDataProviders_bpuedev11")  
.NET Framework Data Provider for SQL Server と .NET Framework Data Provider for OLE DB の比較  
  
 SQL Server クラスの .NET Framework Data Provider は、<xref:System.Data.SqlClient> 名前空間にあります。  
  
 SQL Server の .NET Framework Data Provider では、ローカルトランザクションと分散トランザクションの両方がサポートされます。 分散トランザクションの場合、既定では SQL Server の .NET Framework Data Provider は、トランザクションに自動的に参加し、Windows コンポーネントサービスまたは <xref:System.Transactions>からトランザクションの詳細を取得します。 詳細については、「[トランザクションと同時実行](transactions-and-concurrency.md)」を参照してください。  
  
 名前空間 `System.Data.SqlClient` をユーザーのアプリケーションにインクルードする方法を次のコード サンプルで示します。  
  
```vb  
Imports System.Data.SqlClient  
```  
  
```csharp  
using System.Data.SqlClient;  
```  
  
## <a name="net-framework-data-provider-for-ole-db"></a>.NET Framework Data Provider for OLE DB  
 OLE DB 用の .NET Framework Data Provider (OleDb) は、COM 相互運用機能を介してネイティブ OLE DB を使用して、データアクセスを有効にします。 OLE DB の .NET Framework Data Provider では、ローカルトランザクションと分散トランザクションの両方がサポートされます。 分散トランザクションの場合、既定では OLE DB の .NET Framework Data Provider は、トランザクションに自動的に参加し、トランザクションの詳細を Windows コンポーネントサービスから取得します。 詳細については、「[トランザクションと同時実行](transactions-and-concurrency.md)」を参照してください。  
  
 次の表は、ADO.NET でテストされたプロバイダーを示しています。  
  
|ドライバー|プロバイダー|  
|------------|--------------|  
|SQLOLEDB|Microsoft OLE DB provider for SQL Server|  
|MSDAORA|Microsoft OLE DB Provider for Oracle|  
|Microsoft.Jet.OLEDB.4.0|OLE DB Provider for Microsoft Jet|  
  
> [!NOTE]
> ASP.NET アプリケーションなど、マルチスレッドアプリケーションのデータソースとして Access (Jet) データベースを使用することはお勧めできません。 Jet を ASP.NET アプリケーションのデータソースとして使用する必要がある場合は、Access データベースに接続する ASP.NET アプリケーションで接続の問題が発生する可能性があることに注意してください。  
  
 OLE DB の .NET Framework Data Provider は OLE DB バージョン2.5 のインターフェイスをサポートしていません。 OLE DB 2.5 インターフェイスのサポートを必要とする OLE DB プロバイダーは、OLE DB の .NET Framework Data Provider では正しく機能しません。 これには Microsoft OLE DB Provider for Exchange および Microsoft OLE DB Provider for Internet Publishing が含まれます。  
  
 OLE DB の .NET Framework Data Provider は、OLE DB Provider for ODBC (MSDASQL) では機能しません。 ADO.NET を使用して ODBC データソースにアクセスするには、ODBC の .NET Framework Data Provider を使用します。  
  
 OLE DB クラスの .NET Framework Data Provider は、<xref:System.Data.OleDb> 名前空間にあります。 名前空間 `System.Data.OleDb` をユーザーのアプリケーションにインクルードする方法を次のコード サンプルで示します。  
  
```vb  
Imports System.Data.OleDb  
```  
  
```csharp  
using System.Data.OleDb;  
```  
  
## <a name="net-framework-data-provider-for-odbc"></a>.NET Framework Data Provider for ODBC  
 .NET Framework Data Provider for ODBC (odbc) では、ネイティブ ODBC ドライバーマネージャー (DM) を使用してデータアクセスを有効にします。 ODBC データ プロバイダーはローカル トランザクションと分散トランザクションのどちらもサポートします。 分散トランザクションの場合、既定で、ODBC データ プロバイダーは自動的にトランザクションに参加し、トランザクションの詳細を Windows コンポーネント サービスから取得します。 詳細については、「[トランザクションと同時実行](transactions-and-concurrency.md)」を参照してください。  
  
 次の表は、ADO.NET でテストされた ODBC ドライバーを示しています。  
  
|ドライバー|  
|------------|  
|SQL Server|  
|Microsoft ODBC for Oracle|  
|Microsoft Access ドライバー (*.mdb)|  
  
 ODBC クラスの .NET Framework Data Provider は <xref:System.Data.Odbc> 名前空間にあります。  
  
 名前空間 `System.Data.Odbc` をユーザーのアプリケーションにインクルードする方法を次のコード サンプルで示します。  
  
```vb  
Imports System.Data.Odbc  
```  
  
```csharp  
using System.Data.Odbc;  
```  
  
> [!NOTE]
> ODBC の .NET Framework Data Provider には、MDAC 2.6 以降のバージョンが必要です。 MDAC 2.8 SP1 をお勧めします。 MDAC 2.8 SP1 は、 [Microsoft ダウンロードセンター](https://www.microsoft.com/download/details.aspx?id=5793)からダウンロードできます。
  
## <a name="net-framework-data-provider-for-oracle"></a>.NET Framework Data Provider for Oracle  
 .NET Framework Data Provider for Oracle (System.data.oracleclient) を使用すると、oracle クライアント接続ソフトウェアを介して Oracle データソースへのデータアクセスが可能になります。 このデータ プロバイダーは Oracle クライアント ソフトウェア バージョン 8.1.7 以降をサポートしています。 データ プロバイダーはローカル トランザクションと分散トランザクションのどちらもサポートします。 詳細については、「[トランザクションと同時実行](transactions-and-concurrency.md)」を参照してください。  
  
 Oracle の .NET Framework Data Provider では、oracle データソースに接続する前に、システムに Oracle クライアントソフトウェア (バージョン8.1.7 以降以降) が必要です。  
  
 Oracle クラスの .NET Framework Data Provider は <xref:System.Data.OracleClient> 名前空間にあり、`System.Data.OracleClient.dll` アセンブリに含まれています。 このデータ プロバイダーを使用するアプリケーションをコンパイルする場合は、 `System.Data.dll` と `System.Data.OracleClient.dll` の両方を参照する必要があります。  
  
 名前空間 `System.Data.OracleClient` をユーザーのアプリケーションにインクルードする方法を次のコード サンプルで示します。  
  
```vb  
Imports System.Data  
Imports System.Data.OracleClient  
```  
  
```csharp  
using System.Data;  
using System.Data.OracleClient;  
```  
  
## <a name="choosing-a-net-framework-data-provider"></a>.NET Framework データ プロバイダーの選択  
 アプリケーションの設計とデータソースに応じて、.NET Framework データプロバイダーを選択することで、アプリケーションのパフォーマンス、機能、および整合性を向上させることができます。 次の表では、各 .NET Framework データプロバイダーの利点と制限事項について説明します。  
  
|プロバイダー|メモ|  
|--------------|-----------|  
|.NET Framework Data Provider for SQL Server|Microsoft SQL Server を使用する中間層アプリケーションに推奨されます。<br /><br /> Microsoft データベースエンジン (MSDE) または SQL Server を使用する1層アプリケーションに推奨されます。<br /><br /> OLE DB provider for SQL Server (SQLOLEDB) を OLE DB の .NET Framework Data Provider と共に使用することをお勧めします。|  
|.NET Framework Data Provider for OLE DB|SQL Server の場合、このプロバイダーの代わりに SQL Server の Data Provider .NET Framework をお勧めします。<br /><br /> Microsoft Access データベースを使用する単層アプリケーションに推奨されます。 Access データベースを中間層アプリケーションで使用することはお勧めできません。|  
|.NET Framework Data Provider for ODBC|中間層アプリケーションおよび単層アプリケーションで ODBC データ ソースを使用する場合に推奨します。|  
|.NET Framework Data Provider for Oracle|中間層アプリケーションおよび単層アプリケーションで Oracle データ ソースを使用する場合に推奨します。|  
  
## <a name="entityclient-provider"></a>EntityClient プロバイダー  
 EntityClient プロバイダーは、エンティティ データ モデル (EDM) に基づくデータ アクセスで使用されます。 他の .NET Framework データ プロバイダーとは異なり、データ ソースと直接やり取りしません。 代わりに Entity SQL を使用して、基になるデータ プロバイダーと通信します。 詳細については、「 [Entity Framework 用の EntityClient プロバイダー](./ef/entityclient-provider-for-the-entity-framework.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET の概要](ado-net-overview.md)
- [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)
