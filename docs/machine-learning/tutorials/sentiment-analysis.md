---
title: 'チュートリアル: Web サイトのコメントを分析する - 二項分類'
description: このチュートリアルでは、Web サイトのコメントからセンチメントを分類して適切なアクションを実行する .NET Core コンソール アプリケーションの作成方法について説明します。 この二項センチメント分類子には、Visual Studio で C# を使用します。
ms.date: 09/30/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 47b9a9fe37cbcacab3797ed7fb1398b0c524d746
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241131"
---
# <a name="tutorial-analyze-sentiment-of-website-comments-with-binary-classification-in-mlnet"></a>チュートリアル: ML.NET の二項分類を使用して Web サイトのコメントのセンチメントを分析する

このチュートリアルでは、Web サイトのコメントからセンチメントを分類して適切なアクションを実行する .NET Core コンソール アプリケーションの作成方法について説明します。 この二項センチメント分類子には、Visual Studio 2017 で C# を使用します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - コンソール アプリケーションを作成する
> - データの準備
> - データを読み込む
> - モデルを構築してトレーニングする
> - モデルを評価する
> - モデルを使用して予測する
> - 結果を見る

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis) リポジトリで確認できます。

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)が ".NET Core クロスプラット フォーム開発" ワークロードと共にインストールされている

- [UCI Sentiment Labeled Sentences データセット](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) (ZIP ファイル)

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. "SentimentAnalysis" という名前の **.NET Core コンソール アプリケーション**を作成します。

2. データ セット ファイルを保存するために、プロジェクトに *Data* という名前のディレクトリを作成します。

3. **Microsoft.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択してから、 **[参照]** タブを選択します。**Microsoft.ML** を検索し、目的のパッケージを選択して、 **[インストール]** ボタンを選択します。 選択したパッケージのライセンス条項に同意してインストールを続行します。 **Microsoft.ML.FastTree** NuGet パッケージに対して同じことを行います。

## <a name="prepare-your-data"></a>データを準備する

