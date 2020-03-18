---
title: Apache Spark とは
description: Apache Spark とビッグ データ シナリオについて説明します。
ms.date: 10/15/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 653f355d09a045feabb3dee0f5737cb691cf2dc4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73458169"
---
# <a name="what-is-apache-spark"></a>Apache Spark とは

[Apache Spark](https://spark.apache.org/) は、ビッグ データを分析するアプリケーションのパフォーマンスを向上させるよう、メモリ内処理をサポートするオープンソースの並列処理フレームワークです。 ビッグ データ ソリューションは、従来のデータベースでは大きすぎるか、複雑すぎるデータを処理するように設計されています。 Spark では、ディスクベースの代替手法よりもかなり速く、メモリ内の大量のデータが処理されます。

## <a name="common-big-data-scenarios"></a>一般的なビッグ データ シナリオ

大量のデータを格納して処理するか、非構造化データを変換するか、あるいはストリーミング データを処理する必要がある場合、ビッグ データ アーキテクチャをお勧めします。 Spark は汎用の分散処理エンジンであり、いくつかのビッグデータ シナリオで利用できます。

### <a name="extract-transform-and-load-etl"></a>抽出、変換、読み込み (ETL)

[抽出、変換、読み込み (ETL)](/azure/architecture/data-guide/relational-data/etl) は、1 つまたは複数のソースからデータを収集し、データを変更し、データを新しいデータ ストアに移動するプロセスです。 データの変換にはいくつかの方法があります。

* Filtering
* 並べ替え
* 集計
* 参加
* 消去
* 重複除去
* Validating

### <a name="real-time-data-stream-processing"></a>リアルタイムのデータ ストリーム処理

ストリーミング (あるいはリアルタイム) データは移動中のデータです。 IoT デバイス、ウェブログ、クリックストリームからのテレメトリはすべて、ストリーミング データの例です。 リアルタイム データを処理し、地理空間分析、リモート監視、異常検出など、有益な情報を提供できます。 リレーショナル データと同じように、データを出力シンクに移動する前に、フィルター処理したり、集計したり、ストリーミング データの準備をしたりできます。 Apache Spark では、[Spark Streaming](/azure/architecture/data-guide/big-data/real-time-processing) による[リアルタイムのデータ ストリーム処理](https://spark.apache.org/streaming/)がサポートされています。

### <a name="batch-processing"></a>バッチ処理

[バッチ処理](/azure/architecture/data-guide/big-data/batch-processing)は、保存されているビッグ データの処理です。 ジョブを並列で長時間実行し、非常に大きなデータセットをフィルター処理、集計、準備できます。

### <a name="machine-learning-through-mllib"></a>MLlib による機械学習

機械学習は高度な分析問題に使用されます。 お使いのコンピューターで既存のデータを利用し、将来の行動、成果、傾向を予測できます。 Apache Spark の機械学習ライブラリである [MLlib](https://spark.apache.org/mllib/) には、機械学習のアルゴリズムとユーティリティがいくつか含まれています。

### <a name="graph-processing-through-graphx"></a>GraphX によるグラフ処理

グラフは、エッジによって接続されたノードの集まりです。 階層式のデータや関係が相互に連結されているデータがある場合、グラフ データベースの利用が便利です。 Apache Spark の [GraphX](https://spark.apache.org/graphx/) API を利用し、このデータを処理できます。

### <a name="sql-and-structured-data-processing-with-spark-sql"></a>Spark SQL による SQL と構造化データの処理

構造化 (書式設定された) データを扱っている場合、[Spark SQL](https://spark.apache.org/sql/) を利用し、Spark アプリケーションで SQL クエリを使用できます。

## <a name="apache-spark-architecture"></a>Apache Spark アーキテクチャ

Apache Spark では、マスター/ワーカー アーキテクチャが利用され、ドライバー、エグゼキュータ、クラスター マネージャーという 3 つの主要コンポーネントがあります。

![Apache Spark アーキテクチャ](media/spark-architecture.png)

### <a name="driver"></a>ドライバー

このドライバーは、C# コンソール アプリのようなプログラムと Spark セッションで構成されています。 Spark セッションはプログラムを受け取り、それを小さなタスクに分割します。小さくなったタスクはエグゼキュータで処理されます。

### <a name="executors"></a>エグゼキュータ

各エグゼキュータまたはワーカー ノードはドライバーからタスクを受け取り、そのタスクを実行します。 エグゼキュータは、クラスターと呼ばれるエンティティ上に存在します。

### <a name="cluster-manager"></a>クラスター マネージャー

クラスター マネージャーは、次の目的でドライバーとエグゼキュータの両方と通信します。

* リソースの割り当てを管理する
* プログラム分割を管理する
* プログラム実行を管理する

## <a name="language-support"></a>言語のサポート

Apache Spark では、次のプログラミング言語がサポートされています。

* Scala
* Python
* Java
* SQL
* R
* .NET

## <a name="spark-apis"></a>Spark API

Apache Spark では次の API がサポートされています。

* [Spark Scala API](https://spark.apache.org/docs/2.2.0/api/scala/index.html)
* [Spark Java API](https://spark.apache.org/docs/2.2.0/api/java/index.html)
* [Spark Python API](https://spark.apache.org/docs/2.2.0/api/python/index.html)
* [Spark R API](https://spark.apache.org/docs/2.2.0/api/R/index.html)
* [Spark SQL](https://spark.apache.org/docs/latest/api/sql/index.html)、組み込み関数

## <a name="next-steps"></a>次のステップ

自分の .NET アプリケーションで Apache Spark を使用する方法について学習します。 .NET の経験がある開発者は、ビジネス ロジックが与えられたとき、.NET for Apache Spark を利用し、C# と F# でビッグ データ クエリを記述できます。
> [!div class="nextstepaction"]
> [.NET for Apache Spark とは](what-is-apache-spark-dotnet.md)
