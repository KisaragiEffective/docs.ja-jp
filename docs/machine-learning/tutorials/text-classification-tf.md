---
title: 'チュートリアル: TensorFlow モデルを使用してレビューのセンチメントを分析する'
description: このチュートリアルでは、事前トレーニング済みの TensorFlow モデルを使用して、Web サイトのセンチメントを分類する方法について示します。 センチメントの 2 項分類子は、Visual Studio を使用して開発された C# コンソール アプリケーションです。
ms.date: 11/15/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 688c5b83cef8f21eef8fa24521a85449a9cfbd48
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241118"
---
# <a name="tutorial-analyze-sentiment-of-movie-reviews-using-a-pre-trained-tensorflow-model-in-mlnet"></a>チュートリアル: ML.NET で事前トレーニング済みの TensorFlow モデルを使用して映画レビューのセンチメントを分析する

このチュートリアルでは、事前トレーニング済みの TensorFlow モデルを使用して、Web サイトのセンチメントを分類する方法について示します。 センチメントの 2 項分類子は、Visual Studio を使用して開発された C# コンソール アプリケーションです。

このチュートリアルで使用される TensorFlow モデルは、IMDB データベースの映画レビューを使用してトレーニングされました。 アプリケーションの開発が完了すると、映画レビューのテキストを入力できるようになり、レビューに肯定的または否定的なセンチメントが含まれるかどうかがアプリケーションによって通知されます。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * 事前トレーニング済みの TensorFlow モデルを読み込む
> * Web サイトのコメント テキストをモデルに適したフィーチャーに変換する
> * モデルを使用して予測する

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF) リポジトリで確認できます。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

## <a name="setup"></a>セットアップ

### <a name="create-the-application"></a>アプリケーションを作成する

1. "TextClassificationTF" という名前の **.NET Core コンソール アプリケーション**を作成します。

2. データ セット ファイルを保存するために、プロジェクトに *Data* という名前のディレクトリを作成します。

3. **Microsoft.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 パッケージ ソースとして "nuget.org" を選択してから、 **[参照]** タブを選択します。**Microsoft.ML** を検索し、目的のパッケージを選択して、 **[インストール]** ボタンを選択します。 選択したパッケージのライセンス条項に同意してインストールを続行します。 **Microsoft.ML.TensorFlow** と **SciSharp.TensorFlow.Redist** にこれらの手順を繰り返します。

### <a name="add-the-tensorflow-model-to-the-project"></a>TensorFlow モデルをプロジェクトに追加する

