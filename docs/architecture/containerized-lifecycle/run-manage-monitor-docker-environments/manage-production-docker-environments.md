---
title: Docker の実稼働環境を管理する
description: コンテナーベースの実稼働環境を管理する際に重要な点について説明します。
ms.date: 02/15/2019
ms.openlocfilehash: 26e7a3319afe593d75e2384d023c901a389245dc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "71834500"
---
# <a name="manage-production-docker-environments"></a>Docker の実稼働環境を管理する

クラスターの管理とオーケストレーションは、ホストのグループを制御するプロセスです。 これには、クラスターのホストの追加と削除、ホストとコンテナーの現在の状態に関する情報の取得、プロセスの開始と停止が含まれます。 サービスをスケジュールするには、スケジューラからクラスター内の各ホストにアクセスできる必要があるため、クラスターの管理とオーケストレーションはスケジューリングと密接に関係しています。 このため、両方の目的に同じツールがよく使用されます。

## <a name="container-service-and-management-tools"></a>コンテナー サービスと管理ツール

コンテナー サービスでは、人気のオープン ソースのコンテナー クラスタリングやオーケストレーション ソリューションを短期間でデプロイできます。 アプリケーション コンテナーの完全な移植を確保するため Docker イメージが使用されます。 コンテナー サービスを使用すると、Azure Resource Manager テンプレートまたは Azure portal で DC/OS (Mesosphere および Apache Mesos を搭載) と Docker Swarm クラスターをデプロイし、これらのアプリケーションを数千から数万のコンテナーに確実にスケーリングできるようになります。

これらのクラスターは Azure 仮想マシン スケール セットでデプロイされ、Azure ネットワーキングとストレージ サービスが活用されます。 コンテナー サービスにアクセスするには、Azure サブスクリプションが必要です。 コンテナー サービスでは、オーケストレーション レイヤーなどでアプリケーションの移植性を維持しながら、Azure のエンタープライズ レベルの機能を活用できます。

表 6-1 は、オーケストレーター、スケジューラ、クラスタリング プラットフォームに関連する一般的な管理ツールの一覧です。

**表 6-1**. Docker 管理ツール

| 管理ツール | [説明] | 関連するオーケストレーター |
|------------------|-------------|-----------------------|
| [コンテナー用 Azure Monitor](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview) | Azure 専用 Kubernetes 管理ツール | Azure Kubernetes Services (AKS) |
| [Kubernetes Web UI (ダッシュボード)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) | Kubernetes 管理ツールでは、ローカル Kubernetes クラスターを監視および管理できます | Azure Kubernetes Service (AKS)<br/>ローカルの Kubernetes |
| [Azure portal for Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal)<br/>[Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) | Azure、オンプレミス、ローカル開発、およびその他のクラウド上で Service Fabric クラスターを管理するためのオンラインおよびデスクトップ バージョン | Azure Service Fabric |
| [コンテナー監視 (Azure Monitor)](https://docs.microsoft.com/azure/azure-monitor/insights/containers) | 一般的なコンテナー管理および監視ソリューション。 [コンテナー用 Azure Monitor](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview) を使用して Kubernetes クラスターを管理できます。 | Azure Service Fabric<br/>Azure Kubernetes Service (AKS)<br/>Mesosphere DC/OS など。 |

## <a name="azure-service-fabric"></a>Azure Service Fabric

クラスターのデプロイと管理には、Azure Service Fabric という選択肢もあります。 [Service Fabric](https://azure.microsoft.com/services/service-fabric/) は、コンテナー オーケストレーションだけでなく、非常にスケーラブルなマイクロサービス アプリケーションを構築できる開発者プログラミング モデルを含む Microsoft マイクロサービス プラットフォームです。 Service Fabric では、Linux および Windows コンテナーで Docker がサポートされており、Windows および Linux サーバーで実行できます。

Service Fabric 管理ツールを次に示します。

- [Service Fabric 用 Azure portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) では、クラスター関連の操作 (作成/更新/削除) を行い、クラスターのインフラストラクチャ (VM、ロード バランサー、ネットワークなど) を構成します

- [Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) は、ノード/VM の観点とアプリケーションとサービスの観点から Service Fabric クラスターに関する分析情報と特定の操作を提供する特殊な Web UI およびデスクトップ マルチプラットフォーム ツールです。

>[!div class="step-by-step"]
>[前へ](run-microservices-based-applications-in-production.md)
>[次へ](monitor-containerized-application-services.md)
