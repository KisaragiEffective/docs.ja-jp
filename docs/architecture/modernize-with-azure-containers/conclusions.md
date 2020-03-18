---
title: まとめ
description: Azure Cloud と Windows コンテナーで既存の .NET アプリケーションを最新化する | 結論
ms.date: 10/26/2017
ms.openlocfilehash: c7c4042b224577238ae74bd786d4803e487998e7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69578485"
---
# <a name="conclusions"></a>まとめ

- コンテナーベースのソリューションでは、最終的にコストの節約効果が得られます。 コンテナーにより、運用環境での依存関係の欠如によって生じる摩擦が取り除かれるため、デプロイの問題に対するソリューションです。 これらの問題が取り除かれると、Dev/Test、DevOps、運用の各操作が大幅に改善されます。

- Docker コンテナーは、サーバー ベースのアプリケーションまたはサービスの配置の標準的な単位になっています。

- 運用環境の場合、スケーラブルなコンテナーベースのアプリケーションをホストするには、オーケストレーター (Kubernetes など) を使用しする必要があります。

- コンテナーをホストする Azure VM は、クラウドで小規模な Dev/Test 環境を作る迅速でシンプルな方法です。

- 既存のアプリケーションから Azure にリレーショナル データベースを移行するとき、Azure SQL Database Managed Instance が既定で推奨されています。

- Windows コンテナーを利用して既存の .NET アプリケーションの最新化を始めるとき、Visual Studio 2017 と Image2Docker が基本のツールになります。入門学習曲線を加速します。

- コンテナー化されたアプリケーションを運用環境に配置するとき、Azure DevOps Services や Jenkins など、CI/CD パイプライン向けの DevOps カルチャと DevOps ツールを常に作成するか、導入します。

- Microsoft Azure により、Windows コンテナー、クラウド インフラストラクチャ、PaaS サービスを使用して既存の .NET Framework アプリケーションを最新化するための最も包括的で完全な環境が提供されます。

>[!div class="step-by-step"]
>[[戻る]](walkthroughs-technical-get-started-overview.md)
