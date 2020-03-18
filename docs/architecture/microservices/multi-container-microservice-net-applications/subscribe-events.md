---
title: イベントへのサブスクライブ
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | 統合イベントの発行とサブスクライブについて。
ms.date: 01/30/2020
ms.openlocfilehash: 544af8035ed23dd6507dfed4944b0c327c81d943
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77501805"
---
# <a name="subscribing-to-events"></a>イベントへのサブスクライブ

イベント バスを使用するための最初のステップは、受信したいイベントにマイクロサービスをサブスクライブすることです。 これは、受信側マイクロサービスで行う必要があります。

次の単純なコードは、各受信側マイクロサービスが (`Startup` クラスで) サービスを起動して必要なイベントにサブスクライブするために実装する必要があるコードを示しています。 このケースでは、`basket-api` マイクロサービスが `ProductPriceChangedIntegrationEvent` メッセージと `OrderStartedIntegrationEvent` メッセージにサブスクライブする必要があります。

たとえば、`ProductPriceChangedIntegrationEvent` イベントにサブスクライブした場合、買い物かごマイクロサービスは商品価格に加えられた変更を認識し、その商品がユーザーの買い物かごに入っている場合は変更に関する警告をユーザーに表示できるようになります。

```csharp
var eventBus = app.ApplicationServices.GetRequiredService<IEventBus>();

eventBus.Subscribe<ProductPriceChangedIntegrationEvent,
                   ProductPriceChangedIntegrationEventHandler>();

eventBus.Subscribe<OrderStartedIntegrationEvent,
                   OrderStartedIntegrationEventHandler>();

```

このコードの実行後、サブスクライバー マイクロサービスは、RabbitMQ チャネルを介してリッスンするようになります。 ProductPriceChangedIntegrationEvent 型のメッセージが到着すると、コードは渡されたイベント ハンドラーを起動し、イベントを処理します。

## <a name="publishing-events-through-the-event-bus"></a>イベント バスを介したイベントの発行

最後に、メッセージ送信元 (送信元マイクロサービス) が、次の例のようなコードで統合イベントを発行します (これは原子性を考慮していない単純化された例です)。イベントを複数のマイクロサービスにわたって伝達する必要がある場合は、同様のコードを、通常は送信元マイクロサービスからデータまたはトランザクションがコミットされた直後に実装します。

