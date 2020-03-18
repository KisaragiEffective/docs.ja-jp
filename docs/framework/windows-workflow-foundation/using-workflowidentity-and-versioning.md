---
title: WorkflowIdentity と Versioning の使用
ms.date: 03/30/2017
ms.assetid: b8451735-8046-478f-912b-40870a6c0c3a
ms.openlocfilehash: 97224caa24b38a00a1cbb4fa76781eea3a10faaf
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76787911"
---
# <a name="using-workflowidentity-and-versioning"></a>WorkflowIdentity と Versioning の使用

<xref:System.Activities.WorkflowIdentity> を使用すると、ワークフロー アプリケーションの開発者は、名前と <xref:System.Version> をワークフロー定義に関連付け、永続化されたワークフロー インスタンスにこの情報を関連付けることができます。 この ID 情報は、ワークフロー アプリケーションの開発者がワークフロー定義の複数のバージョンの side-by-side 実行などのシナリオを有効にするために使用できます。また、動的更新などの他の機能の基礎となります。 このトピックでは、<xref:System.Activities.WorkflowIdentity> ホスティングでの <xref:System.Activities.WorkflowApplication> の使用の概要について説明します。 ワークフローサービスでのワークフロー定義の side-by-side 実行の詳細については、「 [WorkflowServiceHost でのサイドバイサイドバージョン管理](../wcf/feature-details/side-by-side-versioning-in-workflowservicehost.md)」を参照してください。 動的更新の詳細については、「[動的更新](dynamic-update.md)」を参照してください。

## <a name="in-this-topic"></a>このトピックの内容

