---
title: Dapr のサービス呼び出し構成ブロック
description: サービス呼び出し構成ブロック、その機能、利点、適用方法の説明
author: amolenk
ms.date: 02/17/2021
ms.openlocfilehash: f6d5f10ae476d85a9925c4fa387a16d575cacf6a
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624098"
---
# <a name="the-dapr-service-invocation-building-block"></a>Dapr のサービス呼び出し構成ブロック

分散システム内では、多くの場合、ビジネス操作を完了するために、あるサービスが別のサービスと通信する必要があります。 [Dapr のサービス呼び出し構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/)は、サービス間の通信を効率化するのに役立ちます。

## <a name="what-it-solves"></a>解決される内容

分散アプリケーションでサービス間の呼び出しを行うのは簡単なように見えますが、多くの課題が関係しています。 次に例を示します。

- 他のサービスが配置されている場所。
- サービス アドレスを指定して、サービスを安全に呼び出す方法。
- 短時間の[一時的なエラー](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling)が発生した場合の再試行の処理方法。

最後に、分散アプリケーションはさまざまなサービスで構成されるため、サービス呼び出しグラフで分析情報をキャプチャすることは、運用上の問題を診断するうえで非常に重要です。

サービス呼び出し構成ブロックでは、Dapr サイドカーをサービスの[リバース プロキシ](https://kemptechnologies.com/reverse-proxy/reverse-proxy/)として使用することにより、これらの課題が対処されます。

## <a name="how-it-works"></a>しくみ

最初に例を見てみましょう。 "サービス A" と "サービス B" という 2 つのサービスについて考えます。 サービス A は、サービス B で `catalog/items` API を呼び出す必要があります。サービス A はサービス B への依存関係を取得し、それを直接呼び出すこともできますが、サービス A は代わりに Dapr サイドカーでサービス呼び出し API を呼び出します。 図 6-1 に、この操作を示します。

![Dapr のサービス呼び出しのしくみ](./media/service-invocation/service-invocation-flow.png)

**図 6-1**。 Dapr のサービス呼び出しのしくみ。

前の図の手順に注意してください。

1. サービス A によって、サービス A サイドカーでサービス呼び出し API を呼び出すことにより、サービス B の `catalog/items` エンドポイントが呼び出されます。

    > [!NOTE]
    > サイドカーによって、プラグ可能な名前解決メカニズムを使用して、サービス B のアドレスが解決されます。セルフホステッド モードの Dapr では、[mDNS](https://www.ionos.com/digitalguide/server/know-how/multicast-dns/) を使用してそれが検索されます。 Kubernetes モードで実行されているときは、Kubernetes DNS サービスによってアドレスが決定されます。  

1. サービス A のサイドカーにより、要求がサービス B に転送されます。

1. サービス B のサイドカーにより、サービス B の API に対する実際の `catalog/items` 要求が行われます。

1. サービス B によって要求が実行され、応答がそのサイドカーに返されます。

1. サービス B のサイドカーによって、応答がサービス A のサイドカーに返送されます。

1. サービス A のサイドカーによって、応答がサービス A に返されます。

呼び出しはサイドカーを通じてフローするので、Dapr で便利な横断的動作を挿入できます。

- 失敗したら、呼び出しを自動的に再試行します。
- 証明書の自動ロールオーバーなど、相互 (mTLS) 認証でセキュリティ保護された、サービス間の呼び出しを行います。
- アクセス制御ポリシーを使用して、クライアントが実行できる操作を制御します。
- サービス間のすべての呼び出しのトレースとメトリックをキャプチャして、分析情報と診断を提供します。

Dapr に組み込まれているネイティブな **呼び出し** API を使用することで、すべてのアプリケーションで Dapr サイドカーを呼び出すことができます。 この API は、HTTP または gRPC を使用して呼び出すことができます。 次の URL を使用して HTTP API を呼び出します。

``` http
http://localhost:<dapr-port>/v1.0/invoke/<application-id>/method/<method-name>
```

- `<dapr-port>` Dapr がリッスンしている HTTP ポート。
- `<application-id>` 呼び出すサービスのアプリケーション ID。
- `<method-name>` リモート サービスで呼び出すメソッドの名前。

次の例では、`Service B` の `catalog/items` "GET" エンドポイントに対して、*curl* の呼び出しが行われます。

```console
curl http://localhost:3500/v1.0/invoke/serviceb/method/catalog/items
```

> [!NOTE]
> Dapr API を使用することで、HTTP または gRPC をサポートするすべてのアプリケーション スタックで、Dapr の構成ブロックを使用できます。 したがって、サービス呼び出し構成ブロックは、プロトコル間のブリッジとして機能できます。 サービスは、HTTP、gRPC、またはその両方の組み合わせを使用して相互に通信できます。

次のセクションでは、.NET SDK を使用してサービス呼び出しを簡略化する方法について説明します。

## <a name="use-the-dapr-net-sdk"></a>Dapr .NET SDK を使用する

Dapr [.NET SDK](https://github.com/dapr/dotnet-sdk) により、Dapr と対話するための直感的で言語固有の方法が、.NET 開発者に提供されます。 SDK からは、リモート サービス呼び出しを行うための 3 つの方法が開発者に提供されます。

1. HttpClient を使用して HTTP サービスを呼び出す
1. DaprClient を使用して HTTP サービスを呼び出す
1. DaprClient を使用して gRPC サービスを呼び出す

### <a name="invoke-http-services-using-httpclient"></a>HttpClient を使用して HTTP サービスを呼び出す

HTTP エンドポイントを呼び出す方法として推奨されるのは、Dapr と `HttpClient` のリッチな統合を使用することです。 次の例では、`orderservice` アプリケーションの `submit` メソッドを呼び出すことにより、注文が送信されます。

```csharp
var httpClient = DaprClient.CreateHttpClient();
await httpClient.PostAsJsonAsync("http://orderservice/submit", order);
```

この例では、Dapr サービスの呼び出しを実行するために使用される `HttpClient` インスタンスが、`DaprClient.CreateHttpClient` から返されます。 返された `HttpClient` では、送信要求の URI を書き換える特別な Dapr メッセージ ハンドラーが使用されます。 ホスト名は、呼び出すサービスのアプリケーション ID として解釈されます。 実際に呼び出される書き換えられた要求は次のようになります。

```http
http://127.0.0.1:3500/v1/invoke/orderservice/method/submit
```

この例では、Dapr HTTP エンドポイントの既定値である `http://127.0.0.1:<dapr-http-port>/` を使用します。 `dapr-http-port` の値は、`DAPR_HTTP_PORT` 環境変数から取得されます。 設定されていない場合は、既定のポート番号 `3500` が使用されます。

または、`DaprClient.CreateHttpClient` の呼び出しでカスタム エンドポイントを構成することもできます。

```csharp
var httpClient = DaprClient.CreateHttpClient(daprEndpoint = "localhost:4000");
```

また、アプリケーション ID を指定することで、ベース アドレスを直接設定することもできます。 これにより、呼び出しを行うときに、相対 URI を使用できるようになります。

```csharp
var httpClient = DaprClient.CreateHttpClient("orderservice");
await httpClient.PostAsJsonAsync("/submit");
```

`HttpClient` オブジェクトは、長い有効期間を意図されています。 アプリケーションの有効期間を通して、1 つの `HttpClient` インスタンスを再利用できます。 次のシナリオでは、`OrderServiceClient` クラスで Dapr の `HttpClient` インスタンスを再利用する方法を示します。  

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddSingleton<IOrderServiceClient, OrderServiceClient>(
        _ => new OrderServiceClient(DaprClient.CreateInvokeHttpClient("orderservice")));
}
```

上のスニペットで、`OrderServiceClient` は、シングルトンとして ASP.NET Core 依存関係挿入システムに登録されます。 実装ファクトリによって、`DaprClient.CreateInvokeHttpClient` を呼び出すことにより新しい `HttpClient` インスタンスが作成されます。 その後、新しく作成された `HttpClient` を使用して、`OrderServiceClient` オブジェクトがインスタンス化されます。 `OrderServiceClient` をシングルトンとして登録することにより、それはアプリケーションの有効期間を通して再利用されます。

`OrderServiceClient` 自体には、Dapr 固有のコードはありません。 Dapr のサービス呼び出しが内部で使用されていても、他の HttpClient と同じように Dapr HttpClient を扱うことができます。

```csharp
public class OrderServiceClient : IOrderServiceClient
{
    private readonly HttpClient _httpClient;

    public OrderServiceClient(HttpClient httpClient)
    {
        _httpClient = httpClient ?? throw new ArgumentNullException(nameof(httpClient));
    }

    public async Task SubmitOrder(Order order)
    {
        var response = await _httpClient.PostAsJsonAsync("submit", order);
        response.EnsureSuccessStatusCode();
    }
}
```

Dapr のサービス呼び出しで HttpClient クラスを使用すると、次のような多くの利点があります。

- HttpClient は、多くの開発者がコードで既に使用している、よく知られたクラスです。 Dapr のサービス呼び出しに HttpClient を使用すると、開発者は既存のスキルを再利用できます。
- HttpClient を使用すると、カスタム ヘッダーなどの高度なシナリオや、要求や応答のメッセージに対する完全な制御がサポートされます。
- .NET 5 の HttpClient では、System.Text.Json を使用した自動シリアル化とシリアル化解除がサポートされています。
- HttpClient は、多くの既存のフレームワークやライブラリと統合されています (たとえば、[Refit](https://github.com/reactiveui/refit)、[RestSharp](https://restsharp.dev/getting-started/getting-started.html#basic-usage)、[Polly](https://github.com/App-vNext/Polly) など)。

### <a name="invoke-http-services-using-daprclient"></a>DaprClient を使用して HTTP サービスを呼び出す

HTTP セマンティクスを使用してサービスを呼び出すには、HttpClient が推奨される方法ですが、メソッドの `DaprClient.InvokeMethodAsync` ファミリを使用することもできます。 次の例では、`orderservice` アプリケーションの `submit` メソッドを呼び出すことにより、注文が送信されます。

``` csharp
var daprClient = new DaprClientBuilder().Build();
try
{
    var confirmation =
        await daprClient.InvokeMethodAsync<Order, OrderConfirmation>(
            "orderservice", "submit", order);
}
catch (InvocationException ex)
{
    // Handle error
}
```

3 番目の引数である `order` オブジェクトは、 内部で (`System.Text.JsonSerializer` を使用して) シリアル化され、要求ペイロードとして送信されます。 サイドカーの呼び出しは、.NET SDK によって処理されます。 また、それによって、`OrderConfirmation` オブジェクトへの応答の逆シリアル化も行われます。 HTTP メソッドが指定されていないため、要求は HTTP POST として実行されます。

次の例では、`HttpMethod` を指定することにより HTTP GET 要求を行う方法を示します。

```csharp
var catalogItems = await daprClient.InvokeMethodAsync<IEnumerable<CatalogItem>>(HttpMethod.Get, "catalogservice", "items");
```

一部のシナリオでは、要求メッセージのより詳細な制御が必要になる場合があります。 たとえば、要求ヘッダーを指定する必要がある場合や、ペイロードにカスタム シリアライザーを使用する場合などです。 `DaprClient.CreateInvokeMethodRequest` によって `HttpRequestMessage` が作成されます。 次の例では、HTTP 承認ヘッダーを要求メッセージに追加する方法を示します。

```csharp
var request = daprClient.CreateInvokeMethodRequest("orderservice", "submit", order);
request.Headers.Authorization = new AuthenticationHeaderValue("bearer", token);
```

`HttpRequestMessage` には、次のプロパティが設定されています。

- Url = `http://127.0.0.1:3500/v1.0/invoke/orderservice/method/submit`
- HttpMethod = POST
- Content = JSON でシリアル化された `order` オブジェクトを含む `JsonContent` オブジェクト
- Headers.Authorization = "bearer \<token>"

意図したとおりに要求が設定されたら、`DaprClient.InvokeMethodAsync` を使用して送信します。

```csharp
var orderConfirmation = await daprClient.InvokeMethodAsync<OrderConfirmation>(request);
```

要求が成功した場合、応答は `DaprClient.InvokeMethodAsync` によって `OrderConfirmation` オブジェクトに逆シリアル化されます。 または、`DaprClient.InvokeMethodWithResponseAsync` を使用して、基になる `HttpResponseMessage` に完全にアクセスすることもできます。

```csharp
var response = await daprClient.InvokeMethodWithResponseAsync(request);
response.EnsureSuccessStatusCode();

var orderConfirmation = response.Content.ReadFromJsonAsync<OrderConfirmation>();
```

> [!NOTE]
> HTTP を使用するサービス呼び出しでは、前のセクションで説明した Dapr HttpClient 統合を使用することを検討してください。 HttpClient を使用すると、既存のフレームワークやライブラリとの統合など、より多くの利点があります。

### <a name="invoke-grpc-services-using-daprclient"></a>DaprClient を使用して gRPC サービスを呼び出す

DaprClient には、gRPC エンドポイントを呼び出すための `InvokeMethodGrpcAsync` メソッドのファミリが用意されています。 HTTP メソッドとの主な違いは、JSON ではなく Protobuf シリアライザーが使用されることです。 次の例では、gRPC を使用して `orderservice` の `submitOrder` メソッドを呼び出しています。

```csharp
var daprClient = new DaprClientBuilder().Build();
try
{
    var confirmation = await daprClient.InvokeMethodGrpcAsync<Order, OrderConfirmation>("orderservice", "submitOrder", order);
}
catch (InvocationException ex)
{
    // Handle error
}
```

上の例では、指定された `order` オブジェクトは DaprClient により [Protobuf](https://developers.google.com/protocol-buffers) を使用してシリアル化され、その結果が gRPC 要求本文として使用されます。 同様に、応答本文は Protobuf で逆シリアル化されて、呼び出し元に返されます。 通常、Protobuf の方が、HTTP サービスの呼び出しで使用される JSON ペイロードより、優れたパフォーマンスを提供します。

## <a name="reference-application-eshopondapr"></a>参照アプリケーション: eShopOnDapr

Microsoft の元の [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) マイクロサービス参照アーキテクチャでは、HTTP/REST と gRPC サービスの組み合わせが使用されていました。 gRPC の使用は、[アグリゲーター サービス](../cloud-native/service-to-service-communication.md#service-aggregator-pattern)とコア バックエンド サービスの間の通信に限定されていました。 図 6-2 は、そのアーキテクチャを示したものです。

![eShopOnContainers での gRPC と HTTP/REST の呼び出し](./media/service-invocation/eshop-on-containers.png)

**図 6-2**。 eShopOnContainers での gRPC と HTTP/REST の呼び出し。

前の図の手順に注意してください。

1. フロントエンドにより、HTTP/REST を使用して [API ゲートウェイ](/azure/architecture/microservices/design/gateway)が呼び出されます。

1. 単純な [CRUD](https://www.sumologic.com/glossary/crud/) (作成、読み取り、更新、削除) 要求は、API ゲートウェイにより、HTTP/REST を使用して、直接コア バックエンド サービスに転送されます。

1. 複数のバックエンド サービスへの調整された呼び出しが含まれる複雑な要求は、API ゲートウェイにより、Web ショッピング アグリゲーター サービスに転送されます。

1. アグリゲーター サービスにより、gRPC を使用して、コア バックエンド サービスが呼び出されます。

最近更新された eShopOnDapr の実装では、Dapr サイドカーがサービスと API ゲートウェイに追加されています。 図 6-3 は、更新されたアーキテクチャを示したものです。

![eShopOnContainers のサイドカーでの gRPC と HTTP/REST の呼び出し](./media/service-invocation/eshop-on-dapr.png)

**図 6-3**。 Dapr を使用する更新された eShop アーキテクチャ。

前の図の更新された手順に注意してください。

1. フロントエンドにより、引き続き HTTP/REST を使用して API ゲートウェイが呼び出されます。

1. API ゲートウェイにより、HTTP 要求が Dapr サイドカーに転送されます。

1. API ゲートウェイのサイドカーにより、アグリゲーターまたはバックエンド サービスのサイドカーに要求が送信されます。

1. アグリゲーター サービスにより、Dapr .NET SDK を使用して、サイドカー アーキテクチャを通じてバックエンド サービスが呼び出されます。

サイドカーと gRPC の間の呼び出しは、Dapr によって実装されます。 したがって、HTTP/REST のセマンティクスを使用してリモート サービスを呼び出している場合でも、トランスポートの部分はまだ gRPC を使用して実装されています。

eShopOnDapr 参照アプリケーションでは、Dapr のサービス呼び出し構成ブロックが利用されています。 そのメリットには、サービスの検出、自動 mTLS、監視などがあります。

### <a name="forward-http-requests-using-envoy-and-dapr"></a>Envoy と Dapr を使用して HTTP 要求を転送する

元と更新後の両方の eShop アプリケーションで、API ゲートウェイとして [Envoy プロキシ](https://www.envoyproxy.io/)が利用されています。 Envoy は、オープンソースのプロキシであり、最新の分散アプリケーションで広く使われている通信バスです。 Lyft が基になっている Envoy は、[Cloud-Native Computing Foundation](https://www.cncf.io/) によって所有および管理されています。

元の eShopOnContainers の実装では、受信 HTTP 要求は、Envoy API ゲートウェイにより、アグリゲーターまたはバックエンド サービスに直接転送されていました。 新しい eShopOnDapr では、要求は Envoy プロキシによって Dapr サイドカーに転送されます。 サイドカーにより、サービスの呼び出し、mTLS、監視が提供されます。

Envoy は、プロキシの動作を制御するための YAML 定義ファイルを使用して構成されます。 Envoy で HTTP 要求を Dapr サイドカー コンテナーに転送できるようにするには、構成に `dapr` クラスターを追加します。 クラスターの構成には、Dapr サイドカーによってリッスンされている HTTP ポートを指すホストが含まれます。

``` yaml
clusters:
- name: dapr
  connect_timeout: 0.25s
  type: strict_dns
  hosts:
  - socket_address:
    address: 127.0.0.1
    port_value: 3500
```

Envoy のルート構成は、受信要求を Dapr サイドカーの呼び出しとして書き換えるように、更新されています (`prefix_rewrite` のキーと値のペアによく注意してください)。

``` yaml
- name: "c-short"
  match:
    prefix: "/c/"
  route:
    auto_host_rewrite: true
    prefix_rewrite: "/v1.0/invoke/catalog-api/method/"
    cluster: dapr
```

フロントエンド クライアントでカタログ商品のリストを取得するシナリオについて考えます。 Catalog API により、カタログ商品を取得するためのエンドポイントが提供されています。

``` csharp
[Route("api/v1/[controller]")]
[ApiController]
public class CatalogController : ControllerBase
{
    [HttpGet("items")]
    public async Task<IActionResult> ItemsAsync(
        [FromQuery] int pageSize = 10,
        [FromQuery] int pageIndex = 0)
    {
        // ...
    }
```

最初に、フロントエンドにより、Envoy API ゲートウェイへの HTTP の直接呼び出しが行われます。

```
GET http://<api-gateway>/c/api/v1/catalog/items?pageSize=20
```

Envoy プロキシにより、ルートが照合され、HTTP 要求が書き換えられて、Dapr サイドカーの `invoke` API に転送されます。

```
GET http://127.0.0.1:3500/v1.0/invoke/catalog-api/method/api/v1/catalog/items?pageSize=20
```

サイドカーにより、サービスの検出が処理され、Catalog API のサイドカーに要求がルーティングされます。 最後に、サイドカーにより Catalog API が呼び出されて、要求が実行され、カタログ商品がフェッチされて、応答が返されます。

```
GET http://localhost/api/v1/catalog/items?pageSize=20
```

### <a name="make-aggregated-service-calls-using-the-net-sdk"></a>.NET SDK を使用して集約されたサービスの呼び出しを行う

eShop フロントエンドからのほとんどの呼び出しは、単純な CRUD 呼び出しです。 それらは、API ゲートウェイによって、処理のために 1 つのサービスに転送されます。 ただし、一部のシナリオでは、要求を完了するために複数のバックエンド サービスが連携する必要があります。 このようなより複雑な呼び出しでは、eShop により、Web ショッピング アグリゲーター サービスを使用して、複数のサービス間のワークフローが仲介されます。 図 6-4 は、買い物かごに商品を追加する処理シーケンスを示しています。

![買い物かご更新シーケンスの図](./media/service-invocation/complex-call.png)

**図 6-4**。 買い物かご更新シーケンス。

最初に、アグリゲーター サービスによって、Catalog API からカタログ商品が取得されます。 次に、商品があるかどうかと、価格が検証されます。 最後に、アグリゲーター サービスにより、Basket API が呼び出されて、更新された買い物かごが保存されます。

アグリゲーター サービスには、買い物かごを更新するためのエンドポイントを提供する `BasketController` が含まれています。

``` csharp
[Route("api/v1/[controller]")]
[Authorize]
[ApiController]
public class BasketController : ControllerBase
{
    private readonly ICatalogService _catalog;
    private readonly IBasketService _basket;

    [HttpPost]
    [HttpPut]
    public async Task<ActionResult<BasketData>> UpdateAllBasketAsync(
        [FromBody] UpdateBasketRequest data, [FromHeader] string authorization)
    {
        // Get the item details from the catalog API.
        var catalogItems = await _catalog.GetCatalogItemsAsync(
            data.Items.Select(x => x.ProductId));

        // Check item availability and prices; store results in basket object.
        var basket = CreateValidatedBasket(data, catalogItems);

        // Save the shopping basket.
        await _basket.UpdateAsync(basket, authorization);

        return basket;
    }

    // ...
}
```

`UpdateAllBasketAsync` メソッドにより、`FromHeader` 属性を使用して、受信要求の *Authorization* ヘッダーが取得されます。 *Authorization* ヘッダーには、保護されたバックエンド サービスを呼び出すために必要なアクセス トークンが含まれています。

買い物かご更新要求を受信した後、アグリゲーター サービスにより Catalog API が呼び出されて、商品の詳細が取得されます。 バスケット コントローラーでは、挿入された `ICatalogService` オブジェクトを使用してその呼び出しが行われ、Catalog API と通信されます。 インターフェイスの元の実装では、gRPC を使用して呼び出しが行われていました。 更新された実装では、HttpClient サポートを備える Dapr のサービス呼び出しが使用されます。

``` csharp
public class CatalogService : ICatalogService
{
    private readonly HttpClient _httpClient;

    public CatalogService(HttpClient httpClient)
    {
        _httpClient = httpClient ?? throw new ArgumentNullException(nameof(httpClient));
    }

    public Task<IEnumerable<CatalogItem>> GetCatalogItemsAsync(IEnumerable<int> ids)
    {
        var requestUri = $"api/v1/catalog/items?ids={string.Join(",", ids)}";

        return _httpClient.GetFromJsonAsync<IEnumerable<CatalogItem>>(requestUri);
    }

    // ...
}
```

サービス呼び出しを行うために、Dapr 固有のコードが必要ないことに注目してください。 すべての通信は、標準の HttpClient オブジェクトを使用して行われます。

Dapr HttpClient は、`Startup.ConfigureServices` メソッドで `CatalogService` クラスに挿入されます。

```csharp
services.AddSingleton<ICatalogService, CatalogService>(
    _ => new CatalogService(DaprClient.CreateInvokeHttpClient("catalog-api")));
```

アグリゲーター サービスによって行われるもう 1 つの呼び出しは、Basket API に対するものです。 それは、承認された要求を許可するだけです。 呼び出しが成功するように、アクセス トークンが *Authorization* 要求ヘッダーと共に渡されます。

``` csharp
public class BasketService : IBasketService
{
    public Task UpdateAsync(BasketData currentBasket, string accessToken)
    {
        var request = new HttpRequestMessage(HttpMethod.Post, "api/v1/basket")
        {
            Content = JsonContent.Create(currentBasket)
        };
        request.Headers.Authorization = new AuthenticationHeaderValue(accessToken);

        var response = await _httpClient.SendAsync(request);
        response.EnsureSuccessStatusCode();
    }

    // ...
}
```

この例でも、サービスを呼び出すために使用されるのは、標準の HttpClient 機能だけです。 これにより、既に HttpClient を使い慣れている開発者は、既存のスキルを再利用できます。 また、何も変更することなく、既存の HttpClient コードで、Dapr サービスの呼び出しを使用できます。

## <a name="summary"></a>まとめ

この章では、サービス呼び出し構成ブロックについて学習しました。 リモート メソッドを呼び出す方法として、Dapr サイドカーを HTTP で直接呼び出す方法と、Dapr .NET SDK を使用する方法を見ました。

Dapr .NET SDK には、リモート メソッドを呼び出すための複数の方法が用意されています。 HttpClient のサポートは、既存のスキルを再利用したい開発者にとって優れており、既存の多くのフレームワークやライブラリと互換性があります。 DaprClient からは、HTTP または gRPC セマンティクスを使用して Dapr サービス呼び出し API を直接使用するためのサポートが提供されています。

eShopOnDapr 参照アーキテクチャでは、元の eShopOnContainers ソリューションを Dapr のサービス呼び出しを使用して最新化する方法が示されています。 eShop に Dapr を追加すると、自動再試行、mTLS を使用したメッセージの暗号化、改善された監視などの利点が得られます。

### <a name="references"></a>リファレンス

- [Dapr のサービス呼び出し構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/)

- [分散型クラウドネイティブ アプリケーションの監視](../cloud-native/observability-patterns.md)

> [!div class="step-by-step"]
> [前へ](state-management.md)
> [次へ](publish-subscribe.md)
