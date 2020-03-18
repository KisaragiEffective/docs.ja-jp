---
title: Web API を使用したマイクロサービス アプリケーション レイヤーの実装
description: Web API アプリケーション レイヤーでの依存関係の挿入、メディエーター パターン、およびそれらの実装の詳細について理解します。
ms.date: 01/30/2020
ms.openlocfilehash: a88f3bfd11ea06df085ca82ed7265cb37006fc31
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77502444"
---
# <a name="implement-the-microservice-application-layer-using-the-web-api"></a>Web API を使用してマイクロサービス アプリケーション レイヤーを実装する

## <a name="use-dependency-injection-to-inject-infrastructure-objects-into-your-application-layer"></a>依存関係挿入を使用し、アプリケーション レイヤーにインフラストラクチャ オブジェクトを挿入する

前のセクションで述べたように、アプリケーション レイヤーは、Web API プロジェクトや MVC Web アプリ プロジェクトなどで作成する成果物 (アセンブリ) の一部として実装できます。 ASP.NET Core を使用して作成されたマイクロサービスの場合、アプリケーション レイヤーは通常、Web API ライブラリになります。 ASP.NET Core から来るもの (そのインフラストラクチャとコントローラー) を、カスタム アプリケーション レイヤー コードと分離したい場合は、アプリケーション レイヤーを別のクラス ライブラリに配置することもできますが、これは任意です。

たとえば、注文マイクロサービスのアプリケーション レイヤー コードは、**Ordering.API** プロジェクト (ASP.NET Core Web API プロジェクト) の一部として直接実装されます (図 7-23 を参照)。

:::image type="complex" source="./media/microservice-application-layer-implementation-web-api/ordering-api-microservice.png" alt-text="ソリューション エクスプローラーでの Ordering.API マイクロサービスのスクリーンショット。":::
Ordering.API マイクロサービスのソリューション エクスプローラー ビュー。[Application] フォルダーの下に、Behaviors、Commands、DomainEventHandlers、IntegrationEvents、Models、Queries、Validations というサブフォルダーを確認できます。
:::image-end:::

**図 7-23**. Ordering.API ASP.NET Core Web API プロジェクト内のアプリケーション レイヤー

ASP.NET Core には、コンストラクター挿入を既定でサポートするシンプルな[組み込み IoC コンテナー](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) (IServiceProvider インターフェイスによって表されます) が含まれています。ASP.NET では、DI によって特定のサービスを利用可能にすることができます。 ASP.NET Core では、ユーザーが登録する型のうち、DI を通じて挿入されるものを*サービス*という用語で表します。 組み込みコンテナーのサービスは、アプリケーションの Startup クラス内にある ConfigureServices メソッドで構成します。 依存関係は、型に必要であり、IoC コンテナーに登録するサービス内に実装されます。

ユーザーは通常、インフラストラクチャ オブジェクトを実装する依存関係を挿入します。 挿入する依存関係として典型的なのは、リポジトリです。 ただし、インフラストラクチャの依存関係が他にもある場合は、それらを挿入することもできます。 単純な実装の場合は、作業単位パターン オブジェクト (EF DbContext オブジェクト) を直接実装することもできます。なぜなら、DBContext はインフラストラクチャ永続オブジェクトの実装でもあるからです。

次の例では、.NET Core において、必要なリポジトリ オブジェクトがコンストラクターを通じてどのように挿入されるかが示されています。 このクラスは、次のセクションで取り上げるコマンド ハンドラーです。

```csharp
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Create the Order AggregateRoot
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

このクラスは、挿入されたリポジトリを使用してトランザクションを実行し、状態の変更を保持します。 このクラスが コマンド ハンドラーなのか、ASP.NET Core Web API コントローラー メソッドなのか、それとも [DDD アプリケーション サービス](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)なのかは重要ではありません。 重要なのは、このクラスがリポジトリ、ドメイン エンティティ、およびその他のアプリケーション調整をコマンド ハンドラーのような方法で使用する、単純なクラスだということです。 依存関係挿入は、コンストラクター ベースの DI を使用したこの例のように、記述されたすべてのクラスに対して同様に動作します。

### <a name="register-the-dependency-implementation-types-and-interfaces-or-abstractions"></a>依存関係実装の型とインターフェイス (または抽象化) の登録

コンストラクターを通じて挿入されたオブジェクトを使用するには、まず、DI を通じてアプリケーション クラスに挿入されたオブジェクトの生成元となるインターフェイスやクラスを、どこに登録するのかを知る必要があります (先の例で示したコンストラクター ベースの DI と同様)。

#### <a name="use-the-built-in-ioc-container-provided-by-aspnet-core"></a>ASP.NET Core で提供される組み込み IoC コンテナーの使用

ASP.NET Core で提供される組み込み IoC コンテナーを使用する場合は、ConfigureServices メソッドに挿入する型を、次のようなコードによって Startup.cs ファイルに登録します。

```csharp
// Registration of types into ASP.NET Core built-in container
public void ConfigureServices(IServiceCollection services)
{
    // Register out-of-the-box framework services.
    services.AddDbContext<CatalogContext>(c =>
        c.UseSqlServer(Configuration["ConnectionString"]),
        ServiceLifetime.Scoped);

    services.AddMvc();
    // Register custom application dependencies.
    services.AddScoped<IMyCustomRepository, MyCustomSQLRepository>();
}
```

IoC コンテナーに型を登録する場合の最も一般的なパターンは、型のペア (インターフェイスとその関連実装クラス) を登録するやり方です。 その後、任意のコンストラクターを通じて IoC コンテナーからオブジェクトを要求する際に、特定のインターフェイス型のオブジェクトを要求します。 たとえば、先の例の最後の行では、いずれかのコンストラクターに IMyCustomRepository (インターフェイスまたは抽象型) への依存関係がある場合に、IoC コンテナーが MyCustomSQLServerRepository 実装クラスのインスタンスを挿入するよう記述されています。

#### <a name="use-the-scrutor-library-for-automatic-types-registration"></a>自動登録用の Scrutor ライブラリの使用

.NET Core で DI を使用する場合は、アセンブリをスキャンし、その型を規則によって自動的に登録したい場合もあるでしょう。 この機能は、現在 ASP.NET Coreでは使用できません。 ただし、この目的のために [Scrutor](https://github.com/khellang/Scrutor) ライブラリを使用することはできます。 この方法は、IoC コンテナーに登録する必要がある型がたくさんある場合に便利です。

#### <a name="additional-resources"></a>その他の技術情報

- **Matthew King。Scrutor でのサービスの登録** \
  <https://www.mking.net/blog/registering-services-with-scrutor>

- **Kristian Hellang。Scrutor。** GitHub リポジトリ。 \
  <https://github.com/khellang/Scrutor>

#### <a name="use-autofac-as-an-ioc-container"></a>Autofac を IoC コンテナーとして使用する

eShopOnContainers の注文マイクロサービスのように、追加の IoC コンテナーを使用し、[Autofac](https://autofac.org/) を使用する ASP.NET Core パイプラインにそれらをプラグインすることもできます。 Autofac を使用する場合は通常、モジュールを通じて型を登録します。これにより、型がどこにあるかに応じて、複数のファイル間で登録型を分割することができます (複数のクラス ライブラリ間でアプリケーション型を分散できたのと同様)。

たとえば、次の例では、挿入する型を [Ordering.API Web API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) プロジェクト用の [Autofac アプリケーション モジュール](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs)に記述しています。

```csharp
public class ApplicationModule : Autofac.Module
{
    public string QueriesConnectionString { get; }
    public ApplicationModule(string qconstr)
    {
        QueriesConnectionString = qconstr;
    }

