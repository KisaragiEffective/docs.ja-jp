---
title: .NET for Apache Spark ワーカーとユーザー定義関数のバイナリを展開する
description: .NET for Apache Spark ワーカーとユーザー定義関数のバイナリを展開する方法について説明します。
ms.date: 01/21/2019
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: f373ccee398149adcadeac91f02d9896214706b0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187597"
---
# <a name="deploy-net-for-apache-spark-worker-and-user-defined-function-binaries"></a>.NET for Apache Spark ワーカーとユーザー定義関数のバイナリを展開する

この手引きでは、.NET for Apache Spark ワーカーとユーザー定義関数のバイナリを展開する方法について説明します。 設定する環境変数と、アプリケーションを `spark-submit` で起動するときによく使用されるパラメーターについて説明します。

## <a name="configurations"></a>構成
構成では、.NET for Apache Spark ワーカーおよびユーザー定義関数のバイナリを展開する場合の一般的な環境変数とパラメーター設定について説明します。

### <a name="environment-variables"></a>環境変数
ワーカーを展開して UDF を記述する場合、次のようないくつかの環境変数を設定する必要があります。

| 環境変数         | 説明
| :--------------------------- | :----------
| DOTNET_WORKER_DIR            | <code>Microsoft.Spark.Worker</code> バイナリが生成されるパス。</br>これは Spark ドライバーによって使用され、Spark Executor に渡されます。 この変数を設定しないと、Spark Executor は <code>PATH</code> 環境変数から指定したパスを検索します。</br>_例:"C:\bin\Microsoft.Spark.Worker"_
| DOTNET_ASSEMBLY_SEARCH_PATHS | <code>Microsoft.Spark.Worker</code> がアセンブリを読み込むコンマ区切りのパス。</br>パスが "." で始まる場合、作業ディレクトリが先頭に追加されます。 **yarn モード**の場合、"." はコンテナーの作業ディレクトリを表します。</br>_例:"C:\Users\\&lt;ユーザー名&gt;\\&lt;mysparkapp&gt;\bin\Debug\\&lt;dotnet バージョン&gt;"_
| DOTNET_WORKER_DEBUG          | <a href="https://github.com/dotnet/spark/blob/master/docs/developer-guide.md#debugging-user-defined-function-udf">UDF をデバッグしたい</a>場合、<code>spark-submit</code> の実行前にこの環境変数を <code>1</code> に設定します。

