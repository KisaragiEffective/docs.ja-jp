---
title: コンテナー ベース アプリケーション用の Azure コンピューティング プラットフォームの選択
description: Azure Cloud および Windows コンテナーで既存の .NET アプリケーションを最新化する |コンテナー ベース アプリケーション用の Azure コンピューティング プラットフォームの選択
ms.date: 02/18/2020
ms.openlocfilehash: 52e7cf1c5dd3a5850564bdb33ac7a4ac4904f432
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503891"
---
# <a name="choosing-azure-compute-platforms-for-container-based-applications"></a>コンテナー ベース アプリケーション用の Azure コンピューティング プラットフォームの選択

前のセクションを読まれたらお気づきのように、Azure は複数の選択肢を提供するオープン クラウドです。 ニーズに最適なものを使用できますが、ここにはコンテナー化されたアプリケーションに使用する製品やテクノロジに関する質問も表示されます。

*既定の*推奨事項として、このガイダンスで推奨される主な条件を次に示します。

- **単一モノリシック アプリ:** Azure App Service を選ぶ
- **n 層アプリ:** バックエンド サービスが 1 つまたは少数の場合は、Azure Kubernetes Service (AKS) や App Service などのオーケストレーターを選択する
- **マイクロサービス:** AKS または Azure Web Apps for Containers を選ぶ
- **サーバーレス関数とイベント ハンドラー:** Azure Functions を選ぶ
- **大規模なバッチ:** Azure Batch を選ぶ

しかし、この推奨事項は、特定のアプリケーションのニーズと特性によって製品の選択が異なるため、完全には信じないことをお勧めします。 すべてのアプリケーションが同様の種類のように見えても、すべてが同じというわけではありません。

アプリケーションのニーズをより詳しく分析した後、選択した製品が違っている場合があります。 しかし、出発点として、特定の優先順位に基づいて評価とテストを開始できる最初のガイダンスを用意することをお勧めします。

次の図で、さまざまな種類のアプリの内訳と、実行可能な推奨される Azure ホスティング シナリオを確認できます。

| アプリケーションのアーキテクチャ | VM - Azure Virtual Machines | ACI - Azure Container Instances | Azure App Service (コンテナーあり/なし) | AKS - Azure Kubernetes Services | Azure Functions | Azure Batch |
|:------------------------:|:--:|:--:|:--:|:--:|:--:|:--:|
| **Web アプリ (モノリシック)**         | ![VM で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![ACI で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![App Service で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![AKS で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | | |
| **N 層アプリ (サービス)**        | ![VM で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![ACI で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![App Service で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![AKS で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![Azure Fuctions で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | |
| **クラウドネイティブ (マイクロサービス)**  | | ![ACI で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | | ![AKS で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> (Linux&nbsp;コンテナー)| ![Azure Functions で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> (イベントドリブン) | |
| **バッチ/ジョブ (バックグラウンド タスク)** | ![VM で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![ACI で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![App Service で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![AKS で可能](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![Azure Functions で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> (バックグラウンド&nbsp;タスク) | ![Azure Batch で推奨](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> (大規模) |

**凡例**

![推奨アイコン](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) 推奨 \
![可能アイコン](media/choosing-azure-compute-options-for-container-based-applications/possible.png) 可能

> [!div class="step-by-step"]
> [前へ](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
> [次へ](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
