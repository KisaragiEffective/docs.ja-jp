---
title: Entity Framework Core でインフラストラクチャの永続レイヤーを実装する
description: コンテナー化された .NET アプリケーション向け .NET マイクロサービス アーキテクチャ | Entity Framework Core を使用してインフラストラクチャの永続レイヤーを実装する方法の詳細を確認します。
ms.date: 01/30/2020
ms.openlocfilehash: 63579dc74ba52551bc1ee02a57337c1b17fdf396
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401497"
---
# <a name="implement-the-infrastructure-persistence-layer-with-entity-framework-core"></a>Entity Framework Core でインフラストラクチャの永続レイヤーを実装する

SQL Server、Oracle、PostgreSQL など、リレーショナル データベースを使用するとき、Entity Framework (EF) に基づいて永続レイヤーを実装することをお勧めします。 EF は LINQ 対応であり、厳密に型指定されたオブジェクトをモデルに与えます。また、データベースにシンプルな永続性が与えられます。

Entity Framework は長い間、.NET Framework の一部でした。 .NET Core を使用するとき、Entity Framework Core も使用してください。 .NET Core と同様に、Windows または Linux で実行されます。 EF Core は Entity Framework を完全に書き換えたものであり、フットプリントが大幅に少なくなっており、パフォーマンス面で重要な改善が行われています。

## <a name="introduction-to-entity-framework-core"></a>Entity Framework Core の概要

Entity Framework (EF) Core は人気の Entity Framework データ アクセス テクノロジの軽量版であり、拡張性に優れ、プラットフォームに依存しません。 2016 年の中頃に .NET Core に導入されました。

EF Core の概要は Microsoft ドキュメントで既に利用可能になっているので、ここではそのリンクのみを掲載しておきます。

### <a name="additional-resources"></a>その他の技術情報

