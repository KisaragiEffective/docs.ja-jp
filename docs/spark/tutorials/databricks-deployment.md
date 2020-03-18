---
title: .NET for Apache Spark アプリケーションを Databricks にデプロイする
description: .NET for Apache Spark アプリケーションを Databricks にデプロイする方法を説明します。
ms.date: 01/23/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: c5308530831fa288bf637849c1342f51769c3ad4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77503961"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-databricks"></a>チュートリアル: .NET for Apache Spark アプリケーションを Databricks にデプロイする

このチュートリアルでは、Azure Databricks を使用してクラウドにアプリをデプロイする方法について説明します。Azure Databricks は、ワンクリックでセットアップでき、ワークフローを合理化し、コラボレーションを可能にするインタラクティブなワークスペースを提供する、Apache Spark ベースの分析プラットフォームです。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - Azure Databricks ワークスペースを作成する。
> - .NET for Apache Spark アプリを発行する。
> - Spark ジョブと Spark クラスターを作成する。
> - Spark クラスターでアプリを実行する。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、以下の作業を行います。

* Azure アカウントがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を作成します。
* [Azure Portal](https://portal.azure.com/) にサインインします。
* 「[.NET for Apache Spark - 10 分で開始する](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)」のチュートリアルを完了します。

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks ワークスペースを作成する

> [!Note]
> **Azure 無料試用版サブスクリプション**を使用してこのチュートリアルを実行することはできません。
> 無料アカウントをお持ちの場合は、お使いのプロファイルにアクセスし、サブスクリプションを **[従量課金制]** に変更します。 詳細については、[Azure 無料アカウント](https://azure.microsoft.com/free/)に関するページをご覧ください。 次に、リージョン内の vCPU について[使用制限を削除し](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)、[クォータの増加を依頼](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request)します。 Azure Databricks ワークスペースを作成するときに、 **[Trial (Premium - 14-Days Free DBUs)]\(試用版 (Premium - 14 日間の無料 DBU)\)** の価格レベルを選択し、ワークスペースから 14 日間無料の Premium Azure Databricks DBU にアクセスできるようにします。

このセクションでは、Azure Portal を使って Azure Databricks ワークスペースを作成します。

1. Azure portal で、 **[リソースの作成]**  >  **[分析]**  >  **[Azure Databricks]** の順に選択します。

   ![Azure portal で Azure Databricks リソースを作成する](./media/databricks-deployment/create-databricks-resource.png)

2. **[Azure Databricks サービス]** で値を指定して、Databricks ワークスペースを作成します。

    |プロパティ  |説明  |
    |---------|---------|
    |**ワークスペース名**     | Databricks ワークスペースの名前を指定します。        |
    |**サブスクリプション**     | ドロップダウンから Azure サブスクリプションを選択します。        |
    |**リソース グループ**     | 新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。 リソース グループは、Azure ソリューションの関連するリソースを保持するコンテナーです。 詳しくは、[Azure リソース グループの概要](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)に関するページをご覧ください。 |
    |**場所**     | 優先リージョンを選択します。 使用可能なリージョンについては、[リージョン別の利用可能な Azure サービス](https://azure.microsoft.com/regions/services/)に関するページを参照してください。        |
    |**価格レベル**     |  **Standard**、**Premium**、**Trial** のいずれかを選択します。 これらのレベルの詳細については、[Databricks の価格に関するページ](https://azure.microsoft.com/pricing/details/databricks/)を参照してください。       |
    |**Virtual Network**     |   いいえ       |

3. **[作成]** を選択します。 ワークスペースの作成には数分かかります。 ワークスペースの作成中に、 **[通知]** でデプロイの状態を表示できます。

## <a name="install-azure-databricks-tools"></a>Azure Databricks ツールのインストール

**Databricks CLI** を使用して Azure Databricks クラスターに接続し、ローカル コンピューターからそれらのクラスターにファイルをアップロードすることができます。 Databricks クラスターからファイルへのアクセスは、DBFS (Databricks ファイル システム) を介して行われます。

1. Databricks CLI には、Python 3.6 以降が必要です。 Python を既にインストール済みである場合は、この手順をスキップできます。

   **Windows の場合:**

   [Windows 用 Python のダウンロード](https://www.python.org/ftp/python/3.7.4/python-3.7.4.exe)

   **Linux の場合:** Python は、ほとんどの Linux ディストリビューションにプレインストールされています。 次のコマンドを実行して、どのバージョンがインストールされているかを確認します。

   ```bash
   python3 --version
   ```

2. pip を使用して Databricks CLI をインストールします。 Python 3.4 以降では、既定で pip が含まれています。 Python 3 には pip3 を使用します。 次のコマンドを実行します。

   ```bash
   pip3 install databricks-cli
   ```

3. Databricks CLI をインストールしたら、新しいコマンド プロンプトを開き、`databricks` コマンドを実行します。 **'databricks' が内部または外部コマンドとして認識されないエラー**が発生した場合は、新しいコマンド プロンプトを開いたことを確認してください。

## <a name="set-up-azure-databricks"></a>Azure Databricks の設定

Databricks CLI がインストールされたので、認証の詳細を設定する必要があります。

1. Databricks CLI コマンド `databricks configure --token` を実行します。

2. 構成コマンドの実行後、ホストを入力するように求められます。 ホスト URL には、**https://<\Location>.azuredatabricks.net** の形式を使用します。 たとえば、Azure Databricks サービスの作成時に **eastus2** を選択した場合、ホストは **https://eastus2.azuredatabricks.net** となります。

3. ホストの入力後、トークンを入力するように求められます。 Azure portal で **[ワークスペースの起動]** を選択して、Azure Databricks ワークスペースを起動します。

   ![Azure Databricks ワークスペースを起動する](./media/databricks-deployment/launch-databricks-workspace.png)

4. ワークスペースのホーム ページで、 **[ユーザー設定]** を選択します。

   ![Azure Databricks ワークスペースのユーザー設定](./media/databricks-deployment/databricks-user-settings.png)

5. [ユーザー設定] ページで、新しいトークンを生成できます。 生成されたトークンをコピーして、コマンド プロンプトに貼り付けます。

   ![Azure Databricks ワークスペースで新しいアクセス トークンを生成する](./media/databricks-deployment/generate-token.png)

これで、作成する Azure Databricks クラスターにアクセスして、DBFS にファイルをアップロードできるようになります。

## <a name="download-worker-dependencies"></a>ワーカーの依存関係のダウンロード

1. Microsoft.Spark.Worker は、自分で作成したユーザー定義関数 (UDF) などのアプリを Apache Spark で実行するのに役立ちます。 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases/download/v0.6.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz) をダウンロードします。

2. *install-worker.sh* は、.NET for Apache Spark 依存ファイルをクラスターのノードにコピーできるスクリプトです。

   ご使用のローカル コンピューターで、**install-worker.sh** という名前の新しいファイルを作成し、GitHub 上にある [install-worker.sh のコンテンツ](https://raw.githubusercontent.com/dotnet/spark/master/deployment/install-worker.sh)を貼り付けます。

3. *db-init.sh* は、依存関係を Databricks Spark クラスターにインストールするスクリプトです。

   ご使用のローカル コンピューターで、**db-init.sh** という名前の新しいファイルを作成し、GitHub 上にある [db-init.sh のコンテンツ](https://github.com/dotnet/spark/blob/master/deployment/db-init.sh)を貼り付けます。

   先ほど作成したファイルで、`DOTNET_SPARK_RELEASE` 変数を `https://github.com/dotnet/spark/releases/download/v0.6.0/Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz` に設定します。 *db-init.sh* ファイルの残りの部分はそのままにしておきます。

> [!Note]
> Windows を使用している場合は、*install-worker.sh* および *db-init.sh* スクリプト内の行の終わりが Unix 形式 (LF) であることを確認します。 行の終わりは、Notepad++ や Atom などのテキスト エディターを使用して変更できます。

## <a name="publish-your-app"></a>アプリケーションの発行

次に、「[.NET for Apache Spark - 10 分で開始する](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)」のチュートリアルで作成した *mySparkApp* を発行し、自分の Spark クラスターから、アプリを実行するために必要なすべてのファイルにアクセスできることを確認します。

1. 次のコマンドを実行して、*mySparkApp* を発行します。

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

2. 発行したアプリケーション ファイルを Databricks Spark クラスターに簡単にアップロードできるように、次のタスクを実行してファイルを圧縮します。

   **Windows の場合:**

   mySparkApp/bin/Release/netcoreapp3.0/ubuntu.16.04-x64 に移動します。 次に、**発行**フォルダーを右クリックして、 **[送信先] > [圧縮 (zip 形式) フォルダー]** の順に選択します。 新しいフォルダーに **publish.zip** という名前を付けます。

   **Linux の場合は、次のコマンドを実行します。**

   ```bash
   zip -r publish.zip .
   ```

## <a name="upload-files"></a>ファイルのアップロード

このセクションでは、DBFS に複数のファイルをアップロードして、クラウドでアプリを実行するために必要なすべてのものがクラスターに含まれるようにします。 DBFS にファイルをアップロードするときは、そのつど、ご使用のコンピューター上でそのファイルがあるディレクトリにいることを確認してください。

1. 次のコマンドを実行して、*db-init.sh*、*install-worker.sh*、*Microsoft.Spark.Worker* を DBFS にアップロードします。

   ```console
   databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
   databricks fs cp install-worker.sh dbfs:/spark-dotnet/install-worker.sh
   databricks fs cp Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz dbfs:/spark-dotnet/   Microsoft.Spark.Worker.netcoreapp2.1.linux-x64-0.6.0.tar.gz
   ```

2. 次のコマンドを実行して、アプリを実行するためにクラスターに必要な残りのファイル (zip 形式の発行フォルダー、*input.txt*、*microsoft-spark-2.4.x-0.3.0.jar*) をアップロードします。

   ```console
   cd mySparkApp
   databricks fs cp input.txt dbfs:/input.txt

   cd mySparkApp\bin\Release\netcoreapp3.0\ubuntu.16.04-x64 directory
   databricks fs cp mySparkApp.zip dbfs:/spark-dotnet/publish.zip
   databricks fs cp microsoft-spark-2.4.x-0.6.0.jar dbfs:/spark-dotnet/microsoft-spark-2.4.x-0.6.0.jar
   ```

## <a name="create-a-job"></a>ジョブの作成

アプリは、**spark-submit** を実行するジョブを介して Azure Databricks で実行されます。spark-submit は、.NET for Apache Spark ジョブを実行するために使用するコマンドです。

1. Azure Databricks ワークスペースで、 **[ジョブ]** アイコンを選択し、次に **[+ ジョブの作成]** を選択します。

   ![Azure Databricks ジョブを作成する](./media/databricks-deployment/create-job.png)

2. ジョブのタイトルを選択し、 **[Configure spark-submit]\(spark-submit の構成\)** を選択します。

   ![Databricks ジョブの spark-submit を構成する](./media/databricks-deployment/configure-spark-submit.png)

3. ジョブ構成に次のパラメーターを貼り付けます。 次に、 **[確認]** を選択します。

   ```
   ["--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/spark-dotnet/microsoft-spark-2.4.x-0.6.0.jar","/dbfs/spark-dotnet/publish.zip","mySparkApp"]
   ```

## <a name="create-a-cluster"></a>クラスターの作成

1. ジョブに移動し、 **[編集]** を選択して、ジョブのクラスターを構成します。

2. クラスターを **Spark 2.4.1** に設定します。 次に、 **[詳細オプション]**  >  **[Init Scripts]\(Init スクリプト\)** を選択します。 Init スクリプトのパスを `dbfs:/spark-dotnet/db-init.sh` として設定します。

   ![Azure Databricks で Spark クラスターを構成する](./media/databricks-deployment/cluster-config.png)

3. **[確認]** を選択して、クラスターの設定を確認します。

## <a name="run-your-app"></a>アプリの実行

1. ジョブに移動し、 **[今すぐ実行]** を選択して、新しく構成した Spark クラスターでジョブを実行します。

2. ジョブのクラスターが作成されるまで数分かかります。 作成されると、ジョブが送信され、出力を表示できるようになります。

3. 左側のメニューから **[クラスター]** を選択し、ジョブの名前と実行を選択します。

4. **[Driver Logs]\(ドライバー ログ\)** を選択して、ジョブの出力を表示します。 アプリの実行が完了すると、標準出力コンソールに書き込まれた、「開始する」のローカル実行と同じ単語件数のテーブルが表示されます。

   ![Azure Databricks ジョブ出力のテーブル](./media/databricks-deployment/table-output.png)

   これで、初めての .NET for Apache Spark アプリケーションをクラウドで実行できました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

Databricks ワークスペースが不要になった場合は、Azure portal で Azure Databricks リソースを削除できます。 リソース グループ名を選び、リソース グループ ページを開いて、 **[リソース グループの削除]** を選ぶこともできます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、.NET for Apache Spark アプリケーションを Databricks にデプロイしました。 Databricks の詳細については、Azure Databricks のドキュメントに進んでください。

> [!div class="nextstepaction"]
> [Azure Databricks のドキュメント](https://docs.microsoft.com/azure/azure-databricks/)
