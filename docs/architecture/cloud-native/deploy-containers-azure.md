---
title: Azure でコンテナーをデプロイする
description: Azure Container Registry、Azure Kubernetes Service、Azure Dev Spaces を使用した Azure でのコンテナーのデプロイ。
ms.date: 06/30/2019
ms.openlocfilehash: 6d95db26b6a45dd6825c88693308ffe90d1ed071
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840485"
---
# <a name="deploying-containers-in-azure"></a>Azure でコンテナーをデプロイする

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

コンテナーは多くの利点を提供しますが、その1つは移植性です。 ローカルで開発およびテストした同じコンテナーを簡単に作成し、それを Azure にデプロイして、ステージング環境と運用環境でアプリを実行することができます。 Azure には、コンテナーベースのアプリホスティング用のさまざまなオプションが用意されており、同様に、さまざまな方法でデプロイを行うことができます。 最も一般的で柔軟性の高い方法は、コンテナーを Azure Container Registry (ACR) にデプロイすることです。これは、それらのサービスをホストするために使用するすべてのサービスからアクセスできます。 Azure Web App for Containers、Azure Kubernetes Services (AKS)、および Azure Container Instance (ACI) はすべて、ACR にプッシュされたコンテナーイメージにアクセスできます。

## <a name="azure-container-registry"></a>Azure Container Registry

Azure Container Registry (ACR) を使用すると、すべてのコンテナーデプロイのイメージを構築、保存、および管理できます。 パブリックとプライベートの両方のコンテナーレジストリがあり、そこにコンテナーをデプロイできます。 他のオプションに対する ACR の利点は、イメージを運用環境の近くに保持して、ビルドと展開の時間を短縮できることです。 また、Azure リソースの残りの部分で使用するのと同じセキュリティ手順を使用してセキュリティを保護することもできます。これにより、セキュリティが向上し、資産管理作業が軽減されます。

[コンテナーレジストリを作成するには、Azure Portal を使用](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)するか[、Azure CLI](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli)または[PowerShell ツール](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-powershell)を使用します。 新しいコンテナーレジストリを作成するには、Azure サブスクリプション、リソースグループ、および一意の名前が必要です。 図3-11 に、レジストリを作成するための基本的なオプションを示します *。レジストリ*は、azurecr.io でホストされます。

コンテナーレジストリを作成 ![には](./media/create-container-registry.png)
**図 3-11**をご確認ください。 コンテナーレジストリの作成

レジストリを作成したら、それを使用する前に認証を行う必要があります。 通常は、Azure CLI コマンドを使用してレジストリにログインします。

```azurecli
az acr login --name *registryname*
```

Azure Container Registry でレジストリを作成したら、docker コマンドを使用してコンテナーイメージをそこにプッシュできます。 ただし、これを行う前に、まず、ACR ログインサーバーの完全修飾名 (URL) でイメージにタグを付ける必要があります。 これは、azurecr.io と*いう形式になります。*

```console
docker tag mycontainer myregistry.azurecr.io/mycontainer:v1
```

イメージにタグを付けたら、`docker push` コマンドを使用して、ACR インスタンスにイメージをプッシュします。

```console
docker push myregistry.azurecr.io/mycontainer:v1
```

イメージをレジストリにプッシュした後、次のコマンドを使用して、ローカルの Docker 環境からイメージを削除することをお勧めします。

```console
docker rmi myregistry.azurecr.io/mycontainer:v1
```

開発者は、自分のマシンからコンテナーレジストリに直接プッシュすることはめったにありません。 代わりに、Azure DevOps などのツールで定義されたビルドパイプラインをこのプロセスに対して行う必要があります。 詳細については、「[クラウド-ネイティブ DevOps](devops.md)」を参照してください。

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

