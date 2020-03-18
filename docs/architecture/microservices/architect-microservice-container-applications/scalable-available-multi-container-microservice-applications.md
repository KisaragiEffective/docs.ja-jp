---
title: 高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する
description: Kubernetes アプリケーション ライフサイクルを開発しながら、高いスケーラビリティおよび可用性と Azure Dev Spaces の可能性のためにマイクロサービスと複数のコンテナー アプリケーションを調整するオプションについて説明します。
ms.date: 01/30/2020
ms.openlocfilehash: ea204941a461794fbeeb2482aa11973b79437027
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77628502"
---
# <a name="orchestrate-microservices-and-multi-container-applications-for-high-scalability-and-availability"></a>高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する

アプリケーションがマイクロサービスに基づく場合や、単に複数のコンテナーに分割されている場合、運用可能なアプリケーションのオーケストレーターを使用することが不可欠です。 前述のとおり、マイクロサービス ベースの方法では、マイクロサービスはそれぞれモデルとデータを所有しているため、開発と展開の観点から自立していることになります。 ただし、複数のサービスで構成される従来のアプリケーション (SOA など) を使用する場合でも、分散システムとして展開する必要がある単一のビジネス アプリケーションを構成する複数のコンテナーまたはサービスを使用する場合もあります。 この種のシステムはスケールアウトや管理が複雑であるため、運用可能でスケーラブルな複数のコンテナー アプリケーションを使用する場合、どうしてもオーケストレーターが必要になります。

図 4-23 は、複数のマイクロサービス (コンテナー) で構成されるアプリケーションのクラスターへの展開を示しています。

![クラスターで構成された Docker アプリケーションを示す図](./media/scalable-available-multi-container-microservice-applications/composed-docker-applications-cluster.png)

**図 4-23**. コンテナーのクラスター

サービス インスタンスごとに 1 つのコンテナーを使用します。 Docker コンテナーは "配置の単位" であり、コンテナーは Docker のインスタンスです。 ホストでは多くのコンテナーが処理されます。 これは論理的な方法のように見えます。 しかし、これらの構成されたアプリケーションの負荷分散、ルーティング、オーケストレーションはどのように処理するのでしょうか。

単一の Docker ホストのプレーン Docker エンジンは 1 つのホストでの単一のイメージ インスタンスの管理に関するニーズを満たしますが、より複雑な分散アプリケーションの複数のホストに展開されている複数のコンテナーの管理に関しては対応できません。 ほとんどの場合、管理プラットフォームが必要になります。このプラットフォームは自動的にコンテナーを開始し、イメージごとに複数のインスタンスでコンテナーをスケールアウトし、必要に応じて一時停止またはシャットダウンします。また、ネットワークおよびデータ ストレージなどのリソースへのアクセス方法を制御するのが理想的です。

個々のコンテナーまたは非常に単純な構成アプリを管理するだけでなく、マイクロサービスを使用してより大きなエンタープライズ アプリケーションを管理するには、オーケストレーションおよびクラスタリング プラットフォームが必要になります。

アーキテクチャと開発の観点から、マイクロサービス ベースのアプリケーションで構成される大規模なエンタープライズを構築する場合、高度なシナリオをサポートする次のプラットフォームと製品を理解することが重要です。

**クラスターとオーケストレーター。** 大規模なマイクロサービス ベースのアプリケーションの場合のように、多くの Docker ホストでアプリケーションをスケールアウトする必要がある場合は、基になるプラットフォームの複雑さを抽象化することで、これらすべてのホストを単一のクラスターとして管理できるようにすることが重要です。 それは、コンテナー クラスターとオーケストレーターによって実現されるものです。 Kubernetes はオーケストレーターの例であり、Azure Kubernetes Service を介して Azure で利用可能です。

**スケジューラ。** *スケジューリング* は、管理者がクラスターのコンテナーを起動して、UI も提供できるようにすることを意味します。 クラスター スケジューラにはいくつかの役割があります。たとえば、クラスターのリソースを効率的に使用すること、ユーザーによって指定される制約を設定すること、ノードまたはホスト全体でのコンテナーの負荷分散を効率的に行うこと、高可用性を提供すると同時にエラーに対して堅牢であることなどです。

クラスターとスケジューラの概念は密接に関連しており、さまざまなベンダーによって提供される製品では、多くの場合、両方の機能セットが提供されます。 以下のリストに、クラスターとスケジューラーで最も重要なプラットフォームとソフトウェアの選択肢を示します。 これらのオーケストレーターは、通常、Azure などのパブリック クラウドで提供されます。

## <a name="software-platforms-for-container-clustering-orchestration-and-scheduling"></a>コンテナーのクラスタリング、オーケストレーション、スケジューリングのためのソフトウェア プラットフォーム

