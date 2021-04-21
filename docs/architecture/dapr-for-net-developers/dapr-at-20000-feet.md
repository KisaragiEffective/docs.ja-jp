---
title: 2 万フィートの Dapr
description: Dapr の概要、その機能、およびしくみについて説明します。
author: robvet
ms.date: 02/17/2021
ms.openlocfilehash: 9f23e9822fd0d4b5eda648d2fc1359cce14cf59d
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624046"
---
# <a name="dapr-at-20000-feet"></a>2 万フィートの Dapr

第 1 章では、分散マイクロサービス アプリケーションの魅力を説明しました。 しかし、アーキテクチャや運用上の複雑さが劇的に増すことも指摘しました。 そのことを念頭に置いて、どうすれば "それを手に入れる" だけでなく "活用することもできる" のでしょうか。 つまり、分散アーキテクチャの機敏性を活用し、複雑さを最小限に抑えるにはどうすればよいでしょうか。

Dapr (*Distributed Application Runtime*) は、最新の分散アプリケーションを構築するための新しい方法です。

プロトタイプとして開始されたことが、優れたオープンソース プロジェクトに発展しました。 そのスポンサーである Microsoft は、お客様やオープンソース コミュニティと緊密に連携して、Dapr の設計と構築を行っています。 Dapr プロジェクトには、あらゆるバックグラウンドの開発者が集結し、分散アプリケーションの開発に関する困難な課題が解決されています。

本書では、.NET 開発者の視点から見た Dapr について説明します。 この章では、Dapr とそのしくみの概念を理解します。 後で、アプリケーションで Dapr を使用する方法について、実践的な説明を提供します。

ジェット機で 2 万フィートの高さを飛ぶことを想像してください。 窓から外を見ると、大きく広がる景色が見えます。 Dapr について同じようにしてみましょう。 Dapr の 2 万フィート上を飛んでいるところを想像してください。 何が見えますか。

## <a name="dapr-and-the-problem-it-solves"></a>Dapr とそれが解決する問題

Dapr は、最新の分散アプリケーションに固有の大きな課題である **複雑さ** に対処します。

プラグ可能なコンポーネントのアーキテクチャにより、Dapr は分散アプリケーションの背後にある仕組みを大幅に簡略化します。 Dapr ランタイムのインフラストラクチャ機能とアプリケーションをバインドする **動的な接着剤** が提供されます。

サービスの 1 つをステートフルにする必要がある場合を考えてみます。 どのような設計になるでしょうか。 Redis Cache のような状態ストアを対象とするカスタム コードを記述することもできます。 しかし、Dapr を利用すれば、すぐに使用できる状態管理機能が提供されます。 Dapr の **コンポーネント構成** yaml ファイルによって状態ストア **コンポーネント** に動的にバインドする Dapr 状態管理 **構成ブロック** を、サービスで呼び出します。 Dapr には、Redis など、事前に構築された複数の状態ストア コンポーネントが付属しています。 このモデルでは、状態管理はサービスから Dapr ランタイムに委任されます。 サービスで SDK やライブラリを使用したり、基になるコンポーネントを直接参照したりすることはありません。 コードを変更せずに、状態ストアを変更することもできます (たとえば、Redis から MySQL または Cassandra に)。

図 2-1 は、2 万フィートから Dapr を見た様子です。

![2 万フィートからの Dapr](./media/dapr-at-20000-feet/dapr-high-level.png)
**図 2-1**.  2 万フィートからの Dapr。

図の一番上の行で、Dapr により一般的な開発プラットフォーム向けの言語固有の SDK が提供されるしくみに注意してください。 Dapr v 1.0 には、Go、Node.js、Python、.NET、Java、JavaScript のサポートが含まれます。 本書では、Dapr .NET SDK に焦点を当てています。これにより、ASP.NET Core の統合も直接サポートされます。

言語固有の SDK によって開発者のエクスペリエンスが強化されますが、Dapr はプラットフォームに依存しません。 内部的には、Dapr のプログラミング モデルの機能は、標準の HTTP/gRPC 通信プロトコルを介して公開されます。 任意のプログラミング プラットフォームで、ネイティブの HTTP および gRPC API を介して Dapr を呼び出すことができます。  

