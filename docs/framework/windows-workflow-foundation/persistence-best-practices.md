---
title: 永続化のベスト プラクティス
ms.date: 03/30/2017
ms.assetid: 6974c5a4-1af8-4732-ab53-7d694608a3a0
ms.openlocfilehash: 8ffbb3ebfa8f85e2b0052a9df9ada30766accd8e
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802519"
---
# <a name="persistence-best-practices"></a>永続化のベスト プラクティス
このドキュメントでは、ワークフローの永続化に関するワークフローのデザインと構成のベスト プラクティスについて説明します。  
  
## <a name="design-and-implementation-of-durable-workflows"></a>永続的ワークフローのデザインと実装  
 一般的に、ワークフローは短く分割された期間に作業を行い、その合間にイベント待機中のアイドル時間がインタリーブされています。 このイベントには、メッセージや期限付きタイマーなどがあります。 ワークフロー インスタンスがアイドルになったときにこのインスタンスをアンロードするには、サービス ホストがワークフロー インスタンスを永続化する必要があります。 これは、ワークフロー インスタンスが非永続化ゾーン (たとえばトランザクションの完了を待機していたり、非同期コールバックを待機している場合) にない場合にのみ可能です。 アイドル状態のワークフロー インスタンスのアンロードを可能にするために、ワークフローの作成者は短時間のアクションに対してのみトランザクション スコープと非同期アクティビティを使用するようにしてください。 特に、これらの非永続化ゾーン内での遅延アクティビティはできるだけ短くしてください。  
  
 ワークフローは、ワークフローが使用するすべてのデータ型がシリアル化可能である場合にのみ永続化できます。 また、永続化されたワークフローで使用されるカスタム型は、<xref:System.Runtime.Serialization.NetDataContractSerializer> を使用してシリアル化可能である必要があります。そうしないと、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を使用してワークフローを永続化することはできません。  
  
 永続化されていないワークフロー インスタンスは、ホストまたはコンピューターに障害が発生した場合、復元できません。 通常は、ワークフローのライフ サイクルの早い時期にワークフロー インスタンスを永続化することをお勧めします。  
  
 ワークフローが長時間ビジー状態になっている場合は、ビジー期間中、定期的にワークフロー インスタンスを永続化することをお勧めします。 永続化するには、ワークフロー インスタンスをビジー状態にする一連のアクティビティに <xref:System.Activities.Statements.Persist> アクティビティを追加します。 こうすると、アプリケーション ドメインのリサイクル、ホストの障害、またはコンピューターの障害が原因でシステムがビジー期間の始めにロールバックするのを避けることができます。 <xref:System.Activities.Statements.Persist> アクティビティをワークフローに追加すると、パフォーマンスが低下する場合があることに注意してください。  
  
 Windows Server App Fabric を使用すると、永続化の構成と使用を大幅に簡潔化できます。 詳細については、「 [Windows Server App Fabric の永続](https://docs.microsoft.com/previous-versions/appfabric/ee677272(v=azure.10))化」を参照してください。  
  
## <a name="configuration-of-scalability-parameters"></a>スケーラビリティ パラメーターの構成  
 スケーラビリティとパフォーマンスの要件により、次のパラメーターの設定が決定されます。  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A>  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A>  
  
- <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior.InstanceLockedExceptionAction%2A>  
  
 これらのパラメーターは、現在のシナリオに応じて次のように設定してください。  
  
### <a name="scenario-a-small-number-of-workflow-instances-that-require-optimal-response-time"></a>シナリオ: 最適な応答時間が要求される少数のワークフロー インスタンスが存在する場合  
 このシナリオでは、ワークフロー インスタンスがアイドル状態になったときに、これらのインスタンスすべてを読み込んだ状態のまま保持する必要があります。 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> を大きな値に設定します。 この設定を使用すると、ワークフロー インスタンスがコンピューター間で移動するのを回避できます。 この設定は、次の 1 つ以上の条件が真である場合にのみ使用してください。  
  
- ワークフロー インスタンスが有効期間中に 1 つのメッセージを受信する。  
  
- すべてのワークフロー インスタンスが 1 台のコンピューター上で実行される。  
  
- ワークフロー インスタンスがすべてのメッセージを同じコンピューターで受信する。  
  
 <xref:System.Activities.Statements.Persist> アクティビティを使用するか、<xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A> を 0 に設定し、サービス ホストまたはコンピューターに障害が発生した場合にワークフロー インスタンスを回復できるようにします。  
  
### <a name="scenario-workflow-instances-are-idle-for-long-periods-of-time"></a>シナリオ: ワークフロー インタンスが長期間アイドル状態になる場合  
 このシナリオでは、<xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> を 0 に設定してリソースをできるだけ早く解放します。  
  
### <a name="scenario-workflow-instances-receive-multiple-messages-in-a-short-period-of-time"></a>シナリオ: ワークフロー インスタンスが短期間に複数のメッセージを受信する場合  
 このシナリオでは、同じコンピューターでこれらのメッセージを受信する場合、<xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> を 60 秒に設定します。 これにより、ワークフロー インスタンスのアンロードと読み込みが頻繁に繰り返される状態を回避できます。 また、インスタンスがメモリ内に長く留まることを避けることもできます。  
  
 これらのメッセージを異なるコンピューターで受信する場合は、<xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> を 0 に設定し、<xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior.InstanceLockedExceptionAction%2A> を BasicRetry または AggressiveRetry に設定します。 これにより、別のコンピューターがワークフロー インスタンスを読み込むことができるようになります。  
  
### <a name="scenario-workflow-uses-delay-activities-with-short-durations"></a>シナリオ: ワークフローが短期間の遅延アクティビティを使用する場合  
 このシナリオでは、読み込む必要のあるインスタンスに対して <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> が定期的に永続性データベースをポーリングします。その理由は、<xref:System.Activities.Statements.Delay> アクティビティが期限切れになるためです。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> が次のポーリング間隔で期限切れになるタイマーを見つけた場合は、SQL Workflow Instance Store によってポーリング間隔が短縮されます。 次のポーリングは、タイマーが期限切れになった直後に発生します。 このようにして、SQL Workflow Instance Store はポーリング間隔よりも長く実行されるタイマーの精度を保持します。タイマーは <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> によって設定されます。 より短い遅延で迅速な処理を実行するために、ワークフロー インスタンスは少なくとも 1 つのポーリング間隔中、メモリに留まる必要があります。  
  
 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A> を 0 に設定し、有効期限を永続性データベースに書き込みます。  
  
 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> を <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 以上に設定し、少なくとも 1 つのポーリング間隔中、インスタンスをメモリに保持します。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> の値を小さくすることはお勧めできません。それによって永続性データベースへの読み込みが増加するためです。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を使用する各サービス ホストは、検出期間ごとにデータベースを 1 回ポーリングします。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> に設定した間隔が小さすぎると、サービス ホストの数が大きい場合にシステムのパフォーマンスが低下することがあります。  
  
## <a name="configuring-the-sql-workflow-instance-store"></a>SQL Workflow Instance Store の構成  
 SQL Workflow Instance Store には、次の構成パラメーターがあります。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceEncodingOption%2A>  
 このパラメーターは、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> に対してワークフロー インスタンス状態を圧縮するように指示します。 圧縮により、永続性データベースに保存されるデータの量が削減され、永続性データベースが専用のデータベース サーバーに常駐している場合のネットワーク トラフィックが緩和されます。 圧縮を使用する場合は、インスタンス状態を圧縮および抽出するための計算リソースが必要になります。 ほとんどの場合は圧縮によってパフォーマンスが向上します。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceCompletionAction%2A>  
 このパラメーターは、完了したインスタンスを保持または削除するように <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> に指示します。 完了したインスタンスを保持しておくと、永続性データベースにより多くのストレージが必要となり、テーブルのサイズが増加するため、テーブルの検索時間が長くなります。 完了したインスタンスがデバックまたは監査の目的で必要とならない限り、完了したインスタンスは削除するように <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> に指示するのが最適です。 削除したインスタンスは、ユーザーが後でこれらを取り除くプロセスを確立する場合にのみ保持してください。 完了したワークフロー インスタンスがインスタンス ストアに残っている限り、相関関係キーを再利用することはできないことに注意してください。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A>  
 このパラメーターは、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> アクティビティが期限切れになったときに読み込む必要のあるインスタンスに対して <xref:System.Activities.Statements.Delay> が永続性データベースをポーリングする最大間隔を定義します。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> が次のポーリング間隔で期限切れになるタイマーを見つけた場合は、ポーリング間隔を短縮し、タイマーの期限切れ直後に次のポーリングが発生するようにします。 このようにして、SQL Workflow Instance Store は <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> よりも長く実行されるタイマーの精度を保持します。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> の値を小さくすることはお勧めできません。それによって永続性データベースへの読み込みが増加するためです。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を使用する各サービス ホストは、検出期間ごとにデータベースを 1 回ポーリングします。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> に設定した間隔が小さすぎると、サービス ホストの数が大きい場合にシステムのパフォーマンスが低下することがあります。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.HostLockRenewalPeriod%2A>  
 このパラメーターは、ホストが永続性データベース内のロックを更新する間隔を定義します。 この間隔を短縮すると、ホストまたはコンピューターに障害が発生した場合にワークフロー インスタンスをよりすばやく回復できるようになります。 一方、ロックの更新間隔を短くすると、永続性データベースへの読み込みが増えます。 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を使用する各サービス ホストは、1 つの更新間隔ごとにデータベース内のロックを 1 回更新します。 コンピューターが多くのサービス ホストを実行する場合は、ロックの更新によって発生する読み込みがシステム パフォーマンスの低下をもたらさないようにしてください。 パフォーマンスが低下する場合は、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.HostLockRenewalPeriod%2A> を大きくしてください。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A>  
 有効にした場合、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> はロック済みインスタンスの読み込みを次の 30 秒間再試行します。 ワークフローが短期間に複数のメッセージを受信し、これらのメッセージを異なるコンピューターで受信する場合は、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A> を BasicRetry または AggressiveRetry に設定してください。  
  
 読み込みが再試行されない限り、再試行メカニズムによってパフォーマンスのオーバーヘッドが発生することはないので、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A> は常に有効にしてください。
