---
title: ASP.NET Core アプリでのデータの操作
description: ASP.NET Core および Azure での最新の Web アプリケーションの設計 | ASP.NET Core アプリでのデータの操作
author: ardalis
ms.author: wiwagn
ms.date: 12/04/2019
ms.openlocfilehash: 5a38ca94b6df676858e7cb058272e450aaf1572e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78241040"
---
# <a name="working-with-data-in-aspnet-core-apps"></a>ASP.NET Core アプリでのデータの操作

> "データは貴重なものであり、システム自体よりも長く存在します。"
>
> Tim Berners-lee

データ アクセスは、ほぼすべてのソフトウェア アプリケーションの重要な部分です。 ASP.NET Core は、Entity Framework Core (および Entity Framework 6 も) を含む、さまざまなデータ アクセス オプションをサポートし、どの .NET データ アクセス フレームワークでも使用できます。 使用するデータ アクセス フレームワークの選択は、アプリケーションのニーズによって異なります。 ApplicationCore および UI プロジェクトからのこれらの選択を抽象化し、インフラストラクチャに実装の詳細をカプセル化することは、疎結合のテスト可能なソフトウェアの生成に役立ちます。

## <a name="entity-framework-core-for-relational-databases"></a>Entity Framework Core (リレーショナル データベース用)

リレーショナル データを操作する必要がある新しい ASP.NET Core アプリケーションを作成する場合、Entity Framework Core (EF Core) を使用して、アプリケーションからそのデータにアクセスすることをお勧めします。 EF Core はオブジェクト リレーショナル マッパー (O/RM) であり、.NET 開発者がデータ ソースのオブジェクトを保持できるようにします。 これにより、通常は開発者が記述する必要があるほとんどのデータ アクセス コードが不要になります。 ASP.NET Core と同様、EF Core は、モジュラー型クロスプラットフォーム アプリケーションをサポートするために、完全に書き換えられました。 NuGet パッケージとしてご利用のアプリケーションに追加し、Startup で構成し、必要に応じて依存関係の挿入を介して要求します。

SQL Server データベースで EE Core を使用するには、次の dotnet CLI コマンドを実行します。

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

テストのために、InMemory データ ソースのサポートを追加するには、次のコマンドを使用します。

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="the-dbcontext"></a>DbContext

EF Core を操作するには、<xref:Microsoft.EntityFrameworkCore.DbContext> のサブクラスが必要です。 このクラスでは、ご利用のアプリケーションで操作するエンティティのコレクションを表すプロパティを保持します。 eShopOnWeb サンプルには、アイテム、ブランド、型のコレクションと共に CatalogContext が含まれます。

```csharp
public class CatalogContext : DbContext
{
    public CatalogContext(DbContextOptions<CatalogContext> options) : base(options)
    {

    }

    public DbSet<CatalogItem> CatalogItems { get; set; }

    public DbSet<CatalogBrand> CatalogBrands { get; set; }

    public DbSet<CatalogType> CatalogTypes { get; set; }
}
```

DbContext には、DbContextOptions を受け入れるコンストラクターが必要です。また、この引数を基本の DbContext コンストラクターに渡す必要があります。 ご利用のアプリケーションに DbContext が 1 つだけ存在する場合は、DbContextOptions のインスタンスを渡すことができますが、複数存在する場合は、ジェネリックの DbContextOptions\<T> 型を使用し、ジェネリック パラメーターとして DbContext 型を渡す必要があります。

### <a name="configuring-ef-core"></a>EF Core の構成

ASP.NET Core アプリケーションでは、通常、ConfigureServices メソッドで EF コアを構成します。 EF Core では、構成を効率化するいくつかの便利な拡張メソッドをサポートする、DbContextOptionsBuilder を使用します。 Configuration で定義されている接続文字列で SQL Server データベースを使用するように CatalogContext を構成するには、次のコードを ConfigureServices に追加します。

```csharp
services.AddDbContext<CatalogContext>(options => options.UseSqlServer (Configuration.GetConnectionString("DefaultConnection")));
```

メモリ内データベースを使用するには、次のようにします。

```csharp
services.AddDbContext<CatalogContext>(options =>
    options.UseInMemoryDatabase());
```

EF Core をインストールして、DbContext の子の型を作成し、それを ConfigureServices で構成したら、EE Core を使用できます。 DbContext 型のインスタンスを必要とする任意のサービスで要求し、単にコレクション内にある場合と同じように、LINQ を使用して永続化されたエンティティの操作を開始することができます。 EF Core では、ご利用のデータを格納して取得するために、LINQ 式を SQL クエリに変換する作業を行います。

図 8-1 に示すように、ロガーを構成し、そのレベルが少なくとも Information に設定されていることを確認することで、EF Core で実行されるクエリを確認できます。

![コンソールへの EF Core クエリのログ記録](./media/image8-1.png)

