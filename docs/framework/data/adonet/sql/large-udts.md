---
title: 大きな UDT
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 420ae24e-762b-4e09-b4c3-2112c470ee49
ms.openlocfilehash: 012bddc0b4c29a0b50abc3a0df5c3cd34dc4725a
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452397"
---
# <a name="large-udts"></a>大きな UDT
ユーザー定義型 (UDT) を利用することで、開発者は共通言語ランタイム (CLR) のオブジェクトを SQL Server データベースに格納することによってサーバーのスカラー型システムを拡張できます。 UDT は複数の要素を持つことができ、動作を定義できます。この点は、1 つの SQL Server システム データ型から構成される従来の別名データ型と異なります。  
  
> [!NOTE]
> 大きな UDT に対する SqlClient のサポート強化を利用するには、.NET Framework 3.5 SP1 以降をインストールする必要があります。  
  
 従来、UDT のサイズは最大 8 KB に制限されていました。 SQL Server 2008 では、<xref:Microsoft.SqlServer.Server.Format.UserDefined> 形式の UDT では、この制限が廃止されています。  
  
 ユーザー定義型の完全な説明については、使用している SQL Server のバージョンに対応する SQL Server オンライン ブックを参照してください。  
  
 **SQL Server のドキュメント**  
  
1. [CLR ユーザー定義型](/sql/relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types)  
  
## <a name="retrieving-udt-schemas-using-getschema"></a>GetSchema による UDT スキーマの取得  
 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> の <xref:System.Data.SqlClient.SqlConnection> メソッドは、データベース スキーマ情報を <xref:System.Data.DataTable> に返します。 詳細については、「 [SQL Server スキーマコレクション](../sql-server-schema-collections.md)」を参照してください。  
  
### <a name="getschematable-column-values-for-udts"></a>UDT の GetSchemaTable 列値  
 <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A> の <xref:System.Data.SqlClient.SqlDataReader> メソッドは、列メタデータを記述する <xref:System.Data.DataTable> を返します。 次の表は、SQL Server 2005 と SQL Server 2008 における、大きな UDT の列メタデータの違いを示しています。  
  
|SqlDataReader 列|SQL Server 2005|SQL Server 2008 以降|  
|--------------------------|---------------------|-------------------------------|  
|`ColumnSize`|不定|不定|  
|`NumericPrecision`|255|255|  
|`NumericScale`|255|255|  
|`DataType`|`Byte[]`|UDT インスタンス|  
|`ProviderSpecificDataType`|`SqlTypes.SqlBinary`|UDT インスタンス|  
|`ProviderType`|21 (`SqlDbType.VarBinary`)|29 (`SqlDbType.Udt`)|  
|`NonVersionedProviderType`|29 (`SqlDbType.Udt`)|29 (`SqlDbType.Udt`)|  
|`DataTypeName`|`SqlDbType.VarBinary`|3 つの部分から成る名前 (*Database.SchemaName.TypeName* として指定)|  
|`IsLong`|不定|不定|  
  
## <a name="sqldatareader-considerations"></a>SqlDataReader に関する注意点  
 SQL Server 2008 以降、<xref:System.Data.SqlClient.SqlDataReader> は大きな UDT 値を取得できるように拡張されました。 どの程度大きな UDT 値が <xref:System.Data.SqlClient.SqlDataReader> によって処理されるかは、使用される SQL Server のバージョンだけでなく、接続文字列に指定されている `Type System Version` によっても異なります。 詳細については、<xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> を参照してください。  
  
 <xref:System.Data.SqlClient.SqlDataReader> の次のメソッドは、<xref:System.Data.SqlTypes.SqlBinary> が SQL Server 2005 に設定されていると、UDT ではなく `Type System Version` を返します。  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>  
  
 次のメソッドは、`Byte[]` が SQL Server 2005 に設定されていると、UDT ではなく `Type System Version` の配列を返します。  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>  
  
 現在のバージョンの ADO.NET では、変換は実行されません。  
  
## <a name="specifying-sqlparameters"></a>SqlParameter の指定  
 大きな UDT を扱うことができるように、次の <xref:System.Data.SqlClient.SqlParameter> プロパティが拡張されています。  
  
|SqlParameter プロパティ|説明|  
|---------------------------|-----------------|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|パラメーターの値を表すオブジェクトを取得または設定します。 既定値は null です。 このプロパティは、`SqlBinary`、`Byte[]`、またはマネージド オブジェクトになります。|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|パラメーターの値を表すオブジェクトを取得または設定します。 既定値は null です。 このプロパティは、`SqlBinary`、`Byte[]`、またはマネージド オブジェクトになります。|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|解決するパラメーター値のサイズを取得または設定します。 既定値は 0 です。 このプロパティは、パラメーター値のサイズを表す整数になります。 大きな UDT の場合は UDT の実際のサイズに、不明な場合は -1 になります。|  
  
## <a name="retrieving-data-example"></a>データの取得例  
 次のコード フラグメントは、大きな UDT を取得する方法を示しています。 `connectionString` 変数は SQL Server データベースへの有効な接続を前提とし、`commandString` 変数は有効な SELECT ステートメントで主キー列が最初に記載されていることを前提とします。  
  
```csharp  
using (SqlConnection connection = new SqlConnection(   
    connectionString, commandString))  
{  
  connection.Open();  
  SqlCommand command = new SqlCommand(commandString);  
  SqlDataReader reader = command.ExecuteReader();  
  while (reader.Read())  
  {  
    // Retrieve the value of the Primary Key column.  
    int id = reader.GetInt32(0);  
  
    // Retrieve the value of the UDT.  
    LargeUDT udt = (LargeUDT)reader[1];  
  
    // You can also use GetSqlValue and GetValue.  
    // LargeUDT udt = (LargeUDT)reader.GetSqlValue(1);  
    // LargeUDT udt = (LargeUDT)reader.GetValue(1);  
  
    Console.WriteLine(  
     "ID={0} LargeUDT={1}", id, udt);  
  }  
reader.close  
}  
```  
  
```vb  
Using connection As New SqlConnection( _  
    connectionString, commandString)  
    connection.Open()  
    Dim command As New SqlCommand(commandString, connection)  
    Dim reader As SqlDataReader  
    reader = command.ExecuteReader  
  
    While reader.Read()  
      ' Retrieve the value of the Primary Key column.  
      Dim id As Int32 = reader.GetInt32(0)  
  
      ' Retrieve the value of the UDT.  
      Dim udt As LargeUDT = CType(reader(1), LargeUDT)  
  
     ' You can also use GetSqlValue and GetValue.  
     ' Dim udt As LargeUDT = CType(reader.GetSqlValue(1), LargeUDT)  
     ' Dim udt As LargeUDT = CType(reader.GetValue(1), LargeUDT)  
  
      ' Print values.  
      Console.WriteLine("ID={0} LargeUDT={1}", id, udt)  
    End While  
    reader.Close()  
End Using  
```  
  
## <a name="see-also"></a>参照

- [パラメーターおよびパラメーター データ型の構成](../configuring-parameters-and-parameter-data-types.md)
- [データベース スキーマ情報の取得](../retrieving-database-schema-information.md)
- [SQL Server データ型のマッピング](../sql-server-data-type-mappings.md)
- [SQL Server のバイナリ データと大きな値のデータ](sql-server-binary-and-large-value-data.md)
- [ADO.NET の概要](../ado-net-overview.md)
