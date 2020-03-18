---
title: クロス検証を使って機械学習モデルをトレーニングする
description: ML.NET で、クロス検証を使用してより堅牢な機械学習モデルを構築する方法について説明します。 クロス検証は、データをいくつかのパーティションに分割し、それらのパーティション上で複数のアルゴリズムをトレーニングするトレーニングおよびモデル評価手法です。
ms.date: 08/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to,title-hack-0625
ms.openlocfilehash: 87eae789478752423f3e682d4db6cead0391aa6e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73976925"
---
# <a name="train-a-machine-learning-model-using-cross-validation"></a>クロス検証を使って機械学習モデルをトレーニングする

ML.NET で、クロス検証を使用してより堅牢な機械学習モデルをトレーニングする方法について説明します。

クロス検証は、データをいくつかのパーティションに分割し、それらのパーティション上で複数のアルゴリズムをトレーニングするトレーニングおよびモデル評価手法です。 この手法は、トレーニング プロセスのデータを提供することでモデルの堅牢性を改善します。 データに制約のある環境では、目に見えない観測のパフォーマンス向上に加え、小規模のデータセットでモデルをトレーニングする場合の効果的なツールになる可能性があります。

## <a name="the-data-and-data-model"></a>データとデータ モデル

次の形式を持つファイルのデータがあるとします。

```text
Size (Sq. ft.), HistoricalPrice1 ($), HistoricalPrice2 ($), HistoricalPrice3 ($), Current Price ($)
620.00, 148330.32, 140913.81, 136686.39, 146105.37
550.00, 557033.46, 529181.78, 513306.33, 548677.95
1127.00, 479320.99, 455354.94, 441694.30, 472131.18
1120.00, 47504.98, 45129.73, 43775.84, 46792.41
```

データは `HousingData` のようなクラスでモデル化し、[`IDataView`](xref:Microsoft.ML.IDataView) にロードできます。

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

## <a name="prepare-the-data"></a>データを準備する

機械学習モデルの構築に使用する前に、データを前処理します。 このサンプルでは、`Size` 列と `HistoricalPrices` 列が 1 つの特徴ベクターに結合されます。これは、`Features`[`Concatenate` メソッドを使用して ](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate*) という新しい列に出力されます。 データを ML.NET アルゴリズムで想定されている形式にすることに加え、列を連結すると、個別の列ではなく、連結した列に対して 1 回操作を適用されるので、パイプライン内の後続の操作が最適化されます。

列が 1 つのベクターに結合されると、[`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax*) が `Features` 列に適用され、0 - 1 という同じ範囲の `Size` と `HistoricalPrices` が得られます。

```csharp
// Define data prep estimator
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data prep transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// Transform data
IDataView transformedData = dataPrepTransformer.Transform(data);
```

## <a name="train-model-with-cross-validation"></a>クロス検証を使用してモデルをトレーニングする

データの前処理が完了したら、次はモデルのトレーニングです。 まず、実行する機械学習タスクと最も密接に連携するアルゴリズムを選択します。 予測値は数値的に連続した値なので、このタスクは回帰です。 ML.NET で実装されている回帰アルゴリズムの 1 つは [`StochasticDualCoordinateAscentCoordinator`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer) アルゴリズムです。 クロス検証を使用してモデルをトレーニングするには、[`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate*) メソッドを使用します。

> [!NOTE]
> このサンプルでは線形回帰モデルを使用しますが、CrossValidate は、異常検出を除く ML.NET の他のすべての機械学習タスクに適用できます。

```csharp
// Define StochasticDualCoordinateAscent algorithm estimator
IEstimator<ITransformer> sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Apply 5-fold cross validation
var cvResults = mlContext.Regression.CrossValidate(transformedData, sdcaEstimator, numberOfFolds: 5);
```

[`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate*) によって次の操作が実行されます。

1. `numberOfFolds` パラメーターに指定された値と等しい数のパーティションにデータを分割します。 各パーティションの結果は [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) オブジェクトです。
1. 各パーティションでは、トレーニング データ セットに対して指定した機械学習アルゴリズム エスティメーターを使用してモデルがトレーニングされます。
1. 各モデルのパフォーマンスは、テスト データ セットに対して [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate*) メソッドを使用して評価されます。
1. 各モデルについて、モデルとそのメトリックが返されます。

`cvResults` に格納される結果は、[`CrossValidationResult`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601) オブジェクトのコレクションです。 このオブジェクトには、トレーニング済みモデルだけでなく、[`Model`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Model) プロパティと [`Metrics`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Metrics) プロパティの両方からそれぞれアクセスできるメトリックが含まれています。 このサンプルでは、`Model` プロパティは [`ITransformer`](xref:Microsoft.ML.ITransformer) 型であり、`Metrics` プロパティは [`RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics) 型です。

## <a name="evaluate-the-model"></a>モデルを評価する

個々の `Metrics`[`CrossValidationResult` オブジェクトの ](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601) プロパティを介してさまざまなトレーニング済みモデルのメトリックにアクセスできます。 この場合は、[R-2 乗メトリック](https://en.wikipedia.org/wiki/Coefficient_of_determination)にアクセスし、変数 `rSquared` に格納されます。

```csharp
IEnumerable<double> rSquared =
    cvResults
        .Select(fold => fold.Metrics.RSquared);
```

`rSquared` 変数の内容を調べると、出力は 0 - 1 の範囲の 5 つの値になります (1 に近いほど適しています)。 R-2 乗などのメトリックを使用して、最適なパフォーマンスから最低のパフォーマンスのモデルを選択します。 次に、最上位のモデルを選択して予測を行うか、追加の操作を実行します。

```csharp
// Select all models
ITransformer[] models =
    cvResults
        .OrderByDescending(fold => fold.Metrics.RSquared)
        .Select(fold => fold.Model)
        .ToArray();

// Get Top Model
ITransformer topModel = models[0];
```
