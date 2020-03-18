---
title: サーキット ブレーカー パターンの実装
description: サーキット ブレーカー パターンを HTTP 再試行の補助システムとして実装する方法について説明します。
ms.date: 10/16/2018
ms.openlocfilehash: 00ca39b4b6fac37ff60adf128c3f4e22c5fc14e2
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73732825"
---
# <a name="implement-the-circuit-breaker-pattern"></a>サーキット ブレーカー パターンを実装する

前述のように、リモート サービスやリソースに接続を試行する際に発生し得る、復旧にどのくらい時間がかかるかわからない障害を処理する必要があります。 この種類の障害を処理することにより、アプリケーションの安定性と回復性が向上します。

分散環境において、リモート リソースとサービスの呼び出しは、低速なネットワーク接続やタイムアウト、あるいはリソースの応答が低速であったり、一時的に利用不可能であったりなどの、一時的な障害が原因で失敗することがあります。 このような障害は通常しばらくすると自動的に修正されます。堅牢なクラウド アプリケーションなら "再試行パターン" などの方法でそうした障害を処理する備えができているはずです。

ただし、予期しないイベントが原因で、障害の修正に非常に長い時間がかかる状況もあり得ます。 このような障害の重大度は、部分的な接続の損失からサービスの完全な不具合まで多岐にわたります。 このような状況では、アプリケーションが成功の見込みの薄い操作を再試行し続けることは無駄かもしれません。

代わりに、アプリケーションが操作の失敗を受け入れ、失敗を処理するようにコードを書く必要があります。

