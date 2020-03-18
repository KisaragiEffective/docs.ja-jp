---
title: 'チュートリアル: 映画レコメンダーをビルドする - マトリックス因子分解'
description: このチュートリアルでは、.NET Core コンソール アプリケーションにおいて ML.NET によって映画レコメンダーを構築する方法を示します。 手順では C# と Visual Studio 2019 を使用します。
author: briacht
ms.date: 09/30/2019
ms.custom: mvc, title-hack-0516
ms.topic: tutorial
ms.openlocfilehash: a221289d0c232863f03a275c26dce835f2878bf7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241105"
---
# <a name="tutorial-build-a-movie-recommender-using-matrix-factorization-with-mlnet"></a>チュートリアル: ML.NET でマトリックス因子分解を使用して映画レコメンダーをビルドする

このチュートリアルでは、.NET Core コンソール アプリケーションにおいて ML.NET によって映画レコメンダーを構築する方法を示します。 手順では C# と Visual Studio 2019 を使用します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * 機械学習アルゴリズムを選択する
> * データを準備して読み込む
> * モデルを構築してトレーニングする
> * モデルを評価する
> * モデルを展開して使用する

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation) リポジトリで確認できます。

## <a name="machine-learning-workflow"></a>機械学習ワークフロー

次の手順を使用して、自分のタスクとその他の ML.NET タスクを実行します。

