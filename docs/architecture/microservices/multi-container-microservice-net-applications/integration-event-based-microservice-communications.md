---
title: マイクロサービス間でイベント ベースの通信を実装する (統合イベント)
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | マイクロサービス間でイベント ベースの通信を実装する統合イベントを理解する
ms.date: 10/02/2018
ms.openlocfilehash: 6d4e324a05def91935a82df41c971a75cb75c3f8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712404"
---
# <a name="implementing-event-based-communication-between-microservices-integration-events"></a>マイクロサービス間でイベント ベースの通信を実装する (統合イベント)

前述のように、イベント ベースの通信を使う場合、ビジネス エンティティの更新などの重要なできごとが発生すると、マイクロサービスがイベントを発行します。 その他のマイクロサービスは、これらのイベントをサブスクライブします。 マイクロサービスは、イベントを受信するときにそれ自体のビジネス エンティティを更新する場合があり、それによってさらにイベントが発行される可能性があります。 これは、最終的な整合性の概念の本質です。 この発行/サブスクライブ システムは、通常はイベント バスの実装を使って実行されます。 イベント バスは、イベントのサブスクライブおよびサブスクライブ解除とイベントの発行に必要な API を含むインターフェイスとして設計することができます。 また、イベント バスは、任意のプロセス間通信またはメッセージング通信に基づく 1 つ以上の実装を使うことができます。たとえば、非同期通信と発行/サブスクライブ モデルをサポートするメッセージング キューまたはサービス バスです。

イベントを使って、複数のサービスにまたがるビジネス トランザクションを実装できます。これにより、サービス間の最終的な整合性が実現されます。 最終的に整合性のあるトランザクションは、一連の分散アクションで構成されます。 各アクションにおいて、マイクロサービスはビジネス エンティティを更新し、次のアクションをトリガーするイベントを発行します。 次の図 6-18 は、PriceUpdated イベントがイベント バスを介して発行され、価格の更新はバスケットやその他のマイクロサービスに反映されることを示しています。

![イベント バスを使用した非同期のイベント ドリブン通信の図。](./media/integration-event-based-microservice-communications/event-driven-communication.png)

**図 6-18**。 イベント バスに基づくイベントドリブン通信

このセクションでは、図 6-18 に示すように、ジェネリックなイベント バスのインターフェイスを使って、この種類の通信を .NET で実装する方法について説明します。 さまざまなテクノロジやインフラストラクチャ (RabbitMQ、Azure Service Bus、またはその他のサード パーティ製のオープン ソースまたは商用のサービス バスなど) を使った、複数の実装が可能です。

## <a name="using-message-brokers-and-services-buses-for-production-systems"></a>運用システムでのメッセージ ブローカーとサービス バスの使用

アーキテクチャのセクションで説明したように、抽象イベント バスを実装するためのメッセージング テクノロジには、複数の選択肢があります。 ただし、これらのテクノロジは動作するレベルが異なります。 たとえば、メッセージング ブローカーのトランスポートである RabbitMQ は、Azure Service Bus、NServiceBus、MassTransit、Brighter などの商用の製品よりも低いレベルで動作します。 これらの製品のほとんどは、RabbitMQ または Azure Service Bus の上部で動作させることができます。 製品の選択は、アプリケーションに必要な機能の数や、すぐに利用できるスケーラビリティの量によって異なります。

開発環境でイベント バスの概念実証を実装するだけの場合は、eShopOnContainers サンプルのように、コンテナーとして実行される RabbitMQ の上部の単純な実装で十分です。 ただし、高いスケーラビリティが求められるミッションクリティカルな運用システムでは、Azure Service Bus を評価して使うことをお勧めします。

