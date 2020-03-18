---
title: Permutation Feature Importance を使用して ML.NET モデルを解釈する
description: ML.NET の Permutation Feature Importance を使ってモデルの特徴の重要度を理解する
ms.date: 01/30/2020
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to
ms.openlocfilehash: c1163a41cd2feb0e8785ae9d4c6a71dfbedf3f12
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77092617"
---
# <a name="interpret-model-predictions-using-permutation-feature-importance"></a>Permutation Feature Importance を使用してモデル予測を解釈する

Permutation Feature Importance (PFI) を使用して、ML.NET 機械学習モデルの予測を解釈する方法について学習します。 PFI によって、各特徴がもたらす予測への相対的な貢献度が示されます。

機械学習モデルは、入力を受け取り、出力を生成するブラック ボックスと考えられることがよくあります。 出力に影響する中間手順や特徴間の相互作用は、あまり理解されていません。 医療など、日常生活の多くの側面に機械学習が導入されるにつれて、機械学習モデルが決定を下す理由を理解することの重要度が最も高くなりました。 たとえば、機械学習モデルによって診断が行われる場合、医療従事者には、その診断が下された要因を調べる方法が必要です。 適切な診断を提供することは、患者が迅速に回復するかどうかに大きな違いをもたらす可能性があります。 そのため、モデル内の説明可能性のレベルが高いほど、モデルによって行われた決定を医療従事者が承認または拒否する必要がある場合の信頼性が高くなります。

モデルの説明にはさまざまな手法が使用されており、その 1 つに PFI があります。 PFI は、[Breiman の「*Random Forests*」(ランダム フォレスト) 論文](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf) (セクション 10 を参照してください) にインスパイアされた、分類および回帰モデルの説明に使用される手法です。 概要を説明すると、データセット全体に対して一度に 1 つの特徴のデータをランダムにシャッフルし、関心のあるパフォーマンス メトリックがどのくらい低下するかを計算するしくみです。 変更が大きいほど、その特徴の重要度は高くなります。

さらに、最も重要な特徴を強調することで、モデル作成者はより重要な特徴のサブセットの使用に集中し、ノイズとトレーニング時間を減らすことができるようになります。

## <a name="load-the-data"></a>データを読み込む

このサンプルに使用されているデータセットの特徴は、1 - 12 列目にあります。 目標は `Price` を予測することです。

| 列 | 機能 | [説明]
| --- | --- | --- |
| 1 | CrimeRate | 1 人当たりの犯罪率
| 2 | ResidentialZones | 町の住宅地区
| 3 | CommercialZones | 町の非住宅地区
| 4 | NearWater | 水域への近さ
| 5 | ToxicWasteLevels | 毒性レベル (PPM)
| 6 | AverageRoomNumber | 住宅内の平均部屋数
| 7 | HomeAge | 住宅の築年数
| 8 | BusinessCenterDistance | 最寄りのビジネス地区までの距離
| 9 | HighwayAccess | 幹線道路までの近さ
| 10 | TaxRate | 固定資産税率
| 11 | StudentTeacherRatio | 教師に対する学生の比率
| 12 | PercentPopulationBelowPoverty | 貧困を下回る生活を送っている人口の割合
| 13 | Price | 住宅の価格

データセットの例を以下に示します。

```text
1,24,13,1,0.59,3,96,11,23,608,14,13,32
4,80,18,1,0.37,5,14,7,4,346,19,13,41
2,98,16,1,0.25,10,5,1,8,689,13,36,12
```

このサンプルのデータは、`HousingPriceData` のようなクラスでモデル化し、[`IDataView`](xref:Microsoft.ML.IDataView) にロードできます。

