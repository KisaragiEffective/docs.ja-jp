---
title: モデルのトレーニングと評価
description: ML.NET を使用して、機械学習モデルの構築、メトリックの収集、およびパフォーマンスの測定を行う方法について説明します。 機械学習モデルによって、トレーニング データ内のパターンが識別され、新しいデータを使って予測が実行されます。
ms.date: 08/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to, title-hack-0625
ms.openlocfilehash: 0e0f43225b9bf243c31b3095817bdcbdb3123012
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73976761"
---
# <a name="train-and-evaluate-a-model"></a>モデルのトレーニングと評価

ML.NET を使用して、機械学習モデルの構築、メトリックの収集、およびパフォーマンスの測定を行う方法について説明します。 このサンプルでは回帰モデルがトレーニングされますが、この概念は他の主なアルゴリズムに適用できます。

## <a name="split-data-for-training-and-testing"></a>トレーニングとテストのためにデータを分割する

機械学習モデルの目的は、トレーニング データ内のパターンを識別することです。 これらのパターンは、新しいデータを使用して予測を行うために使用されます。

データは `HousingData` などのクラスでモデル化できます。

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

[`IDataView`](xref:Microsoft.ML.IDataView) にロードされた、次のデータがあるとします。

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f ,125000f ,122000f },
        CurrentPrice = 170000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 200000f, 250000f, 230000f },
        CurrentPrice = 225000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 126000f, 130000f, 200000f },
        CurrentPrice = 195000f
    },
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
```

[`TrainTestSplit`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit*) メソッドを使用して、データをトレーニング セットとテスト セットに分割します。 結果は、2 つの [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) メンバー (トレーニング セット用とテスト セット用) が含まれる [`IDataView`](xref:Microsoft.ML.IDataView) オブジェクトになります。 データ分割の割合は、`testFraction` パラメーターによって決まります。 以下のスニペットは、テスト セットの元のデータの 20% を保持しています。

```csharp
DataOperationsCatalog.TrainTestData dataSplit = mlContext.Data.TrainTestSplit(data, testFraction: 0.2);
IDataView trainData = dataSplit.TrainSet;
IDataView testData = dataSplit.TestSet;
```

## <a name="prepare-the-data"></a>データを準備する

機械学習モデルをトレーニングする前に、データを前処理する必要があります。 データ準備の詳細については、[データ準備のハウツー記事](prepare-data-ml-net.md)と [`transforms page`](../resources/transforms.md) を参照してください。

ML.NET のアルゴリズムには、入力列の型に制約があります。 さらに、値が指定されていない場合は、入力列名と出力列名に既定値が使用されます。

### <a name="working-with-expected-column-types"></a>予想される列の型を使用する

ML.NET の機械学習アルゴリズムは、入力として既知のサイズの float 型のベクターを想定しています。 すべてのデータが既に数値形式であり、一緒に処理されることが意図されている場合 (つまり、画像のピクセル)、データ モデルに [`VectorType`](xref:Microsoft.ML.Data.VectorTypeAttribute) 属性を適用します。

データがすべて数値ではなく、各列に異なるデータ変換が個別に適用される場合は、すべての列が処理された後に [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) メソッドを使用して、個々の列すべてを、新しい列に出力される 1 つの特徴ベクターに結合します。

次のスニペットは、`Size` 列と `HistoricalPrices` 列を 1 つの特徴ベクターにまとめ、それを `Features` という名前の新しい列に出力します。 スケールに違いがあるため、データを正規化するように [`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*) が `Features` 列に適用されます。

```csharp
// Define Data Prep Estimator
// 1. Concatenate Size and Historical into a single feature vector output to a new column called Features
// 2. Normalize Features vector
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", "Size", "HistoricalPrices")
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data prep transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(trainData);

// Apply transforms to training data
IDataView transformedTrainingData = dataPrepTransformer.Transform(trainData);
```

### <a name="working-with-default-column-names"></a>既定の列名を使用する

何も指定されていない場合、ML.NET アルゴリズムでは既定の列名が使用されます。 すべてのトレーナーには、アルゴリズムの入力用に `featureColumnName` というパラメーターがあり、適用可能な場合は `labelColumnName` という予期される値のパラメーターもあります。 既定では、これらの値はそれぞれ `Features` と `Label` です。

