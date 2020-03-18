---
title: 持続性のある Azure Functions - サーバーレス アプリ
description: 持続性のある Azure Functions によって Azure Functions ランタイムを拡張し、コード内でステートフル ワークフローを有効にすることができます。
author: cecilphillip
ms.author: cephilli
ms.date: 06/26/2018
ms.openlocfilehash: 2c0ad086640409ac187c3aa882add4d6b39b6ff9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72522855"
---
# <a name="durable-azure-functions"></a>永続的な Azure Functions

Azure Functions でサーバーレス アプリケーションを作成する場合、通常、操作はステートレスな方法で実行するように設計されます。 この設計を選択する理由は、プラットフォームの規模が大きくなると、コードが実行されているサーバーを知ることが難しくなるためです。 また、ある特定の時点でアクティブになっているインスタンスの数を把握することも困難になります。 その一方で、プロセスの現在の状態を把握することが必要なアプリケーションの種類があります。 オンライン ストアに注文を送信するプロセスを考えてみましょう。 そのチェックアウトの操作は、複数の操作で構成された 1 つのワークフローであり、各操作でプロセスの状態がわかっていることが必要になる可能性があります。 このような情報として、製品の在庫、顧客のアカウントに残高があるかどうか、クレジット カード処理の結果などが考えられます。 これらの操作はおそらく、独自の内部ワークフローです。または、サードパーティ システムのサービスであるかもしれません。

現在、内部システムと外部システムとの間でアプリケーションの状態を調整するのを支援する、さまざまなパターンが存在します。 よくあるのは、一元化されたキュー システム、分散型のキー値ストア、または共有データベースを使用してその状態を管理するソリューションです。 ただし、これらはすべて、プロビジョニングと管理が必要な追加リソースです。 サーバーレス環境では、これらのリソースを手動で調整しようとすると、コードが煩雑になる可能性があります。 Azure Functions には、Durable Functions と呼ばれるステートフル関数を作成するための代替手段が用意されています。

Durable Functions は、コードでステートフル ワークフローを定義できるようにする、Azure Functions ランタイムの拡張機能です。 Durable Functions 拡張機能を使用すると、ワークフローをアクティビティに分割することにより、状態の管理、進行状況のチェックポイントの作成、サーバー間での関数呼び出しの分散の処理を行うことができます。 バックグラウンドでは、Azure ストレージ アカウントを使用して、実行履歴の保持、アクティビティ関数のスケジュール設定、応答の取得が行われます。 サーバーレス コードでは、そのストレージ アカウントの永続化された情報を操作することはできません。通常、開発者が操作する必要のあるものではありません。

## <a name="triggering-a-stateful-workflow"></a>ステートフル ワークフローのトリガー

Durable Functions のステートフル ワークフローは、オーケストレーション トリガーとアクティビティ トリガーという 2 つの組み込みコンポーネントに分けることができます。 トリガーとバインドは、Azure Functions によって使用されるコア コンポーネントです。トリガーとバインドによって、サーバーレス関数に開始、入力の受け取り、結果の返送を行うきっかけを通知できます。

### <a name="working-with-the-orchestration-client"></a>オーケストレーション クライアントの操作

オーケストレーションは、Azure Functions でトリガーされる他のスタイルの操作と比較した場合、独特なものです。 Durable Functions では、完了までに数時間または数日かかる可能性のある関数を実行できます。 この種類の動作では、実行中のオーケストレーションの状態の確認、プリエンプティブな終了、外部イベントの通知の送信ができる必要があります。

このような場合のために、Durable Functions 拡張機能には、調整された関数を操作できる `DurableOrchestrationClient` クラスが用意されています。 オーケストレーション クライアントにアクセスするには、`OrchestrationClientAttribute` バインドを使用します。 通常、この属性は、`HttpTrigger` や `ServiceBusTrigger` など、別の種類のトリガーに含めます。 ソース関数がトリガーされたら、オーケストレーション クライアントを使用してオーケストレーター関数を開始できます。

```csharp
[FunctionName("KickOff")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "POST")]HttpRequestMessage req,
    [OrchestrationClient ] DurableOrchestrationClient<orchestrationClient>)
{
    OrderRequestData data = await req.Content.ReadAsAsync<OrderRequestData>();

    string instanceId = await orchestrationClient.StartNewAsync("PlaceOrder", data);

    return orchestrationClient.CreateCheckStatusResponse(req, instanceId);
}
```