- **Entity Framework Core** \
  [https://docs.microsoft.com/ef/core/](/ef/core/)

- **Visual Studio を使用した ASP.NET Core と Entity Framework Core の概要** \
  [https://docs.microsoft.com/aspnet/core/data/ef-mvc/](/aspnet/core/data/ef-mvc/)

- **DbContext クラス** \
  [https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext](xref:Microsoft.EntityFrameworkCore.DbContext)

- **EF Core と EF6.x を比較する** \
  [https://docs.microsoft.com/ef/efcore-and-ef6/index](/ef/efcore-and-ef6/index)

## <a name="infrastructure-in-entity-framework-core-from-a-ddd-perspective"></a>DDD の観点から見た Entity Framework Core のインフラストラクチャ

DDD の観点から見ると、EF の重要な機能は、POCO ドメイン エンティティを使用できることです。EF 用語では、*POCO Code First エンティティ*と呼ばれています。 POCO ドメイン エンティティを使用する場合、ドメイン モデル クラスは永続性無視になります。[永続化の無視 (Persistence Ignorance)](https://deviq.com/persistence-ignorance/) の原則と[インフラストラクチャの無視 (Infrastructure Ignorance)](https://ayende.com/blog/3137/infrastructure-ignorance) の原則に従います。

DDD パターンごとに、エンティティ クラス自体の中にドメインの動作とルールをカプセル化してください。そうすることで、エンティティ クラス自体でコレクションにアクセスするとき、インバリアント、検証、ルールを制御できます。 そのため、子エンティティや値オブジェクトのコレクションにパブリック アクセスを許可することは、DDD ではお勧めされません。 それよりも、フィールドとプロパティのコレクションを更新する方法とタイミング、更新時のビヘイビアーとアクションを制御するメソッドを公開することをお勧めします。

EF Core 1.1 以降、このような DDD 要件を満たすために、パブリック プロパティの代わりに、エンティティにプレーン フィールドを含めることができます。 エンティティ フィールドに外部からアクセスできるようにする場合、プロパティの代わりに、属性やフィールドを作成できます。 プライベート プロパティの設定機能を利用することもできます。

同様に、`IReadOnlyCollection<T>` として型指定されたパブリック プロパティを利用することで、コレクションに読み取り専用アクセスできるようになりました。この機能をサポートするのがエンティティのコレクションのプライベート フィールド メンバーです (`List<T>` など)。このメンバーは永続性を EF に依存します。 以前のバージョンの Entity Framework では、コレクション プロパティで `ICollection<T>` をサポートする必要がありました。つまり、開発者が親エンティティ クラスを利用するとき、そのプロパティ コレクションから項目を追加したり、削除したりできました。 その機能は DDD の推奨パターンに反する可能性がありました。

読み取り専用 `IReadOnlyCollection<T>` オブジェクトを公開するとき、プライベート コレクションを使用できます。次のコード例をご覧ください。

```csharp
public class Order : Entity
{
    // Using private fields, allowed since EF Core 1.1
    private DateTime _orderDate;
    // Other fields ...

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    protected Order() { }

    public Order(int buyerId, int paymentMethodId, Address address)
    {
        // Initializations ...
    }

    public void AddOrderItem(int productId, string productName,
                             decimal unitPrice, decimal discount,
                             string pictureUrl, int units = 1)
    {
        // Validation logic...

        var orderItem = new OrderItem(productId, productName,
                                      unitPrice, discount,
                                      pictureUrl, units);
        _orderItems.Add(orderItem);
    }
}
```

`OrderItems` プロパティには `IReadOnlyCollection<OrderItem>` を利用して読み取り専用アクセスのみが可能となることに注意してください。 この型は読み取り専用であり、定期的な外部更新から守られます。

EF Core では、ドメイン モデルを "汚染する" ことなく物理データベースにマッピングできます。 マッピング アクションは永続レイヤーで行われるため、純粋な .NET POCO コードになります。 そのマッピング アクションでは、フィールドとデータベースのマッピングを構成する必要があります。 `OrderingContext` からの `OnModelCreating` メソッドと `OrderEntityTypeConfiguration` クラスが出てくる次の例では、`SetPropertyAccessMode` を呼び出すことで、そのフィールドを介して `OrderItems` プロパティにアクセスするように EF Core に伝えます。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities’ configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
        // Other configuration

        var navigation =
              orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        //EF access the OrderItem collection property through its backing field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        // Other configuration
    }
}
```

プロパティの代わりにフィールドを使用するとき、`List<OrderItem>` プロパティが与えられているかのように `OrderItem` エンティティが永続化されます。 ただし、アクセサーが 1 つ公開されます。新しい項目を注文に追加する `AddOrderItem` メソッドです。 結果として、ビヘイビアーとデータが結び付けられ、ドメイン モデルを使用するアプリケーション コード全体で一貫性が与えられます。

## <a name="implement-custom-repositories-with-entity-framework-core"></a>Entity Framework Core でカスタム リポジトリを実装する

実装レベルでは、リポジトリは、更新の実行時に作業単位 (EF Core の DBContext) で調整されるデータ永続化コードを持つクラスです。次のクラスをご覧ください。

```csharp
// using statements...
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class BuyerRepository : IBuyerRepository
    {
        private readonly OrderingContext _context;
        public IUnitOfWork UnitOfWork
        {
            get
            {
                return _context;
            }
        }

        public BuyerRepository(OrderingContext context)
        {
            _context = context ?? throw new ArgumentNullException(nameof(context));
        }

        public Buyer Add(Buyer buyer)
        {
            return _context.Buyers.Add(buyer).Entity;
        }

        public async Task<Buyer> FindAsync(string BuyerIdentityGuid)
        {
            var buyer = await _context.Buyers
                .Include(b => b.Payments)
                .Where(b => b.FullName == BuyerIdentityGuid)
                .SingleOrDefaultAsync();

            return buyer;
        }
    }
}
```

IBuyerRepository インターフェイスは、コントラクトとしてのドメイン モデル レイヤーから取得されることに注意してください。 ただし、リポジトリ実装は永続化とインフラストラクチャのレイヤーで行われます。

EF DbContext は、依存関係の挿入により、コンストラクター経由で取得されます。 IoC コンテナー (`services.AddDbContext<>` で明示的に設定することもできます) にある既定の有効期間 (`ServiceLifetime.Scoped`) に起因し、同じ HTTP 要求範囲内の複数のリポジトリ間で強要されます。

### <a name="methods-to-implement-in-a-repository-updates-or-transactions-versus-queries"></a>リポジトリで実装するためのメソッド (更新またはトランザクションとクエリ)

各リポジトリ クラス内に、関連集約に含まれるエンティティの状態を更新する永続性メソッドを置いてください。 集約とその関連リポジトリの間には一対一の関係があることに注意してください。 集約ルート エンティティ オブジェクトがその EF グラフ内に子エンティティを埋め込んでいる可能性を考慮してください。 たとえば、購入者には、関連する子エンティティとして複数の支払方法が与えられている可能性があります。

eShopOnContainers でマイクロサービスを注文する手法も CQS/CQRS に基づいているため、クエリの多くはカスタム リポジトリで実装されません。 集約、集約別のカスタム リポジトリ、一般的な DDD によって課される制約なしで、開発者はプレゼンテーション レイヤーに必要なクエリや結合を自由に作成できます。 本ガイドで提案するカスタム リポジトリの多くには更新メソッドとトランザクション メソッドがいくつかあるが、更新するデータの取得に必要なクエリ メソッドに限られます。 たとえば、BuyerRepository リポジトリは FindAsync メソッドを実装します。アプリケーションでは、注文に関連する新しい購入者を作成する前に、特定の購入者が存在するかどうかを認識する必要があるためです。

ただし、前述のように、Dapper を利用するフレキシブル クエリに基づく CQRS クエリで、プレゼンテーション レイヤーまたはクライアント アプリに送信するデータを取得する真のクエリ メソッドが実装されます。

### <a name="using-a-custom-repository-versus-using-ef-dbcontext-directly"></a>カスタム リポジトリの使用と EF DbContext の直接使用の違い

Entity Framework DbContext クラスは Unit of Work と Repository のパターンに基づきます。ASP.NET Core MVC コントローラーなど、コードから直接使用できます。 これで、eShopOnContainers の CRUD カタログ マイクロサービスのように、最も単純なコードを作成できます。 できるだけ単純なコードが必要であれば、DbContext クラスを直接使用することをお勧めします。多くの開発者がそうしています。

ただし、カスタム リポジトリを実装するのであれば、より複雑なマイクロサービスやアプリケーションを実装するとき、いくつかの利点があります。 Unit of Work と Repository のパターンは、インフラストラクチャの永続レイヤーのカプセル化を意図しています。アプリケーションとドメイン モデルのレイヤーから切り離されます。 これらのパターンを実装すると、データベースへのアクセスをシミュレートするモック リポジトリの使用が容易になります。

図 7-18 では、リポジトリを使用しない (EF DbContext を直接使用する) 場合とリポジトリのモック (疑似) を簡単にするリポジトリを使用する場合の違いを確認できます。

![2 つのリポジトリ内のコンポーネントとデータフローを示す図。](./media/infrastructure-persistence-layer-implemenation-entity-framework-core/custom-repo-versus-db-context.png)

**図 7-18**。 カスタム リポジトリとプレーンな DbContext の使用の違い

図 7-18 に示すように、カスタム リポジトリを使用すると抽象化レイヤーが追加され、それを使用してリポジトリのモックを作成することで、テストが容易になります。 モックにはさまざまな代替手法があります。 リポジトリだけをモックしたり、作業単位全体をモックしたりすることができます。 通常、リポジトリだけをモックする手法で十分です。作業単位全体を抽出し、モックする複雑な作業は通常は必要ありません。

この後、アプリケーション レイヤーについて取り上げるとき、依存関係の挿入が ASP.NET Core でどのように機能するか、リポジトリを使用するとき、それがどのように実装されるか説明します。

手短に言えば、カスタム リポジトリでは、データ層の状態の影響を受けない単体テストを利用し、より簡単にコードをテストできます。 Entity Framework 経由で実際のデータベースにもアクセスするテストを実行する場合、単体テストではなく統合テストになり、大幅に遅くなります。

DbContext を直接使用した場合、それをモックするか、メモリ内 SQL Server と単体テスト用の予測可能データを使用して単体テストを実行する必要があります。 しかしながら、DbContext をモックすること、あるいは偽のデータを管理することには、リポジトリ レベルでモックするよりも多くの作業が要求されます。 もちろん、MVC コント ローラーはいつでもテストできます。

## <a name="ef-dbcontext-and-iunitofwork-instance-lifetime-in-your-ioc-container"></a>IoC コンテナーの EF DbContext と IUnitOfWork のインスタンス有効期間

`DbContext` オブジェクト (`IUnitOfWork` オブジェクトとして公開) は、同じ HTTP 要求範囲内にある複数のリポジトリ間で共有する必要があります。 たとえば、実行する操作に複数の集約が伴うときに共有が必要になります。あるいは、複数のリポジトリ インスタンスを利用していると理由で共有が必要になります。 `IUnitOfWork` インターフェイスが EF Core タイプではなく、ドメイン レイヤーに含まれるということも重要です。

そのためには、`DbContext` オブジェクトのインスタンスのサービス有効期間を ServiceLifetime.Scoped に設定する必要があります。 これは、ASP.NET Core Web API プロジェクトの `Startup.cs` ファイルの ConfigureServices メソッドから IoC コンテナーの `services.AddDbContext` に `DbContext` を登録したときの既定の有効期間です。 次に例を示します。

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(HttpGlobalExceptionFilter));
    }).AddControllersAsServices();

    services.AddEntityFrameworkSqlServer()
      .AddDbContext<OrderingContext>(options =>
      {
          options.UseSqlServer(Configuration["ConnectionString"],
                               sqlOptions => sqlOptions.MigrationsAssembly(typeof(Startup).GetTypeInfo().
                                                                                    Assembly.GetName().Name));
      },
      ServiceLifetime.Scoped // Note that Scoped is the default choice
                             // in AddDbContext. It is shown here only for
                             // pedagogic purposes.
      );
}
```

DbContext インスタンス化モードは ServiceLifetime.Transient または ServiceLifetime.Singleton に設定しないでください。

## <a name="the-repository-instance-lifetime-in-your-ioc-container"></a>IoC コンテナーのリポジトリ インスタンスの有効期間

同様に、リポジトリの有効期間は通常、範囲として設定してください (InstancePerLifetimeScope in Autofac)。 一時的として設定することもできますが、範囲が指定された有効期間のほうがサービスのメモリが効率的になります。

```csharp
// Registering a Repository in Autofac IoC container
builder.RegisterType<OrderRepository>()
    .As<IOrderRepository>()
    .InstancePerLifetimeScope();
```

DbContext の有効期間が範囲 (InstancePerLifetimeScope) として設定されているとき (DBContext の既定の有効期間)、リポジトリに単一の有効期間を使用すると、コンカレンシー関連で重大な問題が発生する可能性があります。

### <a name="additional-resources"></a>その他の技術情報

- **ASP.NET MVC アプリケーションでの Repository および Unit of Work パターンの実装** \
  <https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application>

- **Jonathan Allen。Entity Framework、Dapper、Chain を使用する Repository パターンの実装方法** \
  <https://www.infoq.com/articles/repository-implementation-strategies>

- **Cesar de la Torre。ASP.NET Core IoC コンテナー サービスの有効期間と Autofac IoC コンテナー インスタンスの範囲の比較** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="table-mapping"></a>テーブル マッピング

テーブル マッピングでは、データベースに問い合わせたり、保存したりするテーブル データが識別されます。 先に、ドメイン エンティティ (製品や注文のドメインなど) を利用し、関連データベース スキーマを生成する方法を確認しました。 EF は、*規則*という概念を中心に作られています。 規則は “テーブルの名前は何になるのか?” または “主キーはどのようなプロパティか?” のような質問に対処するものです。 規則は通常、慣例的な名前に基づきます。たとえば、主キーであれば、ID で終わるプロパティが一般的に与えられます。

規則では、派生コンテキストでエンティティを公開する `DbSet<TEntity>` プロパティと同じ名前を持つテーブルにマッピングされるように各エンティティが設定されます。 所与のエンティティに `DbSet<TEntity>` 値が指定されない場合、クラス名が使用されます。

### <a name="data-annotations-versus-fluent-api"></a>データの注釈と Fluent API の違い

付加的な EF Core 規則がたくさん存在します。そのほとんどは、OnModelCreating メソッド内に実装されるデータの注釈か Fluent API を利用して変更できます。

データの注釈はエンティティ モデル クラス自体で使用する必要があります。DDD の観点からは、侵入性が高い方法となります。 これはインフラストラクチャ データベースに関連するデータ注釈によってモデルに悪影響を及ぼしているためです。 一方、Fluent API は、データ永続性インフラストラクチャ レイヤー内でほとんどの規則やマッピングを変更するときに便利な方法となります。エンティティ モデルは汚染されず、永続性インフラストラクチャから切り離されます。

### <a name="fluent-api-and-the-onmodelcreating-method"></a>Fluent API と OnModelCreating メソッド

前述のように、規則やマッピングを変更するために、DbContext クラスで OnModelCreating メソッドを使用できます。

eShopOnContainers の注文マイクロサービスでは、必要に応じて、明示的なマッピングと構成が実装されます。次のコードをご覧ください。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities’ configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);

        orderConfiguration.HasKey(o => o.Id);

        orderConfiguration.Ignore(b => b.DomainEvents);

        orderConfiguration.Property(o => o.Id)
            .UseHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

        //Address value object persisted as owned entity type supported since EF Core 2.0
        orderConfiguration
            .OwnsOne(o => o.Address, a =>
            {
                a.WithOwner();
            });

        orderConfiguration
            .Property<int?>("_buyerId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("BuyerId")
            .IsRequired(false);

        orderConfiguration
            .Property<DateTime>("_orderDate")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderDate")
            .IsRequired();

        orderConfiguration
            .Property<int>("_orderStatusId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderStatusId")
            .IsRequired();

        orderConfiguration
            .Property<int?>("_paymentMethodId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("PaymentMethodId")
            .IsRequired(false);

        orderConfiguration.Property<string>("Description").IsRequired(false);

        var navigation = orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        // DDD Patterns comment:
        //Set as field (New since EF 1.1) to access the OrderItem collection property through its field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        orderConfiguration.HasOne<PaymentMethod>()
            .WithMany()
            .HasForeignKey("_paymentMethodId")
            .IsRequired(false)
            .OnDelete(DeleteBehavior.Restrict);

        orderConfiguration.HasOne<Buyer>()
            .WithMany()
            .IsRequired(false)
            .HasForeignKey("_buyerId");

        orderConfiguration.HasOne(o => o.OrderStatus)
            .WithMany()
            .HasForeignKey("_orderStatusId");
    }
}
```

同じ `OnModelCreating` メソッド内ですべての Fluent API マッピングを設定できますが、そのコードを区切り、エンティティあたり 1 つの複数の構成クラスを設定することをお勧めします。例をご覧ください。 特に大規模なモデルでは、個別の構成クラスを設定し、異なる種類のエンティティを構成することをお勧めします。

例のコードでは、明示的な宣言とマッピングをいくつか確認できます。 ただし、EF Core の規則では、これらのマッピングの多くが自動的に行われます。そのため、実際に必要になるコードはより小さくなります。

### <a name="the-hilo-algorithm-in-ef-core"></a>EF Core の Hi/Lo アルゴリズム

先の例のコードで興味深いところは、キーの生成方法として [Hi/Lo アルゴリズム](https://vladmihalcea.com/the-hilo-algorithm/)が使用されていることです。

Hi/Lo アルゴリズムは、変更のコミット前に一意のキーが必要なときに便利です。 手短に言えば、Hi/Lo アルゴリズムでは、一意の識別子がテーブル行に割り当てられます。データベースにすぐに行を格納することには依存しません。 連続する通常のデータベース ID と同様に、識別子の使用をすぐに開始できます。

Hi/Lo アルゴリズムは、関連のあるデータベース シーケンスから一意の ID をまとめて取得するためのメカニズムです。 データベースによって一意性が保証され、ユーザー間の競合がないため、取得した ID は安全に使用できます。 このアルゴリズムは次の理由から興味深いものになっています。

- Unit of Work パターンを壊しません。

- シーケンス ID をまとめて取得し、データベースとのラウンド トリップを最小限に抑えます。

- GUID を利用する手法とは異なり、人間にわかりやすい識別子が生成されます。

EF Core では `UseHiLo` メソッドで [HiLo](https://stackoverflow.com/questions/282099/whats-the-hi-lo-algorithm) がサポートされます。先の例をご覧ください。

### <a name="map-fields-instead-of-properties"></a>プロパティの代わりにフィールドをマッピングする

EF Core 1.1 以降で利用可能になったこの機能では、列をフィールドに直接マッピングできます。 エンティティ クラスのプロパティを使用せず、テーブルからフィールドに列をマッピングできます。 この方法の一般的な使用例に、エンティティの外部からアクセスする必要のない、内部状態用のプライベート フィールドがあります。

1 つのフィールドにマッピングすることも、`List<>` フィールドなど、コレクションにマッピングすることもできます。 この点については、ドメイン モデル クラスのモデリングについて説明したときにお伝えしました。ここでは、先のコードで強調表示されていた `PropertyAccessMode.Field` 構成でそのマッピングが実行されるしくみを確認できます。

### <a name="use-shadow-properties-in-ef-core-hidden-at-the-infrastructure-level"></a>インフラストラクチャ レベルでは非表示のシャドウ プロパティを EF Core で使用する

EF Core のシャドウ プロパティは、エンティティ クラス モデルに存在しないプロパティです。 これらのプロパティの値と状態は、インフラストラクチャ レベルの [ChangeTracker](https://docs.microsoft.com/ef/core/api/microsoft.entityframeworkcore.changetracking.changetracker) で汚染されないように保守管理されます。

## <a name="implement-the-query-specification-pattern"></a>クエリ仕様パターンを実装する

先にデザイン セクションで紹介したように、クエリ仕様パターンは、任意の並べ替え/ページング ロジックと共にクエリの定義を置く場所として作られる Domain-Driven Design パターンです。

クエリ仕様パターンによって、オブジェクトのクエリが定義されます。 たとえば、製品を検索するページ クエリをカプセル化するために、必要な入力パラメーター (pageNumber、pageSize、filter など) を受け取る PagedProduct 仕様を作成できます。 その後、Repository の任意のメソッド (通常は List() オーバーロード) 内で、IQuerySpecification を受け取り、その仕様に基づいて予想されるクエリを実行します。

一般的な Specification (仕様) インターフェイスの例として、[eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb) から抜粋した次のコードをご覧ください。

```csharp
// GENERIC SPECIFICATION INTERFACE
// https://github.com/dotnet-architecture/eShopOnWeb

