---
title: Windows で .NET for Apache Spark アプリケーションをビルドする
description: Windows で .NET for Apache Spark アプリケーションをビルドする方法について学習します。
ms.date: 01/29/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: cb7154185fc9aa08bc447cb846798995301a6651
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79185755"
---
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-windows"></a>Windows で .NET for Apache Spark アプリケーションをビルドする方法を学習する

この記事では、Windows で .NET for Apache Spark アプリケーションをビルドする方法について説明します。

## <a name="prerequisites"></a>必須コンポーネント

以下の必須コンポーネントがすべて揃っている場合は、「[ビルド](#build)」の手順に進んでください。

  1. **[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1)** をダウンロードしてインストールする - SDK をインストールすると、`dotnet` ツールチェーンがパスに追加されます。 .NET Core 2.1、2.2、および 3.1 がサポートされています。
  2. **[Visual Studio 2019](https://www.visualstudio.com/downloads/)** (バージョン 16.3 以降) をインストールする。 コミュニティ バージョンは完全に無料です。 インストールを構成するときは、最小でも次のコンポーネントを含めてください。
     * .NET デスクトップ開発
       * すべての必須コンポーネント
         * .NET Framework 4.6.1 開発ツール
     * .NET Core クロスプラットフォームの開発
       * すべての必須コンポーネント
  3. **[Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)** をインストールする。
     - ご使用のオペレーティング システムに適したバージョンを選択します。 たとえば、Windows x64 コンピューターには *jdk-8u201-windows-x64.exe* を選択します。
     - インストーラーを使ってインストールし、コマンド ラインから `java` を実行できることを確認します。
  4. **[Apache Maven 3.6.0 以降](https://maven.apache.org/download.cgi)** をインストールする。
     - [Apache Maven 3.6.0](http://mirror.metrocast.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip) をダウンロードします。
     - ローカル ディレクトリに抽出します。 たとえば、*C:\bin\apache-maven-3.6.0\* です。
     - Apache Maven をご自分の [PATH 環境変数](https://www.java.com/en/download/help/path.xml)に追加します。 たとえば、*C:\bin\apache-maven-3.6.0\bin* です。
     - コマンド ラインから `mvn` を実行できることを確認します。
  5. **[Apache Spark 2.3 以降](https://spark.apache.org/downloads.html)** をインストールする。
     - [Apache Spark 2.3+](https://spark.apache.org/downloads.html) をダウンロードし、[7-zip](https://www.7-zip.org/) を利用してローカル フォルダー (たとえば、*C:\bin\spark-2.3.2-bin-hadoop2.7\*) に抽出します。(サポートされている Spark のバージョンは 2.3.* 、2.4.0、2.4.1、2.4.3、2.4.4 です)
     - [新しい環境変数](https://www.java.com/en/download/help/path.xml) `SPARK_HOME` を追加します。 たとえば、*C:\bin\spark-2.3.2-bin-hadoop2.7\* です。

       ```powershell
       set SPARK_HOME=C:\bin\spark-2.3.2-bin-hadoop2.7\
       ```

     - Apache Spark をご自分の [PATH 環境変数](https://www.java.com/en/download/help/path.xml)に追加します。 たとえば、*C:\bin\spark-2.3.2-bin-hadoop2.7\bin* です。

       ```powershell
       set PATH=%SPARK_HOME%\bin;%PATH%
       ```

     - コマンド ラインから `spark-shell` を実行できることを確認します。
        コンソール出力の例:

        ```
        Welcome to
              ____              __
             / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
           /___/ .__/\_,_/_/ /_/\_\   version 2.3.2
              /_/

        Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_201)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> sc
        res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@6eaa6b0c
        ```

        </details>

  6. **[WinUtils](https://github.com/steveloughran/winutils)** をインストールする。
     - [WinUtils リポジトリ](https://github.com/steveloughran/winutils)から `winutils.exe` バイナリをダウンロードします。 Spark ディストリビューションをコンパイルした Hadoop のバージョンを選択してください。 たとえば、Spark 2.3.2 には hadoop-2.7.1 を使用します。
     - `winutils.exe` バイナリを任意のディレクトリに保存します。 たとえば、*C:\hadoop\bin* です。
     - winutils.exe があるディレクトリを示すように `HADOOP_HOME` を設定します (bin は含めない)。 たとえば、コマンド ラインで次を実行します。

       ```powershell
       set HADOOP_HOME=C:\hadoop
       ```

     - `%HADOOP_HOME%\bin` が含まれるように PATH 環境変数を設定します。 たとえば、コマンド ラインで次を実行します。

       ```powershell
       set PATH=%HADOOP_HOME%\bin;%PATH%
       ```

次のセクションに進む前に、コマンド ラインから `dotnet`、`java`、`mvn`、`spark-shell` を実行できることを確認してください。 もっと良い方法があると思いますか。 [イシューを作成](https://github.com/dotnet/spark/issues)して、お気軽にご投稿ください。

> [!NOTE]
> 環境変数が更新された場合は、コマンド ラインの新しいインスタンスが必要になることがあります。

## <a name="build"></a>ビルド

このガイドの残りの部分では、.NET for Apache Spark リポジトリをご自分のコンピューターにクローンしておく必要があります。 任意の場所にリポジトリをクローンすることができます。 たとえば、*C:\github\dotnet-spark\* です。

```bash
git clone https://github.com/dotnet/spark.git C:\github\dotnet-spark
```

### <a name="build-net-for-apache-spark-scala-extensions-layer"></a>.NET for Apache Spark の Scala 拡張機能レイヤーをビルドする

.NET アプリケーションを送信したとき、.NET for Apache Spark には、要求の処理方法を Apache Spark に伝える、Scala で記述された必要なロジックが含まれています (たとえば、新しい Spark セッションを作成する要求や、.NET 側から JVM 側にデータを転送する要求など)。 このロジックは、[.NET for Spark の Scala ソース コード](https://github.com/dotnet/spark/tree/master/src/scala)に含まれています。

.NET Framework、.NET Core のどちらを使用しているかにかかわらず、.NET for Apache Spark の Scala 拡張機能レイヤーをビルドする必要があります。

```powershell
cd src\scala
mvn clean package
```

サポートされている Spark バージョンに対して作成された JAR を確認する必要があります。

* `microsoft-spark-2.3.x\target\microsoft-spark-2.3.x-<version>.jar`
* `microsoft-spark-2.4.x\target\microsoft-spark-2.4.x-<version>.jar`

### <a name="build-the-net-for-spark-sample-applications"></a>.NET for Spark のサンプル アプリケーションをビルドする

このセクションでは、.NET for Apache Spark の[サンプル アプリケーション](https://github.com/dotnet/spark/tree/master/examples)をビルドする方法について説明します。 これらの手順は、あらゆる .NET for Spark アプリケーションのビルド プロセス全体を理解するのに役立ちます。

#### <a name="using-visual-studio-for-net-framework"></a>.NET Framework 用に Visual Studio を使用する

  1. Visual Studio で `src\csharp\Microsoft.Spark.sln` を開き、`examples` フォルダーの下に `Microsoft.Spark.CSharp.Examples` プロジェクトをビルドします (その後、これによって .NET バインドのプロジェクトもビルドされます)。 必要に応じて、`Microsoft.Spark.Examples` プロジェクトに独自のコードを記述できます (この例の "input_file.json" は、データフレームの作成に使用するデータが含まれている json ファイルです)。
  
      ```csharp
        // Instantiate a session
        var spark = SparkSession
            .Builder()
            .AppName("Hello Spark!")
            .GetOrCreate();

        // Create initial DataFrame
        DataFrame df = spark.Read().Json(input_file.json);

        // Print schema
        df.PrintSchema();

        // Apply a filter and show results
        df.Filter(df["age"] > 21).Show();
      ```

     ビルドが成功すると、出力ディレクトリに作成された適切なバイナリが表示されます。
     コンソール出力の例:

      ```powershell
            Directory: C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461


        Mode                LastWriteTime         Length Name
        ----                -------------         ------ ----
        -a----         3/6/2019  12:18 AM         125440 Apache.Arrow.dll
        -a----        3/16/2019  12:00 AM          13824 Microsoft.Spark.CSharp.Examples.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.CSharp.Examples.exe.config
        -a----        3/16/2019  12:00 AM           2720 Microsoft.Spark.CSharp.Examples.pdb
        -a----        3/16/2019  12:00 AM         143360 Microsoft.Spark.dll
        -a----        3/16/2019  12:00 AM          63388 Microsoft.Spark.pdb
        -a----        3/16/2019  12:00 AM          34304 Microsoft.Spark.Worker.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.Worker.exe.config
        -a----        3/16/2019  12:00 AM          11900 Microsoft.Spark.Worker.pdb
        -a----        3/16/2019  12:00 AM          23552 Microsoft.Spark.Worker.xml
        -a----        3/16/2019  12:00 AM         332363 Microsoft.Spark.xml
        ------------------------------------------- More framework files -------------------------------------
      ```

#### <a name="using-net-core-cli-for-net-core"></a>.NET Core 用に .NET Core CLI を使用する

> [!NOTE]
> 現在、Spark .NET の .NET Core ビルドを自動化する作業に取り組んでいます。 それまでは、いくつかの手順を手動で実行していただくようお願いいたします。

  1. ワーカーをビルドします。

      ```powershell
      cd C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\
      dotnet publish -f netcoreapp2.1 -r win10-x64
      ```

      コンソール出力の例:

      ```powershell
      PS C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker> dotnet publish -f netcoreapp2.1 -r win10-x64
      Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 299.95 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 306.62 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\Microsoft.Spark.Worker.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.0\Microsoft.Spark.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\Microsoft.Spark.Worker.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish\
      ```

  2. サンプルをビルドします。

      ```powershell
      cd C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\
      dotnet publish -f netcoreapp2.1 -r win10-x64
      ```

      コンソール出力の例:

      ```powershell
      PS C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples> dotnet publish -f netcoreapp2.1 -r win10-x64
      Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 44.22 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 336.94 ms for C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\Microsoft.Spark.CSharp.Examples.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.0\Microsoft.Spark.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\Microsoft.Spark.CSharp.Examples.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish\
      ```

## <a name="run-the-net-for-spark-sample-applications"></a>.NET for Spark のサンプル アプリケーションを実行する

サンプルをビルドしたら、その実行には、.NET Framework、.NET Core のどちらを対象としているかにかかわらず `spark-submit` を使用します。 [必須コンポーネント](#prerequisites)のセクションに従っていることと、Apache Spark がインストール済みであることを確認してください。

  1. `DOTNET_WORKER_DIR` または `PATH` 環境変数を設定し、`Microsoft.Spark.Worker` バイナリが生成されているパスを含めます (たとえば、.NET Framework には *C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.Worker\Debug\net461* を、.NET Core には *C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish* を指定します)。

      ```powershell
      set DOTNET_WORKER_DIR=C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish
      ```
  
  2. PowerShell を起動し、アプリ バイナリが生成されているディレクトリに移動します (たとえば、.NET Framework の場合、*C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461* に、.NET Core の場合、*C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish* に移動します)。

      ```powershell
      cd C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish
      ```

  3. 次の基本構造に従ってアプリを実行します。

     ```powershell
     spark-submit.cmd `
       [--jars <any-jars-your-app-is-dependent-on>] `
       --class org.apache.spark.deploy.dotnet.DotnetRunner `
       --master local `
       <path-to-microsoft-spark-jar> `
       <path-to-your-app-exe> <argument(s)-to-your-app>
     ```

     実行できるいくつかの例を次に示します。

     - **[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs)**

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Batch.Basic %SPARK_HOME%\examples\src\main\resources\people.json
         ```

     - **[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)**

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkWordCount localhost 9999
         ```

     - **[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (Maven アクセス可能)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

         ```powershell
         spark-submit.cmd `
         --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
         ```

     - **[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (jar 提供)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

         ```powershell
         spark-submit.cmd
         --jars path\to\net.jpountz.lz4\lz4-1.3.0.jar,path\to\org.apache.kafka\kafka-clients-0.10.0.1.jar,path\to\org.apache.spark\spark-sql-kafka-0-10_2.11-2.3.2.jar,`path\to\org.slf4j\slf4j-api-1.7.6.jar,path\to\org.spark-project.spark\unused-1.0.0.jar,path\to\org.xerial.snappy\snappy-java-1.1.2.6.jar `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
          ```
