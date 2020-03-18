---
title: 永続インフラストラクチャとして NoSQL データベースを使用する
description: 永続性実装のオプションとしての NoSql データベースの一般的な使用と、特に Azure Cosmos DB の使用について理解します。
ms.date: 01/30/2020
ms.openlocfilehash: 7da4141d9aadc4aaa265ac97d328bc4b7569a0cb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77502395"
---
# <a name="use-nosql-databases-as-a-persistence-infrastructure"></a>永続インフラストラクチャとして NoSQL データベースを使用する

インフラストラクチャ データ層に NoSQL データベースを使用する場合、通常は Entity Framework Core のような ORM は使用しません。 代わりに、Azure Cosmos DB、MongoDB、Cassandra、RavenDB、CouchDB、または Azure Storage Table などの NoSQL エンジンによって提供される API を使用します。

ただし、NoSQL データベース、特にドキュメント指向データベース (Azure Cosmos DB、CouchDB、RavenDB など) を使用する場合、集約ルート、子エンティティ クラス、および値オブジェクト クラスに関しては、DDD 集約を使用してモデルを設計する方法は、EF Core での方法と部分的に似ています。 しかし、最終的には、データベースの選択が設計に影響を与えます。

ドキュメント指向データベースを使用する場合は、JSON または別の形式でシリアル化された 1 つのドキュメントとして集約を実装します。 ただし、データベースの使用は、ドメイン モデルのコードの観点からは透過的です。 NoSQL データベースを使用する場合も、エンティティ クラスと集約ルート クラスを引き続き使用しますが、永続化がリレーショナルではないため、EF Core を使用するときよりも高い柔軟性が得られます。

違いは、そのモデルを永続化する方法にあります。 インフラストラクチャの永続性に依存しない、POCO エンティティ クラスに基づいてドメイン モデルを実装した場合、リレーショナルから NoSQL に移行しても、別の永続化インフラストラクチャに移行したように見える場合があります。 しかし、これを目標とはしないでください。 各種のデータベース技術には常に制約や妥協があり、リレーショナルまたは NoSQL データベースに同じモデルを持つことはできません。 永続化モデルの変更は、トランザクションと永続化の操作がかなり異なるため、簡単な作業ではありません。

たとえば、ドキュメント指向のデータベースでは、複数の子コレクション プロパティを持つためにルートを集約することができます。 リレーショナル データベースでは、複数の子コレクション プロパティへのクエリ実行は簡単に最適化されません。UNION ALL SQL ステートメントが EF から返されるためです。 リレーショナル データベースと NoSQL データベースに対して同じドメイン モデルを持つのは単純なことではないので、行わないでください。 データがそれぞれ特定のデータベースでどのように使用されるかを理解したうえで、モデルを設計する必要があります。

NoSQL データベースを使用する場合の利点は、エンティティがより非正規化されるため、テーブル マッピングを設定しなくてもよい点です。 リレーショナル データベースを使用するときよりも、ドメイン モデルに柔軟性を持たせることができます。

集約に基づいてドメイン モデルを設計すると、NoSQL とドキュメント指向データベースへの移行が、リレーショナル データベースを使用するよりもさらに簡単になる場合があります。これは、設計する集約が、ドキュメント指向データベース内のシリアル化されたドキュメントと似ているからです。 その後、これらの "バッグ" にその集約に必要なすべての情報を含めることができます。

たとえば、次の JSON コードは、ドキュメント指向データベースを使用するときの、注文集約のサンプル実装です。 これは、eShopOnContainers サンプルで実装した注文集約と似ていますが、下に EF Core を使用していません。

```json
{
    "id": "2017001",
    "orderDate": "2/25/2017",
    "buyerId": "1234567",
    "address": [
        {
        "street": "100 One Microsoft Way",
        "city": "Redmond",
        "state": "WA",
        "zip": "98052",
        "country": "U.S."
        }
    ],
    "orderItems": [
        {"id": 20170011, "productId": "123456", "productName": ".NET T-Shirt",
        "unitPrice": 25, "units": 2, "discount": 0},
        {"id": 20170012, "productId": "123457", "productName": ".NET Mug",
        "unitPrice": 15, "units": 1, "discount": 0}
    ]
}
```

