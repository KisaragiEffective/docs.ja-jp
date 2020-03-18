---
title: 正常性の監視
description: 正常性の監視を実施する 1 つの方法を探ります。
ms.date: 01/30/2020
ms.openlocfilehash: a91e51af66049f9774365cd56b90ab792a4dd4fc
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77502683"
---
# <a name="health-monitoring"></a>正常性の監視

正常性を監視することで、コンテナーとマイクロサービスの状態に関する情報をほぼリアルタイムに得ることができます。 正常性の監視はマイクロサービス運用の複数の側面において不可欠であり、オーケストレーターで段階的に部分的なアプリケーションのアップグレードを行う際に特に重要です。

マイクロサービス ベースのアプリケーションでは、多くの場合、ハートビートまたは正常性チェックを使用して、パフォーマンス モニター、スケジューラー、およびオーケストレーターで多数のサービスを追跡できるようにします。 サービスが必要に応じて、またはスケジュールに従って、ある種の "アライブ" シグナルを送信できない場合、更新プログラムの展開時にアプリケーションがリスクに直面する可能性があります。あるいは、単に障害の検出が遅すぎたために障害の連鎖を防ぐことができず、最終的に大規模な停止が発生する可能性があります。

標準的なモデルでは、サービスは状態に関するレポートを送信し、その情報が集約され、アプリケーションの正常性状態の概要が提供されます。 オーケストレーターを使用する場合は、オーケストレーターのクラスターに正常性情報を提供することができるため、クラスターは適切に機能できます。 アプリケーション用にカスタマイズされている高品質の正常性レポートに投資する場合、実行中のアプリケーションの問題をはるかに簡単に検出して修正することができます。

## <a name="implement-health-checks-in-aspnet-core-services"></a>ASP.NET Core サービスでの正常性チェックを実装する

ASP.NET Core のマイクロサービスまたは Web アプリケーションを開発するとき、ASP .NET Core 3.1 でリリースされた組み込みの正常性チェック機能 ([Microsoft.Extensions.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks)) を使用できます。 多くの ASP.NET Core 機能と同様に、正常性チェックには一連のサービスとミドルウェアが伴います。

正常性チェックのサービスとミドルウェアは使いやすく、アプリケーションに必要な外部リソースがあれば (SQL Server データベースやリモート API など)、それが正しく動作していることを検証できる機能を提供します。 この機能を使用すれば、リソースが正常であるかどうかを判断することもできます。これについては後で説明します。

この機能を効果的に使用するには、まず、マイクロサービスでサービスを構成する必要があります。 次に、正常性レポートのクエリを実行するフロント エンド アプリケーションが必要になります。 このフロント エンド アプリケーションはカスタム レポート アプリケーションである場合があります。また、正常性状態に応じて対応できるオーケストレーター自体である場合もあります。

### <a name="use-the-healthchecks-feature-in-your-back-end-aspnet-microservices"></a>バック エンド ASP.NET マイクロサービス内で HealthChecks 機能を使用する

このセクションでは、[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) で実装される HealthChecks 機能が、サンプルの ASP.NET Core 3.1 Web API アプリケーションでどのように使用されるかについて説明します。 eShopOnContainers のような大規模なマイクロサービスでのこの機能の実装については、後のセクションで説明します。 開始するには、各マイクロサービスの正常性状態の構成要素を定義する必要があります。 サンプル アプリケーションでは、マイクロサービス API に HTTP 経由でアクセスできて、それに関連する SQL Server データベースも使用可能であれば、マイクロサービスは正常な状態です。

.NET Core 3.1 では、組み込み API を使用することで、次のようにサービスを構成し、マイクロサービスとそれに依存している SQL Server データベースに対する正常性チェックを追加できます。

```csharp
// Startup.cs from .NET Core 3.1 Web API sample
//
public void ConfigureServices(IServiceCollection services)
{
    //...
    // Registers required services for health checks
    services.AddHealthChecks()
        // Add a health check for a SQL Server database
        .AddSqlServer(
            configuration["ConnectionString"],
            name: "OrderingDB-check",
            tags: new string[] { "orderingdb" });
}
```

前のコードでは、`services.AddHealthChecks()` メソッドによって、ステータス コード **200** と "正常" を返す基本的な HTTP チェックが構成されます。  さらに、`AddCheck()` 拡張メソッドによって、関連 SQL Database の正常性をチェックするカスタム `SqlConnectionHealthCheck` が構成されます。

