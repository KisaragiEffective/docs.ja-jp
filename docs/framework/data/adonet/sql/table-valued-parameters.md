---
title: テーブル値パラメーター
ms.date: 10/12/2018
dev_langs:
- csharp
- vb
ms.assetid: 370c16d5-db7b-43e3-945b-ccaab35b739b
ms.openlocfilehash: d99cea1641dc61c1cae6d6b1634359211ce788ae
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452306"
---
# <a name="table-valued-parameters"></a>テーブル値パラメーター
テーブル値パラメーターは、複数行のデータをクライアント アプリケーションから SQL Server に簡単にマーシャリングするための手段です。複数のラウンド トリップや、データ処理用の特別なサーバー側ロジックは必要ありません。 テーブル値パラメーターは、クライアント アプリケーションのデータの行をカプセル化して、単一のパラメーター化コマンドでサーバーに送信する目的でも使用できます。 受信データ行はテーブル変数に格納され、Transact-SQL によって操作できるようになります。  
  
 テーブル値パラメーターの列値には、Transact-SQL の標準的な SELECT ステートメントを使ってアクセスできます。 テーブル値パラメーターは厳密に型指定されており、その構造が自動的に検証されます。 テーブル値パラメーターのサイズは、サーバーのメモリによってのみ制限されます。  
  
> [!NOTE]
> テーブル値パラメーターにデータを取得することはできません。 テーブル値パラメーターは入力専用です。OUTPUT キーワードはサポートされません。  
  
 テーブル値パラメーターの詳細については、次のリソースを参照してください。  
  
|リソース|説明|  
|--------------|-----------------|  
|[テーブル値パラメーターの使用 (データベース エンジン)](/sql/relational-databases/tables/use-table-valued-parameters-database-engine)|テーブル値パラメーターの作成方法および使用方法について説明します。|  
|[ユーザー定義テーブル型](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/bb522526(v=sql.100))|テーブル値パラメーターを宣言する際に使用するユーザー定義テーブル型について説明します。|  
  
## <a name="passing-multiple-rows-in-previous-versions-of-sql-server"></a>旧バージョンの SQL Server での複数行の受け渡し  
 SQL Server 2008 にテーブル値パラメーターが導入されるまでは、複数行データをストアド プロシージャまたはパラメーター化 SQL コマンドに渡す方法は限られていました。 複数行をサーバーに渡す方法には、次のオプションがありました。  
  
- 複数のデータ列およびデータ行の値を表す一連の個別パラメーターを使用する。 この方法で渡すことのできるデータの量は、使用可能なパラメーターの数によって制限されます。 SQL Server プロシージャが持つことのできるパラメーター数は最大 2,100 です。 これらの個々の値をテーブル変数または一時テーブルにまとめて処理するには、サーバー側のロジックが必要です。  
  
- 複数のデータを区切られた文字列または XML ドキュメントとしてまとめ、そのテキスト値をプロシージャまたはステートメントに渡す。 これには、データ構造を検証して値を処理するためのロジックをプロシージャまたはステートメントに含める必要があります。  
  
- 複数の行に影響を与えるデータ変更のための一連の SQL ステートメントを作成する。たとえば、`Update` の <xref:System.Data.SqlClient.SqlDataAdapter> メソッドを呼び出すことによって作成できます。 変更はサーバーに個別に送ることもグループにまとめて送ることもできます。 ただし、複数のステートメントを含むバッチを送信しても、サーバーでは個々のステートメントが別々に実行されます。  
  
- `bcp` ユーティリティ プログラムまたは <xref:System.Data.SqlClient.SqlBulkCopy> オブジェクトを使用して、多数行のデータをテーブルに読み込む。 この方法は効率的ですが、データが一時テーブルまたはテーブル変数に読み込まれなければ、サーバー側での処理がサポートされません。  
  
## <a name="creating-table-valued-parameter-types"></a>テーブル値パラメーター型の作成  
 テーブル値パラメーターは、Transact-SQL の CREATE TYPE ステートメントを使用して定義された厳密に型指定されたテーブルの構造に基づいています。 クライアント アプリケーションでテーブル値パラメーターを使用するには、まず SQL Server でテーブル型を作成し、その構造を定義する必要があります。 テーブル型の作成の詳細については、「[ユーザー定義テーブル型](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/bb522526(v=sql.100))」を参照してください。  
  
 次のステートメントは、CategoryID と CategoryName 列から成る CategoryTableType というテーブル型を作成します。  
  