HTTP 再試行を不注意に使用すると、自分のソフトウェアにサービス拒否 ([DoS](https://en.wikipedia.org/wiki/Denial-of-service_attack)) 攻撃が発生することがあります。 マイクロサービスが失敗したか、動作が遅いとき、複数のクライアントで失敗した要求の再試行が繰り返されることがあります。 その結果、失敗を繰り返すサービスに宛てられるトラフィックが急激に増えるという危険なリスクが発生します。

そのため、試行を続けるに値しないとき、過剰な要求によって停止するように、何らかの防壁が必要になります。 その防壁こそがサーキット ブレーカーです。

サーキット ブレーカー パターンの目的は、"再試行パターン" とは異なります。 "再試行パターン" では、操作が最終的には成功するとの見込みをもってアプリケーションに操作を再試行させます。 サーキット ブレーカー パターンにより、失敗する可能性の高い操作がアプリケーションで実行されなくなります。 アプリケーションでは、これら 2 つのパターンを組み合わせることができます。 しかしながら、再試行のロジックはサーキット ブレーカーが返す例外に対応できる必要があり、障害が一時的ではないことをサーキット ブレーカーが示す場合、再試行を中止する必要があります。

## <a name="implement-circuit-breaker-pattern-with-httpclientfactory-and-polly"></a>HttpClientFactory と Polly でサーキット ブレーカー パターンを実装する

再試行の実装と同じように、サーキット ブレーカーの推奨されるアプローチは、Polly など実績のある .NET ライブラリを活用し、HttpClientFactory とネイティブ統合することです。

サーキット ブレーカー ポリシーを HttpClientFactory の外向きミドルウェア パイプラインに追加することは、HttpClientFactory の使用時、既にあるものにコードの増分を 1 つ分追加することと同じくらい簡単です。

HTTP 呼び出しの再試行に使用されるコードに追加する必要があるのは、ConfigureServices() メソッドから抜粋された次の増分コードで示されているように、使用するポリシーのリストにサーキット ブレーカー ポリシーを追加するコードだけです。

```csharp
//ConfigureServices()  - Startup.cs
services.AddHttpClient<IBasketService, BasketService>()
        .SetHandlerLifetime(TimeSpan.FromMinutes(5))  //Sample. Default lifetime is 2 minutes
        .AddHttpMessageHandler<HttpClientAuthorizationDelegatingHandler>()
        .AddPolicyHandler(GetRetryPolicy())
        .AddPolicyHandler(GetCircuitBreakerPolicy());
```

`AddPolicyHandler()` メソッドは、使用される `HttpClient` オブジェクトにポリシーを追加するものです。 この場合、サーキット ブレーカーの Polly のポリシーを追加しています。

手法のモジュール性を高める目的で、次のコードに示すように、`GetCircuitBreakerPolicy()` という名前の別個のメソッド内でサーキット ブレーカー ポリシーが定義されています。

```csharp
static IAsyncPolicy<HttpResponseMessage> GetCircuitBreakerPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .CircuitBreakerAsync(5, TimeSpan.FromSeconds(30));
}
```

上記のコード例では、HTTP 要求の再試行時、エラーが連続して 5 つ発生したとき、サーキットを中断する (またはオープン状態にする) ようにサーキット ブレーカー ポリシーが構成されています。 それが発生すると、サーキットは 30 秒間中断されます。その間、呼び出しは実際に配置されるのではなく、サーキット ブレーカーが働いてすぐに失敗します。  [関連する例外および HTTP 状態コード](/aspnet/core/fundamentals/http-requests#handle-transient-faults) は、ポリシーによって自動的にエラーとして解釈されます。  

サーキット ブレーカーは、HTTP 呼び出しを実行しているクライアント アプリケーションまたはサービスとは別の環境にデプロイされている特定のリソースに問題がある場合に、要求をフォールバック インフラストラクチャにリダイレクトするためにも使用する必要があります。 こうすれば、バックエンドのマイクロサービスのみに影響を与え、クライアント アプリケーションには影響を与えないデータセンターの停止が生じた場合に、クライアント アプリケーションはフォールバック サービスにリダイレクトできます。 Polly では、この[フェールオーバー ポリシー](https://github.com/App-vNext/Polly/wiki/Polly-Roadmap#failover-policy) シナリオを自動化するための新しいポリシーが計画されています。

これらすべての機能は、位置を意識することなく Azure に自動的にフェールオーバーを管理させるのとは対照的に、フェールオーバーを .NET コード内から管理する場合のためのものです。

使用という観点からは、HttpClient を使用するとき、ここで新しいものは何も追加する必要がありません。前のセクションで示したように、HttpClient と HttpClientFactory の使用時とコードが同じであるからです。

## <a name="test-http-retries-and-circuit-breakers-in-eshoponcontainers"></a>eShopOnContainers 内で HTTP の再試行とサーキット ブレーカーをテストする

Docker ホスト内で eShopOnContainers ソリューションを起動する場合、このソリューションは複数のコンテナーを起動する必要があります。 SQL Server コンテナーなど、一部のコンテナーは起動と初期化が低速です。 eShopOnContainers アプリケーションを Docker に初めてデプロイする場合は、イメージとデータベースを設定する必要があるため、特にそのようになります。 一部のコンテナーは他のコンテナーより起動が低速だという事実により、以前のセクションで説明したように docker-compose レベルでコンテナー間の依存関係を設定している場合であっても、残りのサービスによる初期化時の HTTP 例外のスローが引き起こされる可能性があります。 コンテナー間の docker-compose 依存関係は、プロセス レベルだけのものです。 コンテナーのエントリ ポイント プロセスは起動されても、SQL Server はクエリに対して準備ができていないことがあります。 結果としてエラーの連鎖が生じ、アプリケーションがその特定のコンテナーを利用しようとするときに例外が発生することがあります。

また、起動時のこのタイプのエラーは、アプリケーションをクラウドに展開する際に生じることもあります。 その場合、オーケストレーターは、クラスターのノード全体でコンテナーの数を分散させる際に、コンテナーを 1 つのノードまたは VM から別の場所に移動することがあります (つまり、新しいインスタンスを起動する)。

すべてのコンテナーを起動するとき、'eShopOnContainers' では、上記の再試行パターンを利用してこれらの問題を解決します。

### <a name="test-the-circuit-breaker-in-eshoponcontainers"></a>eShopOnContainers 内でサーキット ブレーカーをテストする

いくつかの方法でサーキットを中断し/オープン状態にし、eShopOnContainers でテストできます。

1 つの方法は、サーキット ブレーカー ポリシーで許可する再試行の数を 1 まで減らし、ソリューション全体を Docker に再展開することです。 再試行を 1 回にすることで、デプロイ中に HTTP 要求が失敗し、サーキット ブレーカーがオープン状態になって、エラーが発生する可能性が高くなります。

もう 1 つの方法は、**Basket** マイクロサービス内に実装されているカスタムのミドルウェアを使用することです。 このミドルウェアが有効な場合、すべての HTTP 要求を捕捉し、状態コード 500 を返します。 次のように、"failing" URI に GET 要求を実行してミドルウェアを有効にできます。

- `GET http://localhost:5103/failing`\
  この要求は、ミドルウェアの現在の状態を返します。 ミドルウェアが有効な場合、要求は状態コード 500 を返します。 ミドルウェアが無効の場合、応答はありません。

- `GET http://localhost:5103/failing?enable`\
  この要求はミドルウェアを有効にします。

- `GET http://localhost:5103/failing?disable`\
  この要求はミドルウェアを無効にします。

たとえば、アプリケーションが実行されたら、任意のブラウザーで次の URI を使用して要求を実行することにより、ミドルウェアを有効にできます。 注文マイクロサービスにはポート 5103 を使用することにご注意ください。

`http://localhost:5103/failing?enable`

その後、図 8-5 に示されているように、URI `http://localhost:5103/failing` を使用して状態を確認できます。

![失敗したミドルウェアのシミュレーションの状態を確認するスクリーンショット。](./media/implement-circuit-breaker-pattern/failing-middleware-simulation.png)

**図 8-5** "failing" ASP.NET ミドルウェアの状態を確認している。この場合は無効。

この時点で Basket マイクロサービスは、呼び出すたびに状態コード 500 を返します。

ミドルウェアを実行したら、MVC Web アプリから注文してみることができます。 要求は失敗するため、サーキットがオープン状態になります。

次の例では、MVC Web アプリが、注文のためのロジック内に catch ブロックを含んでいることがわかります。  コードがサーキット オープンの例外を捕捉すると、ユーザーに待機を促すメッセージを表示します。

```csharp
public class CartController : Controller
{
    //…
    public async Task<IActionResult> Index()
    {
        try
        {
            var user = _appUserParser.Parse(HttpContext.User);
            //Http requests using the Typed Client (Service Agent)
            var vm = await _basketSvc.GetBasket(user);
            return View(vm);
        }
        catch (BrokenCircuitException)
        {
            // Catches error when Basket.api is in circuit-opened mode
            HandleBrokenCircuitException();
        }
        return View();
    }

    private void HandleBrokenCircuitException()
    {
        TempData["BasketInoperativeMsg"] = "Basket Service is inoperative, please try later on. (Business message due to Circuit-Breaker)";
    }
}
```

まとめます。 再試行ポリシーは、HTTP 要求の実行を数回試行し、HTTP エラーが発生します。 再試行回数がサーキット ブレーカー ポリシーに設定された最大回数に達すると (この場合は 5 回)、アプリケーションは BrokenCircuitException をスローします。 結果として、図 8-6 に示されているようなメッセージが表示されます。

![バスケット サービスが機能しないというエラーを含む MVC Web アプリのスクリーンショット。](./media/implement-circuit-breaker-pattern/basket-service-inoperative.png)

**図 8-6** サーキット ブレーカーが UI にエラーを返している

サーキットをいつオープン状態にするか/中断するかに関して、別のロジックを実装できます。 または、フォールバック データセンターか冗長バックエンド システムがある場合、他のバックエンド マイクロサービスに対して HTTP 要求を試行することができます。

最後に、`CircuitBreakerPolicy` の別の可能性として、`Isolate` (サーキットを強制的にオープン状態にし、オープン状態を維持する) と `Reset` (もう一度クローズド状態にする) を使用する方法があります。 これらは、ポリシーに対して Isolate と Reset を直接呼び出すユーティリティ HTTP エンドポイントを構築するために使用できます。  このような HTTP エンドポイントは、アップグレードしたい場合など、ダウンストリーム システムを一時的に分離するために運用環境で適切な安全性を保つために使用することもできます。 または、障害が発生する恐れがあるダウンストリーム システムを保護するために手動でサーキットを切断することもできます。

## <a name="additional-resources"></a>その他の技術情報

- **サーキット ブレーカー パターン**\
  [https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker](/azure/architecture/patterns/circuit-breaker)

>[!div class="step-by-step"]
>[前へ](implement-http-call-retries-exponential-backoff-polly.md)
>[次へ](monitor-app-health.md)
