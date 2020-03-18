---
title: ASP.NET Core Web API でのモデルのデプロイ
description: ASP.NET Core Web API を使用して、インターネット経由で予測用の ML.NET 感情分析機械学習モデルを提供します
ms.date: 11/07/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to
ms.openlocfilehash: b6801b7de5a17257be706f77a7a67aa87df96524
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397759"
---
# <a name="deploy-a-model-in-an-aspnet-core-web-api"></a>ASP.NET Core Web API でのモデルのデプロイ

ASP.NET Core Web API を使用して、事前トレーニング済みの ML.NET 機械学習モデルを Web 上に提供する方法について説明します。 Web API 経由でモデルを提供すると、標準の HTTP メソッドによる予測が可能になります。

## <a name="prerequisites"></a>前提条件

- [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラット フォーム開発" ワークロードと共にインストールされている
- PowerShell
- 事前トレーニング済みモデル [ML.NET Sentiment Analysis のチュートリアル](../tutorials/sentiment-analysis.md)を使用して独自のモデルを構築するか、この[事前トレーニング済みの感情分析の機械学習モデル](https://github.com/dotnet/samples/blob/master/machine-learning/models/sentimentanalysis/sentiment_model.zip)をダウンロードすること。

## <a name="create-aspnet-core-web-api-project"></a>ASP.NET Core Web API プロジェクトを作成する

1. Visual Studio 2017 を開きます。 メニューバーから、 **[ファイル] > [新規作成] > [プロジェクト]** の順に選択します。 [新しいプロジェクト] ダイアログで、 **[Visual C#]** ノードを選択し、 **[Web]** ノードを選択します。 次に、 **[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。 **[名前]** テキスト ボックスに「SentimentAnalysisWebAPI」と入力し、 **[OK]** を選択します。

1. ASP.NET Core プロジェクトの複数の種類が表示されているウィンドウで、 **[API]** を選択し、 **[OK]** を選択します。

1. 事前トレーニング済みの機械学習モデルを保存するための *MLModels* という名前のディレクトリをプロジェクト内に作成します。

    ソリューションエクスプローラーで、プロジェクトを右クリックし、[追加] > [新しいフォルダー] の順に選択します。 「MLModels」と入力し、Enter キーを押します。

1. **Microsoft.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。[参照] タブを選択し、「**Microsoft.ML**」を検索します。一覧からそのパッケージを選択し、[インストール] を選択します。 **[変更のプレビュー]** ダイアログで **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、[ライセンスの同意] ダイアログの **[同意する]** を選択します。

1. **Microsoft.Extensions.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択します。[参照] タブを選択し、「**Microsoft.Extensions.ML**」を検索します。一覧からそのパッケージを選択して、[インストール] ボタンを選択します。 **[変更のプレビュー]** ダイアログで **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、[ライセンスの同意] ダイアログの **[同意する]** を選択します。

### <a name="add-model-to-aspnet-core-web-api-project"></a>モデルを ASP.NET Core Web API プロジェクトに追加する

1. 事前トレーニング済みのモデルを *MLModels* ディレクトリにコピーします。
1. ソリューション エクスプローラーで、モデルの zip ファイルを右クリックし、[プロパティ] を選択します。 [プロパティ] で [出力ディレクトリにコピー] の値を [新しい場合はコピーする] に変更します。

## <a name="create-data-models"></a>データ モデルを作成する

入力データと予測のために、いくつかのクラスを作成する必要があります。 プロジェクトに新しいクラスを追加します。

1. データモデルを保存するための *DataModels* という名前のディレクトリをプロジェクト内に作成します。

    ソリューションエクスプローラーで、プロジェクトを右クリックし、[追加] > [新しいフォルダー] の順に選択します。 「DataModels」と入力し、**Enter** キーを押します。

2. ソリューション エクスプローラーで、*DataModels* ディレクトリを右クリックし、[追加] > [新しいアイテム] の順に選択します。
3. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを *SentimentData.cs* に変更します。 次に、 **[追加]** を選択します。 コードエディターで *SentimentData.cs* ファイルが開きます。 *SentimentData.cs* の先頭に次の using ステートメントを追加します。

    ```csharp
    using Microsoft.ML.Data;
    ```

    既存のクラス定義を削除し、次のコードを **SentimentData.cs** ファイルに追加します。

    ```csharp
    public class SentimentData
    {
        [LoadColumn(0)]
        public string SentimentText;

        [LoadColumn(1)]
        [ColumnName("Label")]
        public bool Sentiment;
    }
    ```

4. ソリューションエクスプローラーで、*DataModels* ディレクトリを右クリックし、 **[追加] > [新しいアイテム]** の順に選択します。
5. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを *SentimentPrediction.cs* に変更します。 次に、[追加] を選択します。 コード エディターに *SentimentPrediction.cs* ファイルが開きます。 *SentimentPrediction.cs* の先頭に次の using ステートメントを追加します。

    ```csharp
    using Microsoft.ML.Data;
    ```

    既存のクラス定義を削除し、次のコードを *SentimentPrediction.cs* ファイルに追加します。

    ```csharp
    public class SentimentPrediction : SentimentData
    {

        [ColumnName("PredictedLabel")]
        public bool Prediction { get; set; }

        public float Probability { get; set; }

        public float Score { get; set; }
    }
    ```

    `SentimentPrediction` は `SentimentData` を継承します。 これにより、モデルによって生成された出力データと一緒に `SentimentText` プロパティの元のデータを容易に確認できます。

## <a name="register-predictionenginepool-for-use-in-the-application"></a>アプリケーションで使用する PredictionEnginePool を登録する

1 つの予測を作成するには、[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成する必要があります。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 さらに、アプリケーション内で必要なすべての場所にそのインスタンスを作成する必要があります。 アプリケーションの規模が拡大すると、このプロセスが管理不能になる可能性があります。 パフォーマンスとスレッド セーフを向上させるには、依存性の挿入と `PredictionEnginePool` サービスを組み合わせて使用します。これにより、アプリケーション全体で使用する [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) オブジェクトの [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) が作成されます。

[ASP.NET Core での依存関係の挿入](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1)については、リンク先で提供されている詳しい情報を確認してください。

1. *Startup.cs* を開き、ファイルの先頭に次の using ステートメントを追加します。

    ```csharp
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.ML;
    using SentimentAnalysisWebAPI.DataModels;
    ```

2. *ConfigureServices* メソッドに次のコードを追加します。

    ```csharp
    services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
        .FromFile(modelName: "SentimentAnalysisModel", filePath:"MLModels/sentiment_model.zip", watchForChanges: true);
    ```

大まかに言えば、オブジェクトとサービスがアプリケーションによって要求されたときに、このコードでは、後で使用できるように手動ではなく自動的に初期化が実行されます。

機械学習モデルは静的ではありません。 新しいトレーニング データが使用可能になると、モデルの再トレーニングと再展開が行われます。 アプリケーションに最新バージョンのモデルを取り込む方法の 1 つは、アプリケーション全体を再展開することです。 ただし、これによってアプリケーションのダウンタイムが発生します。 `PredictionEnginePool` サービスには、アプリケーションを停止することなく、更新されたモデルを再度読み込むメカニズムが用意されています。

`watchForChanges` パラメーターを `true` に設定すると、`PredictionEnginePool` では、ファイル システムの変更通知をリッスンし、ファイルに変更があったときにイベントを発生させる [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher) が開始されます。 これにより、モデルを自動的に再度読み込む `PredictionEnginePool` のプロンプトが表示されます。

モデルは `modelName` パラメーターによって識別されるため、変更時にアプリケーションごとに複数のモデルを再度読み込むことができます。

> [!TIP]
> また、リモートに格納されているモデルを操作するときは `FromUri` メソッドを使用できます。 `FromUri` では、ファイルの変更イベントを監視するのではなく、リモートの場所の変更をポーリングします。 ポーリング間隔は既定で 5 分に設定されています。 アプリケーションの要件に基づいてポーリング間隔を増減することができます。 次のコード サンプルでは、`PredictionEnginePool` は、指定された URI に格納されているモデルを 1 分ごとにポーリングします。
>
>```csharp
>services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
>   .FromUri(
>       modelName: "SentimentAnalysisModel",
>       uri:"https://github.com/dotnet/samples/raw/master/machine-learning/models/sentimentanalysis/sentiment_model.zip",
>       period: TimeSpan.FromMinutes(1));
>```

## <a name="create-predict-controller"></a>予測コントローラーを作成する

HTTP 要求の受信の処理をするために、コントローラーを作成します。

1. ソリューション エクスプローラーで、*Controllers* ディレクトリを右クリックし、 **[追加] > [コントローラー]** の順に選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[空の API コントローラー]** を選択し、 **[追加]** を選択します。
1. プロンプトで、 **[コントローラー名]** フィールドを*PredictController.cs* に変更します。 次に、[追加] を選択します。 コードエディターで *PredictController.cs* ファイルが開きます。 *PredictController.cs* の先頭に次の using ステートメントを追加します。

    ```csharp
    using System;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.ML;
    using SentimentAnalysisWebAPI.DataModels;
    ```

    既存のクラス定義を削除し、次のコードを *PredictController.cs* ファイルに追加します。

    ```csharp
    public class PredictController : ControllerBase
    {
        private readonly PredictionEnginePool<SentimentData, SentimentPrediction> _predictionEnginePool;

        public PredictController(PredictionEnginePool<SentimentData,SentimentPrediction> predictionEnginePool)
        {
            _predictionEnginePool = predictionEnginePool;
        }

        [HttpPost]
        public ActionResult<string> Post([FromBody] SentimentData input)
        {
            if(!ModelState.IsValid)
            {
                return BadRequest();
            }

            SentimentPrediction prediction = _predictionEnginePool.Predict(modelName: "SentimentAnalysisModel", example: input);

            string sentiment = Convert.ToBoolean(prediction.Prediction) ? "Positive" : "Negative";

            return Ok(sentiment);
        }
    }
    ```

このコードでは、`PredictionEnginePool` を依存関係の挿入によって取得してコントローラーのコンストラクターに渡すことで割り当てています。 次に、`Predict` コントローラーの `Post` メソッドで `PredictionEnginePool` を使用し、`SentimentAnalysisModel` クラスに登録されている `Startup` を使用して予測を行い、成功した場合に結果をユーザーに返します。

## <a name="test-web-api-locally"></a>Web API をローカルでテストする

すべてが設定されたので、アプリケーションをテストしましょう。

1. アプリケーションを実行します。
1. PowerShell を開き、次のコードを入力します。PORT には、アプリケーションがリスニングしているポートを入力します。

    ```powershell
    Invoke-RestMethod "https://localhost:<PORT>/api/predict" -Method Post -Body (@{SentimentText="This was a very bad steak"} | ConvertTo-Json) -ContentType "application/json"
    ```

    成功した場合、出力は次のテキストのようになります。

    ```powershell
    Negative
    ```

おめでとうございます! ASP.NET Core Web API を使用して、インターネット経由で予測を実行するモデルを正常に提供できました。

## <a name="next-steps"></a>次の手順

- [Azure へのデプロイ](/aspnet/core/tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)