前処理中に [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) メソッドを使用して `Features` という名前の新しい列を作成すると、前処理済みの `IDataView` に既に存在するため、アルゴリズムのパラメーターで特徴列の名前を指定する必要がなくなります。 このラベル列は `CurrentPrice` ですが、データ モデルでは [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) 属性が使用されているため、ML.NET によって `CurrentPrice` 列の名前が `Label` に変更され、機械学習アルゴリズム エスティメーターに `labelColumnName` パラメーターを指定する必要がなくなります。

既定の列名を使用しない場合は、後続のスニペットで示すように、機械学習アルゴリズム エスティメーターを定義するときに、特徴列とラベル列の名前をパラメーターとして渡します。

```csharp
var UserDefinedColumnSdcaEstimator = mlContext.Regression.Trainers.Sdca(labelColumnName: "MyLabelColumnName", featureColumnName: "MyFeatureColumnName");
```

## <a name="train-the-machine-learning-model"></a>機械学習モデルをトレーニングする

データの前処理を完了したら、[`Fit`](xref:Microsoft.ML.Trainers.TrainerEstimatorBase`2.Fit*) メソッドを使用して、[`StochasticDualCoordinateAscent`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer) 回帰アルゴリズムで機械学習モデルをトレーニングします。

```csharp
// Define StochasticDualCoordinateAscent regression algorithm estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Build machine learning model
var trainedModel = sdcaEstimator.Fit(transformedTrainingData);
```

## <a name="extract-model-parameters"></a>モデル パラメーターを抽出する

モデルがトレーニングされた後、検査または再トレーニングのために学習済みの [`ModelParameters`](xref:Microsoft.ML.Trainers.ModelParametersBase%601) を抽出します。 [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) には、トレーニング済みモデルの偏り係数、学習済み係数、または重みが用意されています。

```csharp
var trainedModelParameters = trainedModel.Model as LinearRegressionModelParameters;
```

> [!NOTE]
> 他のモデルには、それぞれのタスクに固有のパラメーターがあります。 たとえば、[K-Means アルゴリズム](xref:Microsoft.ML.Trainers.KMeansTrainer)では重心に基づいてデータがクラスターに配置され、[`KMeansModelParameters`](xref:Microsoft.ML.Trainers.KMeansModelParameters) にはその学習済みの重心を格納するプロパティが含まれます。 詳細については、[`Microsoft.ML.Trainers` API ドキュメント](xref:Microsoft.ML.Trainers)を参照し、名前に `ModelParameters` を含むクラスを探します。

## <a name="evaluate-model-quality"></a>モデルの品質を評価する

最適なパフォーマンスを発揮するモデルを選択するには、テスト データでそのパフォーマンスを評価することが重要です。 [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*) メソッドを使用して、トレーニング済みモデルのさまざまなメトリックを測定します。

> [!NOTE]
> `Evaluate` メソッドによって生成されるメトリックは、実行された機械学習タスクによって異なります。 詳細については、[`Microsoft.ML.Data` API ドキュメント](xref:Microsoft.ML.Data)を参照し、名前に `Metrics` を含むクラスを探します。

```csharp
// Measure trained model performance
// Apply data prep transformer to test data
IDataView transformedTestData = dataPrepTransformer.Transform(testData);

// Use trained model to make inferences on test data
IDataView testDataPredictions = trainedModel.Transform(transformedTestData);

// Extract model metrics and get RSquared
RegressionMetrics trainedModelMetrics = mlContext.Regression.Evaluate(testDataPredictions);
double rSquared = trainedModelMetrics.RSquared;
```

前のコード サンプルの内容は次のとおりです。

1. テスト データ セットは、以前に定義されたデータ準備変換を使用して前処理されます。
2. トレーニング済み機械学習モデルは、テスト データに基づいて予測するために使用されます。
3. `Evaluate` メソッドでは、テスト データ セットの `CurrentPrice` 列の値を新しく出力された予測の `Score` 列と比較して、回帰モデルのメトリックを計算します。そのうちの 1 つである R-2 乗値は `rSquared` 変数に格納されます。

> [!NOTE]
> この小さな例では、データのサイズが限られているため、R-2 乗値は 0 から 1 の範囲に含まれない数値です。 実際のシナリオでは、0 から 1 の間の値が示されると想定されます。
