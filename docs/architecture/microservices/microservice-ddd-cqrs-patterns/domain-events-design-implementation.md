---
title: 'ドメイン イベント: 設計と実装'
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | 集約間の通信を確立するための重要な概念であるドメイン イベントの詳細を表示する。
ms.date: 10/08/2018
ms.openlocfilehash: 3bba18d4a77b47abee55c16bae8a64ed27ac9aba
ms.sourcegitcommit: 68a4b28242da50e1d25aab597c632767713a6f81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884229"
---
# <a name="domain-events-design-and-implementation"></a>ドメイン イベント: 設計と実装

ドメイン内での変更の副作用を明示的に実装するには、ドメイン イベントを使います。 DDD の用語を使って言い換えるなら、複数の集約に副作用を明示的に実装するには、ドメイン イベントを使います。 また、スケーラビリティを向上させ、データベース ロックの影響を小さくする必要がある場合は、同じドメイン内の集約の間の最終的な整合性を使います。

## <a name="what-is-a-domain-event"></a>ドメイン イベントとは

イベントとは、過去に発生した出来事です。 ドメイン イベントはドメインで発生する出来事であり、それを同じドメイン (インプロセス) の他の部分に認識させます。 他の部分は通知を受けると、通常、イベントに何らかの方法で対処します。

ドメイン イベントの重要な利点は、副作用を明示的に表現できることです。

たとえば、Entity Framework を使用しているとき、大きな出来事に対して反応が必要であれば、おそらくそのイベントの引き金となるものの近くに必要なコードを記述することになります。 そのため、そのコードに暗黙的にルールが連結されます。コードを調べ、ルールがそこに実装されていることを確認する必要があります。

一方で、ドメイン イベントを使用すると、この概念が明示的になります。これは `DomainEvent` が存在するためであり、また、`DomainEventHandler` が少なくとも 1 つ関与するためです。

たとえば、eShopOnContainers アプリケーションで注文が作成されると、利用者は購入者になります。そのため、`OrderStartedDomainEvent` が発生し、`ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler` で処理されます。つまり、基礎をなす概念は明らかです。

手短に言えば、ドメインの専門家から与えられるユビキタス言語に基づいて、ドメイン ルールを明示的に表現する際にドメイン イベントが役立ちます。 また、ドメイン イベントを使用することにより、同じドメイン内のクラス間で問題をよりはっきりと分離できます。

データベース トランザクションと同様に、ドメイン イベントに関連付けられているすべての操作が正常に完了するか、1 つも完了しないようにすることが重要です。

ドメイン イベントはメッセージング スタイルのイベントに似ていますが、重要な相違点が 1 つあります。 実際のメッセージング、メッセージ キュー、メッセージ ブローカー、または AMQP を使うサービス バスでは、メッセージは常に非同期に送信され、プロセスとマシンの間で通信されます。 これは、複数の境界コンテキスト、マイクロサービス、または異なるアプリケーションを統合する場合に役立ちます。 一方、ドメイン イベントでは、現在実行しているドメイン操作からイベントが生成されて、同じドメイン内で副作用が発生することが望まれます。

ドメイン イベントとその副作用 (その後にトリガーされる、イベント ハンドラーによって管理されるアクション) は、ほぼ瞬時に (通常はインプロセス) 同じドメイン内で発生する必要があります。 したがって、ドメイン イベントは同期でも非同期でもかまいません。 ただし、統合イベントは常に非同期でなければなりません。

## <a name="domain-events-versus-integration-events"></a>統合イベントとドメイン イベント

意味としては、ドメイン イベントと統合イベントは同じものであり、発生したばかりのことに関する通知です。 しかし、それらの実装は異なっている必要があります。 ドメイン イベントは、IoC コンテナーまたは他の何らかの方法に基づくインメモリ メディエーターとして実装されたドメイン イベント ディスパッチャーにプッシュされるだけのメッセージです。

一方、統合イベントの目的は、コミットされたトランザクションや更新を他のサブシステムに伝達することです。サブシステムは、他のマイクロサービス、境界コンテキスト、さらには外部アプリケーションのいずれでもかまいません。 したがって、エンティティが正常に保存された場合にのみ発生する必要があります。正常に保存されない場合、操作全体がもともと行われなかったかのようになります。

