---
title: アクティビティ ライブラリ
ms.date: 03/30/2017
ms.assetid: 5323e9d4-71d6-47eb-bfa6-31feac62044d
ms.openlocfilehash: ce65bf8e7adad6acefd7aac6d69c3f836b94be29
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094697"
---
# <a name="activity-library"></a>アクティビティ ライブラリ
このセクションには、Windows Workflow Foundation (WF) の高度なカスタムアクティビティを示すサンプルが含まれています。  
  
## <a name="in-this-section"></a>このセクションの内容

 [SendMail カスタム アクティビティ](sendmail-custom-activity.md)  
 <xref:System.Activities.AsyncCodeActivity> から派生するカスタム アクティビティを作成して、SMTP を使用して電子メールを送信し、ワークフロー アプリケーション内で使用する方法を示します。  
  
 [制限された並列 ForEach](throttled-parallel-foreach.md)  
 ph x="1" /&gt; アクティビティは、実行するコンカレンシー分岐の数を制限するためのコンカレンシー要因を設定できるという 1 つの例外を除き、`ThrottleParallelForEach` アクティビティと似ていることについて示します。
  
 [データベース アクセス アクティビティ](database-access-activities.md)  
 データベースにアクセスして情報を取得または変更し、 [ADO.NET](../../data/adonet/index.md)を使用してデータベースにアクセスできるようにするアクティビティを作成する方法を示します。  
  
 [.NET Framework 4.5 の外部化されたポリシー アクティビティ](externalized-policy-activity-in-net-framework-4-5.md)  
 ExternalizedPolicy4 アクティビティを使用して、WF 3.5 に同梱されているルールエンジンを使用して、[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] (WF 4.5) の Windows Workflow Foundation にある .NET Framework 3.5 (wf 3.5) <xref:System.Workflow.Activities.Rules.RuleSet> オブジェクトの既存の Windows Workflow Foundation を直接実行する方法を示します。 
  
 [非ジェネリックの ForEach](non-generic-foreach.md)  
 <xref:System.Activities.Statements.ForEach%601> アクティビティの非ジェネリック バージョンを作成する方法を示します。  
  
 [非ジェネリックの ParallelForEach](non-generic-parallelforeach.md)  
 <xref:System.Activities.Statements.ParallelForEach%601> アクティビティの非ジェネリック バージョンを作成する方法を示します。  
  
 [WorkflowInstanceId の取得](get-workflowinstanceid.md)  
 カスタム アクティビティ `GetWorkflowInstanceId` を使用して、ワークフロー インスタンス ID を返す方法を示します。
