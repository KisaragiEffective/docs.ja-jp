---
title: クラウド向けに最適化されたアプリケーションにおけるマイクロソフト テクノロジ
description: Azure Cloud および Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | クラウド向けに最適化されたアプリケーションの Microsoft テクノロジ
ms.date: 04/28/2018
ms.openlocfilehash: 915aa99d2331c5b9c46eabef3335fb809baa9370
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69578225"
---
# <a name="microsoft-technologies-in-cloud-optimized-applications"></a>クラウド向けに最適化されたアプリケーションにおける Microsoft テクノロジ

以下は、クラウド向けに最適化されたアプリの要件として認識されているツール、テクノロジ、ソリューションの説明一覧です。 優先順位に合わせて、クラウド向けに最適化された要素を選択的または段階的に導入できます。

- **クラウド インフラストラクチャ**:コンピューティング プラットフォーム、オペレーティング システム、ネットワーク、ストレージを提供するインフラストラクチャ。 Microsoft Azure のレベルはここです。

- **ランタイム**:この層では、アプリケーションを実行するための環境が与えられます。 コンテナーを使用している場合、この層は通常、Linux ホストまたは Windows ホスト上で実行されている [Docker エンジン](https://docs.docker.com/engine/)を基盤とします。 ([Windows コンテナー](https://docs.microsoft.com/virtualization/windowscontainers/about/) は Windows Server 2016 以降でサポートされています。 Windows コンテナーは、Windows で実行される既存の .NET Framework アプリケーションに最適な選択肢です)

- **マネージド クラウド**:マネージド クラウドを選択すると、基礎インフラストラクチャ、VM、OS パッチ、ネットワーク設定の管理やサポートのための費用と複雑な作業がなくなります。 IaaS を使用して移行することを選択した場合、こうした作業をすべて行わなければならず、関連費用も発生します。 マネージド クラウド オプションでは、自分で開発したアプリケーションとサービスのみを管理します。 クラウド サービス プロバイダーは通常、他のすべてを管理します。 Azure のマネージド クラウド サービスの例には、[Azure SQL Database](https://azure.microsoft.com/services/sql-database)、[Azure Redis Cache](https://azure.microsoft.com/services/cache/)、[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)、[Azure Storage](https://azure.microsoft.com/services/storage/)、[Azure Database for MySQL](https://azure.microsoft.com/services/mysql/)、[Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql/)、[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) に加え、[VM スケール セット](https://azure.microsoft.com/services/virtual-machine-scale-sets/)、[Azure App Service](https://azure.microsoft.com/services/app-service/)、[Azure Kubernetes Service](https://azure.microsoft.com/services/container-service/) などのマネージド コンピューティング サービスがあります。

- **アプリケーション開発**:コンテナーで実行されるアプリケーションを構築するとき、さまざまな言語から選択できます。 このガイドでは [.NET](https://www.microsoft.com/net) に焦点を当てていますが、Node.js、Python、Spring/Java、Go など、他の言語を使用してコンテナーベースのアプリを開発できます。

- **監視、テレメトリ、ログ、監査**:クラウドで実行されているアプリケーションとコンテナーを監視し、監査する機能は、クラウド最適化アプリケーションにとって非常に重要です。 [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) と [Microsoft Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) は、クラウド最適化アプリの監視/監査機能を提供する Microsoft のメイン ツールです。

- **プロビジョニング**:自動化ツールは、インフラストラクチャをプロビジョニングし、アプリケーションを複数の環境 (運用、テスト、ステージング) にデプロイするときに役立ちます。 Chef や Puppet のようなツールを利用し、アプリケーションの構成と環境を管理できます。 この層は、もっと単純で直接的な手法でも実装することができます。 たとえば、Azure コマンドライン インターフェイス (Azure CLI) ツールで直接デプロイし、[Azure DevOps Services](https://azure.microsoft.com/services/devops/) の継続的配置とリリース管理を利用できます。

- **アプリケーションのライフサイクル**:[Azure DevOps Services](https://azure.microsoft.com/services/devops/) と Jenkins のようなその他のツールは、リリース管理などの CI/CD パイプラインの実装を支援する組み込み自動化サーバーです。

この章の後続セクションと関連チュートリアルでは、ランタイム レイヤー (Windows コンテナー) に関する詳細を特に取り上げます。 このガイドでは、Windows Server 2016 (以降のバージョン) VM と Azure Container Instances に Windows コンテナーをデプロイする方法について説明しています。 Azure App Service など、さらに高度な PaaS プラットフォームや Azure Kubernetes Service のようなオーケストレーターも取り上げています。

## <a name="monolithic-applications-can-be-cloud-optimized"></a>モノリシック アプリケーションはクラウド向けに最適化*できる*

その重要性から強調しなくてはならないのが、モノリシック アプリケーション (マイクロサービスを基盤としないアプリケーション) はクラウド向けに最適化*できる*アプリケーションであるということです。 コンテナー、継続的デリバリー、DevOps の組み合わせを利用し、クラウド コンピューティング モデルを活用するモノリシック アプリケーションを構築し、運用することができます。 既存のモノリシック アプリケーションが事業目標に合っている場合、それを最新化し、クラウド向けに最適化できます。

同様に、モノリシック アプリケーションをクラウド向けに最適化できる場合、他の、n 層アプリケーションなど、さらに複雑なアーキテクチャをクラウド最適化アプリケーションとして最新化することもできます。

>[!div class="step-by-step"]
>[前へ](reasons-to-modernize-existing-net-apps-to-cloud-optimized-applications.md)
>[次へ](what-about-cloud-native-applications.md)
