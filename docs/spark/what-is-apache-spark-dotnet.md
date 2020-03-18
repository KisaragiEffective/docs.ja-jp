---
title: .NET for Apache Spark とは
description: .NET for Apache Spark について説明します。これは、.NET コードが記述されたすべての場所で Spark を使用する、無料でオープンソースのクロスプラットフォームなビッグ データ分析フレームワークです。
author: mamccrea
ms.topic: overview
ms.date: 10/15/2019
ms.openlocfilehash: 12fccd478cedaccf455043feb3afa7b12221bf0e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73458203"
---
# <a name="what-is-net-for-apache-spark"></a>.NET for Apache Spark とは

[Apache Spark](what-is-spark.md) は、大規模なデータ セット (一般的にテラバイトまたはペタバイト規模のデータ) の分析に使用される汎用の分散処理エンジンです。 .NET for Apache Spark は、人気のあるオープンソースのビッグ データ分析フレームワークのための、オープンソースでクロスプラットフォームの無料の .NET サポートです。既に知っている言語を使用して、Apache Spark の機能をビッグ データ アプリケーションに追加できるようになりました。

## <a name="why-choose-net-for-apache-spark"></a>.NET for Apache Spark を選択する理由

.NET for Apache Spark は、.NET の経験あるいはコード ベースを持つ開発者が、ビッグ データ分析の世界へ参入できるように支援します。 .NET for Apache Spark は、C# および F# から Spark を使用するためのハイ パフォーマンスの API を提供します。 C# および F# では、次にアクセスできます。

* 構造化データを操作するためのデータフレームおよび SparkSQL。
* ストリーミング データを操作するための Spark Structured Streaming。
* SQL 構文を使用してクエリを作成するための Spark SQL。
* トレーニングと予測を高速化するための機械学習の統合 (つまり、[ML.NET](https://dot.net/ml) と共に .NET for Apache Spark を使用します)。

.NET for Apache Spark は、.NET Standard (.NET 実装全体で共通した .NET API の標準仕様) に準拠しています。 つまり、.NET コードが記述されたすべての場所で Apache Spark を使用できることで、.NET 開発者として既に持っているすべての知識、スキル、コード、およびライブラリを再利用することができます。

.NET for Apache Spark は、.NET Core を使用して Windows、Linux、macOS で実行されます。 Windows では、.NET Framework を使用して実行することもできます。 Azure HDInsight Spark、Amazon EMR Spark、Azure Databricks、AWS 上の Databricks など、主要なすべてのクラウド プロバイダーにアプリケーションをデプロイできます。

## <a name="net-for-apache-spark-architecture"></a>.NET for Apache Spark のアーキテクチャ

Spark への C# および F# の言語バインドは、拡張を簡単にする新しい Spark 相互運用レイヤーに記述されています。 この新しい Spark 相互運用レイヤーは、言語拡張のベスト プラクティスを使用して記述され、相互運用とパフォーマンスのために最適化されています。 長期的には、この拡張機能を使用して、Spark に他の言語のサポートを追加できます。

> [!div class="mx-imgBorder"]
> ![.NET for Apache Spark のアーキテクチャ](media/dotnet-spark-architecture.png)

Spark 言語拡張機能の相互運用のサポートについては[提案](https://issues.apache.org/jira/browse/SPARK-26257)を参照してください。

## <a name="net-for-apache-spark-performance"></a>.NET for Apache Spark のパフォーマンス

[TPC-H ベンチマーク](http://www.tpc.org/tpch/)を使用して Python および Scala と比較した場合、.NET for Apache Spark はほとんどの場合で優れた成績を収め、ユーザー定義関数のパフォーマンスが重要な場合は Python よりも 2 倍高速に実行されます。 パフォーマンスを改善してベンチマークを行うための継続的な取り組みがなされています。

独自のベンチマークを実行するには、[.NET for Apache Spark GitHub](https://github.com/dotnet/spark/tree/master/benchmark) で利用可能なベンチマークを参照してください。

## <a name="net-for-apache-spark-roadmap"></a>.NET for Apache Spark のロードマップ

正規の [.NET for Apache Spark のロードマップ](https://github.com/dotnet/spark/blob/master/ROADMAP.md)から短期および長期の計画について説明します。

## <a name="net-foundation"></a>.NET Foundation

.NET for Apache Spark プロジェクトは、[.NET Foundation](https://www.dotnetfoundation.org/) の一部です。

## <a name="contributions"></a>投稿

.NET for Apache Spark チームでは、GitHub イシューとプル要求の両方での投稿を推奨しています。 まず、[既存のイシュー](https://github.com/dotnet/spark/issues)を探します。 既存のイシューが見つからない場合は、[新しいイシューを開始](https://github.com/dotnet/spark/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+)します。

## <a name="next-steps"></a>次の手順

.NET for Apache Spark を試す。
> [!div class="nextstepaction"]
> [チュートリアル: .NET for Apache Spark の概要](./tutorials/get-started.md)
