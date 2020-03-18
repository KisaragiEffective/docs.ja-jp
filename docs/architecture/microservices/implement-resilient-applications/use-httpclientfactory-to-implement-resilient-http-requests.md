---
title: HttpClientFactory を使用して回復力の高い HTTP 要求を実装する
description: .NET Core 2.1 以降で使用できる HttpClientFactory を使用して、`HttpClient` インスタンスを作成し、それをアプリケーションで簡単に使用できるようにする方法について説明します。
ms.date: 08/08/2019
ms.openlocfilehash: 1a6d65509d669166e73ad907b506bae7fa26536d
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900322"
---
# <a name="use-httpclientfactory-to-implement-resilient-http-requests"></a>HttpClientFactory を使用して回復力の高い HTTP 要求を実装する

`HttpClientFactory` は、自己主張性の強いファクトリで、アプリケーションに使用する <xref:System.Net.Http.HttpClient> インスタンスを作成するため、.NET Core 2.1 以降で使用できます。

## <a name="issues-with-the-original-httpclient-class-available-in-net-core"></a>.NET Core で使用可能な元の HttpClient クラスに関する問題

よく知られている元の <xref:System.Net.Http.HttpClient> クラスは、簡単に使用できますが、場合によっては、多くの開発者が適切に使用していません。

