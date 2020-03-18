---
title: SQL Workflow Instance Store
ms.date: 03/30/2017
ms.assetid: 8cd2f8a5-4bf8-46ea-8909-c7fdb314fabc
ms.openlocfilehash: 1764017369e82cfbed38be06b4a36847576b5fc0
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837637"
---
# <a name="sql-workflow-instance-store"></a>SQL Workflow Instance Store
[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] には SQL Workflow Instance Store が含まれます。これを使用すると、ワークフロー インスタンスに関する状態情報を SQL Server 2005 または SQL Server 2008 のデータベースに永続化できます。 この機能は主に <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> クラスの形式で実装されます。このクラスは永続化フレームワークの <xref:System.Runtime.DurableInstancing.InstanceStore> 抽象クラスから派生します。 SQL Workflow Instance Store 機能によって SQL 永続性プロバイダーを構成します。このプロバイダーは、ホストが永続化コマンドをストアに送信するときに使用する永続化 API の具象実装です。  
  
 SQL Workflow Instance Store は、セルフホストされているワークフローや、<xref:System.Activities.WorkflowApplication> または <xref:System.ServiceModel.WorkflowServiceHost> を使用するワークフロー サービスだけでなく、<xref:System.ServiceModel.WorkflowServiceHost> を使用して WAS でホストされるサービスをサポートします。 セルフホストされているサービスの SQL Workflow Instance Store 機能をプログラムで構成するには、この機能が公開しているオブジェクト モデルを使用します。 プログラムでオブジェクト モデルや XML 構成ファイルを使用することにより、<xref:System.ServiceModel.WorkflowServiceHost> でホストされるサービスについてこの機能を構成できます。  
  
 SQL Workflow Instance Store 機能 (**SqlWorkflowInstanceStore**クラス) には <xref:System.ServiceModel.Persistence.PersistenceProviderFactory> が実装されていないため、永続性のないワークフロー以外の WCF サービスの永続化サポートは提供されません。 また、<xref:System.Workflow.Runtime.Hosting.WorkflowPersistenceService> を実装していないため、3.x ワークフローの永続化は提供しません。 この機能は、WF 4.0 以降のワークフローとワークフロー サービスについてのみ永続化をサポートします。 また、SQL Server 2005 と SQL Server 2008 以外のデータベースはサポートしません。  
  
 このセクションのトピックでは、SQL Workflow Instance Store のプロパティと機能、およびストアの構成方法の詳細を説明します。  
  
 Windows Server App Fabric には、インスタンス ストアを簡単に構成および使用できる独自のインスタンス ストアとツールが用意されています。 詳細については、「 [Windows Server App Fabric インスタンスストア](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))」を参照してください。 App Fabric SQL Server 永続性データベースの詳細については、「 [App fabric SQL Server 永続性データベース](https://docs.microsoft.com/previous-versions/appfabric/ee790819(v=azure.10))」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
- [SQL Workflow Instance Store のプロパティ](properties-of-sql-workflow-instance-store.md)  
  
- [ワークフローとワークフロー サービスの SQL 永続性を有効にする方法](how-to-enable-sql-persistence-for-workflows-and-workflow-services.md)  
  
- [インスタンスのアクティブ化処理](instance-activation.md)  
  
- [クエリのサポート](support-for-queries.md)  
  
- [ストア拡張](store-extensibility.md)  
  
- [Security](security.md)  
  
- [SQL Server 永続性データベース](sql-server-persistence-database.md)  
  
## <a name="see-also"></a>参照

- [永続化のサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd699769(v=vs.100))