`AddCheck()` メソッドによって、指定の名前と型 `IHealthCheck` の実装を利用して新しい正常性チェックが追加されます。 AddCheck メソッドを利用して複数の正常性チェックを追加できます。そのため、そのチェックがすべて正常となるまで、"正常" ステータスはマイクロサービスから出されません。

`SqlConnectionHealthCheck` は `IHealthCheck` を実装するカスタム クラスです。これはコンストラクター パラメーターとして接続文字列を受け取り、簡単なクエリを実行し、SQL データベースに正常に接続できたかどうかを確認します。 クエリが正常に実行された場合、`HealthCheckResult.Healthy()` が返され、失敗した場合、`FailureStatus` と実際の例外が返されます。

```csharp
// Sample SQL Connection Health Check
public class SqlConnectionHealthCheck : IHealthCheck
{
    private static readonly string DefaultTestQuery = "Select 1";

    public string ConnectionString { get; }

    public string TestQuery { get; }

    public SqlConnectionHealthCheck(string connectionString)
        : this(connectionString, testQuery: DefaultTestQuery)
    {
    }

    public SqlConnectionHealthCheck(string connectionString, string testQuery)
    {
        ConnectionString = connectionString ?? throw new ArgumentNullException(nameof(connectionString));
        TestQuery = testQuery;
    }

    public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken cancellationToken = default(CancellationToken))
    {
        using (var connection = new SqlConnection(ConnectionString))
        {
            try
            {
                await connection.OpenAsync(cancellationToken);

                if (TestQuery != null)
                {
                    var command = connection.CreateCommand();
                    command.CommandText = TestQuery;

                    await command.ExecuteNonQueryAsync(cancellationToken);
                }
            }
            catch (DbException ex)
            {
                return new HealthCheckResult(status: context.Registration.FailureStatus, exception: ex);
            }
        }

        return HealthCheckResult.Healthy();
    }
}
```

前のコードでは、データベースの正常性確認に使用されたクエリが `Select 1` であることにご留意ください。 マイクロサービスの可用性を監視するために、Kubernetes などのオーケストレーターでは、要求を送信してマイクロサービスをテストすることで、定期的に正常性チェックを実行します。 そのような操作が速く行われ、リソースの利用率が上がることがないように、データベース クエリを効率的にすることが重要です。

最後に、URL パス `/hc` に応答するミドルウェアを追加します。

```csharp
// Startup.cs from .NET Core 3.1 Web Api sample
//
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    app.UseEndpoints(endpoints =>
    {
        //...
        endpoints.MapHealthChecks("/hc", new HealthCheckOptions()
        {
            Predicate = _ => true,
            ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
        });
        //...
    });
    //…
}
```

エンドポイント `<yourmicroservice>/hc` が呼び出されると、スタートアップ クラスの `AddHealthChecks()` メソッドで構成されているすべての正常性チェックがすべて実行され、結果が表示されます。

### <a name="healthchecks-implementation-in-eshoponcontainers"></a>eShopOnContainers の HealthChecks 実装

eShopOnContainers のマイクロサービスは、そのタスクを実行するために複数のサービスに依存しています。 たとえば、eShopOnContainers の `Catalog.API` マイクロサービスは、Azure Blob Storage、SQL Server、RabbitMQ など、さまざまなサービスに依存しています。 そのため、`AddCheck()` メソッドを使用していくつかの正常性チェックが追加されています。 あらゆる従属サービスに対して、それぞれの正常性状態を定義するカスタム `IHealthCheck` 実装を追加する必要があります。

オープンソース プロジェクト [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) により、.NET Core 3.1 の上に構築されるそれぞれのエンタープライズ サービス向けのカスタムの正常性チェックが実装され、この問題が解決されます。 各正常性チェックは、プロジェクトに簡単に追加できる個別の NuGet パッケージとして利用できます。 eShopOnContainers では、そのすべてのマイクロサービスでそれらが広範囲に使用されます。

たとえば、`Catalog.API` マイクロ サービスでは、次の NuGet パッケージが追加されています。

![AspNetCore.Diagnostics.HealthChecks NuGet パッケージのスクリーンショット。](./media/monitor-app-health/aspnet-core-diagnostics-health-checks.png)

**図 8-7** AspNetCore.Diagnostics.HealthChecks を使用して Catalog.API に実装されたカスタム正常性チェック

次のコードでは、従属サービスごとに正常性チェック実装が追加され、それからミドルウェアが構成されます。

