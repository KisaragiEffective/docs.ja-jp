---
title: Docker アプリケーションの外側のループ DevOps ワークフローの手順
description: DevOps の "外部ループ" ワークフローの手順について学習する
ms.date: 02/15/2019
ms.openlocfilehash: 735f92c00cd6279649ec3b0c35cfb00543f21a8c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75936778"
---
# <a name="steps-in-the-outer-loop-devops-workflow-for-a-docker-application"></a>Docker アプリケーションの外側のループ DevOps ワークフローの手順

図 5-1 は、DevOps の外部ループ ワークフローを構成するエンド ツー エンド手順を表したものです。 これは DevOps の "外側のループ" を示します。 コードがリポジトリにプッシュされると、CI パイプラインが開始されてから CD パイプラインが開始され、そこでアプリケーションが配置されます。 デプロイされたアプリケーションから収集されたメトリックは開発ワークロードにフィードバックされ、そこで "内部ループ" が発生します。したがって、開発チームは実際のデータを得てユーザーおよびビジネスのニーズに応えることができます。

![DevOps の外側のループ ワークフローの 6 つの手順を示す図。](./media/docker-application-outer-loop-devops-workflow/overview-dev-ops-outter-loop-workflow.png)

**図 5-1**. Microsoft ツールを使用する Docker アプリケーションの DevOps 外部ループ ワークフロー

それでは、以下の手順をそれぞれ詳しく見ていきましょう。

## <a name="step-1-inner-loop-development-workflow"></a>手順 1: 内部ループの開発ワークフロー

この手順は第 4 章で詳しく説明されていますが、要約するため、ここでは外部ループが開始され、その時点で開発者は CI パイプライン アクションを開始するソース コントロール管理システム (Git など) にコードをプッシュします。

## <a name="step-2-source-code-control-integration-and-management-with-azure-devops-services-and-git"></a>手順 2: Azure DevOps Services と Git を使用するソース コード コントロールの統合と管理

この手順では、チーム内のさまざまな開発者からの統合されたバージョンのコードをすべて収集するために、バージョン管理システムが必要になります。

ほとんどの開発者がソース コード コントロール (SCC) とソース コード管理に慣れているように見えても、DevOps ライフ サイクルで Docker アプリケーションを作成するときに、開発者のコンピューターからグローバルな Docker Registry (Azure Container Registry や Docker Hub など) に直接、Docker イメージをアプリケーションとともに送信してはいけないことを強調することが重要です。 一方、リリースして運用環境にデプロイする Docker イメージは、ソース コード リポジトリ (Git など) に基づいてグローバル ビルドまたは CI パイプラインで統合される、ソース コードでのみ作成する必要があります。

開発者によって生成される、ローカル イメージは、独自のコンピューター内でテストするときにのみ、使用される必要があります。 これが、SCC コードから DevOps パイプラインをアクティブ化することが重要である理由です。

Azure DevOps Services および Team Foundation Server では、Git と Team Foundation バージョン管理がサポートされます。 いずれかを選択し、それをエンド ツー エンドの Microsoft エクスペリエンスで使用することができます。 しかし、外部リポジトリ (GitHub、オンプレミスの Git リポジトリ、Subversion など) でコードを管理することもでき、引き続き、それに接続し、DevOps CI パイプラインの開始点としてコードを取得することができます。

## <a name="step-3-build-ci-integrate-and-test-with-azure-devops-services-and-docker"></a>手順 3: Azure DevOps Services と Docker を使用するビルド、CI、統合、およびテスト

CI は、最新のソフトウェアのテストおよび配布のための標準として登場しました。 Docker ソリューションでは、開発および運用チーム間の懸念事項を明確に分離させておくことができます。 Docker イメージの不変性により、CI を介して開発し、テストし、運用環境で実行する対象の間で反復可能なデプロイが保証されます。 開発者のノート PC とテスト インフラストラクチャ全体にデプロイされた Docker エンジンにより、環境間でのコンテナーの移植が可能になります。

