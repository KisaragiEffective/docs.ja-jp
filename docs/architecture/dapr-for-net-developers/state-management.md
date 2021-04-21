---
title: Dapr の状態管理構成ブロック
description: 状態管理構成ブロック、その機能、利点、適用方法の説明。
author: amolenk
ms.date: 02/17/2021
ms.reviewer: robvet
ms.openlocfilehash: 67b7f839ccbe24752281fb537b0473d4984d9e37
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623929"
---
# <a name="the-dapr-state-management-building-block"></a>Dapr の状態管理構成ブロック

分散アプリケーションは、独立した複数のサービスで構成されています。 各サービスはステートレスである必要がありますが、一部のサービスでは、ビジネス操作を完了するために状態を追跡する必要があります。 eコマース サイトの買い物かごサービスについて考えてみましょう。 サービスで状態を追跡できない場合、顧客が Web サイトを離れると買い物かごの内容が失われることがあり、その結果、販売が失われ、カスタマー エクスペリエンスが低下する可能性があります。 これらのシナリオでは、状態を分散状態ストアに永続化する必要があります。 [Dapr の状態管理構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/state-management/)を使用すると、状態の追跡が簡単になり、さまざまなデータ ストア間の高度な機能が提供されます。

状態管理構成ブロックを試すには、[第 3 章のカウンター アプリケーション サンプル](getting-started.md)を参照してください。

## <a name="what-it-solves"></a>解決される内容

分散アプリケーションで状態を追跡することは困難な場合があります。 次に例を示します。

- アプリケーションによっては、異なる種類のデータ ストアが必要になる場合があります。
- データへのアクセスとデータの更新には、異なる整合性レベルが必要になる場合があります。
- 複数のユーザーが同時にデータを更新し、競合の解決が必要になる場合があります。
- サービスでデータ ストアとの対話中に短時間の[一時的なエラー](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling)が発生した場合、再試行する必要があります。

Dapr の状態管理構成ブロックは、これらの課題に対処します。 サードパーティのストレージ SDK に関する依存や学習の必要なしに、状態の追跡が合理化されます。

