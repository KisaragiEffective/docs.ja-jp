---
title: Ubuntu で .NET for Apache Spark アプリケーションをビルドする
description: Ubuntu で .NET for Apache Spark アプリケーションをビルドする方法を学習する
ms.date: 01/29/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 6dd6f60bb89a51c47fe17182fc47de818cd00b80
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187569"
---
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-ubuntu"></a>Ubuntu で .NET for Apache Spark アプリケーションをビルドする方法を学習する

この記事では、Ubuntu で .NET for Apache Spark アプリケーションをビルドする方法について説明します。

## <a name="prerequisites"></a>前提条件

以下の必須コンポーネントがすべて揃っている場合は、「[ビルド](#build)」の手順に進んでください。

1. **[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1)** または **[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)** をダウンロードしてインストールする - SDK をインストールすると、`dotnet` ツールチェーンがパスに追加されます。  .NET Core 2.1、2.2、および 3.1 がサポートされています。

2. **[OpenJDK 8](https://openjdk.java.net/install/)** をインストールする。

   - 次のコマンドを使用できます。

   ```bash
   sudo apt install openjdk-8-jdk
   ```

   * コマンド ラインから `java` を実行できることを確認します。

      java -version の出力サンプル:

      ```bash
      openjdk version "1.8.0_191"
      OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
      OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
      ```

   * 既に複数の OpenJDK バージョンをインストールしていて、OpenJDK 8 を選択したい場合は、次のコマンドを使用します。

      ```bash
      sudo update-alternatives --config java
      ```

3. **[Apache Maven 3.6.0 以降](https://maven.apache.org/download.cgi)** をインストールする。

   * 次のコマンドを実行します。

      ```bash
      mkdir -p ~/bin/maven
      cd ~/bin/maven
      wget https://www-us.apache.org/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
      tar -xvzf apache-maven-3.6.0-bin.tar.gz
      ln -s apache-maven-3.6.0 current
      export M2_HOME=~/bin/maven/current
      export PATH=${M2_HOME}/bin:${PATH}
      source ~/.bashrc
      ```

       これらの環境変数は、ターミナルを閉じると失われることに注意してください。 変更を永続させたい場合は、`export` ファイルに `~/.bashrc` の行を追加します。

   * コマンド ラインから `mvn` を実行できることを確認します

       mvn -version の出力サンプル:

       ```
       Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-24T18:41:47Z)
       Maven home: ~/bin/apache-maven-3.6.0
       Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
       Default locale: en, platform encoding: UTF-8
       OS name: "linux", version: "4.4.0-17763-microsoft", arch: "amd64", family: "unix"
       ```

4. **[Apache Spark 2.3 以降](https://spark.apache.org/downloads.html)** をインストールする。
[Apache Spark 2.3 以降](https://spark.apache.org/downloads.html)をダウンロードし、ローカル フォルダーに抽出します (`~/bin/spark-2.3.2-bin-hadoop2.7` など)。 (サポートされている Spark のバージョンは 2.3.*、2.4.0、2.4.1、2.4.3、および 2.4.4 です)

   ```bash
   tar -xvzf /path/to/spark-2.3.2-bin-hadoop2.7.tgz -C ~/bin/spark-2.3.2-bin-hadoop2.7
   ```

   * 必要な[環境変数](https://www.java.com/en/download/help/path.xml) `SPARK_HOME` (`~/bin/spark-2.3.2-bin-hadoop2.7/` など) と `PATH` (`$SPARK_HOME/bin:$PATH` など) を追加します

      ```bash
      export SPARK_HOME=~/bin/spark-2.3.2-hadoop2.7
      export PATH="$SPARK_HOME/bin:$PATH"
      source ~/.bashrc
      ```

      これらの環境変数は、ターミナルを閉じると失われることに注意してください。 変更を永続させたい場合は、`export` ファイルに `~/.bashrc` の行を追加します。

   * コマンド ラインから `spark-shell` を実行できることを確認します。

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

次のセクションに進む前に、コマンド ラインから `dotnet`、`java`、`mvn`、`spark-shell` を実行できることを確認してください。 もっと良い方法があると思いますか。 [イシューを作成](https://github.com/dotnet/spark/issues)して、お気軽にご投稿ください。

## <a name="build"></a>Build

このガイドの残りの部分では、.NET for Apache Spark リポジトリをご自分のコンピューターにクローンしておく必要があります (`~/dotnet.spark/` など)。

```bash
git clone https://github.com/dotnet/spark.git ~/dotnet.spark
```

### <a name="build-net-for-spark-scala-extensions-layer"></a>.NET for Spark の Scala 拡張機能レイヤーをビルドする

.NET アプリケーションを送信したとき、.NET for Apache Spark には、要求の処理方法を Apache Spark に伝える、Scala で記述された必要なロジックが含まれています (たとえば、新しい Spark セッションを作成する要求や、.NET 側から JVM 側にデータを転送する要求など)。 このロジックは、[.NET for Apache Spark の Scala ソース コード](https://github.com/dotnet/spark/tree/master/src/scala)に含まれています。

次の手順は、.NET for Apache Spark の Scala 拡張機能レイヤーをビルドすることです。

```bash
cd src/scala
mvn clean package
```

サポートされている Spark バージョンに対して作成された JAR を確認する必要があります。

* `microsoft-spark-2.3.x/target/microsoft-spark-2.3.x-<version>.jar`
* `microsoft-spark-2.4.x/target/microsoft-spark-2.4.x-<version>.jar`

### <a name="build-net-sample-applications-using-net-core-cli"></a>.NET Core CLI を使用して .NET サンプル アプリケーションをビルドする

このセクションでは、.NET for Apache Spark の[サンプル アプリケーション](https://github.com/dotnet/spark/tree/master/examples)をビルドする方法について説明します。 これらの手順は、あらゆる .NET for Spark アプリケーションのビルド プロセス全体を理解するのに役立ちます。

1. ワーカーをビルドします。

   ```dotnetcli
   cd ~/dotnet.spark/src/csharp/Microsoft.Spark.Worker/
   dotnet publish -f netcoreapp2.1 -r ubuntu.18.04-x64
   ```

   コンソール出力の例:

   ```bash
   user@machine:/home/user/dotnet.spark/src/csharp/Microsoft.Spark.Worker$ dotnet publish -f netcoreapp2.1 -r ubuntu.18.04-x64
   Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

      Restore completed in 36.03 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark.Worker/Microsoft.Spark.Worker.csproj.
      Restore completed in 35.94 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark/Microsoft.Spark.csproj.
      Microsoft.Spark -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark/Debug/netstandard2.0/Microsoft.Spark.dll
      Microsoft.Spark.Worker -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp2.1/ubuntu.18.04-x64/Microsoft.Spark.Worker.dll
      Microsoft.Spark.Worker -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish/
   ```

2. サンプルをビルドします。

   ```dotnetcli
   cd ~/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples/
   dotnet publish -f netcoreapp2.1 -r ubuntu.18.04-x64
   ```

   コンソール出力の例:

   ```bash
   user@machine:/home/user/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples$ dotnet publish -f netcoreapp2.1 -r ubuntu.18.04-x64
   Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

      Restore completed in 37.11 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark/Microsoft.Spark.csproj.
      Restore completed in 281.63 ms for /home/user/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples/Microsoft.Spark.CSharp.Examples.csproj.
      Microsoft.Spark -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark/Debug/netstandard2.0/Microsoft.Spark.dll
      Microsoft.Spark.CSharp.Examples -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp2.1/ubuntu.18.04-x64/Microsoft.Spark.CSharp.Examples.dll
      Microsoft.Spark.CSharp.Examples -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish/
   ```  

## <a name="run-the-net-for-spark-sample-applications"></a>.NET for Spark のサンプル アプリケーションを実行する

サンプルをビルドしたら、`spark-submit` を使用して .NET Core アプリを送信できます。 [必須コンポーネント](#prerequisites)のセクションに従っていることと、Apache Spark がインストール済みであることを確認してください。

1. `DOTNET_WORKER_DIR` または `PATH` 環境変数を設定して、`Microsoft.Spark.Worker` バイナリが生成されたパスが含まれるようにします (`~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish` など)。

   ```bash
   export DOTNET_WORKER_DIR=~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish
   ```

2. ターミナルを開き、アプリのバイナリが生成されたディレクトリに移動します (`~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish` など)。

   ```bash
   cd ~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp2.1/ubuntu.18.04-x64/publish
   ```

3. 次の基本構造に従ってアプリを実行します。

   ```bash
   spark-submit \
     [--jars <any-jars-your-app-is-dependent-on>] \
     --class org.apache.spark.deploy.dotnet.DotnetRunner \
     --master local \
     <path-to-microsoft-spark-jar> \
     <path-to-your-app-binary> <argument(s)-to-your-app>
   ```

   実行できるいくつかの例を次に示します。

   * **[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs)**

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Batch.Basic $SPARK_HOME/examples/src/main/resources/people.json
      ```

   * **[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)**

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredNetworkWordCount localhost 9999
      ```

   * **[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (Maven アクセス可能)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

      ```bash
      spark-submit \
      --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
      ```

   * **[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (jar 提供)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

      ```bash
      spark-submit \
      --jars path/to/net.jpountz.lz4/lz4-1.3.0.jar,path/to/org.apache.kafka/kafka-clients-0.10.0.1.jar,path/to/org.apache.spark/spark-sql-kafka-0-10_2.11-2.3.2.jar,`path/to/org.slf4j/slf4j-api-1.7.6.jar,path/to/org.spark-project.spark/unused-1.0.0.jar,path/to/org.xerial.snappy/snappy-java-1.1.2.6.jar \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
       ```
