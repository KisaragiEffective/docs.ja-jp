---
title: コンテナー化アプリケーションのサービスを監視する
description: コンテナー アーキテクチャのモニターに関する重要な側面をいくつか説明します
ms.date: 02/15/2019
ms.openlocfilehash: e14553d510751d8a75020a1b6beb9fd7bc29596e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68673459"
---
# <a name="monitor-containerized-application-services"></a>コンテナー化アプリケーションのサービスを監視する

アプリケーションを複数のコンテナーとマイクロサービスに分割し、アプリケーション全体の動作を監視および分析する方法を持つことが重要です。

## <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://azure.microsoft.com/services/monitor/) は、ライブ アプリケーションを監視する拡張可能な分析サービスです。 パフォーマンスの問題の検出と診断や、ユーザーがアプリを使用して実際に実行する操作の理解に役立ちます。 開発者向けに設計されており、サービスやアプリケーションのパフォーマンスと使いやすさを継続的に向上させることを目的としています。 Azure Monitor は、オンプレミスまたはクラウドでホストされている .NET、Java、Node.js などのさまざまなプラットフォーム上で、Web/サービスとスタンドアロン アプリの両方と連携します。

### <a name="additional-resources"></a>その他のリソース

- **Azure Monitor の概要** \
  <https://docs.microsoft.com/azure/azure-monitor/overview>

- **Application Insights とは何か?** \
  <https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview>

- **Azure Monitor のメトリック** \
  <https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics>

- **Azure Monitor のコンテナー監視ソリューション** \
  <https://docs.microsoft.com/azure/azure-monitor/insights/containers>

## <a name="security-and-backup-services"></a>セキュリティとバックアップ サービス

アプリケーションとインフラストラクチャが確実にビジネス ニーズをサポートできる最高の状態になるようにするには、多くのサポートの雑事と対処する必要がある細かいことが数多くあります。また、マイクロサービス分野では状況がより複雑になるので、行動を起こす必要があるときには、包括的なビューと詳細なビューを両方持つ方法が必要です。

Azure には、クラウドおよびオンプレミス リソースの両方の 4 つの重要な側面を管理し、統一されたビューを提供するためのツールがあります。

- **セキュリティ**。 [Azure Security Center](https://azure.microsoft.com/services/security-center/) を使用します。
  - 仮想マシン、アプリ、ワークロードのセキュリティを完全に可視化し、制御します。
  - セキュリティ ポリシーの管理を一元化し、既存のプロセスとツールを統合します。
  - 高度な分析で実際の脅威を検出します。

- **バックアップ**。 [Azure Backup](https://azure.microsoft.com/services/backup/) を使用します。
  - コストのかかるビジネスの中断を回避し、コンプライアンスの目標を達成し、ランサムウェアやヒューマン エラーからデータを保護します。
  - バックアップ データを転送中および保存中に暗号化します。
  - 不正使用を防ぐために、多要素認証に基づくアクセスを確保します。

- **オンプレミスのリソース**。 [本当に一貫性が高いハイブリッド クラウド](https://azure.microsoft.com/resources/truly-consistent-hybrid-cloud-with-microsoft-azure/)を使用します。

>[!div class="step-by-step"]
>[前へ](manage-production-docker-environments.md)
>[次へ](../key-takeaways/index.md)
