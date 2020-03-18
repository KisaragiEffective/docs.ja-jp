---
title: チュートリアルと技術的な概要
description: Azure クラウドおよび Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | チュートリアルと技術的な概要
ms.date: 04/28/2018
ms.openlocfilehash: 190b33c4307b09bab0543d481e66ac9328074a0d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69660884"
---
# <a name="walkthroughs-and-technical-get-started-overview"></a>チュートリアルと技術的な概要

この電子書籍のサイズを制限するために、追加の技術ドキュメントと完全なチュートリアルが GitHub リポジトリで利用できるようになりました。 この章で説明する一連のオンライン チュートリアルでは、Windows コンテナーに基づく複数の環境の設定、および Azure へのデプロイに関する詳細な手順が網羅されています。

次のセクションでは、各チュートリアルの内容、その目的、およびおおまかなビジョンについて説明し、必要とされるタスクを図示しています。 チュートリアル自体は、<https://github.com/dotnet-architecture/eShopModernizing/wiki> にある *eShopModernizing* アプリ GitHub リポジトリ Wiki で入手できます。

## <a name="technical-walkthrough-list"></a>テクニカル チュートリアルの一覧

以下の入門チュートリアルでは、コンテナーを使用してリフト アンド シフトしてから Azure で複数のデプロイ オプションを使用して移動することができ、サンプル アプリについて一貫性のある包括的な技術ガイダンスを提供します。

以下の各チュートリアルでは、<https://github.com/dotnet-architecture/eShopModernizing> の GitHub で入手可能な新しいサンプル アプリ eShopLegacy および eShopModernizing を使用します。

- **eShop レガシ アプリのツアー (最新化するベースライン アプリ)**

- **Windows コンテナーを使用して既存の ASP.NET Web アプリ (WebForms & MVC) をコンテナー化する**

- **Windows コンテナーを使用して既存の WCF サービス (N 層アプリ) をコンテナー化する**

- **Windows コンテナーベースのアプリを Azure VM にデプロイする**

- **Windows コンテナーベースのアプリを Azure Container Service の Kubernetes にデプロイする**

## <a name="walkthrough-1-tour-of-eshop-legacy-apps"></a>チュートリアル 1: eShop レガシ アプリのツアー

### <a name="technical-walkthrough-availability"></a>テクニカル チュートリアルの可用性

完全なテクニカル チュートリアルについては、eShopModernizing GitHub リポジトリ Wiki を参照してください。

