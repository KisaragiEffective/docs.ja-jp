---
title: クラウドネイティブ向けアプリ候補
description: クラウドネイティブアプローチによってメリットが得られるアプリケーションの種類について説明します
author: robvet
ms.date: 08/20/2019
ms.openlocfilehash: 2087ef0c327a82419be95552293d1b56742b73c7
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337431"
---
# <a name="candidate-apps-for-cloud-native"></a>クラウドネイティブ向けアプリ候補

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ポートフォリオ内のアプリを確認します。 クラウドネイティブアーキテクチャについては、そのうちのどれが適していますか。 全部ですか。 おそらく、

コストと利益の分析を適用すると、クラウドネイティブの hefty price タグがサポートされない可能性が高くなります。 クラウドネイティブのコストは、アプリケーションのビジネス価値をはるかに超えています。

クラウドネイティブの候補となるアプリケーションの種類

- ビジネス機能や機能を絶えず進化させる必要がある、大規模な戦略的エンタープライズシステム

- 高い信頼性を備えた高いリリース速度を必要とするアプリケーション

- システム全体を完全に再展開*せず*に、個々の機能をリリースする必要があるシステム

- さまざまなテクノロジスタックに専門知識を持つチームによって開発されたアプリケーション

- 個別に拡張する必要があるコンポーネントを含むアプリケーション

レガシシステムがあります。 新しいアプリケーションを構築する必要がありますが、多くの場合、ビジネスにとって重要な従来のワークロードの最新化を担当しています。 時間の経過と共に、レガシアプリケーションをマイクロサービスに分解し、コンテナー化し、最終的にはクラウドネイティブアーキテクチャに "replatformed" することができます。

### <a name="modernizing-legacy-apps"></a>最新化レガシアプリ

[Azure クラウドおよび Windows コンテナーを使用した既存の .net アプリケーション](https://dotnet.microsoft.com/download/thank-you/modernizing-existing-net-apps-ebook)の最新化に関する Microsoft の電子書籍では、オンプレミスのワークロードをクラウドに移行するためのガイダンスが提供されています。 図1-10 に、レガシアプリケーションを最新化するための単一の1つのサイズに適合しない方法が示されています。

![従来のワークロードを移行するための戦略](./media/strategies-for-migrating-legacy-workloads.png)

**図 1-10**. 従来のワークロードを移行するための戦略

非常に重要ではないモノリシックアプリは、迅速なリフトアンドシフト ([クラウドインフラストラクチャ](../modernize-with-azure-containers/lift-and-shift-existing-apps-azure-iaas.md)対応) の移行によるメリットがあります。 ここでは、オンプレミスのワークロードは変更なしでクラウドベースの VM に再ホストされます。 このアプローチでは、 [IaaS (Infrastructure as a Service) モデル](https://azure.microsoft.com/overview/what-is-iaas/)を使用します。 Azure には、このような移動を容易にするための ([Azure Migrate](https://aka.ms/azuremigrate)、 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/)、 [Azure Database Migration Service](https://azure.microsoft.com/campaigns/database-migration/)) などのツールがいくつか含まれています。 この戦略ではコストを削減できますが、このようなアプリケーションは通常、クラウドコンピューティングのメリットをロック解除して活用するように設計されていません。

ビジネスにとって重要なモノリシックアプリは、強化されたリフトアンドシフト (*クラウド最適化*) 移行によってメリットがあります。 このアプローチには、アプリケーションのコアアーキテクチャを変更せずに、キークラウドサービスを有効にするデプロイの最適化が含まれています。 たとえば、アプリケーションを[コンテナー化](https://docs.microsoft.com/virtualization/windowscontainers/about/)して、 [Azure Kubernetes Services](https://azure.microsoft.com/services/kubernetes-service/)のようなコンテナー orchestrator にデプロイすることがあります。これについては後で説明します。 クラウドでは、アプリケーションは、データベース、メッセージキュー、監視、分散キャッシュなどの他のクラウドサービスを使用できます。

最後に、戦略的なエンタープライズ機能を実行するモノリシックアプリでは、*クラウドネイティブ*アプローチ (この書籍の主題) から最適なメリットが得られます。 このアプローチは機敏性とベロシティを実現します。 しかし、基幹、再設計、リライトコードのコストがかかります。

お客様とチームがクラウドにネイティブなアプローチを考えている場合は、組織で決定を合理化することを behooves ます。 クラウドネイティブアプローチによって解決されるビジネス上の問題は、どのようなものですか。 ビジネスニーズとどのように連携しますか。

- 信頼性が向上した機能の迅速なリリース

- 細かいスケーラビリティ-リソースの効率的な使用

- システムの回復性が向上しましたか?

- システムパフォーマンスの向上

- 操作の可視性

- Blend の開発プラットフォームとデータストアは、ジョブに最適なツールに到達できますか。

- 将来のアプリケーション投資

適切な移行戦略は、組織の優先順位と対象とするシステムによって異なります。 多くの場合、モノリシックアプリケーションをクラウドで最適化したり、N 層アプリに粗いサービスを追加したりすると、コスト効率が向上します。 このような場合でも、Azure App Service によって提供されるもののようなクラウド PaaS 機能を十分に活用できます。

## <a name="summary"></a>要約

この章では、クラウドネイティブコンピューティングを導入しました。 クラウドネイティブアプリケーションを駆動する主な機能と共に、定義を提供しました。 この投資と労力を正当化するアプリケーションの種類について見てきました。

ここでは、クラウドネイティブの詳細について説明します。

### <a name="references"></a>参照

- [クラウドネイティブコンピューティングファンデーション](https://www.cncf.io/)

- [.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [Azure クラウドおよび Windows コンテナーで既存の .NET アプリケーションを最新化する](https://dotnet.microsoft.com/download/thank-you/modernizing-existing-net-apps-ebook)

- [Cornelia Davis によるクラウドネイティブパターン](https://www.manning.com/books/cloud-native-patterns)

- [12要素を超えるアプリケーション](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)

- [コードとしてのインフラストラクチャとは](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code)

- [Uber Engineering のマイクロデプロイ: 自信を持って毎日デプロイ](https://eng.uber.com/micro-deploy/)

- [Netflix によるコードのデプロイ方法](https://www.infoq.com/news/2013/06/netflix/)

- [マイクロサービスでの Wat のスケーリングのためのオーバーロードコントロール](https://www.cs.columbia.edu/~ruigu/papers/socc18-final100.pdf)

>[!div class="step-by-step"]
>[前へ](definition.md)
>[次へ](introduce-eshoponcontainers-reference-app.md)
