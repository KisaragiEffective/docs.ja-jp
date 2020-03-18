---
title: ASP.NET Core Web アプリ用の Azure ホスティングの推奨事項
description: ASP.NET Core および Azure での最新の Web アプリケーションの設計 | ASP.NET Web アプリ用の Azure ホスティングの推奨事項
author: ardalis
ms.author: wiwagn
ms.date: 06/06/2019
ms.openlocfilehash: 5587b8b20da8a6801d77b722e9c3326f6e695574
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73416711"
---
# <a name="azure-hosting-recommendations-for-aspnet-core-web-apps"></a>ASP.NET Core Web アプリ用の Azure ホスティングの推奨事項

> "基幹業務のリーダーはどこでも、IT 部門を通さずにクラウドからアプリケーションにアクセスし (別名 SaaS)、雑誌を購読するかのように料金を支払っています。 そして、サービスが不要になったら、無駄なくサブスクリプションを取り消すことができます。"  
> _\- Daryl Plummer、Gartner アナリスト_

Microsoft Azure なら、アプリケーションのニーズとアーキテクチャに応じたサポートが可能です。 ホスティングのニーズは、静的な Web サイトや多数のサービスで構成される高度なアプリケーションと同じくらいシンプルなものになります。 ASP.NET Core のモノリシックな Web アプリケーションとサポートされるサービスには、推奨されるいくつかのよく知られている構成が含まれています。 この記事の推奨事項は、完全なアプリケーション、個別のプロセス、またはデータであっても、ホスティングされるリソースの種類に基づいてグループ化されています。

## <a name="web-applications"></a>Web アプリケーション

Web アプリケーションは次を使用してホスティングできます。

- App Service Web Apps

- コンテナー (複数のオプション)

- 仮想マシン (VM)

このうち、シンプルなコンテナー ベースのアプリを含む、ほとんどのシナリオでお勧めできる手法が App Service Web Apps です。 マイクロサービス アーキテクチャでは、コンテナー ベースの方法を検討してください。 アプリケーションを実行しているマシンをさらに制御する必要がある場合は、Azure Virtual Machines を検討してください。

### <a name="app-service-web-apps"></a>App Service Web Apps

App Service Web Apps は、Web アプリケーションのホスティングに最適化されたフル マネージド プラットフォームを提供します。 これは、サービスとしてのプラットフォーム (PaaS) 製品です。Azure がアプリの実行とスケーリングに必要なインフラストラクチャに対処するため、ビジネス ロジックに集中することができます。 App Service Web Apps の主な機能:

- DevOps の最適化 (継続的インテグレーションと継続的配信、複数の環境、A/B テスト、スクリプトのサポート)

- グローバルなスケーリングと高可用性

- SaaS プラットフォームとオンプレミス データへの接続

- セキュリティとコンプライアンス

- Visual Studio の統合

Azure App Service は、ほとんどの Web アプリに適しています。 デプロイと管理機能がそのプラットフォームに統合され、トラフィックの負荷に応じてサイトのスケールを機敏に調整できるほか、組み込みの負荷分散機能と Traffic Manager によって高い可用性が得られます。 オンライン移行ツールを使用して既存のサイトを簡単に Azure App Service に移動したり、Web アプリケーション ギャラリーからオープンソース アプリを使用したり、選択したフレームワークとツールを使用して新しいサイトを作成したりできます。 Web ジョブ機能により、バックグラウンド ジョブ処理を App Service Web アプリに簡単に追加できます。 ローカル データベースを使用して既に ASP.NET アプリケーションがオンプレミスでホストされている場合、Azure SQL Database でアプリを App Service Web アプリに移行するため (または、希望する場合、オンプレミスのデータベース サーバーに安全にアクセスするため) の明確なパスがあります。

![推奨されるオンプレミスの .NET アプリの Azure App Service への移行戦略](./media/image1-6.png)

