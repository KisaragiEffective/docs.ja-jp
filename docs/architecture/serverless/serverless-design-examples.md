---
title: サーバーレス設計の例 - サーバーレス アプリ
description: スケジュール設定やイベントベースの処理からファイル トリガーやストリーム プロセスまで、サーバーレス アーキテクチャでサポートされるさまざまなシナリオについて説明します。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: b4e8fda0c1423c881c0807602e11f7c49ff7cfe4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73093555"
---
# <a name="serverless-design-examples"></a>サーバーレス設計の例

サーバーレス用に存在する設計パターンは数多くあります。 このセクションでは、サーバーレスを使用する一般的なシナリオをいくつか紹介します。 すべての例で共通しているのは、イベント トリガーとビジネス ロジックの基本的な組み合わせです。

## <a name="scheduling"></a>スケジュール設定

タスクのスケジュール設定は、よくある機能です。 次の図は、適切な整合性チェックが行われていないレガシ データベースを示しています。 データベースは定期的にスクラブされる必要があります。 サーバーレス関数により、無効なデータが検出され、消去されます。 トリガーは、スケジュールに基づいてコードを実行するタイマーです。

![サーバーレス スケジューリング](./media/serverless-scheduling.png)

## <a name="command-and-query-responsibility-segregation-cqrs"></a>コマンド クエリ責務分離 (CQRS)

コマンド クエリ責務分離 (CQRS) とは、データの読み取り (クエリ) とデータを変更する操作に異なるインターフェイスを提供するパターンです。 これで、いくつかの一般的な問題に対処します。 従来の作成、読み取り、更新、削除 (CRUD) ベースのシステムでは、同じデータ ストアに対する読み取りと書き込みの両方が大量に行われ、競合が発生する可能性があります。 ロックが頻繁に発生し、読み取り速度が大幅に低下することがあります。 多くの場合、データは複数のドメイン オブジェクトの複合物として表され、読み取り操作では異なるエンティティのデータを結合する必要があります。

CQRS を使用すると、読み取りで、使用方法によってデータをモデル化する特別な "フラット化された" エンティティが含まれる場合があります。 読み取りは、格納方法とは異なる方法で処理されます。 たとえば、データベースでは、連絡先を子の住所レコードを持つヘッダー レコードとして格納することができますが、読み取りでは、ヘッダーと住所の両方のプロパティを持つエンティティを含めることができます。 読み取りモデルを作成する方法は多数あります。 ビューから具体化されることがあります。 更新操作は、分離されたイベントとしてカプセル化して、2 つの異なるモデルに対する更新をトリガーすることができます。 読み取りと書き込みには異なるモデルが存在します。

![CQRS の例](./media/cqrs-example.png)

