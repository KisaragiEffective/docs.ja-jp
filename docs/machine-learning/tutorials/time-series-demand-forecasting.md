---
title: 'チュートリアル: 自転車レンタル需要の予測 - 時系列'
description: このチュートリアルでは、一変量時系列解析と ML.NET を使用して、自転車レンタル サービスの需要を予測する方法について説明します。
ms.date: 11/07/2019
ms.topic: tutorial
ms.custom: mvc
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: 026421d7b1b2a0e39118ae712780ca7fc8f6e444
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76921252"
---
# <a name="tutorial-forecast-bike-rental-service-demand-with-time-series-analysis-and-mlnet"></a>チュートリアル: 時系列解析と ML.NET を使用して自転車レンタル サービスの需要を予測する

ML.NET を使用して SQL Server データベースに格納されているデータに対して、一変量時系列解析を使用して自転車レンタル サービスの需要を予測する方法について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * 問題を把握する
> * データベースからデータを読み込む
> * 予測モデルを作成する
> * 予測モデルを評価する
> * 予測モデルを保存する
> * 予測モデルを使用する

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

## <a name="time-series-forecasting-sample-overview"></a>時系列予測のサンプルの概要

このサンプルは、特異スペクトル解析 (Singular Spectrum Analysis) と呼ばれる一変量時系列解析アルゴリズムを使用して、自転車のレンタルの需要を予測する、**C# .NET Core コンソール アプリケーション**です。 このサンプルのコードについては、GitHub の [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand) リポジトリで見つけることができます。

## <a name="understand-the-problem"></a>問題を把握する

効率的に事業を運営するには、在庫管理が重要な役割を果たします。 製品の在庫が多すぎるということは、製品が棚に置かれたままで販売されず、何の収益も生み出さないことを意味します。 製品が少なすぎると、販売機会の損失や顧客が競合他社から購入することにつながります。 そのため、在庫を維持するための最適な量はどのくらいかということを常に問いかけます。 時系列解析では、履歴データ、パターンの識別、およびこの情報を使用して、将来の値を予測することで、この質問に対する回答を得ることができます。

このチュートリアルで使用されているデータを解析するための手法は、一変量時系列解析です。 一変量時系列解析では、月単位の売上など、特定の間隔で一定の期間にわたって 1 つの数値の観測を調べます。

このチュートリアルで使用されているアルゴリズムは、[特異スペクトル解析 (SSA: Singular Spectrum Analysis)](http://ssa.cf.ac.uk/zhigljavsky/pdfs/SSA/SSA_encyclopedia.pdf) です。 SSA は、時系列を一連のプリンシパル コンポーネントに分解することによって機能します。 これらのコンポーネントは、傾向、ノイズ、季節性、およびその他の多くの要因に対応するシグナルの部分として解釈することができます。 その後、これらのコンポーネントは再構築され、将来の値の予測に使用されます。

## <a name="create-console-application"></a>コンソール アプリケーションを作成する

1. "BikeDemandForecasting" という名前の新しい **C# .NET Core コンソール アプリケーション**を作成します。
1. **Microsoft.ML** バージョン **1.4.0** NuGet パッケージをインストールします。
    1. ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    1. [パッケージ ソース] として "nuget.org" を選択し、 **[参照]** タブを選択し、"**Microsoft.ML**" を検索します。
    1. **[プレリリースを含める]** チェックボックスをオンにします。
    1. **[インストール]** ボタンを選択します。
    1. **[変更のプレビュー]** ダイアログで **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、[ライセンスの同意] ダイアログの **[同意する]** を選択します。
    1. **System.Data.SqlClient** バージョン **4.7.0** および **Microsoft.ML.TimeSeries** バージョン **1.4.0** に対して、この手順を繰り返します。

### <a name="prepare-and-understand-the-data"></a>データを準備して理解する

1. *Data* というディレクトリを作成します。
1. [*DailyDemand.mdf* データベース ファイル](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Data/DailyDemand.mdf)をダウンロードし、*Data* ディレクトリに保存します。

> [!NOTE]
> このチュートリアルで使用されているデータは、[UCI Bike Sharing Dataset](https://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset) から取得したものです。 Fanaee-T, Hadi, and Gama, Joao, 'Event labeling combining ensemble detectors and background knowledge', Progress in Artificial Intelligence (2013): pp. 1-15, Springer Berlin Heidelberg, [Web リンク](https://link.springer.com/article/10.1007%2Fs13748-013-0040-3)。

元のデータセットには、季節性と天気に対応する複数の列が含まれています。 簡潔にするため、またこのチュートリアルで使用されているアルゴリズムには 1 つの数値列の値しか必要ないため、元のデータセットは次の列のみが含まれるように圧縮されています。

- **dteday**:観察の日付。
- **year**:観察のエンコードされた年 (0 = 2011、1 = 2012)。
- **cnt**:その日にレンタルされた自転車の合計数。

元のデータセットは、SQL Server データベースの次のスキーマを持つデータベース テーブルにマップされます。

```SQL
CREATE TABLE [Rentals] (
    [RentalDate] DATE NOT NULL,
    [Year] INT NOT NULL,
    [TotalRentals] INT NOT NULL
);
```

データのサンプルを次に示します。

| RentalDate | Year | TotalRentals |
| --- | --- | --- |
|1/1/2011|0|985|
|1/2/2011|0|801|
|1/3/2011|0|1349|

### <a name="create-input-and-output-classes"></a>入力クラスと出力クラスを作成する

1. *Program.cs* ファイルを開き、既存の `using` ステートメントを次のステートメントに置き換えます。

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L1-L8)]

