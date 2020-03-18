---
title: Azure Container Instances (ACI) に Windows コンテナーをデプロイするタイミング
description: Azure Cloud と Windows コンテナーで既存の .NET アプリケーションを最新化する | Azure Container Instances (ACI) に Windows コンテナーをデプロイするタイミング
ms.date: 04/29/2018
ms.openlocfilehash: 3b6ae1ced9c4e01f5ab400e2575947a396064ebd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69577935"
---
# <a name="when-to-deploy-windows-containers-to-azure-container-instances-aci"></a>Azure Container Instances (ACI) に Windows コンテナーをデプロイするタイミング

Azure Container Instances の主な価値提案は、それにコンテナーをすぐにデプロイできるということ、その環境を保守管理する必要がないこと、基礎となるオペレーティング システムや VM をアップグレードしたり、パッチを適用したりする必要がないこと、すべてが透過的であり、利用者に求められることはすぐに使える環境にコンテナーをデプロイすることだけであることです。

ACI を使用する理由とシナリオは、Azure VM と共にコンテナーを使用する主なシナリオに似ています。そのため、基本的に、Azure Container Instances を使用する主なシナリオは次のようになります。

- **Dev/Test シナリオ**
- **タスクの自動化**
- **CI/CD エージェント**
- **小規模/スケール バッチ処理**
- **単純な Web アプリ**

単純な Web アプリ シナリオは ACI にとって妥当なシナリオではありますが、ACI では、コンテナー イメージあたりコンテナー インスタンスが 1 つに限られるため、可用性が高くなく、スケーラビリティが限られることを考慮してください。

ただし、1 つだけコンテナー インスタンスを提供するため、ACI をインフラストラクチャとして見なすとしても、通常の Azure VM と Windows Server の組み合わせと比べ、大きな利点があります。 ACI を使用するとき、コンテナーを自己保守環境にデプロイし、そのコンテナーに対してのみ料金を支払います。 VM を管理/更新/パッチ適用する必要はありません。そのため、VM と共にコンテナーを使用するようなほとんどのシナリオで、はるかに優れたプラットフォームとなります。 ACI の使用は単純であり、コンテナーをデプロイするだけです。VM 環境を作る必要はありません。

Azure Container Instances (ACI) の主な利点は次のとおりです。

- サーバーを管理せずにコンテナーを実行する
- オンデマンド コンテナーでアジリティが向上する
- コンテナーをクラウドにデプロイする作業が 1 回の命令で終わり、前例のないくらい単純で速い
- ハイパーバイザー分離というセキュリティでアプリケーションを保護する

つまり、ACI を使用すると、仮想マシンを管理したり、新しいツールを習得したりすることなく、アプリを短期間で開発できます。 自分のアプリケーションをコンテナーに入れ、クラウドで実行するだけです。

> [!div class="step-by-step"]
> [前へ](when-to-deploy-windows-containers-to-azure-vms-iaas-cloud.md)
> [次へ](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