```sql
CREATE TYPE dbo.CategoryTableType AS TABLE  
    ( CategoryID int, CategoryName nvarchar(50) )  
```  
  
 テーブル型を作成したら、その型に基づいてテーブル値パラメーターを宣言できます。 次の Transact-SQL フラグメントは、ストアド プロシージャ定義の中でテーブル値パラメーターを宣言する方法を示しています。 テーブル値パラメーターの宣言には READONLY キーワードが必要であることに注意してください。  
  
```sql
CREATE PROCEDURE usp_UpdateCategories   
    (@tvpNewCategories dbo.CategoryTableType READONLY)  
```  
  
## <a name="modifying-data-with-table-valued-parameters-transact-sql"></a>テーブル値パラメーターによるデータの変更 (Transact-SQL)  
 テーブル値パラメーターは、単一のステートメントを実行して複数行を操作する、セット ベースのデータ変更の中で使用できます。 たとえば、テーブル値パラメーターのすべての行を選択し、それらをデータベース テーブルに挿入できます。また、テーブル値パラメーターを更新対象のテーブルに結合する更新ステートメントを作成することもできます。  
  
 次の Transact-SQL UPDATE ステートメントは、テーブル値パラメーターを Categories テーブルに結合して使用する方法を示しています。 テーブル値パラメーターを FROM 句の JOIN で使用するときは、エイリアスを使用する必要があります。この例ではテーブル値パラメーターに "ec" というエイリアスが使用されています。  
  
```sql
UPDATE dbo.Categories  
    SET Categories.CategoryName = ec.CategoryName  
    FROM dbo.Categories INNER JOIN @tvpEditedCategories AS ec  
    ON dbo.Categories.CategoryID = ec.CategoryID;  
```  
  
 この Transact-SQL の例は、単一のセット ベース操作で INSERT を実行するためにテーブル値パラメーターから行を選択する方法を示しています。  
  
```sql
INSERT INTO dbo.Categories (CategoryID, CategoryName)  
    SELECT nc.CategoryID, nc.CategoryName FROM @tvpNewCategories AS nc;  
```  
  
## <a name="limitations-of-table-valued-parameters"></a>テーブル値パラメーターの制限  
 テーブル値パラメーターにはいくつかの制限があります。  
  
- テーブル値パラメーターを [CLR ユーザー定義の関数](/sql/relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions)に渡すことはできません。  
  
- テーブル値パラメーターでは、UNIQUE 制約または PRIMARY KEY 制約をサポートするためにのみ、インデックスを付けることができます。 SQL Server では、テーブル値パラメーターの統計が保持されません。  
  
- テーブル値パラメーターは Transact-SQL コードの中では読み取り専用です。 テーブル値パラメーターの行内の列の値は更新できません。行を挿入することも削除することもできません。 テーブル値パラメーター内のストアド プロシージャやパラメーター化ステートメントに渡すデータを変更するには、一時テーブルまたはテーブル変数にデータを挿入する必要があります。  
  
- ALTER TABLE ステートメントをテーブル値パラメーターの設計変更に使用することはできません。  
  
## <a name="configuring-a-sqlparameter-example"></a>SqlParameter の構成例  
 <xref:System.Data.SqlClient> では、テーブル値パラメーターのデータを <xref:System.Data.DataTable>、<xref:System.Data.Common.DbDataReader>、または <xref:System.Collections.Generic.IEnumerable%601> \ <xref:Microsoft.SqlServer.Server.SqlDataRecord> オブジェクトから読み込むことができます。 <xref:System.Data.SqlClient.SqlParameter.TypeName%2A> の <xref:System.Data.SqlClient.SqlParameter> プロパティを使用して、テーブル値パラメーターの型名を指定する必要があります。 `TypeName` は、既にサーバー上に作成されている、互換性のある型の名前と一致していることが必要です。 次のコード フラグメントは、データを挿入するための <xref:System.Data.SqlClient.SqlParameter> の構成方法を示しています。  
 