図の中央にある青いボックスは、Dapr の構成ブロックを表します。 それぞれにより、アプリケーションで使用できる分散アプリケーション機能が公開されます。

下の行では、Dapr とそれによって実行できるさまざまな環境の移植性が強調されています。

## <a name="dapr-architecture"></a>Dapr のアーキテクチャ

ここで、ジェット機を旋回させて Dapr の上に戻り、高度を下げて、Dapr の仕組みがよくわかるようにします。

### <a name="building-blocks"></a>構成ブロック

新しい視点からは、Dapr の **構成ブロック** が細部までよく見えます。

構成ブロックには、分散インフラストラクチャの機能がカプセル化されています。 この機能には、HTTP または gRPC の API を使用してアクセスできます。 図 2-2 は、Dapr v 1.0 で使用できるブロックを示したものです。

![Dapr の構成ブロック](./media/dapr-at-20000-feet/building-blocks.png)

**図 2-2** Dapr の構成ブロック。

次の表では、各ブロックによって提供されるインフラストラクチャ サービスについて説明します。

| 構成要素 | Description |
|----------------|-------------|
| [状態管理](state-management.md) | 長時間実行されるステートフル サービスのコンテキスト情報をサポートします。 |
| [サービスの呼び出し](service-invocation.md) | プラットフォームに依存しないプロトコルと既知のエンドポイントを使用して、直接、安全なサービス間呼び出しを呼び出します。 |
| [パブリッシュ-サブスクライブ](publish-subscribe.md) | サービス間にセキュリティで保護されたスケーラブルな pub/sub メッセージングを実装します。 |
| [バインド](bindings.md) | 双方向の通信により、外部リソースによって生成されたイベントからコードをトリガーします。 |
| [可観測性](observability.md) | ネットワークに接続されたサービス間のメッセージ呼び出しを監視して測定します。 |
| [シークレット](secrets.md) | 外部のシークレット ストアに安全にアクセスします。 |
| アクター | 再利用可能なアクター オブジェクトにロジックとデータをカプセル化します。 |

構成ブロックにより、分散アプリケーション機能の実装がサービスから抽象化されます。 図 2-3 は、この相互作用を示したものです。

![Dapr の構成ブロックの統合](./media/dapr-at-20000-feet/building-blocks-integration.png)

**図 2-3** Dapr の構成ブロックの統合。

構成ブロックにより、各リソースの具象実装を提供する Dapr コンポーネントが呼び出されます。 サービスのコードによって認識されるのは、構成ブロックだけです。 外部や SDK やライブラリへの依存関係はありません。仕組みは Dapr によって自動的に処理されます。 各構成ブロックは独立しています。 アプリケーションでは、そのうちの 1 つ、一部、またはすべてを使用できます。 付加価値として、Dapr により包括的な監視などの業界のベスト プラクティスが組み込まれます。

各 Dapr 構成ブロックの詳細な説明とコード サンプルについては、後の章で説明します。 ここで、ジェット機の高度をもっと下げます。 新しい視点からは、Dapr のコンポーネント レイヤーがよく見えます。

### <a name="components"></a>コンポーネント

構成ブロックからは、分散アプリケーション機能を呼び出すための API が公開されるのに対し、Dapr のコンポーネントからは、それを実現するための具象実装が提供されます。

Dapr の **状態ストア** コンポーネントを見てみます。 それによって、CRUD 操作の状態を管理するための一貫した方法が提供されます。 サービスのコードを変更することなく、次のいずれかの Dapr 状態コンポーネントに切り替えることができます。

- AWS DynamoDB
- Aerospike
- Azure Blob Storage
- Azure CosmosDB
- Azure Table Storage
- Cassandra
- Cloud Firestore (データストア モード)
- CloudState
- Couchbase
- Etcd
- HashiCorp Consul
- Hazelcast
- Memcached
- MongoDB
- PostgreSQL
- Redis
- RethinkDB
- SQL Server
- Zookeeper

