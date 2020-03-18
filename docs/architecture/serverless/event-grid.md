---
title: Azure Event Grid - サーバーレス アプリ
description: Azure Event Grid は、イベント単位の従量課金モデルで信頼性の高いイベント配信と大規模なルーティングを実現するサーバーレス ソリューションです。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 3c577139c12567e762aabd58c9dc29457fa37aa1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72522718"
---
# <a name="event-grid"></a>Event Grid

[Azure Event Grid](/azure/event-grid/overview) では、イベントベースのアプリケーションのサーバーレス インフラストラクチャが提供されます。 任意のソースから Event Grid への発行を行い、任意のプラットフォームからのメッセージを使用できます。 また、Event Grid には、アプリケーションとの統合を効率化するために、Azure リソースのイベントに対するサポートが組み込まれています。 たとえば、BLOB ストレージ イベントをサブスクライブして、ファイルがアップロードされたときにアプリに通知することができます。 その後、アプリケーションでは、他のクラウドまたはオンプレミス アプリケーションで使用されるカスタムの Event Grid メッセージを発行できます。 Event Grid は、大規模な処理を確実に処理するように構築されています。 必要なインフラストラクチャを設定するオーバーヘッドを発生させることなく、メッセージの発行とサブスクライブの利点を得ることができます。

![Event Grid ロゴ](./media/event-grid-logo.png)

Event Grid の主な特徴は次のとおりです。

- 完全に管理されたイベント ルーティング。
- ほぼリアルタイムの大規模なイベント配信。
- Azure の内部と外部の両方で幅広くカバー。

## <a name="scenarios"></a>シナリオ

Event Grid では、いくつかの異なるシナリオに対処できます。 このセクションでは、最も一般的な 3 つのシナリオについて説明します。

### <a name="ops-automation"></a>操作の自動化

![操作の自動化](./media/ops-automation.png)