    protected override void Load(ContainerBuilder builder)
    {
        builder.Register(c => new OrderQueries(QueriesConnectionString))
            .As<IOrderQueries>()
            .InstancePerLifetimeScope();
        builder.RegisterType<BuyerRepository>()
            .As<IBuyerRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<OrderRepository>()
            .As<IOrderRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<RequestManager>()
            .As<IRequestManager>()
            .InstancePerLifetimeScope();
   }
}
```

Autofac には、[アセンブリをスキャンし、命名規則で型を登録する](https://autofac.readthedocs.io/en/latest/register/scanning.html)機能もあります。

登録プロセスと概念は、組み込みの ASP.NET Core IoC コンテナーに型を登録する方法とよく似ていますが、Autofac を使用する場合は構文が少し異なります。

このコード例では、IOrderRepository 抽象化が、実装クラス OrderRepository と共に登録されています。 つまり、コンストラクターが IOrderRepository 抽象化またはインターフェイスを通じて依存関係を宣言している場合、IoC コンテナーは OrderRepository クラスのインスタンスを挿入するということです。

同じサービスや依存関係に対する要求間でインスタンスがどのように共有されるかは、インスタンス スコープ型によって決まります。 依存関係に対する要求が行われた場合、IoC コンテナーは次のいずれかの結果を返します。

- 有効期間範囲ごとに 1 つのインスタンス (ASP.NET Core IoC コンテナー内で *scoped* として参照されます)。

- 依存関係ごとに 1 つの新規インスタンス (ASP.NET Core IoC コンテナー内で *transient* として参照されます)。

- IoC コンテナーを使用してすべてのオブジェクト間で共有される 1 つのインスタンス (ASP.NET Core IoC コンテナー内で *singleton* として参照されます)。

#### <a name="additional-resources"></a>その他の技術情報

- **ASP.NET Core での依存関係の挿入の概要** \
  [https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection](/aspnet/core/fundamentals/dependency-injection)

- **Autofac。** 公式ドキュメント。 \
  <https://docs.autofac.org/en/latest/>

- **ASP.NET Core IoC コンテナー サービスの有効期間と Autofac IoC コンテナー インスタンスの範囲の比較 - Cesar de la Torre** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="implement-the-command-and-command-handler-patterns"></a>コマンドおよびコマンド ハンドラー パターンの実装

前のセクションで示したコンストラクター ベースの DI の例では、IoC コンテナーはクラス内のコンストラクターを通じてリポジトリを挿入していました。 ですが、これらは厳密にはどこに挿入されたのでしょうか。 シンプルな Web API (たとえば、eShopOnContainers 内のカタログ マイクロサービス) では、ASP.NET Core の要求パイプラインの一部として、MVC コントローラー レベル (コントローラーのコンストラクター内) でこれらを挿入します。 しかし、このセクションの最初のコード (eShopOnContainers 内の Ordering.API サービスの [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) クラス) では、特定のコマンド ハンドラーのコンストラクターを通じて依存関係の挿入が行われています。 そこで、コマンド ハンドラーとは何か、またどのような理由で使用するのかについて説明していきましょう。

コマンド パターンは本来、このガイドの前の方で説明した、CQRS パターンに関連するものです。 CQRS には 2 つの面があります。 1 つ目の領域はクエリです。簡略化されたクエリを、[Dapper](https://github.com/StackExchange/dapper-dot-net) のマイクロ ORM と共に使用します。 2 つ目の領域はコマンドです。これは、トランザクションの開始点となるものであり、サービス外部からの入力チャネルとなるものです。

図 7-24 に示すように、パターンでは基本的に、クライアント側からコマンドを受け付け、それらをドメイン モデル ルールに基づいて処理し、最後にトランザクションの状態を保持します。

![クライアントからデータベースへの上位レベルのデータ フローを示す図。](./media/microservice-application-layer-implementation-web-api/high-level-writes-side.png)

**図 7-24**. コマンド (CQRS パターンのトランザクション側) の概要

図 7-24 では、UI アプリは API 経由で `CommandHandler` にコマンドを送信します。これは、ドメイン モデルとインフラストラクチャに依存し、データベースが更新されます。

### <a name="the-command-class"></a>コマンド クラス

コマンドは、システムの状態を変更するアクションを実行するための、システムへの要求です。 コマンドは命令型であり、1 回だけ処理されます。

コマンドは命令型なので、通常は命令的な動詞 ("create" や "update" など) で名前が付けられ、集約型 (CreateOrderCommand など) が含められます。 イベントと違って、コマンドは過去のファクトを指すものではありません。単なる要求なので、拒否される場合もあります。

コマンドは、ユーザーが要求を開始した結果として UI から発生することもありますし、プロセス マネージャーが操作実行のための集約を指定した場合には、プロセス マネージャーから発生することもあります。

コマンドの重要な特性は、単一のレシーバーによって 1 回だけ処理されなければならないという点です。 つまりコマンドとは、アプリケーション内で実行する、単一のアクションまたはトランザクションであるということです。 たとえば、同一の注文作成コマンドを 2 回以上処理することはできません。 これは、コマンドとイベントの重要な違いです。 イベントは、複数回処理される場合があります。なぜなら、さまざまなシステムやマイクロサービスがそのイベントに関連している可能性があるからです。

もう 1 つ重要なことは、コマンドがべき等ではない場合、そのコマンドは 1 回だけ処理されるという点です。 コマンドを複数回実行しても、コマンドの性質上、またはシステムでのコマンドの処理方法上、その結果が変わらない場合には、そのコマンドはべき等であるということになります。

ドメインのビジネス ルールやインバリアントに対して合理的である場合には、コマンドや更新プログラムをべき等にすることをお勧めします。 たとえば、同じ例で言うと、何らかの理由 (再試行ロジックやハッキングなど) によって、同じ CreateOrder コマンドがシステムに複数回アクセスする場合には、そのコマンドを識別し、複数の注文が作成されないようにする必要があります。 そのためには、操作に何らかの ID をアタッチして、コマンドや更新プログラムが既に処理されていないかどうかを識別する必要があります。

コマンドは単一のレシーバーに送信します。つまり、コマンドは発行するものではありません。 発行は、何かが起こり、それがイベント レシーバーに関連する可能性がある場合に、そのファクトを記述したイベントに対して行うものです。 イベントの場合、パブリッシャーはイベントをどのレシーバーが取得するかや、それに対してレシーバーがどう対応するかについて、一切関知しません。 ただし、ドメインまたは統合イベントは前のセクションで既に説明した別のトピックです。

コマンドは、データ フィールドやコレクションのほか、コマンドを実行するために必要なすべての情報を含んだクラスを使用して実装されます。 コマンドは特別な種類のデータ転送オブジェクト (DTO) であり、変更やトランザクションを要求するために使用されるものです。 コマンド自体は、コマンドを処理するために必要な情報のみをベースとし、その他の情報は含まれません。

簡略化された `CreateOrderCommand` クラスの例を次に示します。 これは、eShopOnContainers 内の注文マイクロサービスで使用される不変コマンドです。

```csharp
// DDD and CQRS patterns comment
// Note that we recommend that you implement immutable commands
// In this case, immutability is achieved by having all the setters as private
// plus being able to update the data just once, when creating the object
// through the constructor.
// References on immutable commands:
// http://cqrs.nu/Faq
// https://docs.spine3.org/motivation/immutability.html
// http://blog.gauffin.org/2012/06/griffin-container-introducing-command-support/
// https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties
[DataContract]
public class CreateOrderCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    private readonly List<OrderItemDTO> _orderItems;
    [DataMember]
    public string City { get; private set; }
    [DataMember]
    public string Street { get; private set; }
    [DataMember]
    public string State { get; private set; }
    [DataMember]
    public string Country { get; private set; }
    [DataMember]
    public string ZipCode { get; private set; }
    [DataMember]
    public string CardNumber { get; private set; }
    [DataMember]
    public string CardHolderName { get; private set; }
    [DataMember]
    public DateTime CardExpiration { get; private set; }
    [DataMember]
    public string CardSecurityNumber { get; private set; }
    [DataMember]
    public int CardTypeId { get; private set; }
    [DataMember]
    public IEnumerable<OrderItemDTO> OrderItems => _orderItems;

    public CreateOrderCommand()
    {
        _orderItems = new List<OrderItemDTO>();
    }

    public CreateOrderCommand(List<BasketItem> basketItems, string city,
        string street,
        string state, string country, string zipcode,
        string cardNumber, string cardHolderName, DateTime cardExpiration,
        string cardSecurityNumber, int cardTypeId) : this()
    {
        _orderItems = MapToOrderItems(basketItems);
        City = city;
        Street = street;
        State = state;
        Country = country;
        ZipCode = zipcode;
        CardNumber = cardNumber;
        CardHolderName = cardHolderName;
        CardSecurityNumber = cardSecurityNumber;
        CardTypeId = cardTypeId;
        CardExpiration = cardExpiration;
    }

    public class OrderItemDTO
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; }
        public decimal UnitPrice { get; set; }
        public decimal Discount { get; set; }
        public int Units { get; set; }
        public string PictureUrl { get; set; }
    }
}
```

コマンド クラスには基本的に、ドメイン モデル オブジェクトを使用してビジネス トランザクションを実行するために必要な、すべてのデータが含まれています。 つまり、コマンドは読み取り専用データを含んだデータ構造であり、ビヘイビアーではありません。 コマンドの名前は、その目的を示しています。 C# を始めとする多くの言語では、コマンドはクラスとして表されますが、オブジェクト指向における真の意味では、クラスではありません。

もう 1 つの特徴として、コマンドは不変です。なぜなら、コマンドはドメイン モデルによって直接処理されるものと想定されているからです。 予定された有効期間中に変更する必要がないのです。 C# クラスでは、内部状態を変更するセッターやその他のメソッドを使用しないことによって、不変性を達成できます。

シリアル化/逆シリアル化プロセスを実行するコマンドを指定するか想定する場合は、プロパティにプライベート セッターと `[DataMember]` (または `[JsonProperty]`) 属性が必要であることに注意してください。 それ以外の場合、デシリアライザーで、ターゲット先で必要な値を使用してオブジェクトを再構築することはできなくなります。 また、クラスにすべてのプロパティのパラメーターを持つコンストラクターがある場合は、通常のキャメルケース名前付け規則を使用して、コンストラクターに `[JsonConstructor]` として注釈を付けることができます。 ただし、このオプションにはさらに多くのコードが必要です。

たとえば、注文を作成するためのコマンド クラスは通常、データの観点から見れば、作成する注文と同様のものになりますが、通常、同じ属性は必要ありません。 たとえば、注文はまだ作成されていないため、`CreateOrderCommand` に注文 ID はありません。

多くのコマンド クラスは、変更が必要な状態に関するいくつかのフィールドだけを含めればよいので、簡素化することができます。 たとえば、注文の状態を「処理中」から「支払済み」(または「発送済み」) 変更するだけの場合は、次のようなシンプルなコマンドで記述できます。

```csharp
[DataContract]
public class UpdateOrderStatusCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    public string Status { get; private set; }

    [DataMember]
    public string OrderId { get; private set; }

    [DataMember]
    public string BuyerIdentityGuid { get; private set; }
}
```

開発者によっては、UI 要求オブジェクトをコマンド DTO と分離させることもありますが、これは各自の好みで行うものにすぎません。 このような分離は、手間がかかる割にメリットがそれほど大きくはなく、オブジェクトはほぼ同様の形状になります。 たとえば、eShopOnContainers では一部のコマンドがクライアント側から直接来るようになっています。

### <a name="the-command-handler-class"></a>コマンド ハンドラー クラス

各コマンドについては、特定のコマンド ハンドラー クラスを実装する必要があります。 これによってパターンが機能するようになり、そこでコマンド オブジェクト、ドメイン オブジェクト、およびインフラストラクチャ リポジトリ オブジェクトが使用されます。 コマンド ハンドラーは、CQRS と DDD の観点から言えば、アプリケーション レイヤーの中心となるものです。 ただし、ドメイン ロジックはすべてドメイン クラス内に含める必要があります。つまり、アプリケーション レイヤーのクラスであるコマンド ハンドラー内ではなく、集約ルート (ルート エンティティ)、子エンティティ、または[ドメイン サービス](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)内に含める必要があります。

コマンド ハンドラー クラスは、前のセクションで説明した単一責任の原則 (SRP) を達成する堅固な手法となります。

コマンド ハンドラーはコマンドを受け取り、使用される集約の結果を取得します。 結果は、コマンドが正常に実行されるか、例外が返されるかのいずれかになります。 例外が返された場合、システム状態は変更されません。

コマンド ハンドラーでは通常、次の手順が実行されます。

- ([メディエーター](https://en.wikipedia.org/wiki/Mediator_pattern)またはその他のインフラストラクチャ オブジェクトから) コマンド オブジェクト (DTO など) を受け取ります 。

- コマンドが有効であるかどうかを検証します (メディエーターによって検証されなかった場合)。

- 現在のコマンドの対象となっている集約ルート インスタンスをインスタンス化します。

- 集約ルート インスタンスに対してコマンドを実行し、コマンドから必要なデータを取得します。

- 集約の新しい状態を、関連するデータベースに保持します。 この最後の操作が、実際のトランザクションとなります。

通常、コマンド ハンドラーは、その集約ルート (ルート エンティティ) によって駆動される単一の集約を操作します。 単一コマンドの受信によって複数の集約が影響を受ける場合は、ドメイン イベントを使用して複数の集約に状態やアクションを伝達することができます。

ここで重要なポイントは、コマンドの処理中、すべてのドメイン ロジックはドメイン モデル (集約) の内部にあり、単体テスト用に完全にカプセル化されているという点です。 コマンド ハンドラーは、データベースからドメイン モデルを取得する手段として機能し、最後にモデルが変更されたら、その変更を保持するようにインフラストラクチャ レイヤー (リポジトリ) に通知します。 このアプローチの利点は、プラミング レベル (コマンド ハンドラー、Web API、リポジトリなど) に当たるアプリケーション レイヤーやインフラストラクチャ レイヤー内のコードを変更することなく、完全にカプセル化された分離型の高機能なビヘイビアー ドメイン モデル内に、ドメイン ロジックをリファクタリングできるということです。

コマンド ハンドラーが複雑になりすぎる (ロジックが多すぎる) 場合には、コード スメルになる可能性があります。 内容を見直し、ドメイン ロジックがある場合はコードをリファクタリングして、そのドメイン ビヘイビアーをドメイン オブジェクト (集約ルートと子エンティティ) のメソッドに移動してください。

次のコードは、この章の最初に示したものと同じ `CreateOrderCommandHandler` クラスを使用して、コマンド ハンドラー クラスの例を示したものです。 ここでは、Handle メソッドと、ドメイン モデル オブジェクト/集約を使用した操作に注目してください。

```csharp
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Create the Order AggregateRoot
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