ここで、正しいコードが送信され、バージョン管理システムの準備ができた後、コードを選択してグローバル ビルドとテストを実行するための "*ビルドサービス*" が必要になります。

この手順 (CI、ビルド、テスト) の内部ワークフローは、コード リポジトリ (Git など)、ビルド サーバー (Azure DevOps Services)、Docker エンジン、および Docker レジストリで構成される CI パイプラインの構築に関するものです。

アプリケーションのビルドと CI パイプラインの設定のため、およびビルドされた "成果物" の "成果物リポジトリ" へのプッシュのための基盤として、Azure DevOps Services を使用できます。これについては、次の手順で説明します。

デプロイで Docker を使用する場合、デプロイされる "最終的な成果物" は、その中に埋め込まれているアプリケーションまたはサービスを含む Docker イメージです。 これらのイメージは、*Docker レジストリ* (Azure Container Registry で使用できるものなどのプライベート リポジトリ、または公式の基本イメージで一般的に使用される、Docker Hub Registry などのパブリックのもの) にプッシュまたは公開されます。

基本的な概念を以下に示します。CI パイプラインは、Git などの SCC へのコミットによって開始されます。 図 5-2 に示すように、コミットにより、Azure DevOps Services では Docker コンテナー内でビルド ジョブが実行され、そのジョブが正常に完了すると、Docker イメージが Docker レジストリにプッシュされるようになります。 外部ループの最初の部分は手順 1 から 3、つまり、コード、実行、デバッグ、検証、その後のコード リポジトリ、ビルドとテスト CI 手順までが関連します。

![CI ワークフローに関連する 3 つの手順を示す図](./media/docker-application-outer-loop-devops-workflow/continuous-integration-steps.png)

**図 5-2** CI に関連する手順

Docker と Azure DevOps Services を使用する基本的な CI ワークフロー手順を以下に示します。

1. 開発者は、SCC リポジトリ (Git/Azure DevOps Services、GitHub など) にコミットをプッシュします。

2. Azure DevOps Services または Git を使用する場合、CI は組み込まれています。つまり、Azure DevOps Services でチェック ボックスをオンにするのと同じくらいシンプルです。 外部 SCC (GitHub など) を使用する場合は、`webhook` によって、Azure DevOps Services に更新プログラムが通知されるか、Git/GitHub にプッシュされます。

3. Azure DevOps Services では、アプリケーションとテスト コードだけでなく、イメージを記述する Dockerfile を含む、SCC リポジトリがプルされます。

4. Azure DevOps Services によって Docker イメージがビルドされ、ビルド番号でラベルが付けられます。

5. Azure DevOps Services では、プロビジョニング済みの Docker ホスト内で Docker コンテナーがインスタンス化され、適切なテストが実行されます。

6. テストに成功した場合、イメージはまず、わかりやすい名前にラベルが書き換えられるため、それが "blessed ビルド" ("/1.0.0" やその他のラベルなど) であることがわかります。その後、Docker レジストリ (Docker Hub、Azure Container Registry、DTR など) までプッシュされます。

### <a name="implementing-the-ci-pipeline-with-azure-devops-services-and-the-docker-extension-for-azure-devops-services"></a>Azure DevOps Services および Azure DevOps Services 用の Docker 拡張機能を使用する CI パイプラインの実装

Visual Studio の Azure DevOps Services にはビルドとリリース テンプレートが含まれており、これらを CI/CD パイプラインで使用でき、Docker イメージをビルドしたり、認証済みの Docker レジストリに Docker イメージをプッシュしたり、Docker CLI によって提供される他の操作を実行したりすることができます。 また、Docker Compose タスクが追加されます。これを使用して、図 5-3 に示すように、複数コンテナーの Docker アプリケーションをビルド、プッシュ、実行したり、Docker Compose CLI によって提供される他の操作を実行したりすることができます。