> [!IMPORTANT]
> Dapr の状態管理により、[キー/値](/azure/architecture/guide/technology-choices/data-store-overview#keyvalue-stores) API が提供されます。 この機能では、リレーショナルまたはグラフ データ ストレージはサポートされていません。

## <a name="how-it-works"></a>しくみ

アプリケーションは Dapr サイドカーと対話して、キーと値のデータを格納および取得します。 内部的には、データを永続化するため、サイドカー API によって構成可能な状態ストア コンポーネントが使用されます。 開発者は、Azure Cosmos DB、SQL Server、Cassandra など、[サポートされる状態ストア](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/)の拡大するコレクションから選択できます。

この API は、HTTP または gRPC を使用して呼び出すことができます。 次の URL を使用して HTTP API を呼び出します。

```http
http://localhost:<dapr-port>/v1.0/state/<store-name>/
```

- `<dapr-port>`: Dapr がリッスンする HTTP ポート。
- `<store-name>`: 使用する状態ストア コンポーネントの名前。

図 5-1 は、Dapr が有効な買い物かごサービスにより、`statestore` という名前の Dapr 状態ストア コンポーネントを使用してキーと値のペアが格納される方法を示したものです。

![Dapr 状態ストアへのキーと値のペアの格納に関する図。](media/state-management/state-management-flow.png)

**図 5-1**. Dapr 状態ストアへのキーと値のペアの格納。

前の図の手順に注意してください。

1. バスケット サービスにより、Dapr サイドカーの状態管理 API が呼び出されます。 要求の本文には、複数のキーと値のペアを含むことができる JSON 配列が格納されます。
1. Dapr サイドカーにより、コンポーネントの構成ファイルに基づいて状態ストアが決定されます。 この例では、Redis キャッシュの状態ストアです。
1. サイドカーにより、Redis キャッシュにデータが永続化されます。

格納されたデータの取得は、同様の API 呼び出しです。 次の例では、*curl* コマンドで Dapr サイドカー API を呼び出すことによってデータを取得します。

```console
curl http://localhost:3500/v1.0/state/statestore/basket1
```

コマンドからは、応答本文で格納されている状態が返されます。

```json
{
  "items": [
    {
      "itemId": "DaprHoodie",
      "quantity": 1
    }
  ],
  "customerId": 1
}
```

以下のセクションでは、状態管理構成ブロックのさらに高度な機能を使用する方法について説明します。

### <a name="consistency"></a>一貫性

[CAP 定理](https://en.wikipedia.org/wiki/CAP_theorem)は、状態を格納する分散システムに適用される一連の原則です。 図 5-2 は、CAP 定理の 3 つの性質を示したものです。

![CAP 定理。](media/state-management/cap-theorem.png)

**図 5-2** CAP 定理。

この定理では、分散データ システムにより、整合性、可用性、およびパーティション トレランスの間でのトレードオフが提供されることが示されています。 また、どのようなデータストアでも "*保証できるのは 3 つの性質のうちの 2 つ*'' だけであることが述べられています。

- *整合性* (**C**)。 すべてのレプリカが更新されるまでシステムで要求をブロックする必要がある場合でも、クラスター内のすべてのノードが最新のデータで応答します。 "整合性のあるシステム" で現在更新中の項目のクエリを実行した場合、すべてのレプリカが正常に更新されるまで応答は得られません。 しかし、常に最新のデータを受け取ります。

- *可用性* (**A**)。 すべてのノードからは、応答が最新のデータではない場合でも、直ちに応答が返されます。 更新中の項目について "使用可能なシステム" に対してクエリを実行すると、その時点でサービスによって提供できる最適な答えが得られます。

- *パーティション トレランス* (**P**)。 レプリケートされたデータのノードで障害が発生した場合や、他のレプリケートされたデータのノードとの接続が失われた場合でも、システムが引き続き動作することを保証します。

分散アプリケーションでは、**P** の性質に対処する必要があります。 サービスはネットワーク呼び出しを使用して相互に通信するため、ネットワークの中断 (**P**) が発生します。 その点を考慮すると、分散アプリケーションは **AP** または **CP** である必要があります。

**AP** のアプリケーションでは、整合性ではなく可用性が選択されます。 Dapr の場合、この選択は **最終的な整合性** 戦略によってサポートされます。 Azure CosmosDB のように、基になるデータ ストアによって複数のレプリカに冗長データが格納される場合について考えます。 最終的な整合性がある場合、状態ストアによって更新が 1 つのレプリカに書き込まれると、クライアントによる書き込み要求は完了します。 その後、ストアによってレプリカが非同期的に更新されます。 読み取り要求に対しては、最新の更新を受け取っていないレプリカも含め、どのレプリカからでもデータを返すことができます。

**CP** のアプリケーションでは、可用性ではなく整合性が選択されます。 Dapr の場合、この選択は **厳密な整合性** 戦略によってサポートされます。 このシナリオでは、書き込み要求が完了する "*前に*"、状態ストアによって必要なレプリカの "*すべて* " (場合によっては、その "*クォーラム*") が同期的に更新されます。 読み取り操作に対しては、レプリカ間で常に最新のデータが返されます。

状態操作の整合性レベルは、操作に "*整合性ヒント*" をアタッチすることによって指定されます。 次の *curl* コマンドを実行すると、厳密な整合性ヒントを使用して、`Hello=World` というキーと値のペアが状態ストアに書き込まれます。

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name> \
  -H "Content-Type: application/json" \
  -d '[
        {
          "key": "Hello",
          "value": "World",
          "options": {
            "consistency": "strong"
          }
        }
      ]'
```

> [!IMPORTANT]
> 操作にアタッチされている整合性ヒントは、Dapr の状態ストア コンポーネントによって満たされます。 すべてのデータ ストアで両方の整合性レベルがサポートされているわけではありません。 整合性ヒントが設定されていない場合の既定の動作は、**最終的** です。

### <a name="concurrency"></a>コンカレンシー

マルチユーザー アプリケーションの場合、複数のユーザーが同時に同じデータを更新する可能性があります。 Dapr では、競合を管理するためにオプティミスティック同時実行制御 (OCC) がサポートされています。 OCC は、ユーザーはデータのさまざまな部分を操作するため、更新が競合することはめったにない、という前提に基づいています。 更新は成功するはずであり、そうでない場合は再試行する、と想定する方が効率的です。 代わりにペシミスティック ロックを実装すると、長時間のロックによってデータの競合が発生し、パフォーマンスが低下する可能性があります。

Dapr では、オプティミスティック同時実行制御 (OCC) をサポートするために ETag が使用されています。 ETag は、格納されているキーと値のペアの特定のバージョンに関連付けられた値です。 キーと値のペアが更新されるたびに、ETag の値も更新されます。 クライアントがキーと値のペアを取得すると、応答には現在の ETag の値が含まれます。 クライアントでキーと値のペアを更新または削除するときは、要求本文でその ETag 値を返送する必要があります。 それまでの間に別のクライアントによってデータが更新されていた場合、ETag は一致せず、要求は失敗します。 この時点で、クライアントは更新されたデータを取得し、再度変更を行って、更新を送信し直す必要があります。 この戦略は、**最初の書き込み優先** と呼ばれます。

Dapr では、**最後の書き込み優先** 戦略もサポートされています。 この方法の場合、クライアントは ETag を書き込み要求にアタッチしません。 セッションの間に基になる値が変更された場合でも、更新は状態ストア コンポーネントによって常に許可されます。 最後の書き込み優先は、データの競合が少ない高スループットの書き込みシナリオに役立ちます。 また、ユーザーの不定期な更新を上書きすることもできます。

### <a name="transactions"></a>トランザクション

Dapr では、トランザクションとして実装された単一の操作として、データ ストアに *複数項目の変更* を書き込むことができます。 この機能は、[ACID](https://en.wikipedia.org/wiki/ACID) トランザクションをサポートするデータ ストアに対してのみ使用できます。 これを書いている時点で、そのようなストアとしては Redis、MongoDB、PostgreSQL、SQL Server、Azure CosmosDB などがあります。

次の例では、複数項目の操作が 1 つのトランザクションで状態ストアに送信されます。 トランザクションをコミットするには、すべての操作が成功する必要があります。 1 つ以上の操作が失敗した場合、トランザクション全体がロールバックされます。

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name>/transaction \
  -H "Content-Type: application/json" \
  -d '{
        "operations": [
          {
            "operation": "upsert",
            "request": { "key": "Key1", "value": "Value1"
            }
          },
          {
            "operation": "delete",
            "request": { "key": "Key2" }
          }
        ]
      }'
```

トランザクションがサポートされていないデータ ストアの場合でも、複数のキーを 1 つの要求として送信できます。 次に示すのは、**一括** 書き込み操作の例です。

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name> \
  -H "Content-Type: application/json" \
  -d '[
        { "key": "Key1", "value": "Value1" },
        { "key": "Key2", "value": "Value2" }
      ]'
