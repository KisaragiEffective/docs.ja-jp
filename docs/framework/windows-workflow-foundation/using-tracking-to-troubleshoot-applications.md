---
title: 追跡を使用したアプリケーションのトラブルシューティング
ms.date: 03/30/2017
ms.assetid: 8851adde-c3c2-4391-9523-d8eb831490af
ms.openlocfilehash: b07e850810734082568ddca9776a72575c986094
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837559"
---
# <a name="using-tracking-to-troubleshoot-applications"></a>追跡を使用したアプリケーションのトラブルシューティング
Windows Workflow Foundation (WF) を使用すると、ワークフロー関連の情報を追跡して、Windows Workflow Foundation アプリケーションまたはサービスの実行に詳細を提供できます。 Windows Workflow Foundation ホストは、ワークフローインスタンスの実行中にワークフローイベントをキャプチャできます。 ワークフローによってエラーまたは例外が生成された場合は、Windows Workflow Foundation 追跡の詳細を使用して、処理のトラブルシューティングを行うことができます。  
  
## <a name="troubleshooting-a-wf-using-wf-tracking"></a>WF の追跡を使用した WF のトラブルシューティング  
 Windows Workflow Foundation アクティビティの処理中に発生したエラーを検出するには、状態が Faulted である <xref:System.Activities.Tracking.ActivityStateRecord> を照会する追跡プロファイルを使用して追跡を有効にすることができます。 対応するクエリは、次のコードで指定されています。  
  
```xml  
<activityStateQueries>  
              <activityStateQuery activityName="*">  
                <states>  
                  <state name="Faulted" />  
                </states>  
              </activityStateQuery>  
 </activityStateQueries>  
```  
  
 エラーがエラー ハンドラー (<xref:System.Activities.Statements.TryCatch> アクティビティなど) 内で反映および処理される場合、<xref:System.Activities.Tracking.FaultPropagationRecord> を使用して検出できます。 <xref:System.Activities.Tracking.FaultPropagationRecord> は、エラーのソース アクティビティとエラー ハンドラーの名前を示します。 <xref:System.Activities.Tracking.FaultPropagationRecord> には、エラーの例外スタックの形式でエラーの詳細が含まれています。 次の例では、<xref:System.Activities.Tracking.FaultPropagationRecord> をサブスクライブするためのクエリを示しています。  
  
```xml  
<faultPropagationQueries>  
              <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>  
 </faultPropagationQueries>  
```  
  
 エラーがワークフロー内で処理されない場合は、ワークフロー インスタンスでハンドルされない例外になり、ワークフロー インスタンスは中止されます。 ハンドルされない例外の詳細を把握するために、追跡プロファイルでは、次のように `state name="UnhandledException"` を持つワークフロー インスタンス レコードを照会する必要があります。  
  
```xml  
<workflowInstanceQueries>  
              <workflowInstanceQuery>  
                <states>  
                  <state name="UnhandledException" />  
                </states>  
              </workflowInstanceQuery>  
</workflowInstanceQueries>  
```  
  
 ワークフローインスタンスでハンドルされない例外が発生すると、Windows Workflow Foundation の追跡が有効になっている場合、<xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord> オブジェクトが生成されます。  
  
 この追跡レコードには、例外スタックの形式でエラーの詳細が含まれます。 これには、エラーの原因となったエラー (アクティビティなど) の詳細が含まれており、未処理の例外が発生しています。Windows Workflow Foundation からのエラーイベントをサブスクライブするには、追跡参加要素を追加して追跡を有効にします。 この参加要素は、`ActivityStateQuery (state="Faulted")`、<xref:System.Activities.Tracking.FaultPropagationRecord>、および `WorkflowInstanceQuery (state="UnhandledException")` を照会する追跡プロファイルで構成します。  
  
 ETW 追跡参加要素を使用して追跡を有効にした場合、エラー イベントは ETW セッションに書き出されます。 イベントはイベント ビューアーを使用して表示できます。 ノードの下で確認できます **イベント ビューアーは アプリケーションとサービス ログ Microsoft->-> Windows アプリケーション サーバー-アプリケーション->** 分析チャネルにします。  
  
## <a name="see-also"></a>参照

- [Windows Server App Fabric の監視](https://docs.microsoft.com/previous-versions/appfabric/ee677251(v=azure.10))
- [App Fabric を使用したアプリケーションの監視](https://docs.microsoft.com/previous-versions/appfabric/ee677276(v=azure.10))