![Azure DevOps の Docker CI パイプラインのスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/docker-ci-pipeline-azure-devops.png)

**図 5-3** ビルドとリリース テンプレートおよび関連するタスクを含む、Azure DevOps Service の Docker CI パイプライン。

これらのテンプレートとタスクを使用して、Azure Service Fabric、Azure Kubernetes Service、および同様のオファリングでビルド/テストおよびデプロイのための CI/CD 成果物を構築することができます。

これらの Visual Studio Team Services タスク、Azure でプロビジョニングされたビルド Linux-Docker ホスト/VM、および任意の Docker レジストリ (Azure Container Registry、Docker Hub、プライベート Docker DTR、またはその他の Docker レジストリ) では、非常に一貫した方法で Docker CI パイプラインをアセンブルすることができます。

***要件:***

- Azure DevOps Services。オンプレミス インストールの場合は、Team Foundation Server 2015 Update 3 以降。

- Docker バイナリを含む Azure DevOps Services エージェント。

  これらのエージェントのいずれかを作成する簡単な方法は、Docker を使用し、Azure DevOps Services エージェントの Docker イメージに基づいてコンテナーを実行することです。

> [!情報] Azure DevOps Services Docker CI パイプラインのアセンブルの詳細を確認し、チュートリアルを表示する場合は、これらのサイトにアクセスしてください。
>
> - Docker コンテナーとしての Visual Studio Team Services (現在の Azure DevOps Services) エージェントの実行: \
>   <https://hub.docker.com/_/microsoft-azure-pipelines-vsts-agent>
>
> - Azure DevOps Services を使用する .NET Core Linux Docker イメージのビルド: \
>   <https://docs.microsoft.com/archive/blogs/stevelasker/building-net-core-linux-docker-images-with-visual-studio-team-services>
>
> - Docker サポートを利用する Linux ベースの Visual Studio Team Service ビルド コンピューターのビルド: \
>   <http://donovanbrown.com/post/2016/06/03/Building-a-Linux-Based-Visual-Studio-Team-Service-Build-Machine-with-Docker-Support>

### <a name="integrate-test-and-validate-multi-container-docker-applications"></a>複数コンテナーの Docker アプリケーションを統合、テスト、および確認する

通常、ほとんどの Docker アプリケーションは、単一のコンテナーではなく、複数のコンテナーで構成されます。 その良い例が、マイクロサービスごとに 1 つのコンテナーを使用する、マイクロサービス指向のアプリケーションです。 しかし、マイクロサービスのアプローチ パターンに厳密に従わなくても、Docker アプリケーションが複数のコンテナーまたはサービスで構成される可能性があります。

そのため、CI パイプラインでアプリケーション コンテナーをビルドした後、統合 Docker ホスト内、さらにはコンテナーが分散されるテスト クラスター内に、そのすべてのコンテナーを含めまとめてアプリケーションをデプロイ、統合、およびテストする必要もあります。

単一のホストを使用する場合は、docker-compose などの Docker コマンドを使用して、関連するコンテナーをビルドおよびデプロイし、単一の VM で Docker 環境をテストおよび確認することができます。 しかし、DC/OS、Kubernetes、Docker Swarm などのオーケストレーター クラスターを操作する場合は、選択したクラスター/スケジューラに応じて、異なるメカニズムまたはオーケストレーターを介してコンテナーをデプロイする必要があります。

Docker コンテナーに対して実行できるいくつかの種類のテストを以下に示します。

- Docker コンテナーの単体テスト

- 相互に関連するアプリケーションまたはマイクロサービスのグループ テスト

- 運用および "カナリア" リリースでのテスト

重要な点は、統合および機能テストを実行するときに、コンテナーの外部からそれらのテストを実行する必要があることです。 デプロイするコンテナーではテストは含まれず、実行されません。これは、コンテナーが、運用環境にデプロイするのとまったく同じである必要がある静的イメージに基づくためです。

