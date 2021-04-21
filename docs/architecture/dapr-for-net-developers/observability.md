---
title: Dapr の監視構成ブロック
description: 監視構成ブロック、その機能、利点、適用方法の説明
author: edwinvw
ms.date: 02/17/2021
ms.reviewer: robvet
ms.openlocfilehash: 745b9c07c31cc3ee11d5df945f2ccb87d0c9c2ed
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623981"
---
# <a name="the-dapr-observability-building-block"></a>Dapr の監視構成ブロック

最新の分散システムは複雑です。 最初は、小規模で疎結合の独立してデプロイ可能なサービスから始めます。 これらのサービスは、プロセスとサーバーの境界を越えるものです。 その後、さまざまな種類のインフラストラクチャ バッキング サービス (データベース、メッセージ ブローカー、キー コンテナー) を使用します。 最後に、これらのばらばらの部分をまとめてアプリケーションを形成します。

これほど多くの独立した可動部分がある場合、何が行われているのかをどのようにして把握すればよいでしょうか。 残念ながら、従来の監視アプローチは十分ではありません。 代わりに、システムを端から端まで **監視できる** 必要があります。 最新の[監視](../cloud-native/observability-patterns.md)プラクティスを使用すると、アプリケーションの正常性に関する可視性と分析情報が常に提供されます。 これにより、出力を観察することによって内部の状態を推測できます。 監視は、分散アプリケーションの監視とトラブルシューティングに必須です。

監視を行うために使用されるシステム情報は、**テレメトリ** と呼ばれます。 これは、大きく 4 つのカテゴリに分けることができます。

1. **分散トレース** により、分散トランザクションに関係するサービスとサービスの間のトラフィックに関する分析情報が提供されます。
1. **メトリック** により、サービスのパフォーマンスとそのリソース消費に関する分析情報が提供されます。
1. **ログ** により、コードの実行状況とエラーが発生したかどうかに関する分析情報が提供されます。
1. **正常性** エンドポイントにより、サービスの可用性に関する分析情報が提供されます。

テレメトリの深さは、アプリケーション プラットフォームの監視機能によって決まります。 Azure クラウドについて考えます。 そこでは、すべてのテレメトリ カテゴリを含む豊富なテレメトリ エクスペリエンスが提供されます。 何も構成しないと、ほとんどの Azure IaaS および PaaS サービスにより、[Azure Application Insights](/azure/azure-monitor/app/app-insights-overview) サービスにテレメトリが通知および発行されます。 Application Insights の高度に視覚化されたダッシュボードには、システム ログ、トレース、問題領域が表示されます。 また、通信に基づくサービス間の依存関係を示す図も表示できます。

しかし、アプリケーションで Azure PaaS や IaaS リソースを使用できない場合はどうすればよいでしょうか。 それでも、Application Insights の豊富なテレメトリ エクスペリエンスを利用できるでしょうか。 答えは "はい" です。 Azure 以外のアプリケーションでは、ライブラリをインポートし、構成を追加し、コードをインストルメント化して、テレメトリを Azure Application Insights に出力できます。 ただし、この方法を使用すると、アプリケーションが Application Insights に **緊密に結合されます**。 アプリを別の監視プラットフォームに移動すると、リファクタリングに多くの作業が必要になる可能性があります。 緊密な結合を回避し、コードの外部の監視を使用できれば、すばらしいことではないでしょうか。

Dapr ならそれが可能です。 Dapr で監視を分散アプリケーションに追加する方法を見てみましょう。

## <a name="what-it-solves"></a>解決される内容

Dapr の監視構成ブロックを使用すると、アプリケーションから監視が分離されます。 Dapr のコントロール プレーンを構成する Dapr サイドカーと Dapr システム サービスによって生成されたトラフィックが自動的にキャプチャされます。 そのブロックにより、複数のサービスにまたがる 1 つの操作からのトラフィックが関連付けられます。 また、システムのパフォーマンス メトリック、リソース使用率、正常性も公開されます。 テレメトリはオープン標準形式で発行されており、任意の監視バックエンドに情報を取り込むことができます。 そこで、情報の視覚化、クエリ、分析を行うことができます。

Dapr により仕組みが抽象化されるため、アプリケーションで監視の実装方法が認識されることはありません。 ライブラリを参照したり、カスタム インストルメンテーション コードを実装したりする必要はありません。 Dapr を使用すると、開発者はビジネス ロジックの構築に集中することができ、監視の仕組みを気にする必要はありません。 監視は Dapr レベルで構成されており、さまざまなチームによって作成され、さまざまなテクノロジ スタックを使用して構築された場合でも、サービス間で一貫しています。

## <a name="how-it-works"></a>しくみ

