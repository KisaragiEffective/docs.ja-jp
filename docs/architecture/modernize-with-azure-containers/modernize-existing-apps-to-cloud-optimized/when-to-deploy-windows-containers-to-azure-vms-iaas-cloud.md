---
title: Azure Vm (IaaS クラウド) に Windows コンテナーを展開するタイミング
description: Azure Cloud と Windows コンテナーで既存の .NET アプリケーションを最新化する | Azure VM (IaaS クラウド) に Windows コンテナーをデプロイするタイミング
ms.date: 04/28/2018
ms.openlocfilehash: e9a2903662306b607977a7751018e24161ab80ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69577905"
---
# <a name="when-to-deploy-windows-containers-to-azure-vms-iaas-cloud"></a>Azure Vm (IaaS クラウド) に Windows コンテナーを展開するタイミング

組織で Azure VM を利用している場合、Windows コンテナーも使用しているとしても、IaaS を引き続き扱っています。 つまり、負荷分散型のインフラストラクチャで複数の VM にデプロイする必要があるとき、高度にスケーラブルなアプリケーションを実現するには、インフラストラクチャ運用、VM OS パッチ、複雑なインフラストラクチャを扱うことになります。 Azure VM で Windows コンテナーを使用する主なシナリオは次のとおりです。

- **Dev/test 環境**:クラウド内の VM は、クラウドでの開発とテストに最適です。 ニーズに合わせて環境を短時間で構築したり、停止したりできます。

- **小規模/中規模のスケーラビリティ ニーズ**:運用環境に必要な VM が 2 つか 3 つのシナリオでは、オーケストレーターなど、さらに高度な PaaS 環境に移行できるまで少数の VM を管理すれば安上がりになります。

- **既存のデプロイ ツールを使用した運用環境**:VM またはベアメタル サーバー (Puppet や同様のツールなど) に複雑なデプロイを行う目的で、ツールに投資しているオンプレミス環境から移行することがあります。 運用環境のデプロイ プロシージャの変更を最小限に留めてクラウドに移行するために、そのツールを引き続き使用し、Azure VM にデプロイするかもしれません。 しかしながら、開発単位として Windows コンテナーを使用すれば、より良いデプロイが可能になります。

>[!div class="step-by-step"]
>[前へ](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
>[次へ](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)