分散開発を容易にする実行時間の長いプロセスのために、[Sagas](https://docs.particular.net/nservicebus/sagas/) のような高レベルの抽象化と豊富な機能が必要な場合は、NServiceBus、MassTransit、Brighter などのその他の商用およびオープン ソース サービス バスをお勧めします。 この場合は通常、独自の抽象化ではなく、これらの高レベルのサービス バスによって提供される抽象化と API を直接使います ([eShopOnContainers で提供される単純なイベント バスの抽象化](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/BuildingBlocks/EventBus/EventBus/Abstractions/IEventBus.cs)のように)。 さらに、[NServiceBus を使う eShopOnContainers の分岐](https://go.particular.net/eShopOnContainers) (Particular Software によって実装される追加の派生サンプル) を調査することもできます。

もちろん、常に RabbitMQ や Docker のような下位レベルのテクノロジの上に独自のサービス バス機能を構築することもできますが、"車輪の再発明" に必要な作業は、カスタム エンタープライズ アプリケーションにとってコストがかかりすぎる可能性があります。

繰り返しになりますが、eShopOnContainers サンプルで紹介されたサンプルのイベント バスの抽象化と実装は、概念実装としてのみ使用されます。 現在のセクションで説明されているように、非同期とイベント起動型の通信が必要であると判断したら、運用環境でのニーズに最適なサービス バス製品を選ぶ必要がります。

## <a name="integration-events"></a>統合イベント

統合イベントは、ドメインを複数のマイクロサービスまたは外部システムにわたって同期するために使われます。 これは、統合イベントをマイクロサービスの外部に発行することによって実現されます。 複数の受信側マイクロサービス (統合イベントをサブスクライブしているすべてのマイクロサービス) にイベントが発行されると、各受信側マイクロサービスの適切なイベント ハンドラーがイベントを処理します。

統合イベントは、次の例のように、基本的にはデータを保持するクラスです。

```csharp
public class ProductPriceChangedIntegrationEvent : IntegrationEvent
{
    public int ProductId { get; private set; }
    public decimal NewPrice { get; private set; }
    public decimal OldPrice { get; private set; }

    public ProductPriceChangedIntegrationEvent(int productId, decimal newPrice,
        decimal oldPrice)
    {
        ProductId = productId;
        NewPrice = newPrice;
        OldPrice = oldPrice;
    }
}
```

統合イベントは各マイクロサービスのアプリケーション レベルで定義できるため、他のマイクロサービスから切り離されます。これは、ViewModels をサーバーやクライアントで定義する方法に相当します。 共通の統合イベント ライブラリを複数のマイクロサービス間で共有することはお勧めできません。これらのマイクロサービスを、単一のイベント定義データ ライブラリで結合することになるからです。 これを回避する理由は、共通のドメイン モデルを複数のマイクロサービス間で共有しない理由と同じです。つまり、マイクロサービスは完全に自律している必要があります。

マイクロサービス間で共有する必要があるライブラリは数種類のみです。 1 つは、eShopOnContainers の場合ように、[イベント バスのクライアント API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/BuildingBlocks/EventBus) のような、最後のアプリケーション ブロックとなるライブラリです。 もう 1 つは、JSON シリアライザーのように、NuGet コンポーネントとしても共有できるツールを構成するライブラリです。

## <a name="the-event-bus"></a>イベント バス

イベント バスを使うと、図 6-19 に示すように、コンポーネントが互いを明示的に認識せずに、マイクロサービス間の発行/サブスクライブ スタイルの通信が可能になります。

![基本的な発行/サブスクライブ パターンを示す図。](./media/integration-event-based-microservice-communications/publish-subscribe-basics.png)

**図 6-19**。 イベント バスでの発行/サブスクライブの基礎

上の図は、マイクロサービス A でイベント バスに発行し、これにより、発行者がサブスクライバーを知らなくても、マイクロサービス B と C をサブスクライブするように分散されることを示しています。 イベント バスは、オブザーバー パターンと発行-サブスクライブ パターンに関連しています。

### <a name="observer-pattern"></a>オブザーバー パターン

[オブザーバー パターン](https://en.wikipedia.org/wiki/Observer_pattern)では、プライマリ オブジェクト (オブザーバブルと呼ばれます) が登録されている他のオブジェクト (オブザーバーと呼ばれます) に関連情報 (イベント) を通知します。

### <a name="publishsubscribe-pubsub-pattern"></a>発行/サブスクライブ (Pub/Sub) パターン

[発行/サブスクライブ パターン](https://docs.microsoft.com/previous-versions/msp-n-p/ff649664(v=pandp.10))の目的はオブザーバー パターンと同じです。特定のイベントが発生したことを他のサービスに通知する必要があります。 ただし、オブザーバー パターンと Pub/Sub パターンの間には重要な違いがあります。 オブザーバー パターンでは、オブザーバブルからオブザーバーに直接ブロードキャストされるため、これらは互いを "認識" しています。 しかし、Pub/Sub パターンを使う場合は、ブローカー、メッセージ ブローカー、またはイベント バスと呼ばれる、発行者とサブスクライバーの両方から認識される第 3 のコンポーネントが存在します。 そのため、Pub/Sub パターンを使うと、前述したイベント バス (またはメッセージ ブローカー) によって、発行者とサブスクライバーが厳密に切り離されます。

### <a name="the-middleman-or-event-bus"></a>仲介者またはイベント バス

発行者とサブスクライバーの間の匿名性は、どのように実現すればよいのでしょうか。 簡単な方法は、仲介者にすべての通信を処理させることです。 イベント バスは、このような仲介者の 1 つです。

イベント バスは通常 2 つの部分で構成されます。

- 抽象化またはインターフェイス。

- 1 つ以上の実装。

図 6-19 では、イベント バスが、アプリケーションの観点からは、Pub/Sub のチャネルにすぎないことがわかります。 この非同期通信は、さまざまな方法で実装できます。 環境の要件 (たとえば、運用環境と開発環境) に応じて実装を切り替えるために、複数の実装を使うことができます。

図 6-20 では、インフラストラクチャ メッセージング テクノロジ (RabbitMQ、Azure Service Bus、またはその他のイベント/メッセージ ブローカーなど) に基づく複数の実装での、イベント バスの抽象化を確認できます。

![イベント バス抽象化レイヤーの追加を示す図。](./media/integration-event-based-microservice-communications/multiple-implementations-event-bus.png)

**図 6-20**。 イベント バスの複数の実装

RabbitMQ Azure Service バスなどのいくつかのテクノロジで実装できるように、インターフェイスから定義されたイベント バスを用意することをお勧めします。 ただし、前述したように、独自の抽象化 (イベントバスのインターフェイス) を使用することが適しているのは、その抽象化によってサポートされる基本的なイベント バスの機能が必要な場合のみです。 より豊富なサービス バスの機能が必要な場合は、独自の抽象化ではなく、好みの商用サービス バスによって提供される API と抽象化を使うことをお勧めします。

### <a name="defining-an-event-bus-interface"></a>イベント バスのインターフェイスの定義

イベント バスのインターフェイスの実装コードの一部と、探索のための実装例から始めましょう。 以下のように、インターフェイスは汎用的で単純である必要があります。

```csharp
public interface IEventBus
{
    void Publish(IntegrationEvent @event);

    void Subscribe<T, TH>()
        where T : IntegrationEvent
        where TH : IIntegrationEventHandler<T>;

    void SubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void UnsubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void Unsubscribe<T, TH>()
        where TH : IIntegrationEventHandler<T>
        where T : IntegrationEvent;
}
```

`Publish` メソッドは単純です。 イベント バスは、渡される統合イベントを、そのイベントをサブスクライブするすべてのマイクロサービス、または外部アプリケーションに、ブロードキャストします。 このメソッドは、イベントを発行するマイクロサービスによって使われます。

`Subscribe` メソッド (引数に応じていくつかの実装を使えます) は、イベントを受信するマイクロサービスによって使われます。 このメソッドには、2 つの引数があります。 1 つ目は、サブスクライブする統合イベントです (`IntegrationEvent`)。 2 番目の引数は、受信側マイクロサービスが統合イベントのメッセージを取得すると実行される、統合イベントのハンドラー (またはコールバック メソッド) です (`IIntegrationEventHandler<T>`)。

## <a name="additional-resources"></a>その他のリソース

実稼働可能なメッセージング ソリューションの一部:

- **Azure Service Bus** \
  <https://docs.microsoft.com/azure/service-bus-messaging/>
  
- **NServiceBus** \
  <https://particular.net/nservicebus>
  
- **MassTransit** \
  <https://masstransit-project.com/>

> [!div class="step-by-step"]
> [前へ](database-server-container.md)
> [次へ](rabbitmq-event-bus-development-test-environment.md)
