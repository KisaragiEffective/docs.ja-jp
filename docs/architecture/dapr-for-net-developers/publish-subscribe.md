---
title: Dapr のパブリッシュ-サブスクライブ構成ブロック
description: Dapr のパブリッシュ-サブスクライブ構成ブロックとその適用方法についての説明
author: edwinvw
ms.date: 02/17/2021
ms.openlocfilehash: 11898430d897ec85b7e367fa0e93ca912279784b
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623825"
---
# <a name="the-dapr-publish--subscribe-building-block"></a>Dapr のパブリッシュ-サブスクライブ構成ブロック

[パブリッシュ-サブスクライブ パターン](/azure/architecture/patterns/publisher-subscriber) (多くの場合、"pub/sub" と呼ばれます) は、よく知られ、広く使用されているメッセージング パターンです。 通常、設計者はそれを分散アプリケーションで採用します。 ただし、それを実装するための仕組みは複雑な場合があります。 多くの場合、メッセージング製品が違うと、機能に微妙な違いがあります。 Dapr には、pub/sub 機能の実装が大幅に簡単になる構成ブロックが用意されています。

## <a name="what-it-solves"></a>解決される内容

パブリッシュ-サブスクライブ パターンの主な利点は **疎結合** であり、これは[一時的な切り離し](/azure/architecture/guide/technology-choices/messaging#decoupling)と呼ばれることもあります。 このパターンを使用すると、メッセージを送信するサービス (**パブリッシャー**) がメッセージを使用するサービス (**サブスクライバー**) から切り離されます。 パブリッシャーとサブスクライバーは、相互のことを認識していません。どちらも、メッセージを配信する集中 **メッセージ ブローカー** に依存しています。

図 7-1 は、pub/sub パターンの高レベルのアーキテクチャを示したものです。

![pub/sub パターン](./media/publish-subscribe/pub-sub-pattern.png)

**図 7-1**。 pub/sub パターン。

前の図で示されているパターンの手順は次のようになります。

1. パブリッシャーによって、メッセージ ブローカーにメッセージが送信されます。
1. サブスクライバーによって、メッセージ ブローカーのサブスクリプションにバインドされます。
1. メッセージ ブローカーによって、関心のあるサブスクリプションにメッセージのコピーが転送されます。
1. サブスクライバーによって、サブスクリプションからメッセージが使用されます。

ほとんどのメッセージ ブローカーによって、受信後のメッセージを保持できるキュー メカニズムがカプセル化されています。 それを使用してメッセージを格納することにより、メッセージ ブローカーでの **持続性** が保証されます。 パブリッシャーからメッセージが送信された時点で、サブスクライバーはすぐに使用できる状態でなくてもかまわず、オンラインである必要さえありません。 サブスクライバーは、使用可能になると、メッセージを受信して処理します。  Dapr により、メッセージの配信についての **At-Least-Once** (少なくとも 1 回) セマンティクスが保証されます。 発行されたメッセージは、対象のサブスクライバーに少なくとも 1 回配信されます。

 > サービスでメッセージを処理できるのが 1 回だけの場合は、同じメッセージが複数回処理されないように、[べき等チェック](/azure/architecture/microservices/design/api-design#idempotent-operations)を提供する必要があります。 そのようなロジックをコーディングすることもできますが、Azure Service Bus などの一部のメッセージ ブローカーには、組み込みの "*重複検出*" メッセージング機能が用意されています。

商用とオープンソースの両方に、使用できるメッセージ ブローカー製品が複数あります。 それぞれに長所と短所があります。 お客様は、システムの要件を適切なブローカーを一致させる必要があります。 選択した後のベスト プラクティスは、アプリケーションをメッセージ ブローカーの仕組みから切り離すことです。 この機能を実現するには、"*抽象化*" の内側にブローカーをラップします。 抽象化によって、メッセージの仕組みがカプセル化され、汎用の pub/sub 操作がコードに公開されます。 コードは、実際のメッセージ ブローカーではなく、抽象化と通信します。 これは賢明な決定ですが、抽象化とその基になる実装を作成して維持する必要があります。 この方法には、複雑で反復的でエラーが発生しやすい可能性のあるカスタム コードが必要です。

Dapr のパブリッシュ-サブスクライブ構成ブロックを使用すると、すぐに使用できるメッセージングの抽象化と実装が提供されます。 記述する必要があったカスタム コードは、事前に作成され、Dapr の構成ブロック内にカプセル化されています。 お客様はそれにバインドして使用します。 メッセージングの仕組みのコードを記述する代わりに、お客様は顧客向けの価値を高めるビジネス機能の作成に集中できます。

## <a name="how-it-works"></a>しくみ

Dapr のパブリッシュ-サブスクライブ構成ブロックは、メッセージを送受信するための、プラットフォームに依存しない API フレームワークを備えています。 お客様のサービスでは、名前付きの[トピック](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#topics-and-subscriptions)にメッセージを発行します。 サービスでメッセージを使用するには、トピックをサブスクライブします。

サービスで、Dapr サイドカーの pub/sub API を呼び出します。 その後、サイドカーにより、特定のメッセージ ブローカー製品がカプセル化されている定義済みの Dapr pub/sub コンポーネントが呼び出されます。 図 7-2 は、Dapr の pub/sub メッセージング スタックを示したものです。

![pub/sub スタック](./media/publish-subscribe/pub-sub-buildingblock.png)

**図 7-2**。 Dapr の pub/sub スタック。

Dapr のパブリッシュ-サブスクライブ構成ブロックは、さまざまな方法で呼び出すことができます。

最も低いレベルでは、任意のプログラミング プラットフォームで、**Dapr のネイティブ API** を使用して、HTTP または gRPC 経由で構成ブロックを呼び出すことができます。 メッセージを発行するには、次の API 呼び出しを行います。

``` http
http://localhost:<dapr-port>/v1.0/publish/<pub-sub-name>/<topic>
```

上記の呼び出しには、Dapr 固有の URL セグメントがいくつかあります。

- `<dapr-port>` で、Dapr サイドカーがリッスンしているポート番号を指定します。
- `<pub-sub-name>` で、選択された Dapr pub/sub コンポーネントの名前を指定します。
- `<topic>` で、メッセージを発行するトピックの名前を指定します。

*curl* コマンド ライン ツールを使用してメッセージを発行することで、試してみることができます。

``` curl
curl -X POST http://localhost:3500/v1.0/publish/pubsub/newOrder \
  -H "Content-Type: application/json" \
  -d '{ "orderId": "1234", "productId": "5678", "amount": 2 }'
```

メッセージを受信するには、トピックをサブスクライブします。 起動時に、Dapr ランタイムによって、既知のエンドポイントでアプリケーションが呼び出され、必要なサブスクリプションが特定されて作成されます。

``` http
http://localhost:<appPort>/dapr/subscribe
```

- `<appPort>` で、アプリケーションがリッスンしているポートを Dapr サイドカーに通知します。

このエンドポイントはお客様自身が実装できます。 ただし、Dapr により、さらに直感的な実装方法が提供されています。 この機能については、この章で後ほど説明します。

呼び出しからの応答には、アプリケーションでサブスクライブしているトピックの一覧が含まれます。 それぞれには、トピックでメッセージが受信されたときに呼び出すエンドポイントが含まれます。 応答の例を次に示します。

```json
[
  {
    "pubsubname": "pubsub",
    "topic": "newOrder",
    "route": "/orders"
  },
  {
    "pubsubname": "pubsub",
    "topic": "newProduct",
    "route": "/productCatalog/products"
  }
]
```

JSON の応答を見ると、アプリケーションによってトピック `newOrder` と `newProduct` のサブスクライブが要求されていることがわかります。 それぞれについて、エンドポイント `/orders` と `/productCatalog/products` が登録されています。 どちらのサブスクリプションでも、アプリケーションは `pubsub` という名前の Dapr コンポーネントにバインドしています。

図 7-3 に、例のフローを示します。

![Dapr での pub/sub フローの例](media/publish-subscribe/pub-sub-flow.png)

**図 7-3**。 Dapr での pub/sub フロー。

前の図では、次のフローに注意してください。

1. サービス B の Dapr サイドカーにより、サービス B (コンシューマー) の `/dapr/subscribe` エンドポイントが呼び出されます。 サービスからは、作成を望むサブスクリプションが応答されます。
1. サービス B の Dapr サイドカーにより、要求されたサブスクリプションがメッセージ ブローカーに作成されます。
1. サービス A により、サービス A の Dapr サイドカーの `/v1.0/publish/<pub-sub-name>/<topic>` エンドポイントにメッセージが発行されます。
1. サービス A のサイドカーにより、メッセージブローカーにメッセージが発行されます。
1. メッセージ ブローカーにより、メッセージのコピーがサービス B に送信されます。
1. サービス B のサイドカーにより、サービス B のサブスクリプションに対応するエンドポイント (この場合は `/orders`) が呼び出されます。サービスは HTTP 状態コード `200 OK` で応答し、サイドカーはメッセージが正常に処理されているものと判断します。

この例では、メッセージは正常に処理されます。 しかし、サービス B による要求の処理中に何らかの問題が発生した場合は、応答を使用して、メッセージに必要な処理方法を指定できます。 HTTP 状態コード `404` が返されると、エラーがログに記録され、メッセージは破棄されます。 `200` または `404` 以外の状態コードの場合は、警告がログに記録され、メッセージは再試行されます。 または、サービス B で、応答の本文に JSON ペイロードを含めることにより、メッセージに必要な処理を明示的に指定できます。

```json
{
  "status": "<status>"
}
```

次の表は、使用可能な `status` の値を示したものです。

| Status           | アクション                                                       |
| ---------------- | ------------------------------------------------------------ |
| SUCCESS          | メッセージは、正常に処理されて破棄されたものと見なされます。 |
| 再試行            | メッセージは再試行されます。                                      |
| DROP             | 警告がログに記録され、メッセージは破棄されます。              |
| その他の状態 | メッセージは再試行されます。                                      |

### <a name="competing-consumers"></a>競合コンシューマー

トピックをサブスクライブするアプリケーションをスケールアウトする場合は、競合コンシューマーに対処する必要があります。 トピックに送信されたメッセージは、1 つのアプリケーション インスタンスだけで処理する必要があります。 さいわい、その問題は Dapr によって処理されます。 同じアプリケーション ID を持つサービスの複数のインスタンスによって 1 つのトピックがサブスクライブされている場合、各メッセージは Dapr によってそのうちの 1 つだけに配信されます。

### <a name="sdks"></a>SDK

ネイティブの Dapr API に対する HTTP の呼び出しは、時間がかかり、抽象的なものです。 呼び出しは HTTP レベルで作成されるため、シリアル化や HTTP 応答コードなどの仕組みに関する問題を処理する必要があります。 幸いにも、さらに直感的な方法があります。 Dapr には、一般的な開発プラットフォーム向けに複数の言語固有の SDK が用意されています。 このドキュメントの執筆時点では、Go、Node.js、Python、.NET、Java、JavaScript を利用できます。

## <a name="use-the-dapr-net-sdk"></a>Dapr .NET SDK を使用する

.NET 開発者には、[Dapr .NET SDK](https://www.nuget.org/packages/Dapr.Client) により、Dapr をいっそう生産的に操作する方法が提供されます。 SDK によって公開される `DaprClient` クラスを使用すると、Dapr の機能を直接呼び出すことができます。 これは直感的で使いやすい方法です。

メッセージを発行するには、`DaprClient` で `PublishEventAsync` メソッドが公開されています。

```csharp
var data = new OrderData
{
  orderId = "123456",
  productId = "67890",
  amount = 2
};

var daprClient = new DaprClientBuilder().Build();

await daprClient.PublishEventAsync<OrderData>("pubsub", "newOrder", data);
```

- 最初の引数 `pubsub` は、メッセージ ブローカーの実装が提供されている Dapr コンポーネントの名前です。 コンポーネントについては、この章で後ほど説明します。
- 2 番目の引数 `neworder` では、メッセージの送信先であるトピックの名前を指定します。
- 3 番目の引数は、メッセージのペイロードです。
- メソッドのジェネリック型パラメーターを使用して、メッセージの .NET 型を指定できます。

メッセージを受信するには、登録されているトピックのサブスクリプションにエンドポイントをバインドします。 Dapr の AspNetCore ライブラリを使用すると、これを簡単に行うことができます。 たとえば、`CreateOrder` という名前の既存の ASP.NET WebAPI アクション メソッドがあるとします。

```csharp
[HttpPost("/orders")]
public async Task<ActionResult> CreateOrder(Order order)
```

 > Dapr と ASP.NET Core の統合を使用するには、プロジェクトの [Dapr.AspNetCore](https://www.nuget.org/packages/Dapr.AspNetCore) NuGet パッケージへの参照を追加する必要があります。

このアクション メソッドをトピックにバインドするには、それを `Topic` 属性で修飾します。

```csharp
[Topic("pubsub", "newOrder")]
[HttpPost("/orders")]
public async Task<ActionResult> CreateOrder(Order order)
```

この属性では、次の 2 つのキー要素を指定します。

- ターゲットとなる Dapr の pub/sub コンポーネント (この場合は `pubsub`)。
- サブスクライブするトピック (この場合は `newOrder`)。

その後、そのトピックのメッセージを受信すると、Dapr によってそのアクション メソッドが呼び出されます。

また、Dapr を使用するには ASP.NET Core を有効にする必要があります。 Dapr .NET SDK には、`Startup` クラスで呼び出すことができる拡張メソッドがいくつか用意されています。

`ConfigureServices` メソッドで、次の拡張メソッドを追加する必要があります。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddControllers().AddDapr();
}
```

`AddDapr` 拡張メソッドを `AddControllers` 拡張メソッドに追加すると、Dapr を MVC パイプラインに統合するために必要なサービスが登録されます。 また、`DaprClient` のインスタンスが依存関係挿入コンテナーに登録され、サービスの任意の場所に挿入できるようになります。

Dapr を有効にするには、`Configure` メソッドで次のミドルウェア コンポーネントを追加する必要があります。

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ...
    app.UseCloudEvents();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapSubscribeHandler();
        // ...
    });
}
```

`UseCloudEvents` を呼び出すと、**CloudEvents** ミドルウェアが ASP.NET Core のミドルウェア パイプラインに追加されます。 このミドルウェアにより、CloudEvents 構造化形式を使用する要求のラップが解除され、受信側のメソッドでイベントのペイロードを直接読み取ることができるようになります。

> [CloudEvents](https://cloudevents.io/) は標準化されたメッセージング形式であり、プラットフォーム間でイベント情報を記述する一般的な方法が提供されます。 Dapr には CloudEvents が採用されています。 CloudEvents の詳細については、[CloudEvents の仕様](https://github.com/cloudevents/spec/tree/v1.0)に関するページを参照してください。

エンドポイント ルーティング構成で `MapSubscribeHandler` を呼び出すと、Dapr のサブスクライブ エンドポイントがアプリケーションに追加されます。 このエンドポイントは、`/dapr/subscribe` での要求に応答します。 このエンドポイントが呼び出されると、`Topic` 属性で修飾されたすべての WebAPI アクション メソッドが自動的に検出され、それらのサブスクリプションを作成するように Dapr に指示されます。

## <a name="pubsub-components"></a>pub/sub コンポーネント

メッセージの実際の転送は、Dapr の [pub/sub コンポーネント](https://github.com/dapr/components-contrib/tree/master/pubsub)によって処理されます。 使用できるものが複数あります。 それぞれにおいて、pub/sub 機能を実装するために特定のメッセージ ブローカー製品がカプセル化されています。 これを書いている時点では、次の pub/sub コンポーネントを使用できました。

- Apache Kafka
- Azure Event Hubs
- Azure Service Bus
- AWS SNS/SQS
- GCP Pub/Sub
- Hazelcast
- MQTT
- NATS
- Pulsar
- RabbitMQ
- Redis Streams

> [!NOTE]
> Azure クラウド スタックには、メッセージング機能 (Azure Service Bus) とイベント ストリーミング (Azure Event Hubs) の可用性の両方があります。

これらのコンポーネントは、[GitHub の component-contrib リポジトリ](https://github.com/dapr/components-contrib/tree/master/pubsub)のコミュニティによって作成されます。 まだサポートされていないメッセージ ブローカー用には、独自の Dapr コンポーネントを作成することをお勧めします。

### <a name="configure-pubsub-components"></a>pub/sub コンポーネントを構成する

Dapr 構成ファイルを使用して、使用する pub/sub コンポーネントを指定できます。 この構成にはいくつかのフィールドが含まれています。 使用する pub/sub コンポーネントは、`name` フィールドで指定します。 メッセージを送信または受信するときは、この名前を指定する必要があります (前の `PublishEventAsync` メソッドのシグネチャで示されています)。

次に示すのは、RabbitMQ メッセージ ブローカー コンポーネントを構成するための Dapr 構成ファイルの例です。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub-rq
spec:
  type: pubsub.rabbitmq
  version: v1
  metadata:
  - name: host
    value: "amqp://localhost:5672"
  - name: durable
    value: true
```

この例を見ると、`metadata` ブロックでメッセージ ブローカー固有の構成を指定できることがわかります。 この場合、RabbitMQ は永続的なキューを作成するように構成されています。 ただし、RabbitMQ コンポーネントには他にも構成オプションがあります。 各コンポーネントの構成には、使用可能なフィールドの独自のセットがあります。 各 [pub/sub コンポーネント](https://docs.dapr.io/operations/components/setup-pubsub/supported-pubsub/)のドキュメントで、使用できるフィールドを確認できます。

コードからトピックをサブスクライブするプログラム的な方法の他に、Dapr の pub/sub には、トピックをサブスクライブする宣言型の方法もあります。 この方法を使用すると、Dapr の依存関係がアプリケーションのコードから削除されます。 したがって、コードを変更することなく、既存のアプリケーションでトピックをサブスクライブすることもできます。 次に示すのは、サブスクリプションを構成するための Dapr 構成ファイルの例です。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Subscription
metadata:
  name: newOrder-subscription
spec:
  pubsubname: pubsub
  topic: newOrder
  route: /orders
scopes:
- ServiceB
- ServiceC
```

すべてのサブスクリプションで指定する必要のある要素がいくつかあります。

- 使用する Dapr pub/sub コンポーネントの名前 (この場合は `pubsub`)。
- サブスクライブするトピックの名前 (この場合は `newOrder`)。
- このトピックに対して呼び出す必要がある API 操作 (この場合は `/orders`)。
- [スコープ](https://docs.dapr.io/developing-applications/building-blocks/pubsub/pubsub-scopes/)では、トピックを発行およびサブスクライブできるサービスを指定できます。

## <a name="reference-application-eshopondapr"></a>参照アプリケーション: eShopOnDapr

付属する [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) アプリによって、Dapr を実装するマイクロサービス アプリケーションを構築するためのエンドツーエンドの参照アーキテクチャが提供されています。 eShopOnDapr は、数年前に作成され、広く普及している [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) の進化形です。 どちらのバージョンでも、マイクロサービス間で[統合イベント](https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/#integration-events)を通信するために、pub/sub パターンが使用されています。 統合イベントには次のものが含まれます。

- ユーザーが買い物かごを精算したとき。
- 注文の支払いが成功したとき。
- 購入の猶予期間が終了したとき。

eShopOnContainers でのイベント処理は、次の `IEventBus` インターフェイスに基づいています。

```csharp
public interface IEventBus
{
    void Publish(IntegrationEvent integrationEvent);

    void Subscribe<T, THandler>()
        where TEvent : IntegrationEvent
        where THandler : IIntegrationEventHandler<T>;
}
```

このインターフェイスの具象実装は、RabbitMQ と Azure Service Bus 両方の eShopOnContainers に存在します。 各実装には、理解が困難で保守が困難なカスタム プラミング コードが数多く含まれていました。

新しい eShopOnDapr の pub/sub の動作は、Dapr を使用することによって大幅に簡素化されています。 たとえば、`IEventBus` インターフェイスは 1 つのメソッドに縮小されました。

```csharp
public interface IEventBus
{
    Task PublishAsync(IntegrationEvent integrationEvent);
}
```

### <a name="publish-events"></a>イベントの発行

更新された eShopOnDapr では、`DaprEventBus` の 1 つの実装により、Dapr でサポートされる任意のメッセージ ブローカーをサポートできます。 次のコード ブロックは、簡素化された発行メソッドを示したものです。 `PublishAsync` メソッドで Dapr クライアントを使用してイベントを発行する方法に注意してください。

```csharp
public class DaprEventBus : IEventBus
{
    private const string PubSubName = "pubsub";

    private readonly DaprClient _daprClient;
    private readonly ILogger<DaprEventBus> _logger;

    public DaprEventBus(DaprClient daprClient, ILogger<DaprEventBus> logger)
    {
        _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task PublishAsync(IntegrationEvent integrationEvent)
    {
        var topicName = integrationEvent.GetType().Name;

        // Dapr uses System.Text.Json which does not support serialization of a
        // polymorphic type hierarchy by default. Using object as the type
        // parameter causes all properties to be serialized.
        await _daprClient.PublishEventAsync<object>(PubSubName, topicName, integrationEvent);
    }
}
```

コード スニペットからわかるように、トピック名はイベントの種類の名前から派生します。 すべての eShop サービスで `IEventBus` の抽象化が使用されるため、Dapr の改良では、メインのアプリケーション コードを "*変更する必要はまったくありませんでした*"。

> [!IMPORTANT]
> メッセージをシリアル化または逆シリアル化するため、`System.Text.Json` が Dapr SDK で使用されてします。 ただし、既定の `System.Text.Json` では、派生クラスのプロパティはシリアル化されません。 eShop のコードでは、イベントが、統合イベントの基底クラスである `IntegrationEvent` として明示的に宣言されている場合があります。 これは、具象イベント型がビジネス ロジックに基づいて実行時に動的に決定されるためです。 その結果、イベントは、派生クラスではなく、基底クラスの型情報を使用してシリアル化されます。 この場合に、強制的に `System.Text.Json` で派生クラスのすべてのプロパティをシリアル化させるため、コードで `object` がジェネリック型パラメーターとして使用されています。 詳細については、[.NET のドキュメント](../../standard/serialization/system-text-json-polymorphism.md)を参照してください。

Dapr を使用すると、インフラストラクチャのコードが **大幅に簡素化** されます。 異なるメッセージ ブローカーを区別する必要はありません。 このような抽象化は Dapr によって自動的に提供されます。 また、必要に応じて、メッセージ ブローカーを簡単に入れ替えたり、複数のメッセージ ブローカー コンポーネントを構成したりすることができます。

### <a name="subscribe-to-events"></a>イベントをサブスクライブする

以前の eShopOnContainers アプリには、各メッセージ ブローカーのサブスクリプションの実装を処理するために、*SubscriptionManagers* が含まれています。 各マネージャーには、サブスクリプション イベントを処理するための、メッセージ ブローカー固有の複雑なコードが含まれています。 イベントを受け取るには、各サービスでイベントの種類ごとにハンドラーを明示的に登録する必要があります。

eShopOnDapr では、Dapr ASP.NET Core ライブラリを使用することにより、イベント サブスクリプションの仕組みが効率化されています。 各イベントは、コントローラーのアクション メソッドによって処理されます。 アクション メソッドは、`Topic` 属性により、サブスクライブ対象のトピックの名前で装飾されます。 次に示すのは、`PaymentService` から取得したコード スニペットです。

```csharp
[Route("api/v1/[controller]")]
[ApiController]
public class IntegrationEventController : ControllerBase
{
    private const string DAPR_PUBSUB_NAME = "pubsub";

    private readonly IServiceProvider _serviceProvider;

    public IntegrationEventController(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    [HttpPost("OrderStatusChangedToValidated")]
    [Topic(DAPR_PUBSUB_NAME, "OrderStatusChangedToValidatedIntegrationEvent")]
    public async Task OrderStarted(OrderStatusChangedToValidatedIntegrationEvent integrationEvent)
    {
        var handler = _serviceProvider.GetRequiredService<OrderStatusChangedToValidatedIntegrationEventHandler>();
        await handler.Handle(integrationEvent);
    }
}
```

`Topic` 属性では、イベントの .NET 型の名前がトピック名として使用されています。 イベントを処理するため、以前の eShopOnContainers コード ベースに既に存在していたイベント ハンドラーが呼び出されています。 前の例では、`OrderStatusChangedToValidatedIntegrationEvent` トピックから受信したメッセージによって、既存の `OrderStatusChangedToValidatedIntegrationEventHandler` イベント ハンドラーが呼び出されます。 サブスクリプションとメッセージ ブローカーのための基になる仕組みは Dapr に実装されているため、もともとあった大量のコードが不要になり、コード ベースから削除されました。 このコードの多くは、複雑で理解しにくく、保守が困難でした。

### <a name="use-pubsub-components"></a>pub/sub コンポーネントを使用する

eShopOnDapr リポジトリ内の `deployment` フォルダーには、さまざまな展開モード (`Docker Compose` や `Kubernetes`) を使用してアプリケーションを展開するためのファイルが含まれています。 これらの各フォルダーの下には、`components` フォルダーを保持する `dapr` フォルダーが存在します。 このフォルダーに格納されている `eshop-pubsub.yaml` ファイルには、アプリケーションで pub/sub 動作用に使用される Dapr pub/sub コンポーネントの構成が含まれています。 前のコード スニペットで見たように、使用される pub/sub コンポーネントの名前は `pubsub` です。 `deployment/compose/dapr/components` フォルダー内の `eshop-pubsub.yaml` ファイルの内容を次に示します。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub
  namespace: default
spec:
  type: pubsub.nats
  version: v1
  metadata:
  - name: natsURL
    value: nats://demo.nats.io:4222
```

上記の構成では、この例で必要な [NATS メッセージ ブローカー](https://nats.io/)が指定されています。 メッセージ ブローカーを変更するために必要なのは、RabbitMQ や Azure Service Bus などの別のメッセージ ブローカーを構成して、yaml ファイルを更新することだけです。 Dapr を使用すると、メッセージ ブローカーを切り替えるときに、メインのサービス コードを変更する必要はありません。

最後に、"1 つのアプリケーションで複数のメッセージ ブローカーが必要になるのはなぜか" という疑問があるかもしれません。 1 つのシステムで、異なる特性を持つ複数のワークロードを処理することがよくあります。 あるイベントは 1 日に 10 回発生するのに対し、別のイベントは 1 秒あたり 5,000 回発生するかもしれません。 メッセージング トラフィックを異なるメッセージ ブローカーに分割することで、メリットが得られる場合があります。 Dapr を使用すると、複数の pub/sub コンポーネントの構成を、それぞれ異なる名前で追加することができます。

## <a name="summary"></a>まとめ

pub/sub パターンは、分散アプリケーションでサービスを分離するのに役立ちます。 Dapr のパブリッシュ-サブスクライブ構成ブロックを使用すると、アプリケーションにこの動作を簡単に実装できます。

Dapr の pub/sub を使用すると、特定の "*トピック*" にメッセージを発行できます。 また、この構成ブロックにより、サービスのクエリが実行されて、サブスクライブするトピックが決定されます。

Dapr の pub/sub は、HTTP 経由でネイティブに使用することも、.NET SDK for Dapr などの言語固有の SDK のいずれかを介して使用することもできます。 .NET SDK は、ASP.NET Core プラットフォームと緊密に統合されています。

Dapr を使用すると、サポートされているメッセージ ブローカー製品をアプリケーションに組み込むことができます。 その後は、アプリケーションのコードの変更を必要とせずに、メッセージ ブローカーを入れ替えることができます。

> [!div class="step-by-step"]
> [前へ](service-invocation.md)
> [次へ](bindings.md)
