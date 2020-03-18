---
title: サービス メッシュ通信インフラストラクチャ
description: サービスメッシュテクノロジがクラウドネイティブマイクロサービス通信を効率化するしくみについて説明します
author: robvet
ms.date: 09/10/2019
ms.openlocfilehash: 66bc69580cc56efe725683c16a047aeb07e7e840
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76780928"
---
# <a name="service-mesh-communication-infrastructure"></a>サービス メッシュ通信インフラストラクチャ

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

この章では、マイクロサービス通信の課題について詳しく説明しました。 開発チームは、バックエンドサービスが相互に通信する方法を重視する必要があると言いました。 理想的には、サービス間の通信が少なくて済むということです。 ただし、バックエンドサービスが操作の完了に相互に依存していることが多いため、回避することは常に可能であるとは限りません。

同期 HTTP 通信と非同期メッセージングを実装するためのさまざまなアプローチについて説明します。 各ケースでは、開発者はコミュニケーションコードを実装する負担になります。 通信コードは複雑で時間のかかる作業です。 不適切な決定を行うと、パフォーマンスに大きな問題が生じる可能性があります。

*サービスメッシュ*を利用して、新しい急速に進化するテクノロジを中心としたマイクロサービス通信センターのより新しいアプローチ。 [サービスメッシュ](https://www.nginx.com/blog/what-is-a-service-mesh/)は、サービス間の通信、回復性、およびさまざまな横断的懸念を処理する組み込みの機能を備えた、構成可能なインフラストラクチャレイヤーです。 これらの懸念事項については、マイクロサービスからサービスメッシュレイヤーに移行します。 マイクロサービスからは、通信が抽象化されます。

サービスメッシュの重要なコンポーネントはプロキシです。 クラウドネイティブアプリケーションでは、通常、プロキシのインスタンスは各マイクロサービスと共存します。 異なるプロセスで実行されますが、2つは密接にリンクされ、同じライフサイクルを共有します。 このパターンは、サイドカー[パターン](https://docs.microsoft.com/azure/architecture/patterns/sidecar)として知られており、図4-23 に示しています。

![サイドカーを使用したサービスメッシュ](./media/service-mesh-with-side-car.png)

**図 4-23**. サイドカーを使用したサービスメッシュ

前の図では、各マイクロサービスと共に実行されるプロキシによってメッセージが傍受されていることに注意してください。 各プロキシは、マイクロサービスに固有のトラフィックルールを使用して構成できます。 メッセージを認識し、サービスや外部との間でルーティングできます。

サービス間通信の管理と共に、サービスメッシュはサービス検出と負荷分散のサポートを提供します。

構成が完了すると、サービスメッシュが非常に機能します。 メッシュは、サービス探索エンドポイントから、対応するインスタンスのプールを取得します。 このメソッドは、特定のサービスインスタンスに要求を送信し、結果の待機時間と応答の種類を記録します。 これは、最近の要求の待機時間など、さまざまな要因に基づいて高速な応答を返す可能性の高いインスタンスを選択します。

サービスメッシュは、トラフィック、通信、およびネットワークの問題をアプリケーションレベルで管理します。 メッセージと要求を認識します。 通常、サービスメッシュはコンテナー orchestrator と統合されます。 Kubernetes は、サービスメッシュを追加できる拡張可能なアーキテクチャをサポートしています。

6章では、アーキテクチャと使用可能なオープンソースの実装に関する議論など、サービスメッシュテクノロジについて詳しく説明します。

## <a name="summary"></a>要約

この章では、クラウドネイティブの通信パターンについて説明しました。 まず、フロントエンドクライアントがバックエンドマイクロサービスと通信する方法について説明します。 その過程で、API ゲートウェイのプラットフォームとリアルタイム通信について説明します。 次に、マイクロサービスが他のバックエンドサービスと通信する方法について見ていきます。 サービス間の同期 HTTP 通信と非同期メッセージングの両方を検討しました。 GRPC は、クラウドネイティブ環境における今後のテクノロジです。 最後に、マイクロサービスの通信を効率化することができる、サービスメッシュを持つ、急速に進化する新しいテクノロジが導入されました。

クラウドネイティブシステムでの通信の実装に役立つ、管理された Azure サービスに特に重点を置いていました。

- [Azure アプリケーションゲートウェイ](https://docs.microsoft.com/azure/application-gateway/overview)
- [Azure API Management](https://azure.microsoft.com/services/api-management/)
- [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/)
- [Azure Storage キュー](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction)
- [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview)
- [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview)
- [Azure イベントハブ](https://azure.microsoft.com/services/event-hubs/)

次に、クラウドネイティブシステムの分散データと、それが提示する利点と課題に移ります。

### <a name="references"></a>参照

- [.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [マイクロサービスのサービス間通信の設計](https://docs.microsoft.com/azure/architecture/microservices/design/interservice-communication)

- [リアルタイム機能を追加するための完全に管理されたサービスである Azure SignalR サービス](https://azure.microsoft.com/blog/azure-signalr-service-a-fully-managed-service-to-add-real-time-functionality/)

- [Azure API ゲートウェイの受信コントローラー](https://azure.github.io/application-gateway-kubernetes-ingress/)

- [Azure Kubernetes Service の受信について (AKS)](https://vincentlauzon.com/2018/10/10/about-ingress-in-azure-kubernetes-service-aks/)

- [実用的な gRPC](https://www.worldcat.org/title/practical-grpc/oclc/1042342319)

- [gRPC のドキュメント](https://grpc.io/docs/guides/)

- [WCF 開発者向け grpc](https://bing.com) [Mark'S grpc book]

- [GRPC サービスと HTTP Api の比較](https://docs.microsoft.com/aspnet/core/grpc/comparison?view=aspnetcore-3.0)

>[!div class="step-by-step"]
>[前へ](rest-grpc.md)
>[次へ](Database-per-microservice.md)