1. [データを読み込む](#load-your-data)
2. [モデルを構築してトレーニングする](#build-and-train-your-model)
3. [モデルを評価する](#evaluate-your-model)
4. [モデルを使用する](#use-your-model)

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

## <a name="select-the-appropriate-machine-learning-task"></a>適切な機械学習タスクを選択する

レコメンデーションの問題にアプローチするには、映画のリストを推奨する、関連作品のリストを推奨するといった複数の方法がありますが、ここでは、ユーザーが特定の映画にどのような評価 (1 - 5) を付けるかを予測して、定義したしきい値よりも高い場合にその映画を推奨します (評価が高いほど、ユーザーが特定の映画を気に入る可能性が高くなります)。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

### <a name="create-a-project"></a>プロジェクトを作成する

1. Visual Studio 2017 を開きます。 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** をメニュー バーから選択します。 **[新しいプロジェクト]** ダイアログで、 **[Visual C#]** ノードを選択し、 **[.NET Core]** ノードを選択します。 次に、 **[コンソール アプリ (.NET Core)]** プロジェクト テンプレートを選択します。 **[名前]** テキスト ボックスに「MovieRecommender」と入力し、 **[OK]** ボタンを選びます。

2. プロジェクトに *Data* という名前のディレクトリを作成して、データ セットを保存します。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 「Data」と入力して Enter キーを押します。

3. **Microsoft.ML** と **Microsoft.ML.Recommender** NuGet パッケージをインストールします。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。 **[参照]** タブを選択し、「**Microsoft.ML**」を検索します。一覧からそのパッケージを選択し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。 **Microsoft.ML.Recommender** に対してこれらの手順を繰り返します。

4. *Program.cs* の先頭に次の `using` ステートメントを追加します。

    [!code-csharp[UsingStatements](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#UsingStatements "Add necessary usings")]

### <a name="download-your-data"></a>データをダウンロードする

1. 2 つのデータセットをダウンロードし、以前に作成した *Data* フォルダーに保存します。

   * [*recommendation-ratings-train.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-train.csv) を右クリックして、[名前を付けてリンク先を保存] または [対象をファイルに保存] を選択します。
   * [*recommendation-ratings-test.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-test.csv) 右クリックして、[名前を付けてリンク先を保存] または [対象をファイルに保存] を選択します。

     \*.csv ファイルを、*Data* フォルダーに保存したことを確認します。または他の場所に保存した後に、\*.csv ファイルを *Data* フォルダーに移動します。

2. ソリューション エクスプローラーで、各 \*.csv ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

   ![VS の [新しい場合はコピーする] を選択するユーザーの GIF](./media/movie-recommendation/copy-to-output-if-newer.gif)

## <a name="load-your-data"></a>データを読み込む

ML.NET プロセスの最初の手順では、モデルのトレーニングとテストのデータを準備して読み込みます。

レコメンデーションの評価データは、`Train` と `Test` のデータセットに分割されます。 `Train` データは、モデルを適合させるために使用します。 `Test` データは、トレーニング済みモデルを使用して予測を行い、モデルのパフォーマンスを評価するのに使用します。 `Train` データと `Test` データは通常、80 対 20 に分割されます。

\*.csv ファイルからのデータのプレビューを以下に示します。

![CVS データセットのプレビューのスクリーンショット。](./media/movie-recommendation/csv-file-dataset-preview.png)

\*.csv ファイルには、4 つの列があります。

* `userId`
* `movieId`
* `rating`
* `timestamp`

機械学習では、予測に使用される列は[特徴](../resources/glossary.md#feature)と呼ばれ、返された予測がある列は[ラベル](../resources/glossary.md#label)と呼ばれます。

映画の評価を予測するので、rating (評価) 列が `Label` になります。 他の 3 つの列 `userId`、`movieId`、`timestamp` はすべて、`Label` を予測するために使用される `Features` です。

| フィーチャー      | group1         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |
| `timestamp`     |               |

`Label` を予測するために使用する `Features` を決めるかどうかは任意です。 最適な `Features` の選択を支援するため、[順列の特徴量の重要度](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md)のような手法を使用することもできます。

ここでは、`Feature` として `timestamp` 列を削除する必要があります。これは、タイムスタンプはユーザーが特定の映画をどのように評価するかにはまったく影響しないため、より正確な予測を行うことには役立たないからです。

| フィーチャー      | group1         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |

次に、入力クラスのデータ構造を定義する必要があります。

プロジェクトに新しいクラスを追加します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックして、 **[追加]、[新しいアイテム]** の順に選びます。

2. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを「*MovieRatingData.cs*」に変更します。 次に **[追加]** を選択します。

コード エディターに *MovieRatingData.cs* ファイルが開きます。 *MovieRatingData.cs* の先頭に次の `using` ステートメントを追加します。

```csharp
using Microsoft.ML.Data;
```

既存のクラス定義を削除し、*MovieRatingData.cs* に次のコードを追加して、`MovieRating` というクラスを作成します。

[!code-csharp[MovieRatingClass](~/samples/snippets/machine-learning/MovieRecommendation/csharp/MovieRatingData.cs#MovieRatingClass "Add the Movie Rating class")]

`MovieRating` は入力データ クラスを指定します。 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性は、データセット内のどの列 (列インデックス) を読み込むかを指定します。 `userId` 列と `movieId` 列が `Features` (`Label` を予測するためのモデルへの入力) で、rating (評価) 列が予測する `Label` (モデルの出力) です。

*MovieRatingData.cs* で `MovieRating` クラスの後に次のコードを追加して、予測結果を表す別のクラス `MovieRatingPrediction` を作成します。

[!code-csharp[PredictionClass](~/samples/snippets/machine-learning/MovieRecommendation/csharp/MovieRatingData.cs#PredictionClass "Add the Movie Prediction Class")]

*Program.cs* で、`Console.WriteLine("Hello World!")` を `Main()` 内の次のコードに置き換えます。

[!code-csharp[MLContext](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#MLContext "Add MLContext")]

[MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の開始点で、`mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

`Main()` の後に、`LoadData()` と呼ばれるメソッドを作成します。

```csharp
public static (IDataView training, IDataView test) LoadData(MLContext mlContext)
{

}
```

> [!NOTE]
> このメソッドは、次の手順で return ステートメントを追加するまでエラーになります。

`LoadData()` の次のコード行として以下を追加して、データ パス変数を初期化し、\*.csv ファイルからデータを読み込み、`Train` と `Test` のデータを `IDataView` オブジェクトとして返します。

[!code-csharp[LoadData](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#LoadData "Load data from data paths")]

ML.NET 内のデータは、[IDataView クラス](xref:Microsoft.ML.IDataView)として表されます。 `IDataView` は、表形式のデータ (数値とテキスト) を表すための柔軟で効率的な方法です。 データはテキスト ファイルから、またはリアルタイムで (SQL データベースやログ ファイルなど) `IDataView` オブジェクトに読み込むことができます。

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) は、データ スキーマを定義し、ファイルを読み取ります。 データ パス変数を取得して、`IDataView` を返します。 ここでは、`Test` ファイルと `Train` ファイルへのパスを指定して、テキスト ファイル ヘッダー (列名を正しく使用できるようにするため) とコンマ文字のデータ区切り記号 (既定の区切り記号はタブ) の両方を指定します。

`Main()` メソッドで次のコードを追加して `LoadData()` メソッドを呼び出し、`Train` と `Test` のデータを返します。

[!code-csharp[LoadDataMain](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#LoadDataMain "Add LoadData method to Main")]

## <a name="build-and-train-your-model"></a>モデルを構築してトレーニングする

ML.NET には、次の 3 つの主要な概念があります。[データ](../resources/glossary.md#data)、[トランスフォーマー](../resources/glossary.md#transformer)、および[エスティメーター](../resources/glossary.md#estimator)です。

機械学習のトレーニング アルゴリズムには、特定の形式のデータが必要です。 `Transformers` は表形式のデータを互換性のある形式に変換 (トランスフォーム) するために使用されます。

![トランスフォーマー データフローの図。](./media/movie-recommendation/data-transformer-transformed.png)

`Estimators` を作成して、ML.NET で `Transformers` を作成します。 `Estimators` はデータを取得して `Transformers` を返します。

![エスティメーター データフローの図。](./media/movie-recommendation/data-estimator-transformer.png)

モデルのトレーニングに使用するレコメンデーション トレーニング アルゴリズムは、`Estimator` の一例です。

次の手順に従って `Estimator` を構築します。

`LoadData()` メソッドの直後に、次のコードを使用して `BuildAndTrainModel()` メソッドを作成します。

```csharp
public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView trainingDataView)
{

}
```

> [!NOTE]
> このメソッドは、次の手順で return ステートメントを追加するまでエラーになります。

次のコードを `BuildAndTrainModel()` に追加して、データ変換を定義します。

[!code-csharp[DataTransformations](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#DataTransformations "Define data transformations")]

`userId` と `movieId` は実際の値ではなく、ユーザーと映画のタイトルを表しているため、[MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) メソッドを使って `userId` と `movieId` をそれぞれ数値キー型の `Feature` 列 (レコメンデーション アルゴリズムで受け入れられる形式) に変換して、新しいデータセットの列として追加します。

| userId | movieId | group1 | userIdEncoded | movieIdEncoded |
| ------------- |:-------------:| -----:|-----:|-----:|
| 1 | 1 | 4 | userKey1 | movieKey1 |
| 1 | 3 | 4 | userKey1 | movieKey2 |
| 1 | 6 | 4 | userKey1 | movieKey3 |

機械学習アルゴリズムを選択し、`BuildAndTrainModel()` 内に次のコード行として以下を追加することで、データ変換定義にそれを追加します。

[!code-csharp[AddAlgorithm](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#AddAlgorithm "Add the training algorithm with options")]

[MatrixFactorizationTrainer](xref:Microsoft.ML.RecommendationCatalog.RecommendationTrainers.MatrixFactorization%28Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options%29) はレコメンデーション トレーニング アルゴリズムです。  [行列因子分解](https://en.wikipedia.org/wiki/Matrix_factorization_(recommender_systems))は、ユーザーが過去に作品をどのように評価したかに関するデータがある場合 (このチュートリアルでのデータセットがそれにあたります) のレコメンデーションへの一般的なアプローチです。 使用可能なさまざまなデータがある場合は、他のレコメンデーション アゴリズムがあります (詳細については、後述の「[その他のレコメンデーション アルゴリズム](#other-recommendation-algorithms)」をご覧ください)。

ここでは、`Matrix Factorization` アルゴリズムは "協調フィルタリング" と呼ばれる手法を使用します。これは、特定の問題についてユーザー 1 とユーザー 2 の意見が同じ場合、別の問題についてもユーザー 1 はユーザー 2 と同じように感じる可能性が高いと仮定します。

たとえば、ユーザー 1 とユーザー 2 が同じように映画を評価している場合、ユーザー 2 はユーザー 1 が鑑賞して高く評価した映画を気に入る可能性が高くなります。

| | `Incredibles 2 (2018)` | `The Avengers (2012)` | `Guardians of the Galaxy (2014)` |
| -------------:|-------------:| -----:|-----:|
| ユーザー 1 | 映画を鑑賞して気に入った | 映画を鑑賞して気に入った | 映画を鑑賞して気に入った |
| ユーザー 2 | 映画を鑑賞して気に入った | 映画を鑑賞して気に入った | まだ鑑賞していない - おすすめの映画 |

`Matrix Factorization` トレーナーには、複数の[オプション](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options)があります。これについては後述の「[アルゴリズムのハイパーパラメーター](#algorithm-hyperparameters)」セクションで詳しく説明します。

`BuildAndTrainModel()` メソッドの次のコード行として以下を追加して、モデルを `Train` データに適合させ、トレーニング済みモデルを返します。

[!code-csharp[FitModel](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#FitModel "Call the Fit method and return back the trained model")]

[Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) メソッドは、指定されたトレーニング データセットを使用してモデルをトレーニングします。 正確には、データを変換してトレーニングを適用することで、`Estimator` 定義を実行し、トレーニング済みモデル (`Transformer`) を返します。

`Main()` メソッドの次のコード行として以下を追加して、`BuildAndTrainModel()` メソッドを呼び出し、トレーニング済みモデルを返します。

[!code-csharp[BuildTrainModelMain](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#BuildTrainModelMain "Add BuildAndTrainModel method in Main")]

## <a name="evaluate-your-model"></a>モデルを評価する

モデルをトレーニングしたら、テスト データを使用してモデルのパフォーマンスを評価します。

`BuildAndTrainModel()` メソッドの直後に、次のコードを使用して `EvaluateModel()` メソッドを作成します。

```csharp
public static void EvaluateModel(MLContext mlContext, IDataView testDataView, ITransformer model)
{

}
```

次のコードを `EvaluateModel()` に追加して、`Test` データを変換します。

[!code-csharp[Transform](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#Transform "Transform the test data")]

[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドはテスト データセットの指定した複数の入力行に対して予測を行います。

`EvaluateModel()` メソッドの次のコード行として以下を追加して、モデルを評価します。

[!code-csharp[Evaluate](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#Evaluate "Evaluate the model using predictions from the test data")]

予測セットができたら、[Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) メソッドがモデルを評価します。これは予測された値をテスト データセット内の実際の `Labels` と比較して、モデルのパフォーマンスのメトリックを返します。

`EvaluateModel()` メソッドの次のコード行として以下を追加して、評価メトリックをコンソールに出力します。

[!code-csharp[PrintMetrics](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#PrintMetrics "Print the evaluation metrics")]

`Main()` メソッドの次のコード行として以下を追加して、`EvaluateModel()` メソッドを呼び出します。

[!code-csharp[EvaluateModelMain](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#EvaluateModelMain "Add EvaluateModel method in Main")]

ここまでの出力は、次のテキストのようになるはずです。

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5403   3.1262e+05
   1       0.9221   1.6030e+05
   2       0.8687   1.5046e+05
   3       0.8416   1.4584e+05
   4       0.8142   1.4209e+05
   5       0.7849   1.3907e+05
   6       0.7544   1.3594e+05
   7       0.7266   1.3361e+05
   8       0.6987   1.3110e+05
   9       0.6751   1.2948e+05
  10       0.6530   1.2766e+05
  11       0.6350   1.2644e+05
  12       0.6197   1.2541e+05
  13       0.6067   1.2470e+05
  14       0.5953   1.2382e+05
  15       0.5871   1.2342e+05
  16       0.5781   1.2279e+05
  17       0.5713   1.2240e+05
  18       0.5660   1.2230e+05
  19       0.5592   1.2179e+05
=============== Evaluating the model ===============
Rms: 0.994051469730769
RSquared: 0.412556298844873
```

この出力には 20 のイテレーションがあります。 イテレーションごとに測定誤差が低下し、0 にどんどん近づいていきます。

モデル予測値とテスト データセットで観察された値の差を測定するために `root of mean squared error` (RMS または RMSE) が使用されます。 厳密には、これはエラーの 2 乗の平均の平方根です。 RMS が低いほど、優れたモデルになります。

`R Squared` は、データがモデルにどの程度適合しているかを示します。 0 ～ 1 の値になります。 値 0 は、データがランダムであるか、モデルに適合できないことを意味します。 値 1 は、モデルがデータと完全に一致していることを意味します。 `R Squared` スコアは可能な限り 1 に近づけます。

優れたモデルの構築は、反復的なプロセスです。 このチュートリアルでは、モデルのトレーニングを短時間で実行するために小さなデータセットを使用しているため、このモデルの品質は最初は低くなっています。 このモデルの品質に満足できなければ、大規模なトレーニング データセットを使用するか、別のトレーニング アルゴリズムとアルゴリズムごとに異なるハイパーパラメーターを選択することで、モデルを改良することを試行できます。 詳細については、後述する「[モデルを改良する](#improve-your-model)」セクションを参照してください。

## <a name="use-your-model"></a>モデルを使用する

これでトレーニング済みのモデルを使用して、新しいデータで予測を行えます。

`EvaluateModel()` メソッドの直後に、次のコードを使用して `UseModelForSinglePrediction()` メソッドを作成します。

```csharp
public static void UseModelForSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

次のコードを `UseModelForSinglePrediction()` に追加して評価を予測するには、`PredictionEngine` を使用します。

[!code-csharp[PredictionEngine](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#PredictionEngine "Create Prediction Engine")]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

> [!NOTE]
> `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

`testInput` と呼ばれる `MovieRating` のインスタンスを作成し、`UseModelForSinglePrediction()` メソッドの次のコード行として以下を追加することで、それを PredictionEngine に渡します。

[!code-csharp[MakeSinglePrediction](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#MakeSinglePrediction "Make a single prediction with the Prediction Engine")]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 関数はデータの 1 つの列で予測を行います。

そして、`Score` (予測された評価) を使用して、movieId 10 の映画をユーザー 6 に推奨するかどうかを決めることができます。 `Score` が高くなるほど、ユーザーが特定の映画を気に入る可能性が高くなります。 ここでは、予測された評価が 3.5 より高い映画を推奨するとします。

結果を出力するには、`UseModelForSinglePrediction()` メソッドの次のコード行として以下を追加します。

[!code-csharp[PrintResults](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#PrintResults "Print the recommendation prediction results")]

`Main()` メソッドの次のコード行として以下を追加して、`UseModelForSinglePrediction()` メソッドを呼び出します。

[!code-csharp[UseModelMain](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#UseModelMain "Add UseModelForSinglePrediction method in Main")]

このメソッドの出力は、次のテキストのようになるはずです。

```console
=============== Making a prediction ===============
Movie 10 is recommended for user 6
```

### <a name="save-your-model"></a>モデルを保存する

エンド ユーザー アプリケーションでモデルを使用して予測を行うには、最初にモデルを保存する必要があります。

`UseModelForSinglePrediction()` メソッドの直後に、次のコードを使用して `SaveModel()` メソッドを作成します。

```csharp
public static void SaveModel(MLContext mlContext, DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

`SaveModel()` メソッドに次のコードを追加して、トレーニング済みモデルを保存します。

[!code-csharp[SaveModel](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#SaveModel "Save the model to a zip file")]

このメソッドは、トレーニング済みモデルを ("Data" フォルダー内の) .zip ファイルに保存します。保存されたファイルは、他の .NET アプリケーションで予測を行うために使用できます。

`Main()` メソッドの次のコード行として以下を追加して、`SaveModel()` メソッドを呼び出します。

[!code-csharp[SaveModelMain](~/samples/snippets/machine-learning/MovieRecommendation/csharp/Program.cs#SaveModelMain "Create SaveModel method in Main")]

### <a name="use-your-saved-model"></a>保存したモデルを使用する

トレーニング済みモデルを保存すると、さまざまな環境でそのモデルを利用できるようになります。 トレーニング済みの機械学習モデルを運用化する方法については、「[トレーニング済みモデルの保存と読み込み](../how-to-guides/save-load-machine-learning-models-ml-net.md)」を参照してください。

## <a name="results"></a>結果

上記の手順を実行した後、コンソール アプリを実行します (Ctrl + F5)。 上記の 1 つの予測の結果は、次のようになるはずです。 警告メッセージまたは処理中のメッセージが表示される場合がありますが、わかりやすくするため、これらのメッセージは結果から削除してあります。

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5382   3.1213e+05
   1       0.9223   1.6051e+05
   2       0.8691   1.5050e+05
   3       0.8413   1.4576e+05
   4       0.8145   1.4208e+05
   5       0.7848   1.3895e+05
   6       0.7552   1.3613e+05
   7       0.7259   1.3357e+05
   8       0.6987   1.3121e+05
   9       0.6747   1.2949e+05
  10       0.6533   1.2766e+05
  11       0.6353   1.2636e+05
  12       0.6209   1.2561e+05
  13       0.6072   1.2462e+05
  14       0.5965   1.2394e+05
  15       0.5868   1.2352e+05
  16       0.5782   1.2279e+05
  17       0.5713   1.2227e+05
  18       0.5637   1.2190e+05
  19       0.5604   1.2178e+05
=============== Evaluating the model ===============
Rms: 0.977175077487166
RSquared: 0.43233349213192
=============== Making a prediction ===============
Movie 10 is recommended for user 6
=============== Saving the model to a file ===============
```

おめでとうございます! 映画をお勧めするための機械学習モデルが正常に作成されました。 このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation) リポジトリで確認できます。

## <a name="improve-your-model"></a>モデルを改良する

より正確な予測を取得するために、モデルのパフォーマンスを向上させることができるいくつかの方法があります。

### <a name="data"></a>データ

各ユーザーと映画 ID の十分なサンプルが含まれたトレーニング データをさらに追加することで、レコメンデーション モデルの品質を向上させることができます。

[クロス検証](../how-to-guides/train-machine-learning-model-cross-validation-ml-net.md)は、モデルを評価するための手法で、(このチュートリアルで行ったようにデータセットからテスト データを抽出するのではなく) データをサブセットにランダムに分割して、一部のグループをトレーニング データとして取得し、一部のグループをテスト データとして取得します。 この手法は、モデルの品質に関しては、トレーニングとテストを分割するよりも優れています。

### <a name="features"></a>フィーチャー

このチュートリアルでは、データセットで提供される 3 つの `Features` (`user id`、`movie id`、`rating`) のみを使用します。

これは出発点としては適切ですが、実際には、データセットに他の属性や `Features` (年齢、性別、地理的場所など) が含まれている場合には、これらを追加することができます。 より関連性の高い `Features` を追加することで、レコメンデーション モデルのパフォーマンスを向上させることができます。

自分の機械学習タスクにとってどの `Features` が最も関連性が高いかがわからない場合は、最も影響が大きい `Features` を検出するために ML.NET で提供されている Feature Contribution Calculation (FCC) および[順列の特徴量の重要度](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md)を利用することもできます。

### <a name="algorithm-hyperparameters"></a>アルゴリズムのハイパーパラメーター

ML.NET には優れたトレーニング アルゴリズムが既定で提供されていますが、アルゴリズムの[ハイパーパラメーター](../resources/glossary.md#hyperparameter)を変更することで、パフォーマンスをさらに微調整することができます。

`Matrix Factorization` の場合、[NumberOfIterations](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.NumberOfIterations) や [ApproximationRank](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.ApproximationRank) などのハイパーパラメーターを使用して、より良い結果が出るかどうかを実験することができます。

たとえば、このチュートリアルでのアルゴリズムのオプションは次のようになります。

```csharp
var options = new MatrixFactorizationTrainer.Options
{
    MatrixColumnIndexColumnName = "userIdEncoded",
    MatrixRowIndexColumnName = "movieIdEncoded",
    LabelColumnName = "Label",
    NumberOfIterations = 20,
    ApproximationRank = 100
};
```

### <a name="other-recommendation-algorithms"></a>その他のレコメンデーション アルゴリズム

協調フィルタリングを使用した行列因子分解アルゴリズムは、映画のレコメンデーションを実行するための唯一のアプローチです。 利用可能な評価データがなく、ユーザーから映画の履歴を入手するしかない場合がよくあります。 または、ユーザーの評価データ以上のものがある場合もあります。

| アルゴリズム       | シナリオ           | サンプル  |
| ------------- |:-------------:| -----:|
| 1 つのクラスの行列因子分解 | userId と movieId しかない場合はこれを使用します。 このレコメンデーション スタイルは、共同購入シナリオ (よく一緒に購入される製品) に基づいています。つまり、顧客の購入履歴に基づいて、一連の製品を顧客に推奨します。 | [> 試す](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/MatrixFactorization_ProductRecommendation) |
| Field Aware Factorization Machines | userId、productId、rating 以外の特徴 (製品の説明や製品の価格など) がある場合にはこれを使用してレコメンデーションを作成します。 この手法では協調フィルタリング アプローチも使用します。 | [> 試す](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/end-to-end-apps/Recommendation-MovieRecommender) |

### <a name="new-user-scenario"></a>新規ユーザー シナリオ

協調フィルタリングで一般的な問題の 1 つは、推測するための以前のデータがない新規ユーザーがいる場合のコールド スタートの問題です。 この問題は多くの場合、新規ユーザーにプロファイルを作成して、たとえば過去に見た映画を評価してもらうことで解決できます。 この手法はユーザーに多少の負担をかけることになりますが、評価履歴がない新規ユーザーの開始データが得られます。

## <a name="resources"></a>リソース

このチュートリアルで使用したデータは、[MovieLens データセット](http://files.grouplens.org/datasets/movielens/)から派生しています。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
>
> * 機械学習アルゴリズムを選択する
> * データを準備して読み込む
> * モデルを構築してトレーニングする
> * モデルを評価する
> * モデルを展開して使用する

さらに詳しく学習するには、次のチュートリアルに進んでください。
> [!div class="nextstepaction"]
> [感情分析](sentiment-analysis.md)
