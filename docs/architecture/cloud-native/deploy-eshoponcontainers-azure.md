---
title: eShopOnContainers を Azure にデプロイする
description: Azure Kubernetes Service、ヘルム、および DevSpaces を使用した eShopOnContainers アプリケーションのデプロイ。
ms.date: 06/30/2019
ms.openlocfilehash: 21033cc904dc595193c69f3452ce2522740f8ff6
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840491"
---
# <a name="deploying-eshoponcontainers-to-azure"></a>eShopOnContainers を Azure にデプロイする

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

EShopOnContainers アプリケーションをサポートするロジックは、さまざまなサービスを使用して Azure でサポートできます。 Azure Kubernetes Service (AKS) を使用して Kubernetes を利用する方法をお勧めします。 これは、簡単に繰り返されるインフラストラクチャ構成を実現するために、ヘルム配置と組み合わせることができます。 開発プロセスの一部として、開発者は必要に応じて Kubernetes の Azure Dev Spaces を利用できます。 別の方法として、Azure Functions や Azure Logic Apps などの Azure サーバーレス機能を使用してアプリの機能をホストすることもできます。

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

独自の AKS クラスターで eShopOnContainers アプリケーションをホストする場合は、最初の手順としてクラスターを作成します。 これを行うには、Azure portal を使用します。ここでは、必要な手順を順番に説明します。または、Azure CLI を使用して、ロールベースの Access Control (RBAC) とアプリケーションのルーティングを有効にするために注意を払う必要があります。 EShopOnContainers のドキュメントでは、独自の AKS クラスターを作成する手順について説明しています。 クラスターが作成されたら、Kubernetes ダッシュボードへのアクセスを有効にする必要があります。この時点で、Kubernetes ダッシュボードを参照してクラスターを管理することができます。

クラスターを作成して構成したら、そのクラスターに対して、ヘルムと Tiller を使用してアプリケーションをデプロイできます。

## <a name="deploying-to-azure-kubernetes-service-using-helm"></a>ヘルムを使用した Azure Kubernetes Service へのデプロイ

AKS への基本的な配置では、カスタム CLI スクリプトまたは単純な配置ファイルを使用できますが、より複雑なアプリケーションでは、ヘルムなどの依存関係管理ツールを使用する必要があります。 ヘルムは、クラウドネイティブコンピューティングファンデーションによって管理されており、Kubernetes アプリケーションを定義、インストール、およびアップグレードするのに役立ちます。 ヘルムは、ヘルムグラフを使用するコマンドラインクライアント、およびクラスター内コンポーネントの Tiller で構成されています。 ヘルムグラフでは、標準の YAML 形式のファイルを使用して、関連する Kubernetes リソースのセットを記述します。通常は、ここで説明するアプリケーションと共にバージョン管理されます。 ヘルムグラフは、記述されているインストールの要件に応じて、単純なものから複雑なものまで多岐に範囲されています。

EShopOnContainers ヘルムグラフは、/k8s/ヘルムフォルダーにあります。 図2-6 は、アプリケーションのさまざまなコンポーネントを、ヘルムが配置を定義するために使用するフォルダー構造に編成する方法を示しています。

![eShopOnContainers アーキテクチャ](./media/eshoponcontainers-helm-folder.png)
**図 2-6**。 EShopOnContainers ヘルムフォルダー。

個々のコンポーネントは、`helm install` コマンドを使用してインストールされます。 これらのコマンドは簡単にスクリプト化できます。 eShopOnContainers には、さまざまなコンポーネントをループ処理し、それぞれのヘルムグラフを使用してインストールする "deploy all" スクリプトが用意されています。 結果として、ソース管理のアプリケーションでバージョン管理された反復可能なプロセスが生成されます。これにより、チームのすべてのユーザーが1行のスクリプトコマンドで AKS クラスターにデプロイできるようになります。 特に Azure Dev Spaces と組み合わせると、開発者はマイクロサービスベースのクラウドネイティブアプリに対する個々の変更を簡単に診断してテストすることができます。