1 つ目の問題は、このクラスは破棄可能ですが、`HttpClient` オブジェクトを破棄しても、基になるソケットがすぐに解放されず、'ソケットの枯渇' という重大な問題が発生する場合があるため、`using` ステートメントで使用するのは最適な選択ではないということです。 この問題の詳細については、ブログ記事「[You're using HttpClient wrong and it is destabilizing your software (HttpClient の誤った使い方がソフトウェアを不安定にする)](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)」を参照してください。

そのため、`HttpClient` は一度インスタンス化されたら、アプリケーションの有効期間にわたって再利用されることを目的としています。 すべての要求に対して `HttpClient` クラスをインスタンス化すると、高負荷の下で使用可能なソケットの数が枯渇してしまいます。 この問題により、`SocketException` エラーが発生します。 この問題を解決するために可能なアプローチは、HttpClient クライアントの使用に関するこの [Microsoft の記事](../../../csharp/tutorials/console-webapiclient.md)で説明されているように、`HttpClient` オブジェクトをシングルトンまたは静的として作成することに基づいています。

しかし、`HttpClient` には、シングルトンまたは静的オブジェクトとして使用した場合に発生する可能性がある 2 つ目の問題があります。 この場合、dotnet/corefx GitHub リポジトリでこの[問題](https://github.com/dotnet/corefx/issues/11224)について説明されているように、シングルトンまたは静的 `HttpClient` では、DNS の変更が尊重されません。

これらの問題に対処し、`HttpClient` インスタンスの管理を容易にするため、.NET Core 2.1 では、新しい `HttpClientFactory` が導入されています。これは、Polly を統合することで、回復力のある HTTP 呼び出しを実装するためにも使用できます。

[Polly](http://www.thepollyproject.org/) は、事前に定義されたポリシーを緩やかでスレッドセーフな方法で使用することにより、開発者がアプリケーションに回復性を追加できるようにするための一時的な障害処理ライブラリです。

## <a name="what-is-httpclientfactory"></a>HttpClientFactory とは

`HttpClientFactory` は次の目的のために設計されています。

- 論理 `HttpClient` オブジェクトの名前付けと構成を行うために、中央の場所を提供します。 たとえば、特定のマイクロサービスにアクセスするために事前に構成されているクライアント (サービス エージェント) を構成できます。
- `HttpClient` でのハンドラーのデリゲートにより送信ミドルウェアの概念を体系化し、Polly ベースのミドルウェアを実装して、回復性のための Polly のポリシーを利用します。
- `HttpClient` は既に、送信 HTTP 要求用にリンクできるハンドラーのデリゲートの概念を備えています。 ファクトリに HTTP クライアントを登録できるほか、Polly ハンドラーを使用して、再試行、サーキット ブレーカーなどに Polly ポリシーを使用することができます。
- `HttpClientMessageHandlers` の有効期間を管理して、`HttpClient` の有効期間を自分で管理する際に発生する可能性がある上記の問題を回避します。

> [!NOTE]
> `HttpClientFactory` は、`Microsoft.Extensions.DependencyInjection` NuGet パッケージ内の依存関係挿入 (DI) の実装に緊密に関連付けられています。 その他の依存関係挿入コンテナーの使用に関する詳細については、この [GitHub のディスカッション](https://github.com/dotnet/extensions/issues/1345)を参照してください。

## <a name="multiple-ways-to-use-httpclientfactory"></a>HttpClientFactory を使用する複数の方法

アプリケーションで `HttpClientFactory` を使用するには、複数の方法があります。

- `HttpClientFactory` を直接使用する
- 名前付きクライアントを使用する
- 型指定されたクライアントを使用する
- 生成されたクライアントを使用する

簡潔にするために、このガイダンスでは、型指定されたクライアント (サービス エージェント パターン) を使用する、`HttpClientFactory` を使用するための最も構造化された方法を示します。 しかし、すべてのオプションが記載されており、現在、[HttpClientFactory の使用方法が記載されたこの記事](/aspnet/core/fundamentals/http-requests#consumption-patterns)で一覧表示されています。

## <a name="how-to-use-typed-clients-with-httpclientfactory"></a>HttpClientFactory で型指定されたクライアントを使用する方法

それでは、"型指定されたクライアント" とは何でしょうか。 これは、挿入時に `DefaultHttpClientFactory` によって構成される単なる `HttpClient` です。

次の図は、`HttpClientFactory` で型指定されたクライアントを使用する方法を示しています。

![HttpClientFactory で型指定されたクライアントを使用する方法を示している図。](./media/use-httpclientfactory-to-implement-resilient-http-requests/client-application-code.png)

**図 8-4** 型指定されたクライアント クラスで HttpClientFactory を使用する。

上の図では、(コントローラーまたはクライアント コードによって使用される) ClientService によって、登録済みの `IHttpClientFactory` によって作成された `HttpClient` が使用されます。 このファクトリでは、管理するプールからの `HttpMessageHandler` を `HttpClient` に割り当てます。 拡張メソッド `AddHttpClient` を使用して `IHttpClientFactory` を DI コンテナーに登録するときに、Polly のポリシーを使用して `HttpClient` を構成できます。

上の構造を構成するには、`IServiceCollection` に対する `AddHttpClient()` 拡張メソッドを含む `Microsoft.Extensions.Http` NuGet パッケージをインストールすることで、アプリケーションに `HttpClientFactory` を追加します。 この拡張メソッドは、インターフェイス `IHttpClientFactory` のシングルトンとして使用される `DefaultHttpClientFactory` を登録します。 これは `HttpMessageHandlerBuilder` の一時的な構成を定義します。 プールから取得されたこのメッセージ ハンドラー (`HttpMessageHandler` オブジェクト) は、ファクトリから返された `HttpClient` によって使用されます。

次のコードで、`AddHttpClient()` を使用して、`HttpClient` を使用する必要がある型指定されたクライアント (エージェント サービス) を登録する方法を確認できます。

```csharp
// Startup.cs
//Add http client services at ConfigureServices(IServiceCollection services)
services.AddHttpClient<ICatalogService, CatalogService>();
services.AddHttpClient<IBasketService, BasketService>();
services.AddHttpClient<IOrderingService, OrderingService>();
```

前のコードに示されているように、クライアント サービスを登録すると、`DefaultClientFactory` によって、サービスごとに標準の `HttpClient` が作成されるようになります。

また、次のコードに示すように、登録にインスタンス固有の構成を追加し、ベース アドレスの構成や回復性ポリシーの追加などを行うこともできます。

```csharp
services.AddHttpClient<ICatalogService, CatalogService>(client =>
{
    client.BaseAddress = new Uri(Configuration["BaseUrl"]);
})
    .AddPolicyHandler(GetRetryPolicy())
    .AddPolicyHandler(GetCircuitBreakerPolicy());
```

例として、次のコードで上記のポリシーの 1 つを確認できます。

```csharp
static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
        .WaitAndRetryAsync(6, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));
}
```

[次の記事](implement-http-call-retries-exponential-backoff-polly.md)では、Polly の使用に関する詳細を確認できます。

### <a name="httpclient-lifetimes"></a>HttpClient の有効期間

`IHttpClientFactory` から `HttpClient` オブジェクトを取得するたび、新しいインスタンスが返されます。 ただし、各 `HttpClient` では、`HttpMessageHandler` の有効期間が切れていない限り、リソースの消費量を減らすために `IHttpClientFactory` によってプールおよび再利用されている `HttpMessageHandler` を使用します。

通常各ハンドラーは基になる HTTP 接続を独自に管理しており、必要以上に多くのハンドラーを作成すると接続が遅延する可能性があるため、ハンドラーをプールするのは望ましい方法です。 また、一部のハンドラーは接続を無期限に開いており、DNS の変更にハンドラーが対応できないことがあります。

プール内の `HttpMessageHandler` オブジェクトには、有効期間があります。この有効期間は、プール内の `HttpMessageHandler` インスタンスを再利用できる期間です。 既定値は 2 分ですが、型指定されたクライアントごとにオーバーライドできます。 オーバーライドするには、次のコードに示すように、クライアント作成時に返された `IHttpClientBuilder` で `SetHandlerLifetime()` を呼び出します。

```csharp
//Set 5 min as the lifetime for the HttpMessageHandler objects in the pool used for the Catalog Typed Client
services.AddHttpClient<ICatalogService, CatalogService>()
    .SetHandlerLifetime(TimeSpan.FromMinutes(5));
```

型指定された各クライアントには、独自の構成済みハンドラーの有効期間の値を設定できます。 ハンドラーの有効期限を無効にするには、有効期間を `InfiniteTimeSpan` に設定します。

### <a name="implement-your-typed-client-classes-that-use-the-injected-and-configured-httpclient"></a>挿入された構成済みの HttpClient を使用する、型指定されたクライアント クラスを実装する

前の手順では、型指定されたクライアント クラス (サンプル コードにある、'BasketService'、'CatalogService'、'OrderingService' のようなクラスなど) が定義されている必要がありました。型指定されたクライアントは、(そのコンストラクターを通じて挿入された) `HttpClient` オブジェクトを受け取り、それを使用していくつかのリモート HTTP サービスを呼び出すクラスです。 次に例を示します。

```csharp
public class CatalogService : ICatalogService
{
    private readonly HttpClient _httpClient;
    private readonly string _remoteServiceBaseUrl;

    public CatalogService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<Catalog> GetCatalogItems(int page, int take,
                                               int? brand, int? type)
    {
        var uri = API.Catalog.GetAllCatalogItems(_remoteServiceBaseUrl,
                                                 page, take, brand, type);

        var responseString = await _httpClient.GetStringAsync(uri);

        var catalog = JsonConvert.DeserializeObject<Catalog>(responseString);
        return catalog;
    }
}
```

型指定されたクライアント (この例では CatalogService) は、依存関係の挿入 (DI) によってアクティブ化されます。つまり HttpClient だけでなく、そのコンストラクター内に登録されている任意のサービスを受け取ることができます。

型指定されたクライアントは、実際には、一時的なオブジェクトです。つまり、新しいインスタンスは必要になるたびに作成され、新しい `HttpClient` インスタンスが構築されるたびにそれを受け取ります。 ただし、プール内の HttpMessageHandler オブジェクトは、複数の Http 要求で再利用されるオブジェクトです。

### <a name="use-your-typed-client-classes"></a>型指定されたクライアント クラスを使用する

最後に、型指定されたクラスを実装し、`AddHttpClient()` に登録したら、DI によってサービスを挿入できる場所であればどこでもそれらを使用できます。 たとえば、次の eShopOnContainers のコードのように、Razor ページ コード、または MVC Web アプリのコントローラーなどがあります。

```csharp
namespace Microsoft.eShopOnContainers.WebMVC.Controllers
{
    public class CatalogController : Controller
    {
        private ICatalogService _catalogSvc;

        public CatalogController(ICatalogService catalogSvc) =>
                                                           _catalogSvc = catalogSvc;

        public async Task<IActionResult> Index(int? BrandFilterApplied,
                                               int? TypesFilterApplied,
                                               int? page,
                                               [FromQuery]string errorMsg)
        {
            var itemsPage = 10;
            var catalog = await _catalogSvc.GetCatalogItems(page ?? 0,
                                                            itemsPage,
                                                            BrandFilterApplied,
                                                            TypesFilterApplied);
            //… Additional code
        }

        }
}
```

この時点までは、示されているコードは、通常の Http 要求を実行するだけですが、次のセクションでは、不思議なことが起こります。ポリシーを追加して、ハンドラーを登録済みの型指定されているクライアントにデリゲートするだけで、`HttpClient` によって実行されるすべての HTTP 要求が、指数バックオフを含む再試行、サーキット ブレーカー、またはその他の任意のカスタム デリゲート ハンドラーなど、回復力のあるポリシーを考慮して、追加のセキュリティ機能 (認証トークンの使用など) やその他のカスタム機能を実装するようになります。

## <a name="additional-resources"></a>その他の技術情報

- **.NET Core で HttpClientFactory を使用する**  
  [https://docs.microsoft.com/aspnet/core/fundamentals/http-requests](/aspnet/core/fundamentals/http-requests)

- **`dotnet/extensions` GitHub リポジトリ内の HttpClientFactory ソース コード**  
  <https://github.com/dotnet/extensions/tree/master/src/HttpClientFactory>

- **Polly (.NET の復元および一時的な障害処理ライブラリ)**  
  <http://www.thepollyproject.org/>
  
- **依存関係挿入なしで HttpClientFactory を使用する (GitHub の問題)**  
  <https://github.com/dotnet/extensions/issues/1345>

>[!div class="step-by-step"]
>[前へ](explore-custom-http-call-retries-exponential-backoff.md)
>[次へ](implement-http-call-retries-exponential-backoff-polly.md)