次の例では、`addedCategories` 変数に <xref:System.Data.DataTable> が含まれています。 変数がどのように設定されているかを確認するには、次のセクションの例を参照してください。[テーブル値パラメーターをストアドプロシージャに渡し](#passing)ます。

```csharp  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
```  
  
```vb  
' Configure the command and parameter.  
Dim insertCommand As New SqlCommand(sqlInsert, connection)  
Dim tvpParam As SqlParameter = _  
   insertCommand.Parameters.AddWithValue( _  
  "@tvpNewCategories", addedCategories)  
tvpParam.SqlDbType = SqlDbType.Structured  
tvpParam.TypeName = "dbo.CategoryTableType"  
```  
  
 <xref:System.Data.Common.DbDataReader> から派生した任意のオブジェクトを使用して、一連の行データをテーブル値パラメーターに挿入することもできます。その方法を次のフラグメントに示します。  
  
```csharp  
// Configure the SqlCommand and table-valued parameter.  
SqlCommand insertCommand = new SqlCommand("usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", dataReader);  
tvpParam.SqlDbType = SqlDbType.Structured;  
```  
  
```vb  
' Configure the SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  dataReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
```  
  
## <a name="passing"></a>ストアドプロシージャへのテーブル値パラメーターの引き渡し  
 この例は、テーブル値パラメーターのデータをストアド プロシージャに渡す方法を示しています。 このコードは、<xref:System.Data.DataTable> メソッドを使用して、追加された行を新しい <xref:System.Data.DataTable.GetChanges%2A> に抽出します。 次に <xref:System.Data.SqlClient.SqlCommand> を定義し、<xref:System.Data.SqlClient.SqlCommand.CommandType%2A> プロパティを <xref:System.Data.CommandType.StoredProcedure> に設定します。 <xref:System.Data.SqlClient.SqlParameter> へのデータ入力には <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A> メソッドが使用され、<xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> は `Structured` に設定されます。 次に <xref:System.Data.SqlClient.SqlCommand> メソッドを使用して <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> が実行されます。  
  
```csharp  
// Assumes connection is an open SqlConnection object.  
using (connection)  
{  
  // Create a DataTable with the modified rows.  
  DataTable addedCategories = CategoriesDataTable.GetChanges(DataRowState.Added);  

  // Configure the SqlCommand and SqlParameter.  
  SqlCommand insertCommand = new SqlCommand("usp_InsertCategories", connection);  
  insertCommand.CommandType = CommandType.StoredProcedure;  
  SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
  tvpParam.SqlDbType = SqlDbType.Structured;  

  // Execute the command.  
  insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection object.  
Using connection  
   '  Create a DataTable with the modified rows.  
   Dim addedCategories As DataTable = _  
     CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Configure the SqlCommand and SqlParameter.  
   Dim insertCommand As New SqlCommand( _  
     "usp_InsertCategories", connection)  
   insertCommand.CommandType = CommandType.StoredProcedure  
   Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
     "@tvpNewCategories", addedCategories)  
   tvpParam.SqlDbType = SqlDbType.Structured  
  
   '  Execute the command.  
   insertCommand.ExecuteNonQuery()  
End Using  
```  
  
### <a name="passing-a-table-valued-parameter-to-a-parameterized-sql-statement"></a>パラメーター化 SQL ステートメントへのテーブル値パラメーターの受け渡し  
 次の例は、データ ソースとしてテーブル値パラメーターを持つ SELECT サブクエリ付きの INSERT ステートメントを使用して、dbo.Categories テーブルにデータを挿入する方法を示しています。 テーブル値パラメーターをパラメーター化 SQL ステートメントに渡すときは、<xref:System.Data.SqlClient.SqlParameter.TypeName%2A> の新しい <xref:System.Data.SqlClient.SqlParameter> プロパティを使用して、テーブル値パラメーターの型名を指定する必要があります。 この `TypeName` は、既にサーバー上に作成されている、互換性のある型の名前と一致している必要があります。 このコード例では、dbo.CategoryTableType で定義されている型の構造を参照するために `TypeName` プロパティが使用されています。  
  
> [!NOTE]
> テーブル値パラメーターで ID 列の値を指定する場合は、そのセッションの SET IDENTITY_INSERT ステートメントを実行する必要があります。  
  
```csharp  
// Assumes connection is an open SqlConnection.  
using (connection)  
{  
  // Create a DataTable with the modified rows.  
  DataTable addedCategories = CategoriesDataTable.GetChanges(DataRowState.Added);  

  // Define the INSERT-SELECT statement.  
  string sqlInsert =   
      "INSERT INTO dbo.Categories (CategoryID, CategoryName)"  
      + " SELECT nc.CategoryID, nc.CategoryName"  
      + " FROM @tvpNewCategories AS nc;"  

  // Configure the command and parameter.  
  SqlCommand insertCommand = new SqlCommand(sqlInsert, connection);  
  SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
  tvpParam.SqlDbType = SqlDbType.Structured;  
  tvpParam.TypeName = "dbo.CategoryTableType";  

  // Execute the command.  
  insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
Using connection  
  ' Create a DataTable with the modified rows.  
  Dim addedCategories As DataTable = _  
    CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Define the INSERT-SELECT statement.  
  Dim sqlInsert As String = _  
  "INSERT INTO dbo.Categories (CategoryID, CategoryName)" _  
  & " SELECT nc.CategoryID, nc.CategoryName" _  
  & " FROM @tvpNewCategories AS nc;"  
  
  ' Configure the command and parameter.  
  Dim insertCommand As New SqlCommand(sqlInsert, connection)  
  Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
    "@tvpNewCategories", addedCategories)  
  tvpParam.SqlDbType = SqlDbType.Structured  
  tvpParam.TypeName = "dbo.CategoryTableType"  
  
  ' Execute the query  
  insertCommand.ExecuteNonQuery()  
End Using  
```  
  
## <a name="streaming-rows-with-a-datareader"></a>DataReader による行のストリーミング  
 <xref:System.Data.Common.DbDataReader> から派生した任意のオブジェクトを使用して、一連の行データをテーブル値パラメーターに挿入することもできます。 次のコード フラグメントは、<xref:System.Data.OracleClient.OracleCommand> と <xref:System.Data.OracleClient.OracleDataReader> を使用して Oracle データベースからデータを取り出す方法を示しています。 このコードは次に、単一の入力パラメーターを持つストアド プロシージャを呼び出すように <xref:System.Data.SqlClient.SqlCommand> を構成します。 <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> の <xref:System.Data.SqlClient.SqlParameter> プロパティが `Structured` に設定されています。 <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A> は `OracleDataReader` の結果セットをテーブル値パラメーターとしてストアド プロシージャに渡します。  
  
```csharp  
// Assumes connection is an open SqlConnection.  
// Retrieve data from Oracle.  
OracleCommand selectCommand = new OracleCommand(  
   "Select CategoryID, CategoryName FROM Categories;",  
   oracleConnection);  
OracleDataReader oracleReader = selectCommand.ExecuteReader(  
   CommandBehavior.CloseConnection);  
  
 // Configure the SqlCommand and table-valued parameter.  
 SqlCommand insertCommand = new SqlCommand(  
   "usp_InsertCategories", connection);  
 insertCommand.CommandType = CommandType.StoredProcedure;  
 SqlParameter tvpParam =  
    insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", oracleReader);  
 tvpParam.SqlDbType = SqlDbType.Structured;  
  
 // Execute the command.  
 insertCommand.ExecuteNonQuery();  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
' Retrieve data from Oracle.  
Dim selectCommand As New OracleCommand( _  
  "Select CategoryID, CategoryName FROM Categories;", _  
  oracleConnection)  
Dim oracleReader As OracleDataReader = _  
  selectCommand.ExecuteReader(CommandBehavior.CloseConnection)  
  
' Configure SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  oracleReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
' Execute the command.  
insertCommand.ExecuteNonQuery()  
```  
  
## <a name="see-also"></a>参照

- [パラメーターおよびパラメーター データ型の構成](../configuring-parameters-and-parameter-data-types.md)
- [コマンドおよびパラメーター](../commands-and-parameters.md)
- [DataAdapter パラメーター](../dataadapter-parameters.md)
- [ADO.NET における SQL Server データ操作](sql-server-data-operations.md)
- [ADO.NET の概要](../ado-net-overview.md)