まず、次のコードに示すように、(RabbitMQ またはサービス バスに基づく) イベント バス実装オブジェクトをコントローラー コンストラクターで挿入します。

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _context;
    private readonly IOptionsSnapshot<Settings> _settings;
    private readonly IEventBus _eventBus;

    public CatalogController(CatalogContext context,
        IOptionsSnapshot<Settings> settings,
        IEventBus eventBus)
    {
        _context = context;
        _settings = settings;
        _eventBus = eventBus;
    }
    // ...
}
```

次に、コントローラーのメソッド (UpdateProduct メソッドなど) でそれを使用します。

```csharp
[Route("items")]
[HttpPost]
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem product)
{
    var item = await _context.CatalogItems.SingleOrDefaultAsync(
        i => i.Id == product.Id);
    // ...
    if (item.Price != product.Price)
    {
        var oldPrice = item.Price;
        item.Price = product.Price;
        _context.CatalogItems.Update(item);
        var @event = new ProductPriceChangedIntegrationEvent(item.Id,
            item.Price,
            oldPrice);
        // Commit changes in original transaction
        await _context.SaveChangesAsync();
        // Publish integration event to the event bus
        // (RabbitMQ or a service bus underneath)
        _eventBus.Publish(@event);
        // ...
    }
    // ...
}
```

このケースでは、送信元マイクロサービスが単純な CRUD マイクロサービスであるため、そのコードを Web API コントローラー内に直接配置しています。

CQRS アプローチを使用する場合など、より高度なマイクロサービスでは、`CommandHandler` クラスの `Handle()` メソッド内に実装することもできます。

### <a name="designing-atomicity-and-resiliency-when-publishing-to-the-event-bus"></a>イベント バスに発行するときの原子性と回復性の設計

イベント バスのような分散メッセージング システムを通じて統合イベントを発行するときは、元のデータベースの更新とイベントの発行のアトミックな実行という問題が生じます (つまり、両方の操作が完了するか、いずれも完了しない)。 たとえば、先ほどの単純化された例では、コードは商品価格が変更されたときにデータベースにデータをコミットし、その後に ProductPriceChangedIntegrationEvent メッセージを発行します。 一見すると、これらの 2 つの操作がアトミックに実行されることは不可欠のように思われます。 しかし、[Microsoft Message Queuing (MSMQ)](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx) などの以前のシステムを使用している場合と同じく、データベースとメッセージ ブローカーが関与する分散トランザクションを使用している場合には、これは [CAP 定理](https://www.quora.com/What-Is-CAP-Theorem-1)で説明されている理由によって推奨されません。

基本的に、マイクロサービスは、スケーラブルで可用性の高いシステムを構築するために使用します。 簡単に言えば、CAP 定理は、継続的な可用性、強い一貫性、*かつ*分断への耐性を備えた (分散) データベース (またはそのモデルを持つマイクロサービス) を構築することは不可能であると述べています。 これらの 3 つの特性のうち、2 つを選択しなければなりません。

マイクロサービス ベースのアーキテクチャでは、可用性と耐性を選択して、厳密な一貫性は重視しないようにする必要があります。 したがって、最近のほとんどのマイクロサービス ベースのアプリケーションでは、[MSMQ](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx) を使用して Windows の分散トランザクション コーディネーター (DTC) に基づく[分散トランザクション](https://docs.microsoft.com/previous-versions/windows/desktop/ms681205(v=vs.85))を実装する場合と同じく、メッセージングに分散トランザクションを使用しないのが普通です。

最初の問題とその例に話を戻しましょう。 データベースが更新された後 (このケースでは、\_context.SaveChangesAsync() を含むコード行の直後) で、統合イベントが発行される前にサービスがクラッシュした場合、システム全体の一貫性が失われる可能性があります。 これは、処理しているビジネス操作によっては、ビジネスに重大な問題を生じさせるおそれがあります。

前のアーキテクチャのセクションで説明したように、この問題に対処するためのアプローチはいくつかあります。

- 完全な[イベント ソーシング パターン](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)を使用する。

- [トランザクション ログ マイニング](https://www.scoop.it/t/sql-server-transaction-log-mining)を使用する。

- [送信トレイ パターン](https://www.kamilgrzybek.com/design/the-outbox-pattern/)を使用する。 これは、(ローカル トランザクションを拡張して) 統合イベントを格納するトランザクション テーブルです。

このシナリオでは、完全なイベント ソーシング (ES) パターンを使用することが、*最良*とは言わないまでも適切なアプローチの 1 つです。 ただし、多くのアプリケーション シナリオでは、完全な ES システムを実装できない場合があります。 ES とは、現在の状態データを格納するのではなく、ドメイン イベントのみをトランザクション データベースに格納することを意味します。 ドメイン イベントのみを格納する方法には、システムの履歴を保持できる、過去の任意の時点におけるシステムの状態を確認できるなどの大きなメリットがあります。 しかし、完全な ES システムを実装するにはシステムの大部分を再構築する必要があり、それ以外にも多くの複雑さと要件が生じます。 たとえば、イベント ソーシング用に特別に作成されたデータベース ([イベント ストア](https://eventstore.org/)など) や、Azure Cosmos DB、MongoDB、Cassandra、CouchDB、RavenDB といったドキュメント指向のデータベースの使用が必要になる場合があります。 ES はこの問題に対する優れたアプローチですが、イベント ソーシングに精通している人以外には最も簡単なソリューションではありません。

トランザクション ログ マイニングを使用するという選択肢は、一見すると透明性の高い方法のように思われます。 しかし、このアプローチを使用するには、マイクロサービスを SQL Server トランザクション ログなどの RDBMS トランザクション ログと結合する必要があります。 これはおそらく望ましくないでしょう。 もう 1 つの欠点は、トランザクション ログに記録された低レベルの更新が、高レベルの統合イベントと同じレベルではない可能性があるという点です。 その場合、それらのトランザクション ログ操作をリバース エンジニアリングするプロセスが難しくなるおそれがあります。

バランスの取れたアプローチは、トランザクション データベース テーブルと単純化された ES パターンを組み合わせることです。 “イベント発行準備完了” のような状態を使用できます。これは、元のイベントを統合イベント テーブルにコミットするときに、元のイベントで設定します。 その後、イベント バスへのイベントの発行を試みます。 イベント発行アクションが成功した場合は、発行元のサービスで別のトランザクションを開始し、状態を “イベント発行準備完了” から “イベント発行済み” に移行します。

イベント バス内でイベント発行アクションが失敗した場合、発行元のマイクロサービス内ではデータの一貫性がまだ維持されており (“イベント発行準備完了” とマークされたままの状態)、サービスの残りの部分に関しても一貫性が維持されます。 トランザクションや統合イベントの状態は、バックグラウンド ジョブによって常に確認できます。 ジョブが “イベント発行準備完了” 状態のイベントを検出した場合、イベント バスに対するそのイベントの再発行を試みることができます。

このアプローチでは、各発行元マイクロサービスの統合イベントと、他のマイクロサービスまたは外部システムに伝達したいイベントのみを保持するという点に注意してください。 これに対して、完全な ES システムでは、すべてのドメイン イベントも格納します。

したがって、このバランスの取れたアプローチは、単純化された ES システムということになります。 統合イベントとその現在の状態 (“発行準備完了” または “発行済み”) の一覧が必要です。 しかし、必要なのは、統合イベントのこれらの状態を実装することだけです。 また、このアプローチでは、完全な ES システムの場合のように、すべてのドメイン データをイベントとしてトランザクション データベースに格納する必要はありません。

既にリレーショナル データベースを使用している場合は、トランザクション テーブルを使用して統合イベントを格納できます。 アプリケーションで原子性を実現するには、ローカル トランザクションに基づく 2 ステップのプロセスを使用します。 基本的には、ドメイン エンティティを保持しているのと同じデータベースで IntegrationEvent テーブルを保持します。 そのテーブルが原子性を実現するための保証として機能して、保持された統合イベントを、ドメイン データをコミットしているのと同じトランザクションに格納できるようにします。

次のように、このプロセスは段階的に進みます。

1. アプリケーションによってローカル データベース トランザクションが開始されます。

2. 次に、ドメイン エンティティの状態を更新し、統合イベント テーブルにイベントを挿入します。

3. 最後に、トランザクションがコミットされます。これで目的の原子性が与えられます。次に、

4. 何らかの方法でイベントを発行します (次へ)。

イベント発行のステップを実装するときは、次の選択肢があります。

- トランザクションをコミットした直後に統合イベントを発行し、別のローカル トランザクションを使用して、テーブル内のイベントを発行中とマークする。 その後、リモート マイクロサービスで問題が発生した場合は、テーブルを単なるアーティファクトとして使用して統合イベントを追跡し、格納された統合イベントに基づいて補正アクションを実行する。

- テーブルを一種のキューとして使用する。 独立したアプリケーション スレッドまたはプロセスが統合イベント テーブルのクエリを実行し、イベント バスにイベントを発行した後、ローカル トランザクションを使用してイベントを発行済みとマークする。

図 6-22 は、1 番目のアプローチのアーキテクチャを示しています。

![ワーカー マイクロサービスを使用せずに発行する場合の原子性の図。](./media/subscribe-events/atomicity-publish-event-bus.png)

**図 6-22**。 イベント バスにイベントを発行するときの原子性

図 6-22 に示したアプローチには、発行した統合イベントの成功のチェックと確認を行う、追加のワーカー マイクロサービスがありません。 失敗した場合、その追加のチェッカー ワーカー マイクロサービスは、テーブルからイベントを読み取って再発行できます。つまり、手順 2 を繰り返します。

2 番目のアプローチでは、EventLog テーブルをキューとして使用し、常に常にワーカー マイクロサービスを使用してメッセージを発行します。 その場合、プロセスは図 6-23 のようになります。 これには追加のマイクロサービスが表示されており、テーブルはイベントを発行する際の単一のソースです。

![ワーカー マイクロサービスを使用して発行するときの原子性の図。](./media/subscribe-events/atomicity-publish-worker-microservice.png)

**図 6-23**。 ワーカー マイクロサービスを使用してイベント バスにイベントを発行するときの原子性

わかりやすくするために、eShopOnContainers サンプルでは、1 番目のアプローチ (追加のプロセスまたはチェッカー マイクロサービスなし) とイベント バスを使用しています。 ただし、eShopOnContainers は、考えられるすべてのエラー ケースに対応しません。 クラウドにデプロイされた実際のアプリケーションでは、将来的に問題が発生して、チェックと再送信のロジックを実装しなければならなくなることを承知しておく必要があります。 テーブルをキューとして使用する方法は、そのテーブルをイベント バスを通じて (ワーカーで) イベントを発行するときの単一のイベント ソースとする場合には、1 番目のアプローチよりも効果的な可能性があります。

### <a name="implementing-atomicity-when-publishing-integration-events-through-the-event-bus"></a>イベント バスを通じて統合イベントを発行するときの原子性の実装

次のコードは、複数の DbContext オブジェクト (更新される元のデータに関連する 1 つのコンテキストと、IntegrationEventLog テーブルに関連するもう 1 つのコンテキスト) を含む単一のトランザクションを作成する方法を示しています。

下のサンプル コードに示したトランザクションは、コードの実行時にデータベースへの接続に問題が発生した場合の回復性を備えていないことに注意してください。 これは、サーバー間でデータベースを移動する可能性がある Azure SQL DB のようなクラウドベースのシステムで発生する可能性があります。 複数のコンテキストにわたる回復性を備えたトランザクションの実装については、このガイドの後のセクション「[回復性の高い Entity Framework Core SQL 接続の実装](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md)」をご覧ください。

わかりやすくするために、次の例では、プロセス全体を 1 つのコード片で示しています。 ただし、eShopOnContainers の実装は実際にはリファクターされ、このロジックは管理しやすいように複数のクラスに分割されます。

```csharp
// Update Product from the Catalog microservice
//
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem productToUpdate)
{
  var catalogItem =
       await _catalogContext.CatalogItems.SingleOrDefaultAsync(i => i.Id ==
                                                               productToUpdate.Id);
  if (catalogItem == null) return NotFound();

  bool raiseProductPriceChangedEvent = false;
  IntegrationEvent priceChangedEvent = null;

  if (catalogItem.Price != productToUpdate.Price)
          raiseProductPriceChangedEvent = true;

  if (raiseProductPriceChangedEvent) // Create event if price has changed
  {
      var oldPrice = catalogItem.Price;
      priceChangedEvent = new ProductPriceChangedIntegrationEvent(catalogItem.Id,
                                                                  productToUpdate.Price,
                                                                  oldPrice);
  }
  // Update current product
  catalogItem = productToUpdate;

  // Just save the updated product if the Product's Price hasn't changed.
  if (!raiseProductPriceChangedEvent)
  {
      await _catalogContext.SaveChangesAsync();
  }
  else  // Publish to event bus only if product price changed
  {
        // Achieving atomicity between original DB and the IntegrationEventLog
        // with a local transaction
        using (var transaction = _catalogContext.Database.BeginTransaction())
        {
           _catalogContext.CatalogItems.Update(catalogItem);
           await _catalogContext.SaveChangesAsync();

           // Save to EventLog only if product price changed
           if(raiseProductPriceChangedEvent)
               await _integrationEventLogService.SaveEventAsync(priceChangedEvent);

           transaction.Commit();
        }

      // Publish the integration event through the event bus
      _eventBus.Publish(priceChangedEvent);

      integrationEventLogService.MarkEventAsPublishedAsync(
                                                priceChangedEvent);
  }

  return Ok();
}