|     |   |
|:---:|---|
| **Kubernetes** <br> ![Kubernetes ロゴの画像。](./media/scalable-available-multi-container-microservice-applications/kubernetes-container-orchestration-system-logo.png) | [*Kubernetes*](https://kubernetes.io/) はオープンソースの製品であり、クラスターのインフラストラクチャとコンテナーのスケジューリングからオーケストレーションまでのさまざまな機能を提供します。 ホストのクラスター全体のアプリケーション コンテナーの展開、スケーリング、操作を自動化できます。 <br><br> *Kubernetes* ではコンテナー中心のインフラストラクチャが提供され、アプリケーション コンテナーを論理ユニットにグループ化し、管理と検出を容易にします。 <br><br> *Kubernetes* は Linux では完成されていますが、Windows では未完成です。 |
| **Azure Kubernetes Service (AKS)** <br> ![Azure Kubernetes Service のロゴの画像。](./media/scalable-available-multi-container-microservice-applications/azure-kubernetes-service-logo.png) | [AKS](https://azure.microsoft.com/services/kubernetes-service/) は、Azure でのマネージド Kubernetes コンテナー オーケストレーション サービスです。これによって、Kubernetes クラスターの管理、デプロイ、操作が簡略化されます。 |

## <a name="using-container-based-orchestrators-in-microsoft-azure"></a>Microsoft Azure でのコンテナー ベースのオーケストレーターの使用

いくつかのクラウド ベンダーでは Docker コンテナーのサポートに加え、Docker クラスターおよびオーケストレーション サポート (Microsoft Azure、Amazon EC2 Container Service、Google Container Engine を含む) が提供されます。 Microsoft Azure では、Azure Kubernetes Service (AKS) を介して Docker クラスターとオーケストレーターのサポートが提供されます。

## <a name="using-azure-kubernetes-service"></a>Azure Kubernetes Service の使用

Kubernetes クラスターでは複数の Docker ホストがプールされ、それらが単一の仮想 Docker ホストとして公開されるため、複数のコンテナーをクラスターにデプロイし、任意の数のコンテナー インスタンスを使用してスケールアウトできます。 クラスターは、スケーラビリティや正常性など、複雑な管理機能をすべて処理します。

ACS には、コンテナー化されたアプリケーションを実行するように Azure 内で事前に構成されている仮想マシンのクラスターの作成、構成、および管理を簡略化する方法が用意されています。 一般的なオープンソースのスケジューリングおよびオーケストレーション ツールの最適化された構成を使用することで、AKS では、コンテナー ベースのアプリケーションを Microsoft Azure でデプロイして管理するための専門知識を持つ増加し続けるコミュニティを利用するか、既存のスキルを使用することができます。

Azure Kubernetes Service によって、一般的な Docker クラスタリング オープンソース ツールとテクノロジの構成が Azure 専用に最適化されます。 コンテナーとアプリケーションの構成の両方に移植性を提供するオープン ソリューションが得られます。 サイズ、ホストの数、オーケストレーター ツールを選択すると、他のことはすべて AKS によって処理されます。

![Kubernetes クラスター構造を示す図。](./media/scalable-available-multi-container-microservice-applications/kubernetes-cluster-simplified-structure.png)

**図 4-24** Kubernetes クラスターの簡略化された構造とトポロジ

図 4-24 に示したのは Kubernetes クラスターの構造です。ここでは、クラスターの調整の大部分がマスター ノード (VM) によって行われ、アプリケーションの観点から 1 つのプールとして管理される残りのノードにコンテナーをデプロイすることができます。これにより、数千または数万ものコンテナーに対するスケーリングを行うことができます。

## <a name="development-environment-for-kubernetes"></a>Kubernetes 用の開発環境

開発環境において、単に [Docker Desktop](https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel/) をインストールすることにより 1 台の開発マシン (Windows 10 または macOS) でも Kubernetes を実行できることが [2018 年 7 月に Docker から発表](https://docs.docker.com/install/)されました。 図 4-25 に示すように、さらなる統合テストを行うために、後でクラウド (AKS) にデプロイすることができます。

![開発マシン上にあり、AKS にデプロイされる Kubernetes を示す図](./media/scalable-available-multi-container-microservice-applications/kubernetes-development-environment.png)

**図 4-25** 開発マシンとクラウドでの Kubernetes の実行

## <a name="getting-started-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) の使用の開始

AKS の使用を開始するには、Azure portal から、または CLI を使用して AKS クラスターをデプロイします。 Azure の Kubernetes クラスター デプロイの詳細については、[Azure Kubernetes Service (AKS) クラスターのデプロイ](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)に関するページを参照してください。

AKS の一部として既定でインストールされるソフトウェアはいずれも無料です。 既定のすべてのオプションはオープン ソース ソフトウェアと共に実装されます。 AKS は Azure 内の複数の仮想マシンで使用できます。 選択したコンピューティング インスタンスと、ストレージやネットワーキングなどの利用された他の基になるインフラストラクチャ リソースにのみ課金されます。 AKS 自体に従量課金はありません。

Kubernetes 用の既定の運用環境デプロイ オプションでは、次のセクションで説明する Helm Chart が使用されます。

## <a name="deploy-with-helm-charts-into-kubernetes-clusters"></a>Helm Chart を使用した Kubernetes クラスターへのデプロイ

Kubernetes クラスターにアプリケーションをデプロイする場合は、前のセクションで既に述べたように、ネイティブ形式 (.yaml ファイル) に基づくデプロイ ファイルを使用することにより、元の kubectl.exe CLI ツールを使用できます。 ただし、複雑なマイクロサービス ベースのアプリケーションをデプロイする場合など、より複雑な Kubernetes アプリケーションについては、[Helm](https://helm.sh/) の使用をお勧めします。

Helm Chart を使用すると、非常に複雑な Kubernetes アプリケーションについても、その定義、バージョン管理、インストール、共有、アップグレード、またはロールバックを容易に行うことができます。

さらに述べると、Azure におけるその他の Kubernetes 環境 ([Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) など) も Helm Chart に基づいているので、Helm の使用が推奨されます。

Helm は、[Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) が維持しています (Microsoft、Google、Bitnami、および Helm の共同作成者コミュニティとの協力で)。

Helm Chart および Kubernetes の実装の詳細については、[Helm Chart を使用した AKS への eShopOnContainers のデプロイ](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS))に関する投稿記事を参照してください。

## <a name="use-azure-dev-spaces-for-your-kubernetes-application-lifecycle"></a>ご利用の Kubernetes アプリケーション ライフ サイクルに対して Azure Dev Spaces を使用する

[Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces) には、チーム向けの迅速で反復的な Kubernetes 開発エクスペリエンスが用意されています。 開発マシンのセットアップ最小限にして、Azure Kubernetes Service (AKS) で直接、コンテナーの実行およびデバッグを反復的に実行することができます。 Visual Studio、Visual Studio Code、またはコマンド ラインなどの使い慣れたツールを使用して、Windows、Mac、または Linux 上で開発します。

前述のように、Azure Dev Spaces では、コンテナー ベースのアプリケーションのデプロイ時に Helm Chart が使用されます。

Azure Dev Spaces では、Visual Studio 2019 または Visual Studio Code を使用するだけで、Azure 内のグローバルな Kubernetes クラスターで直接、コードを迅速に反復およびデバッグすることができます。このため、Azure Dev Spaces は開発チームが Kubernetes での生産性を高めるのに役立ちます。 Azure 内の Kubernetes クラスターは共有されたマネージド Kubernetes クラスターであるため、チームは共同で作業を行うことができます。 コードを個別に開発してから、グローバル クラスターにデプロイし、依存関係のレプリケートやモックアップの作成なしで、他のコンポーネントでエンドツーエンド テストを実行することができます。

図 4-26 に示すように、Azure Dev Spaces の他と最も異なる機能は、クラスター内の残りのグローバル デプロイに統合された運用可能な "スペース" を作成できるということです。

![Azure Dev Spaces での複数のスペースの使用を示す図](./media/scalable-available-multi-container-microservice-applications/use-multiple-spaces-azure-dev.png)

**図 4-26** Azure Dev Spaces での複数のスペースの使用

基本的には Azure 内に共有開発スペースを設定することができます。 各開発者はアプリケーションの自分の担当部分にのみに集中できます。自分のシナリオが依存する他のすべてのサービスとクラウド リソースが既に含まれている開発スペースで事前コミット コードを繰り返し開発できます。 依存関係は常に最新の状態になります。開発者は運用環境を反映した方法で作業します。

Azure Dev Spaces ではスペースの概念が提供されます。これにより、チームの作業の中断を心配することなく、相対的な分離された環境で作業できます。 各開発スペースは階層構造の一部です。この構造を使用すると、"最上位の" マスター開発スペースから、1 つ (または多数) のマイクロサービスを独自の進行中のマイクロサービスでオーバーライドできます。

この機能は URL プレフィックスに基づいているため、URL で何らかの開発スペース プレフィックスを使用しているとき、要求は、ターゲット マイクロサービスが開発スペース内に存在する場合はそのマイクロサービスから処理されます。そうでない場合、要求は階層内で見つかったターゲット マイクロサービスの最初のインスタンスに、最終的には最上位のマスター開発スペースに転送されます。

実際のビューの具体的な例を確認するには、[Azure Dev Spaces に関する eShopOnContainers Wiki ページ](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Azure-Dev-Spaces)を参照してください。

詳細については、「[Azure Dev Spaces を使用したチーム開発](https://docs.microsoft.com/azure/dev-spaces/team-development-netcore)」を参照してください。

## <a name="additional-resources"></a>その他のリソース

- **Azure Kubernetes Service (AKS) の使用の開始** \
  <https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal>

- **Azure Dev Spaces** \
  <https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces>

- **Kubernetes** 公式サイト。 \
  <https://kubernetes.io/>

>[!div class="step-by-step"]
>[前へ](resilient-high-availability-microservices.md)
>[次へ](../docker-application-development-process/index.md)
