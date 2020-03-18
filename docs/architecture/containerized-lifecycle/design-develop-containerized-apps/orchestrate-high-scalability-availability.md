---
title: 高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する
description: 実際の運用アプリケーションは、すべてのコンテナーの正常性、ワークロードおよびライフ サイクルを管理するオーケストレーターと共に展開して管理する必要があります。
ms.date: 02/15/2019
ms.openlocfilehash: e548e6b3816dec1e56c273c53c9fd052443eb09b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76919537"
---
# <a name="orchestrating-microservices-and-multi-container-applications-for-high-scalability-and-availability"></a>高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する

アプリケーションがマイクロサービスに基づく場合や、複数のコンテナーに分割されている場合、運用可能なアプリケーションのオーケストレーターを使用することが不可欠です。 前述のとおり、マイクロサービス ベースの方法では、マイクロサービスはそれぞれモデルとデータを所有しているため、開発と展開の観点から自立していることになります。 ただし、複数のサービスで構成される従来のアプリケーション (SOA など) を使用する場合でも、分散システムとして展開する必要がある単一のビジネス アプリケーションを構成する複数のコンテナーまたはサービスを使用する場合もあります。 この種のシステムはスケールアウトや管理が複雑であるため、運用可能でスケーラブルな複数のコンテナー アプリケーションを使用する場合、どうしてもオーケストレーターが必要になります。

図 4-6 は、複数のマイクロサービス (コンテナー) で構成されるアプリケーションのクラスターへの展開を示しています。

![クラスターで構成された Docker アプリケーションを示す図](./media/orchestrate-high-scalability-availability/composed-docker-applications-cluster.png)

**図 4-6** コンテナーのクラスター

これは論理的な方法のように見えます。 しかし、これらの構成されたアプリケーションの負荷分散、ルーティング、オーケストレーションはどのように処理するのでしょうか。

Docker CLI は、1 つのホスト上の 1 つのコンテナーを管理するニーズには対応していますが、複数のホストに展開されている複数のコンテナーのより複雑な分散アプリケーションの管理には不十分です。 多くの場合は、自動的にコンテナーを開始し、イメージごとに複数のインスタンスでコンテナーをスケールアウトし、必要に応じてそれらを一時停止またはシャットダウンする管理プラットフォームが必要になります。また、理想的には、ネットワークおよびデータ ストレージなどのリソースへのアクセス方法を制御できる必要があります。

個々のコンテナーまたは単純な構成アプリを管理するだけでなく、マイクロサービスを含むより大きなエンタープライズ アプリケーションを管理するには、オーケストレーションおよびクラスタリング プラットフォームが必要になります。

アーキテクチャと開発の観点から、マイクロサービスを使用する大規模なエンタープライズ アプリケーションを構築する場合、高度なシナリオをサポートする次のプラットフォームと製品を理解することが重要です。

- **クラスターとオーケストレーター。** 大規模なマイクロサービス ベースのアプリケーションなど、多くの Docker ホストでアプリケーションをスケールアウトする必要がある場合は、基になるプラットフォームの複雑さを抽象化することで、これらすべてのホストを単一のクラスターとして管理できるようにすることが重要です。 それは、コンテナー クラスターとオーケストレーターによって実現されるものです。 オーケストレーターの例としては、Azure Service Fabric や Kubernetes が挙げられます。 Kubernetes は、Azure Kubernetes Service を介して Azure で利用可能です。

- **スケジューラ。** "*スケジューリング*" とは、管理者がクラスターのコンテナーを起動できることを意味するので、スケジューラもそれを実行するユーザー インターフェイスを提供します。 クラスター スケジューラにはいくつかの役割があります。たとえば、クラスターのリソースを効率的に使用すること、ユーザーによって指定される制約を設定すること、ノードまたはホスト全体でのコンテナーの負荷分散を効率的に行うこと、高可用性を提供すると同時にエラーに対して堅牢であることなどです。

クラスターとスケジューラの概念は密接に関連しており、さまざまなベンダーによって提供される製品では、多くの場合、両方の機能セットが提供されます。 以下のセクションでは、クラスターとスケジューラで最も重要なプラットフォームとソフトウェアの選択肢を示します。 これらのオーケストレーターは、通常 Azure などのパブリック クラウドで広範に提供されています。