```

ProductPriceChangedIntegrationEvent 統合イベントが作成された後は、元のドメイン操作 (カタログ アイテムの更新) を格納するトランザクションも、EventLog テーブル内のイベントの保持を含むようになります。 これは単一のトランザクションで行われ、イベント メッセージが送信されたかどうかを常に確認できるようになります。

元のデータベース操作では、同じデータベースに対するローカル トランザクションを使用して、イベント ログ テーブルがアトミックに更新されます。 何らかの操作が失敗した場合は、例外がスローされ、完了した操作がトランザクションによってロールバックされます。それによって、ドメイン操作と、テーブルに保存されたイベント メッセージの間の一貫性が維持されます。

### <a name="receiving-messages-from-subscriptions-event-handlers-in-receiver-microservices"></a>サブスクリプションからのメッセージの受信: 受信側マイクロサービスのイベント ハンドラー

イベント サブスクリプションのロジックに加えて、統合イベント ハンドラー (コールバック メソッドなど) 用の内部コードも実装する必要があります。 イベント ハンドラーでは、特定の種類のイベント メッセージが受信および処理される場所を指定します。

イベント ハンドラーは、最初に、イベント バスからイベント インスタンスを受信します。 次に、その統合イベントに関連する処理対象のコンポーネントを見つけ出し、受信側マイクロサービス内でそのイベントを状態の変更として伝達および保持します。 たとえば、カタログ マイクロサービスで ProductPriceChanged イベントが発生した場合、次のコードに示すように、イベントは買い物かごマイクロサービス内で処理され、この受信側買い物かごマイクロサービス内でも状態が変更されます。

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.IntegrationEvents.EventHandling
{
    public class ProductPriceChangedIntegrationEventHandler :
        IIntegrationEventHandler<ProductPriceChangedIntegrationEvent>
    {
        private readonly IBasketRepository _repository;

        public ProductPriceChangedIntegrationEventHandler(
            IBasketRepository repository)
        {
            _repository = repository;
        }

        public async Task Handle(ProductPriceChangedIntegrationEvent @event)
        {
            var userIds = await _repository.GetUsers();
            foreach (var id in userIds)
            {
                var basket = await _repository.GetBasket(id);
                await UpdatePriceInBasketItems(@event.ProductId, @event.NewPrice, basket);
            }
        }

        private async Task UpdatePriceInBasketItems(int productId, decimal newPrice,
            CustomerBasket basket)
        {
            var itemsToUpdate = basket?.Items?.Where(x => int.Parse(x.ProductId) ==
                productId).ToList();
            if (itemsToUpdate != null)
            {
                foreach (var item in itemsToUpdate)
                {
                    if(item.UnitPrice != newPrice)
                    {
                        var originalPrice = item.UnitPrice;
                        item.UnitPrice = newPrice;
                        item.OldUnitPrice = originalPrice;
                    }
                }
                await _repository.UpdateBasket(basket);
            }
        }
    }
}
```