## <a name="introduction-to-azure-cosmos-db-and-the-native-cosmos-db-api"></a>Azure Cosmos DB とネイティブ Cosmos DB API の概要

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) は、ミッション クリティカルなアプリケーションのために、世界各地に分散された Microsoft のデータベース サービスです。 Azure Cosmos DB では、[ターン キー グローバル配布](https://docs.microsoft.com/azure/cosmos-db/distribute-data-globally)、世界規模での[スループットとストレージのエラスティック スケーリング](https://docs.microsoft.com/azure/cosmos-db/partition-data)、99 パーセンタイルで 10 ミリ秒未満の待機時間、[5 つの明確に定義された整合性レベル](https://docs.microsoft.com/azure/cosmos-db/consistency-levels)、および保証された高可用性を提供しており、これらすべてが[業界トップの SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) でサポートされています。 Azure Cosmos DB により、[データが自動的にインデックス付けされる](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)ため、スキーマとインデックスの管理に対処する必要がなくなります。 これはマルチモデルで、ドキュメント、キー値、グラフ、および列指向データ モデルをサポートします。

![Azure Cosmos DB のグローバル配布を示す図。](./media/nosql-database-persistence-infrastructure/azure-cosmos-db-global-distribution.png)

**図 7-19** Azure Cosmos DB のグローバル配布

C\# モデルを使用して Azure Cosmos DB API で使用される集約を実装すると、集約が EF Core で使用される C\# POCO クラスと同じようになる場合があります。 違いは、次のコードのように、それらをアプリケーション層とインフラストラクチャ層から使用する方法にあります。

```csharp
// C# EXAMPLE OF AN ORDER AGGREGATE BEING PERSISTED WITH AZURE COSMOS DB API
// *** Domain Model Code ***
// Aggregate: Create an Order object with its child entities and/or value objects.
// Then, use AggregateRoot’s methods to add the nested objects so invariants and
// logic is consistent across the nested properties (value objects and entities).

Order orderAggregate = new Order
{
    Id = "2017001",
    OrderDate = new DateTime(2005, 7, 1),
    BuyerId = "1234567",
    PurchaseOrderNumber = "PO18009186470"
}

Address address = new Address
{
    Street = "100 One Microsoft Way",
    City = "Redmond",
    State = "WA",
    Zip = "98052",
    Country = "U.S."
}

orderAggregate.UpdateAddress(address);

OrderItem orderItem1 = new OrderItem
{
    Id = 20170011,
    ProductId = "123456",
    ProductName = ".NET T-Shirt",
    UnitPrice = 25,
    Units = 2,
    Discount = 0;
};

//Using methods with domain logic within the entity. No anemic-domain model
orderAggregate.AddOrderItem(orderItem1);
// *** End of Domain Model Code ***

// *** Infrastructure Code using Cosmos DB Client API ***
Uri collectionUri = UriFactory.CreateDocumentCollectionUri(databaseName,
    collectionName);

await client.CreateDocumentAsync(collectionUri, orderAggregate);

// As your app evolves, let's say your object has a new schema. You can insert
// OrderV2 objects without any changes to the database tier.
Order2 newOrder = GetOrderV2Sample("IdForSalesOrder2");
await client.CreateDocumentAsync(collectionUri, newOrder);
```

ドメイン モデルを操作する方法が、インフラストラクチャが EF のときにドメイン モデル レイヤーで使用するのと同様の方法になる可能性があることがわかります。 集約内の整合性、インバリアント、および検証を確認するため、同じ集約ルート メソッドを引き続き使用します。

ただし、モデルを NoSQL データベースに永続化する場合は、EF Core コードやリレーショナル データベースに関連するその他のコードと比べて、コードと API は劇的に変わります。

## <a name="implement-net-code-targeting-mongodb-and-azure-cosmos-db"></a>MongoDB と Azure Cosmos DB をターゲットとする .NET コードを実装する

### <a name="use-azure-cosmos-db-from-net-containers"></a>.NET コンテナーから Azure Cosmos DB を使用する

他の .NET アプリケーションからアクセスする場合と同じように、コンテナーで実行されている .NET コードから Azure Cosmos DB データベースにアクセスできます。 たとえば、eShopOnContainers の Locations.API と Marketing.API のマイクロサービスは、Azure Cosmos DB データベースを使用できるように実装されています。

ただし、Docker 開発環境の観点から、Azure Cosmos DB には制限があります。 ローカルの開発用コンピューターで実行できるオンプレミスの [Azure Cosmos DB エミュレーター](https://docs.microsoft.com/azure/cosmos-db/local-emulator)がある場合でも、Windows のみがサポートされます。 Linux と macOS はサポートされません。

このエミュレーターを Docker で実行する可能性もありますが、可能性があるのは Windows コンテナーのみであり、Linux コンテナーではありません。 現在、Docker for Windows に Linux コンテナーと Windows コンテナーを同時に展開することはできないため、アプリケーションを Linux コンテナーとして展開する場合、これが開発環境にとって最初のハンディキャップとなります。 展開されるすべてのコンテナーは、Linux または Windows 用のどちらかにする必要があります。

開発およびテスト ソリューションにとって理想的でより簡単な展開は、開発およびテスト環境に常に整合性を持たせるために、データベース システムをカスタム コンテナーと共にコンテナーとして展開できることです。

### <a name="use-mongodb-api-for-local-devtest-linuxwindows-containers-plus-azure-cosmos-db"></a>Linux/Windows コンテナーと Azure Cosmos DB のローカル開発/テストに MongoDB API を使用する

Cosmos DB データベースでは、.NET とネイティブ MongoDB ワイヤ プロトコルのために MongoDB API をサポートしています。 つまり、既存のドライバーを使用することで、図 7-20 に示すように、MongoDB 用に記述されたアプリケーションで Cosmos DB と通信し、MongoDB データベースの代わりに Cosmos DB データベースを使用できるようになります。

![Cosmos DB で .NET と MongoDB のワイヤ プロトコルをサポートしていることを示す図。](./media/nosql-database-persistence-infrastructure/mongodb-api-wire-protocol.png)

**図 7-20** MongoDB API とプロトコルを使用して Azure Cosmos DB にアクセスする

[MongoDB Docker イメージ](https://hub.docker.com/r/_/mongo/)は、Docker Linux コンテナーと Docker Windows コンテナーをサポートするマルチアーキテクチャ イメージなので、これは Linux コンテナーを使用する Docker 環境での概念実証にとって非常に便利な方法です。

次の画像に示すように、MongoDB API を使用することで、eShopOnContainers でローカル開発環境に対して MongoDB Linux コンテナーと Windows コンテナーの両方がサポートされますが、その後、[MongoDB の接続文字列を Azure Cosmos DB をポイントするように変更](https://docs.microsoft.com/azure/cosmos-db/connect-mongodb-account)するだけで、Azure Cosmos DB と同じようなスケーラブルな PaaS クラウド ソリューションに移行できます。

![eShopOnContainers の Location マイクロサービスで Cosmos DB または Mongo DB を使用できることを示す図。](./media/nosql-database-persistence-infrastructure/eshoponcontainers-mongodb-containers.png)

**図 7-21** 開発環境に MongoDB コンテナーまたは運用に Azure Cosmos DB を使用する eShopOnContainers

実稼働の Azure Cosmos DB は、PaaS およびスケーラブルなサービスとして Azure のクラウドで実行されます。

.NET Core のカスタム コンテナーは、(Windows 10 コンピューターで Docker for Windows を使用して) ローカルの Docker の開発用ホストで実行することも、Azure AKS または Azure Service Fabric での Kubernetes のように、運用環境に展開することもできます。 この 2 つ目の環境では、稼働中のデータを処理するため、クラウド内で Azure Cosmos DB を使用しているため、.NET Core のカスタム コンテナーのみを展開し、MongoDB コンテナーは展開しません。

MongoDB API を使用することの明らかな利点は、MongoDB と Azure Cosmos DB の両方のデータベース エンジンでソリューションを実行できるため、異なる環境への移行が容易になることです。 ただし、特定のデータベース エンジンの機能をフル活用するために、ネイティブ API (ネイティブ Cosmos DB API) を使用することには意義がある場合があります。

単純に MongoDB を使用することとクラウドでの Cosmos DB との詳しい比較については、[Azure Cosmos DB を使用する利点に関するページ](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)を参照してください。

### <a name="analyze-your-approach-for-production-applications-mongodb-api-vs-cosmos-db-api"></a>実稼働アプリケーションのアプローチを分析する:MongoDB API とCosmos DB API

Microsoft の優先度が根本的に Azure Cosmos DB でも動作する NoSQL データベースを使用して一貫性のある開発/テスト環境を持つことだったため、eShopOnContainers では MongoDB API を使用しています。

ただし、MongoDB API を使用して、実稼働アプリケーションの Azure で Azure Cosmos DB にアクセスするように計画している場合は、ネイティブ Azure Cosmos DB API を使用した場合と比較して、MongoDB API を使用して Azure Cosmos DB データベースにアクセスした場合の機能とパフォーマンスの違いを分析する必要があります。 同様の場合は、MongoDB API を使用でき、同時に 2 つの NoSQL データベース エンジンをサポートするという利点が得られます。

また、[MongoDB Azure Service](https://www.mongodb.com/scale/mongodb-azure-service) を使用して、MongoDB クラスターを Azure のクラウドで運用データベースとして使用することもできます。 ただしこれは、Microsoft が提供している PaaS サービスではありません。 このケースでは、Azure は MongoDB からのそのソリューションをホスティングしているだけです。

基本的にこれは、Azure Cosmos DB に対して MongoDB API を常に使用しないように記述している単なる免責事項です。eShopOnContainers ではそのようにしましたが、Linux コンテナーにとってはそれが便利な選択だったからです。 決定は、特定のニーズと、実稼働アプリケーションに対して行う必要があるテストに基づいて行う必要があります。

### <a name="the-code-use-mongodb-api-in-net-core-applications"></a>コード:.NET Core アプリケーションで MongoDB API を使用する

.NET 用の MongoDB API は、次の図に示されている Locations.API プロジェクトのような、プロジェクトに追加する必要のある NuGet パッケージに基づいています。

![MongoDB NuGet パッケージの依存関係のスクリーンショット。](./media/nosql-database-persistence-infrastructure/mongodb-api-nuget-packages.png)

**図 7-22**。 .NET Core プロジェクト内の MongoDB API NuGet パッケージの参照

次のセクションでは、コードを調査しましょう。

#### <a name="a-model-used-by-mongodb-api"></a>MongoDB API によって使用されるモデル

最初に、データベースから取得したデータをアプリケーションのメモリ領域で保持するモデルを定義する必要があります。 eShopOnContainers で Locations に使用するモデルの例を次に示します。

```csharp
using MongoDB.Bson;
using MongoDB.Bson.Serialization.Attributes;
using MongoDB.Driver.GeoJsonObjectModel;
using System.Collections.Generic;

public class Locations
{
    [BsonId]
    [BsonRepresentation(BsonType.ObjectId)]
    public string Id { get; set; }
    public int LocationId { get; set; }
    public string Code { get; set; }
    [BsonRepresentation(BsonType.ObjectId)]
    public string Parent_Id { get; set; }
    public string Description { get; set; }
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public GeoJsonPoint<GeoJson2DGeographicCoordinates> Location
                                                             { get; private set; }
    public GeoJsonPolygon<GeoJson2DGeographicCoordinates> Polygon
                                                             { get; private set; }
    public void SetLocation(double lon, double lat) => SetPosition(lon, lat);
    public void SetArea(List<GeoJson2DGeographicCoordinates> coordinatesList)
                                                    => SetPolygon(coordinatesList);

    private void SetPosition(double lon, double lat)
    {
        Latitude = lat;
        Longitude = lon;
        Location = new GeoJsonPoint<GeoJson2DGeographicCoordinates>(
            new GeoJson2DGeographicCoordinates(lon, lat));
    }

    private void SetPolygon(List<GeoJson2DGeographicCoordinates> coordinatesList)
    {
        Polygon = new GeoJsonPolygon<GeoJson2DGeographicCoordinates>(
                  new GeoJsonPolygonCoordinates<GeoJson2DGeographicCoordinates>(
                  new GeoJsonLinearRingCoordinates<GeoJson2DGeographicCoordinates>(
                                                                 coordinatesList)));
    }
}
```

MongoDB NuGet パッケージから取得されるいくつかの属性と型があることがわかります。

NoSQL データベースは、通常、非リレーショナルの階層データを操作するのに非常に適しています。 この例では、`GeoJson2DGeographicCoordinates` のように、特に地理上の位置のために作成された MongoDB 型を使用しています。

#### <a name="retrieve-the-database-and-the-collection"></a>データベースとコレクションを取得する

eShopOnContainers では、次のコードのように、コードを実装してデータベースと MongoCollections を取得する、カスタムのデータベース コンテキストを作成しました。

```csharp
public class LocationsContext
{
    private readonly IMongoDatabase _database = null;

    public LocationsContext(IOptions<LocationSettings> settings)
    {
        var client = new MongoClient(settings.Value.ConnectionString);
        if (client != null)
            _database = client.GetDatabase(settings.Value.Database);
    }

    public IMongoCollection<Locations> Locations
    {
        get
        {
            return _database.GetCollection<Locations>("Locations");
        }
    }
}
```

#### <a name="retrieve-the-data"></a>データを取得する

C# コードでは、Web API コントローラーやカスタム リポジトリの実装のように、MongoDB API からクエリを実行するときに、次のようなコードを記述できます。 `_context` オブジェクトは以前の `LocationsContext` クラスのインスタンスであることに注意してください。

```csharp
public async Task<Locations> GetAsync(int locationId)
{
    var filter = Builders<Locations>.Filter.Eq("LocationId", locationId);
    return await _context.Locations
                            .Find(filter)
                            .FirstOrDefaultAsync();
}
```

#### <a name="use-an-env-var-in-the-docker-composeoverrideyml-file-for-the-mongodb-connection-string"></a>MongoDB の接続文字列に対して docker-compose.override.yml ファイル内の環境変数を使用する

MongoClient オブジェクトを作成するときには、正確に適切なデータベースをポイントしている `ConnectionString` パラメーターと同じ、基本的なパラメーターが必要です。 eShopOnContainers の場合、接続文字列はローカル MongoDB Docker コンテナーまたは "実稼働の" Azure Cosmos DB データベースをポイントすることができます。  次の yml コードにあるように、その接続文字列は、docker-compose または Visual Studio を使用して展開するときに使用される、`docker-compose.override.yml` ファイルで定義されている環境変数から取得されます。

```yml
# docker-compose.override.yml
version: '3.4'
services:
  # Other services
  locations-api:
    environment:
      # Other settings
      - ConnectionString=${ESHOP_AZURE_COSMOSDB:-mongodb://nosqldata}

```

`ConnectionString` 環境変数はこの方法で解決されます:`ESHOP_AZURE_COSMOSDB` グローバル変数が Azure Cosmos DB 接続文字列を使用して `.env` ファイルで定義されている場合、そのグローバル変数を使用してクラウド内の Azure Cosmos DB データベースにアクセスします。 定義されていない場合は `mongodb://nosqldata` 値が取得され、開発 MongoDB コンテナーが使用されます。

次のコードは、eShopOnContainers に実装されているように、Azure Cosmos DB グローバル環境変数を持つ `.env` ファイルを示しています。

```yml
# .env file, in eShopOnContainers root folder
# Other Docker environment variables

ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost
ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=<YourDockerHostIP>

#ESHOP_AZURE_COSMOSDB=<YourAzureCosmosDBConnData>

#Other environment variables for additional Azure infrastructure assets
#ESHOP_AZURE_REDIS_BASKET_DB=<YourAzureRedisBasketInfo>
#ESHOP_AZURE_STORAGE_CATALOG_URL=<YourAzureStorage_Catalog_BLOB_URL>
#ESHOP_AZURE_SERVICE_BUS=<YourAzureServiceBusInfo>
```

「[Azure Cosmos DB への MongoDB アプリケーションの接続](https://docs.microsoft.com/azure/cosmos-db/connect-mongodb-account)」で説明したように、ESHOP_AZURE_COSMOSDB 行をコメント解除し、Azure portal から取得した Azure Cosmos DB 接続文字列を使用してそれを更新する必要があります。

`ESHOP_AZURE_COSMOSDB` グローバル変数が空の場合 (つまり、`.env` ファイルでコメントアウトされている場合)、コンテナーでは既定の MongoDB 接続文字列が使用されます。 この接続文字列は、次の .yml コードに示すように、`nosqldata` という名前の eShopOnContainers に配置されたローカル MongoDB コンテナーをポイントし、docker-compose ファイルに定義されていました。

``` yml
# docker-compose.yml
version: '3.4'
services:
  # ...Other services...
  nosqldata:
    image: mongo
```

#### <a name="additional-resources"></a>その他の技術情報

- **NoSQL データベースのドキュメント データのモデリング** \
  <https://docs.microsoft.com/azure/cosmos-db/modeling-data>

- **Vaughn Vernon。理想的なドメイン駆動設計集約ストアとは?** \
  <https://kalele.io/blog-posts/the-ideal-domain-driven-design-aggregate-store/>

- **Azure Cosmos DB の概要:MongoDB の API**  \
  <https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction>

- **Azure Cosmos DB:.NET および Azure portal を使用して MongoDB API の Web アプリを構築する**  \
  <https://docs.microsoft.com/azure/cosmos-db/create-mongodb-dotnet>

- **ローカルの開発とテストでの Azure Cosmos DB Emulator の使用**  \
  <https://docs.microsoft.com/azure/cosmos-db/local-emulator>

- **Azure Cosmos DB への MongoDB アプリケーションの接続**  \
  <https://docs.microsoft.com/azure/cosmos-db/connect-mongodb-account>

- **Cosmos DB エミュレーターの Docker イメージ (Windows コンテナー)**   \
  <https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/>

- **MongoDB Docker イメージ (Linux と Windows コンテナー)**   \
  <https://hub.docker.com/_/mongo/>

- **Azure Cosmos DB:MongoDB API アカウントでの Studio 3T の使用**  \
  <https://docs.microsoft.com/azure/cosmos-db/mongodb-mongochef>

>[!div class="step-by-step"]
>[前へ](infrastructure-persistence-layer-implemenation-entity-framework-core.md)
>[次へ](microservice-application-layer-web-api-design.md)
