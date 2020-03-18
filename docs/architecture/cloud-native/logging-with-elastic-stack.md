---
title: エラスティック スタックを使用したログ記録
description: エラスティックスタック、Logstash、および Kibana を使用したログ記録
ms.date: 02/05/2020
ms.openlocfilehash: 6863c66b63854fe3ecaabe2919beded2926ea64c
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77448920"
---
# <a name="logging-with-elastic-stack"></a>エラスティック スタックを使用したログ記録

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

優れた一元化されたログツールが多数あり、無料のオープンソースツールからコストが高いオプションまで、コストが大きく異なります。 多くの場合、無料のツールは有料オファリングと同じか、それよりも優れています。 このようなツールの1つに、エラスティック検索、Logstash、Kibana という3つのオープンソースコンポーネントが組み合わされています。

これらのツールをまとめて、エラスティックスタックまたは ELK スタックと呼びます。

## <a name="elastic-stack"></a>エラスティックスタック

エラスティックスタックは、Kubernetes クラスターから情報を収集するための強力なオプションです。 Kubernetes では、Elasticsearch エンドポイントへのログの送信がサポートされています。[ほとんど](https://kubernetes.io/docs/tasks/debug-application-cluster/logging-elasticsearch-kibana/)の場合、図7-5 に示すように、環境変数を設定するだけで始める必要があります。

```kubernetes
KUBE_LOGGING_DESTINATION=elasticsearch
KUBE_ENABLE_NODE_LOGGING=true
```

**図 7-5**。 Kubernetes の構成変数

これにより、クラスターに Elasticsearch がインストールされ、すべてのクラスターログをそのクラスターに送信するターゲットになります。

Kibana ダッシュボードの例 ![、取り込まれたからのログに対するクエリの結果を Kubernetes](./media/kibana-dashboard.png)
**図 7-6**に示します。 Kubernetes から取り込まれたのログに対するクエリの結果を表示する Kibana ダッシュボードの例

## <a name="what-are-the-advantages-of-elastic-stack"></a>エラスティックスタックの利点は何ですか?

エラスティックスタックは、低コストでスケーラブルなクラウド対応の方法で集中ログを提供します。 ユーザーインターフェイスを使用すると、データ分析が効率化され、clunky インターフェイスとの連携ではなく、データからの有意の洞察を得ることができます。 さまざまな種類の入力をサポートしているため、分散アプリケーションがさまざまな種類のサービスにまたがるため、継続的にログとメトリックデータをシステムにフィードできることが予想されます。 エラスティックスタックでは、大規模なデータセット全体にわたる高速検索もサポートされています。これにより、大規模なアプリケーションで詳細データをログに記録し、パフォーマンスの高い方法で可視性を維持することもできます。

## <a name="logstash"></a>Logstash

最初のコンポーネントは[Logstash](https://www.elastic.co/products/logstash)です。 このツールは、さまざまなソースからログ情報を収集するために使用されます。 たとえば、Logstash はディスクからログを読み取り、 [Serilog](https://serilog.net/)などのログライブラリからもメッセージを受信できます。 Logstash では、ログの受信時に基本的なフィルター処理と拡張を行うことができます。 たとえば、ログに IP アドレスが含まれている場合は、地理的な参照を行うように Logstash を構成し、そのメッセージの国または郵便の発信元を取得するように構成することができます。

Serilog は、パラメーター化されたログ記録が可能な .NET 言語のログライブラリです。 フィールドを埋め込むテキストログメッセージを生成する代わりに、パラメーターが個別に保持されます。 これにより、よりインテリジェントなフィルター処理と検索が可能になります。 Logstash に書き込むためのサンプル Serilog 構成は、図7-7 に示されています。

```csharp
var log = new LoggerConfiguration()
         .WriteTo.Http("http://localhost:8080")
         .CreateLogger();
```

**図 7-7**。 ログ情報を HTTP 経由で logstash に直接書き込むための serilog config

Logstash は、図7-8 に示すような構成を使用します。

```
input {
    http {
        #default host 0.0.0.0:8080
        codec => json
    }
}

output {
    elasticsearch {
        hosts => "elasticsearch:9200"
        index=>"sales-%{+xxxx.ww}"
    }
}
```

**図 7-8**。 Serilog からログを使用するための Logstash 構成

広範なログ操作が必要ないシナリオでは、[拍](https://www.elastic.co/products/beats)と呼ばれる logstash の代替手段として使用されます。 拍は、ログからネットワークデータや稼働時間の情報まで、さまざまなデータを収集できるツールファミリです。 多くのアプリケーションでは、Logstash と拍の両方を使用します。

ログが Logstash によって収集されたら、そのログを配置する必要があります。 Logstash は多くの異なる出力をサポートしますが、より興味深いものの1つはエラスティック検索です。

## <a name="elastic-search"></a>ElasticSearch

エラスティック検索は、ログの受信時にインデックスを作成できる強力な検索エンジンです。 ログに対してクエリを実行することが簡単になります。 エラスティック検索は膨大な量のログを処理でき、極端なケースでは多数のノードにわたってスケールアウトできます。

パラメーターを格納するために作成されたログメッセージ、またはパラメーターが Logstash 処理によって分割されていることを示すログメッセージは、Elasticsearch がこの情報を保持するように直接照会できます。

`jill@example.com`によってアクセスされた上位10ページを検索するクエリは、図7-9 に示されています。

```
"query": {
    "match": {
      "user": "jill@example.com"
    }
  },
  "aggregations": {
    "top_10_pages": {
      "terms": {
        "field": "page",
        "size": 10
      }
    }
  }
```

**図 7-9**。 ユーザーがアクセスした上位10ページを検索するための Elasticsearch クエリ

## <a name="visualizing-information-with-kibana-web-dashboards"></a>Kibana web ダッシュボードを使用した情報の視覚化

スタックの最後のコンポーネントは Kibana です。 このツールは、web ダッシュボードで対話型の視覚化を提供するために使用されます。 ダッシュボードは、技術以外のユーザーによっても事前に作られている場合があります。 Elasticsearch インデックスに格納されているほとんどのデータは、Kibana ダッシュボードに含めることができます。 個々のユーザーには異なるダッシュボードが必要になる場合があり、Kibana ではユーザー固有のダッシュボードを許可することによってカスタマイズできます。

## <a name="installing-elastic-stack-on-azure"></a>Azure でのエラスティックスタックのインストール

エラスティックスタックは、さまざまな方法で Azure にインストールできます。 常に、[仮想マシンをプロビジョニングし、エラスティックスタックを直接インストール](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-elasticsearch)することができます。 経験豊富なユーザーは、このオプションを使用することをお勧めします。 サービスとしてのインフラストラクチャにを展開すると、管理オーバーヘッドが大きくなります。これにより、コンピューターのセキュリティ保護や修正プログラムによる最新情報の保持など、サービスとしてのインフラストラクチャに関連するすべてのタスクの所有権を取得する必要があります。

オーバーヘッドが少ないオプションは、エラスティックスタックが既に構成されている多くの Docker コンテナーの1つを使用することです。 これらのコンテナーは、既存の Kubernetes クラスターにドロップして、アプリケーションコードと共に実行できます。 [Sebp/elk](https://elk-docker.readthedocs.io/)コンテナーは、適切にドキュメント化され、テストされたエラスティックスタックコンテナーです。

もう1つのオプションは、最近発表された[サービスとしての ELK オファリング](https://devops.com/logz-io-unveils-azure-open-source-elk-monitoring-solution/)です。

## <a name="references"></a>References

- [エラスティックスタックを Azure にインストールする](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-elasticsearch)

>[!div class="step-by-step"]
>[前へ](observability-patterns.md)
>[次へ](monitoring-azure-kubernetes.md)
