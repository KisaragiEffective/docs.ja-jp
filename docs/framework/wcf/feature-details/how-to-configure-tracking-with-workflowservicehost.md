---
description: '詳細については、「方法: WorkflowServiceHost を使用して追跡を構成する」を参照してください。'
title: '方法: WorkflowServiceHost を使用して追跡を構成する'
ms.date: 03/30/2017
ms.assetid: ed1485fe-7529-4351-bca3-8bb915260b17
ms.openlocfilehash: 11c48c3989ab9b788c1e6834d8cbfe53e2b8a53e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99734829"
---
# <a name="how-to-configure-tracking-with-workflowservicehost"></a>方法: WorkflowServiceHost を使用して追跡を構成する

このトピックでは、[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] でホストされている <xref:System.ServiceModel.Activities.WorkflowServiceHost> ワークフロー サービスの追跡を構成する方法について説明します。 これは、Web.config ファイルにサービスの動作を指定することによって指定します。  
  
### <a name="configure-tracking-in-configuration"></a>構成での追跡の構成  
  
1. 次の <xref:System.Activities.Tracking.EtwTrackingParticipant> `behavior` 例に示すように、構成ファイルに <> 要素を使用してを追加します。  
  
    ```xml  
    <behaviors>  
       <serviceBehaviors>  
         <behavior>  
           <etwTracking profileName="Sample Tracking Profile" />  
         </behavior>
       </serviceBehaviors>  
    </behaviors>  
    ```  
  
    > [!NOTE]
    > 前の構成サンプルでは、簡略化された構成を使用しています。 詳細については、「簡略化された [構成](../simplified-configuration.md)」を参照してください。  
  
     前の構成サンプルでは、<xref:System.Activities.Tracking.EtwTrackingParticipant> を追加し、追跡プロファイル名を指定します。 追跡プロファイルは `trackingProfile` <> 要素内の <> 要素に作成され `tracking` ます。 追跡プロファイルには、実行時にワークフロー インスタンスの状態が変化したときに生成されるワークフロー イベントを追跡参加要素が定期受信できるようにする、追跡クエリが含まれています。 追跡プロファイルを作成する方法を次の例に示します。  
  
    ```xml  
    <system.serviceModel>  
        <tracking>
         <trackingProfile name="Sample Tracking Profile">  
            <workflow activityDefinitionId="*">  
               <workflowInstanceQueries>  
                 <workflowInstanceQuery>  
                    <states>  
                       <state name="Started"/>  
                       <state name="Completed"/>  
                    </states>  
                </workflowInstanceQuery>  
             </workflowInstanceQueries>  
           </workflow>  
         </trackingProfile>
       </tracking>  
    </system.serviceModel>  
    ```  
  
     追跡プロファイルの詳細については、「 [追跡プロファイル](../../windows-workflow-foundation/tracking-profiles.md)」を参照してください。  
  
     一般的な追跡の詳細については、「 [ワークフローの追跡とトレース](../../windows-workflow-foundation/workflow-tracking-and-tracing.md)」を参照してください。  
  
### <a name="configure-tracking-in-code"></a>コードでの追跡の構成  
  
1. 次の例に示すように、コードで <xref:System.Activities.Tracking.EtwTrackingParticipant> を動作を使用して <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior> を追加します。  
  
    ```csharp  
    host.Description.Behaviors.Add(new EtwTrackingBehavior { ProfileName = "Sample Tracking Profile" });  
    ```  
  
     前のコード サンプルでは、<xref:System.Activities.Tracking.EtwTrackingParticipant> を追加し、追跡プロファイル名を指定します。 追跡プロファイルは、 `trackingProfile` `tracking` 前のセクションで示したように、<> 要素内の <> 要素に作成されます。  
  
     追跡プロファイルの詳細については、「 [追跡プロファイル](../../windows-workflow-foundation/tracking-profiles.md)」を参照してください。  
  
     一般的な追跡の詳細については、「 [ワークフローの追跡とトレース](../../windows-workflow-foundation/workflow-tracking-and-tracing.md)」を参照してください。 プログラムによって追跡を構成する例については [、「ワークフローの追跡の構成](../../windows-workflow-foundation/configuring-tracking-for-a-workflow.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [WCF サービスの簡略化された構成](../samples/simplified-configuration-for-wcf-services.md)
- [ワークフロー サービス](workflow-services.md)
- [追跡プロファイル](../../windows-workflow-foundation/tracking-profiles.md)