いくつかのクラスター (テスト クラスター、ステージング クラスター、運用クラスター) を含む、より高度なシナリオをテストする場合の実用的なオプションは、イメージをレジストリに公開することです。これにより、さまざまなクラスターでテストすることができます。

### <a name="push-the-custom-application-docker-image-into-your-global-docker-registry"></a>グローバルな Docker レジストリにカスタム アプリケーションの Docker イメージをプッシュする

Docker イメージがテストされ、確認された後、それらにタグを付け、Docker レジストリに公開する必要があります。 Docker レジストリは Docker アプリケーション ライフ サイクルにおける重要な部分です。これは、QA および運用環境にデプロイされる ("blessed イメージ" ともいう) カスタム テストを格納する中央の場所であるためです。

SCC リポジトリ (Git など) に格納されているアプリケーション コードが "信頼できるソース" であるのと同様、Docker リポジトリは QA または運用環境にデプロイされるバイナリ アプリケーションあるいはビットの "信頼できるソース" です。

通常、Azure Container Registry 内のプライベート リポジトリまたは Docker Trusted Registry などのオンプレミス レジストリ、あるいは (Docker Hub などの) アクセスが制限されたパブリック クラウド レジストリでは、カスタム イメージ用のプライベート リポジトリが必要な場合があります。ただし、最後のケースでは、コードがオープンソースでない場合、ベンダーのセキュリティを信頼する必要があります。 いずれの場合も、使用する方法は似ており、図 5-4 に示すように、`docker push` コマンドに基づきます。

![カスタム イメージをコンテナー レジストリにプッシュする方法を示す図。](./media/docker-application-outer-loop-devops-workflow/docker-push-custom-images.png)

**図 5-4** カスタム イメージの Docker レジストリへの公開

ビルド、統合およびテスト (CI) に関する手順 3 では、結果の Docker イメージをプライベートまたはパブリック レジストリに公開する場合があります。 Azure Container Registry、Amazon Web Services Container Registry、Google Container Registry、Quay レジストリなど、クラウド ベンダーからの Docker レジストリには複数のオファリングがあります。

図 5-5 に示すように、Docker タスクを使用することで、`docker-compose.yml` ファイルで定義されている、複数のタグの付いた一連のサービス イメージを、認証済みの Docker レジストリ (Azure Container Registry など) にプッシュすることができます。

![レジストリにイメージを発行する手順を示すスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/publish-custom-image-to-docker-registry.png)

**図 5-5** Azure DevOps Services を使用する Docker レジストリへのカスタム イメージの公開

> [!情報] Azure Container Registry の詳細については、<https://aka.ms/azurecontainerregistry> を参照してください。

## <a name="step-4-cd-deploy"></a>手順 4: CD、デプロイ

Docker イメージの不変性により、CI を介して開発し、テストし、運用環境で実行する対象で反復可能なデプロイが保証されます。 Docker レジストリ (プライベートまたはパブリック) でアプリケーションの Docker イメージを公開した後、Azure DevOps Services パイプライン タスクまたは Azure DevOps Services Release Management を使って、CD パイプラインから使用する可能性のあるいくつかの環境 (運用、QA、ステージングなど) にデプロイすることができます。

しかし、この時点では、これはデプロイする Docker アプリケーションの種類によって異なります。 いくつかのコンテナーまたはサービスを構成し、いくつかのサーバーまたは VM にデプロイされたモノシリック アプリケーションなど、(構成およびデプロイの観点から) シンプルなアプリケーションをデプロイすることは、ハイパースケール機能を備えたマイクロサービス指向のアプリケーションのようなより複雑なアプリケーションをデプロイすることとは異なります。 これら 2 つのシナリオについては、次のセクションで説明します。

### <a name="deploying-composed-docker-applications-to-multiple-docker-environments"></a>複数の Docker 環境への構成された Docker アプリケーションのデプロイ

