---
title: 'チュートリアル: 転移学習を利用した自動ビジュアル検査'
description: このチュートリアルでは、転移学習を利用し、ML.NET で TensorFlow ディープ ラーニング モデルをトレーニングする方法を説明します。具体的には、画像検出 API を利用し、コンクリートの表面の画像をひび割れあり/ひび割れなしに分類します。
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 92c768c429a2eae22071556cc13c7457a7cf26df
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514516"
---
# <a name="tutorial-automated-visual-inspection-using-transfer-learning-with-the-mlnet-image-classification-api"></a>チュートリアル: 転移学習と ML.NET Image Classification API を利用した自動ビジュアル検査

転移学習、事前トレーニング済みの TensorFlow モデル、ML.NET Image Classification API を利用してカスタム ディープ ラーニング モデルをトレーニングし、コンクリートの表面の画像をひび割れあり/ひび割れなしに分類する方法について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - 問題を把握する
> - ML.NET Image Classification API について
> - 事前トレーニング済みモデルについて
> - 転移学習を利用し、カスタム TensorFlow 画像分類モデルをトレーニングする
> - カスタム モデルによる画像の分類

## <a name="prerequisites"></a>必須コンポーネント

- ".NET Core クロスプラットフォーム開発" ワークロードがインストールされた [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 以降または Visual Studio 2017 バージョン 15.6 以降。

## <a name="image-classification-transfer-learning-sample-overview"></a>画像分類の転移学習の概要

このサンプルは、事前トレーニング済みディープ ラーニング TensorFlow モデルを利用して画像を分類する C# .NET Core コンソール アプリケーションです。 このサンプルのコードは[サンプル ブラウザー](/samples/dotnet/machinelearning-samples/mlnet-image-classification-transfer-learning/)にあります。

## <a name="understand-the-problem"></a>問題を把握する

画像分類はコンピューターのビジョンの問題です。 画像分類では、画像を入力として受け取り、指示されたクラスにそれを分類します。 画像分類モデルは、一般的にディープ ラーニングとニューラル ネットワークを使用してトレーニングされます。 詳細については、[ディープ ラーニングと機械学習の違い](/azure/machine-learning/concept-deep-learning-vs-machine-learning)に関するページを参照してください。

画像分類はたとえば次のシナリオで役立ちます。

- 顔認識
- 感情検出
- 医療診断
- 陸標検出

このチュートリアルでは、カスタム画像分類モデルをトレーニングし、橋床の映像を自動検査し、ひび割れしている構造物を特定します。

## <a name="mlnet-image-classification-api"></a>ML.NET Image Classification API

ML.NET からは、画像分類を実行するさまざまな方法が与えられます。 このチュートリアルでは、Image Classification API を使用する転移学習を応用します。 Image Classification API では [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET) を利用します。これは TensorFlow C++ API 用に C# バインディングを提供する低レベル ライブラリです。

## <a name="what-is-transfer-learning"></a>転移学習とは何か

転移学習では、ある問題を解決することで得られた知識が関連する別の問題に応用されます。

ディープ ラーニング モデルを最初からトレーニングするには、いくつかのパラメーター、大量のラベル付きトレーニング データ、膨大な量の計算リソース (数百時間の GPU) を設定する必要があります。 事前トレーニング済みモデルと転移学習を使用すると、トレーニング プロセスをショートカットできます。

## <a name="training-process"></a>トレーニング プロセス

Image Classification API のトレーニング プロセスは、事前トレーニング済み TensorFlow モデルを読み込むことから始まります。 トレーニング プロセスは次の 2 つのステップで構成されます。

1. ボトルネック フェーズ
2. トレーニング フェーズ

![トレーニング ステップ](./media/image-classification-api-transfer-learning/training.png)

### <a name="bottleneck-phase"></a>ボトルネック フェーズ

ボトルネック フェーズの間、事前トレーニング済みモデルの固定レイヤーに対して、一連のトレーニング画像が読み込まれ、ピクセル値が入力として使用されます。 固定レイヤーには、非公式にボトルネック レイヤーと呼ばれている最後から 2 番目のレイヤーまでに含まれる、ニューラル ネットワーク内のあらゆるレイヤーが含まれます。 このようなレイヤーは、その上でトレーニングが発生せず、操作がパススルーであるため、固定と呼ばれています。 モデルでさまざまなクラスを区別するのに役立つ低レベル パターンが計算されるのがこの固定レイヤーです。 レイヤーの数が多ければ多いほど、この手順は計算処理がそれだけ激しくなります。 幸いなことに、これは 1 回限りの計算であるため、結果をキャッシュし、後で異なるパラメーターを使用して実験するときに利用できます。

### <a name="training-phase"></a>トレーニング フェーズ

ボトルネック フェーズからの出力値が計算されると、それが入力として利用され、モデルの最終的レイヤーが保持されます。 このプロセスは繰り返され、モデル パラメーターで指定された回数だけ実行されます。 実行のたびに、損失と精度が評価されます。 次に、損失を最小限に抑え、精度を最大限に高めることを目的としてモデルを改善するための適切な調整が行われます。 トレーニングが完了すると、2 つのモデル形式が出力されます。 そのうちの 1 つがモデルの `.pb` バージョンであり、もう 1 つがモデルの `.zip` ML.NET シリアル化バージョンです。 ML.NET でサポートされる環境で作業するとき、モデルの `.zip` バージョンを使用することが推奨されます。 ただし、ML.NET がサポートされない環境では、`.pb` バージョンを使用できます。

## <a name="understand-the-pretrained-model"></a>事前トレーニング済みモデルについて

このチュートリアルで使用される事前トレーニング済みモデルは、Residual Network (ResNet) v2 モデルの 101 レイヤー型です。 元のモデルは、画像を 1,000 個のカテゴリに分類する目的でトレーニングされています。 このモデルでは、サイズ 224 x 224 の画像を入力として受け取り、トレーニングしたクラス別にクラス確率を出力します。 このモデルの一部は、2 つのクラス間で予測する目的でカスタム画像を利用して新しいモデルをトレーニングするために使用されます。

## <a name="create-console-application"></a>コンソール アプリケーションを作成する

転移学習と Image Classification API の概要を理解できたところで、アプリケーションを構築しましょう。

1. "DeepLearning_ImageClassification_Binary" という名前の **C# .NET Core コンソール アプリケーション** を作成します。
1. **Microsoft.ML** NuGet パッケージをインストールします。

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    1. [パッケージ ソース] として [nuget.org] を選択します。
    1. **[参照]** タブを選択します。
    1. **[プレリリースを含める]** チェックボックスをオンにします。
    1. **Microsoft.ML** を探します。
    1. **[インストール]** ボタンを選択します。
    1. **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。
    1. **Microsoft.ML.Vision**、**SciSharp.TensorFlow.Redist**、および **Microsoft.ML.ImageAnalytics** NuGet パッケージに対して、これらの手順を繰り返します。

### <a name="prepare-and-understand-the-data"></a>データを準備して理解する

> [!NOTE]
> このチュートリアルのデータセットの提供元: Maguire, Marc; Dorafshan, Sattar; and Thomas, Robert J., "SDNET2018:A concrete crack image dataset for machine learning applications" (2018). Browse all Datasets. Paper 48. <https://digitalcommons.usu.edu/all_datasets/48>

SDNET2018 は、ひび割れあり/ひび割れなしのコンクリート構造物 (橋床、壁、歩道) に関する注釈を含む画像データセットです。

![SDNET2018 データセットの橋床のサンプル](./media/image-classification-api-transfer-learning/sdnet2018decksamples.png)

データは 3 つのサブディレクトリで整理されています。

- D には橋床の画像が含まれています
- P には歩道の画像が含まれています
- W には壁の画像が含まれています

これらのサブディレクトリにはそれぞれ、追加のサブディレクトリが 2 つ含まれ、プレフィックスが付けられています。

- C はひび割れありの表面に使用されるプレフィックスです
- U はひび割れなしの表面に使用されるプレフィックスです

このチュートリアルでは、橋床の画像のみ使用します。

1. [データセット](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/assets.zip)をダウンロードし、解凍します。
1. データ セット ファイルを保存するために、プロジェクトに "assets" という名前のディレクトリを作成します。
1. 先ほど解凍したディレクトリから *assets* ディレクトリに *CD* サブディレクトリと *UD* サブディレクトリをコピーします。

### <a name="create-input-and-output-classes"></a>入力クラスと出力クラスを作成する

1. *Program.cs* ファイルを開き、ファイルの先頭にある既存の `using` ステートメントを次で置き換えます。

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L1-L7)]