イベント ハンドラーは、その商品がいずれかの買い物かごインスタンス内に存在するかどうかを確認する必要があります。 また、関連する買い物かご品目の品目価格をすべて更新します。 最後に、図 6-24 に示すように、ユーザーに表示する価格変更についてのアラートを作成します。

![ユーザー カートで価格変更の通知を示しているブラウザーのスクリーンショット。](./media/subscribe-events/display-item-price-change.png)

**図 6-24**。 統合イベントによって通知された、買い物かご内の品目の価格変更の表示

## <a name="idempotency-in-update-message-events"></a>更新メッセージ イベントでのべき等

更新メッセージ イベントにおける重要な側面の 1 つは、通信のどの時点でエラーが発生しても、メッセージが再試行される必要があるという点です。 そうでない場合、バックグラウンド タスクが発行済みのイベントを発行しようと試みて、競合状態が発生するおそれがあります。 更新がべき等であるか、または、確実に重複を検出し、破棄し、1 つの応答のみを送信するために十分な情報が更新によって提供されることを確認する必要があります。

前述のように、べき等とは、操作を複数回実行しても結果が変わらないことを意味します。 メッセージング環境では、イベントを通信する際に、イベントを複数回送信しても受信側マイクロサービスにとっての結果が変わらない場合、イベントはべき等であるといえます。 これは、イベント自体の性質、またはシステムがイベント処理する方法により、必要となります。 メッセージのべき等性は、イベント バス パターンを実装するアプリケーションだけでなく、メッセージングを使用するすべてのアプリケーションにおいて重要です。

