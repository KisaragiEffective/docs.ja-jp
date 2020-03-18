---
title: コード例
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c119657a-9ce6-4940-91e4-ac1d5f0d9584
ms.openlocfilehash: 4f0cbc06c03c0d122fc69b8a396570919ac14970
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980289"
---
# <a name="adonet-code-examples"></a>ADO.NET のコード例
このトピックにリストされたコードは、次の ADO.NET テクノロジを使用してデータベースからデータを取得する方法を示しています。

- ADO.NET データ プロバイダー:

  - [SqlClient](#sqlclient) (`System.Data.SqlClient`)

  - [OleDb](#oledb) (`System.Data.OleDb`)

  - [Odbc](#odbc) (`System.Data.Odbc`)

  - [System.data.oracleclient](#oracleclient) (`System.Data.OracleClient`)

- ADO.NET Entity Framework:

  - [LINQ to Entities](#linq-to-entities)

  - [型指定された ObjectQuery](#typed-objectquery)

  - [EntityClient](#entityclient) (`System.Data.EntityClient`)

- [LINQ to SQL](#linq-to-sql)

## <a name="adonet-data-provider-examples"></a>ADO.NET data provider の例
以下に示した各コードは、ADO.NET データ プロバイダーを使用してデータベースからデータを取得する方法を示しています。 データは `DataReader` で返されます。 詳細については、「 [DataReader を使用したデータの取得](retrieving-data-using-a-datareader.md)」を参照してください。

### <a name="sqlclient"></a>SqlClient
この例のコードでは、Microsoft SQL Server 上の `Northwind` サンプルデータベースに接続できることを前提としています。 このコードは <xref:System.Data.SqlClient.SqlCommand> を作成してProducts テーブルから行を選択し、<xref:System.Data.SqlClient.SqlParameter> を追加して、結果を指定したパラメーター値 (この場合は 5) よりも大きな UnitPrice を持つ行に制限します。 <xref:System.Data.SqlClient.SqlConnection> は `using` ブロック内で開かれます。これにより、コードが終了したときにリソースが閉じられ、破棄されます。 コードは <xref:System.Data.SqlClient.SqlDataReader> を使用してコマンドを実行し、コンソール ウィンドウに結果を表示します。

 [!code-csharp[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/VB/source.vb#1)]

### <a name="oledb"></a>OleDb
このコード例は、Microsoft Access の Northwind サンプル データベースに接続できることを前提としています。 このコードは <xref:System.Data.OleDb.OleDbCommand> を作成してProducts テーブルから行を選択し、<xref:System.Data.OleDb.OleDbParameter> を追加して、結果を指定したパラメーター値 (この場合は 5) よりも大きな UnitPrice を持つ行に制限します。 <xref:System.Data.OleDb.OleDbConnection> は `using` ブロック内に開かれ、これによってコードが終了したときにリソースが閉じられ破棄されます。 コードは <xref:System.Data.OleDb.OleDbDataReader> を使用してコマンドを実行し、コンソール ウィンドウに結果を表示します。

 [!code-csharp[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/VB/source.vb#1)]

### <a name="odbc"></a>Odbc
このコード例は、Microsoft Access の Northwind サンプル データベースに接続できることを前提としています。 このコードは <xref:System.Data.Odbc.OdbcCommand> を作成してProducts テーブルから行を選択し、<xref:System.Data.Odbc.OdbcParameter> を追加して、結果を指定したパラメーター値 (この場合は 5) よりも大きな UnitPrice を持つ行に制限します。 <xref:System.Data.Odbc.OdbcConnection> は `using` ブロック内で開かれます。これにより、コードが終了したときにリソースが閉じられ、破棄されます。 コードは <xref:System.Data.Odbc.OdbcDataReader> を使用してコマンドを実行し、コンソール ウィンドウに結果を表示します。

[!code-csharp[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/CS/source.cs#1)] 
[!code-vb[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/VB/source.vb#1)] 

### <a name="oracleclient"></a>OracleClient
このコード例では、Oracle サーバー上の DEMO.CUSTOMER との接続を前提としています。 また、System.Data.OracleClient.dll への参照を追加する必要があります。 このコードは、データを <xref:System.Data.OracleClient.OracleDataReader> で返します。

 [!code-csharp[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/VB/source.vb#1)]

## <a name="entity-framework-examples"></a>Entity Framework の例
以下に示した各コードは、エンティティ データ モデル (EDM) のエンティティを照会して、データ ソースからデータを取得する方法を示しています。 これらの例では、Northwind サンプルデータベースに基づくモデルを使用します。 Entity Framework の詳細については、「 [Entity Framework の概要](./ef/overview.md)」を参照してください。

### <a name="linq-to-entities"></a>LINQ to Entities
この例のコードは LINQ クエリを使用してデータをカテゴリ オブジェクトとして返します。これは、CategoryID および CategoryName プロパティのみを含んでいる匿名型として射影されます。 詳細については、「 [LINQ to Entities の概要](./ef/language-reference/linq-to-entities.md)」を参照してください。

```csharp
using System;
using System.Linq;
using System.Data.Objects;
using NorthwindModel;

class LinqSample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            try
            {
                var query = from category in context.Categories
                            select new
                            {
                                categoryID = category.CategoryID,
                                categoryName = category.CategoryName
                            };

                foreach (var categoryInfo in query)
                {
                    Console.WriteLine("\t{0}\t{1}",
                        categoryInfo.categoryID, categoryInfo.categoryName);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Linq
Imports System.Data.Objects
Imports NorthwindModel

Class LinqSample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Try
                Dim query = From category In context.Categories _
                            Select New With _
                            { _
                                .categoryID = category.CategoryID, _
                                .categoryName = category.CategoryName _
                            }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                        categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using
    End Sub
End Class
```

### <a name="typed-objectquery"></a>型指定された ObjectQuery
この例のコードは <xref:System.Data.Objects.ObjectQuery%601> を使用し、カテゴリ オブジェクトとしてデータを返します。 詳細については、「[オブジェクトクエリ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896241(v=vs.100))」を参照してください。

```csharp
using System;
using System.Data.Objects;
using NorthwindModel;

class ObjectQuerySample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            ObjectQuery<Categories> categoryQuery = context.Categories;

            foreach (Categories category in 
                categoryQuery.Execute(MergeOption.AppendOnly))
            {
                Console.WriteLine("\t{0}\t{1}",
                    category.CategoryID, category.CategoryName);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data.Objects
Imports NorthwindModel

Class ObjectQuerySample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Dim categoryQuery As ObjectQuery(Of Categories) = context.Categories

            For Each category As Categories In _
                categoryQuery.Execute(MergeOption.AppendOnly)
                Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                    category.CategoryID, category.CategoryName)
            Next
        End Using
    End Sub
End Class
```

### <a name="entityclient"></a>EntityClient
この例のコードは <xref:System.Data.EntityClient.EntityCommand> を使用し、Entity SQL クエリを実行します。 このクエリは、カテゴリ エンティティ型のインスタンスを示すレコードのリストを返します。 <xref:System.Data.EntityClient.EntityDataReader> を使用して、結果セットのデータ レコードにアクセスします。 詳細については、「 [Entity Framework 用の EntityClient プロバイダー](./ef/entityclient-provider-for-the-entity-framework.md)」を参照してください。

```csharp
using System;
using System.Data;
using System.Data.Common;
using System.Data.EntityClient;
using NorthwindModel;

class EntityClientSample
{
    public static void ExecuteQuery()
    {
        string queryString =
            @"SELECT c.CategoryID, c.CategoryName 
                FROM NorthwindEntities.Categories AS c";

        using (EntityConnection conn =
            new EntityConnection("name=NorthwindEntities"))
        {
            try
            {
                conn.Open();
                using (EntityCommand query = new EntityCommand(queryString, conn))
                {
                    using (DbDataReader rdr = 
                        query.ExecuteReader(CommandBehavior.SequentialAccess))
                    {
                        while (rdr.Read())
                        {
                            Console.WriteLine("\t{0}\t{1}", rdr[0], rdr[1]);
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data
Imports System.Data.Common
Imports System.Data.EntityClient
Imports NorthwindModel

Class EntityClientSample
    Public Shared Sub ExecuteQuery()
        Dim queryString As String = _
            "SELECT c.CategoryID, c.CategoryName " & _
                "FROM NorthwindEntities.Categories AS c"

        Using conn As EntityConnection = _
            New EntityConnection("name=NorthwindEntities")

            Try
                conn.Open()
                Using query As EntityCommand = _
                New EntityCommand(queryString, conn)
                    Using rdr As DbDataReader = _
                        query.ExecuteReader(CommandBehavior.SequentialAccess)
                        While rdr.Read()
                            Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                                              rdr(0), rdr(1))
                        End While
                    End Using
                End Using
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using 
    End Sub
End Class
```

## <a name="linq-to-sql"></a>LINQ to SQL
この例のコードは LINQ クエリを使用してデータをカテゴリ オブジェクトとして返します。これは、CategoryID および CategoryName プロパティのみを含んでいる匿名型として射影されます。 この例は Northwind データ コンテキストを基にしています。 詳細については、[概要](./sql/linq/getting-started.md)に関するページを参照してください。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Northwind;

    class LinqSqlSample
    {
        public static void ExecuteQuery()
        {
            using (NorthwindDataContext db = new NorthwindDataContext())
            {
                try
                {
                    var query = from category in db.Categories
                                select new
                                {
                                    categoryID = category.CategoryID,
                                    categoryName = category.CategoryName
                                };

                    foreach (var categoryInfo in query)
                    {
                        Console.WriteLine("vbTab {0} vbTab {1}",
                            categoryInfo.categoryID, categoryInfo.categoryName);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }
```

```vb
Option Explicit On
Option Strict On

Imports System.Collections.Generic
Imports System.Linq
Imports System.Text
Imports Northwind

Class LinqSqlSample
    Public Shared Sub ExecuteQuery()
        Using db As NorthwindDataContext = New NorthwindDataContext()
            Try
                Dim query = From category In db.Categories _
                                Select New With _
                                { _
                                    .categoryID = category.CategoryID, _
                                    .categoryName = category.CategoryName _
                                }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                            categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
            End Using 
    End Sub
End Class
```

## <a name="see-also"></a>関連項目

- [ADO.NET の概要](ado-net-overview.md)
- [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)
- [データ アプリケーションの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/h0y4a0f6(v=vs.120))
- [Entity Data Model のクエリ (Entity Framework タスク)](https://docs.microsoft.com/previous-versions/bb738455(v=vs.90))
- [方法: 匿名型オブジェクトを返すクエリを実行する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738512(v=vs.100))