1. *Program.cs* の `Program` クラスの下で `ImageData` という名前のクラスを作成します。 このクラスは、最初に読み込んだデータを表わすために使用されます。

    [!code-csharp [ImageDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L137-L142)]

    `ImageData` には次のプロパティが含まれます。

    - `ImagePath` は、画像が保存されている完全修飾パスです。
    - `Label` は、画像が属するカテゴリです。 これが予測する値です。

1. 入力データと出力データのクラスを作成する

    1. `ImageData` クラスの下で、`ModelInput` という名前の新しいクラスに入力データのスキーマを定義します。

        [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L144-L153)]

        `ModelInput` には次のプロパティが含まれます。

        - `Image` は画像の `byte[]` 表現です。 モデルでは、トレーニングのために画像データがこの種類になることが求められます。
        - `LabelAsKey` は `Label` の数値表現です。
        - `ImagePath` は、画像が保存されている完全修飾パスです。
        - `Label` は、画像が属するカテゴリです。 これが予測する値です。

        `Image` と `LabelAsKey` のみ、モデルのトレーニングと予測に使用されます。 `ImagePath` プロパティと `Label` プロパティは、元の画像ファイルの名前やカテゴリにアクセスするときに便利なため、維持されます。

    1. 次に、`ModelInput` クラスの下で、`ModelOutput` という名前の新しいクラスに出力データのスキーマを定義します。

        [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L155-L162)]

        `ModelOutput` には次のプロパティが含まれます。

        - `ImagePath` は、画像が保存されている完全修飾パスです。
        - `Label` は、画像が属する元のカテゴリです。 これが予測する値です。
        - `PredictedLabel` はモデルで予測された値です。

        `ModelInput` と同様に、予測には `PredictedLabel` のみが必要になります。モデルによる予測が含まれているためです。 `ImagePath` プロパティと `Label` プロパティは、元の画像ファイルの名前やカテゴリにアクセスするときに便利なため、保持されます。

