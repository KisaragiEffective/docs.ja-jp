---
title: Azure Functions にモデルをデプロイする
description: Azure Functions を使用して、インターネット経由で予測用の ML.NET 感情分析機械学習モデルを提供します
ms.date: 02/21/2020
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 33afd568bb12b855a3888bec31f2e9bbc3c720da
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77628671"
---
# <a name="deploy-a-model-to-azure-functions"></a>Azure Functions にモデルをデプロイする

Azure Functions のサーバーレス環境を介して、HTTP 経由での予測のために、事前トレーニング済みの ML.NET 機械学習モデルをデプロイする方法について説明します。

> [!NOTE]
> このサンプルでは、`PredictionEnginePool` サービスのプレビュー バージョンを実行します。

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラット フォーム開発" ワークロードおよび "Azure 開発" とともにインストールされていること。
- [Azure Functions ツール](/azure/azure-functions/functions-develop-vs#check-your-tools-version)
- PowerShell
- 事前トレーニング済みのモデル。 [ML.NET Sentiment Analysis のチュートリアル](../tutorials/sentiment-analysis.md)を使用して独自のモデルを構築するか、こちらの[事前トレーニング済みの感情分析の機械学習モデル](https://github.com/dotnet/samples/blob/master/machine-learning/models/sentimentanalysis/sentiment_model.zip)をダウンロードすること。

## <a name="azure-functions-sample-overview"></a>Azure Functions サンプルの概要

このサンプルは、事前トレーニング済みの二項分類モデルを使用してテキストのセンチメントを正または負として分類する、**C# HTTP トリガー Azure Functions アプリケーション**です。 Azure Functions では、クラウド内の管理されたサーバーレス環境で小規模なコードを大規模に実行する簡単な方法を提供します。 このサンプルのコードについては、GitHub の [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction) リポジトリで見つけることができます。

## <a name="create-azure-functions-project"></a>Azure Functions プロジェクトを作成する

1. Visual Studio 2017 を開きます。 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** をメニュー バーから選択します。 **[新しいプロジェクト]** ダイアログで、 **[Visual C#]** ノードを選択し、 **[クラウド]** ノードを選択します。 次に、**Azure Functions** プロジェクト テンプレートを選択します。 **[名前]** テキスト ボックスに「SentimentAnalysisFunctionsApp」と入力し、 **[OK]** を選択します。
1. **[新しいプロジェクト]** ダイアログで、プロジェクト オプションの上のドロップダウンを開き、 **[Azure Functions v2 (.NET Core)]** を選択します。 次に、 **[Http トリガー]** プロジェクトを選択し、 **[OK]** ボタンを選択します。
1. モデルを保存するための *MLModels* という名前のディレクトリをプロジェクト内に作成します。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 「MLModels」と入力し、Enter キーを押します。

1. **Microsoft.ML NuGet パッケージ** バージョン **1.3.1** をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として "nuget.org" を選択し、[参照] タブを選択して「**Microsoft.ML**」を検索します。一覧からそのパッケージを選択し、 **[インストール]** を選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

1. **Microsoft.Azure.Functions.Extensions NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択します。[参照] タブを選択し、「**Microsoft.Azure.Functions.Extensions**」を検索します。一覧からそのパッケージを選択し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

1. **Microsoft.Extensions.ML NuGet パッケージ** バージョン **0.15.1** をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択します。[参照] タブを選択し、「**Microsoft.Extensions.ML**」を検索します。一覧からそのパッケージを選択して、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

1. 次に従って **Microsoft.NET.Sdk.Functions NuGet パッケージ** バージョン **1.0.31** をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択します。[インストール済み] タブを選択し、「**Microsoft.NET.Sdk.Functions**」を検索します。一覧からそのパッケージを選択し、[バージョン] ドロップダウンから **1.0.31** を選択し、 **[更新]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

## <a name="add-pre-trained-model-to-project"></a>事前トレーニング済みモデルをプロジェクトに追加する

1. 構築済みのモデルを *MLModels* フォルダーにコピーします。
1. ソリューション エクスプローラーで、構築済みのモデルのファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

## <a name="create-azure-function-to-analyze-sentiment"></a>Azure 関数を作成してセンチメントを分析する

センチメントを予測するクラスを作成します。 プロジェクトに新しいクラスを追加します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。

1. **[新しい項目の追加]** ダイアログ ボックスで、 **[Azure 関数]** を選択し、 **[名前]** フィールドを *SentimentData.cs* に変更します。 次に **[追加]** を選択します。

1. **[新しい Azure 関数]** ダイアログ ボックスで、 **[Http トリガー]** を選択します。 次に、 **[OK]** ボタンを選択します。

    コード エディターに *AnalyzeSentiment.cs* ファイルが開きます。 *AnalyzeSentiment.cs* の先頭に次の `using` ステートメントを追加します。

    [!code-csharp [AnalyzeUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L1-L11)]

    既定では、`AnalyzeSentiment` クラスは `static`です。 クラス定義から `static` キーワードを必ず削除します。

    ```csharp
    public class AnalyzeSentiment
    {

    }
    ```

## <a name="create-data-models"></a>データモデルを作成する

入力データと予測のために、いくつかのクラスを作成する必要があります。 プロジェクトに新しいクラスを追加します。

1. データ モデルを保存するための *DataModels* という名前のディレクトリをプロジェクト内に作成します。ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[追加]、[新しいフォルダー]** の順に選択します。 「DataModels」と入力し、Enter キーを押します。
2. ソリューション エクスプローラーで、*DataModels* ディレクトリを右クリックし、 **[追加]、[新しいアイテム]** の順に選択します。
3. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを *SentimentData.cs* に変更します。 次に **[追加]** を選択します。

    コードエディターで *SentimentData.cs* ファイルが開きます。 *SentimentData.cs* の先頭に次の using ステートメントを追加します。

    [!code-csharp [SentimentDataUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L1)]

    既存のクラス定義を削除し、次のコードを *SentimentData.cs* ファイルに追加します。

    [!code-csharp [SentimentData](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L5-L13)]

4. ソリューションエクスプローラーで、*DataModels* ディレクトリを右クリックし、 **[追加] > [新しいアイテム]** の順に選択します。
5. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを *SentimentPrediction.cs* に変更します。 次に **[追加]** を選択します。 コード エディターに *SentimentPrediction.cs* ファイルが開きます。 *SentimentPrediction.cs* の先頭に次の using ステートメントを追加します。

    [!code-csharp [SentimentPredictionUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L1)]

    既存のクラス定義を削除し、次のコードを *SentimentPrediction.cs* ファイルに追加します。

    [!code-csharp [SentimentPrediction](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L5-L14)]

    `SentimentPrediction` は `SentimentData` を継承し、モデルによって生成された出力だけでなく `SentimentText` プロパティで元のデータへのアクセスを提供します。

## <a name="register-predictionenginepool-service"></a>PredictionEnginePool サービスを登録する

1 つの予測を作成するには、[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成する必要があります。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 さらに、アプリケーション内で必要なすべての場所にそのインスタンスを作成する必要があります。 アプリケーションの規模が拡大すると、このプロセスが管理不能になる可能性があります。 パフォーマンスとスレッド セーフを向上させるには、依存性の挿入と `PredictionEnginePool` サービスを組み合わせて使用します。これにより、アプリケーション全体で使用する [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。

依存関係の注入の詳細については、[こちらのリンク](https://en.wikipedia.org/wiki/Dependency_injection)を参照してください。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを「*Startup.cs*」に変更します。 次に **[追加]** を選択します。
1. *Startup.cs* の先頭に次の using ステートメントを追加します。

    [!code-csharp [StartupUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L1-L6)]

1. using ステートメントの下にある既存のコードを削除し、次のコードを追加します。

    ```csharp
    [assembly: FunctionsStartup(typeof(Startup))]
    namespace SentimentAnalysisFunctionsApp
    {
        public class Startup : FunctionsStartup
        {

        }
    }
    ```

1. アプリが実行されている環境と `Startup` クラス内でモデルが配置されるファイル パスを格納する変数を定義します。

    [!code-csharp [DefineStartupVars](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L13-L14)]

1. その下で、コンストラクターを作成して、`_environment` 変数と `_modelPath` 変数の値を設定します。 アプリケーションがローカルで実行されている場合、既定の環境は *Development* になります。

    [!code-csharp [StartupCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L16-L29)]

1. 次に、`Configure` という名前の新しいメソッドを追加して、`PredictionEnginePool` サービスをコンストラクターの下に登録します。

    [!code-csharp [ConfigureServices](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L31-L35)]

大まかに言えば、オブジェクトとサービスがアプリケーションによって要求されたときに、このコードでは、後で使用できるように手動ではなく自動的に初期化が実行されます。

機械学習モデルは静的ではありません。 新しいトレーニング データが使用可能になると、モデルの再トレーニングと再展開が行われます。 アプリケーションに最新バージョンのモデルを取り込む方法の 1 つは、アプリケーション全体を再展開することです。 ただし、これによってアプリケーションのダウンタイムが発生します。 `PredictionEnginePool` サービスには、アプリケーションを停止することなく、更新されたモデルを再度読み込むメカニズムが用意されています。

`watchForChanges` パラメーターを `true` に設定すると、`PredictionEnginePool` では、ファイル システムの変更通知をリッスンし、ファイルに変更があったときにイベントを発生させる [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher) が開始されます。 これにより、モデルを自動的に再度読み込む `PredictionEnginePool` のプロンプトが表示されます。

モデルは `modelName` パラメーターによって識別されるため、変更時にアプリケーションごとに複数のモデルを再度読み込むことができます。

> [!TIP]
> また、リモートに格納されているモデルを操作するときは `FromUri` メソッドを使用できます。 `FromUri` では、ファイルの変更イベントを監視するのではなく、リモートの場所の変更をポーリングします。 ポーリング間隔は既定で 5 分に設定されています。 アプリケーションの要件に基づいてポーリング間隔を増減することができます。 次のコード サンプルでは、`PredictionEnginePool` は、指定された URI に格納されているモデルを 1 分ごとにポーリングします。
>
>```csharp
>builder.Services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
>   .FromUri(
>       modelName: "SentimentAnalysisModel",
>       uri:"https://github.com/dotnet/samples/raw/master/machine-learning/models/sentimentanalysis/sentiment_model.zip",
>       period: TimeSpan.FromMinutes(1));
>```

## <a name="load-the-model-into-the-function"></a>関数にモデルを読み込む

次のコードを *AnalyzeSentiment* クラス内に挿入します。

[!code-csharp [AnalyzeCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L18-L24)]

このコードでは、依存関係の挿入を通して取得する関数のコンストラクターに渡すことで、`PredictionEnginePool` を割り当てます。

## <a name="use-the-model-to-make-predictions"></a>モデルを使用して予測を行う

*AnalyzeSentiment* クラスの *Run* メソッドの既存の実装を、次のコードに置き換えます。

[!code-csharp [AnalyzeRunMethod](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L26-L45)]

`Run` メソッドが実行されると、HTTP 要求からの受信データが逆シリアル化されて、`PredictionEnginePool` への入力として使用されます。 次に、`Predict` メソッドが呼び出され、`Startup` クラスに登録されている `SentimentAnalysisModel` を使用して予測が行われ、成功した場合に結果がユーザーに返されます。

## <a name="test-locally"></a>ローカルでテストする

すべてが設定されたので、アプリケーションをテストします。

1. アプリケーションの実行
1. PowerShell を開き、プロンプトにコードを入力します。PORT は、アプリケーションが実行されているポートです。 通常、このポートは 7071 です。

    ```powershell
    Invoke-RestMethod "http://localhost:<PORT>/api/AnalyzeSentiment" -Method Post -Body (@{SentimentText="This is a very bad steak"} | ConvertTo-Json) -ContentType "application/json"
    ```

    成功した場合、出力は次のテキストのようになります。

    ```powershell
    Negative
    ```

おめでとうございます! Azure 関数を使用したインターネット経由での予測の実行に対して、モデルを正常に提供できました。

## <a name="next-steps"></a>次の手順

- [Azure に配置する](/azure/azure-functions/functions-develop-vs#publish-to-azure)