**図 8-1**。 コンソールへの EF Core クエリのログ記録

### <a name="fetching-and-storing-data"></a>データのフェッチと格納

EF Core からデータを取得するには、適切なプロパティにアクセスし、LINQ を使用して結果をフィルター処理します。 また、LINQ を使用して射影を実行し、1 つの型から別の型に結果を変換することもできます。 次の例では、CatalogBrands を取得します。ここでは、名前で並べ替え、Enabled プロパティでフィルター処理し、SelectListItem 型に射影します。

```csharp
var brandItems = await _context.CatalogBrands
    .Where(b => b.Enabled)
    .OrderBy(b => b.Name)
    .Select(b => new SelectListItem {
        Value = b.Id, Text = b.Name })
    .ToListAsync();
```

上記の例では、クエリをすぐに実行するために、ToListAsync への呼び出しを追加することが重要です。 それ以外の場合、ステートメントは IQueryable\<SelectListItem> を brandItems に割り当て、列挙されるまで実行されません。 メソッドから IQueryable 結果を返すことには長所と短所があります。 EF Core で構築されるクエリをさらに変更できますが、EF Core で変換できないクエリに操作が追加された場合、実行時にのみ発生するエラーが発生する可能性もあります。 データ アクセスを実行するメソッドにすべてのフィルターを渡して、結果としてメモリ内コレクション (List\<T> など) を戻す方が一般的には安全です。

EF Core は、永続化からフェッチするエンティティの変更を追跡します。 追跡対象エンティティの変更を保存するには、DbContext で SaveChanges メソッドを呼び出すだけです。これで、エンティティのフェッチに使用されたものと同じ DbContext インスタンスであることが確認されます。 エンティティの追加と削除は適切な DbSet プロパティで直接行います。この場合も、データベース コマンドを実行するために SaveChanges を呼び出します。 次の例では、永続化のエンティティの追加、更新、および削除を示します。

```csharp
// create
var newBrand = new CatalogBrand() { Brand = "Acme" };
_context.Add(newBrand);
await _context.SaveChangesAsync();

// read and update
var existingBrand = _context.CatalogBrands.Find(1);
existingBrand.Brand = "Updated Brand";
await _context.SaveChangesAsync();

// read and delete (alternate Find syntax)
var brandToDelete = _context.Find<CatalogBrand>(2);
_context.CatalogBrands.Remove(brandToDelete);
await _context.SaveChangesAsync();
```

EF Core では、フェッチおよび保存のための同期と非同期の両方のメソッドがサポートされます。 Web アプリケーションでは、非同期メソッドで async/await パターンを使用することをお勧めします。これにより、データ アクセス操作が完了するのを待機している間に、Web サーバーのスレッドがブロックされなくなります。

### <a name="fetching-related-data"></a>関連データのフェッチ

EF Core は、エンティティの取得時に、データベースのそのエンティティを直接格納されているすべてのプロパティに設定します。 関連エンティティのリストなどの、ナビゲーション プロパティは設定されず、その値が null に設定される場合があります。 これにより、EF Core は必要以上のデータをフェッチしなくなります。これは、効率的な方法ですばやく要求を処理し、応答を返す必要がある、Web アプリケーションの場合に特に重要です。 _一括読み込み_を使用して、エンティティとのリレーションシップを含めるには、次のように、クエリで Include 拡張メソッドを使用してプロパティを指定します。

```csharp
// .Include requires using Microsoft.EntityFrameworkCore
var brandsWithItems = await _context.CatalogBrands
    .Include(b => b.Items)
    .ToListAsync();
```

複数のリレーションシップを含めることができます。また、ThenInclude を使用して、サブリレーションシップを含めることもできます。 EF Core は単一のクエリを実行して、エンティティの結果セットを取得します。 あるいは、次のように '.' で区切った文字列を `.Include()` 拡張メソッドに渡すことで、ナビゲーション プロパティを含めることができます。

```csharp
    .Include(“Items.Products”)
```

この仕様はフィルタリング ロジックをカプセル化するだけでなく、データを入力するプロパティなど、返すデータのシェイプも指定できます。 eShopOnWeb サンプルには、仕様内で一括読み込み情報のカプセル化を実演するいくつかの仕様が含まれています。 クエリの一部として仕様がどのように使用されるのかここで確認できます。

```csharp
// Includes all expression-based includes
query = specification.Includes.Aggregate(query,
            (current, include) => current.Include(include));

// Include any string-based include statements
query = specification.IncludeStrings.Aggregate(query,
            (current, include) => current.Include(include));
```

関連データを読み込むために、_明示的読み込み_を使用することもできます。 明示的読み込みを使用すれば、既に取得されているエンティティに追加データを読み込むことができます。 これにはデータベースへの個別の要求が必要になるため、要求ごとに行われるデータベース ラウンド トリップの数を最小限に抑える必要がある、Web アプリケーションではお勧めできません。