前述のように、統合イベントは、複数のマイクロサービス (他の境界コンテキスト) 間または外部システム/アプリケーション間の非同期通信に基づいている必要があります。

したがって、イベント バス インターフェイスには、場合によってはリモート サービス間でのプロセス間分散通信を可能にする何らかのインフラストラクチャが必要です。 商用のサービス バス、キュー、メールボックスとして使われる共有データベース、またはその他の分散で、できればプッシュ ベースのメッセージング システムのいずれが基になっていてもかまいません。

## <a name="domain-events-as-a-preferred-way-to-trigger-side-effects-across-multiple-aggregates-within-the-same-domain"></a>同じドメイン内の複数の集約間で副作用をトリガーする望ましい方法としてのドメイン イベント

ある集約インスタンスに関係のあるコマンドを実行するために、1 つ以上の他の集約で他のドメイン ルールを実行する必要がある場合は、このような副作用がドメイン イベントによってトリガーされるように設計および実装する必要があります。 図 7-14 で示すように、最も重要なユース ケースの 1 つとして、ドメイン イベントを使って、同じドメイン モデル内の複数の集約の間で状態の変化を伝達する必要があります。

![Buyer の集約へのデータを制御するドメイン イベントを示す図。](./media/domain-events-design-implementation/domain-model-ordering-microservice.png)

**図 7-14**。 同じドメイン内の複数の集約の間に整合性を適用するためのドメイン イベント

図 7-14 は、集約の間の整合性がドメイン イベントによってどのように実現されるかを示しています。 ユーザーが注文を開始すると、注文の集約によって `OrderStarted` ドメイン イベントが送信されます。 OrderStarted ドメイン イベントは Buyer の集約によって処理され、ID マイクロサービスからの元のユーザー情報に基づいて (CreateOrder コマンドで指定した情報を使用して) 注文マイクロサービスに Buyer オブジェクトが作成されます。

または、集約ルートを作成し、その集約 (子エンティティ) のメンバーによって生成されるイベントをサブスクライブすることもできます。 たとえば、OrderItem の各子エンティティは、品目の価格が指定された金額より高いとき、または製品品目の量が高すぎるとに、イベントを発生させることができます。 集約ルートは、これらのイベントを受け取って、グローバルな計算または集約を実行できます。

このイベント ベースの通信は集約内に直接実装されないことを理解しておくことが重要です。ドメイン イベント ハンドラーを実装する必要があります。

ドメイン イベントの処理は、アプリケーションの問題です。 ドメイン モデル レイヤーでは、ハンドラーやリポジトリを使った副作用の永続化アクションといったアプリケーション インフラストラクチャに関することではなく、ドメインの専門家が理解しているドメインのロジックだけに注目する必要があります。 したがって、ドメイン イベントが発生したときのドメイン イベント ハンドラー トリガー アクションは、アプリケーション レイヤー レベルで行う必要があります。

また、ドメイン イベントを使って、任意の数のアプリケーション アクションをトリガーすることができます。さらに重要なことは、後でその数を他に影響を与えない方法で増やすことができる必要があります。 たとえば、注文が開始されたら、ドメイン イベントを発行してその情報を他の集約に伝達したり、通知などのアプリケーション アクションを発生させたりすることができます。

重要な点は、ドメイン イベントが発生したときに実行されるアクションの数が開放されていることです。 いずれ、ドメインおよびアプリケーション内のアクションとルールは増えていきます。 何かが発生したときの副作用の複雑さや数は上昇しますが、コードが "接着剤" で結合されていると (つまり、`new` で特定のオブジェクトを作成します)、新しいアクションを追加する必要があるたびに、テスト済みで動作するコードを変更する必要があります。

