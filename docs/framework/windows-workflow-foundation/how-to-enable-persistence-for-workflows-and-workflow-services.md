---
title: '方法: ワークフローとワークフロー サービスの永続化を有効にする'
description: ワークフローとワークフロー サービスの永続性をプログラムと構成ファイルから使用して有効にできるように、SQL Workflow Instance Store を構成する方法について説明します。
ms.date: 03/30/2017
ms.assetid: 2b1c8bf3-9866-45a4-b06d-ee562393e503
ms.openlocfilehash: 31fe6e3f06989e9a42254747565342cf97e4b9f1
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2020
ms.locfileid: "83421515"
---
# <a name="how-to-enable-persistence-for-workflows-and-workflow-services"></a>方法: ワークフローとワークフロー サービスの永続化を有効にする

ここでは、ワークフローとワークフロー サービスの永続化を有効にする方法について説明します。

## <a name="enable-persistence-for-workflows"></a>ワークフローの永続化を有効にする

<xref:System.Activities.WorkflowApplication> クラスの <xref:System.Activities.WorkflowApplication.InstanceStore%2A> プロパティを使用して、インスタンス ストアと **WorkflowApplication** を関連付けることができます。 <xref:System.Activities.WorkflowApplication.Persist%2A> メソッドは、アプリケーションに関連付けられたインスタンス ストアにワークフローを保存または永続化します。 <xref:System.Activities.WorkflowApplication.Unload%2A> メソッドは、ワークフローをインスタンス ストアに永続化し、メモリからインスタンスをアンロードします。 **Load** メソッドは、インスタンスの永続化ストアに格納されているワークフロー データを使用し、ワークフローをメモリに読み込みます。

**Persist** メソッドは、次の手順を実行します。

1. ワークフローのスケジューラを一時停止し、アイドル状態に移行するまで待機します。

2. ワークフローを永続化 (永続化ストアに保存) します。

3. ワークフローのスケジューラを再開します。

 **Unload** メソッドは、次の手順を実行します。

1. ワークフローのスケジューラを一時停止し、アイドル状態に移行するまで待機します。

2. ワークフローを永続化 (永続化ストアに保存) します。

3. メモリ内のワークフロー インスタンスを破棄します。

ワークフローが非永続化ゾーンにある間は、ワークフローが非永続化ゾーンを終了するまで、**Persist** メソッドと **Unload** メソッドのどちらもブロックします。 メソッドは、非永続化ゾーンの完了後に、永続化またはアンロードの操作を続行します。 期限切れになる前に非永続化ゾーンが完了しない場合、または永続化プロセスに時間がかかりすぎる場合は、TimeoutException がスローされます。

## <a name="enable-persistence-for-workflow-services-in-code"></a>コードでのワークフロー サービスの永続化を有効にする

<xref:System.ServiceModel.WorkflowServiceHost> クラスの **DurableInstancingOptions** メンバーには **InstanceStore** というプロパティがあり、インスタンス ストアを **WorkflowServiceHost** に関連付けるために使用できます。

```csharp
// wsh is an instance of WorkflowServiceHost class
wsh.DurableInstancingOptions.InstanceStore = new SqlWorkflowInstanceStore();
```

**WorkflowServiceHost** を開くと、**DurableInstancingOptions.InstanceStore** が null ではない場合、永続化が自動的に有効になります。

通常、サービスの動作には、**InstanceStore** プロパティを使用してワークフロー サービス ホストと共に使用できる具象インスタンス ストアが用意されています。 たとえば、SqlWorkflowInstanceStoreBehavior を使用すると、**SqlWorkflowInstanceStore** のインスタンスが作成されて構成され、**DurableInstancingOptions.InstanceStore** に割り当てられます。

## <a name="enable-persistence-for-workflow-services-using-an-application-configuration-file"></a>アプリケーション構成ファイルを使用してワークフロー サービスの永続化を有効にする

次のコードを app.config または web.config file に追加すると、アプリケーション構成ファイルを使用して永続化を有効にできます。

```xml
<configuration>
  <system.serviceModel>
    <behaviors>
      <serviceBehaviors>
        <behavior name="myBehavior">
          <sqlWorkflowInstanceStore connectionString="Data Source=myDatabaseServer;Initial Catalog=myPersistenceDatabase" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```
