---
title: eShopOnContainers を Azure サービスにマッピングする
description: Azure Kubernetes Service、API Gateway、Azure Service Bus などの Azure サービスへの eShopOnContainers のマッピング。
ms.date: 06/30/2019
ms.openlocfilehash: eb37be94461a5373afe328572a94892dec50432d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76781212"
---
# <a name="mapping-eshoponcontainers-to-azure-services"></a>eShopOnContainers を Azure サービスにマッピングする

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

必須ではありませんが、Azure は eShopOnContainers のサポートに適しています。これは、プロジェクトがクラウドネイティブアプリケーションとして構築されたためです。 アプリケーションは .NET Core を使用して構築されているため、Docker ホストに応じて Linux または Windows コンテナーで実行できます。 アプリケーションは複数の自律マイクロサービスで構成され、それぞれに独自のデータがあります。 さまざまなマイクロサービスは、単純な CRUD 操作から複雑な DDD や CQRS パターンまで、さまざまなアプローチを紹介しています。 マイクロサービスは、HTTP 経由でクライアントと通信し、メッセージベースの通信を介して相互に通信します。 アプリケーションでは、標準の通信プロトコルとして HTTP を採用し、Android、iOS、Windows の各プラットフォームで実行される ASP.NET Core および Xamarin mobile アプリを含むため、クライアントの複数のプラットフォームもサポートしています。

図2-5 に、アプリケーションのアーキテクチャを示します。 左側にはクライアントアプリがあり、モバイル、従来の Web、および Web シングルページアプリケーション (SPA) のフレーバーに分割されています。 右側には、システムを構成するサーバー側コンポーネントがあります。各コンポーネントは、Docker コンテナーと Kubernetes クラスターでホストできます。 従来の web アプリには、黄色で示されている ASP.NET Core MVC アプリケーションが搭載されています。 このアプリとモバイルおよび web SPA アプリケーションは、1つまたは複数の API ゲートウェイを介して個々のマイクロサービスと通信します。 API ゲートウェイは、"バックエンドのフロントエンド" (BFF) パターンに従います。つまり、各ゲートウェイは、特定のフロントエンドクライアントをサポートするように設計されています。 個々のマイクロサービスは API ゲートウェイの右側に一覧表示され、ビジネスロジックといくつかの永続化ストアが含まれます。 さまざまなサービスで、SQL Server データベース、Redis cache インスタンス、および MongoDB/CosmosDB ストアを使用します。 一番右には、マイクロサービス間の通信に使用されるシステムのイベントバスがあります。

![eShopOnContainers アーキテクチャ](./media/eshoponcontainers-architecture.png)
**図 2-5**。 EShopOnContainers アーキテクチャ。

このアーキテクチャのサーバー側コンポーネントはすべて、Azure サービスに簡単にマップできます。

## <a name="container-orchestration-and-clustering"></a>コンテナーのオーケストレーションとクラスタリング

Azure Kubernetes Service (AKS) では、アプリケーションのコンテナーでホストされるサービスを ASP.NET Core MVC アプリから個々のカタログおよび注文マイクロサービスに対してホストし、管理することができます。 アプリケーションは Docker と Kubernetes でローカルに実行でき、同じコンテナーを AKS でホストされているステージング環境と運用環境に配置できます。 このプロセスは、次のセクションで説明するように自動化できます。

AKS は、コンテナーの個々のクラスターの管理サービスを提供します。 アプリケーションでは、上のアーキテクチャ図に示されているマイクロサービスごとに個別の AKS クラスターをデプロイします。 この方法では、各サービスがリソース要求に応じて個別にスケールできます。 各マイクロサービスは個別にデプロイすることもできます。このような展開では、システムのダウンタイムをゼロにすることが理想的です。

## <a name="api-gateway"></a>API ゲートウェイ

EShopOnContainers アプリケーションには、複数のフロントエンドクライアントと複数の異なるバックエンドサービスがあります。 クライアントアプリケーションとそれらをサポートするマイクロサービスの間に1対1の対応はありません。 このようなシナリオでは、さまざまなバックエンドサービスとのインターフェイスをセキュリティで保護された方法でクライアントソフトウェアを作成する場合に、複雑になる可能性があります。 各クライアントは、この複雑さに対処する必要があります。その結果、重複が発生し、サービスの変更または新しいポリシーの実装に合わせて、さまざまな場所で更新を行うことができます。

Azure API Management (APIM) を使用すると、組織は一貫した管理しやすい方法で Api を発行できます。 APIM は、API ゲートウェイと管理ポータル (Azure portal) と開発者ポータルの3つのコンポーネントで構成されています。

Api ゲートウェイは API 呼び出しを受け入れ、適切なバックエンド API にルーティングします。 また、API キーや JWT トークンの検証などの追加サービスや、コードを変更せずに実行時の API 変換 (古いインターフェイスを想定しているクライアントに対応するためなど) を提供することもできます。

Azure portal では、API スキーマを定義し、さまざまな Api を製品にパッケージ化します。 また、ユーザーアクセスを構成したり、レポートを表示したり、クォータや変換のポリシーを構成したりすることもできます。

開発者ポータルは、開発者のメインリソースとして機能します。 これにより、開発者は API ドキュメント、対話型のテストコンソール、および独自の使用状況に関するレポートを作成できます。 開発者は、ポータルを使用して、サブスクリプションや API キーのサポートなど、独自のアカウントを作成および管理することもできます。