[eShopModernizing Wiki のチュートリアル](https://github.com/dotnet-architecture/eShopModernizing/wiki)

### <a name="overview"></a>概要

このチュートリアルでは、3 つのサンプル レガシ アプリケーションの初期実装について説明します。 最初の 2 つのサンプル Web アプリは、モノリシック アーキテクチャを備えており、従来の ASP.NET を使用して作成されています。 1 つのアプリケーションは ASP.NET 4.x MVC をベースにしています。2 番目のアプリケーションは、ASP.NET 4.x Web Forms をベースにしています。
3 番目のアプリケーションは、クライアントの WinForms アプリとサーバー側の [Windows Communication Foundation (WCF)](../../framework/wcf/whats-wcf.md) サービスによって構成される 3 層のアプリです。

これらのアプリケーションはすべて、[eShopModernizing GitHub リポジトリ](https://github.com/dotnet-architecture/eShopModernizing) で入手できます。

### <a name="goals"></a>目的

このチュートリアルの主な目的は、これらのアプリと、そのコードおよび構成についてよく理解することです。 テスト目的で、SQL データベースを使用せずに、モック データを生成および使用するようにアプリを構成することができます。 この省略可能な構成は、分離された方法での依存関係の挿入に基づいています。

### <a name="scenario-1-aspnet-web-apps"></a>シナリオ 1:ASP.NET Web アプリ

次の図は、元のレガシ ASP.NET Web アプリケーションのシンプルなシナリオを示しています。

![元のレガシ ASP.NET Web アプリケーションのシンプルなアーキテクチャ シナリオ](./media/image5-1.png)

ビジネス ドメインの観点から見ると、どちらのアプリも同じカタログ管理機能を備えています。 eShop エンタープライズ チームのメンバーの場合は、アプリを使用して、製品カタログを表示および編集します。

次の図は、初期のアプリのスクリーンショットを示しています。

![ASP.NET MVC と ASP.NET Web Forms アプリケーション (既存/レガシ テクノロジ)](./media/image5-2.png)

ASP.NET 4.x またはそれ以前のバージョンでの依存関係 (MVC または Web Forms に対する) は、ASP.NET Core MVC を使用してコードを完全に書き直さない限り、これらのアプリケーションが .NET Core 上で実行されないことを意味します。

### <a name="scenario-2-wcf-service-and-winforms-client-app-3-tier-app"></a>シナリオ 2: WCF サービスと WinForms クライアント アプリ (3 層アプリ)

次の図は、元の 3 層レガシ アプリケーションのシンプルなシナリオを示しています。

![WCF サービスと WinForms クライアント アプリを含む元のレガシ 3 層アプリのシンプルなアーキテクチャ シナリオ](./media/image5-1.5.png)

### <a name="benefits"></a>利点

このチュートリアルの利点は、シンプルです。コードと初期アプリについて理解を深めることができます。

### <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください。

- [ベースライン ASP.NET MVC と Web Forms "レガシ" アプリに関するツアー](https://github.com/dotnet-architecture/eShopModernizing/wiki/01.-Tour-on-the-ASP.NET-MVC-and-WebForms-apps-implementation-code)
- [ベースライン WCF サービスと WinForms (3 層) "レガシ" アプリに関するツアー](https://github.com/dotnet-architecture/eShopModernizing/wiki/21.-Tour-on-the-WCF-service-and-WinForms-apps)

## <a name="walkthrough-2-containerize-your-existing-net-applications-with-windows-containers"></a>チュートリアル 2: Windows コンテナーを使用して既存の .NET アプリケーションをコンテナー化する

### <a name="overview"></a>概要

Windows コンテナーを使用して、運用環境、開発環境、テスト環境への既存の .NET アプリケーション (MVC、Web Forms、WCF に基づくものなど) のデプロイを改善します。

### <a name="goals"></a>目的

このチュートリアルの目的は、既存の .NET Framework アプリケーションをコンテナー化するためのいくつかのオプションを示すことです。 次の操作を行うことができます。

- [Visual Studio 2017 Tools for Docker](/aspnet/core/host-and-deploy/docker/visual-studio-tools-for-docker) (Visual Studio 2017 以降のバージョン) を使用して、ご利用のアプリケーションをコンテナー化します。

- [Dockerfile](https://docs.docker.com/engine/reference/builder/) を手動で追加してから、[Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) を使用することで、ご利用のアプリケーションをコンテナー化します。

- [Image2Docker](https://github.com/docker/communitytools-image2docker-win) ツール (Docker からのオープンソース ツール) を使用して、ご利用のアプリケーションをコンテナー化します。

このチュートリアルでは、Visual Studio 2017 Tools for Docker による手法に焦点を当てていますが、Dockerfile の使用に関しては他の 2 つの手法もよく似ています。

### <a name="scenario-1-containerized-aspnet-web-apps"></a>シナリオ 1:コンテナー化された ASP.NET Web アプリ

次の図は、コンテナー化された eShop レガシ Web Apps アプリケーションのシナリオを示しています。

![開発環境でのコンテナー化された ASP.NET アプリケーションのアーキテクチャの概略図](./media/image5-3.png)

### <a name="scenario-2-containerized-wcf-service"></a>シナリオ 2: コンテナー化された WCF サービス

次の図は、コンテナー化された WCF サービスを使用した 3 層アプリケーションのシナリオを示しています。

![開発環境でのコンテナー化された WCF サービスのアーキテクチャの概略図](./media/image5-3.5.png)

### <a name="benefits"></a>利点

ご利用のモノシリック アプリケーションをコンテナー内で実行する利点があります。 まず、アプリケーションのイメージを作成します。 その時点から、すべてのデプロイが同じ環境で実行されます。 すべてのコンテナーは、同じ OS バージョンが使用され、同じバージョンの依存関係がインストールされており、同じ .NET フレームワーク バージョンが使用されることに加え、同じプロセスを使用してビルドされます。 基本的に、ご利用のアプリケーションの依存関係は、Docker イメージを使用して制御します。 コンテナーを展開すると、依存関係はアプリケーションと共に移動します。

さらに、開発者は Windows コンテナーによって提供される一貫性のある環境でアプリケーションを実行できるという利点もあります。 特定のバージョンでのみ発生する問題は、ステージング環境または運用環境で表面化するのではなく、すぐに発見できます。 アプリケーションをコンテナーで実行する場合、開発チームのメンバーが使用する開発環境の違いは、あまり問題ではありません。

また、コンテナー化されたアプリケーションではスケールアウトのカーブは、より平坦です。 コンテナー化されたアプリを使用すると、コンピューターごとに行われる通常のアプリケーションのデプロイと比較して、VM または物理マシンに、より多くの (コンテナーに基づく) アプリケーションとサービスのインスタンスを用意できます。 このことは、特に Kubernetes のようなオーケストレーター使用する場合に、密度がより高くなり、必要なリソースが減少することを意味します。

コンテナー化の場合、理想的な状況では、アプリケーション コード (C\#) に変更を加える必要はありません。 ほとんどのシナリオでは、Docker デプロイ メタデータ ファイル (Dockerfile および Docker Compose ファイル) が必要なだけです。

### <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください。

- [Windows コンテナーと Docker を使用して .NET Framework Web アプリをコンテナー化する方法](https://github.com/dotnet-architecture/eShopModernizing/wiki/02.-How-to-containerize-the-.NET-Framework-web-apps-with-Windows-Containers-and-Docker)
- [WCF サービスへの Docker サポートの追加](https://github.com/dotnet-architecture/eShopModernizing/wiki/22.-Adding-Docker-Support)

## <a name="walkthrough-3-deploy-your-windows-containers-based-app-to-azure-vms"></a>チュートリアル 3: Windows コンテナーベースのアプリを Azure VM にデプロイする

### <a name="technical-walkthrough-availability"></a>テクニカル チュートリアルの可用性

完全なテクニカル チュートリアルについては、eShopModernizing GitHub リポジトリ Wiki (<https://github.com/dotnet-architecture/eShopModernizing/wiki/06.-Deploying-your-Windows-Containers-based-app-into-Azure-VMs-(Including-CI-CD)>) を参照してください。

### <a name="overview"></a>概要

Azure 内の Windows Server 2016 仮想マシン (VM) 上の Docker ホストにデプロイすると、開発/テスト/ステージングの環境をすばやく設定することができます。 また、それによって、テスト担当者またはビジネス ユーザーがアプリを検証するための共通の場所も提供されます。 VM は、サービスとしてのインフラストラクチャ (IaaS) 運用環境としても有効です。

### <a name="goals"></a>目的

このチュートリアルの目的は、Windows Server 2016 以降のバージョンに基づく Azure VM に Windows コンテナーをデプロイする場合に使用できる複数の選択肢を紹介することです。

### <a name="scenarios"></a>シナリオ

このチュートリアルでは、いくつかのシナリオについて説明します。

#### <a name="scenario-a-deploy-to-an-azure-vm-from-a-dev-pc-through-docker-engine-connection"></a>シナリオ A: Docker エンジン接続を介して開発用 PC から Azure VM にデプロイする

![Docker エンジン接続を介して開発用 PC から Azure VM にデプロイする](./media/image5-4.png)

**図 5-4** Docker エンジン接続を介して開発用 PC から Azure VM にデプロイする

#### <a name="scenario-b-deploy-to-an-azure-vm-through-a-docker-registry"></a>シナリオ B: Docker レジストリを介して Azure VM にデプロイする

![Docker レジストリを介して Azure VM にデプロイする](./media/image5-5.png)

**図 5-5** Docker レジストリを介して Azure VM にデプロイする

#### <a name="scenario-c-deploy-to-an-azure-vm-from-cicd-pipelines-in-azure-devops-services"></a>シナリオ C: Azure DevOps Services の CI/CD パイプラインから Azure VM にデプロイする

![Azure DevOps Services の CI/CD パイプラインから Azure VM にデプロイする](./media/image5-6.png)

**図 5-6** Azure DevOps Services の CI/CD パイプラインから Azure VM にデプロイする

### <a name="azure-vms-for-windows-containers"></a>Windows コンテナー用の Azure VM

Windows コンテナー用の Azure VM は、Windows Server 2016 以降または Windows 10 以降のバージョン (両方とも Docker エンジンが設定されている) に基づく VM です。 ほとんどの場合、Azure VM では Windows Server 2016 が使用されます。

Azure では現在、**Windows Server 2016 with Containers** という名前の VM が提供されています。 この VM を使用すれば、Windows Server Core または Windows Nano Server のいずれかで、新しい Windows Server コンテナー機能を試すことができます。 コンテナー OS イメージがインストールされると、VM を Docker で使用する準備が整います。

### <a name="benefits"></a>利点

Windows コンテナーはオンプレミスの Windows Server 2016 VM にもデプロイできますが、Azure にデプロイすれば、すぐに使用できる Windows Server コンテナー VM で簡単に作業を開始することができます。 また、テスト担当者がアクセスできる共通のオンライン場所や、Azure 仮想マシン スケール セットを介した自動スケーラビリティも得られます。

### <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください。

<https://github.com/dotnet-architecture/eShopModernizing/wiki/06.-Deploying-your-Windows-Containers-based-app-into-Azure-VMs-(Including-CI-CD)>

## <a name="walkthrough-4-deploy-your-windows-containers-based-apps-to-azure-container-instances-aci"></a>チュートリアル 4: ご利用の Windows コンテナーベースのアプリを Azure Container Instances (ACI) にデプロイする

### <a name="technical-walkthrough-availability"></a>テクニカル チュートリアルの可用性

完全なテクニカル チュートリアルについては、eShopModernizing GitHub リポジトリ Wiki を参照してください。

[ACI (Azure Container Instances) へのアプリのデプロイ](https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances))

### <a name="overview"></a>概要

[Azure Container Instances (ACI)](https://docs.microsoft.com/azure/container-instances/) は、コンテナーの単一インスタンスをデプロイできるコンテナー開発/テスト/ステージング環境を作成する上で最も簡単な方法です。

### <a name="goals"></a>目的

このチュートリアルでは、Windows コンテナーを Azure Container Instances (ACI) にデプロイする場合の主なシナリオと、eShopModernizing アプリを ACI にデプロイする方法について説明します。

### <a name="scenarios"></a>シナリオ

ACI への eShopModernizing アプリのデプロイについては、1 つだけ、またはすべてのアプリ (MVC アプリ、WebForms アプリ、WCF サービス) のデプロイなど、さまざまなバリエーションが考えられます。 次に示すシナリオでは、ASP.NET MVC アプリと SQL Server コンテナーの両方が、コンテナーとして ACI (Azure Container Instances) にデプロイされるのを確認できます。

![開発環境から ACI にデプロイする](./media/image5-3.5.6.png)

### <a name="benefits"></a>利点

Azure Container Instances を使用すると、仮想マシンをプロビジョニングしたり、より高度なレベルのサービスを採用したりしなくても、Azure の Docker コンテナーを簡単に作成、管理できます。 ACI を使用すると、Windows コンテナーを Azure に直接デプロイし、完全修飾ドメイン名 (FQDN) を使用して数秒でインターネットに公開することができます (Docker Hub などの Docker レジストリまたは Azure Container Registry 内に Windows コンテナー イメージが用意されている場合)。

### <a name="considerations"></a>注意事項

完全な .NET Framework/ASP.NET または SQL Server のいずれかを使用した Windows コンテナーの Azure Container Instances (ACI) へのデプロイは、通常の Docker ホスト (Windows コンテナーを備えた Windows Server 2016 など) へのデプロイほど高速ではありません。これは、Docker イメージを毎回ダウンロード (Docker レジストリから取得) する必要があり、SQL コンテナー イメージ (15.1 GB) と ASP.NET コンテナー イメージ (13.9 GB) のサイズが非常に大きいためです。しかし前者は、独自の Docker ホスト (Azure 内の Windows コンテナー VM を使用した永続的なオンライン Windows Server 2016)、あるいは Azure 内の Kubernetes (AKS) のような全体的なオーケストレーターを維持するよりもはるかに安価であることは言うまでもありません (ただし、AKS は運用環境のデプロイには最適です)。

主な結論として、Azure Container Instances を使用することは、開発/テスト シナリオおよび CI/CD パイプラインにおいて非常に魅力的なオプションです。

## <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください。

[https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances)](https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances))

## <a name="walkthrough-5-deploy-your-windows-containers-based-apps-to-kubernetes-in-azure-container-service"></a>チュートリアル 5: Windows コンテナーベースのアプリを Azure Container Service の Kubernetes にデプロイする

### <a name="technical-walkthrough-availability"></a>テクニカル チュートリアルの可用性

完全なテクニカル チュートリアルについては、eShopModernizing GitHub リポジトリ Wiki を参照してください。

<https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

### <a name="overview"></a>概要

Windows コンテナーに基づくアプリケーションでは、IaaS VM からさらに離れた場所でも、プラットフォームをすばやく使用する必要があります。 これは、高いスケーラビリティと自動化された優れたスケーラビリティを容易に実現するため、さらに自動化されたデプロイとバージョン管理を大幅に改善するために必要です。 これらの目的を達成するには、[Azure Container Services](https://azure.microsoft.com/services/container-service/) で利用できるオーケストレーター [Kubernetes](https://kubernetes.io/) を使用します。

### <a name="goals"></a>目的

このチュートリアルの目的は、Windows コンテナーベースのアプリケーションを、Azure Container Service の Kubernetes (*K8s* とも呼ばれます) にデプロイする方法について説明することです。 一からの Kubernetes へのデプロイは、次の 2 段階のプロセスとなります。

1. Azure Container Service に Kubernetes クラスターをデプロイする。

2. アプリケーションおよび関連リソースを Kubernetes クラスターにデプロイする。

### <a name="scenarios"></a>シナリオ

#### <a name="scenario-a-deploy-directly-to-a-kubernetes-cluster-from-a-dev-environment"></a>シナリオ A: 開発環境から Kubernetes クラスターに直接デプロイする

![開発環境から Kubernetes クラスターに直接デプロイする](./media/image5-7.png)

**図 5-7** 開発環境から Kubernetes クラスターに直接デプロイする

#### <a name="scenario-b-deploy-to-a-kubernetes-cluster-from-cicd-pipelines-in-azure-devops-services"></a>シナリオ B: Azure DevOps Services の CI/CD パイプラインから Kubernetes クラスターにデプロイする

![Azure DevOps Services の CI/CD パイプラインから Kubernetes クラスターにデプロイする](./media/image5-8.png)

**図 5-8** Azure DevOps Services の CI/CD パイプラインから Kubernetes クラスターにデプロイする

### <a name="benefits"></a>利点

Kubernetes のクラスターにデプロイすることには多くの利点があります。 最大の利点は、使用するコンテナー インスタンスの数 (既存のノードの内部スケーラビリティ) に基づいて、およびクラスター内のノードまたは VM の数 (クラスターのグローバルなスケーラビリティ) に基づいてアプリケーションをスケールアウトできる実稼働可能な環境が得られることです。

Azure Container Service では、広く普及しているオープンソース ツールとテクノロジが Azure 専用に最適化されます。 コンテナーとアプリケーションの構成の両方に移植性を提供するオープン ソリューションが得られます。 サイズ、ホストの数、オーケストレーター ツールを選択すると、Container Service によって他のすべてが処理されます。

Kubernetes を使用することで、開発者は物理マシンおよび仮想マシンについて考えることから、特に次の機能を容易にするコンテナー中心のインフラストラクチャを計画することへと進歩をはかることができます。

- 複数のコンテナーに基づくアプリケーション

- コンテナー インスタンスのレプリケーションと水平方向の自動スケーリング

- 名前付けと検出 (たとえば、内部 DNS)

- 負荷分散

- ローリング アップデート

- シークレットの配布

- アプリケーションの正常性チェック

## <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください: <https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

## <a name="walkthrough-6-deploy-your-windows-containers-based-apps-to-azure-app-service-for-containers"></a>チュートリアル 6: ご利用の Windows コンテナーベースのアプリをコンテナー用の Azure App Service にデプロイする

### <a name="technical-walkthrough-availability"></a>テクニカル チュートリアルの可用性

完全なテクニカル チュートリアルについては、eShopModernizing GitHub リポジトリ Wiki を参照してください。

<https://github.com/dotnet-architecture/eShopModernizing/wiki/Deploy-Windows-Container-to-Azure-App-Service>

### <a name="overview"></a>概要

Windows コンテナーを使用してコンテナー化されたシンプルなアプリケーションは、コンテナー用の Azure App Service に容易にデプロイできます。 これは、ほとんどの Windows コンテナーベースのアプリケーションに推奨される方法です。

### <a name="goals"></a>目的

このチュートリアルの目的は、Windows コンテナーベースのアプリケーションを、レジストリ (Docker Hub または Azure Container Registry) からコンテナー用の Azure App Service にデプロイする方法について説明することです。

### <a name="scenario"></a>シナリオ

![コンテナー用の Azure App Service に Windows コンテナーベースのアプリをデプロイする](./media/image5-11.png)

### <a name="benefits"></a>利点

コンテナー用の Azure App Service にデプロイすると、コンテナーの利点と Azure App Service の PaaS の利点とが組み合わされます。 アプリ サービスを、垂直方向と水平方向の両方に簡単にスケーリングできると共に、変化する需要に合わせて自動スケーリングが行われるように構成することができます。 更新はダウンタイムなしで実行することができます。レジストリからの継続的デプロイの構成も簡単に構成することができます。

## <a name="next-steps"></a>次の手順

このコンテンツの詳細については、GitHub Wiki をご覧ください: <https://github.com/dotnet-architecture/eShopModernizing/wiki/Deploy-Windows-Container-to-Azure-App-Service>

> [!div class="step-by-step"]
> [前へ](modernize-existing-apps-to-cloud-optimized/migrate-to-hybrid-cloud-scenarios.md)
> [次へ](conclusions.md) <!-- Next Chapter -->
