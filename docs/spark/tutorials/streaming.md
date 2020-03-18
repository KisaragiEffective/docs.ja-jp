---
title: .NET for Apache Spark を使用した構造化ストリーミングのチュートリアル
description: このチュートリアルでは、Spark 構造化ストリーミング用の .NET for Apache Spark の使用方法について説明します。
author: mamccrea
ms.author: mamccrea
ms.date: 12/04/2019
ms.topic: tutorial
ms.openlocfilehash: 125ef834da8e42c99c8080a3d5414a7927ce7636
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79186509"
---
# <a name="tutorial-structured-streaming-with-net-for-apache-spark"></a>チュートリアル: .NET for Apache Spark を使用した構造化ストリーミング

このチュートリアルでは、.NET for Apache Spark を使用して Spark 構造化ストリーミングを呼び出す方法について説明します。 Spark 構造化ストリーミングは、リアルタイム データ ストリームを処理するための Apache Spark のサポートです。 ストリーム処理とは、ライブ データの生成中にそれを分析することを意味します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * .NET for Apache Spark アプリケーションを作成して実行する
> * netcat を使用してデータ ストリームを作成する
> * ユーザー定義関数と SparkSQL を使用してストリーミング データを分析する

## <a name="prerequisites"></a>必須コンポーネント

これが初めての .NET for Apache Spark アプリケーションである場合は、基本について理解を深めるために、[概要のチュートリアル](get-started.md)に関する記事から始めてください。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. コマンド プロンプトで、次のコマンドを実行して、新しいコンソール アプリケーションを作成します。

   ```dotnetcli
   dotnet new console -o mySparkStreamingApp
   cd mySparkStreamingApp
   ```

   `dotnet` コマンドで、種類が `console` の `new` アプリケーションを作成します。 `-o` パラメーターによって、*mySparkStreamingApp* という名前のディレクトリが作成され、そこにアプリに必要なファイルが追加されます。 `cd mySparkStreamingApp` コマンドで、ディレクトリを、先ほど作成したアプリ ディレクトリに変更します。

1. アプリで .NET for Apache Spark を使用するには、Microsoft.Spark パッケージをインストールします。 コンソールで、次のコマンドを実行します。

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="establish-and-connect-to-a-data-stream"></a>データ ストリームを確立して接続する

ストリーム処理をテストする一般的な方法の 1 つとして、**netcat** を使用する方法があります。 netcat (*nc* とも呼ばれます) を使用すると、ネットワーク接続に対する読み取りと書き込みを行うことができます。 netcat とのネットワーク接続を確立するには、ターミナル ウィンドウを使用します。

### <a name="create-a-data-stream-with-netcat"></a>netcat を使用してデータ ストリームを作成する