1. `ModelInput` クラスを作成します。 `Program` クラスの下に、次のコードを追加します。

    [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L120-L127)]

    `ModelInput` クラスには、次の列が含まれています。

    - **RentalDate**:観察の日付。
    - **Year**:観察のエンコードされた年 (0 = 2011、1 = 2012)。
    - **TotalRentals**:その日にレンタルされた自転車の合計数。

1. 新しく作成された `ModelInput` クラスの下に `ModelOutput` クラスを作成します。

    [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L129-L136)]

    `ModelOutput` クラスには、次の列が含まれています。

    - **ForecastedRentals**:予測期間の予測値。
    - **LowerBoundRentals**:予測期間の予測される最小値。
    - **UpperBoundRentals**:予測期間の予測される最大値。

### <a name="define-paths-and-initialize-variables"></a>パスを定義し、変数を初期化する

1. `Main` メソッド内で、データの場所、接続文字列、およびトレーニング済みのモデルを保存する場所を格納する変数を定義します。

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L16-L19)]

1. `Main` メソッドに次の行を追加して、[`MLContext`](xref:Microsoft.ML.MLContext) の新しいインスタンスで `mlContext` 変数を初期化します。

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L21)]

    [`MLContext`](xref:Microsoft.ML.MLContext) クラスは、すべての ML.NET 操作の開始点で、mlContext を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

## <a name="load-the-data"></a>データを読み込む

1. `ModelInput` 型のレコードを読み込む `DatabaseLoader` を作成します。

    [!code-csharp [CreateDBLoader](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L23)]

1. データベースからデータを読み込むクエリを定義します。

    [!code-csharp [DefineSQLQuery](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L25)]

    ML.NET アルゴリズムでは、データが [`Single`](xref:System.Single) 型であることが想定されています。 このため、データベースから取得された、[`Real`](xref:System.Data.SqlDbType) 型の単精度浮動小数点値ではない数値は [`Real`](xref:System.Data.SqlDbType) に変換される必要があります。

    `Year` 列と `TotalRental` 列はどちらもデータベース内では整数型です。 `CAST` 組み込み関数を使用すると、どちらも `Real` にキャストされます。

1. データベースに接続してクエリを実行する `DatabaseSource` を作成します。

    [!code-csharp [CreateDBSource](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L27-L29)]

1. `IDataView` にデータを読み込みます。

    [!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L31)]

1. このデータセットには 2 年分のデータが含まれています。 トレーニングに使用されるのは 1 年目のデータのみで、2 年目のデータは、モデルによって生成された予測と実際の値を比較するために保持されます。 [`FilterRowsByColumn`](xref:Microsoft.ML.DataOperationsCatalog.FilterRowsByColumn*) 変換を使用してデータをフィルター処理します。

    [!code-csharp [SplitData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L33-L34)]

    1 年目に対しては、`upperBound` パラメーターを 1 に設定することによって、`Year` 列の 1 未満の値のみが選択されます。 反対に、2 年目に対しては、`lowerBound` パラメーターを 1 に設定することによって、1 以上の値が選択されます。

## <a name="define-time-series-analysis-pipeline"></a>時系列解析パイプラインを定義する

1. [SsaForecastingEstimator](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator) を使用して時系列データセット内の値を予測するパイプラインを定義します。

    [!code-csharp [DefinePipeline](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L36-L45)]

    `forecastingPipeline` では、1 年目用の 365 のデータ ポイントとサンプルを取得するか、`seriesLength` パラメーターで指定された 30 日 (月単位) 間隔に時系列データセットを分割します。 これらの各サンプルは、毎週または 7 日間の期間で解析されます。 次の期間の予測値がどのようになるかを判断する際には、前の 7 日間の値を使用して予測が行われます。 モデルは、`horizon` パラメーターで定義されているように、7 つの期間を将来まで予測するように設定されています。 予測は情報に基づいた推測であるため、100% 正確であるとは限りません。 したがって、上限と下限によって定義される最善のシナリオと最悪のシナリオにおける値の範囲を把握しておくことをお勧めします。 この場合、下限と上限の信頼レベルは 95% に設定されています。 信頼レベルは、状況に応じて増減できます。 値が大きいほど、望ましい信頼レベルを達成するために上限と下限の範囲が広くなります。