最初に、単一の環境または複数の環境 (QA、ステージング、運用) でシンプルな Docker ホスト (VM またはサーバー) にデプロイする、あまり複雑ではないシナリオを見ていきましょう。 このシナリオでは、図 5-6 に示すように、内部的に CD パイプラインで (Azure DevOps Services デプロイ タスクから) docker-compose を使用して、Docker アプリケーションをそれに関連する一連のコンテナーまたはサービスと共にデプロイすることができます。

![3 つの環境にデプロイする CD デプロイ手順を示す図](./media/docker-application-outer-loop-devops-workflow/deploy-app-containers-to-docker-host-environments.png)

**図 5-6**. シンプルな Docker ホスト環境レジストリへのアプリケーション コンテナーのデプロイ

図 5-7 では、[タスクの追加] ダイアログ ボックスで [Docker Compose] をクリックし、Azure DevOps Services を介して QA/テスト環境にビルド CI を接続する方法に焦点を当てています。 しかし、ステージングまたは運用環境にデプロイする場合は、通常、複数の環境 (QA、ステージング、および運用) を処理する Release Management 機能を使用します。 単一 Docker のホストにデプロイする場合は、Azure DevOps Services の "Docker Compose" タスク (内部での `docker-compose up` コマンドに関連する) が使用されます。 Azure Kubernetes Service (AKS) にデプロイする場合は、以降のセクションで説明されているように、Docker デプロイ タスクが使用されます。

![Docker Compose タスクの [タスクの追加] ダイアログを示すスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/add-tasks-docker-compose.png)

**図 5-7** Azure DevOps Services パイプラインでの Docker Compose タスクの追加

Azure DevOps Services でリリースを作成するときに、一連の入力成果物が取得されます。 これらの成果物は、すべての環境全体で、リリースの有効期間に対して不変であることが想定されます。 コンテナーを導入するときに、入力成果物によってデプロイするレジストリ内のイメージが識別されます。 これらのイメージの識別方法によっては、リリース期間を通して同じままであることが保証されません。これが最も明白に現れるのが、`docker-compose` ファイルから `myimage:latest` を参照する場合です。

Azure DevOps Services テンプレートでは、同じイメージ バイナリを一意に識別することが保証される特定のレジストリ イメージ ダイジェストを含む、ビルド成果物を生成することができます。 これらは、リリースへの入力として使用する必要があるものです。

### <a name="managing-releases-to-docker-environments-by-using-azure-devops-services-release-management"></a>Azure DevOps Services Release Management を使用する Docker 環境に対するリリースの管理

図 5-8 に示すように、Azure DevOps Services テンプレートを使用することで、新しいイメージをビルドし、それを Docker レジストリに公開し、Linux または Windows ホストで実行し、`docker-compose` などのコマンドを使って、複数の環境を対象とした Azure DevOps Services Release Management 機能を通じて、アプリケーション全体として複数のコンテナーをデプロイすることができます。

![Docker Compose リリースの構成を示すスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/configure-docker-compose-release.png)

**図 5-8**. Azure DevOps Services Release Management からの Azure DevOps Services Docker Compose タスクの構成

しかし、図 5-6 に示され、図 5-8 で実装されるシナリオはシンプルなものであり (単一 Docker のホストと VM にデプロイされ、コンテナーまたはインスタンスがイメージごとに 1 つとなる)、開発またはテスト シナリオでのみ使用する必要がある場合があることに注意してださい。 ほとんどのエンタープライズ運用シナリオでは、複数のノード、サーバー、および VM 間で負荷を分散し、さらに "インテリジェントなフェールオーバー" により、高可用性 (HA) および管理しやすいスケーラビリティを実現する必要があります。そのため、サーバーまたはノードで障害が発生した場合、そのサービスとコンテナーは別のホスト サーバーまたは VM に移動されます。 その場合、コンテナー クラスター、オーケストレーター、スケジューラなどのより高度なテクノロジが必要になります。 したがって、これらのクラスターにデプロイする方法は、高度なシナリオを処理することです。これについては、次のセクションで説明します。