```csharp
// Startup.cs from Catalog.api microservice
//
public static IServiceCollection AddCustomHealthCheck(this IServiceCollection services, IConfiguration configuration)
{
    var accountName = configuration.GetValue<string>("AzureStorageAccountName");
    var accountKey = configuration.GetValue<string>("AzureStorageAccountKey");

    var hcBuilder = services.AddHealthChecks();

    hcBuilder
        .AddSqlServer(
            configuration["ConnectionString"],
            name: "CatalogDB-check",
            tags: new string[] { "catalogdb" });

    if (!string.IsNullOrEmpty(accountName) && !string.IsNullOrEmpty(accountKey))
    {
        hcBuilder
            .AddAzureBlobStorage(
                $"DefaultEndpointsProtocol=https;AccountName={accountName};AccountKey={accountKey};EndpointSuffix=core.windows.net",
                name: "catalog-storage-check",
                tags: new string[] { "catalogstorage" });
    }
    if (configuration.GetValue<bool>("AzureServiceBusEnabled"))
    {
        hcBuilder
            .AddAzureServiceBusTopic(
                configuration["EventBusConnection"],
                topicName: "eshop_event_bus",
                name: "catalog-servicebus-check",
                tags: new string[] { "servicebus" });
    }
    else
    {
        hcBuilder
            .AddRabbitMQ(
                $"amqp://{configuration["EventBusConnection"]}",
                name: "catalog-rabbitmqbus-check",
                tags: new string[] { "rabbitmqbus" });
    }

    return services;
}
```

最後に、"/hc" エンドポイントをリッスンする HealthCheck ミドルウェアを追加します。

```csharp
// HealthCheck middleware
app.UseHealthChecks("/hc", new HealthCheckOptions()
{
    Predicate = _ => true,
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});
}
```

### <a name="query-your-microservices-to-report-about-their-health-status"></a>マイクロサービスでクエリを実行してその正常性状態についてレポートする

この記事の説明に従って正常性チェックを構成し、マイクロサービスを Docker で実行した後、正常かどうかをブラウザーから直接確認することができます。 図 8-8 に示すように、Docker ホスト内でコンテナー ポートを公開し、外部の Docker ホスト IP または `localhost` を介してコンテナーにアクセスできるようにする必要があります。

![正常性チェックから返された JSON 応答のスクリーンショット。](./media/monitor-app-health/health-check-json-response.png)

**図 8-8** ブラウザーからの単一サービスの正常性状態の確認

このテストでは、`Catalog.API` マイクロサービス (ポート 5101 で実行されている) が正常であり、JSON で HTTP ステータス 200 とステータス情報が返されていることがわかります。 サービスによってその SQL Server データベースの依存関係と RabbitMQ の正常性も確認されました。つまり、その正常性チェック自体が正常とレポートされました。

## <a name="use-watchdogs"></a>ウォッチドッグを使用する

ウォッチドッグは、サービス間の正常性と負荷を監視し、前述の `HealthChecks` ライブラリを使用してクエリを実行することでマイクロサービスに関する正常性をレポートできる別個のサービスです。 これは、単一サービスのビューに基づいて検出されないエラーを防ぐのに役立ちます。 また、ウォッチドッグは、ユーザーの介入なしで既知の状態に応じて修復アクションを実行できるコードをホストする場合に適しています。

eShopOnContainers サンプルには、図 8-9 に示すように、サンプルの正常性チェック レポートを表示する Web ページが含まれています。 これは、eShopOnContainers でマイクロサービスと Web アプリケーションの状態を表示するだけなので、使用可能な最も単純なウォッチドッグです。 通常、ウォッチドッグは異常状態の検出時にもアクションを実行します。

