---
title: ワークフロー永続化
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], persistence
ms.assetid: 39e69d1f-b771-4c16-9e18-696fa43b65b2
ms.openlocfilehash: c49e287c6132103d4bb85a8ae892a76f9b582274
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837533"
---
# <a name="workflow-persistence"></a>ワークフロー永続化
ワークフローの永続化は、ワークフロー インスタンスの状態を、プロセスやコンピューターの情報に依存せず永続的にキャプチャしたものです。 永続化の目的は、システム生涯時にワークフロー インスタンスの既知の回復ポイントを提供するため、アクティブに作業を実行していないワークフロー インスタンスをアンロードしてメモリを節約するため、またはワークフロー インスタンスの状態をサーバー ファーム内のあるノードから別のノードに移行するためです。  
  
 永続化によって、プロセスの迅速性、拡張性、エラー時の回復、およびメモリをより効率的に管理できる機能を実現できます。 永続化プロセスには永続化ポイントの指定や保存するデータの収集が含まれ、最終的にはデータの実際のストレージが永続化プロバイダーにデリゲートされます。  
  
 ワークフローの永続化を有効にするには、 [「方法: ワークフローとワークフローサービスの永続化を有効](how-to-enable-persistence-for-workflows-and-workflow-services.md)にする」で説明されているように、インスタンスストアを**WorkflowApplication**または**WorkflowServiceHost**に関連付ける必要があります。 **WorkflowApplication**と**WorkflowServiceHost**は、関連付けられているインスタンスストアを使用して、永続化ストアへのワークフローインスタンスの永続化と、永続化ストアに格納されているワークフローインスタンスデータに基づいたワークフローインスタンスのメモリへの読み込みを可能にします。  
  
 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] には**SqlWorkflowInstanceStore**クラスが付属しています。これにより、ワークフローインスタンスに関するデータとメタデータを SQL Server 2005 または SQL Server 2008 データベースに永続化できます。 詳細については、「 [SQL Workflow Instance Store](sql-workflow-instance-store.md) 」を参照してください。  
  
 アプリケーション固有のデータをワークフロー インスタンス関連の情報と共に格納し、読み込むために、<xref:System.Activities.Persistence.PersistenceParticipant> クラスを拡張する永続参加要素を作成できます。 永続参加要素は永続化プロセスに参加して、カスタムのシリアル化可能なデータを永続化ストアに保存し、インスタンス ストアからメモリにデータを読み込み、永続化トランザクションで任意の追加ロジックを実行します。 詳細については、「[永続化参加要素](persistence-participants.md)」を参照してください。  
  
 Windows Server App Fabric を使用すると、永続化の構成のプロセスを簡潔化できます。 詳細については、「 [Windows Server App Fabric での永続](https://docs.microsoft.com/previous-versions/appfabric/ee677272(v=azure.10))化の概念」を参照してください。  
  
## <a name="implicit-persistence-points"></a>暗黙的な永続化ポイント  
 次の一覧は、インスタンス ストアがワークフローに関連付けられている場合にワークフローが永続化される条件の例です。  
  
- **TransactionScope**アクティビティが完了するか、 **TransactedReceiveScope**アクティビティが完了したとき。  
  
- ワークフローインスタンスがアイドル状態になり、ワークフローホストで**WorkflowIdleBehavior**が設定されたとき。 これは、たとえば、メッセージングアクティビティや**Delay**アクティビティを使用する場合に発生します。  
  
- WorkflowApplication がアイドル状態になり、アプリケーションの**Persistableidle**プロパティが**PersistableIdleAction**に設定された場合。  
  
- ホスト アプリケーションに対して、ワークフロー インスタンスの永続化またはアンロードが指示されたとき。  
  
- ワークフロー インスタンスが終了したとき。  
  
- **Persist**アクティビティが実行されたとき。  
  
- 以前のバージョンの Windows Workflow Foundation を使用して開発したワークフロー インスタンスが、相互運用可能な実行中に永続化ポイントに到達したとき。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
- [SQL Workflow Instance Store](sql-workflow-instance-store.md)  
  
- [インスタンス ストア](instance-stores.md)  
  
- [永続参加要素](persistence-participants.md)  
  
- [永続化のベスト プラクティス](persistence-best-practices.md)  
  
- [非永続化ワークフロー インスタンス](non-persisted-workflow-instances.md)  
  
- [ワークフローの一時停止と再開](pausing-and-resuming-a-workflow.md)
