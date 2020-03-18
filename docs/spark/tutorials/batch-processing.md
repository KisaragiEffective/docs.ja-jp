---
title: .NET for Apache Spark を使用したバッチ処理のチュートリアル
description: .NET for Apache Spark を使用してバッチ処理を実行する方法について説明します。
author: mamccrea
ms.author: mamccrea
ms.date: 12/13/2019
ms.topic: tutorial
ms.openlocfilehash: 460c37e66c2c0a8a9b197a9abaff9eead842bdeb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187553"
---
# <a name="tutorial-do-batch-processing-with-net-for-apache-spark"></a>チュートリアル: .NET for Apache Spark を使用してバッチ処理を実行する

このチュートリアルでは、.NET for Apache Spark を使用してバッチ処理を実行する方法について説明します。 バッチ処理とは、保存データの変換のことです。これは、ソース データが既にデータ ストレージに読み込まれていることを意味します。

バッチ処理は、通常、さらに詳しい分析を行うために準備する必要がある大規模なフラット データセットに対して実行されます。 ログの処理とデータ ウェアハウスは、一般的なバッチ処理のシナリオです。 このシナリオでは、さまざまなプロジェクトがフォークされた回数や最近のプロジェクトがどのように更新されたかなど、GitHub プロジェクトに関する情報を分析します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * .NET for Apache Spark アプリケーションを作成して実行する
> * データを DataFrame に読み込み、分析を行うために準備する
> * Spark SQL を使用してデータを処理する

## <a name="prerequisites"></a>必須コンポーネント

.NET for Apache Spark を初めて使用する場合は、「[.NET for Apache Spark の概要](../tutorials/get-started.md)」チュートリアルをチェックし、環境を準備して最初の .NET for Apache Spark アプリケーションを実行する方法を確認してください。

## <a name="download-the-sample-data"></a>サンプル データをダウンロードする