ほとんどの場合、ローカルでホストされる ASP.NET アプリから App Service Web アプリへの移行は簡単なプロセスです。 アプリ自体にほとんどまたはまったく変更の必要がなく、Azure App Service Web Apps によって提供される多くの機能をすぐに活用し始めることができます。

クラウド向けに最適化されていないアプリだけでなく、Azure App Service Web Apps は、多くの ASP.NET Core アプリのように、多くのシンプルなモノリシック (非分散) アプリケーションにとって優れたソリューションです。 この手法では、アーキテクチャは基本的であり、簡単に理解して管理できます。

![基本的な Azure アーキテクチャ](./media/image1-5.png)

このようなアプリを管理するには、通常、1 つのリソース グループ内の少数のリソースで十分です。 多くの個々のプロセスで構成されるアプリではなく、通常、1 つのユニットとしてデプロイされるアプリがこの[基本的なアーキテクチャの手法](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)に適しています。 アーキテクチャはシンプルですが、この手法では、ホストされたアプリで、増加する需要を満たすように、スケールアップ (ノードことにリソースの増加) とスケールアウト (ホストされるノードの増加) の両方を引き続き行うことができます。 自動スケーリングを使用して、必要に応じて、また、ノード全体での平均負荷に基づいて、アプリをホストするノード数を自動的に調整するようにアプリを構成できます。

### <a name="app-service-web-apps-for-containers"></a>App Service Web Apps for Containers

