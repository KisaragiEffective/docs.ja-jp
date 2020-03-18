---
title: オーケストレーションのパターン - サーバーレス アプリ
description: Azure Durable Functions
author: cecilphillip
ms.author: cephilli
ms.date: 06/26/2018
ms.openlocfilehash: 2bd81c29e727254af6c8ecf39ee4bfef1f39d009
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72522638"
---
# <a name="orchestration-patterns"></a>オーケストレーションのパターン

Durable Functions を使用すると、サーバーレス環境で、実行時間の長い個別のアクティビティで構成されるステートフル ワークフローを簡単に作成できます。 Durable Functions ではワークフローの進行状況を追跡でき、実行履歴が定期的にチェックポイント処理されるため、いくつかの興味深いパターンを実装するのに役立ちます。

## <a name="function-chaining"></a>関数チェーン

一般的な連続プロセスでは、アクティビティを特定の順序で 1 つずつ実行する必要があります。 必要に応じて、次回のアクティビティで、前の関数からの出力が必要になる場合があります。 このようにアクティビティの順序に依存することで、実行の関数チェーンが作成されます。

Durable Functions を使用してこのワークフロー パターンを実装する利点は、チェックポイント処理を実行できることにあります。 サーバーがクラッシュした場合、ネットワークがタイムアウトした場合、またはその他の問題が発生した場合、Durable Functions では、別のサーバー上にある場合でも最後の既知の状態から再開し、ワークフローの実行を継続することができます。

```csharp
[FunctionName("PlaceOrder")]
public static async Task<string> PlaceOrder([OrchestrationTrigger] DurableOrchestrationContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    await context.CallActivityAsync<bool>("CheckAndReserveInventory", orderData);
    await context.CallActivityAsync<bool>("ProcessPayment", orderData);

    string trackingNumber = await context.CallActivityAsync<string>("ScheduleShipping", orderData);
    await context.CallActivityAsync<string>("EmailCustomer", trackingNumber);

    return trackingNumber;
}
```

前のコード サンプルでは、`CallActivityAsync` 関数により、データ センター内の仮想マシンで特定のアクティビティが実行されます。 await が返され、基になるタスクが完了すると、実行が履歴テーブルに記録されます。 オーケストレーター関数のコードでは、タスク並列ライブラリの任意の使い慣れた構造と async/await キーワードを利用できます。

次のコードは、`ProcessPayment` メソッドがどのようになるかを示す簡単な例です。

```csharp
[FunctionName("ProcessPayment")]
public static bool ProcessPayment([ActivityTrigger] DurableActivityContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    ApplyCoupons(orderData);
    if(IssuePaymentRequest(orderData)) {
        return true;
    }

    return false;
}
```

## <a name="asynchronous-http-apis"></a>非同期 HTTP API

場合によっては、ワークフローに含まれるアクティビティが、完了するのに比較的長い時間がかかることがあります。 BLOB ストレージへのメディア ファイルのバックアップを開始するプロセスを考えてみましょう。 メディア ファイルのサイズと数によっては、このバックアップ プロセスが完了するまでに数時間かかることがあります。

このシナリオでは、実行中のワークフローの状態を確認する `DurableOrchestrationClient` の機能が有用になります。 `HttpTrigger` を使用してワークフローを開始する場合は、`CreateCheckStatusResponse` メソッドを使用して `HttpResponseMessage`のインスタンスを返すことができます。 この応答により、実行中のプロセスの状態を確認するために使用できるペイロードの URI がクライアントに提供されます。

```csharp
[FunctionName("OrderWorkflow")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "POST")]HttpRequestMessage req,
    [OrchestrationClient ] DurableOrchestrationClient orchestrationClient)
{
    OrderRequestData data = await req.Content.ReadAsAsync<OrderRequestData>();

    string instanceId = await orchestrationClient.StartNewAsync("PlaceOrder", data);

    return orchestrationClient.CreateCheckStatusResponse(req, instanceId);
}
```

次のサンプル結果は、応答ペイロードの構造を示しています。