- [WorkflowIdentity の使用](using-workflowidentity-and-versioning.md#UsingWorkflowIdentity)

  - [WorkflowIdentity を使用した side-by-side 実行](using-workflowidentity-and-versioning.md#SxS)

- [ワークフローのバージョン管理をサポートするために .NET Framework 4 つの永続性データベースをアップグレードする](using-workflowidentity-and-versioning.md#UpdatingWF4PersistenceDatabases)

  - [データベーススキーマをアップグレードするには](using-workflowidentity-and-versioning.md#ToUpgrade)

## <a name="UsingWorkflowIdentity"></a>WorkflowIdentity の使用

<xref:System.Activities.WorkflowIdentity> を使用するには、インスタンスを作成および構成し、<xref:System.Activities.WorkflowApplication> インスタンスに関連付けます。 <xref:System.Activities.WorkflowIdentity> インスタンスには 3 種類の識別情報が格納されます。 <xref:System.Activities.WorkflowIdentity.Name%2A> と <xref:System.Activities.WorkflowIdentity.Version%2A> は必須で、名前と <xref:System.Version> を表します。また、<xref:System.Activities.WorkflowIdentity.Package%2A> は省略可能で、アセンブリ名やその他の必要な情報などの情報を格納する追加文字列の指定に使用できます。 <xref:System.Activities.WorkflowIdentity> は、その 3 つのプロパティのいずれかが他の <xref:System.Activities.WorkflowIdentity> と異なる場合に一意です。

> [!IMPORTANT]
> <xref:System.Activities.WorkflowIdentity> には、個人を特定できる情報 (PII) を含めないでください。 インスタンスの作成に使用される <xref:System.Activities.WorkflowIdentity> に関する情報は、ランタイムによるアクティビティ ライフ サイクルのさまざまなポイントで構成されているすべての追跡サービスに出力されます。 WF の追跡には PII (機密ユーザー データ) を非表示にするメカニズムがありません。 そのため、<xref:System.Activities.WorkflowIdentity> インスタンスには PII データを含めないでください。PII データは、ランタイムによって追跡レコードに出力され、追跡レコードを表示するためのアクセス権を持つユーザーに表示できます。

次の例では、<xref:System.Activities.WorkflowIdentity> を作成し、`MortgageWorkflow` のワークフロー定義を使用して作成したワークフローのインスタンスに関連付けます。

```csharp
WorkflowIdentity identityV1 = new WorkflowIdentity
{
    Name = "MortgageWorkflow v1",
    Version = new Version(1, 0, 0, 0)
};

WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow(), identity);

// Configure the WorkflowApplication with persistence and desired workflow event handlers.
ConfigureWorkflowApplication(wfApp);

// Run the workflow.
wfApp.Run();
```

ワークフローを再読み込みして再開すると、永続化されたワークフロー インスタンスの <xref:System.Activities.WorkflowIdentity> と一致するように構成されている <xref:System.Activities.WorkflowIdentity> を使用する必要があります。

```csharp
WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow(), identityV1);

// Configure the WorkflowApplication with persistence and desired workflow event handlers.
ConfigureWorkflowApplication(wfApp);

// Load the workflow.
wfApp.Load(instanceId);

// Resume the workflow...
```

ワークフロー インスタンスの再読み込み時に使用される <xref:System.Activities.WorkflowIdentity> が永続化された <xref:System.Activities.WorkflowIdentity> と一致しない場合は、<xref:System.Activities.VersionMismatchException> がスローされます。 次の例では、前の例で永続化された `MortgageWorkflow` インスタンスで読み込み操作が行われます。 この読み込み操作は、永続化されたインスタンスと一致しない、住宅ローン ワークフローの新しいバージョン用に構成された <xref:System.Activities.WorkflowIdentity> を使用して行われます。

```csharp
WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow_v2(), identityV2);

// Configure the WorkflowApplication with persistence and desired workflow event handlers.
ConfigureWorkflowApplication(wfApp);

// Attempt to load the workflow instance.
wfApp.Load(instanceId);

// Resume the workflow...
```

前のコードが実行されると、次の <xref:System.Activities.VersionMismatchException> がスローされます。

```output
The WorkflowIdentity ('MortgageWorkflow v1; Version=1.0.0.0') of the loaded instance does not match the WorkflowIdentity ('MortgageWorkflow v2; Version=2.0.0.0') of the provided workflow definition. The instance can be loaded using a different definition, or updated using Dynamic Update.
```

### <a name="SxS"></a>WorkflowIdentity を使用した side-by-side 実行

<xref:System.Activities.WorkflowIdentity> を使用すると、ワークフローの複数のバージョンの side-by-side 実行を簡略化できます。 たとえば、実行時間の長いワークフローのビジネス要件を変更します。 ワークフローの多くのインスタンスは、更新されたバージョンを配置すると実行できます。 ホスト アプリケーションは、新しいインスタンスの開始時に更新されたワークフロー定義を使用するよう構成できます。また、インスタンスの再開時に適切なワークフロー定義を指定するのはホスト アプリケーションの役割です。 <xref:System.Activities.WorkflowIdentity> を使用すると、ワークフロー インスタンスの再開時に、一致するワークフロー定義を特定して指定できます。

永続化されたワークフロー インスタンスの <xref:System.Activities.WorkflowIdentity> を取得するには、<xref:System.Activities.WorkflowApplication.GetInstance%2A> メソッドを使用します。 <xref:System.Activities.WorkflowApplication.GetInstance%2A> メソッドは、永続化されたワークフロー インスタンスの <xref:System.Activities.WorkflowApplication.Id%2A>、および永続化されたインスタンスを格納する <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を取得して、<xref:System.Activities.WorkflowApplicationInstance> を返します。 <xref:System.Activities.WorkflowApplicationInstance> には、永続化されたワークフロー インスタンスに関する情報 (関連付けられた <xref:System.Activities.WorkflowIdentity> など) が格納されます。 この関連付けられた <xref:System.Activities.WorkflowIdentity> は、ワークフロー インスタンスを読み込んで再開するときに、適切なワークフロー定義を指定するためにホストで使用できます。

> [!NOTE]
> null の <xref:System.Activities.WorkflowIdentity> は有効で、ホストで使用すると、関連付けられた <xref:System.Activities.WorkflowIdentity> がない、永続化されたインスタンスを適切なワークフロー定義にマップできます。 このシナリオは、ワークフローアプリケーションがワークフローのバージョン管理を使用して最初に記述されていない場合、またはアプリケーションが .NET Framework 4 からアップグレードされた場合に発生する可能性があります。 詳細については、「 [.NET Framework 4 の永続性データベースのアップグレード」を](using-workflowidentity-and-versioning.md#UpdatingWF4PersistenceDatabases)参照してください。

次の例では、`Dictionary<WorkflowIdentity, Activity>` を使用して <xref:System.Activities.WorkflowIdentity> インスタンスとそれに対応するワークフロー定義を関連付けます。ワークフローは、`identityV1` <xref:System.Activities.WorkflowIdentity>に関連付けられている `MortgageWorkflow` ワークフロー定義を使用して開始されます。

```csharp
WorkflowIdentity identityV1 = new WorkflowIdentity
{
    Name = "MortgageWorkflow v1",
    Version = new Version(1, 0, 0, 0)
};

WorkflowIdentity identityV2 = new WorkflowIdentity
{
    Name = "MortgageWorkflow v2",
    Version = new Version(2, 0, 0, 0)
};

Dictionary<WorkflowIdentity, Activity> WorkflowVersionMap = new Dictionary<WorkflowIdentity, Activity>();
WorkflowVersionMap.Add(identityV1, new MortgageWorkflow());
WorkflowVersionMap.Add(identityV2, new MortgageWorkflow_v2());

WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow(), identityV1);

// Configure the WorkflowApplication with persistence and desired workflow event handlers.
ConfigureWorkflowApplication(wfApp);

// Run the workflow.
wfApp.Run();
```

次の例では、前の例の永続化されたワークフロー インスタンスに関する情報を <xref:System.Activities.WorkflowApplication.GetInstance%2A> を呼び出すことによって取得し、永続化された <xref:System.Activities.WorkflowIdentity> の情報を使用して一致するワークフロー定義を取得します。 この情報を使用して <xref:System.Activities.WorkflowApplication> を構成すると、ワークフローが読み込まれます。 <xref:System.Activities.WorkflowApplication.Load%2A> を受け取る <xref:System.Activities.WorkflowApplicationInstance> オーバーロードが使用されるため、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> で構成された <xref:System.Activities.WorkflowApplicationInstance> が <xref:System.Activities.WorkflowApplication> によって使用され、その結果、その <xref:System.Activities.WorkflowApplication.InstanceStore%2A> プロパティを構成する必要はありません。

> [!NOTE]
> <xref:System.Activities.WorkflowApplication.InstanceStore%2A> プロパティを設定する場合は、<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> によって使用されるのと同じ <xref:System.Activities.WorkflowApplicationInstance> インスタンスで設定する必要があります。そうしないと、<xref:System.ArgumentException> がスローされ、"`The instance is configured with a different InstanceStore than this WorkflowApplication.`" というメッセージが表示されます。

```csharp
// Get the WorkflowApplicationInstance of the desired workflow from the specified
// SqlWorkflowInstanceStore.
WorkflowApplicationInstance instance = WorkflowApplication.GetInstance(instanceId, store);

// Use the persisted WorkflowIdentity to retrieve the correct workflow
// definition from the dictionary.
Activity definition = WorkflowVersionMap[instance.DefinitionIdentity];

WorkflowApplication wfApp = new WorkflowApplication(definition, instance.DefinitionIdentity);

// Configure the WorkflowApplication with persistence and desired workflow event handlers.
ConfigureWorkflowApplication(wfApp);

// Load the persisted workflow instance.
wfApp.Load(instance);

// Resume the workflow...
```

## <a name="UpdatingWF4PersistenceDatabases"></a>ワークフローのバージョン管理をサポートするために .NET Framework 4 つの永続性データベースをアップグレードする

.NET Framework 4 つのデータベーススクリプトを使用して作成された永続性データベースをアップグレードするために、Sqlworkflowinstancestoreschemaupgrade.sql データベーススクリプトが用意されています。 このスクリプトは、.NET Framework 4.5 で導入された新しいバージョン管理機能をサポートするようにデータベースを更新します。 データベースで永続化されたすべてのワークフロー インスタンスは、既定のバージョン番号が付与されるため、side-by-side 実行および動的更新に参加できるようになります。

.NET Framework 4.5 ワークフローアプリケーションが、指定されたスクリプトを使用してアップグレードされていない永続性データベースで新しいバージョン管理機能を使用するすべての永続化操作を試みると、次のようなメッセージと共に <xref:System.Runtime.DurableInstancing.InstancePersistenceCommandException> がスローされます。

```output
The SqlWorkflowInstanceStore has a database version of '4.0.0.0'. InstancePersistenceCommand 'System.Activities.DurableInstancing.CreateWorkflowOwnerWithIdentityCommand' cannot be run against this database version.  Please upgrade the database to '4.5.0.0'.
```

### <a name="ToUpgrade"></a>データベーススキーマをアップグレードするには

1. SQL Server Management Studio を開き、 **.\SQLEXPRESS**などの永続性データベースサーバーに接続します。

2. **[ファイル]** メニューの [**開く** **] をクリック**します。 次のフォルダーに移動します: `C:\Windows\Microsoft.NET\Framework\4.0.30319\sql\en`。

3. 選択**SqlWorkflowInstanceStoreSchemaUpgrade.sql** をクリック**オープン**します。

4. **[使用できるデータベース]** ドロップダウンで、永続性データベースの名前を選択します。

5. **[クエリ]** メニューの **[実行]** をクリックします。

クエリが完了すると、データベース スキーマがアップグレードされるため、必要に応じて、永続化されたワークフロー インスタンスに割り当てられた既定のワークフロー ID を確認できます。 **オブジェクトエクスプローラー**の **[データベース]** ノードで永続性データベースを展開し、 **[ビュー]** ノードを展開します。 右クリックして**System.Activities.DurableInstancing.Instances**選択**上位 1000 行**します。 列の最後までスクロールし、ビューに追加された6つの**列 ([** **IdentityPackage**]、 **[ビルド]** 、 **[メジャー]** 、 **[マイナー]** 、 **[リビジョン]** ) が表示されていることを確認します。 永続化されたワークフローでは、null のワークフロー id を表す null 値がこれらのフィールドに対して**null**になります。
