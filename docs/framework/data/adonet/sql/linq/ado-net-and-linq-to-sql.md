---
title: ADO.NET および LINQ to SQL
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 49ac6da0-f2e1-46fa-963e-1b6dcb63fef7
ms.openlocfilehash: 4d2376a2e32ff099497a5dbcd6cb68d8ed526884
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980003"
---
# <a name="adonet-and-linq-to-sql"></a>ADO.NET および LINQ to SQL
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、ADO.NET のテクノロジファミリの一部です。 これは、ADO.NET プロバイダーモデルによって提供されるサービスに基づいています。 そのため [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] コードを既存の ADO.NET アプリケーションと組み合わせて、現在の ADO.NET ソリューションを [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]に移行できます。 次の図は、この関係を高いレベルから見たものです。  
  
 ![LINQ to SQL と ADO.NET](./media/dlinq-3.png "DLinq_3")  
  
## <a name="connections"></a>接続  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.DataContext>を作成するときに、既存の ADO.NET 接続を指定することができます。 <xref:System.Data.Linq.DataContext> に対するすべての操作 (クエリを含む) は、この指定された接続を使用します。 接続が既に開いている場合、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、終了時にそのままにしておきます。  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#4)]
 [!code-vb[DLinqCommunicatingWithDatabase#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#4)]  
  
 次のコードに示すとおり、<xref:System.Data.Linq.DataContext.Connection%2A> プロパティを使用して、いつでも接続にアクセスしたり、任意に閉じたりすることができます。  
  
 [!code-csharp[DLinqAdoNet#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqAdoNet/cs/Program.cs#1)]
 [!code-vb[DLinqAdoNet#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqAdoNet/vb/Module1.vb#1)]  
  
## <a name="transactions"></a>トランザクション  
 既に独自のデータベース トランザクションを初期化していて、<xref:System.Data.Linq.DataContext> をトランザクションに使用する必要がある場合は、<xref:System.Data.Linq.DataContext> をトランザクションに渡すことができます。  
  
 .NET Framework でトランザクションを実行する場合は、<xref:System.Transactions.TransactionScope> オブジェクトを使用することをお勧めします。 この方法を使うと、データベースと他のメモリ常駐リソース マネージャー間で動作する分散トランザクションを作成できます。 トランザクション スコープは、わずかなリソースで開始されます。 トランザクションのスコープ内に複数の接続がある場合のみ、このトランザクションは分散トランザクションに昇格します。  
  
 [!code-csharp[DLinqAdoNet#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqAdoNet/cs/Program.cs#2)]
 [!code-vb[DLinqAdoNet#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqAdoNet/vb/Module1.vb#2)]  
  
 この方法は、すべてのデータベースに使用できるわけではありません。 たとえば、SqlClient 接続では、SQL Server 2000 サーバーに対して動作する場合、システムトランザクションを昇格させることはできません。 代わりに、トランザクション スコープが使用されているときは、完全な分散トランザクションに自動的に参加します。  
  
## <a name="direct-sql-commands"></a>直接 SQL コマンド  
 ときには、クエリを実行したり変更内容を送信したりする <xref:System.Data.Linq.DataContext> 機能に不足があり、実行する必要がある特別なタスクを完了できないこともあります。 このような場合は、<xref:System.Data.Linq.DataContext.ExecuteQuery%2A> メソッドを使用して、SQL コマンドをデータベースに発行し、クエリ結果をオブジェクトに変換することができます。  
  
 たとえば、`Customer` クラスのデータが 2 つのテーブル (customer1 および customer2) に含まれているとします。 次のクエリは `Customer` オブジェクトのシーケンスを返します。  
  
 [!code-csharp[DLinqAdoNet#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqAdoNet/cs/Program.cs#3)]
 [!code-vb[DLinqAdoNet#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqAdoNet/vb/Module1.vb#3)]  
  
 表形式の結果の列名がエンティティクラスの列のプロパティと一致する限り、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] によって、どの SQL クエリからもオブジェクトが作成されます。  
  
### <a name="parameters"></a>パラメーター  
 <xref:System.Data.Linq.DataContext.ExecuteQuery%2A> メソッドは、パラメーターを受け取ります。 次のコードでは、パラメーター化されたクエリが実行されます。  
  
 [!code-csharp[DlinqAdoNet#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqAdoNet/cs/Program.cs#4)]
 [!code-vb[DlinqAdoNet#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqAdoNet/vb/Module1.vb#4)]  
  
> [!NOTE]
> パラメーターは、`Console.WriteLine()` および `String.Format()` で使用されるものと同じ中かっこ表記でクエリ テキストに表現されます。 `String.Format()` は、指定されたクエリ文字列を受け取り、中かっこで囲まれたパラメーターを、`@p0`、`@p1` …、`@p(n)` などの、生成されたパラメーター名に置き換えます。  
  
## <a name="see-also"></a>関連項目

- [背景情報](background-information.md)
- [方法 : ADO.NET コマンドおよび DataContext 間の接続を再利用する](how-to-reuse-a-connection-between-an-ado-net-command-and-a-datacontext.md)