幸い、[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) からは [AspNetCore.HealthChecks.UI](https://www.nuget.org/packages/AspNetCore.HealthChecks.UI/) NuGet パッケージも提供されます。このパッケージを利用し、構成された URI から正常性チェックの結果を表示できます。

![Health Checks UI の eShopOnContainers の正常性状態のスクリーンショット。](./media/monitor-app-health/health-check-status-ui.png)

**図 8-9** eShopOnContainers のサンプルの正常性チェック レポート

まとめると、このウォッチドッグ サービスは各マイクロサービスの "/hc" エンドポイントをクエリします。 これにより、定義されているすべての正常性チェックが実行され、これらすべてのチェックに応じて、全体的な正常性状態が返されます。 HealthChecksUI は、ウォッチドッグ サービスの Startup.cs に追加する必要があるいくつかの構成エントリと 2 つのコード行と共に簡単に使用できます。

正常性チェック UI のサンプル構成ファイル:

```json
// Configuration
{
  "HealthChecks-UI": {
    "HealthChecks": [
      {
        "Name": "Ordering HTTP Check",
        "Uri": "http://localhost:5102/hc"
      },
      {
        "Name": "Ordering HTTP Background Check",
        "Uri": "http://localhost:5111/hc"
      },
      //...
    ]}
}
```

HealthChecksUI を追加する Startup.cs ファイル:

```csharp
// Startup.cs from WebStatus(Watch Dog) service
//
public void ConfigureServices(IServiceCollection services)
{
    //…
    // Registers required services for health checks
    services.AddHealthChecksUI();
}
//…
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    app.UseHealthChecksUI(config=> config.UIPath = “/hc-ui”);
    //…
}
```

## <a name="health-checks-when-using-orchestrators"></a>オーケストレーターを使用する場合の正常性チェック

マイクロサービスの可用性を監視するために、Kubernetes、Service Fabric などのオーケストレーターではマイクロサービスのテスト要求を送信して、定期的に正常性チェックを実行します。 オーケストレーターでサービス/コンテナーが異常であると判断された場合、そのインスタンスへの要求のルーティングが停止します。 通常は、そのコンテナーの新しいインスタンスも作成されます。

たとえば、ほとんどのオーケストレーターでは、ダウンタイムなしの展開を管理するために正常性チェックを使用できます。 サービス/コンテナーの状態が正常に変わった場合にのみ、オーケストレーターはサービス/コンテナー インスタンスへのトラフィックのルーティングを開始します。

正常性の監視は、オーケストレーターがアプリケーションのアップグレードを実行する場合に特に重要です。 いくつかのオーケストレーター (Azure Service Fabric など) ではサービスを段階的に更新します。たとえば、アプリケーションのアップグレードごとに 5 分の 1 のクラスター サーフェスを更新する場合があります。 同時にアップグレードされるノード セットは*アップグレード ドメイン* と呼ばれます。 各アップグレード ドメインがアップグレードされ、ユーザーが使用できるようになったら、そのアップグレード ドメインは、展開が次のアップグレード ドメインに移る前に正常性チェックを渡す必要があります。

サービスの正常性のもう 1 つの側面は、サービスからのメトリックのレポートです。 これは、Service Fabric などの一部のオーケストレーターの正常性モデルの高度な機能です。 メトリックは、リソース使用量のバランスをとるために使用されるため、オーケストレーターの使用時に重要になります。 メトリックをシステム正常性のインジケーターにすることもできます。 たとえば、アプリケーションに多くのマイクロサービスがあり、各インスタンスで要求/秒 (RPS) メトリックをレポートするとします。 1 つのサービスが別のサービスより多くのリソース (メモリやプロセッサなど) を使用している場合、オーケストレーターはクラスター内でサービス インスタンスを移動して、リソース使用率を均一に保つことができます。

Azure Service Fabric には、単純な正常性チェックよりも高度な独自の[正常性の監視モデル](/azure/service-fabric/service-fabric-health-introduction)が提供されていることに注意してください。

## <a name="advanced-monitoring-visualization-analysis-and-alerts"></a>高度な監視: 視覚化、分析、およびアラート

監視の最後の部分は、イベント ストリームの視覚化、サービス パフォーマンスのレポート、および問題の検出時のアラートです。 監視のこの部分ではさまざまなソリューションを使用できます。

[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) の説明の際に示したカスタム ページのように、サービスの状態を示す単純なカスタム アプリケーションを使用できます。 または、[Azure Monitor](https://azure.microsoft.com/services/monitor/) などのより高度なツールを使用して、イベントのストリームに基づいてアラートを生成することができます。

最後に、すべてのイベント ストリームを格納していた場合、Microsoft Power BI、または Kibana や Splunk などの他のソリューションを使用して、データを視覚化することができます。

## <a name="additional-resources"></a>その他の技術情報

- **ASP.NET Core の HealthChecks と HealthChecks UI** \
  <https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks>

- **Service Fabric 正常性監視の概要** \
  [https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction](/azure/service-fabric/service-fabric-health-introduction)

- **Azure Monitor** \
  <https://azure.microsoft.com/services/monitor/>

>[!div class="step-by-step"]
>[前へ](implement-circuit-breaker-pattern.md)
>[次へ](../secure-net-microservices-web-applications/index.md)
