---
title: トレーニング済みモデルの保存と読み込み
description: トレーニング済みモデルの保存と読み込みの方法について説明します
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: e3cebe979b5c279ce8cb90db5510f8758c24c2b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73977005"
---
# <a name="save-and-load-trained-models"></a>トレーニング済みモデルの保存と読み込み

アプリケーションでのトレーニング済みモデルの保存と読み込みの方法について説明します。

モデルは、モデル構築プロセス全体を通してメモリ内に存在し、アプリケーションのライフサイクル全体を通してアクセス可能です。 ただし、アプリケーションの実行が停止した後は、モデルがローカルまたはリモートのどこかに保存されていないと、アクセスできなくなります。 通常、モデルは、トレーニング後のどこかの時点で推論または再トレーニングのために他のアプリケーションで使用されます。 そのため、モデルを保存することが重要です。 後述するようにデータ準備とモデル トレーニング パイプラインを使用する場合は、このドキュメントの以降のセクションで説明する手順を使用してモデルを保存し、読み込みます。 このサンプルでは線形回帰モデルを使用しますが、他の ML.NET アルゴリズムにも同じプロセスが適用されます。

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f, 125000f, 122000f },
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
    }
};

// Create MLContext
MLContext mlContext = new MLContext();

// Load Data
IDataView data = mlContext.Data.LoadFromEnumerable<HousingData>(housingData);

// Define data preparation estimator
EstimatorChain<RegressionPredictionTransformer<LinearRegressionModelParameters>> pipelineEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"))
        .Append(mlContext.Regression.Trainers.Sdca());

// Train model
ITransformer trainedModel = pipelineEstimator.Fit(data);

// Save model
mlContext.Model.Save(trainedModel, data.Schema, "model.zip");
```

ほとんどのモデルとデータ準備パイプラインは同じクラスのセットから継承するため、このようなコンポーネントの save メソッドと load メソッドのシグネチャは同じです。 実際のユース ケースに応じて、データ準備パイプラインとモデルを 1 つの [`EstimatorChain`](xref:Microsoft.ML.Data.TransformerChain%601) に結合して 1 つの [`ITransformer`](xref:Microsoft.ML.ITransformer) を出力するか、それぞれを分けて個別の [`ITransformer`](xref:Microsoft.ML.ITransformer) を作成します。

## <a name="save-a-model-locally"></a>モデルをローカルに保存する

モデルを保存するときには、以下の 2 つが必要です。

1. モデルの [`ITransformer`](xref:Microsoft.ML.ITransformer)。
2. [`DataViewSchema`](xref:Microsoft.ML.DataViewSchema) の想定される入力の [`ITransformer`](xref:Microsoft.ML.ITransformer)。

モデルのトレーニング後、[`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save*) メソッドを使用し、入力データの `model.zip` を使用してトレーニング済みモデルを `DataViewSchema` というファイルに保存します。

```csharp
// Save Trained Model
mlContext.Model.Save(trainedModel, data.Schema, "model.zip");
```

## <a name="load-a-model-stored-locally"></a>ローカルに保存されているモデルを読み込む

ローカルに保存されたモデルは、他のプロセスやアプリケーション (`ASP.NET Core`、`Serverless Web Applications` など) で使用できます。 詳細については、[Web API での ML.NET の使用](./serve-model-web-api-ml-net.md)と [ML.NET サーバーレス Web アプリの展開](./serve-model-serverless-azure-functions-ml-net.md)に関するハウツー記事を参照してください。

個別のアプリケーションまたはプロセスでは、ファイル パスと共に [`Load`](xref:Microsoft.ML.ModelOperationsCatalog.Load*) メソッドを使用して、トレーニング済みモデルをアプリケーションに取り込みます。

```csharp
//Define DataViewSchema for data preparation pipeline and trained model
DataViewSchema modelSchema;

// Load trained model
ITransformer trainedModel = mlContext.Model.Load("model.zip", out modelSchema);
```

## <a name="load-a-model-stored-remotely"></a>リモートに保存されているモデルを読み込む

リモートの場所に保存されているデータ準備パイプラインとモデルをアプリケーションに読み込むには、[`Stream`](xref:System.IO.Stream) メソッドでファイル パスではなく [`Load`](xref:Microsoft.ML.ModelOperationsCatalog.Load*) を使用します。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define DataViewSchema and ITransformers
DataViewSchema modelSchema;
ITransformer trainedModel;

// Load data prep pipeline and trained model
using (HttpClient client = new HttpClient())
{
    Stream modelFile = await client.GetStreamAsync("<YOUR-REMOTE-FILE-LOCATION>");

    trainedModel = mlContext.Model.Load(modelFile, out modelSchema);
}
```

## <a name="working-with-separate-data-preparation-and-model-pipelines"></a>個別のデータ準備パイプラインとモデル パイプラインを使用する

> [!NOTE]
> 個別のデータ準備パイプラインとモデル トレーニング パイプラインの使用は任意です。 パイプラインを分けると、学習したモデルのパラメーターを簡単に調べることができます。 予測のためには、データ準備とモデル トレーニング操作を含む 1 つのパイプラインを保存して読み込む方が簡単です。

個別のデータ準備パイプラインとモデルを使用するときは、1 つのパイプラインと同じプロセスが適用されます。ただし、この場合は両方のパイプラインを同時に保存して読み込む必要があります。

個別のデータ準備パイプラインとモデル トレーニング パイプラインの例を次に示します。

```csharp
// Define data preparation estimator
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data preparation transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// Define StochasticDualCoordinateAscent regression algorithm estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Pre-process data using data prep operations
IDataView transformedData = dataPrepTransformer.Transform(data);

// Train regression model
RegressionPredictionTransformer<LinearRegressionModelParameters> trainedModel = sdcaEstimator.Fit(transformedData);
```

### <a name="save-data-preparation-pipeline-and-trained-model"></a>データ準備パイプラインとトレーニング済みモデルを保存する

データ準備パイプラインとトレーニング済みモデルの両方を保存するには、次のコマンドを使用します。

```csharp
// Save Data Prep transformer
mlContext.Model.Save(dataPrepTransformer, data.Schema, "data_preparation_pipeline.zip");

// Save Trained Model
mlContext.Model.Save(trainedModel, transformedData.Schema, "model.zip");
```

### <a name="load-data-preparation-pipeline-and-trained-model"></a>データ準備パイプラインとトレーニング済みモデルを読み込む

個別のプロセスまたはアプリケーションで、次のようにデータ準備パイプラインとトレーニング済みモデルを同時に読み込みます。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define data preparation and trained model schemas
DataViewSchema dataPrepPipelineSchema, modelSchema;

// Load data preparation pipeline and trained model
ITransformer dataPrepPipeline = mlContext.Model.Load("data_preparation_pipeline.zip",out dataPrepPipelineSchema);
ITransformer trainedModel = mlContext.Model.Load("model.zip", out modelSchema);
```
