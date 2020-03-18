---
title: ハイブリッド クラウド シナリオへの移行
description: Azure クラウドおよび Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | ハイブリッド クラウド シナリオへの移行
ms.date: 04/30/2018
ms.openlocfilehash: dcbb799a45609f8bb811866c4041951abf1fda8b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937360"
---
# <a name="migrate-to-hybrid-cloud-scenarios"></a>ハイブリッド クラウド シナリオへの移行

一部の組織や企業では、規制や独自のポリシーが理由で、アプリケーションの一部を Microsoft Azure を始めとするパブリック クラウドに移行することができません。 しかしながら、どの組織でも、一部のアプリケーションをパブリック クラウドに、その他のアプリケーションをオンプレミスに配置することでメリットが得られる可能性があります。 ところが、パブリック クラウドとオンプレミス環境では使用されるプラットフォームとテクノロジが異なるため、環境が混在することにより、過度に複雑化する場合があります。

Microsoft は最適なハイブリッド クラウド ソリューションを提供しています。そこでは、オンプレミスとパブリック クラウドの既存の資産を最適化しながら、Azure ハイブリッド クラウドの一貫性を確保できます。 Azure Stack (オンプレミス) と Azure (パブリック クラウド) のおかげで、既存のスキルを最大限活用し、クラウドまたはオンプレミスで実行できるアプリを構築するための柔軟で統合されたアプローチが得られます。

セキュリティに関しては、ハイブリッド クラウド全体で管理とセキュリティを一元化できます。 オンプレミスとクラウドのアプリにシングル サインオンを提供することで、データセンターからクラウドまで、すべての資産を制御できます。 これを実現するには、Active Directory をハイブリッド クラウドに拡張し、ID 管理を使用します。

最後に、データのソースには関係なく、データをシームレスに分散して分析し、クラウドとオンプレミスの資産に同じクエリ言語を使用し、Azure で分析とディープ ラーニングを適用してデータを強化することができます。

## <a name="azure-stack"></a>Azure Stack

Azure Stack は、組織のデータセンターから Azure サービスを提供できるようにするハイブリッド クラウド プラットフォームです。 Azure Stack は、エッジ環境や非接続環境など、主要なシナリオで最新アプリケーション向けの新しいオプションをサポートし、セキュリティとコンプライアンスの特定の要件に対応できるように設計されています。

図 4-13 は、Microsoft が提供する真のハイブリッド クラウド プラットフォームの概要を示しています。

![Azure Stack と Azure を使用した Microsoft ハイブリッド クラウド プラットフォームの図。](./media/migrate-to-hybrid-cloud-scenarios/microsoft-hybrid-cloud-platform.png)

**図 4-13** Azure Stack と Azure を使用した Microsoft ハイブリッド クラウド プラットフォーム

Azure Stack には、お客様のニーズに合わせて次の 2 つのデプロイ オプションが用意されています。

- Azure Stack 統合システム

- Azure Stack Development Kit

### <a name="azure-stack-integrated-systems"></a>Azure Stack 統合システム

Azure Stack 統合システムは、Microsoft とハードウェア パートナーのパートナーシップを通じて提供されます。 このパートナーシップでは、管理の簡略化とのバランスが取れたクラウドベースのイノベーションを提供するソリューションが作成されます。 Azure Stack はハードウェアとソフトウェアの統合システムとして提供されるため、クラウドからのイノベーションを引き続き採用しながら、適度な柔軟性と制御性を得られます。 Azure Stack 統合システムは、4 ノードから 12 ノードまでのサイズがあり、ハードウェア パートナーと Microsoft によって共同でサポートされます。 Azure Stack 統合システムを使用して、運用ワークロード向けに新しいシナリオを実装します。

### <a name="azure-stack-development-kit"></a>Azure Stack Development Kit

Microsoft Azure Stack Development Kit は、Azure Stack の評価と学習に使用できる Azure Stack の単一ノード デプロイです。 Azure Stack Development Kit は開発環境としても使用でき、Azure と一貫性のある API とツールを使用して開発できます。 Azure Stack Development Kit は、運用環境での使用を想定していません。

### <a name="additional-resources"></a>その他のリソース

- **Azure ハイブリッド クラウド**

    <https://azure.microsoft.com/overview/hybrid-cloud/>

- **Azure Stack**

    <https://azure.microsoft.com/overview/azure-stack/>

- **Windows コンテナー用の Active Directory サービス アカウント**

    <https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts>

- **Active Directory サポートを含むコンテナーの作成**

    <https://docs.microsoft.com/archive/blogs/containerstuff/create-a-container-with-active-directory-support>

- **Azure ハイブリッド特典のライセンス**

    <https://azure.microsoft.com/pricing/hybrid-benefit/>

>[!div class="step-by-step"]
>[前へ](life-cycle-ci-cd-pipelines-devops-tools.md)
>[次へ](../walkthroughs-technical-get-started-overview.md)