各コンポーネントからは、共通の状態管理インターフェイスを通して必要な実装が提供されます。

```go
 type Store interface {
   Init(metadata Metadata) error
   Delete(req *DeleteRequest) error
   BulkDelete(req []DeleteRequest) error
   Get(req *GetRequest) (*GetResponse, error)
   Set(req *SetRequest) error
   BulkSet(req []SetRequest) error
}
```

> [!TIP]
> Dapr ランタイムとすべての Dapr コンポーネントは、Golang つまり Go 言語で記述されています。 Go は、オープンソース コミュニティで広く普及している言語であり、Dapr のクロスプラットフォーム コミットメントを保証するものです。

おそらく、最初に使用する状態ストアは Azure Redis Cache です。 次の構成でそれを指定します。

 ```yaml
 apiVersion: dapr.io/v1alpha1
 kind: Component
 metadata:
   name: statestore
   namespace: default
 spec:
   type: state.redis
   version: v1
   metadata:
   - name: redisHost
     value: <HOST>
   - name: redisPassword
     value: <PASSWORD>
   - name: enableTLS
     value: <bool> # Optional. Allowed: true, false.
   - name: failover
     value: <bool> # Optional. Allowed: true, false.
 ```

**spec** セクションでは、状態管理に Redis Cache を使用するように Dapr を構成します。 そのセクションには、コンポーネント固有のメタデータも含まれています。 この場合は、それを使用して Redis の追加設定を構成できます。

その後、アプリケーションを運用環境に移行する準備ができました。 運用環境では、状態管理を Azure Table Storage に変更することができます。 Azure Table Storage は、低価格で耐久性の高い状態管理機能を備えています。

これを書いている時点では、Dapr によって次のコンポーネントの種類が提供されています。