次に示すのは、コマンド ハンドラーで実行される追加の手順です。

- コマンドのデータを使用して、集約ルートのメソッドとビヘイビアーを操作します。

- トランザクションの実行中、ドメイン オブジェクト内で内部的にイベントを発生させます。ただしこれは、コマンド ハンドラーの視点から透過的です。

- 集約の操作結果が成功である場合は、トランザクションの完了後に、統合イベントを生成します。 (これらは、リポジトリなどのインフラストラクチャ クラスによって生成される場合もあります)。

#### <a name="additional-resources"></a>その他の技術情報

- **Mark Seemann。境界においては、アプリケーションはオブジェクト指向ではない** \
  <https://blog.ploeh.dk/2011/05/31/AttheBoundaries,ApplicationsareNotObject-Oriented/>

- **コマンドとイベント** \
  <https://cqrs.nu/Faq/commands-and-events>

- **コマンド ハンドラーの機能** \
  <https://cqrs.nu/Faq/command-handlers>

- **Jimmy Bogard。ドメイン コマンド パターン – ハンドラー** \
  <https://jimmybogard.com/domain-command-patterns-handlers/>

- **Jimmy Bogard。ドメイン コマンド パターン – 検証** \
  <https://jimmybogard.com/domain-command-patterns-validation/>

