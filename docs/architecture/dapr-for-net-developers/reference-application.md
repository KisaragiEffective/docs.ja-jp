---
title: eShopOnDapr 参照アプリケーションの概要
description: eShopOnDapr 参照アプリケーションとその履歴の概要。
author: amolenk
ms.date: 02/17/2021
ms.openlocfilehash: 47d99246eec8e3288738c911cccb23bb9cac8df3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874564"
---
# <a name="dapr-reference-application"></a>Dapr 参照アプリケーション

前の記事で、Dapr の基本的な利点について説明しました。 Dapr を使用すると、アーキテクチャや運用上の複雑さを軽減しながら、分散アプリケーションを構築できることを見ました。 その過程で、小さな Dapr アプリを構築する機会がありました。 ここでは、Dapr の構成ブロックとコンポーネントのことがわかるエンドツーエンドのマイクロサービス アプリケーションを調べます。

ただし、まずは歴史について少し説明します。

## <a name="eshop-on-containers"></a>コンテナーでの eShop

数年前、Microsoft は主要なコミュニティ エキスパートと協力して、『[.NET Microservices for Containerized .NET Applications (コンテナー化された .NET アプリケーション用の .NET マイクロサービス)](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)』という一般向けのガイダンス ブックをリリースしました。 図 3-1 で示されている書籍:

![コンテナー化されたマイクロサービス .NET アプリケーションの設計。](./media/reference-application/architecting-microservices-book.png)

**図 3-1**. .NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ。

この書籍では、分散アプリケーションを構築するための原則、パターン、ベスト プラクティスが詳しく解説されていました。 そして、アーキテクチャの概念を紹介する完全な機能を備えたマイクロサービス参照アプリケーションが含まれていました。 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) というこのアプリケーションでは、さまざまな .NET アイテム (衣料やコーヒー マグなど) を販売する eコマース ショップが示されています。  .NET Core に組み込まれているアプリケーションはクロスプラットフォームで、Linux または Windows コンテナーのいずれかで実行できます。 図 3-2 は、元の eShop のアーキテクチャを示したものです。

![eShopOnContainers 参照アプリケーションのアーキテクチャ。](./media/reference-application/eshop-on-containers.png)

**図 3-2**. 元の `ShopOnContainers` 参照アプリケーション。

ご覧のように、eShopOnContainers には多くの可動部分が含まれています。

1. 3 つの異なるフロントエンド クライアント。
1. フロントエンドからバックエンドを抽象化するアプリケーション ゲートウェイ。
1. 複数のバックエンド コア マイクロサービス。
1. 非同期の pub/sub メッセージングを可能にするイベント バス コンポーネント。

eShopOnContainers 参照アプリケーションは、.NET コミュニティ全体で広く受け入れられ、多数の大規模な商用マイクロサービス アプリケーションをモデル化するために使用されています。

## <a name="eshop-on-dapr"></a>Dapr での eShop

この記事には、eShop アプリケーションのもう 1 つのバージョンが付属しています。 それは [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) という名前です。 この更新バージョンは、Dapr の構成ブロックとコンポーネントを統合することによって、以前の eShopOnContainers アプリケーションを発展させたものです。 新しい合理化されたソリューションのアーキテクチャを図 3-3 に示します。  

![eShopOnDapr 参照アプリケーションのアーキテクチャ。](./media/reference-application/eshop-on-dapr.png)

**図 3-3**. eShopOnDapr 参照アプリケーションのアーキテクチャ。

eShopOnDapr 参照アプリケーションの焦点は Dapr であり、元のアプリケーションが更新されています。 このアーキテクチャは、次のもので構成されています。

1. 広く使用されている Angular SPA フレームワークで記述された[シングルページ アプリケーション](/archive/msdn-magazine/2013/november/asp-net-single-page-applications-build-modern-responsive-web-apps-with-asp-net) フロントエンド。 それにより、API ゲートウェイ マイクロサービスにユーザー要求が送信されます。