べき等操作の一例として、データがまだテーブルに存在しない場合にのみテーブルにデータを挿入する SQL ステートメントがあります。 INSERT SQL ステートメントを何回実行するかは重要ではなく、結果 (テーブルにそのデータが含まれること) は同じになります。 このようなべき等性は、複数回送信され、複数回処理される可能性があるメッセージを扱う場合にも必要になる可能性があります。 たとえば、再試行ロジックによって送信元がまったく同じメッセージを複数回送信する場合、メッセージがべき等であることを確認する必要があります。

べき等なメッセージを設計することは可能です。 たとえば、"商品価格を $25 に設定する" というイベントを、"商品価格に $5 を追加する" というイベントの代わりに作成できます。 最初のメッセージを何回でも安全に処理することができ、結果は同じになります。 2 番目のメッセージでは、そうではありません。 しかし、1 番目のケースであっても、システムが新しい価格変更イベントを送信済みでその新しい価格を上書きする可能性もあるため、1 番目のイベントを処理することは望ましくありません。

もう 1 つの例は、複数のサブスクライバーに伝達される注文完了イベントです。 同じ注文完了イベントに対する重複したメッセージ イベントがある場合でも、他のシステムで注文情報が 1 回だけ更新されることが重要です。

各イベントが各受信者に対して 1 回のみ処理されるように強制するロジックを作成できるように、何らかの ID があると便利です。

