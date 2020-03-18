---
title: 回復力の高い Entity Framework Core SQL 接続を実装する
description: 回復力の高い Entity Framework Core SQL 接続を実装する方法について説明します。 この手法は、クラウドで Azure SQL Database を使用する場合に特に重要です。
ms.date: 10/16/2018
ms.openlocfilehash: 7a047edca21d63a451e90f407b23f3358d461330
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78241066"
---
# <a name="implement-resilient-entity-framework-core-sql-connections"></a>回復力の高い Entity Framework Core SQL 接続を実装する

Azure SQL DB の場合、Entity Framework (EF) Core に内部データベース接続の復元機能と再試行ロジックが既に用意されています。 ただし、[回復力のある EF Core 接続](/ef/core/miscellaneous/connection-resiliency)を使用する場合は、各 <xref:Microsoft.EntityFrameworkCore.DbContext> 接続に対して Entity Framework 実行戦略を有効にする必要があります。

たとえば、EF Core 接続レベルで次のコードを実行すると、接続が失敗した場合に再試行される回復力のある SQL 接続が有効になります。

```csharp
// Startup.cs from any ASP.NET Core Web API
public class Startup
{
    // Other code ...
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddDbContext<CatalogContext>(options =>
        {
            options.UseSqlServer(Configuration["ConnectionString"],
            sqlServerOptionsAction: sqlOptions =>
            {
                sqlOptions.EnableRetryOnFailure(
                maxRetryCount: 10,
                maxRetryDelay: TimeSpan.FromSeconds(30),
                errorNumbersToAdd: null);
            });
        });
    }
//...
}
```

## <a name="execution-strategies-and-explicit-transactions-using-begintransaction-and-multiple-dbcontexts"></a>BeginTransaction と複数の DbContext を使用した実行戦略と明示的なトランザクション

EF Core 接続で再試行を有効にすると、EF Core を使用して実行する各操作は、独自の再試行可能な操作になります。 一時的なエラーが発生した場合、`SaveChanges` への各クエリと各呼び出しは 1 つのユニットとして再試行されます。

一方、`BeginTransaction` を使用してトランザクションを開始するコードの場合、1 ユニットとして扱う必要のある独自の操作グループを定義しています。 エラーが発生した場合は、トランザクション内のすべてをロールバックする必要があります。

EF 実行戦略 (再試行ポリシー) を使用しているときにそのトランザクションを実行しようとして、複数の DbContext から `SaveChanges` を呼び出すと、次のような例外を受け取ります。

> System.InvalidOperationException:構成された実行戦略 'SqlServerRetryingExecutionStrategy' は、ユーザーが開始したトランザクションをサポートしていません。 'DbContext.Database.CreateExecutionStrategy()' から返された実行戦略を使用して、再試行可能なユニットとしてトランザクション内のすべての操作を実行します。

この解決策では、実行する必要があるすべてを表すデリゲートを使用して EF 実行戦略を手動で呼び出します。 一時的なエラーが発生した場合、実行戦略によってデリゲートが再び呼び出されます。 たとえば、次のコードは、製品を更新し、別の DbContext を使用する必要がある ProductPriceChangedIntegrationEvent オブジェクトを保存するときに、2 つの DbContext (\_catalogContext と IntegrationEventLogContext) を使用して eShopOnContainers で実装する方法を示しています。

```csharp
public async Task<IActionResult> UpdateProduct(
    [FromBody]CatalogItem productToUpdate)
{
    // Other code ...

    var oldPrice = catalogItem.Price;
    var raiseProductPriceChangedEvent = oldPrice != productToUpdate.Price;

    // Update current product
    catalogItem = productToUpdate;

    // Save product's data and publish integration event through the Event Bus
    // if price has changed
    if (raiseProductPriceChangedEvent)
    {
        //Create Integration Event to be published through the Event Bus
        var priceChangedEvent = new ProductPriceChangedIntegrationEvent(
          catalogItem.Id, productToUpdate.Price, oldPrice);

       // Achieving atomicity between original Catalog database operation and the
       // IntegrationEventLog thanks to a local transaction
       await _catalogIntegrationEventService.SaveEventAndCatalogContextChangesAsync(
           priceChangedEvent);

       // Publish through the Event Bus and mark the saved event as published
       await _catalogIntegrationEventService.PublishThroughEventBusAsync(
           priceChangedEvent);
    }
    // Just save the updated product because the Product's Price hasn't changed.
    else
    {
        await _catalogContext.SaveChangesAsync();
    }
}
```

最初の <xref:Microsoft.EntityFrameworkCore.DbContext> は `_catalogContext` であり、2 つ目の `DbContext` は `_catalogIntegrationEventService` オブジェクト内にあります。 Commit アクションは、EF 実行戦略を使用してすべての `DbContext` オブジェクト全体で実行されます。

この複数の `DbContext` コミットを達成するために、次のコード例に示すように、`SaveEventAndCatalogContextChangesAsync` には `ResilientTransaction` クラスが使用されます。

```csharp
public class CatalogIntegrationEventService : ICatalogIntegrationEventService
{
    //…
    public async Task SaveEventAndCatalogContextChangesAsync(
        IntegrationEvent evt)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        await ResilientTransaction.New(_catalogContext).ExecuteAsync(async () =>
        {
            // Achieving atomicity between original catalog database
            // operation and the IntegrationEventLog thanks to a local transaction
            await _catalogContext.SaveChangesAsync();
            await _eventLogService.SaveEventAsync(evt,
                _catalogContext.Database.CurrentTransaction.GetDbTransaction());
        });
    }
}
```

基本的に、`ResilientTransaction.ExecuteAsync` メソッドでは、渡された `DbContext` (`_catalogContext`) からのトランザクションを開始し、次に `EventLogService` にそのトランザクションを使用して `IntegrationEventLogContext` からの変更を保存してから、トランザクション全体をコミットします。

```csharp
public class ResilientTransaction
{
    private DbContext _context;
    private ResilientTransaction(DbContext context) =>
        _context = context ?? throw new ArgumentNullException(nameof(context));

    public static ResilientTransaction New (DbContext context) =>
        new ResilientTransaction(context);

    public async Task ExecuteAsync(Func<Task> action)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        var strategy = _context.Database.CreateExecutionStrategy();
        await strategy.ExecuteAsync(async () =>
        {
            using (var transaction = _context.Database.BeginTransaction())
            {
                await action();
                transaction.Commit();
            }
        });
    }
}
```

## <a name="additional-resources"></a>その他の技術情報

- **ASP.NET MVC アプリケーションの EF での接続回復性とコマンド傍受** \
  [https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)

- **Cesar de la Torre。回復力の高い Entity Framework Core SQL 接続とトランザクションの使用** \
  <https://devblogs.microsoft.com/cesardelatorre/using-resilient-entity-framework-core-sql-connections-and-transactions-retries-with-exponential-backoff/>

>[!div class="step-by-step"]
>[前へ](implement-retries-exponential-backoff.md)
>[次へ](use-httpclientfactory-to-implement-resilient-http-requests.md)
