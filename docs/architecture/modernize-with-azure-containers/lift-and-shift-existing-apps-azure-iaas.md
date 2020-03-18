---
title: 既存の .NET アプリを Azure IaaS にリフトアンドシフトする (クラウド インフラストラクチャ対応)
description: Azure Cloud および Windows コンテナーを使用して既存の .NET アプリケーションを最新化します。
ms.date: 04/28/2018
ms.openlocfilehash: c7638a034dbb27baea1b097bdb66175bfb5a71f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73089630"
---
# <a name="lift-and-shift-existing-net-apps-to-azure-iaas-cloud-infrastructure-ready"></a>既存の .NET アプリを Azure IaaS にリフトアンドシフトする (クラウド インフラストラクチャ対応)

> 展望:最初の手順として、オンプレミスの投資と、ハードウェアとネットワークのメンテナンスの総コストを削減するために、既存のアプリケーションを単純にクラウド内にホストし直します。

既存のアプリケーションを Azure Infrastructure as a Service (IaaS) プラットフォームに移行するには、"*方法*" より前に、Azure の IaaS に直接移行したい "*理由*" を分析することが重要です。 この最新化成熟度レベルのシナリオでは、基本的に、現在のオンプレミス インフラストラクチャを引き続き使用するのではなく、クラウドで VM の使用を開始します。

もう 1 つの分析ポイントでは、より高度なマネージド サービスを単に Azure に追加するのではなく、純粋な IaaS クラウドに移行したい "*理由*" を分析します。 最初に、IaaS が必要となる状況を特定します。

図 2-1 では、クラウド インフラストラクチャ対応アプリケーションを最新化成熟度レベルに位置付けています。

![クラウド インフラストラクチャ対応アプリケーションの位置付け](./media/image2-1.png)

**図 2-1** クラウド インフラストラクチャ対応アプリケーションの位置付け

## <a name="why-migrate-existing-net-web-applications-to-azure-iaas"></a>既存の .NET Web アプリケーションを Azure IaaS に移行する理由

最初の IaaS レベルであってもクラウドに移行する主な理由は、コスト削減の実現です。 より管理されたインフラストラクチャ サービスを使用することで、組織はハードウェア メンテナンス、サーバーまたは VM のプロビジョニングとデプロイ、インフラストラクチャ管理への投資を削減できます。

アプリをクラウドに移行することを決定した後、PaaS のようなより高度なオプションではなく IaaS を選択するのは、単に IaaS 環境の方がなじみやすいだろうというのが主な理由です。 現在のオンプレミス環境に似た環境に移行することで、より低い学習曲線が得られ、クラウドへの最短パスを実現します。

しかしながら、クラウドへの最短パスを通ることが、アプリケーションをクラウドで実行することによって最大のメリットを得られることを意味するわけではありません。 どの組織でも、既に導入されているクラウド最適化およびクラウドネイティブの成熟度レベルでのクラウド移行によって最も大きなメリットが得られます。

また、アプリケーションが既にクラウド、さらに言えば IaaS で実行されている場合は、アプリケーションの最新化と再設計が容易になることも明らかになっています。 アプリケーション データの移行は既に完了しています。 また、組織はクラウドでの作業に必要なスキルを獲得し、"クラウド文化" での運用にシフトしていることでしょう。

## <a name="when-to-migrate-to-iaas-instead-of-to-paas"></a>PaaS ではなく IaaS に移行する場合

以降のセクションでは、ほとんどが PaaS プラットフォームとサービスに基づいているクラウド最適化アプリケーションについて説明します。 これらのアプリでは、クラウドへの移行によって最も大きなメリットが得られます。

単に既存のアプリケーションをクラウドに移行することが目標の場合は、まず、Azure App Service で実行するために大幅な変更を必要としない既存のアプリケーションを特定します。 これらのアプリが、クラウド最適化の最初の候補になるはずです。

次に、Windows コンテナーと App Service などの PaaS または Azure Kubernetes Service のようなオーケストレーターにまだ移行できないアプリの場合は、それらをシンプルでわかりやすい VM (IaaS) に移行します。

ただし、VM の構成、セキュリティ保護、メンテナンスを正しく行うためには、Azure で PaaS サービスを使用する場合よりもずっと多くの時間がかかり、IT の専門知識が要求されることに留意します。 Azure Virtual Machines を選択する場合は必ず、VM 環境に対する修正プログラムの適用、更新、管理に伴って日々発生するメンテナンスの労力を考慮してください。 Azure Virtual Machines は IaaS です。