メッセージ処理の中には、本質的にべき等なものもあります。 たとえば、システムがサムネイル画像を生成する場合、生成されたサムネイルに関するメッセージが処理される回数は関係ありません。結果はサムネイルが生成されたということであり、これは毎回同じです。 一方、支払いゲートウェイを呼び出してクレジット カードに請求するというような操作は、べき等である必要はまったくありません。 そのようなケースでは、メッセージを複数回処理することで目的の効果が得られるということを確認する必要があります。

### <a name="additional-resources"></a>その他の技術情報

- **メッセージのべき等性に従う** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591565(v=pandp.10)#honoring-message-idempotency>

## <a name="deduplicating-integration-event-messages"></a>統合イベント メッセージの重複除去

メッセージ イベントが 1 つのサブスクライバーにつき 1 回のみ送信および処理されたということは、さまざまなレベルで確認できます。 1 つの方法は、使用しているメッセージング インフラストラクチャが提供している重複除去機能を使用することです。 もう 1 つの方法は、送信先マイクロサービスにカスタムロジックを実装することです。 トランスポート レベルとアプリケーション レベルの両方で検証を行うことは、最善の方法です。

### <a name="deduplicating-message-events-at-the-eventhandler-level"></a>EventHandler レベルでのメッセージ イベントの重複除去

イベントが受信者によって 1 回だけ処理されたことを確認する 1 つの方法は、イベント ハンドラーでメッセージ イベントを処理する際に特定のロジックを実装することです。 たとえば、それは eShopOnContainers アプリケーションで使用されるアプローチで、UserCheckoutAcceptedIntegrationEvent 統合イベントを受け取るときに [UserCheckoutAcceptedIntegrationEventHandler クラスのソース コード](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs)で確認できます (この場合、コマンド ハンドラーに送信する前に、識別子として eventMsg.RequestId を使用し、IdentifiedCommand で CreateOrderCommand をラップします。)。