public interface ISpecification<T>
{
    Expression<Func<T, bool>> Criteria { get; }
    List<Expression<Func<T, object>>> Includes { get; }
    List<string> IncludeStrings { get; }
}
```

一般的な仕様の基底クラスの実装は次のようになります。

```csharp
// GENERIC SPECIFICATION IMPLEMENTATION (BASE CLASS)
// https://github.com/dotnet-architecture/eShopOnWeb

public abstract class BaseSpecification<T> : ISpecification<T>
{
    public BaseSpecification(Expression<Func<T, bool>> criteria)
    {
        Criteria = criteria;
    }
    public Expression<Func<T, bool>> Criteria { get; }

    public List<Expression<Func<T, object>>> Includes { get; } =
                                           new List<Expression<Func<T, object>>>();

    public List<string> IncludeStrings { get; } = new List<string>();

    protected virtual void AddInclude(Expression<Func<T, object>> includeExpression)
    {
        Includes.Add(includeExpression);
    }

    // string-based includes allow for including children of children
    // e.g. Basket.Items.Product
    protected virtual void AddInclude(string includeString)
    {
        IncludeStrings.Add(includeString);
    }
}
```

次の仕様では、買い物かごの ID か買い物かごが属する購入者の ID が指定されると、買い物かごエンティティが 1 つ読み込まれます。 買い物かごの項目コレクションを[集中的に読み込み](https://docs.microsoft.com/ef/core/querying/related-data)ます。

```csharp
// SAMPLE QUERY SPECIFICATION IMPLEMENTATION

