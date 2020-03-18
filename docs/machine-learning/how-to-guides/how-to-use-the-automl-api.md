---
title: ML.NET の自動 ML API を使用する方法
description: ML.NET の自動 ML API によって、モデル構築プロセスが自動化され、展開できる状態のモデルが生成されます。 自動機械学習タスクの構成に使用できるオプションについて説明します。
ms.date: 12/18/2019
ms.custom: mvc,how-to
ms.openlocfilehash: b322c484282d025033d747d2093f7b5b4d216fde
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75636563"
---
# <a name="how-to-use-the-mlnet-automated-machine-learning-api"></a>ML.NET の自動機械学習 API を使用する方法

自動機械学習 (AutoML) によって、機械学習をデータに適用するプロセスが自動化されます。 データセットがある場合、さまざまなデータの特徴付け、機械学習アルゴリズム、およびハイパーパラメーターを反復処理する AutoML の**実験**を実行することで、最適なモデルを選択できます。

> [!NOTE]
> このトピックは、現在プレビュー段階の ML.NET 用の自動機械学習 API について述べています。 内容は変更される場合があります。

## <a name="load-data"></a>データの読み込み

自動機械学習は、[IDataView](xref:Microsoft.ML.IDataView) へのデータ セットの読み込みをサポートしています。 タブ区切り値 (TSV) ファイルとコンマ区切り値 (CSV) ファイルの形式のデータを使用できます。

例:

```csharp
using Microsoft.ML;
using Microsoft.ML.AutoML;
    // ...
    MLContext mlContext = new MLContext();
    IDataView trainDataView = mlContext.Data.LoadFromTextFile<SentimentIssue>("my-data-file.csv", hasHeader: true);
```

## <a name="select-the-machine-learning-task-type"></a>機械学習タスクの種類を選択する

実験を作成する前に、解決する機械学習の問題の種類を決定します。 自動機械学習は、以下の ML タスクをサポートします。

* 二項分類
* 多クラス分類
* 回帰
* 推奨事項

## <a name="create-experiment-settings"></a>実験設定を作成する

決定した ML タスクの種類の実験設定を作成します。

* 二項分類

  ```csharp
  var experimentSettings = new BinaryExperimentSettings();
  ```

* 多クラス分類

  ```csharp
  var experimentSettings = new MulticlassExperimentSettings();
  ```

* 回帰

  ```csharp
  var experimentSettings = new RegressionExperimentSettings();
  ```

* 推奨事項

  ```csharp
  var experimentSettings = new RecommendationExperimentSettings();
  ```

## <a name="configure-experiment-settings"></a>実験設定を構成する