### <a name="create-workspace-directory"></a>ワークスペース ディレクトリを作成する

トレーニング データと検証データが頻繁に変更されない場合は、今後の実行のために、計算されたボトルネック値をキャッシュすることをお勧めします。

1. プロジェクトで *workspace* という名前の新しいディレクトリを作成し、計算されたボトルネック値とモデルの `.pb` バージョンを格納します。

### <a name="define-paths-and-initialize-variables"></a>パスを定義し、変数を初期化する

1. `Main` メソッド内で、アセットの場所、計算されたボトルネック値、モデルの `.pb` バージョンを定義します。

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L15-L17)]

1. [MLContext](xref:Microsoft.ML.MLContext) の新しいインスタンスを使用して `mlContext` 変数を初期化します。

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L19)]

    [MLContext](xref:Microsoft.ML.MLContext) クラスは、すべての ML.NET 操作の開始点で、mlContext を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DbContext` と概念的には同じです。

## <a name="load-the-data"></a>データを読み込む

### <a name="create-data-loading-utility-method"></a>データ読み込みユーティリティ メソッドを作成する

イメージは 2 つのサブディレクトリに保存されます。 データを読み込む前に、`ImageData` オブジェクトの一覧に書式設定する必要があります。 そのためには、`LoadImagesFromDirectory` メソッドを `Main` メソッドの下で作成します。

```csharp
public static IEnumerable<ImageData> LoadImagesFromDirectory(string folder, bool useFolderNameAsLabel = true)
{

}
```

1. `LoadImagesFromDirectory` 内で次のコードを追加し、サブディレクトリからすべてのファイル パスを取得します。

    [!code-csharp [GetFiles](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L104-L105)]

1. 次に、`foreach` ステートメントを利用して各ファイルを反復処理します。

    ```csharp
    foreach (var file in files)
    {

    }
    ```

1. `foreach` ステートメント内で、ファイルの拡張子がサポートされていることを確認します。 Image Classification API では、JPEG 形式と PNG 形式がサポートされています。

    [!code-csharp [CheckExtension](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L109-L111)]

1. 次に、ファイルのラベルを取得します。 `useFolderNameAsLabel` パラメーターが `true` に設定されている場合、ファイルが保存されている親ディレクトリがラベルとして使用されます。 それ以外の場合、ラベルは、ファイルのプレフィックスか名前自体にする必要があります。

    [!code-csharp [GetLabel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L112-L126)]

1. 最後に、`ModelInput` の新しいインスタンスを作成します。

    [!code-csharp [CreateImageData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L128-L132)]

### <a name="prepare-the-data"></a>データを準備する

1. `Main` メソッドに戻り、`LoadImagesFromDirectory` ユーティリティを使用し、トレーニングに使用される画像の一覧を取得します。

    [!code-csharp [LoadImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L22)]

1. 次に、[`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable%2A) メソッドを利用して [`IDataView`](xref:Microsoft.ML.IDataView) に画像を読み込みます。

    [!code-csharp [CreateIDataView](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L24)]

