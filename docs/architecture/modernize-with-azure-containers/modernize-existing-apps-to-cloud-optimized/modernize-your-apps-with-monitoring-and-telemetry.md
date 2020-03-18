---
title: 監視と製品利用統計情報でアプリを最新化する
description: Azure クラウドおよび Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | 監視と製品利用統計情報でアプリを最新化する
ms.date: 04/30/2018
ms.openlocfilehash: 3d629e89a73c870d4b6396c6b1d0ecbe95b79ead
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72393849"
---
# <a name="modernize-your-apps-with-monitoring-and-telemetry"></a>監視と製品利用統計情報でアプリを最新化する

運用環境でアプリケーションを実行する場合、アプリケーションのパフォーマンスについての分析情報を有していることが重要です。 実行レベルは高いですか。 ユーザーがエラーを受け取っていますか、あるいは、アプリケーションは安定して信頼できますか。 確実にアプリケーションを使用でき、期待どおりに動作できるように、多機能なパフォーマンス監視、強力なアラート機能、ダッシュボードが必要です。 また、問題が発生した場合にすばやく表示し、影響を受けている顧客の数を特定し、根本原因の分析を実行して問題を見つけて解決する必要もあります。

## <a name="monitor-your-application-with-application-insights"></a>Application Insights を使用してアプリケーションを監視する

Application Insights は、複数のプラットフォームで作業する Web 開発者向けの拡張可能なアプリケーション パフォーマンス管理 (APM) サービスです。 このサービスを使用して、実行中の Web アプリケーションを監視することができます。 Application Insights は、パフォーマンスの異常を自動的に検出します。 組み込まれている強力な分析ツールを使えば、問題を診断でき、ユーザーがアプリを使用して実行している操作を把握できます。 Application Insights は、パフォーマンスと使いやすさを継続的に改善できるように設計されています。 オンプレミスとクラウドのどちらでホストされているかに関係なく、.NET、Node.js、J2EE などのさまざまなプラットフォーム上のアプリに役立ちます。 Application Insights は、DevOps プロセスと統合され、さまざまな開発ツールへの接続ポイントを備えています。

図 4-10 は、Application Insights でアプリケーションを監視する方法と、その分析情報をダッシュボードに表示する方法の例を示しています。

![Application Insights 監視ダッシュボードのスクリーンショット。](./media/modernize-your-apps-with-monitoring-and-telemetry/application-insights-monitoring-dashboard.png)

**図 4-10** Application Insights 監視ダッシュボード

## <a name="monitor-your-docker-infrastructure-with-log-analytics-and-its-container-monitoring-solution"></a>Log Analytics とそのコンテナー監視ソリューションを使用して Docker インフラストラクチャを監視する

[Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) は、[Microsoft Azure の全体監視ソリューション](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)に含まれています。 また、[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) のサービスでもあります。 Log Analytics では、可用性とパフォーマンスを維持できるように、クラウド環境とオンプレミス環境 (オンプレミスの場合は OMS) を監視します。 Log Analytics を使用すると、クラウドおよびオンプレミスの環境内にあるリソースによって生成されたデータや、他の監視ツールのデータを収集し、複数のソースにわたる分析を行えます。

Azure インフラストラクチャ ログに関して、Log Analytics は、Azure サービスとして、他の Azure サービス ([Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) を使用)、Azure VM、Docker コンテナー、オンプレミスまたはその他のクラウド インフラストラクチャのログとメトリック データが取り込まれます。 Log Analytics では、このデータに加えて、柔軟なログ検索と、すぐに利用できる分析機能が提供されます。 複数のソースのデータ分析に使用できる豊富なツールが提供され、すべてのログに対して複雑なクエリを実行でき、指定した条件に基づいて事前に通知することができます。 さらにカスタム データを Log Analytics の中央リポジトリに収集して、照会および視覚化することができます。 また、Log Analytics の組み込みソリューションを活用して、インフラストラクチャのセキュリティと機能の分析情報をすぐに得ることもできます。

Log Analytics には、任意のブラウザーで稼働する OMS ポータルまたは Azure portal を使用してアクセスでき、構成設定と複数のツールを使用して、収集されたデータの分析と操作を行うことができます。

Log Analytics の[コンテナー監視ソリューション](https://docs.microsoft.com/azure/log-analytics/log-analytics-containers)を使用すると、Docker と Windows のコンテナー ホストを 1 か所に表示して管理できます。 このソリューションは、どのコンテナーが実行中か、何のコンテナー イメージを実行中か、コンテナーがどこで実行中かを表示します。 コンテナーで使用されているコマンドなど、詳細な監査情報を確認できます。 また、Docker または Windows ホストをリモートで確認しなくても、一元化されたログを表示および検索して、コンテナーのトラブルシューティングを行うこともできます。 ノイジー ネイバーで、ホストで過剰なリソースを使用している可能性のあるコンテナーを特定できます。 さらに、コンテナーについて、CPU、メモリ、ストレージ、ネットワークの使用量と、パフォーマンスに関する情報を一元的に確認できます。 Windows を実行しているコンピューターでは、Windows Server、Hyper-V、Docker の各コンテナーのログを一元化して比較できます。 このソリューションは、次のコンテナー オーケストレーターをサポートしています。

- Docker Swarm

- DC/OS

- Kubernetes

- Red Hat OpenShift

図 4-11 は、さまざまなコンテナー ホストおよびエージェントと OMS の関係を示しています。

![Log Analytics コンテナー監視ソリューションのスクリーンショット。](./media/modernize-your-apps-with-monitoring-and-telemetry/log-analytics-container-monitoring-solution.png)

**図 4-11** Log Analytics コンテナー監視ソリューション

Log Analytics コンテナー監視ソリューションを使用して、次のことを行うことができます。

- すべてのコンテナー ホストに関する情報を 1 か所に表示します。

- どのコンテナーが実行中か、何のイメージを実行中か、どこで実行中かを把握します。

- コンテナー上のアクションに対する監査証跡を表示します。

- Docker ホストにリモート ログインすることなく、一元化されたログを表示し、検索してトラブルシューティングを行います。

- "ノイジー ネイバー" で、ホストで過剰なリソースを使用している可能性のあるコンテナーを特定します。

- コンテナーについて、CPU、メモリ、ストレージ、ネットワークの使用量と、パフォーマンスに関する情報を一元的に確認します。

### <a name="additional-resources"></a>その他のリソース

- **Microsoft Azure での監視の概要**

<https://docs.microsoft.com/azure/azure-monitor/overview>

- **Application Insights とは何か?**

<https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview>

- **Log Analytics とは**

<https://docs.microsoft.com/azure/log-analytics/log-analytics-overview>

- **Azure Monitor のコンテナー監視ソリューション**

<https://docs.microsoft.com/azure/azure-monitor/insights/containers>

- **Azure Monitor の概要**

<https://docs.microsoft.com/azure/azure-monitor/overview>

- **Operations Management Suite (OMS) とは**

<https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview>

>[!div class="step-by-step"]
>[前へ](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
>[次へ](life-cycle-ci-cd-pipelines-devops-tools.md)