```

一括操作の場合、Dapr によって、各キーと値のペアの更新が、個別の要求としてデータ ストアに送信されます。

## <a name="use-the-dapr-net-sdk"></a>Dapr .NET SDK を使用する

Dapr .NET SDK により、.NET Core プラットフォームに言語固有のサポートが提供されます。 開発者は、[第 3 章](getting-started.md)で紹介した `DaprClient` クラスを使用して、データの読み取りと書き込みを行うことができます。 次に示すのは、`DaprClient.GetStateAsync<TValue>` メソッドを使用して状態ストアからデータを読み取る方法の例です。 このメソッドには、パラメーターとしてストア名 `statestore` とキー `AMS` を渡す必要があります。

```csharp
var weatherForecast = await daprClient.GetStateAsync<WeatherForecast>("statestore", "AMS");
```

状態ストアにキー `AMS` のデータが含まれていない場合、結果は `default(WeatherForecast)` になります。

データをデータ ストアに書き込むには、`DaprClient.SaveStateAsync<TValue>` メソッドを使用します。

```csharp
daprClient.SaveStateAsync("statestore", "AMS", weatherForecast);
```

この例では、ETag 値を状態ストア コンポーネントに渡さないため、**最後の書き込み優先** 戦略を使用します。 **最初の書き込み優先** 戦略でオプティミスティック同時実行制御 (OCC) を使用するには、最初に `DaprClient.GetStateAndETagAsync` メソッドを使用して現在の ETag を取得します。 次に、`DaprClient.TrySaveStateAsync` メソッドを使用して、更新された値を書き込み、取得した ETag を渡します。

```csharp
var (weatherForecast, etag) = await daprClient.GetStateAndETagAsync<WeatherForecast>("statestore", city);