APIM を使用すると、アプリケーションは複数の異なるサービスグループを公開でき、それぞれが特定のフロントエンドクライアントのバックエンドを提供します。 APIM は複雑なシナリオに使用することをお勧めします。 ニーズを簡単にするために、軽量の API ゲートウェイ Ocelot を使用できます。 EShopOnContainers アプリでは、単純であり、アプリケーション自体と同じアプリケーション環境にデプロイできるため、Ocelot が使用されます。 [EShopOnContainers、APIM、および Ocelot の詳細については、こちらを参照してください。](https://docs.microsoft.com/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern#azure-api-management)

アプリケーションが AKS を使用している場合のもう1つのオプションは、AKS クラスター内のポッドとして Azure ゲートウェイの受信コントローラーをデプロイすることです。 これにより、クラスターを Azure アプリケーションゲートウェイと統合し、ゲートウェイがトラフィックを AKS ポッドに負荷分散できるようになります。 [詳細については、AKS 用の Azure ゲートウェイの受信コントローラーに関するページを参照して](https://github.com/Azure/application-gateway-kubernetes-ingress)ください。

## <a name="data"></a>データ

EShopOnContainers で使用されるさまざまなバックエンドサービスには、異なる記憶域要件があります。 複数のマイクロサービスで SQL Server データベースを使用します。 バスケットマイクロサービスは、永続化のために Redis キャッシュを活用します。 場所マイクロサービスでは、データに MongoDB API が必要です。 Azure では、これらの各データ形式をサポートしています。

SQL Server データベースのサポートのために、Azure には、1つのデータベースから拡張性の高い SQL Database エラスティックプールまで、あらゆるものに対応する製品が用意されています。 個々のマイクロサービスは、個々の SQL Server データベースとすばやく簡単に通信できるように構成できます。 これらのデータベースは、ニーズに応じて各マイクロサービスをサポートするために、必要に応じてスケールすることができます。

EShopOnContainers アプリケーションでは、ユーザーの現在の買い物かごが要求間に保存されます。 これは、データを Redis cache に格納するバスケットマイクロサービスによって管理されます。 開発時には、このキャッシュをコンテナーにデプロイできます。運用環境では、Azure Cache for Redis を利用できます。 Redis の Azure Cache は、完全に管理されたサービスであり、Redis インスタンスやコンテナーを独自にデプロイおよび管理することなく、高いパフォーマンスと信頼性を提供します。

場所マイクロサービスは、その永続化に MongoDB NoSQL データベースを使用します。 開発時には、データベースを独自のコンテナーに配置できます。運用環境では、サービスは[Azure Cosmos DB の MongoDB 用 API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)を活用できます。 Azure Cosmos DB の利点の1つは、SQL API や、MongoDB、Cassandra、NoSQL、Azure Table Storage などの一般的な Api を含む、複数の異なる通信プロトコルを利用できることです。 Azure Cosmos DB は、完全に管理されたグローバルに分散されたデータベースをサービスとして提供し、それを使用するサービスのニーズに合わせて拡張することができます。

クラウドネイティブアプリケーションの分散データの詳細については、[第5章](database-per-microservice.md)を参照してください。

## <a name="event-bus"></a>イベントバス

アプリケーションでは、イベントを使用して異なるサービス間で変更を通知します。 この機能は、さまざまな実装で実装できます。また、ローカルの eShopOnContainers アプリケーションでは、 [RabbitMQ](https://www.rabbitmq.com/)を使用します。 Azure でホストされている場合、アプリケーションはメッセージングのために[Azure Service Bus](https://docs.microsoft.com/azure/service-bus/)を活用します。 Azure Service Bus は、アプリケーションとサービスが分離された信頼性の高い非同期方式で相互に通信できるようにする、完全に管理された統合メッセージブローカーです。 Azure Service Bus は、個々のキューと、パブリッシャーとサブスクライバーのシナリオをサポートするための個別の*トピック*をサポートしています。 EShopOnContainers アプリケーションでは、Azure Service Bus のトピックを活用して、あるマイクロサービスから特定のメッセージに応答するために必要なその他のマイクロサービスへのメッセージの配布をサポートしています。

## <a name="resiliency"></a>柔軟性

運用環境にデプロイされると、eShopOnContainers アプリケーションは、回復性を向上させるために使用できる複数の Azure サービスを活用できるようになります。 アプリケーションは、Application Insights と統合できる正常性チェックを発行して、アプリの可用性に基づいてレポートとアラートを提供します。 Azure リソースには、バグやパフォーマンスの問題を特定して修正するために使用できる診断ログも用意されています。 リソースログは、アプリケーションでさまざまな Azure リソースがどのように使用されているか、およびその方法に関する詳細情報を提供します。 クラウドネイティブの回復性機能の詳細については、[第6章](resiliency.md)を参照してください。

## <a name="references"></a>参照

- [EShopOnContainers アーキテクチャ](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Architecture)
- [高いスケーラビリティと可用性のためにマイクロサービスと複数のコンテナー アプリケーションを調整する](https://docs.microsoft.com/dotnet/architecture/microservices/architect-microservice-container-applications/scalable-available-multi-container-microservice-applications)
- [Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)
- [Azure SQL Database の概要](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
- [Redis 用 Azure キャッシュ](https://azure.microsoft.com/services/cache/)
- [MongoDB 用の Azure Cosmos DB の API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)
- [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview)
- [Azure Monitor の概要](https://docs.microsoft.com/azure/azure-monitor/overview)

>[!div class="step-by-step"]
>[前へ](introduce-eshoponcontainers-reference-app.md)
>[次へ](deploy-eshoponcontainers-azure.md)
