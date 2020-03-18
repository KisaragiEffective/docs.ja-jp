---
title: ワークフロー サービスのホスティング
ms.date: 03/30/2017
ms.assetid: 2d55217e-8697-4113-94ce-10b60863342e
ms.openlocfilehash: 21e24853229d09e3f1af719573f47bb12c8fddb6
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75963738"
---
# <a name="hosting-workflow-services"></a>ワークフロー サービスのホスティング

ワークフロー サービスが受信メッセージに応答するには、ワークフロー サービスがホストされている必要があります。 ワークフロー サービスは WCF メッセージング インフラストラクチャを使用するため、これと似た方法でホストされます。 WCF サービスと同様に、ワークフローサービスは、インターネットインフォメーションサービス (IIS) または Windows プロセスアクティブ化サービス (WAS) の下で、任意のマネージアプリケーションでホストできます。 また、ワークフローサービスは Windows Server App Fabric でホストできます。 Windows Server App Fabric の詳細については、「 [Windows Server App fabric のドキュメント](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10))」、「 [Appfabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))」、および「 [appfabric のホスティングの概念](https://docs.microsoft.com/previous-versions/appfabric/ee677371(v=azure.10))」を参照してください。 WCF サービスをホストするさまざまな方法の詳細については、「[ホスティングサービス](../../../../docs/framework/wcf/hosting-services.md)」を参照してください。

## <a name="hosting-in-a-managed-application"></a>マネージド アプリケーションでのホスト
 マネージド アプリケーションでワークフロー サービスをホストするには、<xref:System.ServiceModel.Activities.WorkflowServiceHost> クラスを使用します。 <xref:System.ServiceModel.Activities.WorkflowServiceHost> コンストラクターにより、シングルトン ワークフロー サービス インスタンス、ワークフロー サービス定義、またはワークフロー メッセージング アクティビティを使用するアクティビティを指定できます。 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> の呼び出しによって、サービスが受信メッセージのリッスンを開始します。

## <a name="hosting-under-iis-or-was"></a>IIS または WAS の下でのホスト
 IIS または WAS の下でワークフロー サービスをホストする場合は、仮想ディレクトリを作成し、サービスとその動作を定義するファイルをその仮想ディレクトリ内に配置します。 IIS または WAS の下でワークフローをホストする場合は、いくつかの方法が考えられます。

- ワークフロー サービスを定義する .xamlx ファイルを、サービス動作、エンドポイント、およびその他の構成要素を指定する Web.config ファイルと共に、IIS/WAS 仮想ディレクトリに配置します。

- ワークフロー サービスを定義する .xamlx ファイルを IIS/WAS 仮想ディレクトリに配置します。 この .xamlx ファイルで、公開するエンドポイントが指定されます。 エンドポイントは、次の例に示すように、`WorkflowService.Endpoints` 要素で指定されます。

    ```xml
    <WorkflowService xmlns="http://schemas.microsoft.com/netfx/2009/xaml/servicemodel"  xmlns:p1="http://schemas.microsoft.com/netfx/2009/xaml/activities" xmlns:sad="clr-namespace:System.Activities.Debugger;assembly=System.Activities" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
      <WorkflowService.Endpoints>
        <Endpoint ServiceContractName="IWorkFlowEchoService" AddressUri="">
          <Endpoint.Binding>
            <BasicHttpBinding />
          </Endpoint.Binding>
        </Endpoint>
      </WorkflowService.Endpoints>
    <!-- ... -->
    </WorkflowService>
    ```

    > [!NOTE]
    > 動作は .xamlx ファイルで指定できないため、動作設定を指定するには Web.config を使用する必要があります。

- ワークフロー サービスを定義する .xamlx ファイルを IIS/WAS 仮想ディレクトリに配置します。 さらに、.svc ファイルを仮想ディレクトリに配置します。 .svc ファイルでは、カスタム Web サービス ホスト ファクトリの指定、カスタム動作の適用、またはカスタムの場所からの構成の読み込みが可能です。

- WCF メッセージング アクティビティを使用するアクティビティを含むアセンブリを IIS/WAS 仮想ディレクトリに配置します。

 ワークフローサービスを定義する .xamlx ファイルには、<xref:System.Workflow.ComponentModel.Activity>から派生した任意の型を含む <`Service`> ルート要素またはルート要素が含まれている必要があります。 Visual Studio アクティビティテンプレートを使用すると、.xamlx ファイルが作成されます。 WCF ワークフローサービステンプレートを使用すると、.xamlx ファイルが作成されます。

## <a name="hosting-workflow-services-under-windows-server-app-fabric"></a>Windows Server AppFabric でのワークフロー サービスのホスティング
 Windows Server AppFabric でのワークフロー サービスのホスティングは IIS/WAS でのホスティングと同じ方法で行われます。 唯一の違いは、Windows Server AppFabric がインストールされるということです。 Windows Server AppFabric には、PowerShell コマンドと同様に、インターネット インフォメーション サービス マネージャーに追加されるツールが用意されています。 これらのツールによって、ワークフロー サービスおよび WCF サービスの配置、管理、および追跡を簡略化することができます。

## <a name="referencing-custom-activities"></a>カスタム アクティビティの参照
 カスタムアクティビティへの参照は、アプリケーションドメインに読み込まれ、XAML デシリアライザーが型を見つけることができるように、<`System.Web.Compilation`> の下の <`Assemblies`> セクションに追加する必要があります。 これらの設定をコンピューター上のすべてのアプリケーションに適用する必要がある場合は、アプリケーション レベルまたはルートの Web.config で設定できます。

## <a name="deployment"></a>展開
 配置作業を容易にするために、Web 配置ツールが作成されています。 このツールを使用すると、アプリケーションの IIS 6.0 および IIS 7.0 間での移行や、サーバー ファームの同期のほか、Web アプリケーションのパッケージ化、アーカイブ、および配置を実行できます。 詳細については、「 [MS Deployment Tool](https://go.microsoft.com/fwlink/?LinkId=178690)」を参照してください。

## <a name="see-also"></a>関連項目

- [ワークフロー サービス ホストの内部](../../../../docs/framework/wcf/feature-details/workflow-service-host-internals.md)
- [WorkflowServiceHost の構成](../../../../docs/framework/wcf/feature-details/configuring-workflowservicehost.md)
