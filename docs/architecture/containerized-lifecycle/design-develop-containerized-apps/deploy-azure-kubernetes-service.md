---
title: 高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する
description: Azure Kubernetes Service を使用して、アプリケーションをデプロイする方法について説明します。
ms.date: 02/15/2019
ms.openlocfilehash: 0aa2f83fbf8f9a8815d65730002943cca748643d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "71182370"
---
# <a name="deploy-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) へのデプロイ

推奨されるクライアント オペレーティング システムを使用して AKS とやりとりできます。ここでは、Microsoft Windows と Windows での Ubuntu Linux の埋め込みバージョンで、Bash コマンドを使用してそれを実行する方法について説明します。

AKS を使用前に準備しておく前提条件は次のとおりです。

- Linux または Mac の開発用コンピューター
- Windows の開発用コンピューター
  - Windows で開発者モードが有効にされている
  - Windows Subsystem for Linux
- [Windows、Mac、または Linux](https://docs.microsoft.com/cli/azure/install-azure-cli) に Azure CLI がインストールされている

> [!NOTE]
> 次に関する完全な情報を検索するには:
>
> Azure-CLI: <https://docs.microsoft.com/cli/azure/index>
>
> Windows Subsystem for Linux: <https://docs.microsoft.com/windows/wsl/about>

## <a name="create-the-aks-environment-in-azure"></a>Azure で AKS 環境を作成する

AKS 環境を作成するにはいくつかの方法があります。 これを行うには、Azure CLI コマンドを使用するか、または Azure portal を使用します。

ここでは、Azure CLI を使用して、クラスターを作成し、Azure portal を使用して結果を確認するいくつかの例を調べることができます。 また、開発用コンピューターに、Kubectl と Docker も備えている必要があります。  

## <a name="create-the-aks-cluster"></a>AKS クラスターを作成する

次のコマンドを使用して、AKS クラスターを作成します。

```console
az aks create --resource-group MSSampleResourceGroup --name MSSampleClusterK801 --agent-count 1 --generate-ssh-keys --location westus2
```

作成ジョブが完了したら、Azure portal で作成した AKS を確認できます。

リソース グループ:

![Azure AKS リソース グループのブラウザー ビュー。](media/aks-resource-group-view.png)

**図 4-17**. Azure からの AKS リソース グループ ビュー。

AKS クラスター:

![AKS リソース グループのブラウザー ビュー。](media/aks-cluster-view.png)

**図 4-18**. Azure からの AKS ビュー。

`Azure-CLI` と `Kubectl` を使用して作成したノードを表示することもできます。

最初に、資格情報を取得します。

```console
az aks get-credentials --resource-group MSSampleK8ClusterRG --name MSSampleK8Cluster
```

![上記のコマンドからのコンソール出力:root/.kube/config 内に "MsSampleK8Cluster" が現在のコンテキストとしてマージされました。](media/get-credentials-command-result.png)

**図 4-19** `aks get-credentials` コマンド結果。

次に、Kubectl からノードを取得します。

```console
kubectl get nodes
```

![上記のコマンドからのコンソール出力:状態、経過時間 (実行時間)、およびバージョンを含むノードの一覧](media/kubectl-get-nodes-command-result.png)

**図 4-20** `kubectl get nodes` コマンド結果。

>[!div class="step-by-step"]
>[前へ](orchestrate-high-scalability-availability.md)
>[次へ](docker-apps-development-environment.md)
