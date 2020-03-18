---
title: 'チュートリアル: 回帰を使用して価格を予測する'
description: このチュートリアルでは、ML.NET を使用して、料金 (具体的にはニューヨーク市のタクシー運賃) を予測する回帰モデルを構築する方法を示します。
ms.date: 09/30/2019
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: 51cef97178c2dbc6a5b572a7045bdad4bc382ba0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78240988"
---
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a>チュートリアル: ML.NET で回帰を使用して価格を予測する

このチュートリアルでは、ML.NET を使用して、料金 (具体的にはニューヨーク市のタクシー運賃) を予測する[回帰モデル](../resources/glossary.md#regression)を構築する方法を示します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * データを準備して理解する
> * データを読み込んで変換する
> * 学習アルゴリズムを選択する
> * モデルをトレーニングする
> * モデルを評価する
> * モデルを使用して予測を行う

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. "TaxiFarePrediction" という名前の **.NET Core コンソール アプリケーション**を作成します。

1. プロジェクトに *Data* という名前のディレクトリを作成して、データ セットとモデルのファイルを保存します。

1. **Microsoft.ML** NuGet パッケージをインストールします。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。 **[参照]** タブを選択し、「**Microsoft.ML**」を検索します。一覧からそのパッケージを選択し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。 **Microsoft.ML.FastTree** NuGet パッケージに対して同じことを行います。

## <a name="prepare-and-understand-the-data"></a>データを準備して理解する

1. [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-train.csv) データ セットと [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-test.csv) データ セットをダウンロードして、前の手順で作成済みの *Data* フォルダーに保存します。 これらのデータ セットを使用して、機械学習モデルをトレーニングし、モデルの正確度を評価します。 これらのデータ セットは、[NYC TLC Taxi Trip データ セット](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)から取得したものです。

1. **ソリューション エクスプローラー**で、各 \*.csv ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

1. **taxi-fare-train.csv** データ セットを開き、最初の行の列ヘッダーを確認します。 各列を確認してください。 データについて把握し、どの列が**特徴**で、どの列が**ラベル**であるかを確認します。

`label` は予測する列です。 識別された `Features` は、`Label` を予測するためにモデルを指定する入力です。

提供されているデータ セットには、次の列が含まれます。

* **vendor_id:** タクシー会社の ID は特徴です。
* **rate_code:** タクシー旅行のレートの種類は特徴です。
* **passenger_count:** 旅行の乗客数は特徴です。
* **trip_time_in_secs:** 旅行の所要時間です。 旅行が終わる前に、旅行の運賃を予測したいと考えます。 その時点では、旅行の所要時間はわかりません。 したがって、旅行の所要時間は特徴ではなく、この列はモデルから除外します。
* **trip_distance:** 旅行距離は特徴です。
* **payment_type:** 支払い方法 (現金またはクレジット カード) は特徴です。
* **fare_amount:** 支払った合計タクシー運賃はラベルです。

## <a name="create-data-classes"></a>データ クラスを作成する

入力データと予測のためのクラスを作成します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを *TaxiTrip.cs* に変更します。 次に **[追加]** を選択します。
1. 以下の `using` ディレクティブを新しいファイルに追加します。

   [!code-csharp[AddUsings](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/TaxiTrip.cs#1 "Add necessary usings")]

既存のクラス定義を削除し、2 つのクラス `TaxiTrip` と `TaxiTripFarePrediction` を含む次のコードを *TaxiTrip.cs* ファイルに追加します。

[!code-csharp[DefineTaxiTrip](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

`TaxiTrip` は入力データ クラスであり、各データ セット列の定義が含まれています。 <xref:Microsoft.ML.Data.LoadColumnAttribute> 属性を使用して、データ セット内のソース列のインデックスを指定します。

`TaxiTripFarePrediction` クラスは予測結果を表します。 このクラスには、`Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> 属性が適用された 1 つの浮動小数点型フィールド `FareAmount` が含まれています。 回帰タスクの場合、**Score** 列には、予測ラベル値が含まれます。

> [!NOTE]
> 入力データと予測データのクラス内の浮動小数点値を表すには、`float` 型を使います。

### <a name="define-data-and-model-paths"></a>データおよびモデルのパスを定義する

次に示す追加の `using` ステートメントを *Program.cs* ファイルの先頭に追加します。

[!code-csharp[AddUsings](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#1 "Add necessary usings")]

データ セットを含むファイルのパスとモデルを保存するファイルのパスを保持するための 3 つのフィールドを追加する必要があります。

* `_trainDataPath` には、モデルのトレーニングに使用するデータ セットがあるファイルへのパスが含まれます。
* `_testDataPath` には、モデルの評価に使用するデータ セットがあるファイルへのパスが含まれます。
* `_modelPath` には、トレーニング済みモデルが格納されるファイルへのパスが含まれます。

`Main` メソッドのすぐ上に次のコードを追加して、それらのパスと `_textLoader` 変数を指定します。

[!code-csharp[InitializePaths](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#2 "Define variables to store the data file paths")]

すべての ML.NET 操作は、[MLContext クラス](xref:Microsoft.ML.MLContext)で始まります。 `mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

### <a name="initialize-variables-in-main"></a>Main で変数を初期化する

`Main` メソッド内の `Console.WriteLine("Hello World!")` の行は、`mlContext` 変数を宣言して初期化する次のコードに置き換えます。

[!code-csharp[CreateMLContext](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#3 "Create the ML Context")]

`Main` メソッドに次のコード行を追加して、`Train` メソッドを呼び出します。

[!code-csharp[Train](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#5 "Train your model")]

`Train()` メソッドは次のタスクを実行します。

* データを読み込みます。
* データを抽出して変換します。
* モデルをトレーニングする。
* モデルを返します。

`Train` メソッドは、モデルをトレーニングします。 次のコードを使用して、`Main` の直下にそのメソッドを作成します。

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a>データを読み込んで変換する

ML.NET では、数値またはテキストの表形式データを記述する柔軟で効率的な方法として、[IDataView クラス](xref:Microsoft.ML.IDataView)を使います。 `IDataView` は、テキスト ファイルを読み込むか、またはリアルタイムで読み込むことができます (たとえば、SQL データベースまたはログ ファイル)。 `Train()` メソッドの最初の行として、次のコードを追加します。

[!code-csharp[LoadTrainData](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#6 "loading training dataset")]

タクシー運賃を予測したいので、`FareAmount` 列は、予測する `Label` です (モデルの出力)。 `CopyColumnsEstimator` 変換クラスを使って `FareAmount` をコピーし、次のコードを追加します。

[!code-csharp[CopyColumnsEstimator](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#7 "Use the CopyColumnsEstimator")]

モデルをトレーニングするアルゴリズムには、**数値**の特徴が必要であるため、カテゴリ データ (`VendorId`、`RateCode`、および `PaymentType`) の値を数値 (`VendorIdEncoded`、`RateCodeEncoded`、および`PaymentTypeEncoded`) に変換する必要があります。 それを行うには、異なる数値キーの値を各列内の異なる値に割り当てる [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer) 変換クラスを使用し、次のコードを追加します。

[!code-csharp[OneHotEncodingEstimator](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#8 "Use the OneHotEncodingEstimator")]

データの準備の最後の手順では、`mlContext.Transforms.Concatenate` 変換クラスを使用して、すべての特徴列を **Features** 列に結合します。 既定では、学習アルゴリズムは **Features** 列の特徴のみを処理します。 次のコードを追加します。

[!code-csharp[ColumnConcatenatingEstimator](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a>学習アルゴリズムを選択する

この問題の中心となるのは、ニューヨーク市のタクシー運賃の予測です。 一見すると、単に乗車距離に依存すると思われるかもしれません。 しかし、ニューヨークのタクシー会社は追加の乗客数や現金でなくクレジット カードによる支払いなど、その他の要因によって請求額を変えます。 データ セット内の他の要因に基づき、実際の値である価格値を予測します。 それを行うには、[回帰](../resources/glossary.md#regression)機械学習タスクを選択します。

`Train()` 内に次のコード行として以下を追加することで、データ変換定義に [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) 機械学習タスクを追加します。

[!code-csharp[FastTreeRegressionTrainer](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a>モデルをトレーニングする

`Train()` メソッドに次のコード行を追加して、モデルをトレーニング `dataview` に適合させ、トレーニング済みモデルを返します。

[!code-csharp[TrainModel](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#11 "Train the model")]

[Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) メソッドでは、データセットを変換して、トレーニングを適用することにより、モデルがトレーニングされます。

`Train()` メソッドで次のコード行を使用して、トレーニング済みモデルを返します。

[!code-csharp[ReturnModel](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a>モデルを評価する

次に、品質保証と検証のためのテスト データを使って、モデルのパフォーマンスを評価します。 次のコードを利用して、`Train()` の直後に `Evaluate()` メソッドを作成します。

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

`Evaluate` メソッドは次のタスクを実行します。

* テスト データ セットを読み込む。
* 回帰エバリュエーターを作成する。
* モデルを評価し、メトリックを作成する。
* メトリックを表示する。

`Train` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main` メソッドからの新しいメソッドの呼び出しを追加します。

[!code-csharp[CallEvaluate](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#14 "Call the Evaluate method")]

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) メソッドを使って、テスト データセットを読み込みます。 次のコードを `Evaluate` メソッドに追加することで、このデータセットを品質チェックとして使って、モデルを評価します。

[!code-csharp[LoadTestDataset](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#15 "Load the test dataset")]

次に、以下のコードを `Evaluate()` に追加して、`Test` データを変換します。

[!code-csharp[PredictWithTransformer](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#16 "Predict using the Transformer")]

[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドによって、テスト データセットの入力行に対する予測が行われます。

`RegressionContext.Evaluate` メソッドは、指定されたデータセットを使用して `PredictionModel` の品質メトリックを計算します。 これによって返される <xref:Microsoft.ML.Data.RegressionMetrics> オブジェクトには、回帰エバリュエーターによって計算されるメトリック全体が含まれます。

これらを表示してモデルの品質を判定するには、最初にメトリックを取得する必要があります。 `Evaluate` メソッドに次のコード行を追加します。

[!code-csharp[ComputeMetrics](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#17 "Compute Metrics")]

予測セットができたら、[Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) メソッドがモデルを評価します。これは予測された値をテスト データセット内の実際の `Labels` と比較して、モデルのパフォーマンスのメトリックを返します。

次のコードを追加してモデルを評価し、評価メトリックを生成します。

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

[RSquared](../resources/glossary.md#coefficient-of-determination) は回帰モデルのもう 1 つの評価メトリックです。 RSquared は 0 から 1 までの値を取ります。 値が 1 に近づくほど、優れたモデルになります。 次のコードを `Evaluate` メソッドに追加して、RSquared 値を表示します。

[!code-csharp[DisplayRSquared](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#18 "Display the RSquared metric.")]

[RMS](../resources/glossary.md#root-of-mean-squared-error-rmse) は回帰モデルの評価メトリックの 1 つです。 RMS が低いほど、優れたモデルになります。 `Evaluate` メソッドに次のコードを追加することで、RMS 値を表示します。

[!code-csharp[DisplayRMS](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a>モデルを使用して予測を行う

`Evaluate` メソッドの直後に、次のコードを使用して `TestSinglePrediction` メソッドを作成します。

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

`TestSinglePrediction` メソッドは次のタスクを実行します。

* テスト データの 1 つのコメントを作成します。
* テスト データに基づいて運賃合計を予測します。
* テスト データと予測をレポート用に結合する。
* 予測された結果を表示する。

`Evaluate` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main` メソッドからの新しいメソッドの呼び出しを追加します。

[!code-csharp[CallTestSinglePrediction](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#20 "Call the TestSinglePrediction method")]

次のコードを `TestSinglePrediction()` に追加することで、`PredictionEngine` を使って運賃を予測します。

[!code-csharp[MakePredictionEngine](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#22 "Create the PredictionFunction")]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

> [!NOTE]
> `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

このチュートリアルでは、このクラス内の 1 つのテスト用の旅行を使用します。 モデルを試行するために後で他のシナリオを追加できます。 `TaxiTrip` のインスタンスを作成して、`TestSinglePrediction()` メソッドでコストのトレーニング済みモデルの予測をテストするために、旅行を追加します。

[!code-csharp[PredictionData](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#23 "Create test data for single prediction")]

次に、`TestSinglePrediction()` メソッドの次のコード行として以下を追加して、タクシー乗車データの 1 つのインスタンスに基づいて運賃を予測し、それを `PredictionEngine` に渡します。

[!code-csharp[Predict](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#24 "Create a prediction of taxi fare")]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 関数では、データの 1 つのインスタンスに対して予測を行います。

指定された旅行の予測運賃を表示するために、次のコードを `TestSinglePrediction` メソッドに追加します。

[!code-csharp[Predict](~/samples/snippets/machine-learning/TaxiFarePrediction/csharp/Program.cs#25 "Display the prediction.")]

プログラムを実行し、テスト ケースに対して予測されたタクシー運賃を確認します。

おめでとうございます! これで、タクシー旅行運賃を予測する機械学習モデルが正常に構築され、その正確度が評価され、このモデルを使用した予測が行われました。 このチュートリアルのソース コードは、[dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TaxiFarePrediction) GitHub リポジトリで確認できます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
>
> * データを準備して理解する
> * 学習パイプラインを作成する
> * データを読み込んで変換する
> * 学習アルゴリズムを選択する
> * モデルをトレーニングする
> * モデルを評価する
> * モデルを使用して予測を行う

さらに詳しく学習するには、次のチュートリアルに進んでください。

> [!div class="nextstepaction"]
> [アヤメのクラスタリング](iris-clustering.md)