| コンポーネント | 説明 |
|-----------|-------------|
| [サービス検出](https://github.com/dapr/components-contrib/tree/master/nameresolution) | ホスト環境と統合してサービス間の検出を提供するため、サービス呼び出し構成ブロックによって使用されます。 |
| [State](https://github.com/dapr/components-contrib/tree/master/state) | さまざまな状態ストアの実装と対話するための、統一されたインターフェイスが提供されます。 |
| [pub/sub](https://github.com/dapr/components-contrib/tree/master/pubsub) | 広範なメッセージ バスの実装と対話するための、統一されたインターフェイスが提供されます。 |
| [バインド](https://github.com/dapr/components-contrib/tree/master/bindings) | 外部システムからアプリケーション イベントをトリガーし、オプションのデータ ペイロードを使用して外部システムを呼び出すための、統一されたインターフェイスが提供されます。 |
| [ミドルウェア](https://github.com/dapr/components-contrib/tree/master/middleware) | カスタム ミドルウェアを要求処理パイプラインにプラグし、要求または応答に対して追加のアクションを呼び出すことができるようにします。 |
| [シークレット ストア](https://github.com/dapr/components-contrib/tree/master/secretstores) | クラウド、エッジ、商用、オープンソース サービスなど、外部シークレット ストアと対話するための、統一されたインターフェイスが提供されます。 |

ジェット機が Dapr の上を飛び終えると、それが相互にどのように接続されているかを確認できます。

### <a name="sidecar-architecture"></a>サイドカー アーキテクチャ

Dapr の構成ブロックとコンポーネントは、[サイドカー アーキテクチャ](/azure/architecture/patterns/sidecar)を通じて公開されます。 サイドカーを使用することにより、Dapr は別のメモリ プロセスで、またはサービスと共に別のコンテナーで、実行できます。 サイドカーはサービスの一部ではなく、サービスに接続されているため、分離とカプセル化が提供されます。 この分離により、それぞれが独自のランタイム環境を持つことができ、さまざまなプログラミング プラットフォームでビルドできます。 図 2-4 は、サイドカーのパターンを示したものです。

![サイドカー アーキテクチャ](./media/dapr-at-20000-feet/sidecar-generic.png)

**図 2-4** サイドカー アーキテクチャ。

このパターンは、オートバイに取り付けられるサイドカーに似ているため、"サイドカー" と名付けられています。 前の図で、分散アプリケーション機能を提供するために、Dapr サイドカーがサービスにどのようにアタッチされているかに注目してください。

### <a name="hosting-environments"></a>ホスティング環境

Dapr はクロスプラットフォームをサポートしており、さまざまな環境で実行できます。 これらの環境には、Kubernetes、VM のグループ、Azure IoT Edge などのエッジ環境が含まれます。

ローカル開発の場合、開始する最も簡単な方法は、[セルフホステッド モード](https://docs.dapr.io/concepts/overview/#self-hosted)を使用することです。 セルフホステッド モードでは、マイクロサービスと Dapr サイドカーは、Kubernetes などのコンテナー オーケストレーターを使用せずに、個別のローカル プロセスで実行されます。 詳細については、[Dapr CLI のダウンロードとインストール](https://docs.dapr.io/getting-started/install-dapr/)に関する記事を参照してください。

図 2-5 は、HTTP または gRPC を介して通信する 2 つの別個のメモリ プロセスでホストされたアプリケーションと Dapr を示したものです。

![セルフホステッド サイドカー アーキテクチャ](./media/dapr-at-20000-feet/self-hosted-dapr-sidecar.png)

**図 2-5**. セルフホステッド Dapr サイドカー。

既定では、Dapr により、既定の状態管理と監視を提供するため、Redis と Zipkin 用の Docker コンテナーがインストールされます。 ローカル コンピューターに Docker をインストールしたくない場合は、[Docker コンテナーを使用せずに、セルフホステッド モードで Dapr を実行する](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)こともできます。 ただし、状態管理や pub/sub 用の Redis などの既定のコンポーネントを、手動でインストールする必要があります。

Dapr は、Kubernetes などの[コンテナー化された環境](https://docs.dapr.io/concepts/overview/#kubernetes-hosted)でも実行できます。 図 2-6 は、同じ Kubernetes ポッド内のアプリケーション コンテナーと共に、別のサイドカー コンテナーで実行されている Dapr を示したものです。

![Kubernetes でホストされたサイドカー アーキテクチャ](./media/dapr-at-20000-feet/kubernetes-hosted-dapr-sidecar.png)

**図 2-6**. Kubernetes でホストされた Dapr サイドカー。

## <a name="dapr-performance-considerations"></a>Dapr のパフォーマンスに関する考慮事項

既に説明したように、アプリケーションを分散アプリケーション機能から分離するため、Dapr によりサイドカー アーキテクチャが公開されます。 Dapr の操作を呼び出すには、少なくとも 1 つのプロセス外ネットワーク呼び出しが必要です。 図 2-7 に、Dapr トラフィック パターンの例を示します。

![Dapr トラフィック パターン](./media/dapr-at-20000-feet/dapr-traffic-patterns.png)

**図 2-7**. Dapr トラフィック パターン。

前の図を見ると、各呼び出しで発生する待機時間とオーバーヘッドが気になるかもしれません。  

Dapr チームは、パフォーマンスに大きく力を入れてきました。 膨大な量のエンジニアリング作業が、Dapr の効率を高めることに費やされました。 Dapr サイドカー間の呼び出しは gRPC で常に行われ、高いパフォーマンスと小さいバイナリ ペイロードが提供されます。 ほとんどの場合、追加のオーバーヘッドはミリ秒単位のはずです。

パフォーマンスを向上させるため、開発者は gRPC を使用して Dapr の構成ブロックを呼び出すことができます。

gRPC は、古い[リモート プロシージャ コール (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call) プロトコルを進化させる最新のハイパフォーマンス フレームワークです。 gRPC のトランスポート プロトコルには HTTP/2 が使用されており、次のように、HTTP RESTFul サービスより大幅にパフォーマンスが向上します。

- 同じ接続を介して複数の並列要求を送信するための多重化のサポート - HTTP 1.1 の場合、処理は一度に 1 つの要求および応答メッセージに制限されます。
- クライアント要求とサーバー応答の両方を同時に送信するための双方向全二重通信。
- 大規模なデータセットを非同期にストリーミングするための要求と応答を可能にする組み込みのストリーミング。

詳細については、「[Azure 向けクラウド ネイティブ .NET アプリケーションの設計](https://docs.microsoft.com/dotnet/architecture/cloud-native/)」電子ブックの [grpc の概要](https://docs.microsoft.com/dotnet/architecture/cloud-native/grpc#what-is-grpc)に関するページを参照してください。  

## <a name="dapr-and-service-meshes"></a>Dapr とサービス メッシュ

サービス メッシュは、分散アプリケーションに関して急速に進化しているもう 1 つのテクノロジです。

サービス メッシュは、サービス間の通信、回復性、負荷分散、テレメトリ キャプチャを処理する機能が組み込まれた、構成可能なインフラストラクチャ レイヤーです。 それにより、これらの問題に対する責任は、サービスからサービス メッシュ レイヤーに移動されます。 Dapr と同様に、サービス メッシュもサイドカー アーキテクチャに従います。

図 2-8 に示すのは、サービス メッシュ テクノロジを実装したアプリケーションです。

![サービス メッシュ](./media/dapr-at-20000-feet/service-mesh-with-side-car.png)

**図 2-8**. サイドカーを使用したサービス メッシュ。

前の図では、各サービスと共に実行されるサイドカー プロキシによってメッセージがインターセプトされる方法が示されています。 各プロキシは、サービスに固有のトラフィック ルールを使用して構成できます。 それによりメッセージが認識され、サービスや外部との間でルーティングできます。

そうなると、"Dapr はサービス メッシュか" ということが疑問になります。

どちらもサイドカー アーキテクチャを使用しますが、各テクノロジの目的は異なります。 Dapr では、分散アプリケーション機能が提供されます。 サービス メッシュでは、専用のネットワーク インフラストラクチャ レイヤーが提供されます。

それぞれが異なるレベルで動作するため、両方とも同じアプリケーションで連携できます。 たとえば、サービス メッシュによって、サービス間のネットワーク通信を提供できます。 Dapr によって、状態管理やアクター サービスなどのアプリケーション サービスを提供できます。

図 2-9 に示すのは、Dapr とサービス メッシュ テクノロジの両方を実装したアプリケーションです。

![Dapr とサービス メッシュの併用](./media/dapr-at-20000-feet/dapr-and-service-mesh.png)

**図 2-9**. Dapr とサービス メッシュの併用。

[Dapr のオンライン ドキュメント](https://docs.dapr.io/concepts/faq/#networking-and-service-meshes)で、Dapr とサービス メッシュの統合について説明されています。

## <a name="summary"></a>まとめ

この章では、分散アプリケーション ランタイムである Dapr について紹介しました。

Dapr は、Microsoft がスポンサーとなり、お客様やオープンソース コミュニティとの密接な共同作業によって行われた、オープンソース プロジェクトです。

Dapr の中核となる部分は、分散マイクロサービス アプリケーションの本質的な複雑さを軽減するのに役立ちます。 これは、構成ブロック API の概念に基づいて構築されています。 Dapr の構成ブロックにより、状態管理、サービス間呼び出し、pub/sub メッセージングなどの一般的な分散アプリケーション機能が公開されます。 構成ブロックの下にある Dapr コンポーネントにより、各機能の具象実装が提供されます。 アプリケーションは、構成ファイルを介してさまざまなコンポーネントにバインドします。

次の章では、アプリケーションで Dapr を使用する方法について、実践的な説明を提供します。

### <a name="references"></a>リファレンス

- [Dapr のドキュメント](https://dapr.io/)
- [Dapr の学習](https://www.amazon.com/Learning-Dapr-Building-Distributed-Applications/dp/1492072427/ref=sr_1_1?dchild=1&keywords=dapr&qid=1604794794&sr=8-1)
- [.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)
- [Azure 向けクラウド ネイティブ .NET アプリの設計](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> [前へ](the-world-is-distributed.md)
> [次へ](getting-started.md)
