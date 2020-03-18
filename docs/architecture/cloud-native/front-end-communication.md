---
title: フロントエンドのクライアント通信
description: フロントエンドクライアントがクラウドネイティブシステムと通信する方法について説明します。
author: robvet
ms.date: 09/08/2019
ms.openlocfilehash: a488337b48e30b99bfcc9894a780350f32af864f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73841793"
---
# <a name="front-end-client-communication"></a>フロントエンドのクライアント通信

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

クラウドネイティブシステムでは、フロントエンドクライアント (モバイル、web、およびデスクトップアプリケーション) は、独立したバックエンドマイクロサービスと対話するために通信チャネルを必要とします。  

オプションにはどのようなものがありますか。

単純にするために、フロントエンドクライアントは、図4-2 に示すように、バックエンドマイクロサービスと*直接通信*できます。

![クライアントからサービスへの直接通信](./media/direct-client-to-service-communication.png)

**図 4-2** クライアントからサービスへの直接通信

この方法では、各マイクロサービスに、フロントエンドクライアントがアクセスできるパブリックエンドポイントがあります。 運用環境では、マイクロサービスの前にロードバランサーを配置し、トラフィックを比例的にルーティングします。

簡単に実装できますが、ダイレクトクライアント通信は、単純なマイクロサービスアプリケーションに対してのみ許容されます。 このパターンは、フロントエンドクライアントをコアバックエンドサービスに密に結合し、次のようなさまざまな問題のドアを開きます。

- バックエンドサービスのリファクタリングに対するクライアントの弱点。
- コアバックエンドサービスが直接公開されるという、より広範な攻撃対象領域。
- 各マイクロサービス間の横断的な問題の複製。
- 過度に複雑なクライアントコード-クライアントは、複数のエンドポイントを追跡し、回復力のある方法でエラーを処理する必要があります。

広く受け入れられているクラウド設計パターンは、フロントエンドアプリケーションとバックエンドサービスの間に[API ゲートウェイサービス](../microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern.md)を実装することです。 このパターンを図4-3 に示します。

![API ゲートウェイパターン](./media/api-gateway-pattern.png)

**図 4-3.** API ゲートウェイパターン

前の図では、API ゲートウェイサービスがバックエンドコアマイクロサービスをどのように抽象化しているかに注目してください。 Web API として実装され、*リバースプロキシ*として機能し、受信トラフィックを内部マイクロサービスにルーティングします。

ゲートウェイは、クライアントを内部サービスのパーティション分割とリファクタリングから分離します。 バックエンドサービスを変更した場合は、クライアントを中断することなく、ゲートウェイに格納することができます。 また、id、キャッシュ、回復性、測定、調整などの横断的な問題に対する防御の第1行です。 これらの横断的な問題の多くは、バックエンドコアサービスからゲートウェイにオフロードすることで、バックエンドサービスを簡素化することができます。