## <a name="the-command-process-pipeline-how-to-trigger-a-command-handler"></a>コマンド プロセス パイプライン: コマンド ハンドラーをトリガーする方法

次に知るべきことは、コマンド ハンドラーを呼び出す方法です。 コマンド ハンドラーは、関連するそれぞれの ASP.NET Core コントローラーから手動で呼び出すこともできます。 ただし、この方法は結合性が高すぎるため、理想的ではありません。

これ以外に次の 2 つの方法があり、これらをお勧めします。

- インメモリのメディエーター パターン成果物を通じて呼び出す。

- コントローラーとハンドラーの間で非同期メッセージ キューを使用して呼び出す。

### <a name="use-the-mediator-pattern-in-memory-in-the-command-pipeline"></a>コマンド パイプラインでメディエーター パターン (インメモリ) を使用する

図 7-25 に示すように、CQRS アプローチではインテリジェント メディエーターを使用します。これはインメモリ バスに似たもので、受信されるコマンドや DTO の種類に基づいて、適切なコマンド ハンドラーへと処理をスマートにリダイレクトすることができます。 コンポーネント間の黒い矢印は、オブジェクト間 (多くの場合、DI を通じて挿入されます) の依存関係と、関連する相互作用を表しています。

![クライアントからデータベースへの詳細なデータ フローを示す図。](./media/microservice-application-layer-implementation-web-api/mediator-cqrs-microservice.png)