この変更の結果、新しいバグが発生する可能性があります。また、この手法は [SOLID](https://en.wikipedia.org/wiki/SOLID) の[開放/閉鎖 (Open/closed) の原則](https://en.wikipedia.org/wiki/Open/closed_principle)に違反します。 それだけでなく、操作を調整していた元のクラスが拡大を重ね、[単一責任原則 (SRP)](https://en.wikipedia.org/wiki/Single_responsibility_principle) に反するようになります。

これに対し、ドメイン イベントでは、以下のアプローチを使って責任を隔離することにより、粒度が細かく分離された実装を作成することができます。

1. コマンドを送信します (例: CreateOrder)。
2. コマンド ハンドラーでコマンドを受信します。
   - 1 つの集約のトランザクションを実行します。
   - (省略可能) 副作用のドメイン イベントを発生させます (例: OrderStartedDomainEvent)。
3. 複数の集約またはアプリケーション アクションにおいて開放された数の副作用を実行する (現在のプロセス内の) ドメイン ベントを処理します。 例:
   - 購入者および支払方法を確認または作成します。
   - 関連する統合イベントを作成してイベント バスに送信し、複数のマイクロサービスに状態を伝達するか、購入者へのメール送信のような外部アクションをトリガーします。
   - 他の副作用を処理します。

図 7-15 で示すように、同じドメイン イベントから開始して、統合イベントおよびイベント バスによって接続されている複数のマイクロサービスを実行するために必要な、ドメイン内の他の集約に関連する複数のアクションまたは他のアプリケーション アクションを処理できます。

![複数のイベント ハンドラーにデータを渡すドメイン イベントを示す図。](./media/domain-events-design-implementation/aggregate-domain-event-handlers.png)

**図 7-15**。 ドメインごとに複数のアクションの処理

アプリケーション レイヤーでは、同じイベントに対してハンドラーが複数存在することがあります。あるハンドラーで集約間の整合性を解決し、別のハンドラーで統合イベントを公開し、他のマイクロサービスがその公開されたイベントで何かの処理を行うといったことがありえます。 マイクロサービスの動作にはリポジトリやアプリケーション API などのインフラストラクチャ オブジェクトを使うため、通常、イベント ハンドラーはアプリケーション レイヤーにあります。 どちらもアプリケーション レイヤーの一部であるという意味で、イベント ハンドラーはコマンド ハンドラーに似ています。 重要な相違点は、コマンドは 1 回だけ処理する必要があることです。 ドメイン イベントは、それぞれが異なる用途を持つ複数のレシーバーまたはイベント ハンドラーで受信できるため、ドメイン イベントはゼロ回または *n* 回処理される可能性があります。

ドメイン イベントあたりのハンドラーの数には制限がないため、現在のコードに影響を与えずに必要な数だけドメイン ルールを追加できます。 たとえば、次のビジネス ルールは、いくつかのイベント ハンドラー (またはたった 1 つ) を追加するだけで簡単に実装できます。

> 顧客がストアで行った任意の数の注文の合計購入金額が $6,000 を超える場合、すべての新規注文に 10% の割引を適用し、将来の注文に対するその割引のことをメールで顧客に通知します。

## <a name="implement-domain-events"></a>ドメイン イベントの実装

C# のドメイン イベントは、ドメインで発生したことに関連するすべての情報を含む、DTO のようなデータ保持構造体またはクラスです。次にその例を示します。

```csharp
public class OrderStartedDomainEvent : INotification
{
    public string UserId { get; }
    public int CardTypeId { get; }
    public string CardNumber { get; }
    public string CardSecurityNumber { get; }
    public string CardHolderName { get; }
    public DateTime CardExpiration { get; }
    public Order Order { get; }

    public OrderStartedDomainEvent(Order order,
                                   int cardTypeId, string cardNumber,
                                   string cardSecurityNumber, string cardHolderName,
                                   DateTime cardExpiration)
    {
        Order = order;
        CardTypeId = cardTypeId;
        CardNumber = cardNumber;
        CardSecurityNumber = cardSecurityNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
    }
}
```

これは、本質的に OrderStarted イベントに関連するすべてのデータを保持するクラスです。

ユビキタス言語の観点からは、イベントは過去に発生した事柄なので、イベントのクラス名は OrderStartedDomainEvent や OrderShippedDomainEvent のような過去形動詞として表される必要があります。 これは、eShopOnContainers の注文マイクロサービスでのドメイン イベントの実装方法です。

前に説明したように、イベントの重要な特性は、イベントは過去に発生したことなので変更できない、ということです。 したがって、不変クラスである必要があります。 上のコードでは、プロパティが読み取り専用であることがわかります。 オブジェクトを更新する方法はありません。作成するときに値を設定することのみ可能です。

ここで強調しておくことが重要ですが、イベント オブジェクトのシリアル化と逆シリアル化を必要としたキューを使用し、ドメイン イベントを非同期で処理するなら、おそらく、キュー解除時、デシリアライザーで値を割り当てられるよう、プロパティを読み取り専用ではなく "プライベート セット" にする必要があります。 ドメイン イベント パブ/サブは MediatR を利用して非同期で実装されるため、これは Ordering マイクロサービスの問題ではありません。

### <a name="raise-domain-events"></a>ドメイン イベントを生成する

次に知りたいことは、関連するイベント ハンドラーに届くようにドメイン イベントを生成する方法です。 複数の方法があります。

Udi Dahan がもともと提案しているのは (たとえば、「[Domain Events – Take 2](http://udidahan.com/2008/08/25/domain-events-take-2/)」(ドメイン イベント – テイク 2 などの複数の関連する投稿を参照))、イベントの管理と生成に静的クラスを使う方法です。 これには、`DomainEvents.Raise(Event myEvent)` のような構文を使用し、呼び出されるとすぐにドメイン イベントを生成する DomainEvents という名前の静的クラスが含まれる場合があります。 Jimmy Bogard も、ブログ投稿「[Strengthening your domain:Domain Events](https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/)」(ドメインの強化: ドメイン イベント) で同様のアプローチを推奨しています。

ただし、ドメイン イベント クラスが静的である場合は、ハンドラーにもすぐにディスパッチにします。 これにより、副作用のロジックを含むイベント ハンドラーがイベント生成直後に実行されるため、テストとデバッグが難しくなります。 テストとデバッグを行うときは、現在の集約クラスで発生していることだけに注目する必要があります。他の集約やアプリケーション ロジックに関連する副作用に対する他のイベント ハンドラーに突然リダイレクトされるのは困ります。 このような理由から、次のセクションで説明する他のアプローチが考案されています。

#### <a name="the-deferred-approach-to-raise-and-dispatch-events"></a>イベントを生成し、ディスパッチする遅延アプローチ

ドメイン イベント ハンドラーにすぐにディスパッチするよりよい方法は、ドメイン イベントをコレクションに追加した後、トランザクションを (EF の SaveChanges で) コミットする "*直前*" または "*直* *後*" に、それらのドメイン イベントをディスパッチすることです (このアプローチについては、Jimmy Bogard の「[A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)」(よりよいドメイン イベント パターン) を参照)。

ドメイン イベントの送信をトランザクションのコミットの直前または直後のどちらにするかは、副作用を同じトランザクションまたは別のトランザクションのどちらに含めればよいかに影響するので、重要な決定です。 後者の場合は、複数の集約の間の最終的な整合性に対処する必要があります。 これについては、次のセクションで説明します。

eShopOnContainers では遅延アプローチを使っています。 最初に、エンティティで発生したイベントを、エンティティごとのコレクションまたはイベント リストに追加します。 このリストは、エンティティ オブジェクトの一部にする必要があり、さらによい方法は、次の Entity 基底クラスの例で示すように、基本エンティティ クラスの一部にすることです。

```csharp
public abstract class Entity
{
     //...
     private List<INotification> _domainEvents;
     public List<INotification> DomainEvents => _domainEvents;

     public void AddDomainEvent(INotification eventItem)
     {
         _domainEvents = _domainEvents ?? new List<INotification>();
         _domainEvents.Add(eventItem);
     }

     public void RemoveDomainEvent(INotification eventItem)
     {
         _domainEvents?.Remove(eventItem);
     }
     //... Additional code
}
```

イベントを生成するときは、集約ルート エンティティの任意のメソッドのコードから、イベント コレクションにイベントを追加するだけです。

次に示すコードの例は、[eShopOnContainers の Order 集約ルート](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs)の一部です。

```csharp
var orderStartedDomainEvent = new OrderStartedDomainEvent(this, //Order object
                                                          cardTypeId, cardNumber,
                                                          cardSecurityNumber,
                                                          cardHolderName,
                                                          cardExpiration);
this.AddDomainEvent(orderStartedDomainEvent);
```

AddDomainEvent メソッドで行われているのが、リストへのイベントの追加だけであることに注意してください。 イベントはまだディスパッチされておらず、イベント ハンドラーはまだ呼び出されていません。

実際にイベントをディスパッチするのは、後でデータベースにトランザクションをコミットするときです。 Entity Framework Core を使っている場合は、次のコードのように、EF DbContext の SaveChanges メソッド内であることを意味します。

```csharp
// EF Core DbContext
public class OrderingContext : DbContext, IUnitOfWork
{
    // ...
    public async Task<bool> SaveEntitiesAsync(CancellationToken cancellationToken = default(CancellationToken))
    {
        // Dispatch Domain Events collection.
        // Choices:
        // A) Right BEFORE committing data (EF SaveChanges) into the DB. This makes
        // a single transaction including side effects from the domain event
        // handlers that are using the same DbContext with Scope lifetime
        // B) Right AFTER committing data (EF SaveChanges) into the DB. This makes
        // multiple transactions. You will need to handle eventual consistency and
        // compensatory actions in case of failures.
        await _mediator.DispatchDomainEventsAsync(this);

        // After this line runs, all the changes (from the Command Handler and Domain
        // event handlers) performed through the DbContext will be committed
        var result = await base.SaveChangesAsync();
    }
}
```

このコードでは、エンティティ イベントを対応するイベント ハンドラーにディスパッチします。

全体な結果として、ドメイン イベントの生成 (単にメモリ内のリストへの追加) がイベント ハンドラーへのディスパッチから切り離されます。 さらに、使っているディスパッチャーの種類によっては、同期または非同期にイベントをディスパッチできます。

ここでは、トランザクション境界が重要な意味を持つことに注意してください。 作業単位とトランザクションが複数の集約にまたがることができる場合 (EF Core とリレーショナル データベースを使っている場合など)、これはうまくいきます。 しかしながら、トランザクションが複数の集約にまたがることができない場合は (Azure CosmosDB のような NoSQL を使っている場合)、整合性を実現するための追加手順を実装する必要があります。 これは、永続性の無視がユニバーサルではないもう 1 つの理由であり、使っているストレージ システムに依存します。

### <a name="single-transaction-across-aggregates-versus-eventual-consistency-across-aggregates"></a>複数の集約にまたがる 1 つのトランザクションと集約の間の最終的な整合性

複数の集約にまたがる 1 つのトランザクションを実行する方がよいか、それともそれらの集約の間の最終的な整合性に依存する方がよいかは、難しい問題です。 Eric Evans や Vaughn Vernon などの多くの DDD 作成者は、1 トランザクション = 1 集約のルールを推奨しており、したがって集約間の最終的な整合性を主張しています。 たとえば、Eric Evans は『*Domain-Driven Design*』(ドメイン ドリブンの設計) で次のように書いています。

> 複数の集約に関係するルールは、常に最新の状態であることを期待できません。 イベント処理、バッチ処理、または他の更新メカニズムにより、他の依存関係はある特定の時間内に解決できます。 (128 ページ)

Vaughn Vernon は、『[Effective Aggregate Design.Part II:Making Aggregates Work Together](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)』(効果的な集約設計パート II: 集約処理の連携) で次のように書いています。

> したがって、1 つの集約インスタンスでコマンドを実行するために、他のビジネス ルールを 1 つ以上の集約で実行する必要がある場合は、最終的な整合性を使います \[...\]。DDD モデルには最終的な整合性をサポートするための実用的な方法があります。 集約メソッドが発行したドメイン イベントは、1 つ以上の非同期サブスクライバーに時間内に配信されます。

この原理は、多数の集約またはエンティティにまたがるトランザクションではなく、粒度が細かいトランザクションの採用に基づいています。 考え方として、2 番目のケースでは、高いスケーラビリティが必要な大規模なアプリケーションではデータベース ロックの数がかなり多くなるということです。 拡張性の高いアプリケーションでは複数の集約間に即時のトランザクション整合性は必要ないという事実を考えると、最終的な整合性の概念を受け入れるのに役立ちます。 アトミックな変更はビジネスで必要ないことが多く、特定の操作にアトミックなトランザクションが必要かどうかを示すのは、常にドメイン専門家の責任です。 常に操作で複数の集約の間にアトミックなトランザクションが必要な場合は、集約をもっと大きくする必要があるかどうか、または集約が正しく設計されているか疑問に感じることがあります。

一方、Jimmy Bogard のような他の開発者や設計者は、単一のトランザクションが複数の集約にまたがってもかまわないと考えています。ただし、このような追加の集約が同じ元のコマンドの副作用に関係している場合だけです。 たとえば、Bogard は「[A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)」(よりよいドメイン イベント パターン) で次のように書いています。

> 通常はドメイン イベントの副作用は同じ論理トランザクション内で発生する必要がありますが、必ずしも同じドメイン イベント発生スコープ内である必要はありません \[...\]。トランザクションをコミットする直前に、対応するハンドラーにイベントをディスパッチします。

元のトランザクションをコミットする "*直前*" にドメイン イベントをディスパッチする場合は、これらのイベントの副作用を同じトランザクションに含まれるようにするためです。 たとえば、EF DbContext SaveChanges メソッドが失敗した場合、トランザクションは、関連するドメイン イベント ハンドラーによって実装されているすべての副作用操作の結果を含む、すべての変更をロールバックします。 これは、DbContext の有効期間の範囲が、既定で "スコープ" と定義されているためです。 したがって、DbContext オブジェクトは、同じスコープ内またはオブジェクト グラフ内でインスタンス化されている複数のリポジトリ オブジェクト間で共有されます。 これは、Web API または MVC アプリを開発するときの HttpRequest スコープと一致します。

実際には、どちらの方法 (単一のアトミック トランザクションと最終的な整合性) も適切な場合があります。 どちらがよいかは、ドメインまたはビジネスの要件と、ドメイン専門家の意見に依存します。 また、サービスに必要なスケーラビリティのレベルにも依存します (トランザクションの粒度が細かいほど、データベース ロックに関する影響は小さくなります)。 また、最終的な整合性では、集約間で可能性のある不整合を検出するためにより複雑なコードが必要であり、補正アクションを実装する必要があるため、コードにかけられる費用によっても左右されます。 元の集約に変更をコミットした後、イベントがディスパッチされるとき、問題があってイベント ハンドラーが副作用をコミットできない場合は、集約の間に不整合が発生することを考慮してください。

補正アクションを実装するには、ドメイン イベントを追加データベース テーブルに格納して、ドメイン インベントが元のトランザクションの一部になるようにします。 その後は、バッチ処理で、イベントのリストと集約の現在の状態を比較することによって不整合を検出し、補正アクションを実行することができます。 補正アクションは開発側からの詳細な分析が必要な複雑なトピックの一部であり、ビジネス ユーザーやドメイン専門家との検討が含まれます。

どのような場合でも、必要なアプローチを選ぶことができます。 ただし、EF Core とリレーショナル データベースを使うときは、初期遅延アプローチ (コミットの前にイベントを生成し、単一のトランザクションを使う) が最も簡単な方法です。 実装が簡単であり、多くのビジネス ケースで有効です。 eShopOnContainers の注文マイクロサービスでも、この方法が使われています。

しかし、これらのイベントを対応するイベント ハンドラーに実際にディスパッチするにはどうすればよいでしょう。 前の例で確認した `_mediator` オブジェクトは何ですか。 それはイベントとそのイベント ハンドラーの間をマップするために使用する手法および成果物に関係します。

### <a name="the-domain-event-dispatcher-mapping-from-events-to-event-handlers"></a>ドメイン イベント ディスパッチャー: イベントからイベント ハンドラーへのマッピング

イベントをディスパッチまたは発行できるようになったら、関連するすべてのハンドラーがイベントを入手し、イベントに基づいて副作用を処理できるように、イベントを発行する何らかの種類の成果物が必要です。

1 つの方法は、おそらくインメモリ イベントではなくサービス バスに基づく、実際のメッセージング システムまたはイベント バスです。 ただし、最初のケースでは、必要なのは同じプロセス内でこれらのイベントを処理することだけなので (つまり、同じドメイン内および同じアプリケーション レイヤー内)、実際のメッセージングを使うのは過剰です。

イベントを複数のイベント ハンドラーにマップするもう 1 つの方法は、イベントをディスパッチする場所を動的に推論できるように、IoC コンテナーへの型の登録を使うことです。 つまり、特定のイベント ハンドラーで必要な特定のイベントを知る必要があります。 図 7-16 はこのアプローチの概要です。

![適切なハンドラーにイベントを送信するドメイン イベント ディスパッチャーを示す図。](./media/domain-events-design-implementation/domain-event-dispatcher.png)

**図 7-16**。 IoC を使ったドメイン イベント ディスパッチャー

このアプローチを実装するためのすべての仕組みと成果物を自分で作成することができます。 ただし、[MediatR](https://github.com/jbogard/MediatR) のようなライブラリを使うこともできます。このライブラリは内部で IoC コンテナーを使っています。 したがって、定義済みのインターフェイスとメディエーター オブジェクトのパブリッシュ/ディスパッチ メソッドを直接使うことができます。

コードでは最初に、イベント ハンドラーの型を IoC コンテナーに登録する必要があります。次に示す [eShopOnContainers の Ordering マイクロサービス](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/MediatorModule.cs)での例をご覧ください。

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        // Other registrations ...
        // Register the DomainEventHandler classes (they implement IAsyncNotificationHandler<>)
        // in assembly holding the Domain Events
        builder.RegisterAssemblyTypes(typeof(ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler)
                                       .GetTypeInfo().Assembly)
                                         .AsClosedTypesOf(typeof(IAsyncNotificationHandler<>));
        // Other registrations ...
    }
}
```

このコードは最初に、いずれかのハンドラーを保持するアセンブリを探すことによってドメイン イベント ハンドラーが含まれているアセンブリを特定します (typeof(ValidateOrAddBuyerAggregateWhenXxxx) を使いますが、他のイベント ハンドラーを選んでアセンブリを探してもかまいません)。 すべてのイベント ハンドラーは IAsyncNotificationHandler インターフェイスを実装しているので、次に、それらの型だけを検索してすべてのイベント ハンドラーを登録します。

### <a name="how-to-subscribe-to-domain-events"></a>ドメイン イベントをサブスクライブする方法

MediatR を使うときは、次のコードでわかるように、INotificationHandler インターフェイスのジェネリック パラメーターで提供されるイベントの型を、各イベント ハンドラーで使う必要があります。

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
  : IAsyncNotificationHandler<OrderStartedDomainEvent>
```

イベントとイベント ハンドラーの間の関係 (これはサブスクリプションと考えることができます) に基づいて、MediatR の成果物は各イベントのすべてのイベント ハンドラーを検出し、それらの各イベント ハンドラーをトリガーすることができます。

### <a name="how-to-handle-domain-events"></a>ドメイン イベントを処理する方法

最後になりますが、通常、イベント ハンドラーでは、インフラストラクチャのリポジトリを使って必要な他の集約を取得して副作用のドメイン ロジックを実行する、アプリケーション レイヤーのコードを実装します。 次に示すのは、[eShopOnContainers のドメイン イベント ハンドラーのコード](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/DomainEventHandlers/OrderStartedEvent/ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler.cs)での実装の例です。

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
                   : INotificationHandler<OrderStartedDomainEvent>
{
    private readonly ILoggerFactory _logger;
    private readonly IBuyerRepository<Buyer> _buyerRepository;
    private readonly IIdentityService _identityService;

    public ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler(
        ILoggerFactory logger,
        IBuyerRepository<Buyer> buyerRepository,
        IIdentityService identityService)
    {
        // ...Parameter validations...
    }

    public async Task Handle(OrderStartedDomainEvent orderStartedEvent)
    {
        var cardTypeId = (orderStartedEvent.CardTypeId != 0) ? orderStartedEvent.CardTypeId : 1;
        var userGuid = _identityService.GetUserIdentity();
        var buyer = await _buyerRepository.FindAsync(userGuid);
        bool buyerOriginallyExisted = (buyer == null) ? false : true;

        if (!buyerOriginallyExisted)
        {
            buyer = new Buyer(userGuid);
        }

        buyer.VerifyOrAddPaymentMethod(cardTypeId,
                                       $"Payment Method on {DateTime.UtcNow}",
                                       orderStartedEvent.CardNumber,
                                       orderStartedEvent.CardSecurityNumber,
                                       orderStartedEvent.CardHolderName,
                                       orderStartedEvent.CardExpiration,
                                       orderStartedEvent.Order.Id);

        var buyerUpdated = buyerOriginallyExisted ? _buyerRepository.Update(buyer)
                                                                      : _buyerRepository.Add(buyer);

        await _buyerRepository.UnitOfWork
                .SaveEntitiesAsync();

        // Logging code using buyerUpdated info, etc.
    }
}
```

このドメイン イベント ハンドラーのコードは、インフラストラクチャ永続レイヤーについて次のセクションで説明するように、インフラストラクチャのリポジトリを使っているので、アプリケーション レイヤーのコードと考えられます。 イベント ハンドラーは、他のインフラストラクチャ コンポーネントを使うこともできます。

#### <a name="domain-events-can-generate-integration-events-to-be-published-outside-of-the-microservice-boundaries"></a>ドメイン イベントは、マイクロサービス境界の外部で発行される統合イベントを生成できます。

最後に、重要なこととして、複数のマイクロサービスにイベントを伝達することが必要になる場合があります。 この伝達は統合イベントであり、任意の特定のドメイン イベント ハンドラーからイベント バスを介して発行できます。

## <a name="conclusions-on-domain-events"></a>ドメイン イベントのまとめ

説明したように、ドメイン内での変更の副作用を明示的に実装するには、ドメイン イベントを使います。 DDD の用語では、1 つ以上の集約に副作用を明示的に実装するには、ドメイン イベントを使います。 さらに、スケーラビリティを向上させ、データベース ロックの影響を小さくする必要がある場合は、同じドメイン内の集約の間の最終的な整合性を使います。

参照アプリは [MediatR](https://github.com/jbogard/MediatR) を使用して、単一のトランザクション内で、集計間のドメイン イベントを同期的に伝達します。 [RabbitMQ](https://www.rabbitmq.com/) のような AMQP 実装や [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) で、最終的な整合性を使用してドメイン イベントを非同期的に伝達することもできますが、前述のように、障害が発生した場合の補正アクションの必要性を考慮する必要があります。

## <a name="additional-resources"></a>その他の技術情報

- **Greg Young。ドメイン イベントとは** \
  <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf#page=25>

- **Jan Stenberg。ドメイン イベントと最終的な整合性** \
  <https://www.infoq.com/news/2015/09/domain-events-consistency>

- **Jimmy Bogard。よりよいドメイン イベント パターン** \
  <https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/>

- **Vaughn Vernon。効果的な集約設計パート II:集約処理の連携** \
  [https://dddcommunity.org/wp-content/uploads/files/pdf\_articles/Vernon\_2011\_2.pdf](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)

- **Jimmy Bogard。ドメインの強化:ドメイン イベント** \
  <https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/>

- **Tony Truong。ドメイン イベント パターンの例** \
  <https://www.tonytruong.net/domain-events-pattern-example/>

- **Udi Dahan。完全にカプセル化されたドメイン モデルを作成する方法** \
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

- **Udi Dahan。ドメイン イベント – テイク 2** \
  <http://udidahan.com/2008/08/25/domain-events-take-2/>

- **Udi Dahan。ドメイン イベント – 救済** \
  <http://udidahan.com/2009/06/14/domain-events-salvation/>

- **Jan Kronquist。ドメイン イベントを発行しないで返す** \
  <https://blog.jayway.com/2013/06/20/dont-publish-domain-events-return-them/>

- **Cesar de la Torre。DDD およびマイクロサービス アーキテクチャでのドメイン イベントとDDD の統合イベントとマイクロサービス アーキテクチャ** \
  <https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/>

>[!div class="step-by-step"]
>[前へ](client-side-validation.md)
>[次へ](infrastructure-persistence-layer-design.md)