Event Grid を使用すると、インフラストラクチャがプロビジョニングされたときに [Azure Automation](https://docs.microsoft.com/azure/automation) に通知することで、自動化を高速化し、ポリシーの適用を簡素化できます。

### <a name="application-integration"></a>アプリケーションの統合

![アプリケーションの統合](./media/app-integration.png)

Event Grid を使用して、アプリを他のサービスに接続できます。 標準の HTTP プロトコルを使用すると、従来のアプリでも、Event Grid メッセージを発行するように簡単に変更できます。 Web フックを使用すると、他のサービスやプラットフォームで Event Grid メッセージを使用できるようになります。

### <a name="serverless-apps"></a>サーバーレス アプリ

![サーバーレス アプリ](./media/serverless-apps.png)

Event Grid により、Azure Functions、Logic Apps、または独自のカスタム コードをトリガーできます。 Event Grid を使用する主な利点は、*プッシュ* メカニズムを使用して、イベントが発生したときにメッセージを送信することにあります。 プッシュ アーキテクチャは、*ポーリング* メカニズムよりも使用されるリソースが少なく、拡張性に優れています。 ポーリングでは、定期的に更新プログラムをチェックする必要があります。

## <a name="event-grid-vs-other-azure-messaging-services"></a>Event Grid と他の Azure メッセージング サービス

Azure には、[Event Hubs](https://docs.microsoft.com/azure/event-hubs) や [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging) など、いくつかのメッセージング サービスが用意されています。 それぞれのサービスは、特定のユース ケース セットに対処するように設計されています。 次の図は、サービス間の相違点の概要を示しています。

![Azure メッセージングの比較](./media/azure-messaging-services.png)

詳細な比較については、[メッセージング サービスの比較](https://docs.microsoft.com/azure/event-grid/compare-messaging-services)に関するページを参照してください。

## <a name="performance-targets"></a>パフォーマンスの目標

Event Grid を使用すると、次のパフォーマンスの保証を活用できます。

- 99 パーセンタイルで秒未満のエンドツーエンドの待機時間。
- 99.99% の可用性。
- リージョンあたり 1 秒間に 1000 万のイベント。
- リージョンあたり 1 億のサブスクライブ数。
- 50 ミリ秒の発行元の待機時間。
- 1 日のウィンドウで保証された配信を行うための指数バックオフによる 24 時間の再試行。
- 透過的なリージョン内フェールオーバー。

## <a name="event-grid-schema"></a>Event Grid スキーマ

Event Grid では、標準スキーマを使用してカスタムイベントがラップされます。 スキーマは、カスタム データ要素をラップするエンベロープに似ています。 Event Grid メッセージの例を次に示します。

```json
[{
    "id": "03e24f21-a955-43cc-8921-1f61a6081ce0",
    "eventType": "myCustomEvent",
    "subject": "foo/bar/12",
    "eventTime": "2018-09-22T10:36:01+00:00",
    "data": {
        "favoriteColor": "blue",
        "favoriteAnimal": "panther",
        "favoritePlanet": "Jupiter"
    },
    "dataVersion": "1.0"
}]
```

メッセージに関するすべての項目は、`data` プロパティを除き、標準です。 メッセージを調べ、`eventType` と `dataVersion` を使用して、ペイロードのカスタム部分を逆シリアル化することができます。

## <a name="azure-resources"></a>管理

Event Grid を使用する主な利点は、Azure によって生成される自動メッセージにあります。 Azure では、さまざまなイベントをサブスクライブできるように、リソースが*トピック*に自動的に発行されます。 次の表は、自動的に使用できるリソースの種類、メッセージの種類、およびイベントを示しています。

| Azure リソース | イベントの種類 | 説明 |
| -------------- | ---------- | ----------- |
| Azure サブスクリプション | Microsoft.Resources.ResourceWriteSuccess | リソースの作成または更新操作が成功したときに発生します。 |
| | Microsoft.Resources.ResourceWriteFailure | リソースの作成または更新操作が失敗したときに発生します。 |
| | Microsoft.Resources.ResourceWriteCancel | リソースの作成または更新操作が取り消されたときに発生します。 |
|  | Microsoft.Resources.ResourceDeleteSuccess | リソースの削除操作が成功したときに発生します。 |
|  | Microsoft.Resources.ResourceDeleteFailure | リソースの削除操作が失敗したときに発生します。 |
| | Microsoft.Resources.ResourceDeleteCancel | リソースの削除操作が取り消されたときに発生します。 このイベントは、テンプレートのデプロイが取り消された場合に発生します。 |
| BLOB ストレージ | Microsoft.Storage.BlobCreated | BLOB が作成されたときに発生します。 |
| | Microsoft.Storage.BlobDeleted | BLOB が削除されたときに発生します。 |
| Event Hubs | Microsoft.EventHub.CaptureFileCreated | キャプチャ ファイルが作成されたときに発生します。
| IoT Hub | Microsoft.Devices.DeviceCreated | デバイスが IoT Hub に登録されると発行されます。 |
| | Microsoft.Devices.DeviceDeleted | デバイスが IoT Hub から削除されると発行されます。 |
| リソース グループ | Microsoft.Resources.ResourceWriteSuccess | リソースの作成または更新操作が成功したときに発生します。 |
| | Microsoft.Resources.ResourceWriteFailure | リソースの作成または更新操作が失敗したときに発生します。 |
| | Microsoft.Resources.ResourceWriteCancel | リソースの作成または更新操作が取り消されたときに発生します。 |
| | Microsoft.Resources.ResourceDeleteSuccess | リソースの削除操作が成功したときに発生します。 |
| | Microsoft.Resources.ResourceDeleteFailure | リソースの削除操作が失敗したときに発生します。 |
| | Microsoft.Resources.ResourceDeleteCancel | リソースの削除操作が取り消されたときに発生します。 このイベントは、テンプレートのデプロイが取り消された場合に発生します。 |

詳細については、「[Azure Event Grid イベント スキーマ](https://docs.microsoft.com/azure/event-grid/event-schema)」を参照してください。

オンプレミスで実行されているアプリケーションも含め、任意の種類のアプリケーションから Event Grid にアクセスできます。

## <a name="conclusion"></a>まとめ

この章では、Azure Functions、Logic Apps、および Event Grid で構成される Azure サーバーレス プラットフォームについて学習しました。 これらのリソースを使用して、完全なサーバーレス アプリ アーキテクチャを構築したり、他のクラウド リソースやオンプレミス サーバーと対話するハイブリッド ソリューションを作成したりできます。 [Azure SQL](https://docs.microsoft.com/azure/sql-database) や [CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction) などのサーバーレス データ プラットフォームと組み合わせることで、完全に管理されたクラウド ネイティブ アプリケーションをビルドできます。

## <a name="recommended-resources"></a>推奨リソース

- [App Service のプラン](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)
- [Application Insights](https://docs.microsoft.com/azure/application-insights)
- [Application Insights Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
- [Azure:サーバーレスの Azure Functions を使用してアプリをクラウドに持ち込む](https://channel9.msdn.com/events/Connect/2017/E102)
- [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview)
- [Azure Event Grid イベント スキーマ](https://docs.microsoft.com/azure/event-grid/event-schema)
- [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs)
- [Azure Functions のドキュメント](https://docs.microsoft.com/azure/azure-functions)
- [Azure Functions でのトリガーとバインドの概念](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)
- [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps)
- [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging)
- [Azure Table Storage](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview)
- [Functions 1.x と 2.x の比較](https://docs.microsoft.com/azure/azure-functions/functions-versions)
- [Azure のオンプレミス データ ゲートウェイを使用してオンプレミスのデータ ソースに接続する](https://docs.microsoft.com/azure/analysis-services/analysis-services-gateway)
- [Azure portal で初めての関数を作成する](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function)
- [Azure CLI での初めての関数の作成](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function-azure-cli)
- [Visual Studio での初めての関数の作成](https://docs.microsoft.com/azure/azure-functions/functions-create-your-first-function-visual-studio)
- [Functions でサポートされる言語](https://docs.microsoft.com/azure/azure-functions/supported-languages)
- [Azure Functions の監視](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
- [Azure Functions プロキシの操作](https://docs.microsoft.com/azure/azure-functions/functions-proxies)

>[!div class="step-by-step"]
>[前へ](logic-apps.md)
>[次へ](durable-azure-functions.md)