**図 7-25**. 単一の CQRS マイクロサービス内のプロセスにおけるメディエーター パターンの使用

上の図は、図 7-24 を拡大したものを示しています。ASP.NET Core コントローラーで MediatR のコマンド パイプラインにコマンドが送信されるため、適切なハンドラーに到達します。

メディエーター パターンの使用がなぜ合理的かというと、エンタープライズ アプリケーションでは、処理要求が複雑になる場合があるからです。 そのため、ログ記録、検証、監査、セキュリティなどの横断的関心事を、多数追加できるようにすることが重要です。 メディエーター パイプライン (「[Mediator pattern (メディエーター パターン)](https://en.wikipedia.org/wiki/Mediator_pattern)」を参照してください) を使用すれば、それらの追加ビヘイビアーや横断的関心事を追加できるようになります。

メディエーターは、このプロセスの「実行方法」をカプセル化するオブジェクトです。メディエーターでは、状態、コマンド ハンドラーの呼び出し方法、またはハンドラーに提供するペイロードに基づいて、プロセスの実行を調整できます。 メディエーター コンポーネントを使用すると、デコレーター (または [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) 以降の[パイプライン ビヘイビアー](https://github.com/jbogard/MediatR/wiki/Behaviors)) を適用することで、横断的関心事を一元的かつ透過的に適用することができます。 詳細については、「[Decorator pattern (デコレーター パターン)](https://en.wikipedia.org/wiki/Decorator_pattern)」を参照してください。

デコレーターとビヘイビアーは、[アスペクト指向プログラミング (AOP)](https://en.wikipedia.org/wiki/Aspect-oriented_programming) に似ていて、メディエーター コンポーネントによって管理される特定のプロセス パイプラインのみに適用されます。 横断的関心事を実装する AOP 内のアスペクトは、コンパイル時に挿入された*アスペクト ウィーバー*か、オブジェクト呼び出しインターセプションに基づいて適用されます。 これらの典型的な AOP アプローチはどちらも、"魔法のように" 動作すると言われます。なぜなら、AOP の動作のしくみを見ることは容易ではないからです。 深刻な問題やバグを扱う場合、AOP はデバッグが困難になることがあります。 一方、これらのデコレーター/ビヘイビアーは明示的であり、メディエーターのコンテキスト内でのみ適用されるため、デバッグの予測可能性がはるかに高く、デバッグが簡単になります。

例として、eShopOnContainers の注文マイクロサービスでは、2 つのサンプル ビヘイビアーを実装しています ([LogBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) クラスと [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) クラス)。 ビヘイビアーの実装については、次のセクションで説明しています。具体的には、eShopOnContainers で [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) の[ビヘイビアー](https://github.com/jbogard/MediatR/wiki/Behaviors)がどのように使用されるかを示しています。

### <a name="use-message-queues-out-of-proc-in-the-commands-pipeline"></a>コマンドのパイプラインでメッセージ キュー (プロセス外) を使用する

もう 1 つの方法は、図 7-26 に示すように、ブローカーやメッセージ キューに基づいて非同期メッセージを使用する方法です。 この方法は、コマンド ハンドラーの直前のメディエーター コンポーネントと組み合わせることもできます。

![HA メッセージ キューを使用したデータフローを示す図。](./media/microservice-application-layer-implementation-web-api/add-ha-message-queue.png)

**図 7-26**. CQRS コマンドでのメッセージ キュー (プロセス外およびプロセス間通信) の使用

コマンドのパイプラインは高可用性のメッセージ キューで処理し、適切なハンドラーにコマンドを配信することもできます。 メッセージ キューを使用してコマンドを受信すると、コマンドのパイプラインがさらに複雑になる恐れがあります。多くの場合、外部メッセージ キューを通じて接続された 2 つのプロセスにパイプラインを分割する必要が生じるからです。 ただし、非同期メッセージングに基づいてスケーラビリティやパフォーマンスを改善する必要がある場合には、この方法を使用することになります。 図 7-26 の場合、コントローラーはコマンド メッセージをキューにポストし、そのまま戻ります。 その後、コマンド ハンドラーは自分のペースでメッセージを処理します。 これは、キューの非常に大きな利点です。高度なスケーラビリティが必要な場合、たとえば、在庫などの大量のイングレス データを扱う場合には、メッセージ キューがバッファーの役割を果たすことができるのです。

ただし、非同期であるというメッセージ キューの性質上、コマンドの処理が成功したかどうかについて、クライアント アプリケーションとどうやって通信するかを検討する必要があります。 原則として、「ファイア アンド フォーゲット」コマンドを使用することはできません。 どのようなビジネス アプリケーションでも、コマンドが正常に処理されたかどうかを確認する必要がありますし、少なくとも、検証を経て受け入れられたかどうかを確認する必要があります。

しかし、非同期キューに送信されたコマンド メッセージの検証後にクライアントに応答できるようにすると、トランザクションの実行後に操作の結果を返すインプロセス コマンド プロセスに比べて、システムが複雑になります。 キューを使用する場合は、コマンド プロセスの結果を、他の操作結果メッセージを通じて返す必要があるため、システムに追加のコンポーネントやカスタム通信が必要になります。

また、非同期コマンドは一方向のコマンドであり、多くの場合は不要になる可能性があります。これについては、Burtsev Alexey 氏と Greg Young 氏が[オンライン会話](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ)で交わした興味深いやりとりの中でも説明されています。

> \[Burtsev Alexey\] 私は、非同期コマンド処理や一方向のコマンド メッセージングが、特に理由もなく使用されているコードをよく見かけます (それらのコードでは、長い操作があるわけでもなく、外部の非同期コードを実行しているわけでもなく、アプリケーション境界をまたいでメッセージ バスを使用しているわけでもありません)。 彼らはなぜ、このようにコードを無駄に複雑化しているのでしょうか。 また、私はこれまでブロッキング コマンド ハンドラーを使用した CQRS コードを見たことがありませんが、この種のコードはほとんどの場合、問題なく機能します。
>
> \[Greg Young\] \[...\] 非同期コマンドは存在しません。これは実際には別のイベントです。 あなたが送った要求を私が受け付け、それに同意しない場合にはイベントを発生させる必要があるのであれば、あなたが私に何かを指示していることにはなりません。\[つまり、それはコマンド (指令) ではありません\]。 何かが実行されたことを、あなたが私に知らせているだけです。 これは一見、わずかな違いにも思えますが、大変深い意味があります。

非同期コマンドを使用する場合、エラーを示すための簡単な方法がないので、システムの複雑さが大幅に増します。 したがって非同期コマンドは、スケーラビリティ上の要件がある場合や、内部のマイクロサービスとメッセージを通じて通信する特別なケースを除き、推奨されません。 これらのケースに該当する場合は、エラー用に個別のレポート/回復システムを設計する必要があります。

eShopOnContainers の初期バージョンでは、HTTP 要求から開始され、メディエーター パターンによって駆動される、同期コマンド処理が採用されました。 これにより、プロセスが成功したかどうかを簡単に返すことが可能になっています ([CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) 実装を参照)。

いずれにせよ、これについては、アプリケーションやマイクロサービスのビジネス要件に基づいて意思決定を行う必要があります。

## <a name="implement-the-command-process-pipeline-with-a-mediator-pattern-mediatr"></a>メディエーター パターン (MediatR) を使用したコマンド プロセス パイプラインの実装

このガイドでは、サンプル実装として、メディエーター パターンに基づくインプロセス パイプラインを使用してコマンド挿入を駆動し、コマンドを (メモリ内で) 適切なコマンド ハンドラーにルーティングする方法を提案します。 またこのガイドでは、横断的関心事を分離するために[ビヘイビアー](https://github.com/jbogard/MediatR/wiki/Behaviors)を適用することを提案します。

.NET Core での実装にあたっては、メディエーターを実装するオープン ソース ライブラリが複数利用可能です。 このガイドで使用するライブラリは、 [MediatR](https://github.com/jbogard/MediatR) オープン ソース ライブラリ (作成者: Jimmy Bogard) ですが、別のアプローチを使用することもできます。 MediatR はコンパクトかつシンプルなライブラリで、デコレーターやビヘイビアーを適用しながら、コマンドなどのインメモリ メッセージを処理することができます。

メディエーター パターンを使用すると、結合性を低減し、要求された作業に関係する問題を分離しながら、その作業を実行するハンドラー (この場合はコマンド ハンドラー) に自動的に接続することができます。

メディエーター パターンを使用する理由については、このガイドのレビュー時に、Jimmy Bogard からもう 1 点、次のような理由が説明されました。

> ここでテストについても触れておいたほうがいいと思います。システムのビヘイビアーに、一貫性ある枠組みを持たせることができます。 リクエストイン、レスポンスアウト。このアスペクトは、作成したテストを一貫性を持って動作させるうえで、非常に価値があります。

まずは、メディエーター オブジェクトを実際に使用する、サンプル WebAPI コントローラーから見ていきましょう。 メディエーター オブジェクトを今まで使用していなかった場合は、そのコントローラーのすべての依存関係 (ロガー オブジェクトなど) を挿入する必要があります。 そのため、コンストラクターがかなり複雑になります。 一方、メディエーター オブジェクトを使用している場合は、コントローラーのコンストラクターがかなりシンプルになります。横断的操作ごとに 1 つの依存関係があれば、多数の依存関係を含めなくても、いくつかの依存関係だけで事足ります。次に例を示します。

```csharp
public class MyMicroserviceController : Controller
{
    public MyMicroserviceController(IMediator mediator,
                                    IMyMicroserviceQueries microserviceQueries)
    {
        // ...
    }
}
```

メディエーターによって、クリーンでコンパクトな Web API コントローラー コンストラクターが実現しています。 また、コントローラー メソッド内では、メディエーター オブジェクトにコマンドを送信するコードが、ほぼ 1 行で記述されています。

```csharp
[Route("new")]
[HttpPost]
public async Task<IActionResult> ExecuteBusinessOperation([FromBody]RunOpCommand
                                                               runOperationCommand)
{
    var commandResult = await _mediator.SendAsync(runOperationCommand);

    return commandResult ? (IActionResult)Ok() : (IActionResult)BadRequest();
}
```

### <a name="implement-idempotent-commands"></a>べき等コマンドの実装

**eShopOnContainers** では、上記の例よりもさらに高度なコードで、注文マイクロサービスから CreateOrderCommand オブジェクトが送信されています。 ただし、この注文ビジネス プロセスはやや複雑で、(このケースでは) バスケット マイクロサービスから開始されているため、CreateOrderCommand オブジェクトを送信するこのアクションは、先の例のようにクライアント アプリから呼び出されるシンプルな WebAPI コントローラーではなく、[UserCheckoutAcceptedIntegrationEventHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) という統合イベント ハンドラーから実行されます。

とは言え、MediatR にコマンドを送信するアクションは、次のコードに示すように、非常に似ています。

```csharp
var createOrderCommand = new CreateOrderCommand(eventMsg.Basket.Items,
                                                eventMsg.UserId, eventMsg.City,
                                                eventMsg.Street, eventMsg.State,
                                                eventMsg.Country, eventMsg.ZipCode,
                                                eventMsg.CardNumber,
                                                eventMsg.CardHolderName,
                                                eventMsg.CardExpiration,
                                                eventMsg.CardSecurityNumber,
                                                eventMsg.CardTypeId);

var requestCreateOrder = new IdentifiedCommand<CreateOrderCommand,bool>(createOrderCommand,
                                                                        eventMsg.RequestId);
result = await _mediator.Send(requestCreateOrder);
```

ただし、この例ではべき等コマンドも実装しているため、コードがもう少し高度なものになっています。 CreateOrderCommand プロセスはべき等なので、何らかの理由 (再試行など) により同じメッセージがネットワークから重複して送られた場合でも、同じビジネス オーダーは 1 回だけ処理されます。

これは、ビジネス コマンド (この場合は CreateOrderCommand) をラップし、それを汎用の IdentifiedCommand 内に埋め込むことで実装されています。IdentifiedCommand は、ネットワークを通じて送られるメッセージのうち、べき等である必要があるすべてのメッセージの ID によって追跡されます。

次のコードでは、IdentifiedCommand に、DTO と ID のほか、ラップされたビジネス コマンド オブジェクトしか含まれていません。

```csharp
public class IdentifiedCommand<T, R> : IRequest<R>
    where T : IRequest<R>
{
    public T Command { get; }
    public Guid Id { get; }
    public IdentifiedCommand(T command, Guid id)
    {
        Command = command;
        Id = id;
    }
}
```

次に、IdentifiedCommand の CommandHandler ([IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs)) が、メッセージの一部として受け取った ID をチェックし、それがテーブル内に既に存在していないかどうかを確認します。 既に存在する場合、そのコマンドは再度処理されることはなく、べき等コマンドとして動作します。 そのインフラストラクチャ コードは、下記の `_requestManager.ExistAsync` メソッド呼び出しによって実行されます。

```csharp
// IdentifiedCommandHandler.cs
public class IdentifiedCommandHandler<T, R> :
                                   IAsyncRequestHandler<IdentifiedCommand<T, R>, R>
                                   where T : IRequest<R>
{
    private readonly IMediator _mediator;
    private readonly IRequestManager _requestManager;

    public IdentifiedCommandHandler(IMediator mediator,
                                    IRequestManager requestManager)
    {
        _mediator = mediator;
        _requestManager = requestManager;
    }

    protected virtual R CreateResultForDuplicateRequest()
    {
        return default(R);
    }

    public async Task<R> Handle(IdentifiedCommand<T, R> message)
    {
        var alreadyExists = await _requestManager.ExistAsync(message.Id);
        if (alreadyExists)
        {
            return CreateResultForDuplicateRequest();
        }
        else
        {
            await _requestManager.CreateRequestForCommandAsync<T>(message.Id);

            // Send the embedded business command to mediator
            // so it runs its related CommandHandler
            var result = await _mediator.Send(message.Command);

            return result;
        }
    }
}
```

IdentifiedCommand はビジネス コマンドのエンベロープのように動作するので、ビジネス コマンドを処理する必要がある場合 (ID が重複していない場合) には、その内部ビジネス コマンドを受け取り、それを [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) からメディエーターへと送信します (`_mediator.Send(message.Command)` を実行している、上記のコードの最後の部分)。

これを行う際には、ビジネス コマンド ハンドラー (この場合は、注文データベースに対してトランザクションを実行している [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs)) がリンクされ、実行されます。次にコードを示します。

```csharp
// CreateOrderCommandHandler.cs
public class CreateOrderCommandHandler
                                   : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Add/Update the Buyer AggregateRoot
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

### <a name="register-the-types-used-by-mediatr"></a>MediatR によって使用される型の登録

MediatR にコマンド ハンドラー クラスを認識させるには、メディエーター クラスとコマンド ハンドラー クラスを IoC コンテナーに登録する必要があります。 既定では、MediatR は Autofac を IoC コンテナーとして使用しますが、組み込みの ASP.NET Core IoC コンテナーや、MediatR でサポートされているその他のコンテナーを使用することもできます。

次のコードは、Autofac モジュールを使用している場合に、メディエーターの型とコマンドを登録する方法を示したものです。

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IAsyncRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IAsyncRequestHandler<,>));
        // Other types registration
        //...
    }
}
```

"魔法が起こる" のは、まさにこの部分です。

各コマンド ハンドラーには汎用の `IAsyncRequestHandler<T>` インターフェイスが実装されているため、アセンブリを登録すると、`IAsyncRequestHandler` としてマークされたすべての型が `RegisteredAssemblyTypes` に登録され、`CommandHandler` クラスに記述されたリレーションシップにより、`CommandHandlers` が対応する `Commands` に関連付けられます。次に例を示します。

```csharp
public class CreateOrderCommandHandler
  : IAsyncRequestHandler<CreateOrderCommand, bool>
{
```

これが、コマンドをコマンド ハンドラーに関連付けるコードです。 ハンドラーは非常にシンプルなクラスですが、`RequestHandler<T>` から継承されるクラスであり、T はコマンドの型になります。また、MediatR により、正しいペイロード (コマンド) で呼び出されます。

## <a name="apply-cross-cutting-concerns-when-processing-commands-with-the-behaviors-in-mediatr"></a>MediatR のビヘイビアーを使用して、コマンドの処理時に横断的関心事を適用する

もう 1 つ重要なことがあります。それは、横断的関心事をメディエーター パイプラインに適用することです。 Autofac 登録モジュール コードの末尾を見ると、ビヘイビアー型 (カスタム LoggingBehavior クラスと ValidatorBehavior クラス) がどのように登録されるかがわかります。 ただし、その他のカスタム ビヘイビアーを追加することもできます。

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IAsyncRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IAsyncRequestHandler<,>));
        // Other types registration
        //...
        builder.RegisterGeneric(typeof(LoggingBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
        builder.RegisterGeneric(typeof(ValidatorBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
    }
}
```

この [LoggingBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) クラスは、次のコードとして実装することができます。これにより、実行されるコマンド ハンドラーに関する情報と、それが成功したかどうかどうかが記録されます。

```csharp
public class LoggingBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly ILogger<LoggingBehavior<TRequest, TResponse>> _logger;
    public LoggingBehavior(ILogger<LoggingBehavior<TRequest, TResponse>> logger) =>
                                                                  _logger = logger;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        _logger.LogInformation($"Handling {typeof(TRequest).Name}");
        var response = await next();
        _logger.LogInformation($"Handled {typeof(TResponse).Name}");
        return response;
    }
}
```

このビヘイビアー クラスを実装し、それをパイプラインで登録することで (上記の MediatorModule)、MediatR を通じて処理されたすべてのコマンドが、実行に関するロギング情報になります。

eShopOnContainers 注文マイクロサービスでは、基本検証用に 2 つ目のビヘイビアーも適用されます。[FluentValidation](https://github.com/JeremySkinner/FluentValidation) ライブラリに依存する [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) クラスです。次にコードを示します。

```csharp
public class ValidatorBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly IValidator<TRequest>[] _validators;
    public ValidatorBehavior(IValidator<TRequest>[] validators) =>
                                                         _validators = validators;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        var failures = _validators
            .Select(v => v.Validate(request))
            .SelectMany(result => result.Errors)
            .Where(error => error != null)
            .ToList();

        if (failures.Any())
        {
            throw new OrderingDomainException(
                $"Command Validation Errors for type {typeof(TRequest).Name}",
                        new ValidationException("Validation exception", failures));
        }

        var response = await next();
        return response;
    }
}
```

検証でエラーが出た場合はここのビヘイビアーによって例外が発行されますが、結果オブジェクトを返すこともできる場合があります。成功であればコマンド結果が含まれ、失敗であれば検証メッセージが含まれます。 おそらくは、より簡単にユーザーに検証結果を示すことができます。

その後、[FluentValidation](https://github.com/JeremySkinner/FluentValidation) ライブラリに基づいて、CreateOrderCommand によって渡されたデータ用の検証が作成されました。次にコードを示します。

```csharp
public class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(command => command.City).NotEmpty();
        RuleFor(command => command.Street).NotEmpty();
        RuleFor(command => command.State).NotEmpty();
        RuleFor(command => command.Country).NotEmpty();
        RuleFor(command => command.ZipCode).NotEmpty();
        RuleFor(command => command.CardNumber).NotEmpty().Length(12, 19);
        RuleFor(command => command.CardHolderName).NotEmpty();
        RuleFor(command => command.CardExpiration).NotEmpty().Must(BeValidExpirationDate).WithMessage("Please specify a valid card expiration date");
        RuleFor(command => command.CardSecurityNumber).NotEmpty().Length(3);
        RuleFor(command => command.CardTypeId).NotEmpty();
        RuleFor(command => command.OrderItems).Must(ContainOrderItems).WithMessage("No order items found");
    }

    private bool BeValidExpirationDate(DateTime dateTime)
    {
        return dateTime >= DateTime.UtcNow;
    }

    private bool ContainOrderItems(IEnumerable<OrderItemDTO> orderItems)
    {
        return orderItems.Any();
    }
}

```

追加の検証を作成することもできます。 これは、コマンド検証を実装するための非常にクリーンで簡潔な方法です。

同様の方法で、コマンドの処理時にそれらに適用したい追加のアスペクトや横断的関心事に対する、その他のビヘイビアーを実装することもできます。

#### <a name="additional-resources"></a>その他の技術情報

##### <a name="the-mediator-pattern"></a>メディエーター パターン

- **メディエーター パターン** \
  [https://en.wikipedia.org/wiki/Mediator\_pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

##### <a name="the-decorator-pattern"></a>デコレーター パターン

- **デコレーター パターン** \
  [https://en.wikipedia.org/wiki/Decorator\_pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

##### <a name="mediatr-jimmy-bogard"></a>MediatR (Jimmy Bogard)

- **MediatR。** GitHub リポジトリ。 \
  <https://github.com/jbogard/MediatR>

- **MediatR と AutoMapper での CQRS** \
  <https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/>

- **コントローラーの簡略化:POST とコマンド。** \
  <https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/>

- **メディエーター パイプラインでの横断的関心事への取り組み** \
  <https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/>

- **CQRS と REST: 完全な一致** \
  <https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/>

- **MediatR パイプラインの例** \
  <https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/>

- **MediatR と ASP.NET Core の縦方向のスライス テスト フィクスチャ** \
  <https://lostechies.com/jimmybogard/2016/10/24/vertical-slice-test-fixtures-for-mediatr-and-asp-net-core/>

- **Microsoft 依存関係挿入用の MediatR 拡張機能のリリース** \
  <https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/>

##### <a name="fluent-validation"></a>Fluent 検証

- **Jeremy Skinner。FluentValidation.** GitHub リポジトリ。 \
  <https://github.com/JeremySkinner/FluentValidation>

> [!div class="step-by-step"]
> [前へ](microservice-application-layer-web-api-design.md)
> [次へ](../implement-resilient-applications/index.md)
