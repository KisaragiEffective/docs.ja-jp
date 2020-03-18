---
title: Windows で .NET for Apache Spark アプリケーションをデバッグする
description: Windows で .NET for Apache Spark アプリケーションをデバッグする方法について学習します。
ms.date: 01/29/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: dac6aed1f7faba7f07b722a6dac0da930ab9ec66
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79185805"
---
# <a name="debug-a-net-for-apache-spark-application"></a>.NET for Apache Spark アプリケーションをデバッグする

このハウツーでは、Windows 上で .NET for Apache Spark アプリケーションをデバッグするための手順について説明します。

## <a name="debug-your-application"></a>アプリケーションをデバッグする

新しいコマンド プロンプト ウィンドウを開き、次のコマンドを実行します。

```shell
spark-submit \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  debug
```

このコマンドを実行すると、次の出力が表示されます。

```console
***********************************************************************
* .NET Backend running debug mode. Press enter to exit *
***********************************************************************
```

デバッグ モードでは、DotnetRunner によって .NET アプリケーションが起動されることはなく、代わりにユーザーが .NET アプリを起動するのを待機します。 このコマンド プロンプト ウィンドウを開いたままにして、C# デバッガーを使用して .NET アプリケーションを起動し、アプリケーションをデバッグします。 C# デバッガー ([Windows または macOS 用 Visual Studio デバッガー](https://visualstudio.microsoft.com/vs/)、または [Visual Studio Code の C# デバッガー拡張機能](https://code.visualstudio.com/Docs/editor/debugging)) を使用して .NET アプリケーションを起動し、アプリケーションをデバッグします。

## <a name="debug-a-user-defined-function-udf"></a>ユーザー定義関数 (UDF) をデバッグする

> [!NOTE]
> ユーザー定義関数は、Windows で Visual Studio デバッガーを使用する場合にのみサポートされます。

`spark-submit` を実行する前に、次の環境変数を設定します。

```bat
set DOTNET_WORKER_DEBUG=1
```

Spark アプリケーションを実行すると、`Choose Just-In-Time Debugger` ウィンドウがポップアップ表示されます。 Visual Studio デバッガーを選択してください。

デバッガーは、[TaskRunner.cs](https://github.com/dotnet/spark/blob/5e9c08b430b4bc56b5f42252c4b73437377afaed/src/csharp/Microsoft.Spark.Worker/TaskRunner.cs#L52) 内の次の場所で中断します。

```csharp
if (EnvironmentUtils.GetEnvironmentVariableAsBool("DOTNET_WORKER_DEBUG"))
{
    Debugger.Launch(); // <-- The debugger will break here.
}
```

デバッグする予定の UDF が含まれている *.cs* ファイルに移動し、[ブレークポイントを設定します](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints?view=vs-2019)。 UDF を含むアセンブリがまだワーカーに読み込まれていないため、このブレークポイントでは `The breakpoint will not currently be hit` と表示されます。

`F5` キーを押してアプリケーションを続行すれば、最終的にこのブレークポイントはヒットするようになります。

> [!NOTE]
> [Just-In-Time デバッガーを選択する] ウィンドウは、タスクごとにポップアップ表示されます。 過剰なポップアップを回避するには、Executor の数を小さい数に設定します。 たとえば、spark-submit に対して **--master local[1]** オプションを指定し、タスク数を 1 に設定できます。これにより、1 つのデバッガー インスタンスが起動されます。

## <a name="debug-scala-code"></a>Scala コードをデバッグする

Scala 側のコード (`DotnetRunner`、`DotnetBackendHandler` など) をデバッグする必要がある場合は、次のコマンドを使い、[IntelliJ](https://www.jetbrains.com/help/idea/attaching-to-local-process.html) を使用して実行中のプロセスにデバッガーをアタッチできます。

```shell
spark-submit \
  --driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  <path-to-your-app-exe> <argument(s)-to-your-app>
```

コマンドを実行した後、[Intellij](https://www.jetbrains.com/help/idea/attaching-to-local-process.html) を使用して実行中のプロセスにデバッガーを追加します。

## <a name="next-steps"></a>次のステップ

* [.NET for Apache Spark の概要](../tutorials/get-started.md)
* [.NET for Apache Spark アプリケーションを Azure HDInsight にデプロイする](../tutorials/hdinsight-deployment.md)
* [.NET for Apache Spark アプリケーションを Databricks にデプロイする](../tutorials/databricks-deployment.md)
* [.NET for Apache Spark アプリケーションを Amazon EMR Spark にデプロイする](../tutorials/amazon-emr-spark-deployment.md)
