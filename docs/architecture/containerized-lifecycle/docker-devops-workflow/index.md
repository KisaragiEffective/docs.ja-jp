---
title: Microsoft ツールを使用した Docker アプリケーション DevOps ワークフロー
description: Microsoft プラットフォームでのコンテナー化された Docker アプリケーションのライフサイクルと Microsoft ツールでの Tools DevOps ワークフロー
ms.date: 02/15/2019
ms.openlocfilehash: 6b138301a7e6794ce0a7b15957684b3b73e9f89f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70295073"
---
# <a name="docker-application-devops-workflow-with-microsoft-tools"></a>Microsoft ツールを使用した Docker アプリケーション DevOps ワークフロー

*Microsoft Visual Studio、Azure DevOps Services、Team Foundation Server、Azure Monitor は、開発および IT 操作の包括的なエコシステムを提供します。これにより、チームでは、プロジェクトを管理し、コンテナー化アプリケーションを迅速にビルド、テスト、展開することができます。*

クラウドの Visual Studio と Azure DevOps Services と、オンプレミスの Team Foundation Server により、開発チームは、Windows または Linux を対象にするコンテナー化アプリケーションを生産的に直接ビルド、テスト、リリースできます。

Microsoft ツールは、Docker、.NET Core、または他のプラットフォームとのあらゆる組み合わせのコンテナー化アプリケーションの特定の実装の、グローバル ビルド、継続的インテグレーション (CI)、Azure DevOps Services または Team Foundation Server でのテストから、Docker 環境 (開発、ステージング、実稼働) への継続的デプロイ (CD) と Azure Monitor を介した開発チームへのサービスに関する分析情報の転送のパイプラインを自動化できます。 すべてのコードのコミットでビルド (CI) を開始して、特定のコンテナー化された環境 (CD) に自動的にサービスを展開できます。

開発者およびテスト担当者は、Microsoft Azure のテンプレートを使用して、Docker に基づく運用環境のような開発とテストの環境を、簡単かつ迅速にプロビジョニングできます。

コンテナー化アプリケーションの開発は、ビジネスが複雑であり、拡張のニーズがあればあるほど、着実に複雑になります。 この複雑性の良い例は、マイクロサービス アーキテクチャに基づいたアプリケーションです。 このような環境で成功するには、プロジェクトのライフ サイクル全体が自動化される必要があります。ビルドやデプロイのみでなく、製品利用統計情報のコレクションと共に、バージョンも管理される必要があります。 Azure DevOps Services と Azure には、次の機能があります。

- (Git または Team Foundation バージョン管理に基づく) Azure DevOps Services と Team Foundation Server のソース コード管理、(アジャイル、スクラム、CMMI がサポートされている) アジャイル計画、CI、リリース管理、アジャイル チーム用の他のツール。

- Azure DevOps Services と Team Foundation Server にはファースト パーティおよびサード パーティの高性能な拡張機能からなるエコシステムが含まれ、そのエコシステムは増加を続けています。このような拡張機能を使用すると、マイクロサービス用に、CI、ビルド、テスト、配信、リリース管理パイプラインを容易に構成できます。

- Azure DevOps Services でビルド パイプラインの一部として、自動テストを実行します。

- Azure DevOps Services では、実稼働環境用のみでなく、A/B 実験、[カナリヤ リリース](https://martinfowler.com/bliki/CanaryRelease.html)など、テスト用を含む複数の環境への配信により、DevOps のライフ サイクルを強化できます。

- 組織は、Azure Container Registry に格納されたプライベート イメージから Docker コンテナーを、Azure Resource Manager テンプレートと既に使い慣れているツールを使用して、(Data、PaaS などの) Azure コンポーネントの任意の依存関係と共に簡単にプロビジョニングできます。

>[!div class="step-by-step"]
>[前へ](../design-develop-containerized-apps/build-aspnet-core-applications-linux-containers-aks-kubernetes.md)
>[次へ](docker-application-outer-loop-devops-workflow.md)
