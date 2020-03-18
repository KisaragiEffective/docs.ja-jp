---
title: SQL Server データ型と ADO.NET
titleSuffix: ''
ms.date: 03/30/2017
ms.assetid: 81b43550-23e8-43bb-b460-7eb8ac825c33
ms.openlocfilehash: f727c69b1dd5c23c6a89911005256de70255fd4c
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452332"
---
# <a name="sql-server-data-types-and-adonet"></a>SQL Server データ型と ADO.NET
SQL Server と .NET Framework は異なる型システムに基づいているので、両者間でデータ損失が発生する可能性があります。 データの整合性を維持するために、.NET Framework Data Provider for SQL Server (<xref:System.Data.SqlClient>) では、SQL Server データを処理するための型指定されたアクセサー メソッドが提供されています。 <xref:System.Data.SqlDbType> クラスの列挙値を使用して、<xref:System.Data.SqlClient.SqlParameter> データ型を指定できます。  
  
 SQL Server と .NET Framework のデータ型の間のデータ型マッピングについては、「[データ型のマッピングの SQL Server](../sql-server-data-type-mappings.md)」を参照してください。  
  
 SQL Server 2008 では、業務上のニーズに対応して、日時データ、構造化データ、半構造化データ、および非構造化データを扱うための新しいデータ型が導入されました。 これらは SQL Server 2008 オンライン ブックで説明されています。  
  
 アプリケーションで使用可能な SQL Server のデータ型は、使用する SQL Server のバージョンによって異なります。 詳細については、次の表にある各バージョンの SQL Server オンライン ブックを参照してください。  
  
 **SQL Server のドキュメント**  
  
1. [データ型 (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql)  
  
## <a name="in-this-section"></a>このセクションの内容  
 [SqlTypes とデータセット](sqltypes-and-the-dataset.md)  
 `SqlTypes` 内の `DataSet` に対する新しい型のサポートについて説明します。  
  
 [null 値の処理](handling-null-values.md)  
 null 値と 3 値ロジックの使用例を示します。  
  
 [GUID と uniqueidentifier 値の比較](comparing-guid-and-uniqueidentifier-values.md)  
 SQL Server と .NET Framework での GUID および uniqueidentifier 値の使用例を示します。  
  
 [日付と時刻のデータ](date-and-time-data.md)  
 SQL Server 2008 で導入された新しい日付と時刻のデータ型の使用方法について説明します。  
  
 [大きな UDT](large-udts.md)  
 SQL Server 2008 で導入された大きな値の UDT からデータを取り出す方法の例を示します。  
  
 [SQL Server における XML データ](xml-data-in-sql-server.md)  
 SQL Server から取得した XML データを使用する方法について説明します。  
  
## <a name="reference"></a>参照  
 <xref:System.Data.DataSet>  
 `DataSet` クラスおよびそのすべてのメンバーについて説明します。  
  
 <xref:System.Data.SqlTypes>  
 `SqlTypes` 名前空間およびそのすべてのメンバーについて説明します。  
  
 <xref:System.Data.SqlDbType>  
 `SqlDbType` 列挙型およびそのすべてのメンバーについて説明します。  
  
 <xref:System.Data.DbType>  
 `DbType` 列挙型およびそのすべてのメンバーについて説明します。  
  
## <a name="see-also"></a>参照

- [SQL Server データ型のマッピング](../sql-server-data-type-mappings.md)
- [パラメーターおよびパラメーター データ型の構成](../configuring-parameters-and-parameter-data-types.md)
- [テーブル値パラメーター](table-valued-parameters.md)
- [SQL Server のバイナリ データと大きな値のデータ](sql-server-binary-and-large-value-data.md)
- [ADO.NET の概要](../ado-net-overview.md)
