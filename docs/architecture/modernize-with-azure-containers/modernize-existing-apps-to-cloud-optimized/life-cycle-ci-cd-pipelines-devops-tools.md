---
title: クラウドの CI/CD パイプラインや DevOps ツールでアプリのライフ サイクルを最新化する
description: Azure クラウドおよび Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | クラウドの CI/CD パイプラインと DevOps ツールを使用してアプリのライフサイクルを最新化する
ms.date: 04/30/2018
ms.openlocfilehash: 17a78c108bfc61471128a34191ec7a5d7cc28289
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503853"
---
# <a name="modernize-your-apps-lifecycle-with-cicd-pipelines-and-devops-tools-in-the-cloud"></a>クラウドの CI/CD パイプラインや DevOps ツールでアプリのライフ サイクルを最新化する

今日の企業は、市場での競争力を高めるために、迅速にイノベーションを行う必要があります。 高品質で最新のアプリケーションを提供するには DevOps ツールおよびプロセスが必要で、この繰り返されるイノベーション サイクルの実装に不可欠です。 適切な DevOps ツールを使用すると、開発者は継続的デプロイを効率化し、革新的なアプリケーションをより迅速にユーザーに届けることができます。

継続的インテグレーションとデプロイの方法は確立されていますが、コンテナーを導入することにより、複数コンテナー アプリケーションを使用している場合は特に、新たな考慮事項が生じます。

Azure DevOps Services では、Azure DevOps Services の公式のデプロイ タスクにより、さまざまな環境への複数コンテナー アプリケーションの継続的インテグレーションとデプロイをサポートしています。

- [Azure Web App for Containers にデプロイする](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-docker-webapp?tabs=dotnet-core)

- [Azure Kubernetes Service にデプロイする](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-aks?tabs=dotnet-core)

ただし、Azure DevOps Services スクリプトベースのタスクを使用して、[Docker Swarm](https://blog.jcorioland.io/archives/2016/11/29/full-ci-cd-pipeline-to-deploy-multi-containers-application-on-azure-container-service-docker-swarm-using-visual-studio-team-services.html) または DC/OS にデプロイすることもできます。

デプロイのアジリティを促進し続けるために、これらのツールにより、開発および CI/CD ソリューションを選択して、コンテナーのワークロードに対して、開発からテスト、運用までの優れたデプロイ エクスペリエンスが提供されます。

図 4-12 は、Azure Container Service で Kubernetes クラスターにデプロイする継続的デプロイ パイプラインを示しています。

![Kubernetes クラスターにデプロイする Azure DevOps Services のスクリーンショット。](./media/life-cycle-ci-cd-pipelines-devops-tools/deploy-mvc-app-container-kubernetes.png)

**図 4-12** Kubernetes クラスターにデプロイする、Azure DevOps Services の継続的デプロイ パイプライン

>[!div class="step-by-step"]
>[前へ](modernize-your-apps-with-monitoring-and-telemetry.md)
>[次へ](migrate-to-hybrid-cloud-scenarios.md)
