---
title: 単純なデータ ドリブン CRUD マイクロサービスの作成
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | マイクロサービス アプリケーションのコンテキストでの単純な CRUD (データ ドリブン) マイクロサービスの作成を理解する。
ms.date: 01/30/2020
ms.openlocfilehash: b72d7defed81e57e2971c5e2b53df2d86b2dc947
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77502357"
---
# <a name="creating-a-simple-data-driven-crud-microservice"></a>単純なデータ ドリブン CRUD マイクロサービスの作成

このセクションでは、データ ソースに対して作成、読み取り、更新、削除 (CRUD) 操作を実行する単純なマイクロサービスの作成方法について概説します。

## <a name="designing-a-simple-crud-microservice"></a>単純な CRUD マイクロサービスの設計

設計の観点からは、この種類のコンテナー化されたマイクロサービスはとても単純です。 おそらく、解決する問題は単純であるか、実装は概念実証のみです。

![シンプルな CRUD マイクロサービスの内部設計パターンを示す図。](./media/data-driven-crud-microservice/internal-design-simple-crud-microservices.png)

**図 6-4**。 単純な CRUD マイクロサービスの内部設計

この種の単純なデータ ドリブン サービスの例として、eShopOnContainers サンプル アプリケーションのカタログ マイクロサービスがあります。 この種類のサービスでは、そのデータ モデル、ビジネス ロジック、データ アクセス コードのクラスを含む 1 つの ASP.NET Core Web API プロジェクト内のすべての機能が実装されます。 また、関連データは (開発/テストが目的の別のコンテナーとして) SQL Server で実行されているデータベース内に格納されますが、図 6-5 に示すように、通常の SQL Server ホストにすることもできます。

![データ ドリブン/CRUD マイクロサービス コンテナーを示す図。](./media/data-driven-crud-microservice/simple-data-driven-crud-microservice.png)

**図 6-5**。 単純なデータ ドリブン/CRUD マイクロサービスの設計