## <a name="software-platforms-for-container-clustering-orchestration-and-scheduling"></a>コンテナーのクラスタリング、オーケストレーション、スケジューリングのためのソフトウェア プラットフォーム

| プラットフォーム | コメント |
|:---:|:---|
| **Kubernetes** <br/> ![Kubernetes ロゴの画像。](./media/orchestrate-high-scalability-availability/kubernetes-container-orchestration-system-logo.png) | [*Kubernetes*](https://kubernetes.io/) はオープンソースの製品であり、クラスターのインフラストラクチャとコンテナーのスケジューリングからオーケストレーションまでのさまざまな機能を提供します。 ホストのクラスター全体のアプリケーション コンテナーの展開、スケーリング、操作を自動化できます。 <br/> <br/> *Kubernetes* ではコンテナー中心のインフラストラクチャが提供され、アプリケーション コンテナーを論理ユニットにグループ化し、管理と検出を容易にします。 <br/> <br/> *Kubernetes* は Linux では完成されていますが、Windows では未完成です。 |
| **Azure Kubernetes Service (AKS)** <br/> ![Azure Kubernetes Service のロゴの画像。](./media/orchestrate-high-scalability-availability/azure-kubernetes-service-logo.png) | [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) は Azure でのマネージド Kubernetes コンテナー オーケストレーション サービスで。これによって Kubernetes クラスターの管理、デプロイ、および操作が簡略化されます。 |
| **Azure Service Fabric** <br/> ![Azure Service Fabric ロゴの画像。](./media/orchestrate-high-scalability-availability/azure-service-fabric-logo.png) | [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) は、アプリケーションを構築するための Microsoft マイクロサービス プラットフォームです。 これはサービスの[オーケストレーター](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-introduction)であり、これによってマシンのクラスターが作成されます。 Service Fabric は、サービスをコンテナーまたはプレーン プロセスとして展開できます。 同じアプリケーションとクラスター内にコンテナーのサービスとプロセスのサービスを混在させることもできます。 <br/> <br/> *Service Fabric* クラスターは、Azure、オンプレミス、または任意のクラウドに配置できます。 ただし、Azure でのデプロイはマネージド アプローチで簡略化されます。 <br/> <br/> *Service Fabric* では、[ステートフル サービス](https://azure.microsoft.com/documentation/articles/service-fabric-reliable-services-introduction/)や [Reliable Actors](https://azure.microsoft.com/documentation/articles/service-fabric-reliable-actors-introduction/) などの追加の省略可能な規範的な [Service Fabric プログラミング モデル](https://azure.microsoft.com/documentation/articles/service-fabric-choose-framework/)が提供されます。 <br/> <br/> *Service Fabric* は Windows (年々進化している Windows) では完成されていますが、Linux では未完成です。 <br/> <br/> Linux と Windows の両方のコンテナーは、2017 年以降の Service Fabric でサポートされています。 |
| **Azure Service Fabric Mesh** <br/> ![Azure Service Fabric Mesh ロゴの画像。](./media/orchestrate-high-scalability-availability/azure-service-fabric-mesh-logo.png) | [*Azure Service Fabric Mesh*](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-overview) では、Service Fabric と同じ信頼性、ミッション クリティカルなパフォーマンス、およびスケールを提供しますが、プラットフォームはフル マネージドのサーバーレス プラットフォームとなります。 クラスター、VM、ストレージ、またはネットワーク構成を管理する必要はありません。 ご自分のアプリケーションの開発に集中できます。 <br/> <br/> *Service Fabric Mesh* では Windows と Linux の両方のコンテナーがサポートされているため、自分で選択した任意のプログラミング言語またはフレームワークを使用して開発を行うことができます。

## <a name="using-container-based-orchestrators-in-azure"></a>Azure でのコンテナー ベースのオーケストレーターの使用

いくつかのクラウド ベンダーでは Docker コンテナーのサポートに加え、Docker クラスターおよびオーケストレーションのサポート (Azure、Amazon EC2 Container Service、Google Container Engine を含む) を提供しています。 Azure では、Azure Kubernetes Service (AKS) と、Azure Service Fabric と、Azure Service Fabric Mesh を介して Docker クラスターおよびオーケストレーターをサポートしています。

## <a name="using-azure-kubernetes-service"></a>Azure Kubernetes Service の使用

Kubernetes クラスターでは複数の Docker ホストをプールし、それらが単一の仮想 Docker ホストとして公開されるため、複数のコンテナーをクラスターにデプロイし、任意の数のコンテナー インスタンスを使用してスケールアウトできます。 クラスターは、スケーラビリティや正常性など、複雑な管理機能をすべて処理します。

ACS には、コンテナー化されたアプリケーションを実行するように Azure 内で事前に構成されている仮想マシンのクラスターの作成、構成、および管理を簡略化する方法が用意されています。 一般的なオープンソースのスケジューリングおよびオーケストレーション ツールの最適化された構成を使用することで、AKS では、コンテナー ベースのアプリケーションを Microsoft Azure でデプロイして管理するための専門知識を持つ増加し続けるコミュニティを利用するか、既存のスキルを使用することができます。

Azure Kubernetes Service によって、一般的な Docker クラスタリング オープンソース ツールとテクノロジの構成が Azure 専用に最適化されます。 コンテナーとアプリケーションの構成の両方に移植性を提供するオープン ソリューションが得られます。 サイズ、ホストの数、オーケストレーター ツールを選択すると、他のことはすべて AKS によって処理されます。

![Kubernetes クラスター構造を示す図。](./media/orchestrate-high-scalability-availability/kubernetes-cluster-simplified-structure.png)

**図 4-7**. Kubernetes クラスターの簡略化された構造とトポロジ

図 4-7 では、マスター ノード (VM) がクラスターの調整の大部分を行う Kubernetes クラスターの構造を示しています。これでは、アプリケーションの観点から 1 つのプールとして管理されているその他のノードに、コンテナーをデプロイできます。 これにより、数千または数万ものコンテナーに対するスケーリングを行うことができます。

## <a name="development-environment-for-kubernetes"></a>Kubernetes 用の開発環境

開発環境に単純に [Docker Desktop](https://www.docker.com/community-edition) をインストールすることで、1 台の開発マシン (Windows 10 または macOS) でも Kubernetes を実行できることが [2018 年 7 月に Docker から発表](https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel/)されました。 図 4-8 に示すように、さらなる統合テストを行うために、後でクラウド (AKS) にデプロイすることができます。

![開発マシン上にあり、AKS にデプロイされる Kubernetes を示す図。](./media/orchestrate-high-scalability-availability/kubernetes-development-environment.png)

**図 4-8** 開発マシンとクラウドでの Kubernetes の実行

## <a name="get-started-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) の使用の開始

AKS の使用を開始するには、Azure portal から、または CLI を使用して AKS クラスターをデプロイします。 Azure の Kubernetes クラスター デプロイの詳細については、[Azure Kubernetes Service (AKS) クラスターのデプロイ](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)に関するページを参照してください。

AKS の一部として既定でインストールされるソフトウェアはいずれも無料です。 既定のすべてのオプションはオープン ソース ソフトウェアと共に実装されます。 AKS は Azure 内の複数の仮想マシンで使用できます。 選択したコンピューティング インスタンスと、ストレージやネットワーキングなどの利用された他の基になるインフラストラクチャ リソースにのみ課金されます。 AKS 自体に従量課金はありません。

`kubectl` および元の `.yaml` ファイルに基づく Kubernetes へのデプロイの詳細については、[AKS (Azure Kubernetes Service) での eShopOnContainers の設定](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.-Setting-the-solution-up-in-AKS-(Azure-Kubernetes-Service))に関する投稿記事を参照してください。

## <a name="deploy-with-helm-charts-into-kubernetes-clusters"></a>Helm Chart を使用した Kubernetes クラスターへのデプロイ

Kubernetes クラスターにアプリケーションをデプロイする場合は、前のセクションで既に述べたように、ネイティブ形式 (`.yaml` ファイル) に基づくデプロイ ファイルを使用することにより、元の `kubectl.exe` CLI ツールを使用できます。 ただし、複雑なマイクロサービス ベースのアプリケーションをデプロイする場合など、より複雑な Kubernetes アプリケーションについては、[Helm](https://helm.sh/) の使用をお勧めします。

Helm Chart を使用すると、非常に複雑な Kubernetes アプリケーションでも、その定義、バージョン管理、インストール、共有、アップグレード、またはロールバックを容易に行うことができます。

さらに述べると、Azure におけるその他の Kubernetes 環境 ([Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) など) も Helm Chart に基づいているので、Helm の使用が推奨されます。

Helm は、Microsoft、Google、Bitnami、および Helm の共同作成者コミュニティが協力し、[Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) が維持しています。

Helm Chart および Kubernetes の実装の詳細については、[Helm Chart を使用した AKS への eShopOnContainers のデプロイ](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.1-Deploying-to-AKS-using-Helm-Charts)に関する投稿記事を参照してください。

## <a name="use-azure-dev-spaces-for-you-kubernetes-application-lifecycle"></a>ご利用の Kubernetes アプリケーション ライフ サイクルに対して Azure Dev Spaces を使用する

[Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) には、チーム向けの迅速で反復的な Kubernetes 開発エクスペリエンスが用意されています。 開発マシンのセットアップ最小限にして、Azure Kubernetes Service (AKS) で直接、コンテナーの実行およびデバッグを反復的に実行することができます。 Visual Studio、Visual Studio Code、またはコマンド ラインなどの使い慣れたツールを使用して、Windows、Mac、または Linux 上で開発できます。

前述のように、Azure Dev Spaces では、コンテナー ベースのアプリケーションのデプロイに Helm Chart を使用します。

Azure Dev Spaces で、Visual Studio 2017 または Visual Studio Code を使用するだけで、Azure 内のグローバルな Kubernetes クラスターで直接、コードを迅速に反復およびデバッグすることができます。このため、Azure Dev Spaces は開発チームが Kubernetes での生産性を高めるのに役立ちます。 Azure 内の Kubernetes クラスターは共有されたマネージド Kubernetes クラスターであるため、チームは共同で作業を行うことができます。 コードを個別に開発してから、グローバル クラスターにデプロイし、依存関係のレプリケートやモックアップの作成なしで、他のコンポーネントでエンドツーエンド テストを実行することができます。

図 4-9 に示すように、Azure Dev Spaces の他と最も異なる機能は、クラスター内の残りのグローバル デプロイに運用可能な "スペース" を作成して統合できるということです。

![Azure Dev Spaces での複数のスペースの使用を示す図](./media/orchestrate-high-scalability-availability/use-multiple-spaces-azure-dev.png)

**図 4-9** Azure Dev Spaces での複数のスペースの使用

Azure Dev Spaces では、運用環境のマイクロ サービスと開発コンテナー インスタンスをさまざまな形で透過的に組み合わせることができます。 基本的には Azure 内に共有の開発スペースを設定することができます。 各開発者はアプリケーションの自分の担当部分にのみに集中でき、自分のシナリオで使用する他のすべてのサービスとクラウド リソースが既に含まれている開発スペースで、事前にコミットされているコードを繰り返し開発できます。 依存関係は常に最新の状態になります。開発者は運用環境を反映した方法で作業します。

Azure Dev Spaces には、スペースという概念があります。このスペースにより、チーム メンバーの邪魔をすることを心配せずに、隔離された状態で作業を行うことができます。 この機能は URL のプレフィックスに基づいているため、コンテナーの要求に対して URL 内の開発スペースのプレフィックスを使用する場合、Azure Dev Spaces ではそのスペースにデプロイされたコンテナーの特別なバージョンを実行します (存在する場合)。 それ以外の場合は、グローバル/統合されたバージョンが実行されます。

実際のビューの具体的な例を確認するには、[Azure Dev Spaces に関する eShopOnContainers Wiki ページ](https://github.com/dotnet-architecture/eShopOnContainers/wiki/10.2-Using-Azure-Dev-Spaces-and-AKS)を参照してください。

詳細については、「[Azure Dev Spaces を使用したチーム開発](https://docs.microsoft.com/azure/dev-spaces/team-development-netcore)」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

- **Azure Kubernetes Service (AKS) の使用の開始** \
  <https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal>

- **Azure Dev Spaces** \
  <https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces>

- **Kubernetes。** 公式サイト。 \
  <https://kubernetes.io/>

## <a name="using-azure-service-fabric"></a>Azure Service Fabric の使用

Azure Service Fabric は、通常はモノリシックなスタイルのボックス製品を提供していたマイクロソフトが、サービスを提供するよう変換したことから生まれました。 Azure SQL Database、Azure Cosmos DB、Azure Service Bus、Cortana のバックエンドなど、大規模なサービスを構築および運用していた経験が Service Fabric を形作りました。 プラットフォームは時間をかけてより多くのサービスを採用するように進化しました。 重要なのは、Service Fabric は、Azure 内だけでなく、スタンドアロン Windows Server の展開でも実行する必要があることです。

Service Fabric の目的は、サービスの構築と実行、インフラストラクチャ リソースの効率的な使用に関連する困難な問題を解決し、チームがマイクロサービス アプローチを使用してビジネスの問題を解決できるようにすることです。

Service Fabric は、マイクロサービス アプローチを使用するアプリケーションの構築に役立つ 2 つの広範な領域を提供します。

- 展開、拡張、アップグレード、検出のためのシステム サービス、失敗したサービスの再起動、サービスの場所の検出、状態の管理、正常性の監視を提供するプラットフォーム。 これらのシステム サービスによって、前に説明したマイクロサービスの特性の多くが実質的に有効になります。

- アプリケーションをマイクロサービスとして構築するために役立つプログラミング API またはフレームワーク: [Reliable Actors と Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-choose-framework)。 もちろん、マイクロサービスの構築にはどのようなコードも選択できますが、これらの API では作業がより簡単になり、より深いレベルでプラットフォームと統合できます。 この方法で正常性情報と診断情報を取得し、信頼できる状態管理を利用することができます。

Service Fabric は、サービスの構築方法に依存しないので、任意のテクノロジを使用することができます。 ただし、マイクロサービスをビルドしやすくする組み込みのプログラミング API を提供します。

図 4-10 に示すように、Service Fabric 内でマイクロサービスは、単純なプロセスまたは Docker コンテナーとして作成して実行することができます。 同じ Service Fabric クラスターには、コンテナーベースのマイクロサービスとプロセッサーベースのマイクロサービスを混在させることもできます。

![Azure Service Fabric クラスターの比較を示す図。](./media/orchestrate-high-scalability-availability/azure-service-fabric-cluster-types.png)

**図 4-10** Azure Service Fabric 内のプロセッサーまたはコンテナーとしてマイクロサービスを展開する

最初の画像では、プロセスとしてのマイクロサービスが示されています。各ノードでは、マイクロサービスごとに 1 つのプロセスが実行されます。 2 つ目の画像では、コンテナーとしてのマイクロサービスが示されています。各ノードでは、複数のコンテナー (マイクロサービスごとに 1 つのコンテナー) を含む Docker が実行されます。 Linux と Windows のホストに基づく Service Fabric クラスターは、Docker Linux コンテナーと Windows コンテナーをそれぞれ実行することができます。

Azure Service Fabric でのコンテナーのサポートの最新情報については、「[Service Fabric とコンテナー](https://docs.microsoft.com/azure/service-fabric/service-fabric-containers-overview)」を参照してください。

Service Fabric は、物理的な実装とは異なる論理アーキテクチャ (ビジネス マイクロサービスまたは境界指定されたコンテキスト) を定義できるプラットフォームの良い例です。 たとえば、次のセクションの「[ステートレス マイクロサービスとステートフル マイクロサービスの比較](#stateless-versus-stateful-microservices)」で紹介しているように、[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) に[ステートフル Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) を実装した場合、複数の物理的なサービスがあるビジネス マイクロサービスのコンセプトがあることになります。

図 4-10 に示すように、論理的/ビジネス マイクロサービスの観点から考えた場合、Service Fabric ステートフル Reliable Service を実装するには、通常 2 層のサービスを実装する必要があります。 1 つ目は、バックエンド ステートフル Reliable Service で、複数のパーティションを処理します (各パーティションはステートフル サービスです)。 2 つ目は、フロントエンド サービスまたはゲートウェイ サービスで、複数のパーティションまたはステートフル サービス インスタンス間のルーティングとデータの集計を担当します。 そのゲートウェイ サービスは、バックエンド サービスにアクセスする再試行のループが使用されたクライアント側の通信も処理します。 これは、カスタム サービスを実装している場合は、ゲートウェイ サービスと呼ばれます。または、そのままで使用できる Service Fabric の[リバース プロキシ サービス](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy)を使用することもできます。

![コンテナー内のいくつかのステートフル サービスを示す図。](./media/orchestrate-high-scalability-availability/service-fabric-stateful-business-microservice.png)

**図 4-11** いくつかのステートフルなサービス インスタンスとカスタム ゲートウェイ フロントエンドがあるビジネスのマイクロサービス

いずれの場合でも、Service Fabric ステートフル Reliable Service を使用する場合、通常は複数の物理サービスで構成される論理的/ビジネス マイクロサービス (境界指定されたコンテキスト) もあります。 図 4-11 に示すように、ゲートウェイ サービスおよびパーティションのそれぞれのサービスは ASP.NET Web API サービスとして実装できます。 Service Fabric には、コンテナーでいくつかのステートフル Reliable Service をサポートする方法があります。

Service Fabric では、サービスをグループ化して、サービスのグループを [Service Fabric アプリケーション](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model)として展開することができます。これは、オーケストレーターおよびクラスター用のパッケージ化および展開の単位です。 そのため、Service Fabric アプリケーションをこの自律的なビジネスおよび論理マイクロサービス境界または境界指定されたコンテキストにマッピングすることができ、さらにこれらのサービスを自律的に展開することもできます。

### <a name="service-fabric-and-containers"></a>Service Fabric とコンテナー

Service Fabric 内のコンテナーに関して、Service Fabric クラスター内のコンテナー イメージにサービスを展開することもできます。 図 4-12 に示すように、コンテナーは、多くの場合サービスごとに 1 つのみがあります。

![データベースにフィードするサービスごとに 1 つのコンテナーを示す図。](./media/orchestrate-high-scalability-availability/azure-service-fabric-business-microservice.png)

**図 4-12**. Service Fabric 内に複数のサービス (コンテナー) を持つビジネス マイクロサービス

Service Fabric アプリケーションは、外部データベースにアクセスする複数のコンテナーを実行でき、セット全体は、ビジネス マイクロサービスの論理的な境界になります。 ただし、いわゆる "サイドカー" コンテナー (論理サービスの一部としてまとめて展開する必要がある 2 つのコンテナー) は Service Fabric でも可能です。 重要な点は、ビジネス マイクロサービスが、いくつかのまとまりのある要素を囲む論理的な境界であることです。 これは、多くの場合、1 つのデータ モデルの 1 つのサービスかもしれませんが、場合によっては複数の物理的なサービスがある場合もあります。

図 4-13 に示すように、同じ Service Fabric アプリケーションにプロセスのサービスとコンテナーのサービスを混在させることができることに注意してください。

![同じアプリのプロセス内およびコンテナー内のサービスを示す図。](./media/orchestrate-high-scalability-availability/business-microservice-mapped-to-service-fabric-application.png)

**図 4-13**. コンテナーとステートフル サービスを含む Service Fabric アプリケーションにマップされているビジネス マイクロサービス

Azure Service Fabric でのコンテナーのサポートの詳細については、「[Service Fabric とコンテナー](https://docs.microsoft.com/azure/service-fabric/service-fabric-containers-overview)」を参照してください。

## <a name="stateless-versus-stateful-microservices"></a>ステートレス マイクロサービスとステートフル マイクロサービスの比較

前述のように、各マイクロサービス (論理的な境界指定されたコンテキスト) は、ドメイン モデル (データとロジック) を所有している必要があります。 ステートレス マイクロサービスの場合、データベースはサービス外部に用意することになります。このデータベースには、SQL Server などのリレーショナル オプション、または Azure Cosmos DB や MongoDB などの NoSQL データベースを使用できます。

Service Fabric でサービス自体もステートフルにして、データをマイクロサービス内に常駐させることもできます。 このデータは、同じサーバー上に存在できるだけでなく、マイクロサービスのプロセス内であれば、メモリ内に存在することも、ハード ドライブに保存することも、他のノードにレプリケートすることもできます。 図 4-14 では、別の方法を示します。

![ステートレス サービスとステートフル サービスの比較を示す図。](./media/orchestrate-high-scalability-availability/stateless-vs-stateful-microservices.png)

**図 4-14**. ステートレス マイクロサービスとステートフル マイクロサービスの比較

ステートレス サービスでは、状態 (永続化、データベース) はマイクロサービス外で保持されます。 ステートフル サービスでは、状態はマイクロサービス内で保持されます。 ステートレス アプローチは、完全に有効で、従来の既知のパターンに似ているので、ステートフルなマイクロサービスよりも容易に実装できます。 ステートレス マイクロサービスは、プロセスおよびデータ ソース間の遅延を強制します。 キャッシュとキューを追加してパフォーマンスを改善しようとしている場合は、移動する部分も増えます。 結果として、階層の数が多すぎる複雑なアーキテクチャになる可能性があります。

一方、[ステートフルなマイクロサービス](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction#when-to-use-reliable-services-apis)は、ドメイン ロジックとデータの間に遅延が生じないため、高度なシナリオに優れています。 負荷の高いデータ処理、ゲームのバックエンド、サービスとしてのデータベースなど、あまり遅延が許されないシナリオでは、ローカルな状態で高速にアクセスできるステートフル サービスのメリットを生かすことができます。

ステートレス サービスおよびステートフル サービスは相互に補完します。 たとえば、図 4-14 の右の図でわかるように、ステートフル サービスを複数のパーティションに分割できます。 それらのパーティションにアクセスするには、パーティション キーに基づく各パーティションへのアドレス指定方法を知っているゲートウェイ サービスとして、ステートレス サービスが動作する必要があります。

ステートフル サービスには欠点があります。 スケールアウトする場合、非常に複雑です。通常外部データベース システムによって実装される機能は、ステートフル マイクロサービスにまたがるレプリケーションや、データのパーティション分割などの機能として対処する必要があります。 ただし、[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-platform-architecture) などのオーケストレーターとその[ステートフル Reliable Service](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction#when-to-use-reliable-services-apis) が最も役立つ領域の 1 つです。これにより、[Reliable Services API](https://docs.microsoft.com/azure/service-fabric/service-fabric-work-with-reliable-collections) と [Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction) を使用するステートフル マイクロサービスの開発とライフサイクルが簡素化されます。

ステートフル サービスを許可し、Actor パターンをサポートし、ビジネス ロジックとデータ間のフォールト トレランスと遅延を改善する他のマイクロサービス フレームワークは、Microsoft [Orleans](https://github.com/dotnet/orleans)、Microsoft Research、および [Akka.NET](https://getakka.net/) です。 両方のフレームワークは、現在 Docker のサポートを向上させています。

Docker コンテナーはそれ自体ステートレスなことに覚えておいてください。 ステートフル サービスを実装する場合は、前に説明した追加の規範的で高度なフレームワークの 1 つが必要です。

## <a name="using-azure-service-fabric-mesh"></a>Azure Service Fabric Mesh の使用

Azure Service Fabric Mesh は、開発者がインフラストラクチャをまったく管理せずに、ミッション クリティカルなアプリケーションをビルドすることができる完全に管理されたサービスです。 Service Fabric Mesh を使用すると、必要に応じて拡大縮小できる分散型マイクロサービス アプリケーションをビルドして安全に実行することができます。

図 4-15 のとおり、Service Fabric Mesh にホストされているアプリケーションは、その基礎になっているインフラストラクチャを考慮する必要なく、実行および拡大縮小できます。

![ローカル リポジトリから Service Fabric Mesh へのデプロイを示す図。](media/orchestrate-high-scalability-availability/deploy-microservice-containers-apps-service-fabric-mesh.png)

**図 4-15**. Service Fabric Mesh にマイクロサービス/コンテナー アプリケーションをデプロイする

Service Fabric Mesh の背後には、何千ものマシンで構成されるクラスターがあります。 クラスターのすべての操作は、開発者には表示されません。 ユーザーは、コンテナーをアップロードし、必要なリソース、可用性の要件、リソース制限を単純に指定するだけです。 Service Fabric Mesh は、アプリケーション開発で要求されたインフラストラクチャを自動的に割り当て、インフラストラクチャの障害を処理し、アプリケーションの高可用性を保証します。 ユーザーは、インフラストラクチャではなく、アプリケーションの正常性と応答性のみを考慮します。

詳細については、[Service Fabric Mesh のドキュメント](https://docs.microsoft.com/azure/service-fabric-mesh/)を参照してください。

## <a name="choosing-orchestrators-in-azure"></a>Azure でオーケストレーターを選ぶ

次の表では、ワークロードおよび OS が焦点を当てていることに応じて、どのオーケストレーターを使用するかを示します。

![Kubernetes と Service Fabric を比較する表の画像。](media/orchestrate-high-scalability-availability/orchestrator-selection-azure-guidance.png)

**図 4-16**. Azure でのオーケストレーターの選択のガイダンス

>[!div class="step-by-step"]
>[前へ](soa-applications.md)
>[次へ](deploy-azure-kubernetes-service.md)
