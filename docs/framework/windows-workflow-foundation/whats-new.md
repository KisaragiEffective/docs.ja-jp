---
title: Windows Workflow Foundation の新機能
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Workflow Foundation [WF], what's new
- WF [WF], what's new
ms.assetid: 11f96014-001e-41a0-bcc2-d0684a52fa43
ms.openlocfilehash: 8f79c6d2a564571f8b753f322a79e91a01b1cf2f
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74142006"
---
# <a name="whats-new-in-windows-workflow-foundation"></a>Windows Workflow Foundation の新機能

.NET Framework 4 の Windows Workflow Foundation (WF) は、以前のバージョンのいくつかの開発パラダイムを変更します。 ワークフローでは、新しい機能のホストの作成、実行、保守、実装が簡単になっています。 .NET 3.0 と .NET 3.5 ワークフローアプリケーションを移行して最新バージョンを使用する方法の詳細については、「[移行のガイダンス](migration-guidance.md)」を参照してください。  
  
## <a name="workflow-activity-model"></a>ワークフロー アクティビティ モデル  
 アクティビティは、現在はワークフロー作成の基本単位であり、<xref:System.Workflow.Activities.SequentialWorkflowActivity> クラスや <xref:System.Workflow.Activities.StateMachineWorkflowActivity> クラスは使用されなくなっています。 <xref:System.Activities.Activity> クラスは、ワークフロー動作の基本抽象クラスです。 このため、アクティビティの作成者は、基本的なカスタム アクティビティ機能用に <xref:System.Activities.CodeActivity> を実装するか、一定範囲のランタイムを使用するカスタム アクティビティ機能用に <xref:System.Activities.NativeActivity> を実装することができます。 <xref:System.Activities.Activity> は、カスタム開発されているか、[組み込みのアクティビティライブラリ](net-framework-4-5-built-in-activity-library.md)に含まれているかにかかわらず、他の <xref:System.Activities.NativeActivity>、<xref:System.Activities.CodeActivity>、<xref:System.Activities.AsyncCodeActivity>、または <xref:System.Activities.DynamicActivity> オブジェクトの観点から、アクティビティ作成者が新しい動作を宣言によって表現するために使用されるクラスです。  
  
## <a name="rich-composite-activity-options"></a>豊富な複合アクティビティ オプション  
 <xref:System.Activities.Statements.Flowchart> は新しい強力な制御フロー アクティビティです。作成者は、これを使用して任意のループや条件分岐をモデル化できます。 <xref:System.Activities.Statements.Flowchart> により、以前は <xref:System.Workflow.Activities.StateMachineWorkflowActivity> でのみ実装が可能であったイベント ドリブン プログラミング モデルを使用できます。 手続き型のワークフローでは、<xref:System.Activities.Statements.TryCatch> や <xref:System.Activities.Statements.Switch%601> などの従来のフロー制御構造をモデル化する新しいフロー制御アクティビティを利用できます。  
  
## <a name="expanded-built-in-activity-library"></a>拡張ビルトイン アクティビティ ライブラリ  
 アクティビティ ライブラリには、次のような新しい機能があります。  
  
- <xref:System.Activities.Statements.DoWhile>、<xref:System.Activities.Statements.Pick>、<xref:System.Activities.Statements.TryCatch>、<xref:System.Activities.Statements.ForEach%601>、<xref:System.Activities.Statements.Switch%601>、<xref:System.Activities.Statements.ParallelForEach%601> などの新しいフロー制御アクティビティ  
  
- <xref:System.Activities.Statements.Assign> などのメンバー データを操作するためのアクティビティと、<xref:System.Activities.Statements.AddToCollection%601> などのコレクション アクティビティ  
  
- <xref:System.Activities.Statements.TransactionScope> や <xref:System.Activities.Statements.Compensate> などのトランザクションを制御するアクティビティ  
  
- <xref:System.ServiceModel.Activities.SendContent> や <xref:System.ServiceModel.Activities.ReceiveReply> などの新しいメッセージング アクティビティ  
  
## <a name="explicit-activity-data-model"></a>明示的なアクティビティ データ モデル  
 .NET Framework 4 には、データを格納または移動するための新しいオプションが含まれています。 データは、<xref:System.Activities.Variable> を使用してアクティビティに格納できます。 データをアクティビティに移動したり、アクティビティから移動したりするときは、特殊な引数型を使用してデータの移動方向が判定されます。 これらの型は、<xref:System.Activities.InArgument>、<xref:System.Activities.InOutArgument>、<xref:System.Activities.OutArgument>です。 詳細については、「 [Windows Workflow Foundation データモデル](data-model.md)」を参照してください。  
  
## <a name="enhanced-hosting-persistence-and-tracking-options"></a>ホスティング、永続化、追跡の強化されたオプション  
 .NET Framework 4 には、次のような永続性の強化が含まれています。  
  
- <xref:System.ServiceModel.Activities.WorkflowServiceHost>、<xref:System.Activities.WorkflowApplication>、<xref:System.Activities.WorkflowInvoker> などの、ワークフローを実行するための多数のオプションがあります。  
  
- <xref:System.Activities.Statements.Persist> アクティビティを使用してワークフロー状態データを明示的に永続化できます。  
  
- ホストは、<xref:System.Activities.ActivityInstance> をアンロードせずに永続化できます。  
  
- ワークフローは、永続化できないデータを処理するときは非永続化ゾーンを指定できます。このため、非永続化ゾーンを終了するまで永続化は延期されます。  
  
- トランザクションは、<xref:System.Activities.Statements.TransactionScope> を使用してワークフローに流し込むことができます。  
  
- 追跡は、<xref:System.Activities.Tracking.TrackingParticipant> を使用して、簡単に実行できます。  
  
- システムのイベント ログへの追跡は <xref:System.Activities.Tracking.EtwTrackingParticipant> を使用して実現されます。  
  
- 保留中のワークフローの再開は、<xref:System.Activities.Bookmark> オブジェクトを使用して管理されるようになりました。  
  
## <a name="easier-ability-to-extend-wf-designer-experience"></a>WF デザイナー エクスペリエンスの容易な拡張  
 新しい WF デザイナーは Windows Presentation Foundation (WPF) に基づいて構築されており、Visual Studio の外部で WF デザイナーを再ホストするときに使用する簡単なモデルが用意されています。また、カスタムアクティビティデザイナーを作成するための簡単なメカニズムも用意されています。 詳細については、「[ワークフローデザインエクスペリエンスのカスタマイズ](customizing-the-workflow-design-experience.md)」を参照してください。