// ... make some changes to the retrieved weather forecast

var result = await daprClient.TrySaveStateAsync("statestore", city, weatherForecast, etag);
```

データを取得した後で、状態ストア内のデータ (および関連する ETag) が変更されている場合、`DaprClient.TrySaveStateAsync` メソッドは失敗します。 メソッドからは、呼び出しが成功したかどうかを示すブール値が返されます。 障害を処理するには、単に状態ストアから更新されたデータを再度読み込み、変更を行ってから更新を再送信します。

データに対する他の変更に関係なく書き込みが常に成功するようにするには、**最後の書き込み優先** 戦略を使用します。

SDK には、データの一括取得、データの削除、トランザクションの実行のための他のメソッドが用意されています。 詳細については、[Dapr .NET SDK のリポジトリ](https://github.com/dapr/dotnet-sdk)を参照してください。

### <a name="aspnet-core-integration"></a>ASP.NET Core の統合

Dapr では、最新のクラウドベースの Web アプリケーションを構築するためのクロスプラットフォームのフレームワークである ASP.NET Core もサポートされています。 Dapr SDK により、[ASP.NET Core のモデル バインド](/aspnet/core/mvc/models/model-binding)機能に状態管理機能が直接統合されます。 構成は簡単です。 次の例に示すように、`Startup.cs` クラスに `.AddDapr` 拡張メソッドを追加することにより、`IMVCBuilder.AddDapr` を追加します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers().AddDapr();
}
```

構成が完了すると、Dapr により、ASP.NET Core の `FromState` 属性を使用して、キーと値のペアをコントローラー アクションに直接挿入できます。 `DaprClient` オブジェクトを参照する必要はなくなります。 次に示すのは、特定の都市の天気予報を返す Web API の例です。

```csharp
[HttpGet("{city}")]
public ActionResult<WeatherForecast> Get([FromState("statestore", "city")] StateEntry<WeatherForecast> forecast)
{
    if (forecast.Value == null)
    {
      return NotFound();
    }

    return forecast.Value;
}
```