直接 Web アプリをホストするサポートに加えて、[App Service Web Apps for Containers](https://azure.microsoft.com/services/app-service/containers/) は、Windows と Linux 上でのコンテナー化されたアプリケーションの実行に使用できます。 このサービスを使用すると、自分のビジネスに合わせてサイズを変更できる、コンテナー化されたアプリケーションを簡単にデプロイして実行できます。 アプリには、上記にある App Service Web Apps のすべての機能が備わっています。 さらに、Web Apps for Containers では、Docker Hub、Azure Container Registry、GitHub を使用した効率的な CI/CD をサポートします。 Azure DevOps を使用して、レジストリに変更を発行するビルド パイプラインとデプロイ パイプラインを定義できます。 これらの変更は、ステージング環境でテストでき、ダウンタイムなしで更新できるデプロイ スロットを使用して本番環境に自動的にデプロイできます。 以前のバージョンへのロールバックも簡単に実行できます。

Web Apps for Containers が最も役立つシナリオがいくつかあります。 コンテナー化できる既存のアプリがある場合、Windows コンテナーか Linux コンテナーのいずれであっても、このツールセットを使用してこれらを簡単にホストできます。 単にコンテナーを発行して、Web Apps for Containers を構成し、選択したレジストリからイメージの最新バージョンをプルします。 これはクラシック アプリ ホスティング モデルからクラウドに最適化されたモデルに移行するための "リフト アンド シフト" 手法です。

![コンテナー化されたオンプレミスの .NET アプリケーションを Azure Web Apps for Containers に移行する](./media/image1-8.png)

また、この手法は、開発チームがコンテナー ベースの開発プロセスに移行できる場合にも適しています。 コンテナーを含む開発中のアプリの "内部ループ" には、コンテナーを含むアプリのビルドも含まれます。 コードおよびコンテナーの構成に行われた変更はソース管理にプッシュされ、自動化されたビルドには Docker Hub または Azure Container Registry などのレジストリに新しいコンテナー イメージを発行する責任があります。 次の図に示すように、これらのイメージは追加の開発、および本番環境へのデプロイに対する基本として使用されます。

![エンドツーエンドの Docker DevOps ライフサイクル ワークフロー](./media/image1-7.png)

コンテナーでの開発は、特にコンテナーが本番環境で使用されるときに多くの利点が提供されます。 同じコンテナー構成は、システムをビルドしてテストするローカル開発コンピューターから本番環境まで、実行する各環境でアプリをホストするために使用されます。 これにより、コンピューターの構成またはソフトウェア バージョンの違いから生じる障害の可能性が大幅に削減されます。 また、コンテナーは任意の OS で実行できるため、開発者はオペレーティング システムを含む、最も生産的なツールを使用することもできます。 場合によっては、多くのコンテナーに関連する配布されるアプリケーションは、1 つの開発コンピューターで実行するためにリソースを非常に集中的に使用する場合があります。 このシナリオでは、次のセクションで説明される、Kubernetes と Azure Dev Spaces を使用してアップグレードするのが便利な場合があります。

大規模なアプリケーションの一部がそれぞれのより小さい部分に分割されるとき (独立した*マイクロサービス*)、追加の設計パターンをアプリの動作を向上させるために使用できます。 個別のサービスを直接操作する代わりに、*API ゲートウェイ*によってアクセスを簡素化し、そのバックエンドからクライアントを切り離すことができます。 さまざまなフロントエンドに対して個別のサービス バックエンドがある場合、サービスによってコンシューマーとの連携を進化させることができます。 一般的なサービスには、個別の "*サイドカー*" コンテナーを介してアクセスできます。これには "*アンバサダー*" パターンを使用する一般的なクライアント接続ライブラリが含まれる可能性があります。

![複数の一般的な設計パターンが示されたマイクロサービスのサンプル アーキテクチャ。](./media/image1-10.png)

[マイクロサービス ベースのシステムをビルドするときに考慮する設計パターンを参照してください。](https://docs.microsoft.com/azure/architecture/microservices/design/patterns)

### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Azure Kubernetes Service (AKS) は、ホストされている Kubernetes 環境を管理するサービスで、コンテナー オーケストレーションの専門知識がなくても、コンテナー化したアプリケーションのデプロイと管理をすばやく簡単に行うことができます。 また、アプリケーションをオフラインにすることなく、要求に応じてリソースをプロビジョニング、アップグレード、スケーリングすることにより、実行中の操作およびメンテナンスの負担もなくなります。

AKS では、Azure に対する責任の多くをオフロードすることによって、Kubernetes クラスターの管理における複雑な運用のオーバーヘッドを削減します。 ホストされている Kubernetes サービスとして、Azure では、正常性の監視や保守などの重要なタスクが処理されます。 また、マスターではなく、クラスター内のエージェント ノードのみの料金を支払います。 Managed Kubernetes サービスとして、AKS は次の機能を提供します。

- 自動化された Kubernetes バージョンのアップグレードと修正。
- 簡単なクラスターのスケーリング。
- 自己復旧のホストされているコントロール プレーン (マスター)。
- コストの削減 - 実行中のエージェント プール ノードの料金のみを支払う。

AKS クラスター内のノードの管理は Azure が処理するので、管理者は、クラスターのアップグレードなどの多くのタスクを手動で実行する必要がありません。 Azure ではこれらの重要な保守タスクが処理されるため、AKS ではクラスターへの直接アクセスを提供しません (SSH の使用など)。

AKS を活用しているチームは、Azure Dev Spaces の利点を利用することもできます。 Azure Dev Spaces を利用すると、チームは AKS で実行されているアプリケーションやマイクロサービス アーキテクチャ全体を直接取り扱えるようになるため、マイクロサービス アプリケーションに関する開発と迅速な反復作業に集中できます。 Azure Dev Spaces では、残りの AKS クラスターまたはその他の開発者に影響を与えることなく、ご利用のマイクロサービス アーキテクチャの一部を分離させて個別に更新する方法も提供されます。

![Azure Dev Spaces ワークフローの例](./media/image1-9.gif)

Azure Dev Spaces:

- ローカル コンピューターの準備時間とリソース要件を最小限にする
- チームがより迅速に反復処理することを許可する
- チームが必要とする統合環境の数を減らす
- 開発/テスト時に分散システムで特定のサービスを模倣する必要性を取り除く

[Azure Dev Spaces の詳細を参照してください](https://docs.microsoft.com/azure/dev-spaces/about)

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

既存のアプリケーションにおいて App Service で実行するための大幅な変更が必要になった場合は、クラウドへの移行を簡略化するために Virtual Machines を選択できます。 ただし、Azure App Service と比較すると、VM の構成、セキュリティ保護、保守を正しく行うには、さらなる時間と IT の専門知識が必要になります。 Azure Virtual Machines を検討している場合は、現在の VM 環境のパッチ適用、更新、管理に必要な、継続的な保守作業を考慮してください。 Azure 仮想マシンはサービスとしてのインフラストラクチャ (IaaS) であり、App Service は PaaS です。 また、アプリを Windows Container として Web App for Containers にデプロイすることが、シナリオにとって有効なオプションになり得るかどうかも検討する必要があります。

## <a name="logical-processes"></a>論理プロセス

アプリケーションから分離できる個々の論理プロセスは、Azure Functions に関係なく "サーバーレス" な方法でデプロイできます。 Azure Functions では、アプリケーションまたはインフラストラクチャの実行を気にせずに、特定の問題で必要なコードのみを記述できます。 C\#、F\#、Node.js、Python、PHP などのさまざまなプログラミング言語を選択できるため、当面のタスクに対して最も生産性の高い言語を使用できます。 ほとんどのクラウドベースのソリューションと同様、使用した時間に対してのみ料金を支払います。Azure Functions は必要に応じて確実にスケール アップされます。

## <a name="data"></a>Data

Azure はさまざまなデータ記憶域のオプションを提供しているため、アプリケーションが対象となるデータに適したデータ プロバイダーを使用できます。

トランザクションに基づくリレーショナル データの場合、Azure SQL Database が最も適しています。 高パフォーマンスの読み取りが多くのデータの場合、Azure SQL Database に基づく Redis キャッシュが適しています。

構造化されていない JSON データは、SQL Database 列から Azure Storage の Blob またはテーブル、Azure Cosmos DB まで、さまざまな方法で格納できます。 このうち、Azure Cosmos DB がクエリ機能では最も優れており、クエリのサポートを必要とする JSON ベースのドキュメントが多数ある場合は、このオプションが推奨されます。

アプリケーションの動作を調整するために使用する一時的なコマンドベースの、またはイベントベースのデータでは、Azure Service Bus または Azure Storage キューを使用できます。 Azure Storage Bus はより柔軟性に優れているため、アプリケーション内、およびアプリケーション間で重要なメッセージをやり取りする場合に推奨されるサービスです。

## <a name="architecture-recommendations"></a>アーキテクチャに関する推奨事項

アプリケーションの要件で、アーキテクチャについて指示している必要があります。 利用できるさまざまな Azure サービスが多くあります。 適切なサービスを選ぶことが重要です。 Microsoft では、一般的なシナリオに最適化された典型的なアーキテクチャを特定できるように、参照アーキテクチャのギャラリーを提供しています。 アプリケーションの要件に最も近い、または少なくとも使い始めに適している参照アーキテクチャを見つけることができます。

図 11-1 は、参照アーキテクチャの例です。 この図は、マーケティングに最適化された Sitecore コンテンツ管理システム Web サイトに推奨されるアーキテクチャの手法を示しています。

![図 11-1](./media/image11-2.png)

**図 11-1** Sitecore マーケティング Web サイトの参照アーキテクチャ。

**参照 – Azure のホスティングの推奨事項**

- Azure ソリューション アーキテクチャ\
  <https://azure.microsoft.com/solutions/architecture/>

- Azure Basic Web アプリケーション アーキテクチャ\
  <https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app>

- マイクロサービスの設計パターン\
  <https://docs.microsoft.com/azure/architecture/microservices/design/patterns>

- Azure の開発者ガイド\
  <https://azure.microsoft.com/campaigns/developer-guide/>

- Web Apps の概要\
  <https://docs.microsoft.com/azure/app-service/app-service-web-overview>

- Web App for Containers\
  <https://azure.microsoft.com/services/app-service/containers/>

- Azure Kubernetes Service (AKS) の概要\
  <https://docs.microsoft.com/azure/aks/intro-kubernetes>

>[!div class="step-by-step"]
>[[戻る]](development-process-for-azure.md)