1. [netcat をダウンロードします](https://sourceforge.net/projects/nc110/files/)。 次に、zip ダウンロードからファイルを抽出し、抽出したディレクトリを "PATH" 環境変数に追加します。

2. 新しい接続を開始するには、新しいコンソールを開き、localhost にポート 9999 で接続する次のコマンドを実行します。

   Windows の場合:

   ```console
   nc -vvv -l -p 9999
   ```

   Linux の場合:

   ```console
   nc -lk 9999
   ```

   Spark プログラムでは、このコマンド プロンプトに入力する入力がリッスンされます。

### <a name="create-a-sparksession"></a>SparkSession を作成する

1. 次の追加の `using` ステートメントを、*mySparkStreamingApp* の *Program.cs* ファイルの先頭に追加します。

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using Microsoft.Spark.Sql.Streaming;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. 新しい `SparkSession` を作成するために、次のコードを `Main` メソッドに追加します。 Spark セッションは、Dataset API と DataFrame API を使用して Spark をプログラミングするためのエントリ ポイントです。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("Streaming example with a UDF")
        .GetOrCreate();
   ```

   上で作成した *spark* オブジェクトを呼び出すことで、プログラムの至るところで Spark と DataFrame の機能にアクセスできます。

### <a name="connect-to-a-stream-with-readstream"></a>ReadStream () を使用してストリームに接続する

`ReadStream()` メソッドによって、ストリーミング データを `DataFrame` として読み取るために使用できる `DataStreamReader` が返されます。 ストリーミング データを予期する場所を Spark アプリに通知する、ホストとポートの情報が含まれています。

```csharp
DataFrame lines = spark
    .ReadStream()
    .Format("socket")
    .Option("host", hostname)
    .Option("port", port)
    .Load();
```

## <a name="register-a-user-defined-function"></a>ユーザー定義関数を登録する

Spark アプリケーションで UDF ("*ユーザー定義関数*") を使用して、データの計算と分析を実行できます。

`udfArray` という名前の UDF を登録するために、次のコードを `Main` メソッドに追加します。

```csharp
Func<Column, Column> udfArray =
    Udf<string, string[]>((str) => new string[] { str, $"{str} {str.Length}" });
```

この UDF によって、netcat ターミナルから受け取った各文字列を処理して、元の文字列 (*str* に含まれています) と、その後ろに元の文字列の長さに連結された元の文字列が続く配列が生成されます。

たとえば、netcat ターミナルに *Hello world* と入力すると、次のような配列が生成されます。

* array\[0] = Hello world
* array\[1] = Hello world 11

## <a name="use-sparksql"></a>SparkSQL を使用する

SparkSQL を使用して、DataFrame に格納されているデータに対してさまざまな関数を実行します。 DataFrame の各行に対して 1 つの UDF を適用するために、複数の UDF と SparkSQL を組み合わせるのが一般的です。

```csharp
DataFrame arrayDF = lines.Select(Explode(udfArray(lines["value"])));
```

このコード スニペットでは、*udfArray* を DataFrame 内の各値に適用します。値は、netcat ターミナルから読み取られた各文字列を表します。 SparkSQL メソッド <xref:Microsoft.Spark.Sql.Functions.Explode%2A> を適用して、配列の各エントリを各行に配置します。 最後に、<xref:Microsoft.Spark.Sql.DataFrame.Select%2A> を使用して、生成された列を新しい DataFrame *arrayDF* に配置します。

## <a name="display-your-stream"></a>ストリームを表示する

<xref:Microsoft.Spark.Sql.DataFrame.WriteStream> を使用して、出力の特性を確立します (コンソールへの結果の出力や、最新の出力のみの表示など)。

```csharp
StreamingQuery query = arrayDf
    .WriteStream()
    .Format("console")
    .Start();
```

## <a name="run-your-code"></a>コードを実行する

Spark の構造化ストリーミングでは、一連の小さな**バッチ**を通してデータが処理されます。  プログラムを実行すると、netcat 接続を確立するコマンド プロンプトで入力を開始できます。 そのコマンド プロンプトでデータを入力して Enter キーを押すたびに、Spark によって入力がバッチとみなされ、UDF が実行されます。

### <a name="use-spark-submit-to-run-your-app"></a>spark-submit を使用してアプリを実行する

新しい netcat セッションを開始した後、新しいターミナルを開き、次のコマンドのように `spark-submit` コマンドを実行します。

```powershell
spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /path/to/microsoft-spark-<version>.jar Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkCharacterCount localhost 9999
```

> [!NOTE]
> 必ず上記のコマンドを実際の Microsoft Spark jar ファイルへのパスに更新してください。 上記のコマンドでは、netcat サーバーが localhost ポート 9999 で実行されていることも前提としています。

## <a name="get-the-code"></a>コードを取得する

このチュートリアルでは [StructuredNetworkCharacterCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkCharacterCount.cs) の例を使用していますが、GitHub には、他に次の 3 つの完全なストリームの処理例があります。

* [StructuredNetworkWordCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs): 任意のソースからストリーミングされたデータのワード カウント
* [StructuredNetworkWordCountWindowed.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCountWindowed.cs): ウィンドウ ロジックを使用したデータのワード カウント
* [StructuredKafkaWordCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs): Kafka からストリーミングされたデータのワード カウント

## <a name="next-steps"></a>次の手順

次の記事に進んで、.NET for Apache Spark アプリケーションを Databricks にデプロイする方法を確認してください。
> [!div class="nextstepaction"]
> [チュートリアル: .NET for Apache Spark アプリケーションを Databricks にデプロイする](databricks-deployment.md)