```json
{
    "id": "instanceId",
    "statusQueryGetUri": "http://host/statusUri",
    "sendEventPostUri": "http://host/eventUri",
    "terminatePostUri": "http://host/terminateUri"
}
```

任意の HTTP クライアントを使用して、statusQueryGetUri の URI に対して GET 要求を作成し、実行中のワークフローの状態を調べることができます。 返される状態の応答は、次のコードのようになります。

```json
{
    "runtimeStatus": "Running",
    "input": {
        "$type": "DurableFunctionsDemos.OrderRequestData, DurableFunctionsDemos"
    },
    "output": null,
    "createdTime": "2018-01-01T00:22:05Z",
    "lastUpdatedTime": "2018-01-01T00:22:09Z"
}
```

プロセスが続行されると、状態の応答は **Failed** または **Completed** に変わります。 正常に完了した場合、ペイロード内の **output** プロパティには、返されたデータがすべて含まれます。

## <a name="monitoring"></a>監視

単純な定期的なタスクの場合、Azure Functions では、CRON 式に基づいてスケジュールできる `TimerTrigger` が提供されます。 タイマーは、単純で短時間のタスクには適していますが、より柔軟なスケジュール設定が必要なシナリオもあります。 このようなシナリオでは、監視パターンと Durable Functions が役に立ちます。

Durable Functions では、柔軟なスケジュール設定間隔と有効期間の管理を実現できるほか、1 つのオーケストレーション関数から複数の監視プロセスを作成できます。 この機能のユース ケースの 1 つとして、特定のしきい値が満たされると完了する株価の変動に対してウォッチャーを作成することが考えられます。

```csharp
[FunctionName("CheckStockPrice")]
public static async Task CheckStockPrice([OrchestrationTrigger] DurableOrchestrationContext context)
{
    StockWatcherInfo stockInfo = context.GetInput<StockWatcherInfo>();
    const int checkIntervalSeconds = 120;
    StockPrice initialStockPrice = null;

    DateTime fireAt;
    DateTime exitTime = context.CurrentUtcDateTime.Add(stockInfo.TimeLimit);

    while (context.CurrentUtcDateTime < exitTime)
    {
        StockPrice currentStockPrice = await context.CallActivityAsync<StockPrice>("GetStockPrice", stockInfo);

        if (initialStockPrice == null)
        {
            initialStockPrice = currentStockPrice;
            fireAt = context.CurrentUtcDateTime.AddSeconds(checkIntervalSeconds);
            await context.CreateTimer(fireAt, CancellationToken.None);
            continue;
        }

        decimal percentageChange = (initialStockPrice.Price - currentStockPrice.Price) /
                               ((initialStockPrice.Price + currentStockPrice.Price) / 2);

        if (Math.Abs(percentageChange) >= stockInfo.PercentageChange)
        {
            // Change threshold detected
            await context.CallActivityAsync("NotifyStockPercentageChange", currentStockPrice);
            break;
        }

        // Sleep til next polling interval
        fireAt = context.CurrentUtcDateTime.AddSeconds(checkIntervalSeconds);
        await context.CreateTimer(fireAt, CancellationToken.None);
    }
}
```

`DurableOrchestrationContext` の `CreateTimer` メソッドにより、株価の変動を確認するために、ループの次回の呼び出しのスケジュールが設定されます。 `DurableOrchestrationContext` には、現在の DateTime 値 (UTC) を取得するための `CurrentUtcDateTime` プロパティも含まれます。 テスト用に簡単にモックできるため、`DateTime.UtcNow` の代わりにこのプロパティを使用することをお勧めします。

## <a name="recommended-resources"></a>推奨リソース

- [Azure Durable Functions](https://docs.microsoft.com/azure/azure-functions/durable-functions-overview)
- [.NET Core と .NET Standard の単体テスト](../../core/testing/index.md)

>[!div class="step-by-step"]
>[前へ](durable-azure-functions.md)
>[次へ](serverless-business-scenarios.md)