## <a name="azure-dev-spaces"></a>Azure Dev Spaces

Azure Dev Spaces は、開発中に個人の開発者が Azure で独自の AKS クラスターをホストするのに役立ちます。 これにより、ローカルコンピューターの要件が最小限に抑えられ、チームメンバーは、実際の AKS 環境での変更の動作をすばやく確認できます。 Azure Dev Spaces には、開発者が開発スペースを管理し、必要に応じて特定の子開発領域に配置するために使用する CLI が用意されています。 各子 dev 空間は、一意の URL サブドメインを使用して参照されます。これにより、個々の開発者が、進行中の他の作業との競合を回避できるように、変更されたクラスターをサイドバイサイドで配置できます。 図2-7 では、developer Susie が独自のバージョンの Bikes マイクロサービスを開発領域にデプロイした方法を確認できます。 次に、自分のスペースの名前 (susie.s.dev.myapp.eus.azds.io) で始まるカスタム URL を使用して、自分の変更をテストできます。

![eShopOnContainers アーキテクチャ](./media/azure-devspaces-one.png)
**図 2-7**。 Developer Susie は独自のバージョンの Bikes マイクロサービスをデプロイしてテストします。

同時に、開発者 John は予約マイクロサービスをカスタマイズし、変更をテストする必要があります。 図2-8 に示すように、Susie の変更と競合することなく、自分の開発スペースに変更をデプロイできます。 彼は自分の URL を使用して自分の変更をテストすることができます。自分の URL の先頭には、スペース (john.s.dev.myapp.eus.azds.io) の名前が付いています。

![eShopOnContainers アーキテクチャ](./media/azure-devspaces-two.png)
**図 2-8**。 開発者 John は独自のバージョンの予約マイクロサービスをデプロイし、他の開発者と競合することなくテストします。

Azure Dev Spaces を使用すると、チームは AKS を直接操作しながら、変更の変更、配置、およびテストを行うことができます。 この方法では、すべての開発者が独自の AKS 環境を使用するため、個別の専用ホスト環境が必要になります。 開発者は CLI を使用して Azure Dev Spaces を操作したり、アプリケーションを起動して Visual Studio から直接 Azure Dev Spaces したりできます。 [Azure Dev Spaces のしくみと構成方法の詳細については、こちらを参照してください。](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works)

## <a name="azure-functions-and-logic-apps-serverless"></a>Azure Functions と Logic Apps (サーバーレス)

EShopOnContainers サンプルには、オンラインマーケティングキャンペーンの追跡のサポートが含まれています。 Azure 関数は、指定されたキャンペーン ID のマーケティングキャンペーンの詳細を取得するために使用されます。 この目的のために完全な ASP.NET Core アプリケーションを作成するのではなく、1つの Azure 関数エンドポイントがシンプルで十分です。 Azure Functions は、特に Kubernetes で実行するように構成されている場合に、完全 ASP.NET Core アプリケーションよりもはるかに簡単なビルドおよび配置モデルを備えています。 関数の配置は、Azure Resource Manager (ARM) テンプレートと Azure CLI を使用してスクリプト化されます。 このキャンペーン詳細マイクロサービスは顧客向けではなく、オンラインストアと同じ要件がないため、Azure Functions の候補として適しています。 関数を使用するには、データベース接続文字列データやイメージベース URI 設定など、一部の構成が適切に機能する必要があります。 Azure Functions は、Azure Portal で構成します。

## <a name="references"></a>参照

- [eShopOnContainers: AKS で Kubernetes クラスターを作成する](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS)#create-kubernetes-cluster-in-aks)
- [eShopOnContainers: Azure Dev Spaces](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Azure-Dev-Spaces)
- [Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/about)

>[!div class="step-by-step"]
>[前へ](map-eshoponcontainers-azure-services.md)
>[次へ](centralized-configuration.md)