Dapr の[サイドカー アーキテクチャ](dapr-at-20000-feet.md#sidecar-architecture)により、組み込みの監視機能が有効になります。 サービスで通信が行われると、Dapr によってトラフィックがインターセプトされ、トレース、メトリック、ログの情報が抽出されます。 テレメトリはオープン標準形式で発行されます。 Dapr によって既定でサポートされているのは、[OpenTelemetry](https://opentelemetry.io/) と [Zipkin](https://zipkin.io/) です。

さまざまなバックエンド監視ツールにテレメトリを発行できる[コレクター](https://docs.dapr.io/operations/monitoring/tracing/open-telemetry-collector/) が、Dapr によって提供されています。 これらのツールにより、分析とクエリのために Dapr テレメトリが提示されます。 図 9-1 は、Dapr の監視アーキテクチャを示したものです。

![Dapr の監視アーキテクチャ](media/observability/observability-architecture.png)

**図 9-1** Dapr の監視アーキテクチャ。

1. サービス A により、サービス B の操作が呼び出されます。この呼び出しは、サービス A の Dapr サイドカーから、サービス B のサイドカーにルーティングされます。
1. サービス B で操作が完了すると、応答が Dapr サイドカー経由でサービス A に返されます。 それにより、すべての要求と応答で利用可能なすべてのテレメトリが収集されて発行されます。
1. 構成されているコレクターによってテレメトリが取り込まれ、監視バックエンドに送信されます。  

開発者は、監視を追加することは、pub/sub や状態管理などの他の Dapr 構成ブロックを構成することとは異なることに注意してください。 構成ブロックを参照するのではなく、コレクターと監視バックエンドを追加します。 図 9-1 は、さまざまな監視バックエンドと統合する複数のコレクターを構成できることを示しています。

この章の最初に、テレメトリの 4 つのカテゴリを示しました。 以下のセクションでは、各カテゴリの詳細について説明します。 一般的な監視バックエンドと統合するコレクターを構成する方法についての説明が含まれます。

### <a name="distributed-tracing"></a>分散トレース

分散トレースを使用すると、分散アプリケーション内のサービス間を流れるトラフィックについての分析情報が提供されます。 交換される要求と応答のメッセージのログは、問題のトラブルシューティングを行うための非常に重要な情報源です。 難しいのは、同じ操作から発生した "*メッセージを相互に関連付けること*" です。

Dapr により、[W3C トレース コンテキスト](https://www.w3.org/TR/trace-context)を使用して、関連メッセージが関連付けられます。 一意の操作を形成する要求と応答に、同じコンテキスト情報が挿入されます。 図 9-2 は、相関関係の仕組みを示したものです。

![W3C トレース コンテキストの例](media/observability/w3c-trace-context.png)

**図 9-2** W3C トレース コンテキストの例。

1. サービス A により、サービス B の操作が呼び出されます。サービス A によって呼び出しが開始されると、Dapr により一意のトレース コンテキストが作成されて、要求に挿入されます。
1. サービス B により要求が受信され、サービス C の操作が呼び出されます。Dapr により、受信要求にトレース コンテキストが含まれていることが検出され、サービス C への送信要求にそれを挿入することによって伝達されます。  
1. サービス C により、要求が受信されて処理されます。 Dapr により、受信要求にトレース コンテキストが含まれることが検出され、それをサービス B への送信応答に挿入することによって伝達されます。
1. サービス B により応答が受信されて処理されます。 次に、新しい応答が作成され、サービス A への送信応答に挿入することによってトレース コンテキストが伝達されます。

ひとまとまりになった要求と応答のセットは、"*トレース*" と呼ばれます。 図 9-3 にトレースを示します。

![トレースとスパン](media/observability/traces-spans.png)

**図 9-3** トレースとスパン。

図で、トレースによって、多くのサービスにまたがって実行される一意のアプリケーション トランザクションが表されていることに注意してください。 トレースは、"*スパン*" のコレクションです。 各スパンは、トレース内で実行される 1 つの操作または作業単位を表します。 スパンは、一意のトランザクションを実装するサービス間で送信される要求と応答です。

次のセクションでは、監視バックエンドに発行することによってトレーステ レメトリを検査する方法について説明します。

#### <a name="use-a-zipkin-monitoring-back-end"></a>Zipkin 監視バックエンドを使用する

[Zipkin](https://zipkin.io/) は、オープンソースの分散トレース システムです。 テレメトリ データを取り込んで視覚化することができます。 Dapr には、Zipkin の既定のサポートが用意されています。 次の例では、Dapr テレメトリを視覚化するように Zipkin を構成する方法を示します。

##### <a name="enable-and-configure-tracing"></a>トレースを有効にして構成する

最初に、Dapr 構成ファイルを使用して、Dapr ランタイム用にトレースを有効にする必要があります。 `tracing-config.yaml` という名前の構成ファイルの例を次に示します。  

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: tracing-config
  namespace: default
spec:
  tracing:
    samplingRate: "1"
    zipkin:
      endpointAddress: "http://zipkin.default.svc.cluster.local:9411/api/v2/spans"
```

`samplingRate` 属性により、トレースの発行に使用する間隔が指定されます。 値は、`0` (トレース無効) と `1` (すべてのトレースを発行) の間である必要があります。 たとえば、`0.5` という値を指定すると、トレースが 1 つおきに発行されるので、発行されるトラフィックが大幅に減少します。 `endpointAddress` は、Kubernetes クラスターで実行されている Zipkin サーバー上のエンドポイントを指します。 Zipkin の既定のポートは `9411` です。 Kubernetes CLI を使用して、Kubernetes クラスターに構成を適用する必要があります。

```console
kubectl apply -f tracing-config.yaml
```

##### <a name="install-the-zipkin-server"></a>Zipkin サーバーをインストールする

Dapr をセルフホステッド モードでインストールすると、Zipkin サーバーが自動的にインストールされ、Windows 上の `$HOME/.dapr/config.yaml` または `%USERPROFILE%\.dapr\config.yaml` にある既定の構成ファイルでトレースが有効になります。

ただし、Dapr を Kubernetes クラスターにインストールする場合は、Zipkin は既定では追加されません。 `zipkin.yaml` という名前の次の Kubernetes マニフェスト ファイルにより、標準の Zipkin サーバーがクラスターにデプロイされます。

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: zipkin
  namespace: eshop
  labels:
    service: zipkin
spec:
  replicas: 1
  selector:
    matchLabels:
      service: zipkin
  template:
    metadata:
      labels:
        service: zipkin
    spec:
      containers:
        - name: zipkin
          image: openzipkin/zipkin-slim
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 9411
              protocol: TCP

---

kind: Service
apiVersion: v1
metadata:
  name: zipkin
  namespace: eshop
  labels:
    service: zipkin
spec:
  type: NodePort
  ports:
    - port: 9411
      targetPort: 9411
      nodePort: 32411
      protocol: TCP
      name: zipkin
  selector:
    service: zipkin

```

デプロイでは、標準の `openzipkin/zipkin-slim` コンテナー イメージが使用されます。 Zipkin サービスにより Zipkin の Web フロントエンドが公開され、それを使用してポート `32411` でテレメトリを表示できます。 Kubernetes CLI を使用して、Zipkin マニフェスト ファイルを Kubernetes クラスターに適用し、Zipkin サーバーをデプロイします。

```console
kubectl apply -f zipkin.yaml
```

##### <a name="configure-the-services-to-use-the-tracing-configuration"></a>トレース構成を使用するようにサービスを構成する

現在、テレメトリの発行を開始するためのすべてのものが正しく設定されています。 アプリケーションの一部としてデプロイされているすべての Dapr サイドカーに、開始されたらテレメトリを出力するように指示する必要があります。 これを行うには、`tracing-config` の構成を参照する注釈 `dapr.io/config` を、各サービスのデプロイに追加します。 注釈が含まれる eShop 注文 API サービスのマニフェスト ファイルの例を次に示します。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ordering-api
  namespace: eshop
  labels:
    app: eshop
spec:
  replicas: 1
  selector:
    matchLabels:
      app: eshop
  template:
    metadata:
      labels:
        app: simulation
      annotations:
        dapr.io/enabled: "true"
        dapr.io/app-id: "ordering-api"
        dapr.io/config: "tracing-config"
    spec:
      containers:
      - name: simulation
        image: eshop/ordering.api:linux-latest
```

##### <a name="inspect-the-telemetry-in-zipkin"></a>Zipkin でテレメトリを検査する

アプリケーションが開始されると、Dapr サイドカーにより Zipkin サーバーにテレメトリが出力されます。 このテレメトリを検査するには、Web ブラウザーで <http://localhost:32411> を参照します。 Zipkin の Web フロントエンドが表示されます。

![Zipkin の開始ページ](media/observability/zipkin.png)

*[Find a trace]\(トレースの検索\)* タブで、トレースのクエリを実行できます。 制限を指定せずに *[RUN QUERY]\(クエリの実行\)* ボタンをクリックすると、取り込まれたすべての "*トレース*" が表示されます。

![トレースの一覧](media/observability/zipkin-traces-overview.png)

特定のトレースの横にある *[SHOW]\(表示\)* ボタンをクリックすると、そのトレースの詳細が表示されます。

![トレースの詳細](media/observability/zipkin-trace-details.png)

詳細ページの各項目は、選択したトレースの一部である要求を表すスパンです。

##### <a name="inspect-the-dependencies-between-services"></a>サービス間の依存関係を調べる

Dapr サイドカーによってサービス間のトラフィックが処理されるので、Zipkin でトレース情報を使用してサービス間の依存関係を判断できます。 実際の動作を確認するには、Zipkin の Web ページの *[Dependencies]\(依存関係\)* タブに移動し、虫眼鏡のボタンを選択します。 Zipkin に、サービスとその依存関係の概要が表示されます。

![Zipkin の依存関係グラフ](media/observability/zipkin-dependencies.png)

サービス間の線のアニメーション化された点は、要求を表し、送信元から送信先に移動します。 赤い点は、要求が失敗したことを示します。

#### <a name="use-a-jaeger-or-new-relic-monitoring-back-end"></a>Jaeger または New Relic の監視バックエンドを使用する

Zipkin 自体以外の他の監視バックエンド ソフトウェアでも、Zipkin 形式を使用するテレメトリの取り込みがサポートされています。 [Jaeger](https://www.jaegertracing.io/) は、Uber Technologies によって作成されたオープンソースのトレース システムです。 これは、分散サービス間のトランザクションをトレースし、複雑なマイクロサービス環境のトラブルシューティングを行うために使用されます。 [New Relic](https://newrelic.com/) は、"*フルスタック*" の監視プラットフォームです。 分散アプリケーションから関連するデータをリンクして、システムの全体像を提供します。 これを試すには、Dapr 構成ファイルで Jaeger または New Relic サーバーをポイントする `endpointAddress` を指定します。 Jaeger サーバーにテレメトリを送信するように Dapr を構成する構成ファイルの例を次に示します。 Jaeger の URL は、Zipkin の URL と同じです。 唯一の違いは、サーバーが実行されているポートです。

 ```yaml
 apiVersion: dapr.io/v1alpha1
 kind: Configuration
 metadata:
   name: tracing-config
   namespace: default
 spec:
   tracing:
     samplingRate: "1"
     zipkin:
       endpointAddress: "http://localhost:9415/api/v2/spans"
 ```

New Relic を試すには、New Relic の API のエンドポイントを指定します。 New Relic の構成ファイルの例を次に示します。

 ```yaml
apiVersion: dapr.io/v1alpha1
 kind: Configuration
 metadata:
   name: tracing-config
   namespace: default
 spec:
   tracing:
     samplingRate: "1"
     zipkin:
       endpointAddress: "https://trace-api.newrelic.com/trace/v1?Api-Key=<NR-API-KEY>&Data-Format=zipkin&Data-Format-Version=2"
 ```

使用方法の詳細については、Jaeger と New Relic の Web サイトを参照してください。

### <a name="metrics"></a>メトリック

メトリックを使用すると、パフォーマンスとリソース消費に関する分析情報を得ることができます。 内部的に、システムとランタイムのメトリックの広範なコレクションが Dapr によって生成されています。 Dapr によって使用されるメトリック標準は [Prometheus](https://prometheus.io/) です。 Dapr サイドカーとシステム サービスにより、ポート `9090` でメトリック エンドポイントが公開されています。 "*Prometheus スクレーパー*" により、事前に定義された間隔でこのエンドポイントが呼び出されて、メトリックが収集されます。 スクレーパーにより、メトリック値が監視バックエンドに送信されます。 図9-4 は、スクレイピング プロセスを示したものです。

![Prometheus メトリックのスクレイピング](media/observability/prometheus-scraper.png)

**図 9-4** Prometheus メトリックのスクレイピング。

上の図では、各サイドカーとシステム サービスにより、ポート 9090 でリッスンするメトリック エンドポイントが公開されています。 Prometheus Metrics Scrapper により、各エンドポイントからメトリックがキャプチャされて、その情報が監視バックエンドに発行されます。  

#### <a name="service-discovery"></a>サービス検出

メトリックを収集する場所をメトリック スクレーパーがどのように認識するのか不思議に思うかもしれません。 Prometheus は、ターゲット デプロイ環境に組み込まれている検出メカニズムと統合することができます。 たとえば、Kubernetes で実行すれている Prometheus は、Kubernetes API と統合して、環境で実行されている使用可能なすべての Kubernetes リソースを検出できます。

#### <a name="metrics-list"></a>メトリックの一覧

Dapr により、Dapr システム サービスとそのランタイムに関する多数のメトリックが生成されます。 次に例をいくつか示します。

| メトリック                                         | ソース | 説明                                                  |
| ---------------------------------------------- | :----: | ------------------------------------------------------------ |
| dapr_operator_service_created_total            | システム | Dapr Operator サービスによって作成された Dapr サービスの総数。 |
| dapr_injector_sidecar_injection/requests_total | システム | Dapr Sidecar-Injector サービスによって受信されたサイドカー挿入要求の総数。 |
| dapr_placement_runtimes_total                  | システム | Dapr Placement サービスに報告されたホストの総数。 |
| dapr_sentry_cert_sign_request_received_total   | システム | Dapr Sentry サービスによって受信された証明書署名要求 (CRS) の数。 |
| dapr_runtime_component_loaded      | ランタイム | 正常に読み込まれた Dapr コンポーネントの数。           |
| dapr_grpc_io_server_completed_rpcs | ランタイム | メソッドと状態による gRPC 呼び出しの数。                    |
| dapr_http_server_request_count     | ランタイム | HTTP サーバーで開始された HTTP 要求の数。           |
| dapr_http/client/sent_bytes        | ランタイム | HTTP クライアントによって要求本文で送信された総バイト数 (ヘッダーは含まれません)。 |

使用可能なメトリックの詳細については、[Dapr メトリックのドキュメント](https://docs.dapr.io/operations/monitoring/metrics/)を参照してください。

#### <a name="configure-dapr-metrics"></a>Dapr のメトリックを構成する

実行時には、Dapr コマンドに `--enable-metrics=false` 引数を含めることで、メトリック収集エンドポイントを無効にできます。 または、`--metrics-port 9090` 引数を使用して、エンドポイントの既定のポートを変更することもできます。

また、Dapr 構成ファイルを使用して、実行時のメトリック収集を静的に有効または無効にすることもできます。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: dapr-config
  namespace: eshop
spec:
  tracing:
    samplingRate: "1"
  metric:
    enabled: false
```

#### <a name="visualize-dapr-metrics"></a>Dapr のメトリックを視覚化する

メトリックは Prometheus スクレーパーによって収集されて監視バックエンドに発行されますが、生データはどのようにして調べればよいでしょうか。 メトリック分析によく使用される視覚化ツールは [Grafana](https://grafana.com/grafana/) です。 Grafana を使用すると、利用可能なメトリックからダッシュボードを作成できます。 次に示すのは、Dapr システム サービスのメトリックが表示されているダッシュボードの例です。

![Dapr システム サービスのメトリックが表示されている Grafana ダッシュボード](media/observability/grafana-sample.png)

Dapr のドキュメントには、[Prometheus と Grafana のインストールに関するチュートリアル](https://docs.dapr.io/operations/monitoring/metrics/grafana/)が含まれています。

### <a name="logging"></a>ログの記録

ログを使用すると、実行時にサービスに起きていることについての分析情報が提供されます。 アプリケーションを実行すると、Dapr により Dapr サイドカーと Dapr システム サービスからのログ エントリが自動的に生成されます。 ただし、アプリケーションのコードにインストルメント化されたログ エントリは、自動的には含まれ **ません**。 アプリケーションのコードからログを生成するには、[OpenTelemetry SDK for .NET](https://opentelemetry.io/docs/net/) などの特定の SDK をインポートします。 アプリケーションのコードのログ記録については、この章の「*Dapr .NET SDK を使用する*」セクションを参照してください。  

#### <a name="log-entry-structure"></a>ログ エントリの構造

Dapr によって構造化されたログが生成されます。 各ログ エントリの形式は次のとおりです。

| フィールド    | Description                                          | 例                             |
| -------- | ---------------------------------------------------- | ----------------------------------- |
| time     | ISO8601 形式のタイムスタンプ                          | `2021-01-10T14:19:31.000Z`          |
| レベル    | エントリのレベル (`debug` \| `info` \| `warn` \| `error`) | `info`                              |
| 型     | ログの種類                                             | `log`                               |
| msg      | ログ メッセージ                                          | `metrics server started on :62408/` |
| scope    | ログ記録のスコープ                                        | `dapr.runtime`                      |
| instance | Dapr が実行されているホスト名                             | TSTSRV01                            |
| app_id   | Dapr アプリ ID                                          | ordering-api                        |
| ver      | Dapr ランタイム バージョン                                 | `1.0.0`-rc.2                        |

トラブルシューティングのシナリオでログ エントリを検索するときは、`time` および `level` フィールドが特に役立ちます。 time フィールドを使用して、特定の期間を特定できるようにログ エントリを並べ替えます。 トラブルシューティングのときは、"*デバッグ レベル*" のログ エントリによって、コードの動作に関する詳細情報が提供されます。

#### <a name="plain-text-versus-json-format"></a>プレーンテキストと JSON 形式

Dapr によって既定で生成される構造化されたログは、プレーンテキスト形式です。 すべてのログ エントリは、キーと値のペアが含まれる文字列として書式設定されます。 プレーンテキストでのログの例を次に示します。

```text
== DAPR == time="2021-01-12T16:11:39.4669323+01:00" level=info msg="starting Dapr Runtime -- version 1.0.0-rc.2 -- commit 196483d" app_id=ordering-api instance=TSTSRV03 scope=dapr.runtime type=log ver=1.0.0-rc.2
== DAPR == time="2021-01-12T16:11:39.467933+01:00" level=info msg="log level set to: info" app_id=ordering-api instance=TSTSRV03 scope=dapr.runtime type=log ver=1.0.0-rc.2
== DAPR == time="2021-01-12T16:11:39.467933+01:00" level=info msg="metrics server started on :62408/" app_id=ordering-api instance=TSTSRV03 scope=dapr.metrics type=log ver=1.0.0-rc.2
```

単純ではあっても、この形式を解析するのは困難です。 監視ツールでログ エントリを表示する場合、JSON 形式のログを有効にしたくなります。 JSON エントリを使用すると、監視ツールで個々のフィールドにインデックスを作成し、クエリを実行できます。 同じログ エントリを JSON 形式にしたものを次に示します。

```json
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "starting Dapr Runtime -- version 1.0.0-rc.2 -- commit 196483d", "scope": "dapr.runtime", "time": "2021-01-12T16:11:39.4669323+01:00", "type": "log", "ver": "1.0.0-rc.2"}
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "log level set to: info", "scope": "dapr.runtime", "type": "log", "time": "2021-01-12T16:11:39.467933+01:00", "ver": "1.0.0-rc.2"}
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "metrics server started on :62408/", "scope": "dapr.metrics", "type": "log", "time": "2021-01-12T16:11:39.467933+01:00", "ver": "1.0.0-rc.2"}
```

JSON 形式を有効にするには、各 Dapr サイドカーを構成する必要があります。 セルフホステッド モードでは、コマンド ラインで `--log-as-json` フラグを指定できます。

```console
dapr run --app-id ordering-api --log-level info --log-as-json dotnet run
```

Kubernetes では、アプリケーションの各デプロイに注釈 `dapr.io/log-as-json` を追加できます。

```yaml
annotations:
   dapr.io/enabled: "true"
   dapr.io/app-id: "ordering-api"
   dapr.io/app-port: "80"
   dapr.io/config: "dapr-config"
   dapr.io/log-as-json: "true"
```

Helm を使用して Kubernetes クラスターに Dapr をインストールするときに、すべての Dapr システム サービスで JSON 形式のログを有効にすることができます。

```console
helm repo add dapr https://dapr.github.io/helm-charts/
helm repo update
kubectl create namespace dapr-system
helm install dapr dapr/dapr --namespace dapr-system --set global.logAsJson=true
```

#### <a name="collect-logs"></a>ログの収集

Dapr によって生成されたログを、分析のために監視バックエンドにフィードすることができます。 ログ コレクターは、システムからログを収集して監視バックエンドに送信するコンポーネントです。 よく使用されるログ コレクターは [Fluentd](https://www.fluentd.org/) です。 Dapr のドキュメントの「[How-To: Set up Fluentd, Elastic search and Kibana in Kubernetes (方法: Kubernetes で Fluentd、Elastic Search、Kibana をセットアップする)](https://docs.dapr.io/operations/monitoring/logging/fluentd/)」を参照してください。 この記事には、ログ コレクターとして Fluentd を、また監視バックエンドとして [ELK Stack](https://www.elastic.co/elastic-stack) (Elastic Search と Kibana) を設定する手順が含まれます。

### <a name="health-status"></a>正常性状態

サービスの正常性状態により、その可用性に関する分析情報が提供されます。 各 Dapr サイドカーによって公開されている正常性 API をホスト環境で使用して、サイドカーの正常性を判断できます。 API には、1 つの操作があります。

```console
GET http://localhost:3500/v1.0/healthz
```

この操作からは、2 つの HTTP 状態コードが返されます。

- 204: サイドカーが正常であるとき
- 500: サイドカーが正常ではないとき

セルフホステッド モードで実行している場合、正常性 API は自動的には呼び出されません。 アプリケーションのコードまたは正常性監視ツールから、API を呼び出すことができます。

Kubernetes で実行されている場合は、Dapr サイドカー インジェクターにより、"*活動性プローブ*" と "*対応性プローブ*" を実行するために正常性 API を使用するよう、Kubernetes が自動的に構成されます。

Kubernetes により、活動性プローブを使用して、コンテナーが稼働しているかどうかが判断されます。 活動性プローブからエラー コードが返された場合、Kubernetes はコンテナーが停止しているものと見なし、自動的に再起動します。 この機能により、アプリケーションの全体的な可用性が向上します。

Kubernetes により、対応性プローブを使用して、コンテナーがトラフィックの受け入れを開始できる状態かどうかが判断されます。 すべてのコンテナーの準備が整っている場合、ポッドは準備完了と見なされます。 対応性により、負荷分散シナリオにおいて Kubernetes サービスがポッドにトラフィックを送信できるかどうかが決定されます。 準備ができていないポッドは、ロード バランサーから自動的に削除されます。

活動性プローブと対応性プローブには、いくつかの構成可能なパラメーターがあります。 どちらも、ポッドのマニフェスト ファイルのコンテナー仕様セクションで構成されます。 既定では、Dapr により各サイドカー コンテナーに対し次の構成が使用されます。

```yaml
livenessProbe:
      httpGet:
        path: v1.0/healthz
        port: 3500
      initialDelaySeconds: 5
      periodSeconds: 10
      timeoutSeconds : 5
      failureThreshold : 3
readinessProbe:
      httpGet:
        path: v1.0/healthz
        port: 3500
      initialDelaySeconds: 5
      periodSeconds: 10
      timeoutSeconds : 5
      failureThreshold: 3
```

 プローブには、次のパラメーターを使用できます。

- `path` では、Dapr 正常性 API エンドポイントを指定します。
- `port` では、Dapr 正常性 API ポートを指定します。
- `initialDelaySeconds` では、Kubernetes が最初にコンテナーのプローブを開始する前に待機する秒数を指定します。
- `periodSeconds` では、各プローブ間で Kubernetes が待機する秒数を指定します。
- `timeoutSeconds` では、タイムアウトする前に Kubernetes が API からの応答を待機する秒数を指定します。タイムアウトはエラーとして解釈されます。
- `failureThreshold` では、Kubernetes によってコンテナーが動作していないか準備されていないと見なされる前に、受け入れる失敗状態コードの数を指定します。

### <a name="dapr-dashboard"></a>Dapr ダッシュボード

Dapr には、Dapr アプリケーション、コンポーネント、構成に関する状態情報が提供されるダッシュボードがあります。 ポート 8080 を使用するローカル コンピューター上の Web アプリケーションとしてダッシュボードを開始するには、Dapr CLI を使用します。

```console
dapr dashboard
```

Kubernetes で実行されている Dapr アプリケーションの場合は、次のコマンドを使用します。

```console
dapr dashboard -k
```

ダッシュボードが開き、アプリケーション内で Dapr サイドカーを持っているすべてのサービスの概要が表示されます。 次に示すスクリーンショットは、Kubernetes で実行されている eShopOnDapr アプリケーションの Dapr ダッシュボードです。

![Dapr ダッシュボードの概要ページ](media/observability/dapr-dashboard-overview.png)

Dapr ダッシュボードは、Dapr アプリケーションのトラブルシューティングを行うときに非常に役立ちます。 Dapr サイドカーとシステム サービスに関する情報が提供されます。 ログ エントリなど、各サービスの構成にドリルダウンすることができます。

また、ダッシュボードには、アプリケーションの構成済みのコンポーネント (およびその構成) も表示されます。

![Dapr ダッシュボードのコンポーネント ページ](media/observability/dapr-dashboard-components.png)

ダッシュボードを通じて大量の情報が提供されます。 それを検出するには、Dapr アプリケーションを実行し、ダッシュボードを参照します。 付属の eShopOnDapr アプリケーションを使用して開始できます。

Dapr ダッシュボードのコマンドの詳細については、Dapr ドキュメントの [Dapr ダッシュボード CLI コマンド リファレンス](https://docs.dapr.io/reference/cli/dapr-dashboard/)を参照してください。

## <a name="use-the-dapr-net-sdk"></a>Dapr .NET SDK を使用する

Dapr .NET SDK には、特定の監視機能は含まれていません。 すべての監視機能は、Dapr レベルで提供されます。

.NET アプリケーションのコードからテレメトリを生成する場合は、[OpenTelemetry SDK for .NET](https://opentelemetry.io/docs/net/) を検討する必要があります。 Open Telemetry プロジェクトは、クロスプラットフォームで、オープンソースであり、ベンダーに依存しません。 テレメトリデータを生成、出力、収集、処理、エクスポートするためのエンドツーエンドの実装が提供されます。 自動と手動のインストルメンテーションがサポートされるインストルメンテーション ライブラリが、言語ごとに 1 つあります。 テレメトリは Open Telemetry 標準を使用して発行されます。 このプロジェクトは、業界により幅広くサポートされており、クラウド プロバイダー、ベンダー、エンドユーザーによって導入されています。

## <a name="reference-application-eshopondapr"></a>参照アプリケーション: eShopOnDapr

付属する eShopOnDapr 参照アプリケーションでの監視は、いくつかの部分で構成されています。 すべてのサイドカーのテレメトリがキャプチャされます。 また、以前の eShopOnContainers サンプルから継承された他の監視機能もあります。

### <a name="custom-health-dashboard"></a>カスタム正常性ダッシュボード

eShopOnDapr の **WebStatus** プロジェクトは、eShop サービスの正常性に関する分析情報を提供するカスタム正常性ダッシュボードです。 このダッシュボードでは Dapr の正常性 API は使用されていませんが、ASP.NET Core の組み込みの[正常性チェック メカニズム](/aspnet/core/host-and-deploy/health-checks)が使用されています。 このダッシュボードでは、サービスの正常性状態だけでなく、サービスの依存関係の正常性も提供されます。 たとえば、次のスクリーンショットに示すように、データベースを使用するサービスの場合、このデータベースの正常性状態も提供されます。

![eShopOnDapr のカスタム正常性ダッシュボード](media/observability/eshop-health-dashboard.png)

### <a name="seq-log-aggregator"></a>Seq ログ アグリゲーター

[Seq](https://datalust.co/seq) は、ログを集計するために eShopOnDapr で使用されている、人気のあるログ アグリゲーター サーバーです。 Seq によって、Dapr システム サービスやサイドカーからではなく、アプリケーション サービスからログが取り込まれます。 Seq によって、アプリケーション ログにインデックスが付けられ、ログの分析とクエリを実行するための Web フロントエンドが提供されます。 また、監視ダッシュボードを構築するための機能も用意されます。

eShopOnDapr アプリケーション サービスにより、[SeriLog](https://serilog.net/) ログ ライブラリを使用して、構造化されたログが生成されます。 Serilog からは、**シンク** と呼ばれるコンストラクトにログ イベントが発行されます。 シンクは、Serilog によってログ イベントが書き込まれるターゲット プラットフォームに過ぎません。 Seq 用のものなど、[多くの Serilog シンクを使用できます](https://github.com/serilog/serilog/wiki/Provided-Sinks)。 Seq は、eShopOnDapr で使用される Serilog シンクです。

### <a name="application-insights"></a>Application Insights

また、eShopOnDapr サービスからは、Microsoft Application Insights SDK for .NET Core を使用して Azure Application Insights にもテレメトリが直接送信されます。 詳細については、Microsoft docs で [ASP.NET Core アプリケーション用の Azure Application Insights](/azure/azure-monitor/app/asp-net-core) に関する記事を参照してください。

## <a name="summary"></a>まとめ

運用環境で分散システムを実行する場合は、優れた監視が不可欠です。

Dapr からは、分散トレース、ログ、メトリック、正常性状態など、さまざまな種類のテレメトリが提供されています。

Dapr によって生成されるのは、Dapr システム サービスとサイドカーのテレメトリのみです。 アプリケーションのコードからのテレメトリは自動的には含まれません。 ただし、OpenTelemetry SDK for .NET のような特定の SDK を使用して、アプリケーションのコードからテレメトリを生成することができます。

Dapr のテレメトリはオープン標準に基づく形式で生成されるため、使用可能な多数の監視ツールで取り込むことができます。 たとえば、Zipkin、Azure Application Insights、ELK Stack、New Relic、Grafana などがあります。 特定の監視バックエンドを使用して Dapr アプリケーションを監視する方法のチュートリアルについては、Dapr のドキュメントで [Dapr を使用したアプリケーションの監視](https://docs.dapr.io/operations/monitoring/)に関する記事を参照してください。

テレメトリを取り込んで監視バックエンドに発行するテレメトリ スクレーパーが必要になります。

Dapr は、構造化されたログを出力するように構成できます。 バックエンド監視ツールでインデックスを作成できるため、構造化ログが推奨されます。 インデックス付きのログを使用すると、ユーザーはログを検索するときにリッチなクエリを実行できます。

Dapr には、Dapr のサービスと構成に関する情報が表示されるダッシュボードが用意されています。

## <a name="references"></a>リファレンス

- [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview/)
- [Open Telemetry](https://opentelemetry.io/)
- [Zipkin](https://zipkin.io/)
- [W3C トレース コンテキスト](https://www.w3.org/TR/trace-context/)
- [Jaeger](https://www.jaegertracing.io/)
- [New Relic](https://newrelic.com/)
- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/grafana/)
- [Open Telemetry SDK for .NET](https://opentelemetry.io/docs/net/)
- [Fluentd](https://www.fluentd.org/)
- [ELK スタック](https://www.elastic.co/elastic-stack)
- [Seq](https://datalust.co/seq)
- [Serilog](https://serilog.net/)

> [!div class="step-by-step"]
> [前へ](bindings.md)
> [次へ](secrets.md)