```csharp
class HousingPriceData
{
    [LoadColumn(0)]
    public float CrimeRate { get; set; }

    [LoadColumn(1)]
    public float ResidentialZones { get; set; }

    [LoadColumn(2)]
    public float CommercialZones { get; set; }

    [LoadColumn(3)]
    public float NearWater { get; set; }

    [LoadColumn(4)]
    public float ToxicWasteLevels { get; set; }

    [LoadColumn(5)]
    public float AverageRoomNumber { get; set; }

    [LoadColumn(6)]
    public float HomeAge { get; set; }

    [LoadColumn(7)]
    public float BusinessCenterDistance { get; set; }

    [LoadColumn(8)]
    public float HighwayAccess { get; set; }

    [LoadColumn(9)]
    public float TaxRate { get; set; }

    [LoadColumn(10)]
    public float StudentTeacherRatio { get; set; }

    [LoadColumn(11)]
    public float PercentPopulationBelowPoverty { get; set; }

    [LoadColumn(12)]
    [ColumnName("Label")]
    public float Price { get; set; }
}
```

## <a name="train-the-model"></a>モデルをトレーニングする

次のコード サンプルは、線形回帰モデルをトレーニングして住宅価格を予測するプロセスを示しています。

```csharp
// 1. Get the column name of input features.
string[] featureColumnNames =
    data.Schema
        .Select(column => column.Name)
        .Where(columnName => columnName != "Label").ToArray();

// 2. Define estimator with data pre-processing steps
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", featureColumnNames)
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// 3. Create transformer using the data pre-processing estimator
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// 4. Pre-process the training data
IDataView preprocessedTrainData = dataPrepTransformer.Transform(data);

// 5. Define Stochastic Dual Coordinate Ascent machine learning estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// 6. Train machine learning model
var sdcaModel = sdcaEstimator.Fit(preprocessedTrainData);
```

## <a name="explain-the-model-with-permutation-feature-importance-pfi"></a>Permutation Feature Importance (PFI) を使用してモデルを説明する

ML.NET では、それぞれのタスクに [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) メソッドを使用します。

```csharp
ImmutableArray<RegressionMetricsStatistics> permutationFeatureImportance =
    mlContext
        .Regression
        .PermutationFeatureImportance(sdcaModel, preprocessedTrainData, permutationCount:3);
```

トレーニング データセットに対して [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) を使用した結果は、[`ImmutableArray`](xref:System.Collections.Immutable.ImmutableArray) オブジェクトの [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics) になります。 [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics) は、[ パラメーターに指定された順列の数と等しい `RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics)`permutationCount`の複数の観測値について、平均や標準偏差などの概要の統計情報を提供します。

重要度、このケースで言い換えると、[`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) によって計算される R-2 乗メトリックの絶対平均減少は、最も高い重要度から最も低い重要度の順に並べ替えることができます。

```csharp
// Order features by importance
var featureImportanceMetrics =
    permutationFeatureImportance
        .Select((metric, index) => new { index, metric.RSquared })
        .OrderByDescending(myFeatures => Math.Abs(myFeatures.RSquared.Mean));

Console.WriteLine("Feature\tPFI");

foreach (var feature in featureImportanceMetrics)
{
    Console.WriteLine($"{featureColumnNames[feature.index],-20}|\t{feature.RSquared.Mean:F6}");
}
```

`featureImportanceMetrics` の各特徴の値を出力すると、以下のような出力が生成されます。 これらの値は指定されたデータに基づいて変わるため、異なる結果が表示されることを想定する点に注意してください。

| 機能 | R-2 乗に変更 |
|:--|:--:|
HighwayAccess       |   -0.042731
StudentTeacherRatio |   -0.012730
BusinessCenterDistance| -0.010491
TaxRate             |   -0.008545
AverageRoomNumber   |   -0.003949
CrimeRate           |   -0.003665
CommercialZones     |   0.002749
HomeAge             |   -0.002426
ResidentialZones    |   -0.002319
NearWater           |   0.000203
PercentPopulationLivingBelowPoverty|    0.000031
ToxicWasteLevels    |   -0.000019

このデータセットの重要度が高い特徴の上位 5 つを見ると、このモデルによって予測される住宅の価格は、幹線道路までの近さ、その地域の学校の学生と教師の比率、主要な雇用中心地への近さ、固定資産税率、住宅内の平均部屋数の影響を受けます。