前の図では、論理 Catalog マイクロサービスが示されています。そこには、その Catalog データベースが含まれており、Docker ホストは同じ場合もそうでない場合もあります。 同じ Docker ホストにデータベースを置くことは、開発には適しているかもしれませんが、運用の場合はお勧めしません。 この種のサービスを開発するときに必要なのは、[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) と、データ アクセス API または [Entity Framework Core](https://docs.microsoft.com/ef/core/index) のような ORM だけです。 次のセクションで説明するように、[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) を通じて [Swagger](https://swagger.io/) メタデータを自動的に生成して、サービスが提供する内容の説明を提供することもできます。

クラウドまたはオンプレミスでデータベースをプロビジョニングしなくてもすべての依存関係を稼働させることができるので、Docker コンテナー内の SQL Server のようなデータベース サーバーの実行は開発環境に適しています。 これは、統合テストの実行時にとても便利です。 ただし運用環境では、コンテナーでデータベース サーバーを実行すると、通常は高可用性が実現されないため、このアプローチはお勧めしません。 Azure の運用環境では、Azure SQL DB、または高可用性と高スケーラビリティを提供できるその他のデータベース テクノロジを使用することをお勧めします。 たとえば、NoSQL アプローチでは、CosmosDB を選択できます。

最後に、Dockerfile と docker-compose.yml メタデータ ファイルを編集して、このコンテナーのイメージの作成方法 (使用される基本イメージに加えて、内部および外部の名前と TCP ポートなどの設計設定) を構成できます。

## <a name="implementing-a-simple-crud-microservice-with-aspnet-core"></a>ASP.NET Core での単純な CRUD マイクロサービスの実装

.NET Core と Visual Studio を使用して単純な CRUD マイクロサービスを実装するには、図 6-6 に示すように、まず単純な ASP.NET Core Web API プロジェクトを作成します (.NET Core で稼働するため Linux Docker ホスト上で実行できます)。

![プロジェクトの設定が示されている Visual Studio のスクリーンショット。](./media/data-driven-crud-microservice/create-asp-net-core-web-api-project.png)

**図 6-6**。 Visual Studio 2019 での ASP.NET Core Web API プロジェクトの作成

ASP.NET Core Web API プロジェクトを作成するには、最初に ASP.NET Core Web アプリケーションを選択し、次に API の種類を選択します。 プロジェクトを作成した後、Entity Framework API またはその他の API を使用して、他の Web API プロジェクトの場合と同様に MVC コントローラーを実装できます。 新しい Web API プロジェクトでは、そのマイクロサービスの依存関係が ASP.NET Core 自体のみにあることがわかります。 内部的には、*Microsoft.AspNetCore.All* 依存関係内で、図 6-7 に示すように Entity Framework やその他の多くの .NET Core NuGet パッケージを参照しています。

![Catalog.Api の NuGet 依存関係が示されている VS のスクリーンショット。](./media/data-driven-crud-microservice/simple-crud-web-api-microservice-dependencies.png)

**図 6-7**。 単純な CRUD Web API マイクロサービス内の依存関係

API プロジェクトには Microsoft.AspNetCore.App NuGet パッケージの参照が含まれ、そのパッケージには、すべての必須パッケージの参照が含まれています。 その他のパッケージも含まれている場合があります。

### <a name="implementing-crud-web-api-services-with-entity-framework-core"></a>Entity Framework Core での CRUD Web API サービスの実装

Entity Framework (EF) Core は人気の Entity Framework データ アクセス テクノロジの軽量版であり、拡張性に優れ、プラットフォームに依存しません。 EF Core はオブジェクト リレーショナル マッパー (ORM) であり、.NET 開発者は .NET オブジェクトを利用してデータベースを操作できます。

カタログ マイクロサービスは、そのデータベースが Linux Docker イメージ用の SQL Server を持つコンテナーで実行されているため、EF と SQL Server プロバイダーを使用します。 ただし、データベースは、Windows オンプレミスや Azure SQL DB など、任意の SQL Server にデプロイできます。 変更が必要なのは、ASP.NET Web API マイクロサービス内の接続文字列だけです。

#### <a name="the-data-model"></a>データ モデル

EF Core では、データ アクセスはモデルを利用して実行されます。 モデルは (ドメイン モデル) エンティティ クラスと、データベースとのセッションを表す、派生コンテキスト (DbContext) から構成されます。このセッションにより、データのクエリと保存が可能になります。 既存データベースからモデルを生成したり、自分のデータベースに合わせてモデルのコードを手動で記述したり、Code First アプローチを使用して EF 移行を利用してモデルからデータベースを作成したり (モデルが時間の経過と共に変化するのに合わせ、データベースを進化させることが簡単になります) できます。 カタログ マイクロサービスでは、最後のアプローチを使用しています。 次のコード例には、CatalogItem エンティティ クラスの例があります。これは単純な従来の CLR オブジェクト ([POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) エンティティ クラスです。

```csharp
public class CatalogItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public string PictureFileName { get; set; }
    public string PictureUri { get; set; }
    public int CatalogTypeId { get; set; }
    public CatalogType CatalogType { get; set; }
    public int CatalogBrandId { get; set; }
    public CatalogBrand CatalogBrand { get; set; }
    public int AvailableStock { get; set; }
    public int RestockThreshold { get; set; }
    public int MaxStockThreshold { get; set; }

    public bool OnReorder { get; set; }
    public CatalogItem() { }

    // Additional code ...
}
```

データベースとのセッションを表す DbContext も必要です。 カタログ マイクロサービスでは、CatalogContext クラスは次の例で示すように DbContext 基底クラスから派生します。

```csharp
public class CatalogContext : DbContext
{
    public CatalogContext(DbContextOptions<CatalogContext> options) : base(options)
    { }
    public DbSet<CatalogItem> CatalogItems { get; set; }
    public DbSet<CatalogBrand> CatalogBrands { get; set; }
    public DbSet<CatalogType> CatalogTypes { get; set; }

    // Additional code ...
}
```

追加の `DbContext` 実装も使用できます。 たとえば、サンプルの Catalog.API マイクロサービスには `CatalogContextSeed` という名前の 2 番目の `DbContext` があり、データベースに初めてアクセスしようとしたときにサンプル データが自動的に設定されます。 このメソッドは、デモ データと自動テストのシナリオにも役立ちます。

`DbContext` 内で、`OnModelCreating` メソッドを使用し、オブジェクト/データベース エンティティのマッピングとその他の [EF 機能拡張ポイント](https://devblogs.microsoft.com/dotnet/implementing-seeding-custom-conventions-and-interceptors-in-ef-core-1-0/)をカスタマイズします。

##### <a name="querying-data-from-web-api-controllers"></a>Web API コントローラーからのデータのクエリ

エンティティ クラスのインスタンスは、次の例に示すように、通常は統合言語クエリ (LINQ) を利用してデータベースから取得されます。

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _catalogContext;
    private readonly CatalogSettings _settings;
    private readonly ICatalogIntegrationEventService _catalogIntegrationEventService;

    public CatalogController(
        CatalogContext context,
        IOptionsSnapshot<CatalogSettings> settings,
        ICatalogIntegrationEventService catalogIntegrationEventService)
    {
        _catalogContext = context ?? throw new ArgumentNullException(nameof(context));
        _catalogIntegrationEventService = catalogIntegrationEventService
            ?? throw new ArgumentNullException(nameof(catalogIntegrationEventService));

        _settings = settings.Value;
        context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
    }

    // GET api/v1/[controller]/items[?pageSize=3&pageIndex=10]
    [HttpGet]
    [Route("items")]
    [ProducesResponseType(typeof(PaginatedItemsViewModel<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType(typeof(IEnumerable<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    public async Task<IActionResult> ItemsAsync(
        [FromQuery]int pageSize = 10,
        [FromQuery]int pageIndex = 0,
        string ids = null)
    {
        if (!string.IsNullOrEmpty(ids))
        {
            var items = await GetItemsByIdsAsync(ids);

            if (!items.Any())
            {
                return BadRequest("ids value invalid. Must be comma-separated list of numbers");
            }

            return Ok(items);
        }

        var totalItems = await _catalogContext.CatalogItems
            .LongCountAsync();

        var itemsOnPage = await _catalogContext.CatalogItems
            .OrderBy(c => c.Name)
            .Skip(pageSize * pageIndex)
            .Take(pageSize)
            .ToListAsync();

        itemsOnPage = ChangeUriPlaceholder(itemsOnPage);

        var model = new PaginatedItemsViewModel<CatalogItem>(
            pageIndex, pageSize, totalItems, itemsOnPage);

        return Ok(model);
    }
    //...
}
```

##### <a name="saving-data"></a>データの保存

データはエンティティ クラスのインスタンスを利用し、データベース内で作成、削除、変更されます。 次のハード コーディングされた例のようなコード (この場合はモック データ) を Web API コントローラーに追加できます。

```csharp
var catalogItem = new CatalogItem() {CatalogTypeId=2, CatalogBrandId=2,
                                     Name="Roslyn T-Shirt", Price = 12};
_context.Catalog.Add(catalogItem);
_context.SaveChanges();
```

##### <a name="dependency-injection-in-aspnet-core-and-web-api-controllers"></a>ASP.NET Core と Web API コントローラーでの依存関係の挿入

ASP.NET Core では、既定の依存関係の挿入 (DI) を使用できます。 優先 IoC コンテナーを ASP.NET Core インフラストラクチャに接続することもできますが、サード パーティ製の制御の反転 (IoC) コンテナーを設定する必要はありません。 この場合、必須の EF DBContext またはコントローラー コンストラクターを通じた追加のリポジトリを直接挿入できることを意味します。

上記の `CatalogController` クラスの例では、`CatalogController()` コンストラクターを通じて `CatalogContext` 型のオブジェクトやその他のオブジェクトを挿入しています。

Web API プロジェクトを設定するための重要な構成は、サービスの IoC コンテナーへの DbContext クラスの登録です。 通常、この操作は、次の**簡略化された**例に示すように、`ConfigureServices()` メソッド内で `services.AddDbContext<DbContext>()` メソッドを呼び出して `Startup` クラス内で実行します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Additional code...

    services.AddDbContext<CatalogContext>(options =>
    {
        options.UseSqlServer(Configuration["ConnectionString"],
        sqlServerOptionsAction: sqlOptions =>
        {
            sqlOptions.MigrationsAssembly(
                typeof(Startup).GetTypeInfo().Assembly.GetName().Name);

           //Configuring Connection Resiliency:
           sqlOptions.
               EnableRetryOnFailure(maxRetryCount: 5,
               maxRetryDelay: TimeSpan.FromSeconds(30),
               errorNumbersToAdd: null);

       });

       // Changing default behavior when client evaluation occurs to throw.
       // Default in EFCore would be to log warning when client evaluation is done.
       options.ConfigureWarnings(warnings => warnings.Throw(
           RelationalEventId.QueryClientEvaluationWarning));
   });

  //...
}
```

### <a name="additional-resources"></a>その他の技術情報

- **データのクエリ** \
  [https://docs.microsoft.com/ef/core/querying/index](/ef/core/querying/index)

- **データの保存** \
  [https://docs.microsoft.com/ef/core/saving/index](/ef/core/saving/index)

## <a name="the-db-connection-string-and-environment-variables-used-by-docker-containers"></a>Docker コンテナーで使用される DB 接続文字列と環境変数

次の例に示すように、ASP.NET Core 設定を使用でき、ConnectionString プロパティを settings.json ファイルに追加できます。

```json
{
    "ConnectionString": "Server=tcp:127.0.0.1,5433;Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word",
    "ExternalCatalogBaseUrl": "http://localhost:5101",
    "Logging": {
        "IncludeScopes": false,
        "LogLevel": {
            "Default": "Debug",
            "System": "Information",
            "Microsoft": "Information"
        }
    }
}
```

settings.json ファイルには、ConnectionString プロパティやその他のプロパティの既定値を格納できます。 ただし、Docker を使用する場合、これらのプロパティは docker-compose.override.yml ファイルで指定した環境変数の値でオーバーライドされます。

次の docker-compose.override.yml ファイルに示すように、docker-compose.yml または docker-compose.override.yml ファイルからこれらの環境変数を初期化して、Docker がそれらを OS 環境変数として設定するようにすることができます (この例では接続文字列とその他の行が折り返されていますが、実際のファイルでは折り返されません)。

```yml
# docker-compose.override.yml

#
catalog-api:
  environment:
    - ConnectionString=Server=sqldata;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word
    # Additional environment variables for this service
  ports:
    - "5101:80"
```

ソリューション レベルでの docker-compose.yml ファイルは、プロジェクトまたはマイクロサービス レベルの構成ファイルよりも柔軟性が高いだけでなく、docker-compose ファイルで宣言されている環境変数を Azure DevOps Services Docker デプロイ タスクからの値のようなデプロイ ツールで設定された値でオーバーライドするとセキュリティが向上します。

最後に、前のコード例の ConfigureServices メソッドで示されているように、構成 \["ConnectionString"\] を使用してコードから値を取得できます。

ただし、運用環境では、接続文字列などのシークレットを格納する方法について、追加の方法を調べることが必要な場合があります。 アプリケーション シークレットを管理する優れた方法は [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) を使用することです。

Azure Key Vault は、暗号化キーと、クラウド アプリケーションやサービスで使用されるシークレットを保管したり、保護したりするのに役立ちます。 API キー、接続文字列、パスワードなど、厳重に管理するあらゆるものがシークレットです。厳重な管理としては、使用を記録したり、有効期限を設定したり、アクセスを管理したり*します*。

Azure Key Vault を利用することで、誰にも知らせる必要なく、アプリケーション シークレットの使用を非常に細かく制御できます。 シークレットは一定の周期で再利用し、開発や運用を妨げることなくセキュリティを強化できます。

アプリケーションで Azure Key Vault を使用するには、組織の Active Directory に登録する必要があります。

詳細については、*Azure Key Vault の概念に関するドキュメント*をご覧いただけます。

### <a name="implementing-versioning-in-aspnet-web-apis"></a>ASP.NET Web API でのバージョン管理の実装

ビジネス要件が変わると、リソースの新しいコレクションが追加され、リソース間の関係が変更され、リソース内のデータの構造が修正される可能性があります。 新しい要件を処理する Web API の更新は比較的簡単なプロセスですが、このような変更が Web API を使用するクライアント アプリケーションに与える影響を考慮する必要があります。 Web API を設計および実装する開発者はその API を完全に制御できますが、遠隔地で操業しているサード パーティ組織が構築した可能性のあるクライアント アプリケーションを同じように制御することはできません。

バージョン管理によって、Web API が公開する機能とリソースを指定することができます。 クライアント アプリケーションは、特定のバージョンの機能やリソースに要求を送信できます。 バージョン管理を実装するアプローチはいくつかあります。

- URI バージョン管理

- クエリ文字列バージョン管理

- ヘッダー バージョン管理

クエリ文字列および URI バージョン管理は、実装するのが最も簡単です。 ヘッダー バージョン管理は適切なアプローチです。 ただし、ヘッダー バージョン管理は URI バージョン管理ほど明示的でも簡単でもありません。 URL バージョン管理は最も単純で最も明示的なので、eShopOnContainers サンプル アプリケーションでは URI バージョン管理を使用します。

eShopOnContainers サンプル アプリケーションのように、URI バージョン管理では、Web API を変更したりリソースのスキーマを変更したりするたびに、各リソースの URI にバージョン番号が追加されます。 既存の URI はそれまでと同様に動作を続け、要求されたバージョンに対応するスキーマに準拠したリソースを返します。

次のコード例に示すように、バージョンは Web API コントローラーの Route 属性を使用して設定できます。それにより URI でバージョン (このケースでは v1) が明示的になります。

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    // Implementation ...
```

このバージョン管理メカニズムは単純で、要求を適切なエンドポイントにルーティングするサーバーに依存します。 ただし、より高度なバージョン管理と REST を使用するときに最適な方法としては、ハイパーメディアを使用し、[HATEOAS (Hypertext as the Engine of Application State)](https://docs.microsoft.com/azure/architecture/best-practices/api-design#use-hateoas-to-enable-navigation-to-related-resources) を実装する必要があります。

### <a name="additional-resources"></a>その他の技術情報

- **Scott Hanselman。簡単になった ASP.NET Core RESTful Web API のバージョン管理** \
  <https://www.hanselman.com/blog/ASPNETCoreRESTfulWebAPIVersioningMadeEasy.aspx>

- **RESTful Web API のバージョン管理** \
  <https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api>

- **Roy Fielding。バージョン管理、ハイパーメディア、REST** \
  <https://www.infoq.com/articles/roy-fielding-on-versioning>

## <a name="generating-swagger-description-metadata-from-your-aspnet-core-web-api"></a>ASP.NET Core Web API からの Swagger 記述メタデータの生成

[Swagger](https://swagger.io/) は一般に使用されるオープン ソース フレームワークであり、RESTful API の設計、構築、文書化、使用に役立つツールの大規模なエコシステムによってサポートされています。 API 記述メタデータ ドメインの標準になりつつあります。 Swagger 記述メタデータを任意の種類のマイクロサービス (次のセクションで説明するデータ ドリブン マイクロサービスまたはより高度なドメイン ドリブン マイクロサービス) と共に含める必要があります。

Swagger の中心となるのは、JSON または YAML ファイル内の API 記述メタデータである Swagger 仕様です。 仕様では、API の RESTful コントラクトを作成し、開発、検出、統合を簡単にするためにすべてのリソースと操作を人間とマシンの両方が判読できる形式で詳しく記述します。

仕様は、OpenAPI 仕様 (OAS) の基盤であり、RESTful インターフェイスを定義する方法を標準化するためにオープンで透過的なコラボレーション コミュニティで開発されています。

仕様では、サービスの検出方法とその機能を理解する方法に関する構造体を定義します。 Web エディターや、Spotify、Uber、Slack、Microsoft などの企業の Swagger 仕様の例については、Swagger のサイト (<https://swagger.io>) をご覧ください。

### <a name="why-use-swagger"></a>Swagger を使用する理由

API 用の Swagger メタデータを生成する主な理由は次のとおりです。

**他の製品が API を自動的に使用して統合できる**。 数十個の製品と[商用ツール](https://swagger.io/commercial-tools/)、多くの[ライブラリとフレームワーク](https://swagger.io/open-source-integrations/)で Swagger がサポートされます。 Microsoft には、Swagger ベースの API を自動的に使用できる次のような高度な製品とツールがあります。

- [AutoRest](https://github.com/Azure/AutoRest)。 Swagger を呼び出すための .NET クライアント クラスを自動的に生成することができます。 このツールは CLI から使用することができ、GUI から簡単に使用できるように Visual Studio とも統合されています。

- [Microsoft Flow](https://flow.microsoft.com/)。 自動的に [API 使用し、高レベルの Microsoft Flow ワークフローと統合](https://flow.microsoft.com/blog/integrating-custom-api/)できます。プログラミング スキルは不要です。

- [Microsoft PowerApps](https://powerapps.microsoft.com/)。 [PowerApps Studio](https://powerapps.microsoft.com/build-powerapps/) で作成された [PowerApps モバイル アプリ](https://powerapps.microsoft.com/blog/register-and-use-custom-apis-in-powerapps/)から API を自動的に使用できます。プログラミング スキルは不要です。

- [Azure App Service Logic Apps](https://docs.microsoft.com/azure/app-service-logic/app-service-logic-what-are-logic-apps)。 自動的に [API を使用でき、Azure App Service Logic App に統合](https://docs.microsoft.com/azure/app-service-logic/app-service-logic-custom-hosted-api)できます。プログラミング スキルは不要です。

**API のドキュメントを自動的に生成できる**。 複雑なマイクロサービス ベースのアプリケーションなど、大規模な RESTful API を作成する場合は、要求と応答のペイロードで使用されるさまざまなデータ モデルで多数のエンドポイントを処理する必要があります。 Swagger で利用できるような適切なドキュメントと堅牢な API Explorer があることは、API の成功と開発者による採用の鍵となります。

Swagger のメタデータは、Microsoft Flow、PowerApps、Azure Logic Apps が API の使用方法と API への接続方法を理解するために使用するものです。

機能 API のヘルプ ページのフォームでは、ASP.NET Core REST API アプリケーションの Swagger メタデータ生成はいくつかの方法で自動化でき、*swagger-ui* によって異なります。

最もよく知られている方法はおそらく、[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) です。これは現在、[eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) で使用されています。これについてはこのガイドで取り上げますが、[NSwag](https://github.com/RSuter/NSwag) を使用するという選択肢もあります。その場合、Swagger または OpenAPI の仕様から、さらには [NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio) を使用し、コントローラーを含む .dll をスキャンするという方法によって、Typescript、C\# API クライアント、C\# コントローラーを生成できます。

### <a name="how-to-automate-api-swagger-metadata-generation-with-the-swashbuckle-nuget-package"></a>Swashbuckle NuGet パッケージで API Swagger メタデータの生成を自動化する方法

手動での (JSON または YAML ファイルでの) Swagger メタデータの生成は面倒な作業となる場合があります。 ただし、[Swashbuckle NuGet パッケージ](https://aka.ms/swashbuckledotnetcore)を使用して Swagger API メタデータを動的に生成して、ASP.NET Web API サービスの API 検出を自動化することができます。

Swashbuckle では、ASP.NET Web API プロジェクトの Swagger メタデータが自動的に生成されます。 ASP.NET Core Web API プロジェクトと従来の ASP.NET Web API、さらに Azure API アプリ、Azure Mobile App、ASP.NET に基づく Azure Service Fabric マイクロサービスなどのその他のフレーバーがサポートされます。 また、参照アプリケーションの場合と同様に、コンテナーにデプロイされている単純な Web API もサポートされます。

Swashbuckle は API Explorer と Swagger または [swagger-ui](https://github.com/swagger-api/swagger-ui) を結合して API コンシューマーに充実した検出およびドキュメント エクスペリエンスを提供します。 Swagger メタデータ ジェネレーター エンジンに加えて、Swashbuckle には、Swashbuckle のインストール後に自動的に提供される埋め込みバージョンの swagger-ui も含まれています。

これは、優れた検出 UI で API を補完し、開発者による API の使用を支援できることを意味します。 これは自動的に生成されるため、必要なコードとメンテナンスの量がとても少なく、API の構築に専念することができます。 API Explorer の結果は、図 6-8 のようになります。

![eShopOContainers API を表示している Swagger API Explorer のスクリーンショット。](./media/data-driven-crud-microservice/swagger-metadata-eshoponcontainers-catalog-microservice.png)

**図 6-8**。 Swagger メタデータに基づく Swashbuckle API Explorer - eShopOnContainers カタログ マイクロサービス

Swashbuckle で生成された Swagger UI API ドキュメントには、公開されたすべてのアクションが含まれます。 API Explorer は、ここで最も重要なものではありません。 Swagger メタデータで自身を記述できる Web API があると、多くのプラットフォームを対象とするクライアント プロキシ クラス コード ジェネレーターなど、Swagger ベースのツールから API をシームレスに使用できます。 たとえば、前に説明したように、[AutoRest](https://github.com/Azure/AutoRest) では .NET クライアント クラスが自動的に生成されます。 ただし、API クライアント ライブラリ、サーバー スタブ、ドキュメントのコード生成を自動化できる [swagger-codegen](https://github.com/swagger-api/swagger-codegen) のようなツールも利用できます。

現時点では、Swashbuckle は、ASP.NET Core アプリケーション用の [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore) のハイレベルのメタ パッケージ下の 5 つの内部 NuGet パッケージから構成されます。

Web API プロジェクトでこれらの NuGet パッケージをインストールした後、次の**簡略化された**コードのように、Startup クラスで Swagger を構成する必要があります。

```csharp
public class Startup
{
    public IConfigurationRoot Configuration { get; }
    // Other startup code...

    public void ConfigureServices(IServiceCollection services)
    {
        // Other ConfigureServices() code...

        // Add framework services.
        services.AddSwaggerGen(options =>
        {
            options.DescribeAllEnumsAsStrings();
            options.SwaggerDoc("v1", new OpenApiInfo
            {
                Title = "eShopOnContainers - Catalog HTTP API",
                Version = "v1",
                Description = "The Catalog Microservice HTTP API. This is a Data-Driven/CRUD microservice sample"
            });
        });

        // Other ConfigureServices() code...
    }

    public void Configure(IApplicationBuilder app,
        IHostingEnvironment env,
        ILoggerFactory loggerFactory)
    {
        // Other Configure() code...
        // ...
        app.UseSwagger()
            .UseSwaggerUI(c =>
            {
                c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
            });
    }
}
```

これが完了したら、アプリケーションを起動し、次のような URL を使用して次の Swagger JSON および UI エンドポイントを参照することができます。

```console
  http://<your-root-url>/swagger/v1/swagger.json

  http://<your-root-url>/swagger/
```

`http://<your-root-url>/swagger` のような URL に対して Swashbuckle で作成された生成済みの UI を前に参照しました。 図 6-9 では、API メソッドをテストする方法も確認できます。

![使用可能なテスト ツールを示す Swagger UI のスクリーンショット。](./media/data-driven-crud-microservice/swashbuckle-ui-testing.png)

**図 6-9**。 カタログ/アイテム API メソッドをテストする Swashbuckle UI

Swagger UI の API 詳細画面に応答のサンプルが表示されています。実際の API を実行する際に利用でき、開発者にとって検出に役立ちます。 図 6-10 は、[Postman](https://www.getpostman.com/) を使用して `http://<your-root-url>/swagger/v1/swagger.json` を要求したときに eShopOnContainers マイクロサービス (これは、ツールが表面下で使用するものです) から生成された Swagger JSON メタデータを示しています。

![Swagger JSON メタデータを示す Postman UI のサンプルのスクリーンショット。](./media/data-driven-crud-microservice/swagger-json-metadata.png)

**図 6-10**。 Swagger JSON メタデータ

これは単純です。 自動的に生成されるため、API に機能を追加すると Swagger メタデータが拡張されます。

### <a name="additional-resources"></a>その他の技術情報

- **Swagger を使用する ASP.NET Web API のヘルプ ページ** \
  [https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger)

- **Swashbuckle と ASP.NET Core の概要** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle](/aspnet/core/tutorials/getting-started-with-swashbuckle)

- **NSwag と ASP.NET Core の概要** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-nswag](/aspnet/core/tutorials/getting-started-with-nswag)

> [!div class="step-by-step"]
> [前へ](microservice-application-design.md)
> [次へ](multi-container-applications-docker-compose.md)