_遅延読み込み_は、アプリケーションでの参照時に関連データを自動的に読み込む機能です。 EF Core では、バージョン 2.1 で遅延読み込みのサポートが追加されました。 遅延読み込みは既定では有効になりません。`Microsoft.EntityFrameworkCore.Proxies` をインストールする必要があります。 明示的読み込みと同様、遅延読み込みは通常、Web アプリケーションでは無効にする必要があります。遅延読み込みを使用すると、各 Web 要求内で追加のデータベース クエリが行われるためです。 待機時間が短く、テストに使用されるデータ セットが小さい場合、残念ながら、遅延読み込みによって発生するオーバーヘッドについては開発時にあまり気付きません。 しかし、ユーザーやデータがより多く、待機時間がより長い運用環境では、追加のデータベース要求により、遅延読み込みを多用する Web アプリケーションのパフォーマンスが低下する可能性があります。

[Web アプリケーションでのエンティティの遅延読み込みを回避する](https://ardalis.com/avoid-lazy-loading-entities-in-asp-net-applications)

### <a name="encapsulating-data"></a>データをカプセル化する

EF Core は、モデルでその状態を正しくカプセル化するための機能をいくつか備えています。 ドメイン モデルでよくある問題として、コレクションのナビゲーション プロパティが一般にアクセス可能なリスト型として公開されることがあります。 そうすると、コラボレーターが誰でもこれらのコレクション型のコンテンツを操作できるようになり、結果として、コレクションに関連する重要なビジネス ルールがバイパスされ、オブジェクトが無効な状態のままになる可能性があります。 これに対する解決策は、関連するコレクションへの読み取り専用アクセスを公開することと、クライアントがこれらのコレクションを操作する方法を定義したメソッドを明示的に提供することです。次の例をご覧ください。

```csharp
public class Basket : BaseEntity
{
    public string BuyerId { get; set; }
    private readonly List<BasketItem> _items = new List<BasketItem>();
    public IReadOnlyCollection<BasketItem> Items => _items.AsReadOnly();

    public void AddItem(int catalogItemId, decimal unitPrice, int quantity = 1)
    {
        if (!Items.Any(i => i.CatalogItemId == catalogItemId))
        {
            _items.Add(new BasketItem()
            {
                CatalogItemId = catalogItemId,
                Quantity = quantity,
                UnitPrice = unitPrice
            });
            return;
        }
        var existingItem = Items.FirstOrDefault(i => i.CatalogItemId == catalogItemId);
        existingItem.Quantity += quantity;
    }
}
```

このエンティティ型ではパブリックの `List` または `ICollection` プロパティが公開されず、代わりに、基になるリスト型をラップする `IReadOnlyCollection` 型が公開されます。 このパターンを使用するとき、次のように、バッキング フィールドを使用するように Entity Framework Core に指示できます。

```csharp
private void ConfigureBasket(EntityTypeBuilder<Basket> builder)
{
    var navigation = builder.Metadata.FindNavigation(nameof(Basket.Items));

    navigation.SetPropertyAccessMode(PropertyAccessMode.Field);
}
```

ドメイン モデルを改善する別の方法としては、ID がなく、そのプロパティだけで区別される型に値オブジェクトを使用するという方法があります。 エンティティのプロパティとしてこのような型を使用すると、ロジックはそれが属する値オブジェクトに固有となり、同じ概念を使用する複数のエンティティ間でロジックが重複することがありません。 Entity Framework Core では、次のように、所有されるエンティティとして型を構成することで、所有するエンティティと同じテーブルに値オブジェクトを保持できます。

```csharp
private void ConfigureOrder(EntityTypeBuilder<Order> builder)
{
    builder.OwnsOne(o => o.ShipToAddress);
}
```

この例では、`ShipToAddress` プロパティの型は `Address` です。 `Address` は、`Street` や `City` など、いくつかのプロパティを持つ値オブジェクトです。 EF Core では、`Address` プロパティごとに 1 列の配分で `Order` オブジェクトがそのテーブルにマッピングされます。各列の名前の先頭にプロパティの名前が接頭辞として付きます。 この例で、`Order` テーブルに `ShipToAddress_Street` や `ShipToAddress_City` などの列が含まれます。 必要に応じて、所有型を別のテーブルに格納することもできます。

詳細については、[EF Core の所有エンティティのサポート](/ef/core/modeling/owned-entities)に関する記事を参照してください。

### <a name="resilient-connections"></a>回復力のある接続

SQL データベースなどの外部リソースが使用できなくなる場合があります。 一時的に使用できなくなった場合、アプリケーションは再試行ロジックを使用して、例外の発生を防ぐことができます。 この手法は一般的に_接続の復元性_と呼ばれます。 [指数バックオフを含む独自の再試行](https://docs.microsoft.com/azure/architecture/patterns/retry)手法は、最大再試行回数に達するまで、指数関数的に増加する待機時間で再試行することで実装できます。 この手法を使用すると、クラウド リソースが短時間、断続的に使用できなくなる可能性があり、その結果、一部の要求が失敗します。

Azure SQL DB の場合、Entity Framework Core に内部データベース接続の復元機能と再試行ロジックが既に用意されています。 ただし、回復力のある EF Core 接続を使用する場合は、各 DbContext 接続に対して Entity Framework 実行戦略を有効にする必要があります。

たとえば、EF Core 接続レベルで次のコードを実行すると、接続が失敗した場合に再試行される回復力のある SQL 接続が有効になります。

```csharp
// Startup.cs from any ASP.NET Core Web API
public class Startup
{
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        //...
        services.AddDbContext<OrderingContext>(options =>
        {
            options.UseSqlServer(Configuration["ConnectionString"],
            sqlServerOptionsAction: sqlOptions =>
        {
            sqlOptions.EnableRetryOnFailure(
            maxRetryCount: 5,
            maxRetryDelay: TimeSpan.FromSeconds(30),
            errorNumbersToAdd: null);
        });
    });
}
//...
```

#### <a name="execution-strategies-and-explicit-transactions-using-begintransaction-and-multiple-dbcontexts"></a>BeginTransaction と複数の DbContext を使用した実行戦略と明示的なトランザクション

EF Core 接続で再試行を有効にすると、EF Core を使用して実行する各操作は、独自の再試行可能な操作になります。 一時的なエラーが発生した場合、SaveChanges への各クエリと各呼び出しは 1 つのユニットとして再試行されます。

一方、BeginTransaction を使用してトランザクションを開始するコードの場合、1 ユニットとして扱う必要のある独自の操作グループを定義しています。エラーが発生した場合、トランザクション内のすべてがロールバックされる必要があります。 EF 実行戦略 (再試行ポリシー) を使用し、複数の DbContext からの SaveChanges をいくつかトランザクションに含める場合、そのトランザクションを実行しようとすると、次のような例外が表示されます。

System.InvalidOperationException:構成された実行戦略 'SqlServerRetryingExecutionStrategy' は、ユーザーが開始したトランザクションをサポートしていません。 'DbContext.Database.CreateExecutionStrategy()' から返された実行戦略を使用して、再試行可能なユニットとしてトランザクション内のすべての操作を実行します。

この解決策では、実行する必要があるすべてを表すデリゲートを使用して EF 実行戦略を手動で呼び出します。 一時的なエラーが発生した場合、実行戦略によってデリゲートが再び呼び出されます。 次のコードは、この方法を実装する方法を示しています。

```csharp
// Use of an EF Core resiliency strategy when using multiple DbContexts
// within an explicit transaction
// See:
// https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
var strategy = _catalogContext.Database.CreateExecutionStrategy();
await strategy.ExecuteAsync(async () =>
{
    // Achieving atomicity between original Catalog database operation and the
    // IntegrationEventLog thanks to a local transaction
    using (var transaction = _catalogContext.Database.BeginTransaction())
    {
        _catalogContext.CatalogItems.Update(catalogItem);
        await _catalogContext.SaveChangesAsync();

        // Save to EventLog only if product price changed
        if (raiseProductPriceChangedEvent)
            await _integrationEventLogService.SaveEventAsync(priceChangedEvent);
        transaction.Commit();
    }
});
```

最初の DbContext は \_catalogContext で、2 つ目の DbContext は \_integrationEventLogService オブジェクト内にあります。 最後に、Commit アクションが、複数の DbContext で EF 実行戦略を使用して実行されます。

> ### <a name="references--entity-framework-core"></a>参照 – Entity Framework Core
>
> - **EF Core ドキュメント**
>   <https://docs.microsoft.com/ef/>
> - **EF Core: 関連データ**
>   <https://docs.microsoft.com/ef/core/querying/related-data>
> - **ASPNET アプリケーションでのエンティティの遅延読み込みを回避する**
>   <https://ardalis.com/avoid-lazy-loading-entities-in-asp-net-applications>

## <a name="ef-core-or-micro-orm"></a>EF Core または micro-ORM の選択

EF Core は、永続化を管理するための優れた選択肢であり、ほとんどの場合でアプリケーション開発者からデータベースの詳細をカプセル化することができますが、これが唯一の選択肢というわけではありません。 もう 1 つの一般的なオープンソースの代替手段として、[Dapper](https://github.com/StackExchange/Dapper) (いわゆる micro-ORM) もあります。 micro-ORM はデータ構造にオブジェクトをマップするための軽量なツールであり、すべての機能が備わっているわけではありません。 Dapper の場合、その設計目標は、データの取得と更新に使用される基になるクエリの完全なカプセル化ではなく、パフォーマンスに重点を置くことです。 開発者からの SQL が抽象化されないため、Dapper は "機械により近い" ものであり、開発者は特定のデータ アクセス操作で使用する正確なクエリを記述することができます。

EF Core では 2 つの重要な機能が提供されます。これらは Dapper とは異なり、パフォーマンスのオーバーヘッドも増えます。 1 つ目は、LINQ 式から SQL への変換です。 これらの変換はキャッシュされますが、それでも最初の実行時にオーバーヘッドが発生します。 2 つ目は、エンティティの変更追跡です (効率的な更新ステートメントを生成できます)。 AsNotTracking 拡張機能を使用して、この動作を特定のクエリに対してオフにすることができます。 また、EF Core は、通常は非常に効率的で、パフォーマンスの観点から完全に受け入れ可能である場合に SQL クエリを生成します。ただし、実行する正確なクエリを微調整する必要がある場合は、EF Core を使用して、カスタム SQL を渡す (またはストアド プロシージャを実行する) こともできます。 この場合も、Dapper はほんのわずかですが EE Core より優れています。 Julie Lerman は、2016 年 5 月の MSDN 記事「[Dapper、Entity Framework、およびハイブリッド アプリ](https://docs.microsoft.com/archive/msdn-magazine/2016/may/data-points-dapper-entity-framework-and-hybrid-apps)」で一部のパフォーマンス データを提供しています。 さまざまなデータ アクセス方法の追加のパフォーマンス ベンチマーク データについては、[Dapper サイト](https://github.com/StackExchange/Dapper)を参照してください。

EF Core と Dapper との構文の違いを確認するために、アイテム リストを取得する同じ方法の次の 2 つのバージョンについて考えてみます。

```csharp
// EF Core
private readonly CatalogContext _context;
public async Task<IEnumerable<CatalogType>> GetCatalogTypes()
{
    return await _context.CatalogTypes.ToListAsync();
}

// Dapper
private readonly SqlConnection _conn;
public async Task<IEnumerable<CatalogType>> GetCatalogTypesWithDapper()
{
    return await _conn.QueryAsync<CatalogType>("SELECT * FROM CatalogType");
}
```

Dapper でより複雑なオブジェクト グラフを作成する必要がある場合は、(EF Core の場合と同じように Include を追加するのではなく) 関連付けられているクエリを自分で記述する必要があります。 これは、複数のマップされたオブジェクトに個々の行をマップできる、マルチ マッピングという機能を含む、さまざまな構文を通じてサポートされます。 たとえば、User という種類の Owner プロパティを指定した Post クラスの場合、次の SQL では必要なデータがすべて返されます。

```sql
select * from #Posts p
left join #Users u on u.Id = p.OwnerId
Order by p.Id
```

返された各行には、User と Post の両方のデータが含まれます。 User データを Owner プロパティを使用して Post データにアタッチする必要があるため、次の関数が使用されます。

```csharp
(post, user) => { post.Owner = user; return post; }
```

関連付けられているユーザー データが設定された Owner プロパティを使用してポストのコレクションを返すための完全なコード リストは、次のようになります。

```csharp
var sql = @"select * from #Posts p
left join #Users u on u.Id = p.OwnerId
Order by p.Id";
var data = connection.Query<Post, User, Post>(sql,
(post, user) => { post.Owner = user; return post;});
```

あまり優れたカプセル化は提供されないため、Dapper では、開発者がデータの格納方法と、データを効率的にクエリし、さらにコードを記述してフェッチする方法の詳細を把握する必要があります。 モデルが変更された場合、単に新しい移行を作成したり (EE Core の別の機能)、DbContext の 1 つの場所でマッピング情報を更新するのではなく、影響を受けるすべてのクエリを更新する必要があります。 これらのクエリではコンパイル時間が保証されないため、モデルやデータベースの変更に応じて実行時に中断し、エラーの迅速な検出がより困難になる可能性があります。 これらのトレードオフと引き換えに、Dapper では非常に高速なパフォーマンスが提供されます。

ほとんどのアプリケーション、およびほぼすべてのアプリケーションのほとんどの部分に対して、EE Core は許容可能なパフォーマンスを提供します。 したがって、その開発者の生産性の利点が、そのパフォーマンスのオーバーヘッドを上回る可能性があります。 キャッシュから利点を得られるクエリの場合、実際のクエリはごくわずかな時間にのみ実行され、比較的小さなクエリ パフォーマンスの違いが問題になる可能性があります。

## <a name="sql-or-nosql"></a>SQL または NoSQL

従来、SQL Server などのリレーショナル データベースが永続的なデータ記憶域のマーケットプレースの多くを占めますが、使用可能な唯一のソリューションではありません。 [MongoDB](https://www.mongodb.com/what-is-mongodb) などの NoSQL データベースでは、オブジェクトを格納するさまざまな方法が提供されます。 オブジェクトをテーブルと行にマップするのではなく、オブジェクト グラフ全体をシリアル化して、結果を格納することもできます。 この方法の利点は、少なくとも最初は単純さとパフォーマンスとなります。 オブジェクトを、そのオブジェクトがデータベースから最後に取得されてから変更された可能性のある、リレーションシップ、更新、および行を持つ多くのテーブルに分解するよりも、キーを持つ単一のシリアル化されたオブジェクトを格納する方が簡単です。 同様に、キーベースのストアから単一のオブジェクトをフェッチして逆シリアル化することは、リレーショナル データベースから同じオブジェクトを完全に構成するために必要な複雑な結合や複数のデータベース クエリよりも通常ははるかに速くて簡単です。 また、ロックやトランザクションあるいは固定スキーマがないため、NoSQL データベースは多くのコンピューターにわたるスケーリングに適しており、非常に大規模なデータセットをサポートできます。

その一方で、NoSQL データベース (通常は呼び出し時に) には欠点があります。 リレーショナル データベースでは、正規化を使用して整合性を適用し、データの重複を防ぎます。 これにより、データベースの合計サイズが小さくなり、共有データの更新プログラムがデータベース全体ですぐに使用できるようになります。 リレーショナル データベースでは、Address テーブルによって、ID を使用して Country テーブルが参照される場合があります。国/地域の名前が変更された場合、アドレス レコードは更新プログラムの利点が得られ、それ自体の更新は必要ありません。 ただし、NoSQL データベースでは、Address とそれに関連付けられている Country が、格納されている多くのオブジェクトの一部としてシリアル化される可能性があります。 国/地域の名前を更新するには、単一行ではなく、すべてのオブジェクトを更新する必要があります。 リレーショナル データベースでは、外部キーのようなルールを適用することで、リレーショナルの整合性を確認することもできます。 通常、NoSQL データベースでは、そのデータに対するこのような制約は提供されません。

NoSQL データベースで対処する必要があるもう 1 つの複雑なものとして、バージョン管理があります。 オブジェクトのプロパティが変更された場合、格納された過去のバージョンから逆シリアル化できなくなる可能性があります。 したがって、シリアル化された (以前の) バージョンのオブジェクトを持つ既存のすべてのオブジェクトを更新して、新しいスキーマに準拠する必要があります。 これは、スキーマの変更に更新スクリプトとマッピング更新が必要な場合がある、リレーショナル データベースとは概念的に異なります。 ただし、NoSQL の方法では多くの場合、変更する必要があるエントリの数がはるかに増えます。これは、データの重複が増えるためです。

NoSQL データベースには、リレーショナル データベースでは通常サポートされない固定スキーマなど、複数のバージョンのオブジェクトを格納できます。 ただし、その場合、アプリケーション コードでは以前のバージョンのオブジェクトの存在を考慮する必要があり、複雑さが増します。

通常、NoSQL データベースでは [ACID](https://en.wikipedia.org/wiki/ACID) が適用されません。つまり、リレーショナル データベースよりもパフォーマンスとスケーラビリティの両方が優れています。 これは、正規化されたテーブル構造のストレージには適さない非常に大規模なデータセットとオブジェクトに最適です。 単一のアプリケーションでリレーショナル データベースと NoSQL データベースの両方 (それぞれ適宜使用) を利用できない理由はありません。

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB はフル マネージドの NoSQL データベース サービスであり、クラウドベースのスキーマレスなデータ ストレージを提供します。 Azure Cosmos DB は、高速で予測可能なパフォーマンス、高可用性、エラスティック スケーリング、およびグローバル配布用に作成されています。 NoSQL データベースであるにもかかわらず、開発者は JSON データで豊富で使い慣れた SQL クエリ機能を使用できます。 Azure Cosmos DB のすべてのリソースは、JSON ドキュメントとして保存されます。 リソースは、_アイテム_ (メタデータを含むドキュメント) および_フィード_ (アイテムのコレクション) として管理されます。 図 8-2 は、さまざまな Azure Cosmos DB リソース間の関係を示しています。

![Azure Cosmos DB (NoSQL JSON データベース) のリソース間の階層関係](./media/image8-2.png)

**図 8-2** Azure Cosmos DB リソースの編成。

Azure Cosmos DB のクエリ言語は、JSON ドキュメントを照会するためのシンプルで強力なインターフェイスです。 言語では ANSI SQL 文法のサブセットがサポートされ、JavaScript のオブジェクト、配列、オブジェクトの構築、関数呼び出しと緊密に統合できます。

**参照 – Azure Cosmos DB**

- Azure Cosmos DB の概要 <https://docs.microsoft.com/azure/cosmos-db/introduction>

## <a name="other-persistence-options"></a>その他の永続性オプション

リレーショナルおよび NoSQL ストレージ オプションに加え、ASP.NET Core アプリケーションでは Azure Storage を使用して、クラウドベースのスケーラブルな方法でさまざまなデータ形式とファイルを格納できます。 Azure Storage は非常にスケーラブルであるため、最初に少量のデータを格納し、アプリケーションで必要になった場合に数百または数テラバイトのデータを格納するようにスケール アップできます。 Azure Storage では次の 4 つの種類のデータがサポートされます。

- 非構造化テキストまたはバイナリ ストレージ用の Blob Storage。これは、オブジェクト ストレージとも呼ばれます。

- 行キーを使用してアクセス可能な、構造化データセット用の Table Storage。

- 信頼性の高いキューベースのメッセージング用の Queue Storage。

- Azure 仮想マシンとオンプレミス アプリケーション間での共有ファイル アクセス用の File Storage。

**参照 – Azure Storage**

- Azure Storage の概要 <https://docs.microsoft.com/azure/storage/storage-introduction>

## <a name="caching"></a>キャッシュ

Web アプリケーションでは、各 Web 要求をできるだけ短時間に完了する必要があります。 これを実現する 1 つの方法は、サーバーが要求を完了するために行う必要がある、外部呼出しの数を制限することです。 キャッシュでは、サーバー (または、データ ソースよりクエリが簡単な別のデータ ストア) にデータのコピーを格納します。 Web アプリケーション、および特に非 SPA の従来の Web アプリケーションでは、要求ごとにユーザー インターフェイス全体を構築する必要があります。 そのため、ユーザー要求ごとに多くの同じデータベース クエリが何度も繰り返されます。 ほとんどの場合、このデータはめったに変更されないため、データベースから常に要求する理由はほとんどありません。 ASP.NET Core では (ページ全体をキャッシュする) 応答キャッシュおよび (より詳細なキャッシュ動作をサポートする) データ キャッシュがサポートされます。

キャッシュを実装する場合、関心の分離に注意することが重要です。 データ アクセス ロジック、またはユーザー インターフェイスでのキャッシュ ロジックの実装は避けてください。 代わりに、独自のクラスにキャッシュをカプセル化し、その動作を管理する構成を使用します。 これは開放/閉鎖および単一責任の原則に従っており、拡大するアプリケーションでのキャッシュの使用方法を管理しやすくします。

### <a name="aspnet-core-response-caching"></a>ASP.NET Core の応答キャッシュ

ASP.NET Core では、2 つのレベルの応答キャッシュがサポートされます。 最初のレベルではサーバーには何もキャッシュしませんが、応答をキャッシュするようにクライアントおよびプロキシ サーバーに指示する HTTP ヘッダーを追加します。 これは、個々のコントローラーまたはアクションに ResponseCache 属性を追加することで実装されます。

```csharp
[ResponseCache(Duration = 60)]
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";
    return View();
}
```

前の例では、応答に次のヘッダーが追加され、最大で 60 秒間、結果をキャッシュするようクライアントに指示します。

Cache-Control: public,max-age=60

サーバー側のメモリ内キャッシュをアプリケーションに追加するには、Microsoft.AspNetCore.ResponseCaching NuGet パッケージを参照し、応答キャッシュ ミドルウェアを追加する必要があります。 このミドルウェは、Startup の ConfigureServices と Configure の両方で構成されます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCaching();
}

public void Configure(IApplicationBuilder app)
{
    app.UseResponseCaching();
}
```

応答キャッシュ ミドルウェアは、カスタマイズ可能な一連の条件に基づいて、自動的に応答をキャッシュします。 既定では、GET または HEAD メソッドを使用して要求された 200 (OK) の応答のみがキャッシュされます。 さらに、要求には、Cache-Control: public ヘッダーを含む応答が必要であり、Authorization や Set-Cookie のヘッダーを含めることはできません。 [応答キャッシュ ミドルウェアで使用されるキャッシュ条件の完全なリスト](/aspnet/core/performance/caching/middleware#conditions-for-caching)を参照してください。

### <a name="data-caching"></a>データ キャッシュ

完全な Web 応答のキャッシュではなく (または、このようなキャッシュに加えて)、個々のデータ クエリの結果をキャッシュすることができます。 その場合、Web サーバーでメモリ内キャッシュを使用するか、[分散キャッシュ](/aspnet/core/performance/caching/distributed)を使用できます。 このセクションでは、メモリ内キャッシュの実装方法を示します。

次のように、ConfigureServices でメモリ (または分散) キャッシュのサポートを追加します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMemoryCache();
    services.AddMvc();
}
```

必ず、Microsoft.Extensions.Caching.Memory NuGet パッケージも追加してください。

サービスを追加したら、キャッシュにアクセスする必要がある場合は常に、依存関係の挿入を使用して IMemoryCache を要求します。 この例では、CachedCatalogService で、基になる CatalogService 実装へのアクセスを制御する (または動作を追加する) ICatalogService の代替実装を提供して、プロキシ (またはデコレーター) 設計パターンを使用します。

```csharp
public class CachedCatalogService : ICatalogService
{
    private readonly IMemoryCache _cache;
    private readonly CatalogService _catalogService;
    private static readonly string _brandsKey = "brands";
    private static readonly string _typesKey = "types";
    private static readonly TimeSpan _defaultCacheDuration = TimeSpan.FromSeconds(30);
    public CachedCatalogService(IMemoryCache cache,
    CatalogService catalogService)
    {
        _cache = cache;
        _catalogService = catalogService;
    }

    public async Task<IEnumerable<SelectListItem>> GetBrands()
    {
        return await _cache.GetOrCreateAsync(_brandsKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetBrands();
        });
    }

    public async Task<Catalog> GetCatalogItems(int pageIndex, int itemsPage, int? brandID, int? typeId)
    {
        string cacheKey = $"items-{pageIndex}-{itemsPage}-{brandID}-{typeId}";
        return await _cache.GetOrCreateAsync(cacheKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetCatalogItems(pageIndex, itemsPage, brandID, typeId);
        });
    }

    public async Task<IEnumerable<SelectListItem>> GetTypes()
    {
        return await _cache.GetOrCreateAsync(_typesKey, async entry =>
        {
            entry.SlidingExpiration = _defaultCacheDuration;
            return await _catalogService.GetTypes();
        });
    }
}
```

キャッシュされたバージョンのサービスを使用するが、コンストラクターで必要になる CatalogService のインスタンスをサービスが引き続き取得できるようにアプリケーションを構成するには、ConfigureServices に以下を追加します。

```csharp
services.AddMemoryCache();
services.AddScoped<ICatalogService, CachedCatalogService>();
services.AddScoped<CatalogService>();
```

こうすることで、カタログ データをフェッチするデータセット呼び出しは、要求ごとではなく、1 分ごとに 1 回だけ行われるようになります。 サイトへのトラフィックによっては、データベースに対して行われるクエリの数と、このサービスによって公開されている 3 つのクエリすべてに現在依存しているホーム ページの平均ページ読み込み時間に、これが大きな影響を与える可能性があります。

キャッシュの実装時に発生する問題は、"_古いデータ_" です。つまり、ソースで変更されても、その古いバージョンがキャッシュ内に残っているデータです。 この問題を緩和する簡単な方法は、短いキャッシュ期間を使用することです。ビジー状態のアプリケーションの場合、データのキャッシュ期間を延長することで得られる限られた追加の利点があるためです。 たとえば、単一のデータベース クエリを実行し、1 秒ごとに 10 回要求されるページがあるとします。 このページが 1 分間キャッシュされると、1 分ごとに実行されるデータベース クエリの数は 600 から 1 に減り、99.8% の減少率となります。 代わりにキャッシュ期間を 1 時間にした場合、全体的な減少率は 99.997% となりますが、古いデータの確率的および潜在的な経過期間はどちらも大幅に増えます。

含まれているデータを更新する場合、事前にキャッシュ エントリを削除することもできます。 キーがわかっている場合は、個々のエントリを削除できます。

```csharp
_cache.Remove(cacheKey);
```

アプリケーションでキャッシュされているエントリを更新する機能が公開されている場合は、更新を実行するコードの対応するキャッシュ エントリを削除することができます。 特定のデータ セットに依存する多くの異なるエントリが存在する場合もあります。 その場合は、CancellationChangeToken を使用して、キャッシュ エントリ間の依存関係を作成すると便利です。 CancellationChangeToken を使用すれば、トークンをキャンセルすることで一度に複数のキャッシュ エントリを期限切れにすることができます。

```csharp
// configure CancellationToken and add entry to cache
var cts = new CancellationTokenSource();
_cache.Set("cts", cts);
_cache.Set(cacheKey,
itemToCache,
new CancellationChangeToken(cts.Token));

// elsewhere, expire the cache by cancelling the token\
_cache.Get<CancellationTokenSource>("cts").Cancel();
```

キャッシュでは、データベースから同じ値を繰り返し要求する Web ページのパフォーマンスを大幅に向上させることができます。 キャッシュを適用する前に、必ずデータ アクセスとページのパフォーマンスを測定し、改善の必要性があると判断した場合にのみ、キャッシュを適用するようにしてください。 キャッシュでは Web サーバーのメモリ リソースが消費され、アプリケーションの複雑さが増すため、この手法を使用して不完全な最適化を行わないことが重要です。

>[!div class="step-by-step"]
>[前へ](develop-asp-net-core-mvc-apps.md)
>[次へ](test-asp-net-core-mvc-apps.md)