## <a name="use-azure-migrate-to-analyze-and-migrate-your-existing-applications-to-azure"></a>Azure Migrate を使用して、既存のアプリケーションを分析し、Azure に移行する

クラウドへの移行は必ず難しいというわけではありません。 ところが、多くの組織では、環境を深く可視化して、アプリケーション、ワークロード、データ間の密接な相互依存関係を把握する最初の段階で苦労しています。 そうした可視化ができないと、その後のパスの計画が困難になる可能性があります。 移行を成功させるために何が必要なのかについて詳細な情報がないと、組織内で適切な会話を交わすことはできません。 潜在的なコスト上の利点について、またはワークロードを単にリフトアンドシフトできるかどうか、移行を成功させるために大幅な手直しが必要になるのかについてよくわかりません。 多くの組織がためらうのも無理はありません。

[Azure Migrate](https://aka.ms/azuremigrate) は、Azure への移行を支援するために必要なガイダンス、分析情報、メカニズムが提供される新しいサービスです。 Azure Migrate では次のものが提供されます。

- オンプレミスの仮想マシンを対象にした検出と評価

- 多階層アプリケーションの信頼性の高い検出を行うための組み込みの依存関係マッピング

- Azure 仮想マシンへのインテリジェントな適切なサイズ変更

- 潜在的な問題を修正するためのガイドラインを含む互換性レポート

- データベースの検出と移行のための Azure Database Management Service との統合

Azure Migrate により、ビジネスへの影響を最小限に抑えてワークロードを移行し、Azure で期待どおりに実行できるという自信が得られます。 適切なツールとガイダンスを使用すると、重要なパフォーマンスと信頼性のニーズを確実に満たしながら、投資収益率を最大限に高めることができます。

図 2-2 は、Azure Migrate によって実行されるすべてのサーバーとアプリケーションの接続に対する組み込みの依存関係マッピングを示しています。

![クラウド インフラストラクチャ対応アプリケーションの位置付け](./media/image2-2.png)

**図 2-2** クラウド インフラストラクチャ対応アプリケーションの位置付け

## <a name="use-azure-site-recovery-to-migrate-your-existing-vms-to-azure-vms"></a>Azure Site Recovery を使用して既存の VM を Azure VM に移行する

エンドツーエンドの [Azure Migrate](https://aka.ms/azuremigrate) に含まれている [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) は、Web アプリを Azure の VM に簡単に移行するために使用できるツールです。 Site Recovery を使用すると、オンプレミスの VM と物理サーバーを Azure にレプリケートすることも、オンプレミスのセカンダリ ロケーションにレプリケートすることもできます。 さらには、サポートされている Azure VM、オンプレミスの "*Hyper-V*" VM、"*VMware*" VM、Windows 物理サーバー、または Linux 物理サーバー上で実行されているワークロードをレプリケートすることもできます。 Azure へのレプリケートにより、セカンダリ データセンターの管理に伴うコストと手間が削減されます。

Site Recovery は、一部がオンプレミスで、一部が Azure 上にあるハイブリッド環境専用にも作成されています。 Site Recovery の利点は、サイトがダウンした場合でも VM やオンプレミスの物理サーバー上で実行されているアプリの可用性が維持されて、ビジネス継続性が確保されることです。 VM や物理サーバー上で実行されているワークロードは、プライマリ サイトが利用できなくなった場合でもセカンダリ ロケーションで利用できるようにレプリケートされます。 ワークロードは、プライマリ サイトが稼働状態に戻った時点でプライマリ サイトに復元されます。

図 2-3 は Azure Site Recovery を使用して複数の VM 移行が実行されていることを示しています。

![クラウド インフラストラクチャ対応アプリケーションの位置付け](./media/image2-3.png)

**図 2-3** クラウド インフラストラクチャ対応アプリケーションの位置付け

### <a name="additional-resources"></a>その他の技術情報

- **Azure Migrate データシート**

    <https://aka.ms/azuremigration\_datasheet>

- **Azure Migrate**

    <https://aka.ms/azuremigrate>

- **Azure Migration Center**

    <https://azure.microsoft.com/migration/>

- **Site Recovery を使用した Azure への移行**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-to-azure>

- **Azure Site Recovery サービスの概要**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-overview>

- **AWS の VM の Azure VM への移行**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-aws-to-azure>

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](migrate-your-relational-databases-to-azure.md) <!-- Next Chapter -->