### <a name="the-orchestrator-function"></a>オーケストレーター関数

Azure Functions の OrchestrationTriggerAttribute を使用して関数に注釈を付けると、その関数がオーケストレーター関数としてマークされます。 ステートフル ワークフローを構成するさまざまなアクティビティを管理する責任を負います。

オーケストレーター関数で OrchestrationTriggerAttribute 以外のバインドを使用することはできません。 この属性は、パラメーターの種類が DurableOrchestrationContext の場合にのみ使用できます。 関数シグネチャの入力の逆シリアル化がサポートされていないため、その他の入力を使用することはできません。 オーケストレーター クライアントによって提供される入力を取得するには、GetInput\<T\> メソッドを使用する必要があります。

また、オーケストレーション関数の戻り値の型は、void、タスク、または JSON のシリアル化可能な値のいずれかである必要があります。

> *簡潔にするため、エラー処理コードは省略されています*

```csharp
[FunctionName("PlaceOrder")]
public static async Task<string> PlaceOrder([OrchestrationTrigger] DurableOrchestrationContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    await context.CallActivityAsync<bool>("CheckAndReserveInventory", orderData);
    await context.CallActivityAsync<string>("ProcessPayment", orderData);

    string trackingNumber = await context.CallActivityAsync<string>("ScheduleShipping", orderData);
    await context.CallActivityAsync<string>("EmailCustomer", trackingNumber);

    return trackingNumber;
}
```

オーケストレーションの複数のインスタンスを同時に開始して実行できます。 `StartNewAsync` で `DurableOrchestrationClient` メソッドを呼び出すと、オーケストレーションの新しいインスタンスが起動されます。 そのメソッドからは、オーケストレーションが開始されたときに完了する `Task<string>` が返されます。 オーケストレーションが 30 秒以内に開始されなかった場合は、`TimeoutException` 型の例外がスローされます。

`Task<string>` から完了した `StartNewAsync` には、オーケストレーション インスタンスの一意の ID が含まれているはずです。 このインスタンス ID を使用して、その特定のオーケストレーションに対する操作を呼び出すことができます。 オーケストレーションに状態を照会したり、イベント通知を送信したりすることができます。

### <a name="the-activity-functions"></a>アクティビティ関数

アクティビティ関数は、ワークフローを作成するためにオーケストレーション関数内にまとめて構成される個別の操作です。 実際の作業のほとんどがここで行われます。 ビジネス ロジック、長時間実行されるプロセス、大規模なソリューションに対するパズルのピースを表します。

`ActivityTriggerAttribute` は、`DurableActivityContext` 型の関数パラメーターに注釈を付けるために使用されます。 注釈を使用すると、アクティビティ関数としての使用を意図した関数であることを、ランタイムに通知できます。 アクティビティ関数への入力値は、`GetInput<T>` パラメーターの `DurableActivityContext` メソッドを使用して取得されます。

オーケストレーション関数と同様に、アクティビティ関数の戻り値の型は、void、タスク、または JSON のシリアル化可能な値のいずれかである必要があります。

アクティビティ関数内でスローされたハンドルされない例外は、呼び出し元のオーケストレーター関数に送信され、`TaskFailedException` として示されます。 この時点でエラーを捕捉し、オーケストレーターのログに記録して、アクティビティを再試行できます。

```csharp
[FunctionName("CheckAndReserveInventory")]
public static bool CheckAndReserveInventory([ActivityTrigger] DurableActivityContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    // Connect to inventory system and try to reserve items
    return true;
}
```

## <a name="recommended-resources"></a>推奨リソース

- [Durable Functions](https://docs.microsoft.com/azure/azure-functions/durable-functions-overview)
- [Durable Functions のバインド](https://docs.microsoft.com/azure/azure-functions/durable-functions-bindings)
- [Durable Functions でのインスタンスの管理](https://docs.microsoft.com/azure/azure-functions/durable-functions-instance-management)

>[!div class="step-by-step"]
>[前へ](event-grid.md)
>[次へ](orchestration-patterns.md)