サーバーレスでは、分離されたエンドポイントを提供することで、CQRS パターンに対応できます。 あるサーバーレス関数がクエリまたは読み取りに対応し、別のサーバーレス関数または関数セットで更新操作を処理します。 また、サーバーレス関数は、読み取りモデルを最新の状態に保つ必要があり、データベースの[変更フィード](https://docs.microsoft.com/azure/cosmos-db/change-feed)によりトリガーできます。 フロントエンド開発は、必要なエンドポイントに接続するために簡略化されます。 イベントの処理はバックエンドで処理されます。 さまざまなチームが異なる操作を実行する可能性があるため、このモデルは大規模なプロジェクトにも適しています。

## <a name="event-based-processing"></a>イベントベースの処理

メッセージベースのシステムでは、多くの場合、イベントはキューまたはパブリッシャー/サブスクライバーのトピックで収集され、処理されます。 これらのイベントでは、サーバーレス関数をトリガーしてビジネス ロジックを実行することができます。 イベントベースの処理の例としては、イベントソースのシステムがあります。 "イベント" が発生して、タスクを完了としてマークします。 1 つのサーバーレス関数がイベントによってトリガーされ、該当するデータベース ドキュメントが更新されます。 2 番目のサーバーレス関数で、イベントを使用してシステムの読み取りモデルを更新できます。 [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) により、イベントをサブスクライバーとして関数に統合する方法が提供されます。

> イベントは情報メッセージです。 詳細については、「[イベント ソーシング パターン](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)」を参照してください。

## <a name="file-triggers-and-transformations"></a>ファイル トリガーと変換

抽出、変換、読み込み (ETL) は、一般的なビジネス機能です。 サーバーレスは、パイプラインの一部としてコードをトリガーできるため、ETL に適したソリューションです。 個々のコード コンポーネントで、さまざまな側面に対応できます。 あるサーバーレス関数でファイルをダウンロードし、別の関数で変換を適用し、さらに別の関数でデータを読み込みます。 コードを個別にテストしてデプロイできるため、必要に応じた保守とスケーリングが容易になります。

![サーバーレスのファイル トリガーと変換](./media/serverless-file-triggers.png)

図中の "クール ストレージ" により、[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics) で解析されるデータが提供されます。 データ ストリームで問題が発生すると、異常に対処する Azure 関数がトリガーされます。

## <a name="asynchronous-background-processing-and-messaging"></a>非同期のバックグラウンド処理とメッセージング

非同期メッセージングおよびバックグラウンド処理を使用すると、アプリケーションは待機することなくプロセスを開始できます。 非同期処理の例としては、OCR アプリがあります。 イメージが送信され、処理のためにキューに入れられます。 イメージをスキャンしてテキストを抽出するには時間がかかる場合があり、完了すると通知が送信されます。 サーバーレスでは、このシナリオの呼び出しと結果の両方を処理できます。

## <a name="web-apps-and-apis"></a>Web アプリと API

サーバーレスの一般的なシナリオは n 層アプリケーションで、最も一般的なシナリオでは UI レイヤーが Web アプリです。 シングル ページ アプリケーション (SPA) の人気が最近急上昇しています。 SPA アプリでは、1 ページをレンダリングしてから、API 呼び出しと返されたデータに応じて新しい UI を動的にレンダリングし、ページ全体の再読み込みはしません。 クライアント側のレンダリングにより、より高速で応答性の高いアプリケーションがエンド ユーザーに提供されます。

HTTP 呼び出しによってトリガーされるサーバーレス エンドポイントを使用して、API 要求を処理できます。 たとえば、広告サービス会社は、ユーザー プロファイル情報を使用してサーバーレス関数を呼び出して、カスタム広告を要求できます。 サーバーレス関数からカスタム広告が返され、Web ページでレンダリングされます。

![サーバーレス Web API](./media/serverless-web-api.png)

## <a name="data-pipeline"></a>データ パイプライン

サーバーレス関数を使用して、データ パイプラインを容易にできます。 次の例では、ファイルによって関数がトリガーされ、CSV ファイルのデータがテーブルのデータ行に変換されます。 編成されたテーブルにより、エンド ユーザーは Power BI ダッシュボードで分析を表示できます。

![サーバーレス データ パイプライン](./media/serverless-data-pipeline.png)

## <a name="stream-processing"></a>ストリーム処理

デバイスとセンサーでは、多くの場合、リアルタイムで処理する必要があるデータのストリームが生成されます。 [Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs) と [IoT Hub](https://docs.microsoft.com/azure/iot-hub) から [Service Bus](https://docs.microsoft.com/azure/service-bus) にメッセージとストリームをキャプチャできるテクノロジが数多くあります。 サーバーレスは、トランスポートを問わず、メッセージとデータ ストリームを受信したときの処理に最適なメカニズムです。 サーバーレスは、大量のデータ需要に合わせて迅速にスケールできます。 サーバーレス コードで、ビジネス ロジックを適用してデータを解析し、アクションと分析用に構造化された形式で出力することができます。

![サーバーレス ストリーム処理](./media/serverless-stream-processing.png)

## <a name="api-gateway"></a>API ゲートウェイ

API ゲートウェイは、クライアントに対して単一のエントリ ポイントが提供され、要求がバックエンド サービスにインテリジェントにルーティングされます。 大規模なサービス セットの管理に有用です。 また、クライアントをさまざまな環境に簡単に接続することで、バージョン管理を処理し、開発を簡素化することもできます。 サーバーレスでは、個々のマイクロサービスのバックエンド スケーリングを処理しながら、API ゲートウェイを介して単一のフロントエンドを提供できます。

![サーバーレス API ゲートウェイ](./media/serverless-api-gateway.png)

## <a name="recommended-resources"></a>推奨リソース

- [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview)
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)
- [分散データ管理に関する課題とソリューション](../microservices/architect-microservice-container-applications/distributed-data-management.md)
- [マイクロサービスの設計: マイクロサービス境界の識別](https://docs.microsoft.com/azure/architecture/microservices/microservice-boundaries)
- [Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
- [イベント ソーシング パターン](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)
- [直流高速度遮断器パターンの実装](../microservices/implement-resilient-applications/implement-circuit-breaker-pattern.md)
- [IoT Hub](https://docs.microsoft.com/azure/iot-hub)
- [Service Bus](https://docs.microsoft.com/azure/service-bus)
- [Azure Cosmos DB での Change Feed サポートの使用](https://docs.microsoft.com/azure/cosmos-db/change-feed)

>[!div class="step-by-step"]
>[前へ](serverless-architecture-considerations.md)
>[次へ](azure-serverless-platform.md)
