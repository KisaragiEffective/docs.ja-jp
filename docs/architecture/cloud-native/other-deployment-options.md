---
title: その他のコンテナー配置オプション
description: Azure を使用したその他のコンテナーデプロイオプション
ms.date: 06/30/2019
ms.openlocfilehash: 1fcb57eedec8c9f5574fffcf409b316332032062
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2019
ms.locfileid: "73841163"
---
# <a name="other-container-deployment-options"></a>その他のコンテナー配置オプション

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

AKS にデプロイするだけでなく、コンテナーと Azure Container Instances の Azure App Service にコンテナーをデプロイすることもできます。

## <a name="when-does-it-make-sense-to-deploy-to-app-service-for-containers"></a>コンテナーの App Service に展開するのはどのような場合に適していますか。

オーケストレーションを必要としない単純な実稼働アプリケーションは、コンテナーの Azure App Service に適しています。

## <a name="how-to-deploy-to-app-service-for-containers"></a>コンテナーの App Service にデプロイする方法

[コンテナーの Azure App Service](https://azure.microsoft.com/services/app-service/containers/)にデプロイするには、AZURE CONTAINER REGISTRY (ACR) と、それにアクセスするための資格情報を構成しておく必要があります。 ホストするコンテナーをレジストリにプッシュして、Azure App Service にプルできるようにします。 作成したアプリを継続的にデプロイするように構成できます。これにより、ACR で対応するイメージを更新するたびに、アプリに更新プログラムが自動的に展開されます。

## <a name="when-does-it-make-sense-to-deploy-to-azure-container-instances"></a>Azure Container Instances に展開するにはどのような意味がありますか。

Azure Container Instances は、シナリオのテストに最適です。 クラウドでホストされているコンテナーインスタンスにアプリケーションをデプロイする高速で簡単な方法を提供します。 Azure Kubernetes Service によって提供されるスケーリングおよびオーケストレーション機能を必要としない場合に、アプリケーションをテストまたはデモするために使用します。

## <a name="how-to-deploy-an-app-to-azure-container-instances"></a>Azure Container Instances にアプリを展開する方法

[Azure Container Instances (ACI)](https://docs.microsoft.com/azure/container-instances/)にデプロイするには、AZURE CONTAINER REGISTRY (ACR) と、それにアクセスするための資格情報を構成しておく必要があります。 また、コンテナーイメージをレジストリにプッシュしておく必要があります。これにより、ACI にプルできます。 Azure CLI またはポータルを使用して ACI を操作できます。 Azure Container registry を使用すると、図3-14 に示すように、レジストリ内から直接、個々のコンテナーインスタンスを ACI に簡単にデプロイできます。

![実行インスタンスの Azure Container Registry](./media/acr-runinstance-contextmenu.png)

**図 3-14**. 実行インスタンスの Azure Container Registry

レジストリからコンテナーインスタンスを作成するには、通常の Azure 設定 (名前、サブスクリプション、リソースグループ、場所)、コンテナーに割り当てるメモリの量、およびリッスンするポートを指定する必要があります。 この[クイックスタートでは、Azure portal を使用して、コンテナーインスタンスを ACI にデプロイする方法を示し](https://docs.microsoft.com/azure/container-instances/container-instances-quickstart-portal)ます。

デプロイが完了したら、新しくデプロイされたコンテナーの IP アドレスを見つけて、指定したポートを介して通信します。

Azure Container Instances には、Azure でコンテナーを実行する最速で簡単な方法が用意されています。 App service や orchestrator を構成したり、仮想マシンに対処したりする必要はありません。 ただし、単純であるため、ACI は主にテスト目的で使用する必要があります。 アプリケーションで自動スケーラビリティが必要な場合、複数のコンテナーが連携するように構成されている場合、またはその他の複雑な機能がある場合は、アプリをホストするために使用できるその他の適切な Azure サービスがあります。

## <a name="references"></a>参照

- [Azure Container Instances Docs](https://docs.microsoft.com/azure/container-instances/)
- [ACR からコンテナーインスタンスをデプロイする](https://docs.microsoft.com/azure/container-instances/container-instances-using-azure-container-registry#deploy-with-azure-portal)

>[!div class="step-by-step"]
>[前へ](scale-containers-serverless.md)
>[次へ](communication-patterns.md)
