---
title: .NET for Apache Spark の概要
description: Windows、macOS、Ubuntu で .NET Core を使用して .NET for Apache Spark アプリを実行する方法について説明します。
ms.date: 01/31/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 7375c385245a05d7dc29d5df89d875bf6cb4141a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187537"
---
# <a name="tutorial-get-started-with-net-for-apache-spark"></a>チュートリアル: .NET for Apache Spark の概要

このチュートリアルでは、Windows、macOS、Ubuntu で .NET Core を使用して .NET for Apache Spark アプリを実行する方法について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * .NET for Apache Spark の環境を準備する
> * 最初の .NET for Apache Spark アプリケーションを作成する
> * .NET for Apache Spark アプリケーション用の単純な .NET をビルドして実行する

## <a name="prepare-your-environment"></a>環境を準備する

アプリの作成を開始する前に、いくつかの前提条件となる依存関係を設定する必要があります。 コマンドライン環境から `dotnet`、`java`、`mvn`、`spark-shell` を実行できる場合は、環境が既に準備されているため、次のセクションに進むことができます。 コマンドのいずれかまたはすべてを実行できない場合は、次の手順を行います。

### <a name="1-install-net"></a>1..NET のインストール

.NET アプリのビルドを開始するには、.NET SDK (ソフトウェア開発キット) をダウンロードしてインストールする必要があります。

[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) をダウンロードしてインストールします。 SDK をインストールすると、`dotnet` ツールチェーンが PATH に追加されます。

.NET Core SDK をインストールしたら、新しいコマンド プロンプトまたはターミナルを開き、`dotnet` を実行します。

コマンドが実行され、dotnet の使用方法に関する情報が出力された場合は、次の手順に進むことができます。 `'dotnet' is not recognized as an internal or external command` エラーが発生した場合は、コマンドを実行する前に**新しい**コマンド プロンプトまたはターミナルを開いたことを確認してください。

### <a name="2-install-java"></a>2.Java のインストール