1. データはディレクトリから読み取られた順序で読み込まれます。 データのバランスを維持するために、[`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows%2A) メソッドを利用してシャッフルします。

    [!code-csharp [ShuffleRows](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L26)]

1. 機械学習モデルでは、数値形式で入力する必要があります。 そのため、トレーニングの前にデータでいくつかの処理を行う必要があります。 [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 変換と `LoadRawImageBytes` 変換から構成される [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) を作成します。 `MapValueToKey` 変換では、`Label` 列のカテゴリ値を受け取り、それを数値 `KeyType` に変換し、`LabelAsKey` という名前の新しい列に保存します。 `LoadImages` では、`imageFolder` パラメーターと共に `ImagePath` から値を取得し、トレーニングのために画像を読み込みます。

    [!code-csharp [PreprocessingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L28-L34)]

1. データを `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) に適用する [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) メソッドを使用し、事前処理済みのデータが含まれる [`IDataView`](xref:Microsoft.ML.IDataView) メソッドを返す [`Transform`](xref:Microsoft.ML.Data.TransformerChain%601.Transform%2A) メソッドを続けます。

    [!code-csharp [PreprocessData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L36-L38)]

1. モデルをトレーニングするには、トレーニング データセットと検証データセットを用意することが重要です。 モデルはトレーニング セットでトレーニングされます。 初見のデータの予測精度は、検証セットに対するパフォーマンスで計測されます。 そのパフォーマンスの結果に基づき、改善努力の中でモデルが学習したものに調整が加えられます。 検証セットは、元のデータセットを分割することで取得するか、この目的のために既に用意されていた別の情報源から取得します。 今回、事前処理済みのデータセットがトレーニング セット、検証セット、テスト セットに分割されています。

    [!code-csharp [CreateDataSplits](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L40-L41)]

    上のコード サンプルでは、2 回分割されます。 まず、事前処理済みのデータが分割され、70% がトレーニングに使用され、残りの 30% が検証に使用されます。 次に、30% の検証セットがさらに検証セットとテスト セットに分割され、90% が検証に使用され、10% がテストに使用されます。

    このようなデータ分割の目的は試験を受けるようなものです。 試験勉強するとき、ノート、教科書、参考書で復習し、試験に出る概念をつかみます。 トレーニング セットがこれに相当します。 次に、模擬試験を受け、自分の知識を確認します。 ここで役立つのが検証セットです。 実際に試験を受ける前に概念をしっかり理解しているかを確認したくなることでしょう。 模擬試験の結果に基づき、間違った箇所や良く理解していなかった箇所をメモし、本番の試験に向けて復習する中、変えるべきところを組み込みます。 最後に、試験を受けます。 テスト セットがこれに相当します。 試験に出ている問題は今まで見たことがありません。トレーニングと検証から学習したことを活用し、手元の課題に自分の知識を応用します。

