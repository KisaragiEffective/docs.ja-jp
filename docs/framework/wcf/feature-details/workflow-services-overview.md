---
title: ワークフローサービスの概要
ms.date: 03/30/2017
ms.assetid: e536dda3-e286-441e-99a7-49ddc004b646
ms.openlocfilehash: cb013dd419d09af61eaff290709164427b1b655f
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347859"
---
# <a name="workflow-services-overview"></a>ワークフローサービスの概要

ワークフローサービスは、ワークフローを使用して実装される WCF ベースのサービスです。 ワークフローサービスは、メッセージングアクティビティを使用して Windows Communication Foundation (WCF) メッセージを送受信するワークフローです。 .NET Framework 4.5 では、ワークフロー内からメッセージを送受信できるようにする多数のメッセージ アクティビティが導入されています。 メッセージングアクティビティの詳細と、それらを使用してさまざまなメッセージ交換パターンを実装する方法については、「[メッセージングアクティビティ](messaging-activities.md)」を参照してください。

## <a name="benefits-of-using-workflow-services"></a>ワークフローサービスを使用する利点

アプリケーションが分散型になるにつれ、負荷を軽減させるために、個々のサービスが他のサービスを呼び出す役割を果たすようになっています。 これらの呼び出しを非同期操作として実装すると、コードがやや複雑になります。 エラー処理によって、例外処理と詳細な追跡情報の形式がさらに複雑になります。 一部のサービスには、長時間実行されることが多く、入力を待機する間に貴重なシステム リソースを占有するものがあります。 このような問題があるため、分散アプリケーションは、非常に複雑で、作成と保守が困難であることがよくあります。 ワークフローは、特に外部サービスへの呼び出しなど、非同期操作の連携を表す方法として最適です。 また、実行時間が長いビジネス プロセスを表す場合にも効率的です。 ワークフローが、分散環境でのサービス構築に役立つ資産となるのは、これらの特性があるためです。

## <a name="implementing-a-workflow-service"></a>ワークフローサービスの実装

WCF サービスを実装する場合は、サービスと、そのサービスが送受信するデータを記述する多くのコントラクトを定義します。 データは、データ コントラクトとメッセージ コントラクトで表されます。 WCF サービスもワークフロー サービスも、サービスの説明の一環として、データ コントラクトとメッセージ コントラクトの定義を使用します。 サービス自体は、(WSDL の形で) メタデータを公開して、サービスの操作を説明します。 WCF では、サービス コントラクトと操作コントラクトによって、サービス、およびサービスがサポートする操作を定義します。 ただし、ワークフロー サービスでは、これらのコントラクトはビジネス プロセス自体の一部です。 これらは、コントラクト推論と呼ばれるプロセスによってメタデータとして公開されます。 ワークフロー サービスが <xref:System.ServiceModel.Activities.WorkflowServiceHost> を使用してホストされている場合は、ワークフローで見つかった一連のメッセージ アクティビティに基づいて、ワークフロー定義が確認され、コントラクトが生成されます。 具体的には、次のアクティビティとプロパティが、コントラクトの生成に使用されます。

<xref:System.ServiceModel.Activities.Receive> アクティビティ

- <xref:System.ServiceModel.Activities.Receive.ServiceContractName%2A>

- <xref:System.ServiceModel.Activities.Receive.OperationName%2A>

- <xref:System.ServiceModel.Activities.Receive.Action%2A>

<xref:System.ServiceModel.Activities.SendReply> アクティビティ

- <xref:System.ServiceModel.Activities.SendReply.Action%2A>

<xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティ

コントラクト推論の最終的な結果は、WCF サービスと操作コントラクトと同じデータ構造を使用するサービスの説明です。 この情報を使用して、ワークフロー サービス用に WSDL が公開されます。

> [!NOTE]
> [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] では、ツール サポートを追加せずに、既存のコントラクト定義を使用してワークフロー サービスを記述することはできません。 ワークフロー サービスのコントラクトは、前述のコントラクト推論プロセスによって作成されます。 ただし、メッセージ コントラクトおよびデータ コントラクトは完全にサポートされます。

## <a name="workflow-services-and-msmq-based-bindings"></a>ワークフローサービスと MSMQ ベースのバインド

WCF は、2 つの MSMQ ベースのバインディングである <xref:System.ServiceModel.NetMsmqBinding> と <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> を定義します。  MSMQ ベースのバインディングは、多くの場合、ワークフロー サービスと共に使用されます。これは、これらのサービスに、実行時間が長いという性質があるためです。 MSMQ ベースのバインディングには、MSMQ メッセージが有効と見なされる時間を指定する `ValidityDuration` プロパティがあります。 ワークフロー サービスは本質的に実行時間が長いため、ワークフロー サービスによるメッセージの処理が可能になる前に、MSMQ メッセージの有効期間が切れることがあります。 そのため、MSMQ バインディングの有効期間を適切な値に設定することが重要です。 この値は、ワークフローとそのメッセージの処理方法に基づいて選択する必要があります。 たとえば、<xref:System.ServiceModel.Activities.Receive> アクティビティがあり、これに続いて 10 分間実行されるカスタム アクティビティがあり、その後に別の <xref:System.ServiceModel.Activities.Receive> アクティビティがある場合、`ValidityDuration` の適切な値は 10 分を超えることになります。

## <a name="hosting-a-workflow-service"></a>ワークフローサービスのホスト

WCF サービスと同様に、ワークフローサービスをホストする必要があります。 WCF サービスは、<xref:System.ServiceModel.ServiceHost> クラスを使用してサービスとワークフローサービスをホストし、<xref:System.ServiceModel.Activities.WorkflowServiceHost> を使用してサービスをホストします。 WCF サービスと同様に、ワークフローサービスはさまざまな方法でホストできます。次に例を示します。

- マネージ .NET Framework アプリケーションの場合。

- インターネット インフォメーション サービス (IIS)。

- Windows プロセス アクティブ化サービス (WAS)。

- マネージド Windows サービス。

マネージ .NET Framework アプリケーションまたはマネージ Windows サービスでホストされるワークフローサービスは、<xref:System.ServiceModel.Activities.WorkflowServiceHost> クラスのインスタンスを作成し、<xref:System.ServiceModel.Activities.WorkflowService.Body%2A> プロパティ内にワークフロー定義を含む <xref:System.ServiceModel.Activities.WorkflowService> のインスタンスを渡します。 メッセージ アクティビティを格納するワークフロー定義は、ワークフロー サービスとして公開されます。

ワークフロー サービスを IIS または WAS でホストするには、ワークフロー サービス定義を格納する .xamlx ファイルを仮想ディレクトリに配置します。 既定のエンドポイント (<xref:System.ServiceModel.BasicHttpBinding>を使用) は自動的に作成されます。詳細については、「簡略化された[構成](../../../../docs/framework/wcf/simplified-configuration.md)」を参照してください。 また、Web.config ファイルを仮想ディレクトリに配置し、独自のエンドポイントを指定することもできます。 ワークフロー定義がアセンブリ内にある場合は、.svc ファイルを仮想ディレクトリに配置し、ワークフロー アセンブリを App_Code ディレクトリに配置できます。 この .svc ファイルには、サービス ホスト ファクトリと、ワークフロー サービスを実装するクラスを指定する必要があります。 次の例に、サービス ホスト ファクトリを指定し、ワークフロー サービスを実装するクラスを指定する方法を示します。

```
<%@ServiceHost Factory=" System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory
Service="EchoService"%>
```
