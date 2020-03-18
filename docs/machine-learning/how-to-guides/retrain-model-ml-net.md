---
title: モデルを再トレーニングする
description: ML.NET でモデルを再トレーニングする方法について説明します
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 735782a4a0877a917b6e1885f009aa49d834170f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73976964"
---
# <a name="re-train-a-model"></a>モデルを再トレーニングする

ML.NET で機械学習モデルを再トレーニングする方法について説明します。

世界とその周りのデータは一定のペースで変化します。 そのため、モデルも同様に変更および更新する必要があります。 ML.NET には、学習したモデルのパラメーターを出発点として使用してモデルを再トレーニングする機能が用意されており、毎回最初から始めるのではなく、以前のエクスペリエンスを継続的に利用できます。

ML.NET では以下のアルゴリズムを再トレーニングできます。

- [AveragedPerceptronTrainer](xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer)
- [FieldAwareFactorizationMachineTrainer](xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer)
- [LbfgsLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer)
- [LbfgsMaximumEntropyMulticlassTrainer](xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer)
- [LbfgsPoissonRegressionTrainer](xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer)
- [LinearSvmTrainer](xref:Microsoft.ML.Trainers.LinearSvmTrainer)
- [OnlineGradientDescentTrainer](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer)
- [SgdCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdCalibratedTrainer)
- [SgdNonCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdNonCalibratedTrainer)
- [SymbolicSgdLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer)

## <a name="load-pre-trained-model"></a>トレーニング済みモデルを読み込む

まず、トレーニング済みモデルをアプリケーションに読み込みます。 トレーニング パイプラインとモデルの読み込みの詳細については、「[トレーニング済みモデルの保存と読み込み](save-load-machine-learning-models-ml-net.md)」を参照してください。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define DataViewSchema of data prep pipeline and trained model
DataViewSchema dataPrepPipelineSchema, modelSchema;

// Load data preparation pipeline
ITransformer dataPrepPipeline = mlContext.Model.Load("data_preparation_pipeline.zip", out dataPrepPipelineSchema);

// Load trained model
ITransformer trainedModel = mlContext.Model.Load("ogd_model.zip", out modelSchema);
```

## <a name="extract-pre-trained-model-parameters"></a>トレーニング済みモデルのパラメーターを抽出する

モデルが読み込まれたら、トレーニング済みモデルの [`Model`](xref:Microsoft.ML.Data.PredictionTransformerBase`1.Model*) プロパティにアクセスして、学習済みモデルのパラメーターを抽出します。 トレーニング済みモデルは、[`OnlineGradientDescentTrainer`](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer) を出力する [`RegressionPredictionTransformer`](xref:Microsoft.ML.Data.RegressionPredictionTransformer%601) を作成する線形回帰モデル [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) を使用して学習されています。 これらの線形回帰モデルのパラメーターには、学習済みのバイアスと、モデルの重みまたは係数が含まれています。 これらの値は、新しい再トレーニング済みモデルの出発点として使用されます。

```csharp
// Extract trained model parameters
LinearRegressionModelParameters originalModelParameters =
    ((ISingleFeaturePredictionTransformer<object>)trainedModel).Model as LinearRegressionModelParameters;
```

## <a name="re-train-model"></a>モデルを再トレーニングする

モデルを再トレーニングするプロセスは、モデルをトレーニングするプロセスと同じです。 唯一の違いは、データに加えて [`Fit`](xref:Microsoft.ML.Trainers.OnlineLinearTrainer`2.Fit*) メソッドも元の学習済みモデル パラメーターを入力として受け取り、それらを再トレーニング プロセスの開始点として使用することです。

```csharp
// New Data
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 850f,
        HistoricalPrices = new float[] { 150000f,175000f,210000f },
        CurrentPrice = 205000f
    },
    new HousingData
    {
        Size = 900f,
        HistoricalPrices = new float[] { 155000f, 190000f, 220000f },
        CurrentPrice = 210000f
    },
    new HousingData
    {
        Size = 550f,
        HistoricalPrices = new float[] { 99000f, 98000f, 130000f },
        CurrentPrice = 180000f
    }
};

//Load New Data
IDataView newData = mlContext.Data.LoadFromEnumerable<HousingData>(housingData);

// Preprocess Data
IDataView transformedNewData = dataPrepPipeline.Transform(newData);

// Retrain model
RegressionPredictionTransformer<LinearRegressionModelParameters> retrainedModel =
    mlContext.Regression.Trainers.OnlineGradientDescent()
        .Fit(transformedNewData, originalModelParameters);
```

## <a name="compare-model-parameters"></a>モデルのパラメーターを比較する

再トレーニングが実際に行われたかどうかを判断するにはどうすればよいでしょうか。 1 つの方法は、再トレーニング済みモデルのパラメーターが元のモデルのものと異なるかどうかを比較することです。 次のコード サンプルでは、元のモデルと再トレーニング済みモデルの重みを比較し、それをコンソールに出力します。

```csharp
// Extract Model Parameters of re-trained model
LinearRegressionModelParameters retrainedModelParameters = retrainedModel.Model as LinearRegressionModelParameters;

// Inspect Change in Weights
var weightDiffs =
    originalModelParameters.Weights.Zip(
        retrainedModelParameters.Weights, (original, retrained) => original - retrained).ToArray();

Console.WriteLine("Original | Retrained | Difference");
for(int i=0;i < weightDiffs.Count();i++)
{
    Console.WriteLine($"{originalModelParameters.Weights[i]} | {retrainedModelParameters.Weights[i]} | {weightDiffs[i]}");
}
```

以下の表は、出力がどのようになるかを示しています。

|変更元 | 再トレーニング済み | 相違点 |
|---|---|---|
| 33039.86 | 56293.76 | -23253.9 |
| 29099.14 | 49586.03 | -20486.89 |
| 28938.38 | 48609.23 | -19670.85 |
| 30484.02 | 53745.43 | -23261.41 |
