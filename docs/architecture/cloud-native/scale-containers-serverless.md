---
title: コンテナーとサーバーレスのアプリケーションのスケーリング
description: 個々のマシンリソースを増やしたり、アプリケーションクラスター内のマシンの数を増やしたりすることにより、クラウドネイティブアプリケーションを Azure Kubernetes Service でスケーリングし、ユーザーのニーズを満たすことができます。
ms.date: 09/23/2019
ms.openlocfilehash: 2d0537fb3ed56beb4eccbf9b8c34a5d87793413b
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73841013"
---
# <a name="scaling-containers-and-serverless-applications"></a>コンテナーとサーバーレスのアプリケーションのスケーリング

アプリケーションをスケーリングするには、スケールアップとスケールアウトの2つの一般的な方法があります。前者は、1つのホストに機能を追加することを指し、後者はホストの合計数に追加することを示しています。 これについて考えてみるための一般的な比喩は、友人と友人の間で友人を手にする方法です。 友人が1人だけの場合は、2台の座席レース車にホップできます。 ただし、3つまたは4つの場合は、容量を増やすためにスケールアップする、SUVs または minivan のいずれかを使用する必要があります。 ただし、合計数が数十以上になると、場合によっては、複数の車両 (だれかがバスを運転している場合を除く) が必要になることがあります。これは、インスタンスを追加することによるスケールアウトの概念を示しています (この例では、より多くの車両)。 これがアプリケーションにどのように適用されるかを見てみましょう。

## <a name="the-simple-solution-scaling-up"></a>単純な解決策: スケールアップ

既存のサーバーをアップグレードしてより多くのリソースを提供するプロセス (CPU、メモリ、ディスク i/o 速度、ネットワーク i/o 速度) は、*スケールアップ*として知られています。 クラウドネイティブアプリケーションでは、スケールアップは通常、物理マシンに実際のハードウェアを購入してインストールすることを意味するのではなく、利用可能なオプションの一覧からより多くの機能を備えたプランを選択します。 クラウドネイティブアプリは、通常、Kubernetes ノードプール内の個々のノードをホストするために使用される仮想マシン (VM) のサイズを変更することによってスケールアップします。 ノード、クラスター、ポッドなどの Kubernetes の概念については、[次のセクション](leverage-containers-orchestrators.md)で詳しく説明します。 Azure では、 [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)と[Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)の両方を実行するさまざまな VM サイズをサポートしています。 アプリケーションを垂直方向にスケーリングするには、より大きなノードの VM サイズで新しいノードプールを作成し、ワークロードを新しいプールに移行します。 これには、 [AKS クラスターに複数のノードプール](https://docs.microsoft.com/azure/aks/use-multiple-node-pools)が必要です。これは、現在プレビュー段階にある機能です。 サーバーレスアプリは、 [premium プラン](https://docs.microsoft.com/azure/azure-functions/functions-scale)と premium インスタンスのサイズを選択するか、別の専用 app service プランを選択することでスケールアップします。

## <a name="scaling-out-cloud-native-apps"></a>クラウドネイティブアプリのスケールアウト

クラウドネイティブアプリは、追加のノードまたはポッドをサービス要求に追加することによって、スケールアウトをサポートします。 これは、アプリの構成設定 ([ノードプールのスケーリング](https://docs.microsoft.com/azure/aks/use-multiple-node-pools#scale-a-node-pool-manually)など) を調整するか、自動*スケール*を使用して手動で行うことができます。 自動スケールでは、要求に応答するためにアプリによって使用されるリソースを調整します。これは、サーモスタットが追加の暖房や冷却のためにを呼び出すことによって温度に応答する方法と似ています。 自動スケールを使用する場合、手動によるスケーリングは無効になります。

AKS クラスターは、次の2つの方法のいずれかで拡張できます。

- [クラスターオートスケーラー](https://docs.microsoft.com/azure/aks/cluster-autoscaler)は、リソースの制約のためにノードにスケジュールできないポッドを監視します。 必要に応じてノードが追加されます。
- **水平ポッドオートスケーラー**は、Kubernetes クラスターのメトリックサーバーを使用してポッドのリソース要求を監視します。 サービスにより多くのリソースが必要な場合、オートスケーラーはポッドの数を増やします。

図3-13 に、これら2つのスケーリングサービスの関係を示します。

![App Service プランをスケールアウトします。](./media/aks-cluster-autoscaler.png)

**図 3-13**. App Service プランをスケールアウトします。

これらのサービスは、必要に応じてポッドまたはノードの数を減らすこともできます。 これらの2つのサービスは連携して動作し、多くの場合、クラスターにデプロイされます。 これを組み合わせると、アプリケーションの需要に対応するために必要なポッドの数を実行することが、水平ポッドのオートスケーラーに集中します。 クラスターオートスケーラーは、スケジュールされたポッドをサポートするために必要なノード数の実行に重点を置いています。

### <a name="scaling-azure-functions"></a>スケーリング Azure Functions

Azure Functions は、スケールアウトを自動的にサポートします。既定の従量課金プランでは、受信するトリガーイベントの数に基づいて、リソースが動的に追加 (および削除) されます。 使用されているコンピューティングリソースに対して課金されるのは、実行回数、実行時間、およびメモリ使用量に基づいて、関数が実行されている場合のみです。 Premium プランを使用すると、これらの同じ機能を利用できますが、使用するインスタンスサイズを制御したり、インスタンスが既に稼働状態になっていることを確認したり (コールドスタートの遅延を回避するため)、関数を実行する専用 Vm を構成したりすることもできます。 既定の構成では、ほとんどのアプリに対して経済的でスケーラブルなソリューションが提供されますが、premium オプションを使用すると、開発者はカスタム Azure Functions 要件を柔軟に設定できます。

## <a name="references"></a>参照

- [複数のノードプールの AKS](https://docs.microsoft.com/azure/aks/use-multiple-node-pools)
- [AKS クラスターオートスケーラー](https://docs.microsoft.com/azure/aks/cluster-autoscaler)
- [チュートリアル: AKS でのアプリケーションのスケーリング](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale)
- [Azure Functions のスケールとホスト](https://docs.microsoft.com/azure/azure-functions/functions-scale)

>[!div class="step-by-step"]
>[前へ](deploy-containers-azure.md)
>[次へ](other-deployment-options.md)