1. トレーニング データ、検証データ、テスト データそれぞれの値をパーティションに割り当てます。

    [!code-csharp [CreateDatasets](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L43-L45)]

## <a name="define-the-training-pipeline"></a>トレーニング パイプラインを定義する

モデル トレーニングはいくつかのステップから構成されます。 まず、Image Classification API を利用し、モデルがトレーニングされます。 次に、`PredictedLabel` 列のエンコード済みラベルが `MapKeyToValue` 変換により元のカテゴリ値に戻されます。

1. `ImageClassificationTrainer` の必須パラメーターと省略可能なパラメーターのセットを格納するために、新しい変数を作成します。

    [!code-csharp [ClassifierOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L47-L57)]

    `ImageClassificationTrainer` では、いくつかの省略可能なパラメーターを取得します。

    - `FeatureColumnName` はモデルの入力として使用される列です。
    - `LabelColumnName` は予測する値の列です。
    - `ValidationSet` は検証データが含まれる [`IDataView`](xref:Microsoft.ML.IDataView) です。
    - `Arch` では、使用する事前トレーニング済みモデル アーキテクチャが定義されます。 このチュートリアルでは、ResNetv2 モデルの 101 レイヤー型が使用されます。
    - `MetricsCallback` では、トレーニング中に進捗状況を追跡記録する関数がバインドされます。
    - `TestOnTrainSet` からは、検証セットがないとき、トレーニング セットに対してパフォーマンスを検証するようにモデルに指示が出ます。
    - `ReuseTrainSetBottleneckCachedValues` からは、後続の実行でボトルネック フェーズからキャッシュされた値を使用するかどうかがモデルに伝えられます。 ボトルネック フェーズは、初回実行時の計算処理が激しいワンタイム パススルー計算です。 トレーニング データが変わらないとき、エポック数やバッチ サイズを変えて実験する場合、キャッシュした値を利用すると、モデルのトレーニングに必要な時間が大幅に短縮されます。
    - `ReuseValidationSetBottleneckCachedValues` は `ReuseTrainSetBottleneckCachedValues` に似ていますが、これは検証データセット用になります。
    - `WorkspacePath` では、計算されたボトルネック値と `.pb` バージョンのモデルを格納するディレクトリを定義します。

1. `mapLabelEstimator` と `ImageClassificationTrainer` の両方から構成される [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) トレーニング パイプラインを定義します。

    [!code-csharp [TrainingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L59-L60)]

1. [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) メソッドを利用してモデルをトレーニングします。

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L62)]

## <a name="use-the-model"></a>モデルを使用する

モデルをトレーニングできたところで、モデルを利用して画像を分類しましょう。

`Main` メソッドの下で、コンソールに予測情報を表示する、`OutputPrediction` という名前の新しいユーティリティ メソッドを作成します。

[!code-csharp [OuputPredictionMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L96-L100)]

### <a name="classify-a-single-image"></a>1 枚の画像を分類する