### <a name="deduplicating-messages-when-using-rabbitmq"></a>RabbitMQ を使用するときのメッセージの重複除去

断続的なネットワーク エラーが発生する場合、メッセージが重複している可能性があり、メッセージ受信側はこれらの重複したメッセージを処理する準備ができている必要があります。 受信側は、可能な場合、べき等な方法でメッセージを処理する必要があります。これは、重複除去でメッセージを明示的に処理する方法よりも優れています。

[RabbitMQ のドキュメント](https://www.rabbitmq.com/reliability.html#consumer)によると、メッセージがコンシューマーに配信された後で (コンシューマーとの接続が切断される前に受信確認されなかったなどの理由で) キューに再登録された場合、RabbitMQ は、メッセージが再配信されるときに (配信先が同じコンシューマーか別のコンシューマーかにかかわらず) 再配信フラグをメッセージに設定します。

“再配信” フラグが設定されている場合、受信側はそれを考慮する必要があります。なぜなら、メッセージが既に処理されている可能性があるためです。 しかしそれは保証されているわけではありません。メッセージはメッセージ ブローカーから送信された後、ネットワークの問題が原因で一度も受信側に到達していない可能性もあります。 その一方で、“再配信” フラグが設定されていない場合、メッセージが複数回送信されていないことが保証されます。 したがって、受信側は、“再配信” フラグがメッセージに設定されている場合にのみ、メッセージを重複除去するか、べき等な方法でメッセージを処理する必要があります。

### <a name="additional-resources"></a>その他の技術情報

- **NServiceBus を使用するフォークされた eShopOnContainers (Particular Software)**  \
    <https://go.particular.net/eShopOnContainers>

- **イベント駆動型メッセージング** \
    <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- **Jimmy Bogard。復元性を目指したリファクタリング: 結合の評価** \
    <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

- **発行-サブスクライブ チャンネル** \
    <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **境界コンテキスト間の通信** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **最終的な整合性** \
    <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Philip Brown。境界コンテキストを統合するための戦略** \
    <https://www.culttt.com/2014/11/26/strategies-integrating-bounded-contexts/>

- **Chris Richardson。集約、イベント ソース、CQRS を使用したトランザクション マイクロサービスの開発 - パート 2** \
    <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-2-richardson>

- **Chris Richardson。イベント ソーシング パターン** \
    <https://microservices.io/patterns/data/event-sourcing.html>

- **イベント ソーシングの概要** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591559(v=pandp.10)>

- **イベント ストア データベース**。 公式サイト。 \
    <https://geteventstore.com/>

- **Patrick Nommensen。マイクロサービス用のイベント ドリブン データ管理** \
    <https://dzone.com/articles/event-driven-data-management-for-microservices-1>

- **CAP 定理** \
    <https://en.wikipedia.org/wiki/CAP_theorem>

- **CAP 定理とは?** \
    <https://www.quora.com/What-Is-CAP-Theorem-1>

- **データ整合性の概要** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- **Rick Saling。CAP 定理: クラウドとインターネットでは "すべてが異なる" 理由** \
    <https://docs.microsoft.com/archive/blogs/rickatmicrosoft/the-cap-theorem-why-everything-is-different-with-the-cloud-and-internet/>

- **Eric Brewer。12 年後の CAP: "規則" はどのように変わったか** \
    <https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>

- **Azure Service Bus。ブローカー メッセージング: 重複データ検出**  \
    <https://code.msdn.microsoft.com/Brokered-Messaging-c0acea25>

- **信頼性ガイド** (RabbitMQ ドキュメント) \
    <https://www.rabbitmq.com/reliability.html#consumer>

> [!div class="step-by-step"]
> [前へ](rabbitmq-event-bus-development-test-environment.md)
> [次へ](test-aspnet-core-services-web-apps.md)