コンテナーベースのアプリケーションに複数のコンテナーが含まれている場合、ほとんどの場合、Kubernetes などの*orchestrator*を使用してコンテナー間の相互作用を定義し、管理する必要があります。 ACR にコンテナーイメージをデプロイすると、Azure Kubernetes Services を簡単に構成して、ACR から更新されたイメージを自動的にデプロイできます。 完全な CI/CD パイプラインが配置されているので、更新プログラムを迅速にデプロイするときのリスクを最小限に抑えるために、[カナリアリリース](https://martinfowler.com/bliki/CanaryRelease.html)戦略を構成することができます。 アプリの新しいバージョンは、最初に運用環境で構成され、トラフィックはルーティングされません。その後、少数のユーザーが、新しくデプロイされたバージョンのアプリにルーティングされます。 新しいバージョンのソフトウェアの信頼性を高めるために、新しいバージョンのインスタンスの数が増え、以前のバージョンのインスタンスが廃止されています。 AKS は、このスタイルのデプロイを簡単にサポートします。

Azure のほとんどのリソースと同様に、ポータルを使用して、またはコマンドラインツールや、ヘルムや Terraform などのインフラストラクチャ自動化ツールを使用して、Azure Kubernetes クラスターを作成できます。 新しいクラスターの使用を開始するには、次の情報を提供する必要があります。

- Azure サブスクリプション
- リソース グループ
- クラスター名の Kubernetes
- Region
- Kubernetes のバージョン
- DNS 名のプレフィックス
- ノードサイズ
- ノード数

作業を開始するには、この情報で十分です。 Azure Portal での作成プロセスの一環として、クラスターの次の機能のオプションを構成することもできます。

- [スケール]
- 認証
- ネットワーク
- 監視
- Tags

この[クイックスタートでは、Azure portal を使用して AKS クラスターをデプロイ](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)する手順について説明します。

## <a name="azure-dev-spaces"></a>Azure Dev Spaces

複雑な Kubernetes クラスターでは、をホストするために大量のリソースが必要になることがあります。これにより、開発者は、アプリケーション全体を1台のコンピューター (特にラップトップ) で実行することが難しくなります。 Azure Dev Spaces は、Azure でホストされている独自のバージョンの Azure Kubernetes クラスターを開発者が操作できるようにすることで、このソリューションを提供します。 Azure Dev Spaces は、AKS を使用してマイクロサービスベースのアプリケーションの開発を容易にするように設計されています。

Azure Dev Spaces の価値を理解するために、Microsoft Azure でコンテナーの Gabe Monroy PM の潜在顧客からこの引用を共有しましょう。

"多くのコンポーネントで構成される複雑なマイクロサービスアプリケーションで、それぞれ独自の構成サービスとバッキングサービスを備えた、新しい従業員がバグを修正しようとしているとします。 使用を開始するには、ローカル開発環境を構成して、IDE のセットアップ、ツールチェーンの構築、コンテナー化されたサービスの依存関係、ローカルの Kubernetes 環境、バッキングサービスのモックなどの運用を模倣できるようにする必要があります。 開発環境の設定に関連する時間があれば、最初のバグを修正するには数日かかる可能性があります。

> または、Dev Spaces と AKS を使用することもできます。

Azure Dev Spaces を操作するプロセスには、次の手順が含まれます。

1. 開発領域を作成します。
2. ルートの dev space を構成します。
3. (独自のバージョンのシステムの) 子の開発領域を構成します。
4. Dev 空間に接続します。

これらの手順はすべて、Azure CLI と新しい `azds` コマンドラインツールを使用して実行できます。 たとえば、特定の Kubernetes クラスター用に新しい Azure Dev Space を作成するには、次のようなコマンドを使用します。

```azurecli
az aks use-dev-spaces -g my-aks-resource-group -n MyAKSCluster
```

次に、[`azds prep`] コマンドを使用して、アプリケーションを実行するために必要な Docker およびヘルムグラフの資産を生成できます。 次に、`azds up`を使用して AKS でコードを実行します。 このコマンドを初めて実行すると、ヘルムグラフがインストールされ、指示に従ってコンテナーがビルドおよび配置されます。 初回実行時には数分かかることがあります。 ただし、変更を行った後、`azds space select` を使用して独自の子開発領域に接続し、分離された子開発領域に更新プログラムをデプロイしてデバッグすることができます。 開発領域を準備して実行すると、`azds up` コマンドを再発行することによって更新を送信できます。または、Visual Studio または Visual Studio Code の組み込みツールを使用することもできます。 VS Code では、コマンドパレットを使用して、開発領域に接続します。 図3-12 は、Visual Studio で Azure Dev Spaces を使用して web アプリケーションを起動する方法を示しています。

Visual Studio で Azure Dev Spaces に接続 ![には](./media/azure-dev-spaces-visual-studio-launchsettings.png)
**図 3-12**をご確認ください。 Visual Studio での Azure Dev Spaces への接続

## <a name="references"></a>参照

- [カナリアリリース](https://martinfowler.com/bliki/CanaryRelease.html)
- [VS Code での Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/quickstart-netcore)
- [Visual Studio での Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/quickstart-netcore-visualstudio)

>[!div class="step-by-step"]
>[前へ](combine-containers-serverless-approaches.md)
>[次へ](scale-containers-serverless.md)