### <a name="parameter-options"></a>パラメーター オプション
一度[バンドル](https://spark.apache.org/docs/latest/submitting-applications.html#bundling-your-applications-dependencies)した Spark アプリケーションは、`spark-submit` を使用して起動できます。 一般的に使用されるオプションは、次の表のとおりです。

| パラメーター名        | 説明
| :---------------------| :----------
| --class               | お使いのアプリケーションのエントリ ポイント。</br>_例: org.apache.spark.deploy.dotnet.DotnetRunner_
| --master              | クラスターの<a href="https://spark.apache.org/docs/latest/submitting-applications.html#master-urls">マスター URL</a>。</br>_例: yarn_
| --deploy-mode         | ご自分のドライバーをワーカー ノードに展開 (<code>cluster</code>) するか、外部クライアントとしてローカルに展開 (<code>client</code>) するか。</br>既定値: <code>client</code>
| --conf                | Spark の <code>key=value</code> 形式での任意の構成プロパティ。</br>_例: spark.yarn.appMasterEnv.DOTNET_WORKER_DIR=.\worker\Microsoft.Spark.Worker_
| --files               | 各 Executor の作業ディレクトリに展開されるファイルのコンマ区切りのリスト。<br/><ul><li>なお、このオプションは、yarn モードでのみ使用できます。</li><li>ファイル名は、Hadoop と同様 # を使用して指定できます。</br></ul>_例: <code>myLocalSparkApp.dll#appSeen.dll</code>YARN で実行する場合、<code>myLocalSparkApp.dll</code> を参照するには、お使いのアプリケーションで <code>appSeen.dll</code> の名前を使用する必要があります。_</li>
| --archives          | 各 Executor の作業ディレクトリに抽出されるアーカイブのコンマ区切りの一覧。</br><ul><li>なお、このオプションは、yarn モードでのみ使用できます。</li><li>ファイル名は、Hadoop と同様 # を使用して指定できます。</br></ul>_例: <code>hdfs://&lt;path to your worker file&gt;/Microsoft.Spark.Worker.zip#worker</code>これで、この zip ファイルはコピーされ <code>worker</code> フォルダーに抽出されます。_</li>
| application-jar       | お使いのアプリケーションとすべての依存関係が含まれた、バンドルされている jar へのパス。</br>_例: hdfs://&lt;お使いの jar へのパス&gt;/microsoft-spark-&lt;バージョン&gt;.jar_
| application-arguments | (存在する場合) お使いのメイン クラスのメイン メソッドに渡される引数。</br>_例: hdfs://&lt;お使いのアプリへのパス&gt;/&lt;お使いのアプリ&gt;.zip &lt;お使いのアプリ名&gt; &lt;アプリの引数&gt;_

> [!NOTE]
> `spark-submit` を使用してアプリケーションを起動する場合、`--options` はすべて `application-jar` の前に指定してください。そのようにしないと、それらは無視されます。 詳細については、[`spark-submit` オプション](https://spark.apache.org/docs/latest/submitting-applications.html)に関するページと [YARN での spark の実行](https://spark.apache.org/docs/latest/running-on-yarn.html)に関するページを参照してください。

## <a name="frequently-asked-questions"></a>よく寄せられる質問
### <a name="when-i-run-a-spark-app-with-udfs-i-get-a-filenotfoundexception-error-what-should-i-do"></a>UDF で spark アプリを実行すると、`FileNotFoundException' エラーが表示されます。 どうすればいいですか。
> **エラー:** [エラー] [TaskRunner] [0] ProcessStream() が次の例外で失敗しました:System.IO.FileNotFoundException:Assembly 'mySparkApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null' ファイルが見つかりません: 'mySparkApp.dll'

**回答:** `DOTNET_ASSEMBLY_SEARCH_PATHS` 環境変数の設定が正しいことを確認します。 これは、お使いの `mySparkApp.dll` を含むパスである必要があります。

### <a name="after-i-upgraded-my-net-for-apache-spark-version-and-reset-the-dotnet_worker_dir-environment-variable-why-do-i-still-get-the-following-ioexception-error"></a>.NET for Apache Spark バージョンをアップグレードし、`DOTNET_WORKER_DIR` 環境変数をリセットした後も次の `IOException` エラーが表示されるのはなぜですか。
> **エラー:** ステージ 11.0 でタスク 0.0 が失われました (TID 24, localhost, executor driver): java.io.IOException:"Microsoft.Spark.Worker.exe" プログラムを実行できません:CreateProcess error=2, システムに指定されたファイルが見つかりません。

**回答:** 最新の環境変数値が使用されるよう、まずはお使いの PowerShell ウィンドウ (またはその他のコマンド ウィンドウ) の再起動を試行してください。 その後、お使いのプログラムを起動します。

### <a name="after-submitting-my-spark-application-i-get-the-error-systemtypeloadexception-could-not-load-type-systemruntimeremotingcontextscontext"></a>自分の Spark アプリケーションの送信後、エラー `System.TypeLoadException: Could not load type 'System.Runtime.Remoting.Contexts.Context'` が表示されます。
> **エラー:** [エラー] [TaskRunner] [0] ProcessStream() が次の例外で失敗しました:System.TypeLoadException:型 'System.Runtime.Remoting.Contexts.Context' をアセンブリ 'mscorlib から読み込むことができませんでした, Version=4.0.0.0, Culture=neutral, PublicKeyToken=...'.

**回答:** お使いの `Microsoft.Spark.Worker` のバージョンを確認してください。 **.NET Framework 4.6.1** と **.NET Core 2.1.x** の 2 つのバージョンがあります。 `System.Runtime.Remoting.Contexts.Context` は .NET Framework のみ用であるため、この場合は、([ダウンロード](https://github.com/dotnet/spark/releases)可能な) `Microsoft.Spark.Worker.net461.win-x64-<version>` を使用する必要があります。

### <a name="how-do-i-run-my-spark-application-with-udfs-on-yarn-which-environment-variables-and-parameters-should-i-use"></a>YARN 上の UDF で spark アプリケーションはどのように実行できますか。 どの環境変数とパラメーターを使用すればよいでしょう。

**回答:** YARN で spark アプリケーションを起動するには、`spark.yarn.appMasterEnv.[EnvironmentVariableName]` のように環境変数を指定する必要があります。 `spark-submit` の使用例を次に示します。

```powershell
spark-submit \
--class org.apache.spark.deploy.dotnet.DotnetRunner \
--master yarn \
--deploy-mode cluster \
--conf spark.yarn.appMasterEnv.DOTNET_WORKER_DIR=./worker/Microsoft.Spark.Worker-<version> \
--conf spark.yarn.appMasterEnv.DOTNET_ASSEMBLY_SEARCH_PATHS=./udfs \
--archives hdfs://<path to your files>/Microsoft.Spark.Worker.net461.win-x64-<version>.zip#worker,hdfs://<path to your files>/mySparkApp.zip#udfs \
hdfs://<path to jar file>/microsoft-spark-2.4.x-<version>.jar \
hdfs://<path to your files>/mySparkApp.zip mySparkApp
```

## <a name="next-steps"></a>次の手順

* [.NET for Apache Spark の概要](../tutorials/get-started.md)
* [Windows で .NET for Apache Spark アプリケーションをデバッグする](../how-to-guides/debug.md)