### <a name="deploying-docker-applications-to-docker-clusters"></a>Docker クラスターへの Docker アプリケーションのデプロイ

分散アプリケーションの性質上、コンピューティング リソースが必要であり、これらも分散されます。 運用スケールの機能を備えるには、プールされたリソースに基づいて高いスケーラビリティと高可用性を提供するクラスタリング機能が必要です。

CLI ツールまたは Web UI からこれらのクラスターに手動でコンテナーをデプロイすることはできますが、スケールアウトや監視などのデプロイ テストまたは管理の目的を特定するために、この種の手動による作業を予約する必要があります。

図 5-9 に示すように、CD の観点から、また、Azure DevOps Services では特に、Container Service で分散クラスターにコンテナー化されたアプリケーションをデプロイする Azure DevOps Services Release Management 環境から特別に作成されたデプロイ タスクを実行できます。

![オーケストレーターにデプロイする CD デプロイ手順を示す図。](./media/docker-application-outer-loop-devops-workflow/cd-deploy-to-orchestrators.png)

**図 5-9** Container Service への分散アプリケーションのデプロイ

最初は、特定のクラスターまたはオーケストレーターにデプロイするときに、通常、`docker-compose.yml` 定義ファイルに基づくよりシンプルで使いやすい `docker-compose` ツールではなく、オーケストレーターごとに特定のデプロイ スクリプトとメカニズムを使用します (つまり、Kubernetes と Service Fabric ではデプロイ メカニズムが異なる)。 しかし、図 5-10 に示すように、Azure DevOps Services Docker Deploy タスクのおかげで、現在、使い慣れた `docker-compose.yml` ファイルのみを使用して、サポートされているオーケストレーターにデプロイすることもできます。これは、ツールでユーザーに代わって、(`docker-compose.yml` ファイルから、オーケストレーターで必要な形式への) "変換" が行われるためです。

![Kubernetes へのデプロイ タスクを示すスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/add-deploy-to-kubernetes-task.png)

**図 5-10** Kubernetes へのデプロイ タスクの環境への追加

図 5-11 では、構成で使用可能なセクションを使用して Kubernetes へのデプロイ タスクをどのように編集できるかを示します。 これは、クラスター内のコンテナーとしてデプロイされる、すぐに使用できるカスタム Docker イメージを取得するタスクです。

![Kubernetes へのデプロイ タスクの構成を示すスクリーンショット。](./media/docker-application-outer-loop-devops-workflow/edit-deploy-to-kubernetes-task.png)

**図 5-11** ACS DC/OS にデプロイする、Docker デプロイ タスクの定義

> [!情報] Azure DevOps Services と Docker を使用する CD パイプラインの詳細については、<https://azure.microsoft.com/services/devops/pipelines> を参照してください

## <a name="step-5-run-and-manage"></a>手順 5: 実行と管理

エンタープライズ運用レベルでのアプリケーションの実行と管理はそれ自体が主題であるため、また、そのレベル (IT 操作) で作業するユーザーと操作の種類およびこの領域の大きなスコープに起因し、次の章全体がこれについての説明となります。

## <a name="step-6-monitor-and-diagnose"></a>手順 6: 監視と診断

このトピックについては、運用システムで IT 担当者によって行われるタスクの一部として次の章でも説明されています。しかし、この手順で得られた洞察を開発チームにフィードバックし、アプリケーションが常に改善されるようにする必要があることを強調することが重要です。 その観点から、これは DevOps の一部でもありますが、タスクと操作は一般的に IT 担当者によって実行されます。

監視と診断が DevOps の領域内で 100% である場合にのみ、テストまたはベータ環境に対して、開発チームによって監視プロセスと分析が行われます。 これは、ロード テストを実行することで、あるいはベータ テスターが新しいバージョンを試す、ベータまたは QA 環境を監視することで行われます。

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](create-ci-cd-pipelines-azure-devops-services-aspnetcore-kubernetes.md)