Windows および macOS の場合は [Java 8.1](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)、Ubuntu の場合は [OpenJDK 8](https://openjdk.java.net/install/) をインストールします。

ご使用のオペレーティング システムに適したバージョンを選択します。 たとえば、(次のように) Windows x64 マシンの場合は **jdk-8u201-windows-x64.exe**、macOS の場合は **jdk-8u231-macosx-x64.dmg** を選択します。 次に、コマンド `java` を使用してインストールを確認します。

![Java のダウンロード](https://dotnet.microsoft.com/static/images/java-jdk-downloads-windows.png?v=6BbJHoNyDO-PyYVciImr5wzh2AW_YHNcyb3p093AwPA)

### <a name="3-install-compression-software"></a>3.圧縮ソフトウェアのインストール

Apache Spark は、圧縮された .tgz ファイルとしてダウンロードされます。 [7-Zip](https://www.7-zip.org/)、[WinZip](https://www.winzip.com/) などの抽出プログラムを使用してファイルを抽出します。

### <a name="4-install-apache-spark"></a>4.Apache Spark のインストール

[Apache Spark をダウンロードしてインストールします](https://spark.apache.org/downloads.html)。 バージョン 2.3.*、2.4.0、2.4.1、2.4.3、または 2.4.4 から選択する必要があります (.NET for Apache Spark は、他のバージョンの Apache Spark と互換性がありません)。

次の手順で使用するコマンドは、[Apache Spark 2.4.1 をダウンロードしてインストールしていること](https://archive.apache.org/dist/spark/spark-2.4.1/spark-2.4.1-bin-hadoop2.7.tgz)を前提としています。 別のバージョンを使用する場合は、**2.4.1** を適切なバージョン番号に置き換えます。 その後、 **.tar** ファイルと Apache Spark ファイルを抽出します。

入れ子になった **.tar** ファイルを抽出するには、次のようにします。

* ダウンロードした **spark-2.4.1-bin-hadoop2.7.tgz** ファイルを見つけます。
* ファイルを右クリックし、 **[7-Zip] -> [ここに展開]** の順に選択します。
* ダウンロードした **.tgz** ファイルの横に **spark-2.4.1-bin-hadoop2.7.tar** が作成されます。

Apache Spark ファイルを抽出するには、次のようにします。

* **spark-2.4.1-bin-hadoop2.7.tar** を右クリックし、 **[7-Zip] -> [展開]** の順に選択します。
* **[展開先]** フィールドに「**C:\bin**」と入力します。
* **[展開先]** フィールドの下のチェックボックスをオフにします。
* **[OK]** を選択します。
* Apache Spark ファイルが C:\bin\spark-2.4.1-bin-hadoop2.7\ に抽出されます。

![Spark のインストール](https://dotnet.microsoft.com/static/images/spark-extract-with-7-zip.png?v=YvjUv54LIxI9FbALPC3h8zSQdyMtK2-NKbFOliG-f8M)

次のコマンドを実行して、**Windows** 上で Apache Spark を検索するために使用する環境変数を設定します。

```console
setx HADOOP_HOME C:\bin\spark-2.4.1-bin-hadoop2.7\
setx SPARK_HOME C:\bin\spark-2.4.1-bin-hadoop2.7\
```

次のコマンドを実行して、**macOS** と **Ubuntu** 上で Apache Spark を検索するために使用する環境変数を設定します。

```bash
export SPARK_HOME=~/bin/spark-2.4.1-bin-hadoop2.7/
export PATH="$SPARK_HOME/bin:$PATH"
source ~/.bashrc
```

すべてをインストールし、環境変数を設定したら、**新しい**コマンド プロンプトまたはターミナルを開き、次のコマンドを実行します。

`%SPARK_HOME%\bin\spark-submit --version`

コマンドが実行され、バージョン情報が出力された場合は、次の手順に進むことができます。

`'spark-submit' is not recognized as an internal or external command` エラーが発生した場合は、**新しい**コマンド プロンプトを開いたことを確認してください。

### <a name="5-install-net-for-apache-spark"></a>5..NET for Apache Spark のインストール

.NET for Apache Spark GitHub から、[Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases) リリースをダウンロードします。 たとえば、Windows マシンを使用していて、.NET Core の使用を計画している場合は、[Windows x64 netcoreapp3.1 リリースをダウンロード](https://github.com/dotnet/spark/releases/download/v0.8.0/Microsoft.Spark.Worker.netcoreapp3.1.win-x64-0.8.0.zip)します。

Microsoft.Spark.Worker を抽出するには、次のようにします。

* ダウンロードした **Microsoft.Spark.Worker.netcoreapp3.1.win-x64-0.8.0.zip** ファイルを見つけます。
* 右クリックし、 **[7-Zip] -> [ここに展開]** の順に選択します。
* **[展開先]** フィールドに「**C:\bin**」と入力します。
* **[展開先]** フィールドの下のチェックボックスをオフにします。
* **[OK]** を選択します。

![.NET Spark のインストール](https://dotnet.microsoft.com/static/images/dotnet-for-spark-extract-with-7-zip.png?v=jwCyum9mL0mGIi4V5zC7yuvLfcj1_nL-QFFD8TClhZk)

### <a name="6-install-winutils-windows-only"></a>6.WinUtils のインストール (Windows のみ)

.NET for Apache Spark では、Apache Spark と共に WinUtils をインストールする必要があります。 [winutils.exe をダウンロード](https://github.com/steveloughran/winutils/blob/master/hadoop-2.7.1/bin/winutils.exe)します。 次に、WinUtils を **C:\bin\spark-2.4.1-bin-hadoop2.7\bin** にコピーします。

> [!NOTE]
> Spark インストール フォルダー名の末尾に注釈が付けられている別のバージョンの Hadoop を使用している場合は、使用している Hadoop のバージョンと互換性のある[バージョンの WinUtils を選択](https://github.com/steveloughran/winutils)します。

### <a name="7-set-dotnet_worker_dir-and-check-dependencies"></a>7.DOTNET_WORKER_DIR の設定と依存関係の確認

次のコマンドのいずれかを実行して `DOTNET_WORKER_DIR` 環境変数を設定します。この変数は、.NET アプリで .NET for Apache Spark を検索するために使用されます。

**Windows** 上では、[新しい環境変数](https://www.java.com/en/download/help/path.xml) `DOTNET_WORKER_DIR` を作成し、Microsoft.Spark.Worker をダウンロードして抽出したディレクトリ (`C:\bin\Microsoft.Spark.Worker\` など) に設定します。

**macOS** 上では、`export DOTNET_WORKER_DIR <your_path>` を使用して新しい環境変数を作成し、Microsoft.Spark.Worker をダウンロードして抽出したディレクトリ ( *~/bin/Microsoft.Spark.Worker/* など) に設定します。

**Ubuntu** 上では、[新しい環境変数](https://help.ubuntu.com/community/EnvironmentVariables) `DOTNET_WORKER_DIR` を作成し、Microsoft.Spark.Worker をダウンロードして抽出したディレクトリ ( *~/bin/Microsoft.Spark.Worker* など) に設定します。

最後に、次のセクションに進む前に、コマンド ラインから `dotnet`、`java`、`mvn`、`spark-shell` を実行できることを再度確認します。

## <a name="write-a-net-for-apache-spark-app"></a>.NET for Apache Spark アプリを作成する

### <a name="1-create-a-console-app"></a>1.コンソール アプリを作成する

コマンド プロンプトまたはターミナルで、次のコマンドを実行して、新しいコンソール アプリケーションを作成します。

```dotnetcli
dotnet new console -o mySparkApp
cd mySparkApp
```

`dotnet` コマンドで、種類が `console` の `new` アプリケーションを作成します。 `-o` パラメーターで、アプリが格納されるディレクトリ *mySparkApp* を作成し、必要なファイルを指定します。 `cd mySparkApp` コマンドで、ディレクトリを、先ほど作成したアプリ ディレクトリに変更します。

### <a name="2-install-nuget-package"></a>2.NuGet パッケージのインストール

アプリで .NET for Apache Spark を使用するには、Microsoft.Spark パッケージをインストールします。 コマンド プロンプトまたはターミナルで、次のコマンドを実行します。

`dotnet add package Microsoft.Spark --version 0.8.0`

### <a name="3-code-your-app"></a>3.アプリのコーディング

Visual Studio Code または任意のテキスト エディターで *Program.cs* を開き、すべてのコードを次のコードで置き換えます。

```csharp
using Microsoft.Spark.Sql;

namespace MySparkApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create a Spark session.
            SparkSession spark = SparkSession
                .Builder()
                .AppName("word_count_sample")
                .GetOrCreate();

            // Create initial DataFrame.
            DataFrame dataFrame = spark.Read().Text("input.txt");

            // Count words.
            DataFrame words = dataFrame
                .Select(Functions.Split(Functions.Col("value"), " ").Alias("words"))
                .Select(Functions.Explode(Functions.Col("words"))
                .Alias("word"))
                .GroupBy("word")
                .Count()
                .OrderBy(Functions.Col("count").Desc());

            // Show results.
            words.Show();

            // Stop Spark session.
            spark.Stop();
        }
    }
}
```

### <a name="4-create-and-add-a-data-file"></a>4.データ ファイルの作成と追加

コマンド プロンプトまたはターミナルを開き、アプリ フォルダーに移動します。

```bash
cd <your-app-output-directory>
```

アプリでは、テキスト行を含むファイルが処理されます。 *mySparkApp* ディレクトリに、次のテキストを含む *input.txt* ファイルを作成します。

```text
Hello World
This .NET app uses .NET for Apache Spark
This .NET app counts words with Apache Spark
```

## <a name="run-your-net-for-apache-spark-app"></a>.NET for Apache Spark アプリを実行する

1. 次のコマンドを実行して、アプリケーションをビルドします。

   ```dotnetcli
   dotnet build
   ```

2. 次のコマンドを実行して、Apache Spark で実行するようにアプリケーションを送信します。

   ```console
   spark-submit \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   --master local \
   microsoft-spark-2.4.x-<version>.jar \
   dotnet HelloSpark.dll
   ```

   > [!NOTE]
   > このコマンドは、Apache Spark をダウンロードして PATH 環境変数に追加し、`spark-submit` を使用できる状態であることを前提としています。 そうでなければ、完全なパスを使用する必要があります (たとえば、*C:\bin\apache-spark\bin\spark-submit* や *~/spark/bin/spark-submit*)。

3. アプリを実行すると、*input.txt* ファイルのワード カウント データがコンソールに書き込まれます。

おめでとうございます! .NET for Apache Spark アプリの作成と実行が正常に完了しました。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> * .NET for Apache Spark 用に Windows 環境を準備する
> * 最初の .NET for Apache Spark アプリケーションを作成する
> * .NET for Apache Spark アプリケーション用の単純な .NET をビルドして実行する

上記の手順を説明したビデオを見るには、[.NET for Apache Spark 101 ビデオ シリーズ](https://channel9.msdn.com/Series/NET-for-Apache-Spark-101/Run-Your-First-NET-for-Apache-Spark-App)を参照してください。

詳細については、リソースのページを参照してください。
> [!div class="nextstepaction"]
> [.NET for Apache Spark のリソース](../resources/index.md)
