---
title: 'チュートリアル: TensorFlow からの ML.NET 画像分類モデル'
description: 既存の TensorFlow モデルから新しい ML.NET 画像分類モデルに知識を転移する方法について説明します。 TensorFlow モデルは、画像を 1,000 個のカテゴリに分類するためにトレーニングされました。 ML.NET モデルでは、転移学習を利用して、さらに少ない数のカテゴリに画像を分類します。
ms.date: 01/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: 1e5478f53c82f36ddafe19e3659e2234ff9687b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241027"
---
# <a name="tutorial-generate-an-mlnet-image-classification-model-from-a-pre-trained-tensorflow-model"></a>チュートリアル: 事前トレーニング済みの TensorFlow モデルから ML.NET 画像分類モデルを生成する

既存の TensorFlow モデルから新しい ML.NET 画像分類モデルに知識を転移する方法について説明します。

TensorFlow モデルは、画像を 1,000 個のカテゴリに分類するためにトレーニングされました。 ML.NET モデルでは、パイプラインで TensorFlow モデルの一部を利用し、モデルをトレーニングし、画像を 3 つのカテゴリに分類しています。

[画像分類](https://en.wikipedia.org/wiki/Outline_of_object_recognition)モデルを最初からトレーニングするには、数百万のパラメーター、大量のラベル付きトレーニング データ、膨大な量の計算リソース (数百時間の GPU) を設定する必要があります。 最初からカスタム モデルをトレーニングするほど効果的ではありませんが、転移学習を使用すると、何千もの画像と何百万ものラベル付き画像を使用し、カスタマイズされたモデルを短時間 (GPU が搭載されていないマシンで 1 時間以内) で構築できます。 このチュートリアルでは、1 ダースのトレーニング画像のみを使用して、そのプロセスをさらにスケールダウンします。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * 問題を把握する
> * 事前トレーニング済みの TensorFlow モデルを ML.NET パイプラインに組み込む
> * ML.NET モデルのトレーニングと評価
> * テスト画像を分類する

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TransferLearningTF) リポジトリで確認できます。 既定では、このチュートリアルの .NET プロジェクトの構成は .NET Core 2.2 を対象としています。

## <a name="what-is-transfer-learning"></a>転移学習とは何か

転移学習は、1 つの問題を解決し、それを別の関連する問題に適用するときに得た知識を使用するプロセスです。

