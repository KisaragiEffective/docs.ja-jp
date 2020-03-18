---
title: .NET for Apache Spark ジョブを Databricks に送信する
description: spark-submit と Set Jar を使用して、.NET for Apache Spark ジョブを Databricks に送信する方法について説明します。
ms.date: 12/05/2019
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 65976f9095ecef66e0538c398492033c612c1430
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187609"
---
# <a name="submit-a-net-for-apache-spark-job-to-databricks"></a>.NET for Apache Spark ジョブを Databricks に送信する

.NET for Apache Spark ジョブの Databricks へのデプロイには、`spark-submit` と Set Jar の 2 つの方法があります。

## <a name="deploy-using-spark-submit"></a>spark-submit を使用して展開する

[spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) コマンドを使用して、.NET for Apache Spark ジョブを Databricks に送信できます。 `spark-submit` を使用すると、オンデマンドで作成されたクラスターにのみ送信できます。

1. Databricks ワークスペースに移動して新しいジョブを作成します。 ジョブのタイトルを選択し、 **[Configure spark-submit]\(spark-submit の構成\)** を選択します。 次のパラメーターをジョブ構成に貼り付け、 **[確認]** を選択します。

    ```
    ["--files","/dbfs/<path-to>/<app assembly/file to deploy to worker>","--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/<path-to>/microsoft-spark-<spark_majorversion.spark_minorversion.x>-<spark_dotnet_version>.jar","/dbfs/<path-to>/<app name>.zip","<app bin name>","app arg1","app arg2"]
    ```

    > [!NOTE]
    > 使用する特定のファイルと構成に基づいて、上記のパラメーターの内容を更新します。 たとえば、DBFS にアップロードした Microsoft Spark jar ファイルのバージョンを参照し、アプリの適切な名前と発行済みのアプリの zip ファイルを使用します。

2. ジョブに移動し、 **[編集]** を選択して、ジョブのクラスターを構成します。 デプロイで使用する Apache Spark のバージョンに基づいて、Databricks Runtime のバージョンを設定します。 次に、 **[詳細オプション] > [Init スクリプト]** を選択し、Init スクリプトのパスを `dbfs:/spark-dotnet/db-init.sh` に設定します。 **[確認]** を選択して、クラスターの設定を確認します。

3. ジョブに移動し、 **[今すぐ実行]** を選択して、新しく構成した Spark クラスターでジョブを実行します。 ジョブのクラスターが作成されるまで数分かかります。 作成されると、ジョブが送信されます。 出力を表示するには、Databricks ワークスペースの左側のメニューから **[クラスター]** を選択し、 **[Driver Logs]\(ドライバーのログ\)** を選択します。

## <a name="deploy-using-set-jar"></a>Set Jar を使用してデプロイする

別の方法として、Databricks ワークスペースで [Set Jar](https://docs.microsoft.com/azure/databricks/jobs#--create-a-job) を使用して、.NET for Apache Spark ジョブを Databricks に送信できます。 *Set Jar* を使用すると、既存のアクティブなクラスターにジョブを送信できます。

### <a name="one-time-setup"></a>1 回限りのセットアップ

1. Databricks クラスターに移動し、左側のメニューから **[ジョブ]** を選択し、 **[Set JAR]** を選択します。

2. 適切な `microsoft-spark-<spark-version>-<spark-dotnet-version>.jar` をアップロードします。

3. `<your-app-name>` の代わりに発行済みの実行可能ファイルの正しい名前が含まれるように、次のパラメーターを変更します。

    ```
    Main Class: org.apache.spark.deploy.dotnet.DotnetRunner
    Arguments /dbfs/apps/<your-app-name>.zip <your-app-name>
    ```

4. Init スクリプトが既に設定されている既存のクラスターを指すようにクラスターを構成します。

### <a name="publish-and-run-your-app"></a>アプリを発行して実行する

1. アプリを発行したこと、およびアプリケーション コードで `SparkSession.Stop()` が使用されていないことを確認します。

2. [Databricks CLI](https://docs.microsoft.com/azure/databricks/dev-tools/databricks-cli) を使用して、アプリケーションを Databricks クラスターにアップロードします。 たとえば、発行済みのアプリをクラスターにアップロードするには、次のコマンドを使用します。

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <your-app-name>.zip dbfs:/apps/<your-app-name>.zip
    ```

3. アプリにユーザー定義関数が含まれている場合は、アプリ アセンブリ (ユーザー定義関数とそれらの依存関係を含む DLL など) を各 *Microsoft.Spark.Worker* の作業ディレクトリに配置する必要があります。

    アプリケーション アセンブリを Databricks クラスターにアップロードします。

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <assembly>.dll dbfs:/apps/dependencies
    ```

    [db-init.sh](https://github.com/dotnet/spark/blob/master/deployment/db-init.sh) 内のアプリの依存関係セクションをコメント解除して、アプリの依存関係パスをポイントするように変更します。 次に、更新された *db-init.sh* をクラスターにアップロードします。

    ```console
    cd <path-to-db-init-and-install-worker>
    databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
    ```

4. **[Databricks クラスター] > [ジョブ] > <ジョブ名> > [今すぐ実行]** を選択して、ジョブを実行します。

## <a name="next-steps"></a>次のステップ

* [.NET for Apache Spark の概要](../tutorials/get-started.md)
* [.NET for Apache Spark アプリケーションを Databricks にデプロイする](../tutorials/databricks-deployment.md)
* [Azure Databricks のドキュメント](https://docs.microsoft.com/azure/azure-databricks/)