[GHTorrent](http://ghtorrent.org/) は、プロジェクト、コミット、およびウォッチャーに関する情報などのすべてのパブリック GitHub イベントを監視し、イベントとその構造をデータベースに格納します。 さまざまな期間にわたって収集されたデータは、ダウンロード可能なアーカイブとして入手できます。 ダンプ ファイルは非常に大きいため、このガイドでは、GitHub からダウンロードできる、[ダンプ ファイルの切り詰められたバージョン](https://github.com/dotnet/spark/tree/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/projects_smaller.csv)を使用します。

> [!NOTE]
> GHTorrent データセットは、デュアル ライセンス スキーム ([Creative Commons +](https://wiki.creativecommons.org/wiki/CCPlus)) で配布されます。 商用以外の用途 (教育、研究、個人的使用などを含みますが、これらに限定されません) では、データセットは [CC-BY-SA ライセンス](https://creativecommons.org/licenses/by-sa/4.0/)に基づいて配布されます。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. コマンド プロンプトで、次のコマンドを実行して、新しいコンソール アプリケーションを作成します。

   ```dotnetcli
   dotnet new console -o mySparkBatchApp
   cd mySparkBatchApp
   ```

   `dotnet` コマンドで、種類が `console` の `new` アプリケーションを作成します。 `-o` パラメーターは、アプリが格納される *mySparkApp* というディレクトリを作成して、必要なファイルを取り込みます。 `cd mySparkBatchApp` コマンドで、ディレクトリを、先ほど作成したアプリ ディレクトリに変更します。

1. アプリで .NET for Apache Spark を使用するには、Microsoft.Spark パッケージをインストールします。 コンソールで、次のコマンドを実行します。

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="create-a-sparksession"></a>SparkSession を作成する

1. 次の追加の `using` ステートメントを *mySparkBatchApp* 内の *Program.cs* ファイルの先頭に追加します。

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. 次のコードをプロジェクトの名前空間に追加します。 *s_referenceData* は、後で日付に基づいてフィルター処理を行うためにプログラムで使用されます。

   ```csharp
   static readonly DateTime s_referenceDate = new DateTime(2015, 10, 20);
   ```

1. 次のコードを Main メソッド内に追加して、新しい SparkSession を確立します。 SparkSession は、Dataset と DataFrame API を使用して Spark をプログラミングするためのエントリ ポイントです。 `spark` オブジェクトを呼び出すことにより、プログラム全体で Spark および DataFrame の機能にアクセスできます。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("GitHub and Spark Batch")
        .GetOrCreate();
   ```

## <a name="prepare-the-data"></a>データを準備する

1. 入力ファイルを `DataFrame` に読み込みます。これは、名前付きの列に編成されたデータの分散コレクションです。 <xref:Microsoft.Spark.Sql.DataFrame.Schema%2A> を使用して、データの列を設定できます。 <xref:Microsoft.Spark.Sql.DataFrame.Show%2A> メソッドを使用して、DataFrame 内のデータを表示します。 ダウンロードした GitHub データの場所を指すように、CSV ファイルのパスを必ず更新してください。

   ```csharp
   DataFrame projectsDf = spark
       .Read()
       .Schema("id INT, url STRING, owner_id INT, " +
       "name STRING, descriptor STRING, language STRING, " +
       "created_at STRING, forked_from INT, deleted STRING" +
       "updated_at STRING")
       .Csv("filepath");

   projectsDf.Show();
   ```

1. <xref:Microsoft.Spark.Sql.DataFrame.Na%2A> メソッドを使用して、NA (null) 値を持つ行をドロップし、<xref:Microsoft.Spark.Sql.DataFrame.Drop%2A> メソッドを使用してデータから特定の列を削除します。 これにより、最終的な分析に関連しない null データまたは列を分析しようとした場合のエラーを回避することができます。

   ```csharp
   // Drop any rows with NA values
   DataFrameNaFunctions dropEmptyProjects = projectsDf.Na();
   DataFrame cleanedProjects = dropEmptyProjects.Drop("any");

   // Remove unnecessary columns
   cleanedProjects = cleanedProjects.Drop("id", "url", "owner_id");
   cleanedProjects.Show();
   ```

## <a name="analyze-the-data"></a>データを分析する

Spark SQL を使用すると、データに対して SQL 呼び出しを行うことができます。 ユーザー定義関数と Spark SQL を組み合わせて、ユーザー定義関数を DataFrame のすべての行に適用するのが一般的です。

明示的に `spark.Sql` を呼び出して、他の種類のアプリで見られる標準的な SQL 呼び出しを模倣することができます。 また、<xref:Microsoft.Spark.Sql.DataFrame.GroupBy%2A> や <xref:Microsoft.Spark.Sql.DataFrame.Agg%2A> などのメソッドを呼び出して、データの計算を明示的に結合、フィルター処理、および実行することもできます。

このアプリの目的は、GitHub プロジェクトのデータに関する分析情報を得ることです。 次のコード スニペットをプログラムに追加してデータを分析します。

1. 次のコード ブロックを追加すると、各言語がフォークされた回数が検出されます。 まず、データが言語別にグループ化されます。 次に、各言語からのフォークの平均回数が取得されます。

   ```csharp
   // Average number of times each language has been forked
   DataFrame groupedDF = cleanedProjects
       .GroupBy("language")
       .Agg(Avg(cleanedProjects["forked_from"]);
   ```

1. 次のコード ブロックを追加して、フォークの平均回数を降順に並べ替え、どの言語が最も多くフォークされているかを確認します。 つまり、フォークの回数が最も多いものが最初に表示されます。

   ```csharp
   // Sort by most forked languages first
   groupedDF.OrderBy(Desc("avg(forked_from)")).Show();
   ```

1. 次のコード ブロックは、最近のプロジェクトがどのように更新されたかを示します。 *MyUDF* という名前の新しいユーザー定義関数を登録し、それを、チュートリアルの冒頭で宣言した日付 (*s_referenceDate*) と比較します。 各プロジェクトの日付がこの参照日付と比較されます。 次に、Spark SQL を使用してデータの各行で UDF を呼び出し、データセット内の各プロジェクトを分析します。

   ```csharp
   spark.Udf().Register<string, bool>(
       "MyUDF",
       (date) => DateTime.TryParse(date, out DateTime convertedDate) &&
           (convertedDate > s_referenceDate);
   cleanedProjects.CreateOrReplaceTempView("dateView");

   DataFrame dateDf = spark.Sql(
       "SELECT *, MyUDF(dateView.updated_at) AS datebefore FROM dateView");
   dateDf.Show();
   ```

1. `spark.Stop()` を呼び出して SparkSession を終了します。

## <a name="use-spark-submit-to-run-your-app"></a>spark-submit を使用してアプリを実行する

1. 次のコマンドを使用して .NET アプリをビルドします。

   ```dotnetcli
   dotnet build
   ```

1. `spark-submit` を使用してアプリを実行します。 次のコマンドは、必ず Microsoft Spark jar ファイルの実際のパスを使って更新してください。

   ```console
   spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /<path>/to/microsoft-spark-<version>.jar dotnet /<path>/to/netcoreapp<version>/GitHubProjects.dll
   ```

## <a name="get-the-code"></a>コードを取得する

[完全なソリューション](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/GitHubProjects.cs)は、GitHub で確認できます。

## <a name="next-steps"></a>次の手順

次の記事に進み、.NET for Apache Spark でストリーミング データを処理する方法を学習してください。
> [!div class="nextstepaction"]
> [チュートリアル: .NET for Apache Spark を使用した構造化ストリーミング](streaming.md)