このチュートリアルでは、TensorFlow モデル (画像を 1,000 個のカテゴリに分類するようにトレーニング済み) の一部を、画像を 3 つのカテゴリに分類する ML.NET モデルで使用します。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。
* [チュートリアル資産ディレクトリの .ZIP ファイル](https://github.com/dotnet/samples/blob/master/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)
* [InceptionV1 機械学習モデル](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)

## <a name="select-the-right-machine-learning-task"></a>適切な機械学習タスクを選択する

### <a name="deep-learning"></a>ディープ ラーニング

[ディープ ラーニング](https://en.wikipedia.org/wiki/Deep_learning)は、Computer Vision や音声認識などの分野に革命を起こしている Machine Learning のサブセットです。

ディープ ラーニング モデルは、複数の学習レイヤーを含む多数の[ラベル付きデータ](https://en.wikipedia.org/wiki/Labeled_data)と[ニューラル ネットワーク](https://en.wikipedia.org/wiki/Artificial_neural_network)を使用してトレーニングされます。 ディープ ラーニング:

* Computer Vision などの一部のタスクでより効果的に機能します。
* 大量のトレーニング データが必要です。

画像分類は、画像を次のようなカテゴリに自動的に分類することができる一般的な機械学習タスクです。

* 画像から人の顔を検出するかどうか。
* 猫と犬の検出。

 または、次の画像のように、画像が食品、おもちゃ、または電化製品のいずれであるかを判断する場合。

![ピザの画像](./media/image-classification/220px-Pepperoni_pizza.jpg)
![テディ ベアの画像](./media/image-classification/119px-Nalle_-_a_small_brown_teddy_bear.jpg)
![トースターの画像](./media/image-classification/193px-Broodrooster.jpg)

>[!Note]
> 上の画像は Wikimedia Commons に属し、次のように属性が付けられています。
>
> * "220px-Pepperoni_pizza.jpg" パブリック ドメイン、 https://commons.wikimedia.org/w/index.php?curid=79505 、
> * "119px-Nalle_-_a_small_brown_teddy_bear.jpg" By [Jonik](https://commons.wikimedia.org/wiki/User:Jonik) - 自己撮影、CC BY-SA 2.0、 https://commons.wikimedia.org/w/index.php?curid=48166 。
> * "193px-Broodrooster.jpg" By [M.Minderhoud](https://nl.wikipedia.org/wiki/Gebruiker:Michiel1972) - 自分の作品、CC BY-SA 3.0、 https://commons.wikimedia.org/w/index.php?curid=27403

`Inception model` は、画像を 1,000 個のカテゴリに分類するようにトレーニングされていますが、このチュートリアルでは、画像をより小さなカテゴリ セットにのみ分類する必要があります。 `transfer learning` の `transfer` 部分を入力します。 `Inception model` の画像を認識および分類する能力をカスタム画像分類器の新しい限られたカテゴリに転移ことができます。

* 食品
* おもちゃ
* アプライアンス

このチュートリアルでは、TensorFlow の [Inception モデル](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) ディープ ラーニング モデルを使用します。これは、`ImageNet` データセットに対してトレーニングされた、よく使われる画像認識モデルです。 TensorFlow モデルでは、画像全体を "傘"、"ジャージー"、"食器洗い機" などの 1,000 個のクラスに分類します。

`Inception model` は何千種類もの画像に対して事前にトレーニングされているため、画像の識別に必要な[画像の特徴](https://en.wikipedia.org/wiki/Feature_(computer_vision))が内部に含まれています。 モデルでこれらの内部画像機能を利用して、はるかに少数のクラスで新しいモデルをトレーニングすることができます。

次の図に示すように、.NET Core または .NET Framework アプリケーションに ML.NET NuGet パッケージへの参照を追加します。 内部的には、既存のトレーニング済み `TensorFlow` モデル ファイルを読み込むコードを作成できるネイティブ `TensorFlow` ライブラリが ML.NET には含まれ、参照されています。

![TensorFlow 変換 ML.NET Arch 図](./media/image-classification/tensorflow-mlnet.png)

### <a name="multiclass-classification"></a>多クラス分類

TensorFlow Inception モデルを使用して、従来の機械学習アルゴリズムの入力に適した特徴を抽出したら、ML.NET の[多クラス分類器](../resources/tasks.md#multiclass-classification)を追加します。

この場合に使用される特定のトレーナーは、[多項ロジスティック回帰アルゴリズム](https://en.wikipedia.org/wiki/Multinomial_logistic_regression)です。

このトレーナーに実装されているアルゴリズムは、多数の特徴を持つ問題 (画像データに対して機能するディープ ラーニング モデルのケース) に適しています。

### <a name="data"></a>データ

データ ソースは 2 つあります。`.tsv` ファイルと画像ファイルです。  `tags.tsv` ファイルには 2 つの列があります。最初の列は `ImagePath` と定義され、2 番目の列は画像に対応する `Label` です。 次のファイル例にはヘッダー行がなく、次のような内容です。

<!-- markdownlint-disable MD010 -->
```tsv
broccoli.jpg    food
pizza.jpg   food
pizza2.jpg  food
teddy2.jpg  toy
teddy3.jpg  toy
teddy4.jpg  toy
toaster.jpg appliance
toaster2.png    appliance
```
<!-- markdownlint-enable MD010 -->

トレーニングとテストの画像は、zip ファイルでダウンロードする資産フォルダー内にあります。 これらの画像は Wikimedia Commons に属します。
> *[Wikimedia Commons](https://commons.wikimedia.org/w/index.php?title=Main_Page&oldid=313158208)、無料メディア リポジトリ。* 2018 年 10 月 17 日 10:48 に以下から取得: https://commons.wikimedia.org/wiki/Pizza https://commons.wikimedia.org/wiki/Toaster https://commons.wikimedia.org/wiki/Teddy_bear

## <a name="setup"></a>セットアップ

### <a name="create-a-project"></a>プロジェクトを作成する

1. "TransferLearningTF" という **.NET Core Console アプリケーション**を作成します。

1. **Microsoft.ML NuGet パッケージ**をインストールします。

    * ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    * [パッケージ ソース] として "nuget.org" を選択し、[参照] タブを選択し、"**Microsoft.ML**" を検索します。
    * **[バージョン]** ドロップダウンをクリックし、一覧から **1.4.0** パッケージを選択し、 **[インストール]** ボタンを選択します。
    * **[変更のプレビュー]** ダイアログで **[OK]** ボタンを選択します。
    * 一覧表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスへの同意]** ダイアログで **[同意する]** ボタンを選択します。
    * **Microsoft.ML.ImageAnalytics v1.4.0**、**SciSharp.TensorFlow.Redist v1.15.0**、**Microsoft.ML.TensorFlow v1.4.0** にこれらの手順を繰り返します。

### <a name="download-assets"></a>資産をダウンロードする

1. [プロジェクト資産ディレクトリの zip ファイル](https://github.com/dotnet/samples/blob/master/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)をダウンロードし、展開します。

1. `assets` ディレクトリを *TransferLearningTF* プロジェクト ディレクトリにコピーします。 このディレクトリとそのサブディレクトリには、このチュートリアルに必要なデータ ファイルとサポート ファイル (次の手順でダウンロードして追加する Inception モデルを除く) が含まれています。

1. [Inception モデル](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)をダウンロードして展開します。

1. `inception5h` ディレクトリの内容をコピーし、*TransferLearningTF* プロジェクトの `assets/inception` ディレクトリに展開します。 次の画像に示すように、このディレクトリには、このチュートリアルに必要なモデルと追加のサポート ファイルが含まれています。

   ![Inception ディレクトリの内容](./media/image-classification/inception-files.png)

1. ソリューション エクスプローラーで、資産ディレクトリとサブディレクトリ内の各ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

### <a name="create-classes-and-define-paths"></a>クラスを作成してパスを定義する

1. 次に示す追加の `using` ステートメントを *Program.cs* ファイルの先頭に追加します。

    [!code-csharp[AddUsings](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#AddUsings)]

1. `Main` メソッドのすぐ上にある行に次のコードを追加して、資産のパスを指定します。

    [!code-csharp[DeclareGlobalVariables](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DeclareGlobalVariables)]

1. 入力データと予測のためにクラスを作成します。

    [!code-csharp[DeclareImageData](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DeclareImageData)]

    `ImageData` は、入力画像データ クラスであり、次の <xref:System.String> フィールドがあります。

    * `ImagePath` には、画像ファイル名が含まれます。
    * `Label` には、画像ラベルの値が含まれます。

1. `ImagePrediction` のプロジェクトに新しいクラスを追加します。

    [!code-csharp[DeclareImagePrediction](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DeclareImagePrediction)]

    `ImagePrediction` は、画像予測クラスであり、次のフィールドがあります。

    * `Score` には、指定した画像分類の信頼度が含まれています。
    * `PredictedLabelValue` には、予測された画像分類ラベルの値が含まれています。

    `ImagePrediction` は、モデルがトレーニングされた後に、予測に使用されるクラスです。 これには画像パスの `string` (`ImagePath`) が含まれます。 `Label` はモデルの再利用とトレーニングに使用されます。 `PredictedLabelValue` は予測と評価の際に使用されます。 評価では、トレーニング データを含む入力、予測された値、およびモデルが使用されます。

### <a name="initialize-variables-in-main"></a>Main で変数を初期化する

1. 新しいインスタンスの `MLContext` を使用して `mlContext` 変数を初期化します。  `Main` メソッドの `Console.WriteLine("Hello World!")` 行を、次のコードで置き換えます。

    [!code-csharp[CreateMLContext](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#CreateMLContext)]

    [MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の開始点で、`mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

### <a name="create-a-struct-for-inception-model-parameters"></a>Inception モデル パラメーターの構造体を作成する

1. Inception モデルには、渡す必要があるパラメーターがいくつかあります。 次のコードを使用して、`Main()` メソッドの直後に、パラメーター値をフレンドリ名にマップする構造体を作成します。

    [!code-csharp[InceptionSettings](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#InceptionSettings)]

### <a name="create-a-display-utility-method"></a>表示ユーティリティ メソッドを作成する

画像データとそれに関連する予測を複数回表示するので、画像と予測結果の表示を処理する表示ユーティリティ メソッドを作成します。

1. `InceptionSettings` 構造体の直後に、次のコードを使用して `DisplayResults()` メソッドを作成します。

    ```csharp
    private static void DisplayResults(IEnumerable<ImagePrediction> imagePredictionData)
    {

    }
    ```

1. `DisplayResults` メソッドの本体を入力します。

    [!code-csharp[DisplayPredictions](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DisplayPredictions)]

### <a name="create-a-tsv-file-utility-method"></a>.tsv ファイル ユーティリティ メソッドを作成する

1. `DisplayResults()` メソッドの直後に、次のコードを使用して `ReadFromTsv()` メソッドを作成します。

    ```csharp
    public static IEnumerable<ImageData> ReadFromTsv(string file, string folder)
    {

    }
    ```

1. `ReadFromTsv` メソッドの本体を入力します。

    [!code-csharp[ReadFromTsv](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#ReadFromTsv)]

    このコードでは、`tags.tsv` ファイルを解析して、ファイルパスを `ImagePath` プロパティの画像ファイル名に追加し、それと `Label` を `ImageData` オブジェクトに読み込みます。

### <a name="create-a-method-to-make-a-prediction"></a>予測を行うメソッドを作成する

1. `DisplayResults()` メソッドの直前に、次のコードを使用して `ClassifySingleImage()` メソッドを作成します。

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. 1 つの `ImagePath` の完全修飾パスと画像ファイル名を含む `ImageData` オブジェクトを作成します。 `ClassifySingleImage()` メソッドの次の行として、次のコードを追加します。

    [!code-csharp[LoadImageData](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#LoadImageData)]

1. 次のコードを `ClassifySingleImage` メソッドの次の行として追加して、1 つの予測を作成します。

    [!code-csharp[PredictSingle](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#PredictSingle)]

    予測を取得するには、[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) メソッドを使用します。 [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

    > [!NOTE]
    > `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

1. `ClassifySingleImage()` メソッドの次のコード行として予測結果を表示します。

   [!code-csharp[DisplayPrediction](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DisplayPrediction)]

## <a name="construct-the-mlnet-model-pipeline"></a>ML.NET モデルのパイプラインを構築する

ML.NET モデルのパイプラインは推定器のチェーンです。 パイプラインの構築中に実行は発生しないことに注意してください。 推定器オブジェクトは作成されますが、実行されません。

1. モデルを生成するメソッドを追加する

    このメソッドは、このチュートリアルの中核となるものです。 モデルのパイプラインを作成し、パイプラインをトレーニングして ML.NET モデルを生成します。 また、未認識のテスト データに対してモデルを評価します。

    `InceptionSettings` 構造体の直後、かつ `DisplayResults()` メソッドの直前に、次のコードを使用して `GenerateModel()` メソッドを作成します。

    ```csharp
    public static ITransformer GenerateModel(MLContext mlContext)
    {

    }
    ```

1. 画像データを読み込み、サイズを変更し、ピクセルを抽出する推定器を追加します。

    [!code-csharp[ImageTransforms](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#ImageTransforms)]

    画像データは、TensorFlow モデルで想定されている形式に処理する必要があります。 この場合、画像はメモリに読み込まれ、サイズが一貫したサイズに変更され、ピクセルが数値ベクターに抽出されます。

1. TensorFlow モデルを読み込み、評価する推定器を追加します。

    [!code-csharp[ScoreTensorFlowModel](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#ScoreTensorFlowModel)]

    パイプラインのこのステージでは、TensorFlow モデルをメモリに読み込み、次に TensorFlow モデル ネットワークを介してピクセル値のベクターを処理します。 ディープ ラーニング モデルに入力を適用し、モデルを使用して出力を生成することは、**スコアリング**と呼ばれます。 モデル全体を使用する場合、スコアリングによって推論または予測が行われます。

    この場合、最後のレイヤー (推論を行うレイヤー) を除き、すべての TensorFlow モデルを使用します。 最後から 2 番目のレイヤーの出力には、`softmax_2_preactivation` というラベルが付けられます。 このレイヤーの出力は、実質的に、元の入力画像を特徴付ける特徴のベクターです。

    TensorFlow モデルによって生成されるこの特徴ベクターは、ML.NET トレーニング アルゴリズムへの入力として使用されます。

1. トレーニング データの文字列ラベルを整数キー値にマップする推定器を追加します。

    [!code-csharp[MapValueToKey](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#MapValueToKey)]

    次に追加される ML.NET トレーナーでは、ラベルが任意の文字列ではなく `key` 形式である必要があります。 キーは、文字列値への 1 対 1 のマッピングを持つ数値です。

1. ML.NET トレーニング アルゴリズムを追加します。

    [!code-csharp[AddTrainer](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#AddTrainer)]

1. 予測されたキー値を元の文字列にマップする推定器を追加します。

    [!code-csharp[MapKeyToValue](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#MapKeyToValue)]

## <a name="train-the-model"></a>モデルをトレーニングする

1. [LoadFromTextFile](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile(Microsoft.ML.DataOperationsCatalog,System.String,Microsoft.ML.Data.TextLoader.Options)) ラッパーを使用してトレーニング データを読み込みます。 `GenerateModel()` メソッドに次のコード行を追加します。

    [!code-csharp[LoadData](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#LoadData "Load the data")]

    ML.NET 内のデータは、[IDataView クラス](xref:Microsoft.ML.IDataView)として表されます。 `IDataView` は、表形式のデータ (数値とテキスト) を表すための柔軟で効率的な方法です。 データはテキスト ファイルから、またはリアルタイムで (SQL データベースやログ ファイルなど) `IDataView` オブジェクトに読み込むことができます。

1. 前の手順で読み込まれたデータを使用してモデルをトレーニングします。

    [!code-csharp[TrainModel](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#TrainModel)]

    `Fit()` メソッドで、トレーニング データセットをパイプラインに適用してモデルをトレーニングします。

## <a name="evaluate-the-accuracy-of-the-model"></a>モデルの精度を評価する

1. `GenerateModel` メソッドの次の行に次のコードを追加して、テスト データを読み込んで変換します。

    [!code-csharp[LoadAndTransformTestData](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#LoadAndTransformTestData "Load and transform test data")]

    モデルの評価に使用できるサンプル画像がいくつかあります。 トレーニング データと同様に、モデルで変換できるように、これらを `IDataView` に読み込む必要があります。

1. `GenerateModel()` メソッドに次のコードを追加して、モデルを評価します。

    [!code-csharp[Evaluate](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#Evaluate)]

    予測セットを作成したら、[Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) メソッドを呼び出します。

    * モデルを評価します (予測値とテスト データセット `labels` を比較します)。
    * モデルのパフォーマンス メトリックを返します。

1. モデルの精度のメトリックを表示する

    次のコードを使用してメトリックを表示し、結果を共有し、それに対してアクションを実行します。

    [!code-csharp[DisplayMetrics](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#DisplayMetrics)]

    画像分類では、次のメトリックが評価されます。

    * `Log-loss` - 「[対数損失](../resources/glossary.md#log-loss)」を参照してください。 対数損失は可能な限り 1 に近づけます。
    * `Per class Log-loss`。 クラスごとの対数損失は可能な限り 1 に近づけます。

1. 次の行としてトレーニング済みモデルを返すように次のコードを追加します。

    [!code-csharp[SaveModel](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#ReturnModel)]

## <a name="run-the-application"></a>アプリケーションを実行します。

1. MLContext クラスを作成した後、`Main` メソッドに `GenerateModel` の呼び出しを追加します。

    [!code-csharp[CallGenerateModel](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#CallGenerateModel)]

1. `Main` メソッドの次のコード行として、`ClassifySingleImage()` メソッドの呼び出しを追加します。

    [!code-csharp[CallClassifySingleImage](../../../samples/snippets/machine-learning/TransferLearningTF/csharp/Program.cs#CallClassifySingleImage)]

1. コンソール アプリを実行します (Ctrl + F5 キー)。 結果は以下の出力のようになるはずです。  警告メッセージまたは処理中のメッセージが表示される場合がありますが、わかりやすくするため、これらのメッセージは結果から削除してあります。

    ```console
    =============== Training classification model ===============
    Image: broccoli2.jpg predicted as: food with score: 0.8955513
    Image: pizza3.jpg predicted as: food with score: 0.9667718
    Image: teddy6.jpg predicted as: toy with score: 0.9797683
    =============== Classification metrics ===============
    LogLoss is: 0.0653774699265059
    PerClassLogLoss is: 0.110315812569315 , 0.0204391272836966 , 0
    =============== Making single image classification ===============
    Image: toaster3.jpg predicted as: appliance with score: 0.9646884
    ```

おめでとうございます! これで、ML.NET で `TensorFlow` モデルに転移学習を適用して、画像分類の機械学習モデルを構築できました。

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TransferLearningTF) リポジトリで確認できます。

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> * 問題を把握する
> * 事前トレーニング済みの TensorFlow モデルを ML.NET パイプラインに組み込む
> * ML.NET モデルのトレーニングと評価
> * テスト画像を分類する

Machine Learning サンプルの GitHub リポジトリを確認し、拡張された画像分類サンプルを探索してください。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub リポジトリ](https://github.com/dotnet/machinelearning-samples/)