API ゲートウェイを簡単かつ迅速に維持するには、慎重に行う必要があります。 通常、ビジネスロジックはゲートウェイから保持されます。 複雑なゲートウェイのリスクはボトルネックになり、最終的にはモノリス自体になります。 多くの場合、大規模なシステムでは、クライアントの種類 (モバイル、web、デスクトップ) またはバックエンドの機能によってセグメント化された複数の API ゲートウェイを公開します。 [フロントエンドのバックエンド](https://docs.microsoft.com/azure/architecture/patterns/backends-for-frontends)パターンは、複数のゲートウェイを実装するための方向を提供します。 このパターンを図4-4 に示します。

![API ゲートウェイパターン](./media/backend-for-frontend-pattern.png)

**図 4-4.** フロントエンドパターンのバックエンド

前の図では、クライアントの種類 (web、モバイル、またはデスクトップアプリ) に基づいて、着信トラフィックが特定の API ゲートウェイに送信されることに注意してください。 この方法は、各デバイスの機能がフォームファクター、パフォーマンス、および表示の制限を超えて大幅に異なるため、意味があります。 通常、モバイルアプリケーションは、ブラウザーやデスクトップアプリケーションよりも少ない機能を公開します。 各ゲートウェイは、対応するデバイスの機能に合わせて最適化できます。

最初に、独自の API ゲートウェイサービスを構築できます。 GitHub のクイック検索には、多くの例が用意されています。 ただし、いくつかのフレームワークと市販のゲートウェイ製品が用意されています。

## <a name="ocelot-gateway"></a>Ocelot ゲートウェイ

単純な .NET クラウドネイティブアプリケーションの場合は、 [Ocelot ゲートウェイ](https://github.com/ThreeMammals/Ocelot)を検討してください。 Ocelot は、.NET マイクロサービス用に作成されたオープンソース API ゲートウェイで、システムへの統合されたポイントの入力が必要です。 軽量で高速でスケーラブルです。

API ゲートウェイと同様、主な機能は、受信 HTTP 要求を下流のサービスに転送することです。 また、.NET Core ミドルウェアパイプラインで構成可能なさまざまな機能をサポートしています。 その機能セットを次の表に示します。

|Ocelot の機能  | |
| :-------- | :-------- |
| ルーティング | 認証 |
| 要求の集計 | Authorization |
| サービスの検出 (Consul と Eureka を使用) | 調整 |
| 負荷分散 | ログ記録, トレース |
| キャッシュ | ヘッダー/クエリ文字列の変換 |
| 相関関係パススルー | カスタムミドルウェア |
| サービス品質 | 再試行ポリシー |

各 Ocelot ゲートウェイは、JSON 構成ファイルの上流および下流のアドレスと構成可能な機能を指定します。 クライアントは、HTTP 要求を Ocelot ゲートウェイに送信します。 Ocelot は、受け取った後、そのパイプラインを介して HttpRequest オブジェクトを渡し、その構成によって指定された状態にします。 パイプラインの最後に、Ocelot によって新しい HTTPResponseObject が作成され、下流のサービスに渡されます。 応答の場合、Ocelot はパイプラインを反転し、応答をクライアントに送り返します。

Ocelot は NuGet パッケージとして入手できます。 NET Standard 2.0 を対象としており、.NET Core 2.0 + と .NET Framework 4.6.1 + ランタイムの両方と互換性があります。 Ocelot は、HTTP を話す任意のものと統合されており、.NET Core がサポートしているプラットフォーム (Linux、macOS、Windows) で実行されます。 Ocelot は拡張可能であり、Docker コンテナー、Azure Kubernetes Services、その他のパブリッククラウドなどの最新のプラットフォームを多数サポートしています。  Ocelot は、 [Consul](https://www.consul.io)、 [Graphql](https://graphql.org)、Netflix の[Eureka](https://github.com/Netflix/eureka)などのオープンソースパッケージと統合されています。

商用 API ゲートウェイの豊富な機能セットを必要としない単純なクラウドネイティブアプリケーションでは、Ocelot を検討してください。

## <a name="azure-application-gateway"></a>Azure アプリケーションゲートウェイ

単純なゲートウェイの要件については、 [Azure アプリケーションゲートウェイ](https://docs.microsoft.com/azure/application-gateway/overview)を検討してください。 Azure [PaaS サービス](https://azure.microsoft.com/overview/what-is-paas/)として利用できます。これには、URL ルーティング、SSL 終了、Web アプリケーションファイアウォールなどの基本的なゲートウェイ機能が含まれています。 このサービスでは、[レイヤー7の負荷分散](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)機能がサポートされています。 レイヤー7では、低レベルの TCP ネットワークパケットだけでなく、HTTP メッセージの実際のコンテンツに基づいて要求をルーティングすることができます。

この書籍全体を通して、クラウドネイティブシステムを[Kubernetes](https://www.infoworld.com/article/3268073/what-is-kubernetes-your-next-application-platform.html)でホストすることを伝えるしています。 コンテナー orchestrator は、コンテナー化されたワークロードのデプロイ、スケーリング、および運用上の懸念事項を Kubernetes に自動化します。 Azure アプリケーションゲートウェイは、 [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/)クラスターの API ゲートウェイとして構成できます。

[Application Gateway 受信コントローラー](https://azure.github.io/application-gateway-kubernetes-ingress/)は、Azure アプリケーションゲートウェイが[Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/)と直接連携できるようにします。 図4.5 にアーキテクチャを示します。

![Application Gateway 受信コントローラー](./media/application-gateway-ingress-controller.png)

**図 4-5.** Application Gateway 受信コントローラー

Kubernetes には、受信と呼ばれる HTTP (レベル 7) の負荷分散をサポートする組み込み機能が含まれ[ています](https://kubernetes.io/docs/concepts/services-networking/ingress/)。 受信では、AKS 内のマイクロサービスインスタンスを外部に公開する方法に関する一連のルールを定義します。 前のイメージでは、受信コントローラーがクラスター用に構成された受信規則を解釈し、Azure アプリケーションゲートウェイを自動的に構成します。 これらのルールに基づいて、Application Gateway は AKS 内で実行されているマイクロサービスにトラフィックをルーティングします。 受信コントローラーは、受信規則の変更をリッスンし、Azure アプリケーションゲートウェイに適切な変更を加えます。

## <a name="azure-api-management"></a>Azure API Management

大規模から大規模なクラウドネイティブシステムの場合は、 [Azure API Management](https://azure.microsoft.com/services/api-management/)を検討してください。 これは、API ゲートウェイのニーズを解決するだけでなく、完全な機能を備えた開発者と管理者エクスペリエンスを提供するクラウドベースのサービスです。 図4-6 に API Management を示します。

![Azure API Management](./media/azure-api-management.png)

**図 4-6.** Azure API Management

まず、API Management 構成可能なルールとポリシーに基づいて、バックエンドサービスへのアクセスを制御できるゲートウェイサーバーを公開します。 これらのサービスは、Azure クラウド、オンプレミスのデータセンター、または他のパブリッククラウドに配置できます。 API キーと JWT トークンによって、だれが何を実行できるかが決まります。 すべてのトラフィックは、分析のためにログに記録されます。

開発者向けに、API Management には、サービス、ドキュメント、およびそれらを呼び出すためのサンプルコードへのアクセスを提供する開発者ポータルが用意されています。 開発者は、Swagger/Open API を使用してサービスエンドポイントを検査し、その使用状況を分析できます。 このサービスは、.NET、Java、Golang など、主要な開発プラットフォーム全体で動作します。

パブリッシャーポータルは、管理者が Api を公開し、その動作を管理する管理ダッシュボードを公開します。 サービスアクセスを許可し、サービスの正常性を監視し、サービステレメトリを収集できます。 管理者は、各エンドポイントに*ポリシー*を適用して、動作に影響を与えます。 [ポリシー](https://docs.microsoft.com/azure/api-management/api-management-howto-policies)は、サービス呼び出しごとに順番に実行される、事前に構築されたステートメントです。  ポリシーは、着信呼び出し、発信呼び出し、またはエラー発生時に呼び出されるように構成されます。 ポリシーを組み合わせるときに決定的な順序付けを可能にするために、さまざまなサービススコープでポリシーを適用できます。 製品には、多数の事前に構築された[ポリシー](https://docs.microsoft.com/azure/api-management/api-management-policies)が付属しています。

次に、ポリシーがクラウドネイティブサービスの動作にどのように影響するかを示します。  

- サービスアクセスを制限します。
- 認証を強制します。  
- 必要に応じて、1つのソースからの呼び出しを調整します。
- キャッシュを有効にします。
- 特定の IP アドレスからの呼び出しをブロックします。
- サービスのフローを制御します。
- SOAP から REST または別のデータ形式 (XML から JSON など) に要求を変換します。

Azure API Management は、クラウドまたはデータセンター内でホストされているバックエンドサービスを公開できます。 クラウドネイティブシステムで公開される可能性があるレガシサービスについては、REST と SOAP Api の両方がサポートされます。 その他の Azure サービスも API Management 経由で公開できます。 マネージ API は、 [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)や[Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)などの Azure バッキングサービスの上に配置できます。 Azure API Management には、組み込みの負荷分散サポートは含まれていません。負荷分散サービスと組み合わせて使用する必要があります。

Azure API Management は、次の[4 つの異なるレベル](https://azure.microsoft.com/pricing/details/api-management/)で利用できます。

- 開発者
- Basic
- 標準
- 割り増し

Developer レベルは、非運用環境のワークロードと評価を目的としています。 他の層では、より多くの電力、機能、およびより高いサービスレベルアグリーメント (Sla) が提供されます。 Premium レベルでは、 [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)と[複数リージョンのサポート](https://docs.microsoft.com/azure/api-management/api-management-howto-deploy-multi-region)が提供されます。 すべてのレベルには、1時間あたりの固定料金があります。

Microsoft は最近、Azure API Management の[API Management サーバーレス層](https://azure.microsoft.com/blog/announcing-azure-api-management-for-serverless-architectures/)を発表しました。 *従量課金レベル*と呼ばれるサービスは、サーバーレスコンピューティングモデルを中心に設計された API Management の一種です。 前に示した "事前に割り当てられた" 価格レベルとは異なり、従量課金レベルでは迅速なプロビジョニングとアクションごとの料金が提供されます。

これにより、次のユースケースで API ゲートウェイの機能が有効になります。

- [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)や[Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)などのサーバーレステクノロジを使用して実装されたマイクロサービス。
- Service Bus キューやトピック、Azure storage など、azure をバッキングするサービスリソース。
- トラフィックが急増する可能性がありますが、ほとんどの時間が短くなるマイクロサービス。

従量課金レベルでは、基になる同じサービス API Management コンポーネントが使用されますが、動的に割り当てられたリソースに基づいてまったく異なるアーキテクチャが採用されます。 サーバーレスコンピューティングモデルと完全に整合しています。

- 管理するインフラストラクチャがありません。
- アイドル状態の容量はありません。
- 高可用性。
- 自動スケーリング。
- コストは実際の使用量に基づいています。
  
新しい従量課金レベルは、サーバーレスリソースを Api として公開するクラウドネイティブシステムに適しています。

> このドキュメントの執筆時点では、Azure クラウドでの従量課金レベルはプレビュー段階です。

## <a name="real-time-communication"></a>リアルタイム通信

リアルタイム (プッシュ) 通信は、HTTP 経由でバックエンドのクラウドネイティブシステムと通信するフロントエンドアプリケーションのためのもう1つのオプションです。 金融情報、オンライン教育、ゲーム、ジョブ進行状況の更新などのアプリケーションでは、バックエンドからリアルタイムにリアルタイムに応答する必要があります。 通常の HTTP 通信では、新しいデータが利用可能になったことをクライアントが知る方法はありません。 クライアントは、サーバーに対して継続的に要求を*ポーリング*または送信する必要があります。 *リアルタイム*通信では、サーバーはいつでも新しいデータをクライアントにプッシュできます。

リアルタイムシステムは、多くの場合、頻度の高いデータフローと多数の同時クライアント接続によって特徴付けられます。 リアルタイム接続を手動で実装することはすぐに複雑になり、接続されたクライアントへのスケーラビリティと信頼性の高いメッセージングを確保するために、非単純なインフラストラクチャが必要になります。 Azure Redis Cache のインスタンスと、クライアントアフィニティ用に固定セッションで構成されている一連のロードバランサーを管理していることがわかります。

[Azure SignalR service](https://azure.microsoft.com/services/signalr-service/)は完全に管理された azure サービスであり、クラウドネイティブアプリケーションのリアルタイム通信を簡略化します。 容量のプロビジョニング、スケーリング、固定接続などの技術的な実装の詳細は、抽象化されています。 これらは、99.9% のサービスレベルアグリーメントで処理されます。 インフラストラクチャの組み込みではなく、アプリケーションの機能に注目します。

有効にすると、クラウドベースの HTTP サービスは、ブラウザー、モバイル、およびデスクトップアプリケーションなど、接続されているクライアントにコンテンツの更新を直接プッシュできるようになります。 クライアントは、サーバーをポーリングすることなく更新されます。 Azure SignalR は、Websocket、サーバー側のイベント、長いポーリングなど、リアルタイム接続を作成するトランスポートテクノロジを抽象化しています。 開発者は、接続されたクライアントのすべてまたは特定のサブセットへのメッセージの送信に焦点を当てます。

図4-7 は、Azure SignalR が有効になっているクラウドネイティブアプリケーションに接続する HTTP クライアントのセットを示しています。

![Azure SignalR](./media/azure-signalr-service.png)

**図 4-7.** Azure SignalR

Azure SignalR Service のもう1つの利点は、サーバーレスクラウドネイティブサービスを実装することです。 Azure Functions のトリガーを使用して、必要に応じてコードが実行される可能性があります。 このシナリオは、コードがクライアントとの長い接続を維持していないため、厄介な場合があります。 この状況は、サービスが既に接続を管理しているため、Azure SignalR サービスで処理できます。

Azure SignalR サービスは、Azure SQL Database、Service Bus、Redis Cache などの他の Azure サービスと密接に統合されているため、クラウドネイティブアプリケーションで多くの可能性が生まれます。

>[!div class="step-by-step"]
>[前へ](communication-patterns.md)
>[次へ](service-to-service-communication.md)