1. `Main` メソッドの下に `ClassifySingleImage` という名前の新しいメソッドを追加します。このメソッドは画像を 1 回予測し、出力します。

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. `ClassifySingleImage` メソッド内に [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成します。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスを渡してから、その予測を実行できる便利な API です。

    [!code-csharp [CreatePredictionEngine](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L73)]

1. 1 つの `ModelInput` インスタンスにアクセスするには、[`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) メソッドを利用して `data` [`IDataView`](xref:Microsoft.ML.IDataView) を [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) に変換し、最初の観察を取得します。

    [!code-csharp [GetTestInputData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L75)]

1. [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) メソッドを使用し、画像を分類します。

    [!code-csharp [MakeSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L77)]

1. `OutputPrediction` メソッドでコンソールに予測を出力します。

    [!code-csharp [OuputSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L79-L80)]

1. `Main` メソッド内で、画像のテスト セットを利用して `ClassifySingleImage` を呼び出します。

    [!code-csharp [ClassifySingleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L64)]

### <a name="classify-multiple-images"></a>複数の画像を分類する

1. `ClassifySingleImage` メソッドの下に `ClassifyImages` という名前の新しいメソッドを追加します。このメソッドは画像を複数回予測し、出力します。

    ```csharp
    public static void ClassifyImages(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを利用し、予測を含む [`IDataView`](xref:Microsoft.ML.IDataView) を作成します。 `ClassifyImages` メソッド内に次のコードを追加します。

    [!code-csharp [MakeMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L85)]

1. 予測を反復処理するには、[`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) メソッドを利用して `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) を [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) に変換し、最初の 10 件の観察を取得します。

    [!code-csharp [IEnumerablePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L87)]

1. 予測を反復処理し、元のラベルと予測後のラベルを出力します。

    [!code-csharp [OutputMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L89-L93)]

1. 最後に、`Main` メソッド内で、画像のテスト セットを利用して `ClassifyImages` を呼び出します。

    [!code-csharp [ClassifyImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L66)]

## <a name="run-the-application"></a>アプリケーションの実行

コンソール アプリを実行します。 出力は下の出力のようになるはずです。 警告メッセージまたは処理中のメッセージが表示される場合がありますが、わかりやすくするため、これらのメッセージは結果から削除してあります。 簡潔にするため、出力は要約されています。

**ボトルネック フェーズ**

画像は `byte[]` として読み込まれており、表示する画像名がないため、画像名には値が出力されません。

```test
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 279
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 280
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   1
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   2
```

**トレーニング フェーズ**

```text
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  21, Accuracy:  0.6797619
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  22, Accuracy:  0.7642857
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  23, Accuracy:  0.7916667
```

**画像分類の出力**

```text
Classifying single image
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD

Classifying multiple images
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-163.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-210.jpg | Actual Value: UD | Predicted Value: UD
```

*7001-220.jpg* 画像の検査後、実際のところ、ひび割れがないことがわかりました。

![予測に使用された SDNET2018 データセット画像](./media/image-classification-api-transfer-learning/predictedimage.jpg)

おめでとうございます! これで画像を分類するディープ ラーニング モデルが正しく作成されました。

### <a name="improve-the-model"></a>画像を改善する

モデルの結果に不満が残る場合、次の手法を試し、パフォーマンスを向上してみることができます。

- **データを増やす**:モデルが学習するサンプルの数が多ければ多いほど、パフォーマンスがそれだけ上がります。 [SDNET2018 データセット](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional)を全部ダウンロードし、それを利用してトレーニングします。
- **データを拡張する**:データに変化を与える一般的な手法は、画像にさまざまな変換 (回転、裏返し、移動、トリミング) を適用してデータを強化することです。 これでモデルが学習するサンプルに変化が与えられます。
- **トレーニング時間を増やす**:トレーニング時間が長ければ長いほど、モデルがそれだけ微調整されます。 エポックの数を増やすことで、モデルのパフォーマンスが上がることもあります。
- **パイパーパラメーターで実験する**:このチュートリアルで使用されているパラメーターに加え、他のパラメーターを微調整するとパフォーマンスが上がることがあります。 各エポック後のモデル更新の規模を決定する学習率を変更すると、パフォーマンスが上がることがあります。
- **別のモデル アーキテクチャを使用する**:目で見たときのデータによっては、その特徴を最も効果的に学習できるモデルが異なることがあります。 モデルのパフォーマンスに不満が残る場合、アーキテクチャの変更をお試しください。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、転移学習、事前トレーニング済みの画像分類 TensorFlow モデル、ML.NET Image Classification API を利用してカスタム ディープ ラーニング モデルを構築し、コンクリートの表面の画像をひび割れあり/ひび割れなしに分類する方法について学習しました。

さらに詳しく学習するには、次のチュートリアルに進んでください。

> [!div class="nextstepaction"]
> [物体検出](object-detection-onnx.md)