> [!NOTE]
> このチュートリアルのデータセットは、「From Group to Individual Labels using Deep Features」 (Kotzias 他 著、 KDD 2015) のものであり、UCI 機械学習リポジトリ (Dua, D. and Karra Taniskidou, E.(2017)) でホストされています。 UCI 機械学習リポジトリ [http://archive.ics.uci.edu/ml ]。 カリフォルニア州アーバイン: カリフォルニア大学情報コンピュータサイエンス学部。

1. [UCI Sentiment Labeled Sentences データセットの ZIP ファイル](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)をダウンロードし、展開します。

2. `yelp_labelled.txt` ファイルを、作成した *Data* ディレクトリにコピーします。

3. ソリューション エクスプローラーで、`yelp_labeled.txt` ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

### <a name="create-classes-and-define-paths"></a>クラスを作成してパスを定義する

1. 次に示す追加の `using` ステートメントを *Program.cs* ファイルの先頭に追加します。

    [!code-csharp[AddUsings](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#AddUsings "Add necessary usings")]

1. `Main` メソッドの直前の行に次のコードを追加して、最近ダウンロードしたデータセット ファイルのパスを保持するフィールドを作成します。

    [!code-csharp[Declare global variables](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

1. 次に、入力データと予測のためにクラスを作成します。 プロジェクトに新しいクラスを追加します。

    - **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。

    - **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを *SentimentData.cs* に変更します。 次に **[追加]** を選択します。

1. コード エディターで *SentimentData.cs* ファイルが開きます。 *SentimentData.cs* の先頭に次の `using` ステートメントを 追加します。

    [!code-csharp[AddUsings](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/SentimentData.cs#AddUsings "Add necessary usings")]

1. 既存のクラス定義を削除し、`SentimentData` と `SentimentPrediction` の 2 つのクラスを含む次のコードを *SentimentData.cs* ファイルに追加します。

    [!code-csharp[DeclareTypes](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/SentimentData.cs#DeclareTypes "Declare data record types")]

### <a name="how-the-data-was-prepared"></a>データの準備方法

入力データセット クラスの `SentimentData` には、ユーザー コメント用の `string` (`SentimentText`) と、センチメント用の 1 (肯定的) または 0 (否定的) のいずれかの `bool` (`Sentiment`) 値があります。 どちらのフィールドにも [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性が添付され、各フィールドのデータ ファイルの順序が記述されています。  さらに、`Sentiment` プロパティには、それを `Label` フィールドとして指定する [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute.%23ctor%2A) 属性があります。 次のファイル例にはヘッダー行がなく、次のような内容です。

|SentimentText                         |Sentiment (ラベル) |
|--------------------------------------|----------|
|Waitress was a little slow in service.|    0     |
|Crust is not good.                    |    0     |
|Wow...Loved this place.              |    1     |
|Service was very prompt.              |    1     |

`SentimentPrediction` はモデルのトレーニング後に使用される予測クラスです。 `SentimentData` から継承されるため、入力 `SentimentText` を出力予測と共に表示することができます。 `Prediction` ブール値は、新しい入力 `SentimentText` が指定されたときにモデルで予測される値です。

出力クラス `SentimentPrediction` にはモデルによって計算されたその他の 2 つのプロパティが含まれています。1 つはモデルによって計算された生のスコアの `Score`、もう 1 つは肯定的センチメントを持つテキストの尤度に調整されたスコアの `Probability` です。

このチュートリアルで最も重要なプロパティは `Prediction` です。

## <a name="load-the-data"></a>データを読み込む

ML.NET 内のデータは、[IDataView クラス](xref:Microsoft.ML.IDataView)として表されます。 `IDataView` は、表形式のデータ (数値とテキスト) を表すための柔軟で効率的な方法です。 データはテキスト ファイルから、またはリアルタイムで (SQL データベースやログ ファイルなど) `IDataView` オブジェクトに読み込むことができます。

[MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の始点です。 `mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

アプリを準備してからデータを読み込みます。

1. `Main` メソッドの `Console.WriteLine("Hello World!")` の行は、mlContext 変数を宣言して初期化する次のコードに置き換えます。

    [!code-csharp[CreateMLContext](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CreateMLContext "Create the ML Context")]

2. `Main()` メソッドに次のコード行を追加します。

    [!code-csharp[CallLoadData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CallLoadData)]

3. `Main()` メソッドの直後に、次のコードを使用して `LoadData()` メソッドを作成します。

    ```csharp
    public static TrainTestData LoadData(MLContext mlContext)
    {

    }
    ```

    `LoadData()` メソッドは次のタスクを実行します。

    - データを読み込みます。
    - 読み込んだデータセットを、トレーニングデータセットとテスト データセットに分割します。
    - 分割されたトレーニングデータセットとテスト データセットを返します。

4. `LoadData()` メソッドの最初の行として、次のコードを追加します。

    [!code-csharp[LoadData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#LoadData "loading dataset")]

    [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) は、データ スキーマを定義し、ファイルを読み取ります。 データ パス変数を取得して、`IDataView` を返します。

### <a name="split-the-dataset-for-model-training-and-testing"></a>モデルのトレーニング用とテスト用にデータセットを分割する

モデルを準備するときは、データセットの一部を使用してモデルをトレーニングし、データセットの一部を使用してモデルの正確度をテストします。

1. 読み込まれたデータを必要なデータセットに分割するには、`LoadData()` メソッドの次の行として、次のコードを追加します。

    [!code-csharp[SplitData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#SplitData "Split the Data")]

    前のコードでは、[TrainTestSplit()](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit%2A) メソッドを使用して、読み込まれたデータセットをトレーニング データセットとテスト データセットに分割し、それらを [TrainTestData](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) クラスで返します。 `testFraction` パラメーターを使用して、データに占めるテスト セットの割合を指定します。 既定値は 10% です。この場合、より多くのデータを評価するために 20% を使用します。

2. `LoadData()` メソッドの最後に、`splitDataView` が返されます。

    [!code-csharp[ReturnSplitData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#ReturnSplitData)]

## <a name="build-and-train-the-model"></a>モデルを構築してトレーニングする

1. `Main()` メソッドの次のコード行として、`BuildAndTrainModel` メソッドに次の呼び出しを追加します。

    [!code-csharp[CallBuildAndTrainModel](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CallBuildAndTrainModel)]

    `BuildAndTrainModel()` メソッドは次のタスクを実行します。

    - データを抽出して変換します。
    - モデルをトレーニングする。
    - テスト データに基づいてセンチメントを予測する。
    - モデルを返します。

2. `Main()` メソッドの直後に、次のコードを使用して `BuildAndTrainModel()` メソッドを作成します。

    ```csharp
    public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView splitTrainSet)
    {

    }
    ```

### <a name="extract-and-transform-the-data"></a>データを抽出して変換する

1. 次のコード行として `FeaturizeText` を呼び出します。

    [!code-csharp[FeaturizeText](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#FeaturizeText "Featurize the text")]

    前のコードの `FeaturizeText()` メソッドでは、テキスト列 (`SentimentText`) を機械学習アルゴリズムから使用される数値キー型の `Features` 列に変換し、それを新しいデータセット列として追加します。

    |SentimentText                         |Sentiment |フィーチャー              |
    |--------------------------------------|----------|----------------------|
    |Waitress was a little slow in service.|    0     |[0.76, 0.65, 0.44, …] |
    |Crust is not good.                    |    0     |[0.98, 0.43, 0.54, …] |
    |Wow...Loved this place.              |    1     |[0.35, 0.73, 0.46, …] |
    |Service was very prompt.              |    1     |[0.39, 0, 0.75, …]    |

### <a name="add-a-learning-algorithm"></a>学習アルゴリズムを追加する

このアプリでは、データの項目または行を分類する分類アルゴリズムを使用しています。 このアプリでは、Web サイトのコメントが肯定的または否定的に分類されるので、二項分類タスクが使用されます。

データ変換定義に機械学習タスクを追加するには、`BuildAndTrainModel()` の次のコード行に以下を追加します。

[!code-csharp[SdcaLogisticRegressionBinaryTrainer](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#AddTrainer "Add a SdcaLogisticRegressionBinaryTrainer")]

[SdcaLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer) が分類トレーニング アルゴリズムです。 これは `estimator` に追加されます。また、特徴付けされた `SentimentText` (`Features`) と `Label` 入力パラメーターを受け取って履歴データから学習します。

### <a name="train-the-model"></a>モデルをトレーニングする

`BuildAndTrainModel()` メソッドの次のコード行として以下を追加して、モデルを `splitTrainSet` データに適合させ、トレーニング済みモデルを返します。

[!code-csharp[TrainModel](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#TrainModel "Train the model")]

[Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) メソッドでは、データセットを変換して、トレーニングを適用することにより、モデルがトレーニングされます。

### <a name="return-the-model-trained-to-use-for-evaluation"></a>評価に使用する、トレーニング済みのモデルを返す

 `BuildAndTrainModel()` メソッドの終了時にモデルを返します。

[!code-csharp[ReturnModel](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#ReturnModel "Return the model")]

## <a name="evaluate-the-model"></a>モデルを評価する

モデルのトレーニングが終わったら、テスト データを使用してモデルのパフォーマンスを検証します。

1. `BuildAndTrainModel()` の直後に、次のコードを使用して `Evaluate()` メソッドを作成します。

    ```csharp
    public static void Evaluate(MLContext mlContext, ITransformer model, IDataView splitTestSet)
    {

    }
    ```

    `Evaluate()` メソッドは次のタスクを実行します。

    - テスト データ セットを読み込む。
    - BinaryClassification エバリュエーターを作成します。
    - モデルを評価し、メトリックを作成する。
    - メトリックを表示する。

2. `BuildAndTrainModel()` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main()` メソッドからの新しいメソッドの呼び出しを追加します。

    [!code-csharp[CallEvaluate](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CallEvaluate "Call the Evaluate method")]

3. 次のコードを `Evaluate()` に追加して、`splitTestSet` データを変換します。

    [!code-csharp[PredictWithTransformer](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#TransformData "Predict using the Transformer")]

    前のコードでは、[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを使用して、テスト データセットの指定した複数の入力行に対して予測しています。

4. `Evaluate()` メソッドの次のコード行として以下を追加して、モデルを評価します。

    [!code-csharp[ComputeMetrics](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#Evaluate "Compute Metrics")]

予測セット (`predictions`) ができると、[Evaluate()](xref:Microsoft.ML.BinaryClassificationCatalog.Evaluate%2A)メソッドによってモデルが評価され、予測値とテスト データセット内の実際の `Labels` が比較され、モデルのパフォーマンスに関する [CalibratedBinaryClassificationMetrics](xref:Microsoft.ML.Data.CalibratedBinaryClassificationMetrics) オブジェクトが返されます。

### <a name="displaying-the-metrics-for-model-validation"></a>モデル検証のためのメトリックを表示する

次のコードを使用してメトリックを表示します。

[!code-csharp[DisplayMetrics](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#DisplayMetrics "Display selected metrics")]

- `Accuracy` メトリックでは、モデルの正確度が取得されます。これは、テスト セットに含まれる正しい予測の割合です。

- `AreaUnderRocCurve` メトリックは、そのモデルで肯定的クラスと否定的クラスが正しく分類されている信用度を示します。 `AreaUnderRocCurve` を可能な限り 1 に近づけることが目標です。

- `F1Score` メトリックでは、モデルの F1 スコアが取得されます。これは[精度](../resources/glossary.md#precision)と[再現率](../resources/glossary.md#recall)間のバランスのメジャーです。  `F1Score` を可能な限り 1 に近づけることが目標です。

### <a name="predict-the-test-data-outcome"></a>テスト データの結果を予測する

1. `Evaluate()` メソッドの直後に、次のコードを使用して `UseModelWithSingleItem()` メソッドを作成します。

    ```csharp
    private static void UseModelWithSingleItem(MLContext mlContext, ITransformer model)
    {

    }
    ```

    `UseModelWithSingleItem()` メソッドは次のタスクを実行します。

    - テスト データの 1 つのコメントを作成する。
    - テスト データに基づいてセンチメントを予測する。
    - テスト データと予測をレポート用に結合する。
    - 予測された結果を表示する。

2. `Evaluate()` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main()` メソッドからの新しいメソッドの呼び出しを追加します。

    [!code-csharp[CallUseModelWithSingleItem](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CallUseModelWithSingleItem "Call the UseModelWithSingleItem method")]

3. 次のコードを追加して、`UseModelWithSingleItem()` メソッドの 1 行目として作成します。

    [!code-csharp[CreatePredictionEngine](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CreatePredictionEngine1 "Create the PredictionEngine")]

    [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

    > [!NOTE]
    > `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

4. コメントを追加して、`UseModelWithSingleItem()` メソッドでトレーニングされたモデルの予測をテストします。これには `SentimentData` のインスタンスを作成します。

    [!code-csharp[PredictionData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CreateTestIssue1 "Create test data for single prediction")]

5. `UseModelWithSingleItem()` メソッドの次のコード行として以下を追加して、テスト コメント データを [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) に渡します。

    [!code-csharp[Predict](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#Predict "Create a prediction of sentiment")]

    [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 関数では、データの 1 つの行に対して予測を行います。

6. 次のコードを使用して、`SentimentText` とそれに対応するセンチメント予測を表示します。

    [!code-csharp[OutputPrediction](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#OutputPrediction "Display prediction output")]

## <a name="use-the-model-for-prediction"></a>予測にモデルを使用する

### <a name="deploy-and-predict-batch-items"></a>バッチ項目の展開と予測

1. `UseModelWithSingleItem()` メソッドの直後に、次のコードを使用して `UseModelWithBatchItems()` メソッドを作成します。

    ```csharp
    public static void UseModelWithBatchItems(MLContext mlContext, ITransformer model)
    {

    }
    ```

    `UseModelWithBatchItems()` メソッドは次のタスクを実行します。

    - バッチ テスト データを作成します。
    - テスト データに基づいてセンチメントを予測する。
    - テスト データと予測をレポート用に結合する。
    - 予測された結果を表示する。

2. `UseModelWithSingleItem()` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main` メソッドからの新しいメソッドの呼び出しを追加します。

    [!code-csharp[CallPredictModelBatchItems](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CallUseModelWithBatchItems "Call the CallUseModelWithBatchItems method")]

3. `UseModelWithBatchItems()` メソッドでトレーニングされるモデルの予測をテストするために、いくつかのコメントを追加します。

    [!code-csharp[PredictionData](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#CreateTestIssues "Create test data for predictions")]

### <a name="predict-comment-sentiment"></a>コメントのセンチメントを予測する

モデルを使用し、[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを使用してコメント データのセンチメントを予測します。

[!code-csharp[Predict](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#Prediction "Create predictions of sentiments")]

### <a name="combine-and-display-the-predictions"></a>予測を組み合わせて表示する

次のコードを使用して、予測のヘッダーを作成します。

[!code-csharp[OutputHeaders](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#AddInfoMessage "Display prediction outputs")]

`SentimentPrediction` は `SentimentData` から継承されているため、`Transform()` メソッドによって `SentimentText` に予測されたフィールドが設定されました。 ML.NET プロセスが進むにつれて、各コンポーネントに列が追加されます。これで、結果が簡単に表示されるようになります。

[!code-csharp[DisplayPredictions](~/samples/snippets/machine-learning/SentimentAnalysis/csharp/Program.cs#DisplayResults "Display the predictions")]

## <a name="results"></a>結果

結果は以下のようになるはずです。 処理中にメッセージが表示されます。 警告または処理メッセージが表示されることがありますが、 わかりやすくするために、それらは次の結果から削除してあります。

```console
Model quality metrics evaluation
--------------------------------
Accuracy: 83.96%
Auc: 90.51%
F1Score: 84.04%

=============== End of model evaluation ===============

=============== Prediction Test of model with a single sample and test dataset ===============

Sentiment: This was a very bad steak | Prediction: Negative | Probability: 0.1027377
=============== End of Predictions ===============

=============== Prediction Test of loaded model with a multiple samples ===============

Sentiment: This was a horrible meal | Prediction: Negative | Probability: 0.1369192
Sentiment: I love this spaghetti. | Prediction: Positive | Probability: 0.9960636
=============== End of predictions ===============

=============== End of process ===============
Press any key to continue . . .

```

おめでとうございます! これで、メッセージのセンチメントを分類および予測するための機械学習モデルをビルドできました。

優れたモデルの構築は、反復的なプロセスです。 このチュートリアルでは、モデルのトレーニングを短時間で実行するために小さなデータセットを使用しているため、このモデルの品質は最初は低くなっています。 このモデルの品質に満足できなければ、大規模なトレーニング データセットを使用するか、別のトレーニング アルゴリズムとアルゴリズムごとに異なる[ハイパーパラメーター](../resources/glossary.md#hyperparameter)を選択してモデルの改良を試すことができます。

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis) リポジトリで確認できます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> - コンソール アプリケーションを作成する
> - データの準備
> - データを読み込む
> - モデルを構築してトレーニングする
> - モデルを評価する
> - モデルを使用して予測する
> - 結果を見る

さらに詳しく学習するには、次のチュートリアルに進んでください。
> [!div class="nextstepaction"]
> [問題の分類](github-issue-classification.md)