実験は高度な構成が可能です。 構成設定の詳細な一覧については、[AutoML API ドキュメント](https://docs.microsoft.com/dotnet/api/microsoft.ml.automl?view=ml-dotnet-preview)を参照してください。

次に、それらの例の一部を示します。

1. 実験を実行できる最長時間を指定します。

    ```csharp
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    ```

1. 実験が完了する前にキャンセル トークンを使用して実験を取り消します。

    ```csharp
    experimentSettings.CancellationToken = cts.Token;

    // Cancel experiment after the user presses any key
    CancelExperimentAfterAnyKeyPress(cts);
    ```

1. 別の最適化メトリックを指定します。

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.OptimizingMetric = RegressionMetric.MeanSquaredError;
    ```

1. `CacheDirectory` 設定は、AutoML タスク中にトレーニングされたすべてのモデルが保存されるディレクトリへのポインターです。 `CacheDirectory` が null に設定されている場合、モデルはディスクに書き込まれるのではなくメモリに保持されます。

    ```csharp
    experimentSettings.CacheDirectory = null;
    ```

1. 特定のトレーナーを使用しないように自動 ML に指示します。

    最適化するトレーナーの既定の一覧がタスクごとに探索されます。 この一覧は実験ごとに変更できます。 たとえば、データセットの実行速度が遅いトレーナーは一覧から削除できます。 特定のトレーナーを最適化するには、`experimentSettings.Trainers.Clear()` を呼び出してから、使用するトレーナーを追加します。

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.Trainers.Remove(RegressionTrainer.LbfgsPoissonRegression);
    experimentSettings.Trainers.Remove(RegressionTrainer.OnlineGradientDescent);
    ```

ML タスクごとにサポートされるトレーナーの一覧は、以下の対応するリンクを参照してください。

* [サポートされる二項分類アルゴリズム](xref:Microsoft.ML.AutoML.BinaryClassificationTrainer)
* [サポートされる多クラス分類アルゴリズム](xref:Microsoft.ML.AutoML.MulticlassClassificationTrainer)
* [サポートされる回帰アルゴリズム](xref:Microsoft.ML.AutoML.RegressionTrainer)
* [サポートされるレコメンデーション アルゴリズム](xref:Microsoft.ML.AutoML.RecommendationTrainer)

## <a name="optimizing-metric"></a>最適化メトリック

上の例に示すように、最適化メトリックによって、モデルのトレーニング中に最適化されるメトリックが決まります。 選択できる最適化メトリックは、選択したタスクの種類によって決まります。 利用できるメトリックの一覧を次に示します。

|[二項分類](xref:Microsoft.ML.AutoML.BinaryClassificationMetric) | [多クラス分類](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric) |[回帰とレコメンデーション](xref:Microsoft.ML.AutoML.RegressionMetric)
|-- |-- |--
|正確度| LogLoss | RSquared
|AreaUnderPrecisionRecallCurve | LogLossReduction | MeanAbsoluteError
|AreaUnderRocCurve | MacroAccuracy | MeanSquaredError
|F1Score | MicroAccuracy | RootMeanSquaredError
|NegativePrecision | TopKAccuracy
|NegativeRecall |
|PositivePrecision
|PositiveRecall

## <a name="data-pre-processing-and-featurization"></a>データの前処理と特徴付け

> [!NOTE]
> 特徴列では、<xref:System.Boolean>、<xref:System.Single>、<xref:System.String> の種類のみがサポートされています。

データの前処理は既定で行われ、次の手順が自動的に実行されます。

1. 有用な情報がない特徴を削除する

    トレーニングおよび検証セットから有用な情報がない特徴を削除します。 これには、すべての値が欠落している、すべての行の値が同じである、またはカーディナリティが非常に高い (ハッシュ、ID、GUID など) 特徴が含まれます。

1. 欠落値の表示と補完

    欠落値のセルにそのデータ型の既定値を入力します。 入力列と同じ数のスロットを持つインジケーター特徴を追加します。 追加されるインジケーター特徴の値は、入力列の値が欠落している場合は `1`、それ以外の場合は `0` です。

1. 追加の特徴を生成する

    テキスト特徴の場合:ユニグラムとトライキャラクターグラムを使用する bag-of-word 特徴。

    カテゴリ別特徴の場合:カーディナリティの低い特徴向きのワンホット エンコードと、カーディナリティの高いカテゴリ別特徴向きのワンホットハッシュ エンコード。

1. 変換とエンコード

    一意の値がほとんどなく、カテゴリ別特徴に変換されるテキスト特徴。 カテゴリ別特徴のカーディナリティに応じて、ワンホット エンコードまたはワンホットハッシュ エンコードを実行します。

## <a name="exit-criteria"></a>終了基準

タスクを完了する基準を定義します。

1. 一定の時間で終了する - 実験設定に `MaxExperimentTimeInSeconds` を使用して、タスクを継続して実行する時間を秒単位で定義することができます。

1. キャンセル トークンで終了する - キャンセル トークンを使用して、タスクが完了する予定の前に取り消すことができます。

    ```csharp
    var cts = new CancellationTokenSource();
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    experimentSettings.CancellationToken = cts.Token;
    ```

## <a name="create-an-experiment"></a>実験を作成する

実験設定を構成したら、実験を作成する準備は完了です。

```csharp
RegressionExperiment experiment = mlContext.Auto().CreateRegressionExperiment(experimentSettings);
```

## <a name="run-the-experiment"></a>実験を実行する

実験を実行すると、データの前処理、学習アルゴリズムの選択、およびハイパーパラメーターの調整が開始されます。 `MaxExperimentTimeInSeconds` に達するか実験が終了するまで、AutoML によって特徴付け、学習アルゴリズム、およびハイパーパラメーターの組み合わせが継続して生成されます。

```csharp
ExperimentResult<RegressionMetrics> experimentResult = experiment
    .Execute(trainingDataView, LabelColumnName, progressHandler: progressHandler);
```

検証データ、列の目的を示す列情報、または事前特徴付け器を渡す場合は、`Execute()` のその他のオーバーロードを調べてください。

## <a name="training-modes"></a>トレーニング モード

### <a name="training-dataset"></a>トレーニング データセット

AutoML には、オーバーロードされた実験の実行でトレーニング データを指定できる方法が用意されています。 内部的には、自動 ML によってトレーニング分割と検証分割にデータが分けられます。

```csharp
experiment.Execute(trainDataView);
```

### <a name="custom-validation-dataset"></a>カスタム検証データセット

時系列データでは通常のことですが、ランダムな分割を許容できない場合は、カスタム検証データセットを使用します。 独自の検証データセットを指定できます。 モデルは、1 つ以上のランダムなデータセットではなく、指定した検証データセットに対して評価されます。

```csharp
experiment.Execute(trainDataView, validationDataView);
```

## <a name="explore-model-metrics"></a>モデルのメトリックを調べる

ML 実験の各反復処理の後に、そのタスクに関するメトリックは保存されます。

たとえば、最適な実行から検証メトリックにアクセスできます。

```csharp
RegressionMetrics metrics = experimentResult.BestRun.ValidationMetrics;
Console.WriteLine($"R-Squared: {metrics.RSquared:0.##}");
Console.WriteLine($"Root Mean Squared Error: {metrics.RootMeanSquaredError:0.##}");
```

ML タスクごとに利用できるすべてのメトリックを次に示します。

* [二項分類メトリック](xref:Microsoft.ML.AutoML.BinaryClassificationMetric)
* [多クラス分類メトリック](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric)
* [回帰とレコメンデーション メトリック](xref:Microsoft.ML.AutoML.RegressionMetric)

## <a name="see-also"></a>関連項目

すべてのコード サンプルについては、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master#automate-mlnet-models-generation-preview-state) GitHub リポジトリを参照してください。