この例では、コントローラーにより `FromState` 属性を使用して天気予報が読み込まれます。 1 つ目の属性パラメーターは、状態ストア `statestore` です。 2 つ目の属性パラメーター `city` は、状態キーを取得する[ルート テンプレート](/aspnet/core/mvc/controllers/routing#route-templates)変数の名前です。 2 つ目のパラメーターを省略した場合は、バインドされたメソッド パラメーターの名前 (`forecast`) を使用して、ルート テンプレート変数が検索されます。

`StateEntry` クラスには、1 つのキーと値のペアに対して取得されるすべての情報のプロパティ (`StoreName`、`Key`、`Value`、`ETag`) が含まれています。 ETag は、オプティミスティック同時実行制御 (OCC) 戦略を実装する場合に便利です。 クラスには、`DaprClient` インスタンスを必要とせずに取得したキーと値のデータを削除または更新するためのメソッドも用意されています。 次の例では、`TrySaveAsync` メソッドを使用し、取得した気象予測を OCC を使用して更新します。

```csharp
[HttpPut("{city}")]
public async Task Put(WeatherForecast updatedForecast, [FromState("statestore", "city")] StateEntry<WeatherForecast> currentForecast)
{
    // update cached current forecast with updated forecast passed into service endpoint
    currentForecast.Value = updatedForecast;

    // update state store
    var success = await currentForecast.TrySaveAsync();

    // ... check result
}
```

## <a name="state-store-components"></a>状態ストアのコンポーネント

これを書いている時点で、Dapr によってサポートされているトランザクション状態ストアは次のとおりです。

- Azure CosmosDB
- Azure SQL Server
- MongoDB
- PostgreSQL
- Redis

また、Dapr には、CRUD 操作には対応してもトランザクション機能には対応しない状態ストアのサポートも含まれています。

- Aerospike
- Azure Blob Storage
- Azure Table Storage
- Cassandra
- Cloudstate
- Couchbase
- etcd
- Google Cloud Firestore
- Hashicorp Consul
- Hazelcast
- Memcached
- Zookeeper

### <a name="configuration"></a>構成

ローカル環境のセルフホステッド開発用に初期化されている場合、Dapr により Redis が既定の状態ストアとして登録されます。 既定の状態ストアの構成の例を次に示します。 既定の名前 `statestore` に注意してください。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

 > [!NOTE]
 > 1 つのアプリケーションに、多くの状態ストアを、それぞれ異なる名前で登録できます。

Redis 状態ストアの場合、Redis インスタンスに接続するために `redisHost` と `redisPassword` のメタデータが必要です。 上の例では、Redis パスワード (既定では空の文字列) がプレーン文字列として格納されています。 ベスト プラクティスとして、クリア テキスト文字列を避け、常にシークレット参照を使用することをお勧めします。 シークレット管理の詳細については、[第 10 章](secrets.md)を参照してください。

もう 1 つのメタデータ フィールド `actorStateStore` は、状態ストアをアクター構成ブロックで使用できるかどうかを示します。

### <a name="key-prefix-strategies"></a>キー プレフィックス戦略

状態ストア コンポーネントを使用すると、さまざまな方法で基になるストアにキーと値のペアを格納できます。 顧客が買いたい品物を入れる買い物かごサービスの前の例を思い出してください。

```console
curl -X POST http://localhost:3500/v1.0/state/statestore \
  -H "Content-Type: application/json" \
  -d '[{
        "key": "basket1",
        "value": {
          "customerId": 1,
          "items": [
            { "itemId": "DaprHoodie", "quantity": 1 }
          ]
        }
     }]'
```

Redis コンソール ツールを使用して、Redis キャッシュの内部を調べ、Redis 状態ストア コンポーネントによってデータがどのように永続化されているかを確認します。

```
127.0.0.1:6379> KEYS *
1) "basketservice||basket1"

127.0.0.1:6379> HGETALL basketservice||basket1
1) "data"
2) "{\"items\":[{\"itemId\":\"DaprHoodie\",\"quantity\":1}],\"customerId\":1}"
3) "version"
4) "1"
```

出力には、データの完全な Redis **キー** が `basketservice||basket1` と表示されています。 既定では、Dapr により、キーのプレフィックスとして、Dapr インスタンス (`basketservice`) の `application id` が使用されます。 この名前付け規則を使用すると、複数の Dapr インスタンスで、キー名の競合を発生させずに同じデータ ストアを共有できます。 開発者にとっては、Dapr でアプリケーションを実行するときに常に同じ `application id` を指定することが重要です。 省略した場合、Dapr によって一意のアプリケーション ID が生成されます。 `application id` が変更されると、前のキー プレフィックスで格納された状態にアプリケーションでアクセスできなくなります。

ただし、状態ストア コンポーネント ファイルの `keyPrefix` メタデータ フィールドで、キー プレフィックスに "*定数値*" を構成することができます。 次の例を確認してください。

```yaml
spec:
  metadata:
  - name: keyPrefix
  - value: MyPrefix
```

定数のキー プレフィックスを使用すると、複数の Dapr アプリケーションで状態ストアにアクセスできます。 さらに、プレフィックスを完全に省略するには、`keyPrefix` を `none` に設定します。

## <a name="reference-application-eshopondapr"></a>参照アプリケーション: eShopOnDapr

この記事には、`eShopOnDapr` という名前の参照アプリケーションが含まれています。 これは、以前の Microsoft マイクロサービス参照アプリケーション `eShopOnContainers` からモデル化されています。

元の [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) のアーキテクチャでは、`IBasketRepository` インターフェイスを使用して、バスケット サービスのデータの読み取りと書き込みが行われていました。 `RedisBasketRepository` クラスにより、基になるデータ ストアとして Redis を使用する実装が提供されていました。

```csharp
public class RedisBasketRepository : IBasketRepository
{
    private readonly ConnectionMultiplexer _redis;
    private readonly IDatabase _database;

    public RedisBasketRepository(ConnectionMultiplexer redis)
    {
        _redis = redis;
        _database = redis.GetDatabase();
    }

    public async Task<CustomerBasket> GetBasketAsync(string customerId)
    {
        var data = await _database.StringGetAsync(customerId);

        if (data.IsNullOrEmpty)
        {
            return null;
        }

        return JsonConvert.DeserializeObject<CustomerBasket>(data);
    }

    // ...
}
```

このコードでは、サードパーティの `StackExchange.Redis` NuGet パッケージを使用します。 特定の顧客の買い物かごを読み込むには、次の手順を実行する必要があります。

1. `ConnectionMultiplexer` をコンストラクターに挿入します。 `ConnectionMultiplexer` は、`Startup.cs` ファイルで依存関係挿入フレームワークに登録されます。

   ```csharp
   services.AddSingleton<ConnectionMultiplexer>(sp =>
   {
       var settings = sp.GetRequiredService<IOptions<BasketSettings>>().Value;
       var configuration = ConfigurationOptions.Parse(settings.ConnectionString, true);
       configuration.ResolveDns = true;
       return ConnectionMultiplexer.Connect(configuration);
   });
   ```

1. `ConnectionMultiplexer` を使用して、使用する各クラスに `IDatabase` インスタンスを作成します。

1. `IDatabase` インスタンスを使用し、キーとして指定された `customerId` を使用して Redis StringGet の呼び出しを実行します。

1. データが Redis から読み込まれているかどうかを確認します。そうでない場合は、`null` を返します。

1. Redis から `CustomerBasket` オブジェクトにデータを逆シリアル化し、その結果を返します。

更新された [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) 参照アプリケーションでは、新しい `DaprBasketRepository` クラスで `RedisBasketRepository` クラスが置き換えられています。

```csharp
public class DaprBasketRepository : IBasketRepository
{
    private const string StoreName = "eshop-basket-statestore";

    private readonly DaprClient _daprClient;

    public DaprBasketRepository(DaprClient daprClient)
    {
        _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));;
    }

    public async Task<CustomerBasket> GetBasketAsync(string customerId)
    {
        return await _daprClient.GetStateAsync<CustomerBasket>(StoreName, customerId);
    }

    // ...
}
```

更新されたコードでは、状態管理構成ブロックを使用してデータの読み取りと書き込みを行うために、Dapr .NET SDK が使用されています。 顧客の買い物かごを読み込む新しい手順は、大幅に簡素化されています。

1. `DaprClient` をコンストラクターに挿入します。 `DaprClient` は、`Startup.cs` ファイルで依存関係挿入フレームワークに登録されます。
1. `DaprClient.GetStateAsync` メソッドを使用して、構成されている状態ストアから顧客の買い物かごの商品を読み込み、結果を返します。

更新された実装では、引き続き Redis が基になるデータ ストアとして使用されます。 ただし、Dapr によって `StackExchange.Redis` の参照と複雑さがアプリケーションから取り除かれます。 必要なものは Dapr 構成ファイルだけです。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-basket-statestore
  namespace: eshop
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: redis:6379
  - name: redisPassword
    secretKeyRef:
      name: redisPassword
auth:
  secretStore: eshop-secretstore
```

Dapr の実装により、基になるデータ ストアの変更も簡略化されます。 たとえば、Azure Table Storage に切り替えるために必要なのは、構成ファイルの内容の変更だけです。 コードに変更を加える必要はありません。

## <a name="summary"></a>まとめ

Dapr の状態管理構成ブロックにより、さまざまなデータ ストアにキーと値のデータを格納するための API が提供されます。 API により以下のサポートが提供されます。

- 一括操作
- 厳密および最終的な整合性
- オプティミスティック コンカレンシー
- 複数項目のトランザクション

.NET SDK により、.NET Core および ASP.NET Core 用の言語固有のサポートが提供されます。 モデル バインドの統合により、ASP.NET Core コントローラー アクション メソッドからの状態へのアクセスと更新が簡単になります。

eShopOnDapr 参照アプリケーションでは、Dapr の状態管理に移行する利点が明らかです。

1. 新しい実装では、使用するコード行が少なくなります。
1. これにより、サードパーティの `StackExchange.Redis` API の複雑さが解消されます。
1. 基になる Redis キャッシュを別の種類のデータ ストアに置き換えるときに必要なことが、状態ストアの構成ファイルの変更だけになります。

### <a name="references"></a>リファレンス

- [Dapr でサポートされている状態ストア](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/)

> [!div class="step-by-step"]
> [前へ](reference-application.md)
> [次へ](service-invocation.md)