> [!NOTE]
> このチュートリアルのモデルは、[dotnet/machinelearning-testdata](https://github.com/dotnet/machinelearning-testdata/tree/master/Microsoft.ML.TensorFlow.TestModels/sentiment_model) GitHub リポジトリにあります。 このモデルは TensorFlow SavedModel 形式です。

1. [sentiment_model ZIP ファイル](https://github.com/dotnet/samples/blob/master/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true)をダウンロードして解凍します。

    ZIP ファイルには次のものが含まれています。

    * `saved_model.pb`: TensorFlow モデルそのもの。 このモデルは、IMDB レビューの文字列内のテキストを表すフィーチャーの固定長 (サイズ 600) の整数配列を受け取り、合計が 1 になる 2 つの確率を出力します。これは、入力レビューに肯定的センチメントが含まれる確率と、入力レビューに否定的センチメントが含まれる確率です。
    * `imdb_word_index.csv`: 個々の単語から整数値へのマッピング。 マッピングは、TensorFlow モデル用に入力機能を作成するために使用されます。

2. 最深部の `sentiment_model` ディレクトリの内容を *TextClassificationTF* プロジェクトの `sentiment_model` ディレクトリにコピーします。 次の画像に示すように、このディレクトリには、このチュートリアルに必要なモデルと追加のサポート ファイルが含まれています。

   ![sentiment_model ディレクトリ コンテンツ](./media/text-classification-tf/sentiment-model-files.png)

3. ソリューション エクスプローラーで、`sentiment_model` ディレクトリとサブディレクトリ内の各ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

### <a name="add-using-statements-and-global-variables"></a>using ステートメントとグローバル変数を追加する

1. 次に示す追加の `using` ステートメントを *Program.cs* ファイルの先頭に追加します。

   [!code-csharp[AddUsings](../../../samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#AddUsings "Add necessary usings")]

1. 保存したモデルのファイル パスとフィーチャーのベクターの長さを保持するために、`Main` メソッドの真上にグローバル変数を 2 つ作成します。

   [!code-csharp[DeclareGlobalVariables](../../../samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

    * `_modelPath` は、トレーニング済みモデルのファイル パスです。
    * `FeatureLength` は、モデルで想定している整数フィーチャー配列の長さです。

### <a name="model-the-data"></a>データのモデル化

映画レビューは自由形式のテキストです。 アプリケーションでは、テキストがさまざまな個別のステージでモデルによって想定される入力形式に変換されます。

最初にテキストを別々の単語に分割し、指定されたマッピング ファイルを使用して各単語を整数のエンコードにマップします。 この変換の結果、文章内の単語数に応じた長さの可変長の整数配列になります。

|プロパティ| [値]|種類|
|-------------|-----------------------|------|
|ReviewText|この映画は本当に良い|string|
|VariableLengthFeatures|14、22、9、66、78、... |int[]|

可変長フィーチャーの配列は、固定長が 600 に変更されます。 これは、TensorFlow モデルで想定される長さです。

|プロパティ| [値]|種類|
|-------------|-----------------------|------|
|ReviewText|この映画は本当に良い|string|
|VariableLengthFeatures|14、22、9、66、78、... |int[]|
|フィーチャー|14、22、9、66、78、... |int[600]|

1. `Main` メソッドの後に、入力データ用のクラスを作成します。

    [!code-csharp[MovieReviewClass](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#MovieReviewClass "Declare movie review type")]

    入力データ クラス (`MovieReview`) には、ユーザー コメント (`ReviewText`) 用の `string` があります。

1. 次の `Main` メソッドの後に、可変長フィーチャー用のクラスを作成します。

    [!code-csharp[VariableLengthFeatures](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#VariableLengthFeatures "Declare variable length features type")]

    `VariableLengthFeatures` プロパティには、ベクターとして指定する [VectorType](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) 属性が含まれます。  ベクター要素はすべて同じ型である必要があります。 列が多いデータ セットでは、複数の列を 1 つのベクターとして読み込むことで、データ変換を適用するときのデータ パスの数が減少します。

    このクラスは、`ResizeFeatures` アクションで使用されます。 プロパティの名前 (この場合は 1 つのみ) を使用して、カスタム マッピング アクションへの_入力_として使用できる DataView 内の列を示します。

1. 次の `Main` メソッドの後に、固定長フィーチャー用のクラスを作成します。

    [!code-csharp[FixedLengthFeatures](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#FixedLengthFeatures)]

    このクラスは、`ResizeFeatures` アクションで使用されます。 プロパティの名前 (この場合は 1 つのみ) を使用して、カスタム マッピング アクションへの_出力_として使用できる DataView 内の列を示します。

    プロパティ `Features` の名前は、TensorFlow モデルによって決定されることに注意してください。 このプロパティ名は変更できません。

1. 次の `Main` メソッドの後に、予測用のクラスを作成します。

    [!code-csharp[Prediction](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#Prediction "Declare prediction class")]

    `MovieReviewSentimentPrediction` はモデルのトレーニング後に使用される予測クラスです。 `MovieReviewSentimentPrediction` には、1 つの `float` 配列 (`Prediction`) と `VectorType` 属性が含まれます。

### <a name="create-the-mlcontext-lookup-dictionary-and-action-to-resize-features"></a>MLContext、検索ディクショナリ、フィーチャーのサイズを変更するアクションを作成する

[MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の始点です。 `mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

1. `Main` メソッドの `Console.WriteLine("Hello World!")` の行は、mlContext 変数を宣言して初期化する次のコードに置き換えます。

   [!code-csharp[CreateMLContext](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CreateMLContext "Create the ML Context")]

1. 次の表に示すように、[`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) メソッドを使用して単語を整数としてエンコードするディクショナリを作成し、ファイルからマッピング データを読み込みます。

    |単語     |インデックス    |
    |---------|---------|
    |子供向け     |  362    |
    |必要     |  181    |
    |不適切    |  355    |
    |効果  |  302    |
    |感覚  |  547    |

    次のコードを追加して、検索マップを作成します。

    [!code-csharp[CreateLookupMap](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CreateLookupMap)]

1. `Action` を追加して、可変長単語の整数配列のサイズを固定サイズの整数配列に変更します。次のコード行を使用します。

   [!code-csharp[ResizeFeatures](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#ResizeFeatures)]

## <a name="load-the-pre-trained-tensorflow-model"></a>事前トレーニング済みの TensorFlow モデルを読み込む

1. TensorFlow モデルを読み込むためのコードを追加します。

    [!code-csharp[LoadTensorFlowModel](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#LoadTensorFlowModel)]

    モデルが読み込まれたら、その入力および出力スキーマを抽出できます。 スキーマは、参考および学習の目的のためにのみ表示されます。 最終的なアプリケーションを機能させるためには、このコードは必要ありません。

    [!code-csharp[GetModelSchema](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#GetModelSchema)]

    入力スキーマは、整数でエンコードされた単語の固定長配列です。 出力スキーマは、レビューのセンチメントが否定的であるか、肯定的であるかを示す、確率の float 型配列です。 肯定的な確率は否定的なセンチメントの確率の補数であるため、これらの値の合計は 1 になります。

## <a name="create-the-mlnet-pipeline"></a>ML.NET パイプラインを作成する

1. パイプラインを作成し、[TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 変換を使用して入力テキストを単語に分割し、次のコード行としてテキストを単語に分割します。

   [!code-csharp[TokenizeIntoWords](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#TokenizeIntoWords)]

   [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 変換では、スペースを使用してテキストまたは文字列を単語に解析します。 これにより、新しい列が作成され、ユーザー定義の区切り記号に基づいて各入力文字列を部分文字列のベクターに分割します。

1. 上記で宣言した参照テーブルを使用して、単語を整数エンコードにマップします。

    [!code-csharp[MapValue](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#MapValue)]

1. 可変長の整数エンコーディングのサイズを、モデルで必要とされる固定長に変更します。

    [!code-csharp[CustomMapping](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CustomMapping)]

1. 読み込まれた TensorFlow モデルで入力を分類します。

    [!code-csharp[ScoreTensorFlowModel](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#ScoreTensorFlowModel)]

    TensorFlow モデルの出力は `Prediction/Softmax` と呼ばれます。 この名前 `Prediction/Softmax` は、TensorFlow モデルによって決定されることに注意してください。 この名前は変更できません。

1. 出力予測用に新しい列を作成します。

    [!code-csharp[SnippetCopyColumns](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#SnippetCopyColumns)]

    `Prediction/Softmax` 列を C# クラス `Prediction` のプロパティとして使用できる名前を持つ列にコピーする必要があります。 `/` 文字は、C# プロパティ名では使用できません。

## <a name="create-the-mlnet-model-from-the-pipeline"></a>パイプラインから ML.NET モデルを作成する

1. パイプラインからモデルを作成するためのコードを追加します。

    [!code-csharp[SnippetCreateModel](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#SnippetCreateModel)]

    ML.NET モデルは、`Fit` メソッドを呼び出すことによって、パイプラインのエスティメーターのチェーンから作成されます。 この場合、TensorFlow モデルは既に以前トレーニングされているため、モデルを作成するためのデータを調整することはありません。 `Fit` メソッドの要件を満たすために、空のデータ ビュー オブジェクトを提供します。

## <a name="use-the-model-to-make-a-prediction"></a>モデルを使用して予測する

1. `PredictSentiment` メソッドを `Main` メソッドの下に追加します。

    ```csharp
    public static void PredictSentiment(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. 次のコードを追加して、`PredictionEngine` を `PredictSentiment()` メソッドの 1 行目として作成します。

    [!code-csharp[CreatePredictionEngine](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CreatePredictionEngine)]

    [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

    > [!NOTE]
    > `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

1. コメントを追加して、`Predict()` メソッドでトレーニングされたモデルの予測をテストします。これには `MovieReview` のインスタンスを作成します。

    [!code-csharp[CreateTestData](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CreateTestData)]

1. `PredictSentiment()` メソッドに次のコード行を追加することで、テスト コメント データを `Prediction Engine` に渡します。

    [!code-csharp[Predict](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#Predict)]

1. [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 関数では、データの単一行に対して予測を行います。

    |プロパティ| [値]|種類|
    |-------------|-----------------------|------|
    |予測|[0.5459937, 0.454006255]|float[]|

1. 次のコードを使用して、センチメント予測を表示します。

    [!code-csharp[DisplayPredictions](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#DisplayPredictions)]

1. `Main` メソッドの末尾に、`PredictSentiment` に対する呼び出しを追加します。

    [!code-csharp[CallPredictSentiment](~/samples/snippets/machine-learning/TextClassificationTF/csharp/Program.cs#CallPredictSentiment)]

## <a name="results"></a>結果

アプリケーションをビルドして実行します。

結果は以下のようになるはずです。 処理中にメッセージが表示されます。 警告または処理メッセージが表示されることがありますが、 わかりやすくするために、これらのメッセージは次の結果から削除してあります。

```console
Number of classes: 2
Is sentiment/review positive ? Yes
```

おめでとうございます! これで、ML.NET で事前トレーニング済みの `TensorFlow` モデルを再利用することにより、メッセージのセンチメントを分類および予測するための機械学習モデルをビルドできました。

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF) リポジトリで確認できます。

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> * 事前トレーニング済みの TensorFlow モデルを読み込む
> * Web サイトのコメント テキストをモデルに適したフィーチャーに変換する
> * モデルを使用して予測する
