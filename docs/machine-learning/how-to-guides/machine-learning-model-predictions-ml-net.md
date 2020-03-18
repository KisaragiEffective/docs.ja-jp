---
title: トレーニング済みモデルを使用して予測する
description: トレーニング済みモデルを使用して予測する方法について説明します
ms.date: 09/18/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 182350cc5143155133385c6fd77986b271f6db91
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73977040"
---
# <a name="make-predictions-with-a-trained-model"></a>トレーニング済みモデルを使用して予測する

トレーニング済みモデルを使用して予測する方法について説明します

## <a name="create-data-models"></a>データ モデルを作成する

### <a name="input-data"></a>入力データ

```csharp
public class HousingData
{
    [LoadColumn(0)]
    public float Size { get; set; }

    [LoadColumn(1, 3)]
    [VectorType(3)]
    public float[] HistoricalPrices { get; set; }

    [LoadColumn(4)]
    [ColumnName("Label")]
    public float CurrentPrice { get; set; }
}
```

### <a name="output-data"></a>出力データ

`Features` と `Label` の入力列名と同様に、ML.NET にはモデルによって生成される予測値列の既定の名前があります。 タスクによっては名前が異なる場合があります。

このサンプルで使用されているアルゴリズムは線形回帰アルゴリズムなので、出力列の既定の名前は `Score` です。これは [ プロパティの `ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute)`PredictedPrice` 属性によって定義されます。

```csharp
class HousingPrediction
{
    [ColumnName("Score")]
    public float PredictedPrice { get; set; }
}
```

## <a name="set-up-a-prediction-pipeline"></a>予測パイプラインを設定する

1 つの予測でもバッチ予測でも、予測パイプラインをアプリケーションに読み込む必要があります。 このパイプラインには、データの前処理変換とトレーニング済みモデルの両方が含まれています。 次のコード スニペットは、`model.zip` という名前のファイルから予測パイプラインを読み込みます。

```csharp
//Create MLContext
MLContext mlContext = new MLContext();

// Load Trained Model
DataViewSchema predictionPipelineSchema;
ITransformer predictionPipeline = mlContext.Model.Load("model.zip", out predictionPipelineSchema);
```

## <a name="single-prediction"></a>1 つの予測

1 つの予測を行うには、読み込まれた予測パイプラインを使用して [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成します。

```csharp
// Create PredictionEngines
PredictionEngine<HousingData, HousingPrediction> predictionEngine = mlContext.Model.CreatePredictionEngine<HousingData, HousingPrediction>(predictionPipeline);
```

次に、[`Predict`](xref:Microsoft.ML.PredictionEngineBase%602.Predict*) メソッドを使用して、入力データをパラメーターとして渡します。 [`Predict`](xref:Microsoft.ML.PredictionEngineBase%602.Predict*) メソッドを使用する場合、入力が [`IDataView`](xref:Microsoft.ML.IDataView) である必要はない点に注意してください。 これは、入力データ型のオブジェクトを渡すことができるように、入力データ型の操作を簡単に内部化できるためです。 さらに、`CurrentPrice` は新しいデータを使用して予測しようとしているターゲットまたはラベルなので、この時点では値がないと見なされます。

```csharp
// Input Data
HousingData inputData = new HousingData
{
    Size = 900f,
    HistoricalPrices = new float[] { 155000f, 190000f, 220000f }
};

// Get Prediction
HousingPrediction prediction = predictionEngine.Predict(inputData);
```

`Score` オブジェクトの `prediction` プロパティにアクセスすると、`150079` のような値になります。

## <a name="multiple-predictions"></a>複数の予測

次のデータがあるとして、[`IDataView`](xref:Microsoft.ML.IDataView) に読み込みます。 この場合、[`IDataView`](xref:Microsoft.ML.IDataView) の名前は `inputData` です。 `CurrentPrice` は新しいデータを使用して予測しようとしているターゲットまたはラベルなので、この時点では値がないと見なされます。

```csharp
// Actual data
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 850f,
        HistoricalPrices = new float[] { 150000f, 175000f, 210000f }
    },
    new HousingData
    {
        Size = 900f,
        HistoricalPrices = new float[] { 155000f, 190000f, 220000f }
    },
    new HousingData
    {
        Size = 550f,
        HistoricalPrices = new float[] { 99000f, 98000f, 130000f }
    }
};
```

次に、[`Transform`](xref:Microsoft.ML.ITransformer.Transform*) メソッドを使用してデータ変換を適用し、予測を生成します。

```csharp
// Predicted Data
IDataView predictions = predictionPipeline.Transform(inputData);
```

[`GetColumn`](xref:Microsoft.ML.Data.ColumnCursorExtensions.GetColumn*) メソッドを使用して予測値を調べます。

```csharp
// Get Predictions
float[] scoreColumn = predictions.GetColumn<float>("Score").ToArray();
```

スコア列の予測値は次のようになります。

| 監視 | 予測 |
|---|---|
| 1 | 144638.2 |
| 2 | 150079.4 |
| 3 | 107789.8 |