1. [`Fit`](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator.Fit*) メソッドを使用して、モデルをトレーニングし、以前に定義した `forecastingPipeline` にデータを適合させます。

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L47)]

## <a name="evaluate-the-model"></a>モデルを評価する

来年のデータを予測し、それを実際の値と比較することによって、モデルのパフォーマンスを評価します。

1. `Main` メソッドの下に、`Evaluate` という新しいユーティリティ メソッドを作成します。

    ```csharp
    static void Evaluate(IDataView testData, ITransformer model, MLContext mlContext)
    {

    }
    ```

1. `Evaluate` メソッド内で、トレーニング済みのモデルで [`Transform`](xref:Microsoft.ML.ITransformer.Transform*) メソッドを使用して、2 年目のデータを予測します。

    [!code-csharp [EvaluateForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L62)]

1. [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) メソッドを使用して、データから実際の値を取得します。

    [!code-csharp [GetActualRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L65-L67)]

1. [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) メソッドを使用して、予測値を取得します。

    [!code-csharp [GetForecastRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L70-L72)]

1. 実際の値と予測値の差を計算します (一般的にエラーと呼ばれます)。

    [!code-csharp [CalculateError](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L75)]

1. 平均絶対誤差値と二乗平均平方根誤差値を計算して、パフォーマンスを測定します。

    [!code-csharp [CalculateMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L78-L79)]

    パフォーマンスを評価するため、次のメトリックが使用されます。

    - **平均絶対誤差**:予測が実際の値にどれだけ近いかを測定します。 この値の範囲は 0 から無限大です。 0 に近いほど、モデルの品質が高くなります。
    - **二乗平均平方根誤差**:モデル内のエラーを集約します。 この値の範囲は 0 から無限大です。 0 に近いほど、モデルの品質が高くなります。

1. メトリックをコンソールに出力します。

    [!code-csharp [OutputMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L82-L85)]

1. `Main` メソッド内で `Evaluate` メソッドを使用します。

    [!code-csharp [EvaluateModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L49)]

## <a name="save-the-model"></a>モデルを保存する

モデルに問題がなければ、後で他のアプリケーションで使用できるように保存します。

1. `Main` メソッドで、[`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) を作成します。 [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) は、単一の予測を行うための便利な方法です。

    [!code-csharp [CreateTimeSeriesEngine](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L51)]

1. 以前に定義した `modelPath` 変数によって指定された `MLModel.zip` という名前のファイルにモデルを保存します。 [`Checkpoint`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.CheckPoint*) メソッドを使用してモデルを保存します。

    [!code-csharp [SaveModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L52)]

## <a name="use-the-model-to-forecast-demand"></a>モデルを使用して需要を予測する

1. `Evaluate` メソッドの下に、`Forecast` という新しいユーティリティ メソッドを作成します。

    ```csharp
    static void Forecast(IDataView testData, int horizon, TimeSeriesPredictionEngine<ModelInput, ModelOutput> forecaster, MLContext mlContext)
    {

    }
    ```

1. `Forecast` メソッド内で、[`Predict`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.Predict*) メソッドを使用して、次の 7 日間のレンタルを予測します。

    [!code-csharp [SingleForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L91)]

1. 7 つの期間の実際の値と予測値を合わせます。

    [!code-csharp [GetForecastOutput](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L93-L108)]

1. 予測出力を反復処理し、コンソールに表示します。

    [!code-csharp [DisplayForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L111-L116)]

## <a name="run-the-application"></a>アプリケーションの実行

1. `Main` メソッド内で、`Forecast` メソッドを呼び出します。

    [!code-csharp [BuildForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L54)]

1. アプリケーションを実行します。 次のような出力がコンソールに表示されます。 簡潔にするため、出力は要約されています。

    ```text
    Evaluation Metrics
    ---------------------
    Mean Absolute Error: 726.416
    Root Mean Squared Error: 987.658

    Rental Forecast
    ---------------------
    Date: 1/1/2012
    Actual Rentals: 2294
    Lower Estimate: 1197.842
    Forecast: 2334.443
    Upper Estimate: 3471.044

    Date: 1/2/2012
    Actual Rentals: 1951
    Lower Estimate: 1148.412
    Forecast: 2360.861
    Upper Estimate: 3573.309
    ```

実際の値と予測値を検査すると、次のリレーションシップが表示されます。

![実際の値と予測値の比較](./media/time-series-demand-forecasting/forecast.png)

予測値は、レンタルの正確な数を予測するものではありませんが、より絞り込んだ値の範囲が提供されるため、リソースの使用を最適化した事業が可能になります。

おめでとうございます! これで、自転車のレンタル需要を予測するための時系列の機械学習モデルが正常に作成されました。

このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/Forecasting_BikeSharingDemand) リポジトリにあります。

## <a name="next-steps"></a>次の手順

- [ML.NET での機械学習のタスク](../resources/tasks.md)
- [モデルの正確度を改善する](../resources/improve-machine-learning-model-ml-net.md)
