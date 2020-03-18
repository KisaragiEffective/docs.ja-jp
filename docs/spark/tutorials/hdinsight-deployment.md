---
title: .NET for Apache Spark アプリケーションを Azure HDInsight にデプロイする
description: .NET for Apache Spark アプリケーションを Azure HDInsight にデプロイする方法を説明します。
ms.date: 01/23/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 77b57463375c36444532bdd383ec4b3bfe3ab056
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77504169"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-azure-hdinsight"></a>チュートリアル: .NET for Apache Spark アプリケーションを Azure HDInsight にデプロイする

このチュートリアルでは、.NET for Apache Spark アプリケーションを Azure HDInsight クラスターを介してクラウドにデプロイする方法について説明します。 HDInsight の Spark クラスターは Azure Storage および Azure Data Lake Storage と互換性があるため、HDInsight では、Spark クラスターの作成と構成をより簡単に行うことができます。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * Azure Storage Explorer を使用してストレージ アカウントにアクセスする。
> * Azure HDInsight クラスターを作成する。
> * .NET for Apache Spark アプリを発行する。
> * HDInsight スクリプト アクションを作成する。
> * HDInsight クラスターで .NET for Apache Spark アプリケーションを実行する。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、以下の作業を行います。

* Azure サブスクリプションをお持ちでない場合は、[無料アカウント](https://azure.microsoft.com/free/)を作成してください。
* [Azure Portal](https://portal.azure.com/) にサインインします。
* Azure Storage Explorer を [Windows](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409)、[Linux](https://go.microsoft.com/fwlink/?LinkId=722418&clcid=0x409)、または [MacOS](https://go.microsoft.com/fwlink/?LinkId=708342&clcid=0x409) コンピューターにインストールします。
* チュートリアル「[.NET for Apache Spark - Get Started in 10-Minutes](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)」 (.NET for Apache Spark - 10 分で開始する) を完了します。

## <a name="access-your-storage-accounts"></a>ストレージ アカウントにアクセスする

1. Azure ストレージ エクスプローラーを開きます。

2. 左側のメニューで **[アカウントの追加]** を選択し、自分の Azure アカウントにサインインします。

    ![Storage Explorer から Azure アカウントにサインインする](./media/hdinsight-deployment/signin-azure-storage-explorer.png)

   サインインすると、お持ちのすべてのストレージ アカウントと、それらのストレージ アカウントにアップロードしたすべてのリソースが表示されます。

## <a name="create-an-hdinsight-cluster"></a>HDInsight クラスターの作成

> [!IMPORTANT]
> HDInsight クラスターの料金は、そのクラスターを使用していない場合でも、分単位で課金されます。 使用後は、クラスターを必ず削除してください。 詳細については、この記事の「[リソースのクリーンアップ](#clean-up-resources)」セクションを参照してください。

1. [Azure Portal](https://portal.azure.com) にアクセスします。

2. **[+ リソースの作成]** を選択します。 次に、 **[分析]** カテゴリから **[HDInsight]** を選択します。

    ![Azure portal から HDInsight リソースを作成する](./media/hdinsight-deployment/create-hdinsight-resource.png)

3. **[基本]** で次の値を指定します。

    |プロパティ  |説明  |
    |---------|---------|
    |サブスクリプション  | ドロップダウンから、アクティブな Azure サブスクリプションの 1 つを選択します。 |
    |リソース グループ | 新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。 リソース グループは、Azure ソリューションの関連するリソースを保持するコンテナーです。 |
    |クラスター名 | HDInsight Spark クラスターの名前を指定します。|
    |場所   | リソース グループの場所を選びます。 テンプレートでは、この場所をクラスターの作成および既定のクラスター ストレージに使用します。 |
    |クラスターの種類| クラスターの種類として **[Spark]** を選択します。|
    |クラスターのバージョン|クラスターの種類が選択されると、このフィールドに既定のバージョンが自動的に入力されます。 Spark バージョン 2.3 または 2.4 を選択します。|
    |クラスター ログイン ユーザー名| クラスターのログイン ユーザー名を入力します。  既定の名前は *admin*です。 |
    |クラスター ログイン パスワード| ログイン パスワードを入力します。 |
    |Secure Shell (SSH) ユーザー名| SSH ユーザー名を入力します。 既定では、このアカウントは "*クラスター ログイン ユーザー名*" アカウントと同じパスワードを共有します。 |

4. **ストレージ >>** を選択して **ストレージ** ページに進みます。 **[ストレージ]** で次の値を指定します。

    |プロパティ  |説明  |
    |---------|---------|
    |プライマリ ストレージの種類|既定値の **[Azure Storage]** を使用します。|
    |メソッドの選択|既定値の **[一覧から選択する]** を使用します。|
    |プライマリ ストレージ アカウント|サブスクリプションと、そのサブスクリプション内のアクティブなストレージ アカウントの 1 つを選択します。|
    |コンテナー|このコンテナーはストレージ アカウント内の特定の BLOB コンテナーで、クラスターはこのコンテナーを検索して、クラウドでアプリケーションを実行するためのファイルを見つけます。 任意の使用可能な名前を付けることができます。|

5. **[Review + create]\(確認と作成\)** を選択し、 **[作成]** を選択します。 クラスターの作成には約 20 分かかります。 次のセッションに進む前に、クラスターを作成する必要があります。

## <a name="publish-your-app"></a>アプリケーションの発行

次に、チュートリアル「[.NET for Apache Spark - Get Started in 10-Minutes](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)」 (.NET for Apache Spark - 10 分で開始する) で作成した *mySparkApp* を公開します。これにより、Spark クラスターは、アプリケーションを実行するために必要なすべてのファイルにアクセスできます。

1. *mySparkApp*を公開するには、次のコマンドを実行します。

   **Windows の場合:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

   **Linux の場合:**

   ```bash
   cd mySparkApp
   foo@bar:~/path/to/app$ dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

2. 公開するアプリケーション ファイルを HDInsight クラスターに簡単にアップロードできるように、次のタスクを実行してファイルを圧縮します。

   **Windows の場合:**

   *mySparkApp/bin/Release/netcoreapp3.0/ubuntu.16.04-x64* に移動します。 次に、**発行**フォルダーを右クリックして、 **[送信先]、[圧縮 (zip 形式) フォルダー]** の順に選択します。 新しいフォルダーに **publish.zip** という名前を付けます。

   **Linux の場合は、次のコマンドを実行します。**

   ```bash
   zip -r publish.zip
   ```

## <a name="upload-files-to-azure"></a>Azure にファイルをアップロードする

次に、Azure Storage Explorer を使用して、クラスターのストレージ用に選択した BLOB コンテナーに次の 5 つのファイルをアップロードします。

* Microsoft.Spark.Worker
* install-worker.sh
* publish.zip
* microsoft-spark-2.3.x-0.3.0.jar
* input.txt

1. Azure Storage Explorer を開いて、左側のメニューからストレージ アカウントに移動します。 ストレージ アカウント内の **[BLOB コンテナー]** でクラスターの BLOB コンテナーをドリルダウンします。

2. *Microsoft.Spark.Worker* は、Apache Spark が作成されたユーザー定義関数 (UDF) などのアプリケーションを実行するのに役立ちます。 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases/download/v0.3.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.3.0.tar.gz) をダウンロードします。 次に、Azure Storage Explorer で **[アップロード]** を選択して、worker をアップロードします。

   ![Azure Storage Explorer にファイルをアップロードする](./media/hdinsight-deployment/upload-files-to-storage.png)

3. *install-worker.sh* は、.NET for Apache Spark 依存ファイルをクラスターのノードにコピーできるスクリプトです。

   ご使用のローカル コンピューターで、**install-worker.sh** という名前の新しいファイルを作成し、GitHub 上にある [install-worker.sh のコンテンツ](https://raw.githubusercontent.com/dotnet/spark/master/deployment/install-worker.sh)を貼り付けます。 次に、*install-worker.sh* を BLOB コンテナーにアップロードします。

4. クラスターは、アプリケーションの発行済みファイルを含む publish.zip ファイルを必要とします。 発行済みフォルダー **mySparkApp/bin/Release/netcoreapp3.0/ubuntu.16.04-x64** に移動し、**publish.zip** を見つけます。 次に、BLOB コンテナーに *publish.zip* をアップロードします。

5. クラスターは、jar ファイルにパッケージ化されたアプリケーション コードを必要とします。 発行済みフォルダー **mySparkApp/bin/Release/netcoreapp3.0/ubuntu.16.04-x64** に移動し、**microsoft-spark-2.3.x-0.3.0.jar** を見つけます。 次に、BLOB コンテナーに jar ファイルをアップロードします。

   複数の .jar ファイル (Spark のバージョン 2.3.x 用と 2.4.x 用) が存在する場合があります。 クラスターの作成時に選択した Spark のバージョンと一致する .jar ファイルを選択する必要があります。 たとえば、クラスターの作成時に Spark 2.3.2 を選択した場合、*microsoft-spark-2.3.x-0.3.0.jar* を選択します。

6. クラスターは、アプリケーションへの入力を必要とします。 **mySparkApp**ディレクトリに移動して、**input.txt** を見つけます。 BLOB コンテナー内の **user/sshuser** ディレクトリに入力ファイルをアップロードします。 ssh を使用してクラスターに接続すると、クラスターはこのフォルダーで入力を検索します。 特定のディレクトリにアップロードされるのは、*input.txt* ファイルのみです。

## <a name="run-the-hdinsight-script-action"></a>HDInsight スクリプト アクションを実行する

クラスターの実行中、ファイルを Azure にアップロードしたら、クラスターで **install-worker.sh** スクリプトを実行します。

1. Azure portal の HDInsight Spark クラスターに移動し、 **[スクリプト アクション]** を選択します。

2. **[+ 新規で送信]** を選択し、次の値を指定します。

   |プロパティ  |説明  |
   |---------|---------|
   | スクリプトの種類 |カスタム|
   | 名前 | Install Worker|
   | Bash スクリプト URI |https://mystorageaccount.blob.core.windows.net/mycontainer/install-worker.sh </br> この URI を確認するには、Azure Storage Explorer で install-worker.sh を右クリックして、[プロパティ] を選択します。 |
   | ノードの種類| ワーカー|
   | パラメーター | azure </br> wasbs://mycontainer@myStorageAccount.blob.core.windows.net/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz </br> /usr/local/bin

3. **[作成]** を選択して、スクリプトを送信します。

## <a name="run-your-app"></a>アプリの実行

1. Azure portal の HDInsight Spark クラスターに移動し、 **[SSH およびクラスターのログイン]** を選択します。

2. ssh ログイン情報をコピーし、ログインをターミナルに貼り付けます。 クラスターの作成時に設定したパスワードを使用してクラスターにサインインします。 Ubuntu および Spark のウェルカム メッセージが表示されます。

3. **spark-submit** コマンドを使用して、HDInsight クラスターでアプリケーションを実行します。 このスクリプト例の **mycontainer** と **mystorageaccount** を、実際の BLOB コンテナーの名前とストレージ アカウントに置き換えることを忘れないでください。

   ```bash
   $SPARK_HOME/bin/spark-submit \
   --master yarn \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/microsoft-spark-2.3.x-0.6.0.jar \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/publish.zip mySparkApp
   ```

   アプリケーションが実行されると、ローカル実行が開始されてからコンソールに書き込まれた文字数と同じ文字数のテーブルが表示されます。 これで、クラウド内の最初の .NET for Apache Spark アプリケーションが実行されました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

HDInsight を使用すると、データは Azure Storage に格納されるため、使用されていないクラスターを安全に削除できます。 また、HDInsight クラスターは、使用していない場合でも課金されます。 クラスターの料金は Storage の料金の何倍にもなるため、クラスターを使用しない場合は削除するのが経済的にも合理的です。

リソース グループ名を選び、リソース グループ ページを開いて、 **[リソース グループの削除]** を選ぶこともできます。 リソース グループを削除すると、HDInsight Spark クラスターと既定のストレージ アカウントの両方が削除されます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、.NET for Apache Spark アプリケーションを Azure HDInsight にデプロイしました。 HDInsight の詳細については、Azure HDInsight のドキュメントに進んでください。

> [!div class="nextstepaction"]
> [Azure HDInsight のドキュメント](https://docs.microsoft.com/azure/hdinsight/)
