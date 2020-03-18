---
title: 'チュートリアル: サポートの問題の分類 - 多クラス分類'
description: GitHub の問題を分類し、それを特定の領域に割り当てるための多クラス分類シナリオで、ML.NET を使用する方法について説明します。
ms.date: 01/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: 50980cd933054825bf21f955b0341dd8e66f3e62
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78239938"
---
# <a name="tutorial-categorize-support-issues-using-multiclass-classification-with-mlnet"></a>チュートリアル: ML.NET での多クラス分類を使用したサポートの問題の分類

このサンプル チュートリアルでは ML.NET を使用して GitHub の問題の分類子を作成し、Visual Studio で C# を使用する .NET Core コンソール アプリケーションから GitHub の問題に対する区分ラベルを分類して予測するモデルをトレーニングする方法を示します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * データを準備する
> * データを変換する
> * モデルをトレーニングする
> * モデルを評価する
> * トレーニング済みモデルを使用して予測する
> * 読み込み済みのモデルを使用して配置および予測する

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) リポジトリで確認できます。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

* [Github の問題のタブ区切りのファイル (issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv)。
* [Github の問題のテスト タブ区切りのファイル (issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv)。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

### <a name="create-a-project"></a>プロジェクトを作成する

1. Visual Studio 2017 を開きます。 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** をメニュー バーから選択します。 **[新しいプロジェクト]** ダイアログで、 **[Visual C#]** ノードを選択し、 **[.NET Core]** ノードを選択します。 次に、 **[コンソール アプリ (.NET Core)]** プロジェクト テンプレートを選択します。 **[名前]** テキスト ボックスに「GitHubIssueClassification」と入力し、 **[OK]** を選択します。

2. プロジェクトに *Data* という名前のディレクトリを作成して、データ セット ファイルを保存します。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 「Data」と入力して Enter キーを押します。

3. プロジェクトに *Models* という名前のディレクトリを作成して、モデルを保存します。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 「Models」と入力し、Enter キーを押します。

4. **Microsoft.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。[参照] タブを選択し、「**Microsoft.ML**」を検索し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

### <a name="prepare-your-data"></a>データを準備する

1. [issues_train.tsv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) データ セットと [issues_test.tsv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) データ セットをダウンロードして、それらを作成済みの *Data* フォルダーに保存します。 1 番目のデータ セットでは機械学習モデルをトレーニングし、2 番目のデータ セットはモデルの精度を評価するために使用します。

2. ソリューション エクスプローラーで、各 \*.tsv ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

### <a name="create-classes-and-define-paths"></a>クラスを作成してパスを定義する

次に示す追加の `using` ステートメントを *Program.cs* ファイルの先頭に追加します。

[!code-csharp[AddUsings](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#AddUsings)]

最近ダウンロードしたファイルのパスを保持する 3 つのグローバル フィールドと、`MLContext`、`DataView`、および `PredictionEngine` のためのグローバル変数を作成します。

* `_trainDataPath` には、モデルのトレーニングに使用するデータ セットのパスが含まれます。
* `_testDataPath` には、モデルの評価に使用するデータ セットのパスが含まれます。
* `_modelPath` には、トレーニング済みのモデルを保存するパスが含まれます。
* `_mlContext` は処理コンテキストを提供する <xref:Microsoft.ML.MLContext> です。
* `_trainingDataView` は、トレーニング データセットを処理するために使用される <xref:Microsoft.ML.IDataView> です。
* `_predEngine` は、1 つの予測に使用される <xref:Microsoft.ML.PredictionEngine%602> です。

`Main` メソッドのすぐ上にある行に次のコードを追加して、それらのパスとその他の変数を指定します。

[!code-csharp[DeclareGlobalVariables](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#DeclareGlobalVariables)]

入力データと予測のために、いくつかのクラスを作成します。 プロジェクトに新しいクラスを追加します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。

1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを「*GitHubIssueData.cs*」に変更します。 次に **[追加]** を選択します。

    コード エディターで *GitHubIssueData.cs* ファイルが開きます。 *GitHubIssueData.cs* の先頭に次の `using` ステートメントを追加します。

[!code-csharp[AddUsings](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/GitHubIssueData.cs#AddUsings)]

既存のクラス定義を削除し、`GitHubIssue` と `IssuePrediction` の 2 つのクラスを含む次のコードを *GitHubIssueData.cs* ファイルに追加します。

[!code-csharp[DeclareGlobalVariables](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/GitHubIssueData.cs#DeclareTypes)]

`label` は予測する列です。 識別された `Features` は、ラベルを予測するためにモデルを指定する入力です。

データ セット内のソース列のインデックスを指定するには、[LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute) を使用します。

`GitHubIssue` は、入力データセット クラスで、次の <xref:System.String> フィールドがあります。

* 最初の列 `ID` (GitHub 問題 ID)
* 2 番目の列 `Area` (トレーニングの予測)
* 3 番目の列 `Title` (GitHub の問題のタイトル) は、`Area` の予測に使用される最初の `feature` です
* 第 4 列 `Description` は、`Area` の予測に使用される 2 番目の `feature` です

`IssuePrediction` は、モデルがトレーニングされた後に、予測に使用されるクラスです。 これには、1 つの `string` (`Area`) と、`PredictedLabel` `ColumnName` 属性があります。  `PredictedLabel` は予測と評価の際に使用されます。 評価では、トレーニング データを含む入力、予測された値、およびモデルが使用されます。

すべての ML.NET 操作は、[MLContext ](xref:Microsoft.ML.MLContext)クラスで始まります。 `mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 `Entity Framework` における `DBContext` と、概念的にはほぼ同じです。

### <a name="initialize-variables-in-main"></a>Main で変数を初期化する

複数のトレーニング間で反復可能な決定論的結果を得るために、`_mlContext` グローバル変数をランダム シード (`seed: 0`) を使用して `MLContext` の新しいインスタンスで初期化します。  `Main` メソッドの `Console.WriteLine("Hello World!")` 行を、次のコードで置き換えます。

[!code-csharp[CreateMLContext](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a>データを読み込む

ML.NET では、数値またはテキストの表形式データを記述する柔軟で効率的な方法として、[IDataView クラス](xref:Microsoft.ML.IDataView)を使います。 `IDataView` は、テキスト ファイルを読み込むか、またはリアルタイムで読み込むことができます (たとえば、SQL データベースまたはログ ファイル)。

パイプラインで利用するために `_trainingDataView` グローバル変数を初期化して読み込むには、`mlContext` 初期化の後に次のコードを追加します。

[!code-csharp[LoadTrainData](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#LoadTrainData)]

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) は、データ スキーマを定義し、ファイルを読み取ります。 データ パス変数を取得して、`IDataView` を返します。

`Main` メソッドに次のコード行を追加します。

[!code-csharp[CallProcessData](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CallProcessData)]

`ProcessData` メソッドは次のタスクを実行します。

* データを抽出して変換します。
* 処理パイプラインを返します。

`Main` メソッドの直後に、次のコードを使用して `ProcessData` メソッドを作成します。

```csharp
public static IEstimator<ITransformer> ProcessData()
{

}
```

## <a name="extract-features-and-transform-the-data"></a>特徴を抽出してデータを変換する

`GitHubIssue` に対する GitHub の区分ラベルを予測する場合、[MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) メソッドを使用して `Area` 列を数値キー型の `Label` 列 (分類アルゴリズムによって許可される形式) に変換し、それを新しいデータセット列として追加します。

[!code-csharp[MapValueToKey](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#MapValueToKey)]

次に、テキスト (`Title` および `Description`) 列をそれぞれ `TitleFeaturized` および `DescriptionFeaturized` と呼ばれる数値ベクターに変換する `mlContext.Transforms.Text.FeaturizeText` を呼び出します。 次のコードを使用して、両列の特徴付けをパイプラインに追加します。

[!code-csharp[FeaturizeText](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#FeaturizeText)]

データ準備の最後の手順では、[Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) メソッドを使用して、すべての特徴列を **Features** 列に結合します。 既定では、学習アルゴリズムは **Features** 列の特徴のみを処理します。 次のコードを使用してパイプラインに、この変換を追加します。

[!code-csharp[Concatenate](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#Concatenate)]

 次に、DataView をキャッシュするために <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A> を追加します。つまり、次のコードでキャッシュを使用してデータが複数回反復されると、パフォーマンスが向上する場合があります。

[!code-csharp[AppendCache](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#AppendCache)]

> [!WARNING]
> 小/中規模のデータセットの場合は、AppendCacheCheckpoint を使用して、トレーニング時間を短くします。 非常に大きいデータセットを処理するときは、使用しないでください (.AppendCacheCheckpoint() を削除します)。

`ProcessData` メソッドの最後で、パイプラインが返されます。

[!code-csharp[ReturnPipeline](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#ReturnPipeline)]

この手順で前処理と特徴付けが処理されます。 ML.NET に用意されているその他のコンポーネントを使用すると、モデルでより良い結果が得られます。

## <a name="build-and-train-the-model"></a>モデルを構築してトレーニングする

`Main` メソッドの次のコード行として、`BuildAndTrainModel` メソッドに次の呼び出しを追加します。

[!code-csharp[CallBuildAndTrainModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CallBuildAndTrainModel)]

`BuildAndTrainModel` メソッドは次のタスクを実行します。

* トレーニング アルゴリズムのクラスを作成します。
* モデルをトレーニングする。
* トレーニング データに基づいて区分を予測します。
* モデルを返します。

`Main` メソッドの直後に、次のコードを使用して `BuildAndTrainModel` メソッドを作成します。

```csharp
public static IEstimator<ITransformer> BuildAndTrainModel(IDataView trainingDataView, IEstimator<ITransformer> pipeline)
{

}
```

### <a name="about-the-classification-task"></a>分類タスクについて

分類とは、データを使用して項目またはデータ行のカテゴリ、型、クラスを**判断**する機械学習タスクであり、多くの場合、次のいずれかの種類になります。

* バイナリ: A か B のいずれかである。
* 多クラス: 1 つのモデルを使用して予測できるカテゴリが多数ある。

この種の問題では、問題カテゴリの予測が 2 つだけ (バイナリ) ではなく複数のカテゴリの 1 つ (多クラス) なので、多クラス分類学習アルゴリズムを使用します。

`BuildAndTrainModel()` 内に最初のコード行として以下を付加することで、データ変換定義に機械学習アルゴリズムを追加します。

[!code-csharp[AddTrainer](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#AddTrainer)]

[SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer) は、マルチクラス分類のトレーニング アルゴリズムです。 これは `pipeline` に付加され、特徴付けされた `Title` と `Description` (`Features`) および `Label` 入力パラメータ―を受け入れて、履歴データから学習します。

### <a name="train-the-model"></a>モデルをトレーニングする

`BuildAndTrainModel()` メソッドの次のコード行として以下を追加して、モデルを `splitTrainSet` データに適合させ、トレーニング済みモデルを返します。

[!code-csharp[TrainModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#TrainModel)]

`Fit()` メソッドでは、データセットを変換してトレーニングを適用することで、モデルをトレーニングします。

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスを渡してから、その予測を実行できる便利な API です。 `BuildAndTrainModel()` メソッドに次の行として以下を追加します。

[!code-csharp[CreatePredictionEngine1](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CreatePredictionEngine1)]

### <a name="predict-with-the-trained-model"></a>トレーニング済みモデルを使用して予測する

GitHub の問題を追加して、`Predict` メソッドでトレーニングされたモデルの予測をテストします。これには `GitHubIssue` のインスタンスを作成します。

[!code-csharp[CreateTestIssue1](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CreateTestIssue1)]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 関数を使用して、データの 1 つの行に対して予測を行います。

[!code-csharp[Predict](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#Predict)]

### <a name="using-the-model-prediction-results"></a>モデルの使用: 予測結果

結果を共有し、それに応じたアクションを実行するために、`GitHubIssue` および対応する `Area` ラベル予測を表示します。  次の <xref:System.Console.WriteLine?displayProperty=nameWithType> コードを使用して、結果の表示を作成します。

[!code-csharp[OutputPrediction](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#OutputPrediction)]

### <a name="return-the-model-trained-to-use-for-evaluation"></a>評価に使用する、トレーニング済みのモデルを返す

`BuildAndTrainModel` メソッドの最後で、モデルを返します。

[!code-csharp[ReturnModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#ReturnModel)]

## <a name="evaluate-the-model"></a>モデルを評価する

これでモデルの作成とトレーニングが完了したので、品質保証と検証のために、別のデータ セットを使用してモデルを評価します。 `Evaluate` メソッドでは、`BuildAndTrainModel` で作成されたモデルが渡されて評価されます。 `BuildAndTrainModel` の直後に、次のコードに示すように `Evaluate` メソッドを作成します。

```csharp
public static void Evaluate(DataViewSchema trainingDataViewSchema)
{

}
```

`Evaluate` メソッドは次のタスクを実行します。

* テスト データ セットを読み込む。
* 多クラス評価器を作成する。
* モデルを評価し、メトリックを作成する。
* メトリックを表示する。

`BuildAndTrainModel` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main` メソッドからの新しいメソッドの呼び出しを追加します。

[!code-csharp[CallEvaluate](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CallEvaluate)]

トレーニング データセットで以前にも行ったように、次のコードを `Evaluate` メソッドに追加することで、テスト データセットを読み込みます。

[!code-csharp[LoadTestDataset](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#LoadTestDataset)]

[Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) メソッドは、指定されたデータセットを使用して、モデルに対する品質メトリックを計算します。 これによって返される <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> オブジェクトには、多クラス分類評価器によって計算されるメトリック全体が含まれます。
メトリックを表示してモデルの品質を判定するには、最初にメトリックを取得する必要があります。
特徴を入力して予測を返すために、機械学習の `_trainedModel` グローバル変数 ([ITransformer](xref:Microsoft.ML.ITransformer)) の [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを使用していることに注目してください。 `Evaluate` メソッドに次のコード行を追加します。

[!code-csharp[Evaluate](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#Evaluate)]

多クラス分類では、次のメトリックが評価されます。

* マイクロ精度: すべてのサンプルとクラスのペアが、精度メトリックに均等に作用します。  マイクロ精度は可能な限り 1 に近づけます。

* マクロ精度: すべてのクラスが、精度メトリックに均等に作用します。 少数派のクラスは、大規模なクラスと同じ重みが与えられています。 マクロ精度は可能な限り 1 に近づけます。

* 対数損失: [対数損失](../resources/glossary.md#log-loss)に関するページを参照してください。 対数損失は可能な限り 1 に近づけます。

* 対数損失還元: 範囲は [-inf, 1.00] です。ここで、1.00 は完璧な予測で、0 は平均の予測です。 対数損失還元は可能な限り 1 に近づけます。

### <a name="displaying-the-metrics-for-model-validation"></a>モデル検証のためのメトリックを表示する

次のコードを使用してメトリックを表示し、結果を共有し、それに対してアクションを実行します。

[!code-csharp[DisplayMetrics](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#DisplayMetrics)]

### <a name="save-the-model-to-a-file"></a>モデルをファイルに保存する

モデルに問題がなければ、後で、または別のアプリケーションで予測を行うために、そのモデルをファイルに保存します。 `Evaluate` メソッドに次のコードを追加します。

[!code-csharp[SnippetCallSaveModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#SnippetCallSaveModel)]

`Evaluate` メソッドの下に `SaveModelAsFile` メソッドを作成します。

```csharp
private static void SaveModelAsFile(MLContext mlContext,DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

次のコードを `SaveModelAsFile` メソッドに追加します。 このコードでは [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save*) メソッドを使用して、トレーニング済みのモデルを zip ファイルとしてシリアル化し、格納します。

[!code-csharp[SnippetSaveModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#SnippetSaveModel)]

## <a name="deploy-and-predict-with-a-model"></a>モデルによるデプロイと予測

`Evaluate` メソッドの呼び出しのすぐ下に、次のコードを使用して、`Main` メソッドからの新しいメソッドの呼び出しを追加します。

[!code-csharp[CallPredictIssue](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CallPredictIssue)]

`Evaluate` メソッドの直後 (および `SaveModelAsFile` メソッドの直前) に、次のコードを使用して `PredictIssue` メソッドを作成します。

```csharp
private static void PredictIssue()
{

}
```

`PredictIssue` メソッドは次のタスクを実行します。

* 保存されたモデルを読み込む
* テスト データの問題を 1 つ作成します。
* テスト データに基づいて区分を予測します。
* テスト データと予測をレポート用に結合する。
* 予測された結果を表示する。

`PredictIssue` メソッドに次のコードを追加して、保存されたモデルをアプリケーションに読み込みます。

[!code-csharp[SnippetLoadModel](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#SnippetLoadModel)]

GitHub の問題を追加して、`Predict` メソッドでトレーニングされたモデルの予測をテストします。これには `GitHubIssue` のインスタンスを作成します。

[!code-csharp[AddTestIssue](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#AddTestIssue)]

以前に行ったように、次のコードを使って `PredictionEngine` インスタンスを作成します。

[!code-csharp[CreatePredictionEngine](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#CreatePredictionEngine)]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

> [!NOTE]
> `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

`PredictionEngine` を使用して、予測のための `PredictIssue` メソッドに次のコードを追加して、GitHub の区分ラベルを予測します。

[!code-csharp[PredictIssue](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#PredictIssue)]

### <a name="using-the-loaded-model-for-prediction"></a>予測のために読み込み済みのモデルを使用する

問題を分類し、それに応じて処理するために `Area` を表示します。 次の <xref:System.Console.WriteLine?displayProperty=nameWithType> コードを使用して、結果の表示を作成します。

[!code-csharp[DisplayResults](~/samples/snippets/machine-learning/GitHubIssueClassification/csharp/Program.cs#DisplayResults)]

## <a name="results"></a>結果

結果は以下のようになるはずです。 パイプラインが処理されると、メッセージが表示されます。 警告または処理メッセージが表示されることがありますが、 わかりやすくするために、これらのメッセージは次の結果から削除してあります。

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.738
*       MacroAccuracy:    0.668
*       LogLoss:          .919
*       LogLossReduction: .643
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

おめでとうございます! これで、GitHub の問題用の区分ラベルを分類および予測するための機械学習モデルをビルドできました。 このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) リポジトリで確認できます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> * データを準備する
> * データを変換する
> * モデルをトレーニングする
> * モデルを評価する
> * トレーニング済みモデルを使用して予測する
> * 読み込み済みのモデルを使用して配置および予測する

さらに詳しく学習するには、次のチュートリアルに進んでください。
> [!div class="nextstepaction"]
> [タクシー代予測](predict-prices.md)