1. API ゲートウェイにより、フロントエンド クライアントからバックエンド コア マイクロサービスが抽象化されます。 高パフォーマンスのオープンソース サービス プロキシである [Envoy](https://www.envoyproxy.io/) を使用して実装されています。 Envoy により、着信した要求はさまざまなバックエンド マイクロサービスにルーティングされます。 ほとんどの要求は単純な CRUD 操作 (たとえば、カタログからのブランドの一覧の取得など) であり、バックエンド マイクロサービスを直接呼び出すことによって処理されます。

1. その他の要求は論理的に複雑で、複数のマイクロサービスの連携が必要です。 このような場合のために eShopOnDapr に実装されている[アグリゲーター マイクロサービス](../cloud-native/service-to-service-communication.md#service-aggregator-pattern)により、操作を完了するために必要なマイクロサービス間のワークフローが調整されます。

1. 一連のコア バックエンド マイクロサービスには、eコマース ストアに必要な機能が含まれています。 それぞれは自己完結しており、互いに独立しています。 広く受け入れられているドメイン分解パターンに従って、各マイクロサービスにより特定の "*ビジネス機能*" が分離されています。

   - バスケット サービスにより、顧客のショッピング バスケットのエクスペリエンスが管理されます。
   - カタログ サービスにより、販売できる製品アイテムが管理されます。
   - ID サービスにより、認証と ID が管理されます。
   - 注文サービスにより、注文の実行と管理のすべての側面が処理されます。
   - 支払いサービスにより、顧客の支払いが処理されます。

   各サービスには、専用の永続ストレージがあります。 マイクロサービスの[ベスト プラクティス](../cloud-native/distributed-data.md#database-per-microservice-why)に従って、すべてのサービスがやり取りする共有データ ストアはありません。

   各マイクロサービスの設計は、個別の要件に基づいています。 単純なサービスで基になるデータ ストアにアクセスするには、基本的な CRUD 操作が使用されます。 注文などの高度なサービスでは、ドメイン駆動設計アプローチを使用して、ビジネスの複雑さが管理されます。 必要な場合は、.NET Core、Java、Go、NodeJS などの異なるテクノロジ スタックをまたいで、サービスを構築できます。

1. 最後に、イベント バスにより、Dapr のパブリッシュ-サブスクライブ コンポーネントがラップされます。 それにより、マイクロサービス間での非同期のパブリッシュ-サブスクライブ メッセージングが可能になります。 開発者は、Dapr でサポートされている任意のメッセージ ブローカーを組み込むことができます。

### <a name="application-of-dapr-building-blocks"></a>Dapr 構成ブロックの適用

eShopOnDapr のコードベースは、eShopOnContainers のコードベースより簡素化されています。 Dapr の構成ブロックによって、エラーが発生しやすい仕組みの大量のコードが置き換えられています。

図 3-4 は、eShop 参照アプリケーションにおける Dapr の統合を示したものです。

![eShopOnDapr 参照アプリケーションのアーキテクチャ](./media/reference-application/eshop-on-dapr-buildingblocks.png)

**図 3-4**. eShopOnDapr での Dapr の統合。

前の図では、どのサービスによってどの Dapr 構成ブロックが使用されているかがわかります。

1. 元の eShopOnContainers アプリケーションの注文サービスでは、DDD の概念とパターンが示されています。 更新された eShopOnDapr の注文サービスでは、代替の実装として "*アクター構成ブロック*" が使用されています。 アクターのターン ベースのアクセス モデルを使用すると、キャンセルがサポートされているステートフルな注文プロセスを簡単に実装できます。
1. 注文サービスにより、[バインド構成ブロック](bindings.md)を使用して、注文確認メールが送信されます。
1. バックエンド サービスによる非同期通信は、[パブリッシュ-サブスクライブ構成ブロック](publish-subscribe.md)を使用して行われます。
1. シークレットの管理は、[シークレット構成ブロック](secrets.md)によって行われます。
1. API ゲートウェイと Web ショッピング アグリゲーター サービスで、バックエンド サービスのメソッドを呼び出すには、[サービス呼び出し構成ブロック](service-invocation.md)が使用されます。
1. バスケット サービスで顧客の買い物かごの状態を格納するには、[状態管理構成ブロック](state-management.md)が使用されます。

### <a name="benefits-of-applying-dapr-to-eshop"></a>eShop に Dapr を適用する利点

一般に、Dapr の構成ブロックを使用すると、アプリケーションの監視と柔軟性が向上します。

1. 監視: Dapr の構成ブロックを使用することにより、コードを記述しなくても、サービス間および Dapr コンポーネントの両方の呼び出しについて、豊富な分散トレースを取得できます。 eShopOnContainers では、分析情報を提供するために大量のカスタム ログが使用されます。
1. 柔軟性: コンポーネント構成ファイルを変更するだけで、インフラストラクチャを "*入れ替える*" ことができるようになります。 コードに変更を加える必要はありません。

次に、特定の構成ブロックで提供される利点の例をさらにいくつか示します。

- **サービスの呼び出し**
  - Dapr では [mTLS](https://blog.cloudflare.com/introducing-tls-client-auth/) がサポートされているので、サービスは暗号化されたチャネルを介して通信できるようになります。
  - 一時的なエラーが発生すると、サービス呼び出しは自動的に再試行されます。
  - 自動的なサービス検出により、サービスの相互検出に必要な構成の量を減らすことができます。

- **パブリッシュ-サブスクライブ**
  - eShopOnContainer には、Azure Service Bus と RabbitMQ の 両方をサポートするために、大量のカスタム コードが含まれていました。 開発者は、運用には Azure Service Bus を使用し、ローカル環境での開発とテストには RabbitMQ を使用していました。 これらのメッセージ ブローカーの入れ替えを可能にするため、`IEventBus` 抽象化レイヤーが作成されました。 このレイヤーは、約 "*700 行のエラーが発生しやすいコード*" で構成されていました。 Dapr を使用して更新された実装で必要なのは、わずか "*35 行のコード*" です。 つまり、元のコード行の **5%** です。 さらに重要なのは、実装が簡単でわかりやすいことです。
  - eShopOnDapr で pub/sub を使用するには、Dapr の豊富な ASP.NET Core 統合が使用されています。 メッセージをサブスクライブするには、ASP.NET Core コントローラー メソッドに `Topic` 属性を追加します。 したがって、メッセージ ブローカーごとに個別のメッセージ ハンドラー ループを記述する必要はありません。
  - HTTP 呼び出しとしてサービスにルーティングされるメッセージにより、ASP.NET Core ミドルウェアを使用して機能を追加することができます。新しい概念や SDK を導入して学習する必要はありません。

- **バインド**
  - eShopOnContainers ソリューションには、顧客に注文確認メールを送信するための *To-Do* 項目が含まれていました。 最終的に、SendGrid などのサードパーティ製のメール API を実装することになりました。 Dapr でのメール通知の実装は、リソース バインドを構成するだけの簡単なものでした。 外部 API や SDK について学習する必要はありませんでした。

> [!NOTE]
> アクター構成ブロックは、この記事の最初のバージョンでは説明されていません。 アクター構成ブロックおよびその eShopOnDapr との統合についての広範な章は、1.1 の更新で盛り込まれる予定です。

## <a name="summary"></a>まとめ

この章では、eShopOnDapr 参照アプリケーションについて紹介しました。 それは、広く普及している eShopOnContainers マイクロサービス参照アプリケーションが進化したものです。 eShopOnDapr では、大量のカスタム機能が Dapr の構成ブロックとコンポーネントに置き換えられ、マイクロサービス アプリケーションの構築に必要な複雑さが劇的に簡素化されています。

### <a name="references"></a>リファレンス

- [GitHub の eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)

- [GitHub の eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)

- [コンテナー化された .NET アプリケーション用の .NET マイクロサービス](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)

- [Azure 向けクラウド ネイティブ .NET アプリの設計](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> [前へ](getting-started.md)
> [次へ](state-management.md)