public class BasketWithItemsSpecification : BaseSpecification<Basket>
{
    public BasketWithItemsSpecification(int basketId)
        : base(b => b.Id == basketId)
    {
        AddInclude(b => b.Items);
    }

    public BasketWithItemsSpecification(string buyerId)
        : base(b => b.BuyerId == buyerId)
    {
        AddInclude(b => b.Items);
    }
}
```

最後になりますが、一般的 EF リポジトリでこのような仕様を利用し、指定されたエンティティ型 T に関連するデータにフィルターを適用し、一括で読み込むしくみを下の画像で確認してください。

```csharp
// GENERIC EF REPOSITORY WITH SPECIFICATION
// https://github.com/dotnet-architecture/eShopOnWeb

public IEnumerable<T> List(ISpecification<T> spec)
{
    // fetch a Queryable that includes all expression-based includes
    var queryableResultWithIncludes = spec.Includes
        .Aggregate(_dbContext.Set<T>().AsQueryable(),
            (current, include) => current.Include(include));

    // modify the IQueryable to include any string-based include statements
    var secondaryResult = spec.IncludeStrings
        .Aggregate(queryableResultWithIncludes,
            (current, include) => current.Include(include));

    // return the result of the query using the specification's criteria expression
    return secondaryResult
                    .Where(spec.Criteria)
                    .AsEnumerable();
}
```

この仕様はフィルタリング ロジックをカプセル化するだけでなく、データを入力するプロパティなど、返すデータのシェイプも指定できます。

リポジトリから `IQueryable` を返すことはお勧めできませんが、リポジトリ内で使用して結果の集まりを作ることには何の問題もありません。 上の List メソッドでこの手法の使用を確認できます。中間の `IQueryable` 式を利用してクエリのインクルード リストを作成し、それから最後の行にある仕様の基準に合わせてクエリが実行されています。

### <a name="additional-resources"></a>その他の技術情報

- **テーブル マッピング** \
  [https://docs.microsoft.com/ef/core/modeling/relational/tables](/ef/core/modeling/relational/tables)

- **HiLo を使用して Entity Framework Core でキーを生成する** \
  <https://www.talkingdotnet.com/use-hilo-to-generate-keys-with-entity-framework-core/>

- **バッキング フィールド** \
  [https://docs.microsoft.com/ef/core/modeling/backing-field](/ef/core/modeling/backing-field)

- **Steve Smith。Entity Framework Core のカプセル化されたコレクション** \
  <https://ardalis.com/encapsulated-collections-in-entity-framework-core>

- **シャドウ プロパティ** \
  [https://docs.microsoft.com/ef/core/modeling/shadow-properties](/ef/core/modeling/shadow-properties)

- **仕様パターン** \
  <https://deviq.com/specification-pattern/>

> [!div class="step-by-step"]
> [前へ](infrastructure-persistence-layer-design.md)
> [次へ](nosql-database-persistence-infrastructure.md)
