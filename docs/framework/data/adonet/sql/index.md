---
title: SQL Server と ADO.NET
titleSuffix: ''
ms.date: 03/30/2017
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.openlocfilehash: 6e88c35936de72f0d426c23493bbe5a08e707ee1
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76979977"
---
# <a name="sql-server-and-adonet"></a>SQL Server と ADO.NET
このセクションでは、.NET Framework Data Provider for SQL Server (<xref:System.Data.SqlClient>) 固有の機能および動作について説明します。  
  
 <xref:System.Data.SqlClient> は SQL Server のバージョンへのアクセスを提供し、データベース固有のプロトコルをカプセル化します。 このデータ プロバイダーの機能は、OLE DB、ODBC、および Oracle に対する .NET Framework データ プロバイダーの機能と同等になるように設計されています。 <xref:System.Data.SqlClient> には、SQL Server と直接通信するための表形式データ ストリーム (TDS) パーサーが含まれています。  
  
> [!NOTE]
> .NET Framework Data Provider for SQL Server を使用するには、アプリケーションで <xref:System.Data.SqlClient> 名前空間を参照する必要があります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [SQL Server のセキュリティ](sql-server-security.md)  
 SQL Server のセキュリティの概要を説明し、アプリケーションのシナリオを交えながら、SQL Server を対象とした安全な ADO.NET アプリケーションを作成するための指針を提供します。  
  
 [SQL Server データ型と ADO.NET](sql-server-data-types.md)  
 SQL Server のデータ型の操作方法、および .NET Framework データ型との相互作用について説明します。  
  
 [SQL Server のバイナリ データと大きな値のデータ](sql-server-binary-and-large-value-data.md)  
 SQL Server で大きな値のデータを扱う方法について説明します。  
  
 [ADO.NET における SQL Server データ操作](sql-server-data-operations.md)  
 SQL Server でのデータの操作方法について説明します。 一括コピー操作、MARS、非同期操作、およびテーブル値パラメーターに関するセクションがあります。  
  
 [SQL Server の機能と ADO.NET](sql-server-features-and-adonet.md)  
 ADO.NET アプリケーション開発者にとって役立つ SQL Server の機能について説明します。  
  
 [LINQ to SQL](./linq/index.md)  
 LINQ to SQL アプリケーションを作成するのに必要な基本的なビルド ブロック、プロセス、および手法について説明します。  
  
 SQL Server データベース エンジンの詳細なドキュメントについては、使用している SQL Server のバージョンに対応する SQL Server オンライン ブックを参照してください。  
  
 [SQL Server オンライン ブック](/sql/sql-server/sql-server-technical-documentation)  
  
## <a name="see-also"></a>関連項目

- [ADO.NET アプリケーションのセキュリティ保護](../securing-ado-net-applications.md)
- [ADO.NET でのデータ型のマッピング](../data-type-mappings-in-ado-net.md)
- [DataSet、DataTable、および DataView](../dataset-datatable-dataview/index.md)
- [ADO.NET でのデータの取得および変更](../retrieving-and-modifying-data.md)
- [ADO.NET の概要](../ado-net-overview.md)
