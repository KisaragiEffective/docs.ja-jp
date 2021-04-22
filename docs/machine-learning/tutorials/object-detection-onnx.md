---
title: 'チュートリアル: ONNX ディープ ラーニング モデルを使用してオブジェクトを検出する'
description: このチュートリアルでは、ML.NET の事前トレーニング済みの ONNX ディープ ラーニング モデルを使用して画像内のオブジェクトを検出する方法について説明します。
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 2146772d8db28568884e485b95c4cfe8e2ebba2d
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514490"
---
# <a name="tutorial-detect-objects-using-onnx-in-mlnet"></a>チュートリアル: ML.NET で ONNX を使用してオブジェクトを検出する

ML.NET の事前トレーニング済みの ONNX モデルを使用して画像内のオブジェクトを検出する方法について説明します。

オブジェクト検出モデルを最初からトレーニングするには、数百万のパラメーター、大量のラベル付きトレーニング データ、膨大な量の計算リソース (数百時間の GPU) を設定する必要があります。 事前トレーニング済みモデルを使用すると、トレーニング プロセスをショートカットできます。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - 問題を把握する
> - ONNX の概要と ML.NET でどのように動作するかについて説明します。
> - モデルの概要
> - 事前トレーニング済みモデルを再利用する
> - 読み込まれたモデルを使用してオブジェクトを検出する

## <a name="pre-requisites"></a>前提条件

- ".NET Core クロスプラットフォーム開発" ワークロードがインストールされた [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 以降または Visual Studio 2017 バージョン 15.6 以降。
- [Microsoft.ML NuGet パッケージ](https://www.nuget.org/packages/Microsoft.ML/)
- [Microsoft.ML.ImageAnalytics NuGet パッケージ](https://www.nuget.org/packages/Microsoft.ML.ImageAnalytics/)
- [Microsoft.ML.OnnxTransformer NuGet パッケージ](https://www.nuget.org/packages/Microsoft.ML.OnnxTransformer/)
- [Tiny YOLOv2 事前トレーニング済みモデル](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2)
- [Netron](https://github.com/lutzroeder/netron) (省略可能)

## <a name="onnx-object-detection-sample-overview"></a>ONNX オブジェクト検出サンプルの概要

このサンプルでは、事前トレーニング済みのディープ ラーニング ONNX モデルを使用して、画像内のオブジェクトを検出する .NET Core コンソール アプリケーションを作成します。 このサンプルのコードについては、GitHub の [dotnet/machinelearning-samples リポジトリ](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx)を参照してください。

## <a name="what-is-object-detection"></a>オブジェクトの検出とは

オブジェクト検出はコンピューターのビジョンの問題です。 画像の分類に密接に関連していますが、オブジェクト検出では、より詳細なスケールで画像分類が実行されます。 オブジェクト検出では、画像内のエンティティの特定 "_と_" 分類の両方が行われます。 物体検出モデルは、一般的にディープ ラーニングとニューラル ネットワークを使用してトレーニングされます。 詳細については、[ディープ ラーニングと機械学習の違い](/azure/machine-learning/concept-deep-learning-vs-machine-learning)に関するページを参照してください。

オブジェクト検出は、画像に異なる種類のオブジェクトが複数含まれる場合に使用します。

![画像の分類とオブジェクトの分類を示すスクリーンショット。](./media/object-detection-onnx/img-classification-obj-detection.png)

オブジェクト検出のユース ケースには、次のようなものがあります。

- 自動運転車
- ロボティクス
- 顔検出
- 職場の安全
- オブジェクトのカウント
- アクティビティ認識

## <a name="select-a-deep-learning-model"></a>ディープ ラーニング モデルを選択する

ディープ ラーニングは、機械学習のサブセットです。 ディープ ラーニング モデルをトレーニングするには、大量のデータが必要です。 データ内のパターンは、一連のレイヤーによって表されます。 データ内のリレーションシップは、重みを含むレイヤー間の接続としてエンコードされます。 重みが高いほど、リレーションシップが強くなります。 この一連のレイヤーと接続は、まとめて人工ニューラル ネットワークと呼ばれます。 ネットワーク内のレイヤーが多くなるほど、"深く" なり、ディープ ニューラル ネットワークになります。

ニューラル ネットワークにはさまざまな種類があり、よく知られているものとして、マルチレイヤー パーセプトロン (MLP)、畳み込みニューラル ネットワーク (CNN)、および再帰型ニューラル ネットワーク (RNN) があります。 最も基本的なのは MLP であり、一連の入力が一連の出力にマップされます。 このニューラル ネットワークは、データに空間と時間のコンポーネントがない場合に適しています。 CNN では、畳み込みレイヤーを利用し、データに含まれている空間情報を処理します。 CNN の良いユース ケースは、画像の領域内に特徴が存在するかどうかを検出する画像処理です (画像の中央に鼻があるか、など)。 最後に、RNN では、状態またはメモリの永続化を入力として使用できます。 RNN は、時系列分析に使用されます。この場合、イベントの順序付けとコンテキストが重要です。

### <a name="understand-the-model"></a>モデルの概要

オブジェクト検出は、画像処理タスクです。 そのため、この問題を解決するためにトレーニングされるほとんどのディープ ラーニング モデルは、CNN です。 このチュートリアルで使用するモデルは、次のドキュメントで説明されている YOLOv2 モデルのコンパクトなバージョンである Tiny YOLOv2 モデルです。[「YOLO9000:より良く、より速く、より強く」 (Redmon、Farhadi 著)](https://arxiv.org/pdf/1612.08242.pdf)。 Tiny YOLOv2 は Pascal VOC データセットでトレーニングされ、20 種類のクラスのオブジェクトを予測できる 15 個のレイヤーで構成されています。 Tiny YOLOv2 は元の YOLOv2 モデルを凝縮したバージョンなので、速度と精度のトレードオフが生じます。 Netron などのツールを使用して、モデルを構成するさまざまなレイヤーを視覚化できます。 モデルを検査すると、ニューラル ネットワークを構成するすべてのレイヤー間の接続のマッピングが生成されます。各レイヤーには、レイヤーの名前と共にそれぞれの入力/出力の寸法が含まれます。 モデルの入力と出力を記述するために使用されるデータ構造は、テンソルと呼ばれます。 テンソルは、データを N 次元に格納するコンテナーと考えることができます。 Tiny YOLOv2 の場合、入力レイヤーの名前は `image` であり、`3 x 416 x 416` の寸法のテンソルが想定されています。 出力レイヤーの名前は `grid` であり、`125 x 13 x 13` の寸法の出力テンソルが生成されます。

![非表示レイヤーに分割される入力レイヤー、次に出力レイヤー](./media/object-detection-onnx/netron-model-map-layers.png)

YOLO モデルは `3(RGB) x 416px x 416px` の画像を受け取ります。 この入力はモデルによって取得され、さまざまなレイヤーを経由して出力が生成されます。 出力では入力画像が `13 x 13` グリッドに分割されます。グリッド内の各セルは、`125` 値で構成されます。

### <a name="what-is-an-onnx-model"></a>ONNX モデルとは

Open Neural Network Exchange (ONNX) は、AI モデルのオープン ソース形式です。 ONNX は、フレームワーク間の相互運用性をサポートしています。 つまり、PyTorch などの多くの一般的な機械学習フレームワークのいずれかでモデルをトレーニングして ONNX 形式に変換し、ML.NET などの別のフレームワークで ONNX モデルを使用することができます。 詳細については、[ONNX の Web サイト](https://onnx.ai/)を参照してください。

![使用されている、ONNX でサポートされる形式の図。](./media/object-detection-onnx/onnx-supported-formats.png)

事前トレーニング済みの Tiny YOLOv2 モデルは ONNX 形式で格納されます。これはレイヤーのシリアル化された表現であり、それらのレイヤーの学習済みパターンです。 ML.NET では、ONNX との相互運用性は [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) および [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) NuGet パッケージを使用して実現されます。 [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) パッケージには、画像を受け取り、予測またはトレーニング パイプラインへの入力として使用できる数値にエンコードする一連の変換が含まれています。 [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) パッケージでは、ONNX ランタイムを利用して ONNX モデルを読み込み、それを使用して、指定された入力に基づいて予測を行います。

![ONNX ランタイムへの ONNX ファイルのデータ フロー。](./media/object-detection-onnx/onnx-ml-net-integration.png)

## <a name="set-up-the-net-core-project"></a>.NET Core プロジェクトを設定する

ONNX の概要と Tiny YOLOv2 のしくみについて全般的な知識が得られたので、次はアプリケーションをビルドします。

### <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. "ObjectDetection" という名前の **.NET Core コンソール アプリケーション** を作成します。

1. **Microsoft.ML NuGet パッケージ** をインストールします。

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    - ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    - [パッケージ ソース] として "nuget.org" を選択し、[参照] タブを選択し、"**Microsoft.ML**" を検索します。
    - **[インストール]** ボタンを選択します。
    - **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。
    - **Microsoft.ML.ImageAnalytics**、**Microsoft.ML.OnnxTransformer**、**Microsoft.ML.OnnxRuntime** に対してこれらの手順を繰り返します。

### <a name="prepare-your-data-and-pre-trained-model"></a>データと事前トレーニング済みモデルを準備する

1. [プロジェクト資産ディレクトリの zip ファイル](https://github.com/dotnet/machinelearning-samples/raw/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/assets.zip)をダウンロードし、解凍します。

1. `assets` ディレクトリを "*ObjectDetection*" プロジェクト ディレクトリにコピーします。 このディレクトリとそのサブディレクトリには、このチュートリアルに必要な画像ファイル (次の手順でダウンロードして追加する Tiny YOLOv2 モデルを除く) が含まれています。

1. [ONNX Model Zoo](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2) から [Tiny YOLOv2 モデル](https://onnxzoo.blob.core.windows.net/models/opset_8/tiny_yolov2/tiny_yolov2.tar.gz)をダウンロードし、解凍します。

    コマンド プロンプトを開き、次のコマンドを入力します。

    ```shell
    tar -xvzf tiny_yolov2.tar.gz
    ```

1. 解凍したディレクトリから抽出した `model.onnx` ファイルを "*ObjectDetection*" プロジェクトの `assets\Model` ディレクトリにコピーし、名前を `TinyYolo2_model.onnx` に変更します。 このディレクトリには、このチュートリアルに必要なモデルが含まれています。

1. ソリューション エクスプローラーで、資産ディレクトリとサブディレクトリ内の各ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

### <a name="create-classes-and-define-paths"></a>クラスを作成してパスを定義する

"*Program.cs*" ファイルを開き、ファイルの先頭に次の追加の `using` ステートメントを追加します。

[!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L1-L7)]

次に、さまざまな資産のパスを定義します。

1. 最初に、`GetAbsolutePath` メソッドを `Program` クラスの `Main` メソッドの下に追加します。

    [!code-csharp [GetAbsolutePath](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L66-L74)]

1. 次に、`Main` メソッド内にアセットの場所を格納するフィールドを作成します。

    [!code-csharp [AssetDefinition](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L17-L21)]

入力データと予測クラスを格納する新しいディレクトリをプロジェクトに追加します。

**ソリューション エクスプローラー** で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 ソリューション エクスプローラーに新しいフォルダーが表示されたら、"DataStructures" と名前を付けます。

新しく作成した "*DataStructures*" ディレクトリに入力データ クラスを作成します。

1. **ソリューション エクスプローラー** で "*DataStructures*" ディレクトリを右クリックし、 **[追加]**  >  **[新しい項目]** の順に選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを "*ImageNetData.cs*" に変更します。 次に **[追加]** を選択します。

    コード エディターで "*ImageNetData.cs*" ファイルが開きます。 次の `using` ステートメントを "*ImageNetData.cs*" の先頭に追加します。

    [!code-csharp [ImageNetDataUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L1-L4)]

    既存のクラス定義を削除し、`ImageNetData` クラスの次のコードを "*ImageNetData.cs*" ファイルに追加します。

    [!code-csharp [ImageNetDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L8-L23)]

    `ImageNetData` は、入力画像データ クラスであり、次の <xref:System.String> フィールドがあります。

    - `ImagePath` には、画像が保存されているパスが格納されます。
    - `Label` には、ファイルの名前が格納されます。

    また、`ImageNetData` には、指定された `imageFolder` パスに格納されている複数の画像ファイルを読み込み、それらを `ImageNetData` オブジェクトのコレクションとして返すメソッド `ReadFromFile` が含まれています。

"*DataStructures*" ディレクトリに予測クラスを作成します。

1. **ソリューション エクスプローラー** で "*DataStructures*" ディレクトリを右クリックし、 **[追加]**  >  **[新しい項目]** の順に選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを "*ImageNetPrediction.cs* "に変更します。 次に **[追加]** を選択します。

    コード エディターで "*ImageNetPrediction.cs*" ファイルが開きます。 "*ImageNetPrediction.cs*" の先頭に次の `using` ステートメントを追加します。

    [!code-csharp [ImageNetPredictionUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L1)]

    既存のクラス定義を削除し、`ImageNetPrediction` クラスの次のコードを "*ImageNetPrediction.cs*" ファイルに追加します。

    [!code-csharp [ImageNetPredictionClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L5-L9)]

    `ImageNetPrediction` は予測データ クラスであり、次の `float[]` フィールドがあります。

    - `PredictedLabel` には、画像で検出された各境界ボックスの寸法、物体らしさのスコア、およびクラスの確率が含まれます。

### <a name="initialize-variables-in-main"></a>Main で変数を初期化する

[MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の開始点で、`mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

"*Program.cs*" の `Main` メソッドで `outputFolder` フィールドの下に次の行を追加して、`MLContext` の新しいインスタンスで `mlContext` 変数を初期化します。

[!code-csharp [InitMLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L24)]

## <a name="create-a-parser-to-post-process-model-outputs"></a>モデル出力を後処理するパーサーを作成する

このモデルでは、画像を `13 x 13` グリッドに分割します。各グリッド セルは `32px x 32px` です。 各グリッド セルには、5 個の潜在的なオブジェクト境界ボックスが含まれます。 境界ボックスには 25 個の要素があります。

![左側にグリッドのサンプル、右側に境界ボックスのサンプル](./media/object-detection-onnx/model-output-description.png)

- `x` 関連付けられているグリッド セルを基準にした、境界ボックスの中心の x 位置。
- `y` 関連付けられているグリッド セルを基準にした、境界ボックスの中心の y 位置。
- `w` 境界ボックスの幅。
- `h` 境界ボックスの高さ。
- `o` オブジェクトが境界ボックス内に存在する信頼度の値。これは物体らしさのスコアとも呼ばれます。
- `p1-p20` モデルによって予測される 20 個のクラスそれぞれのクラスの確率。

5 個の境界ボックスそれぞれを 25 個の要素で記述し、合計すると、各グリッド セルに 125 個の要素が含まれます。

事前トレーニング済みの ONNX モデルによって生成される出力は、長さ `21125` の float 型の配列であり、寸法が `125 x 13 x 13` のテンソルの要素を表します。 モデルによって生成される予測をテンソルに変換するには、処理後の作業が必要です。 これを行うには、出力を解析する一連のクラスを作成します。

新しいディレクトリをプロジェクトに追加し、一連のパーサー クラスを編成します。

1. **ソリューション エクスプローラー** で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 ソリューション エクスプローラーに新しいフォルダーが表示されたら、"YoloParser" と名前を付けます。

### <a name="create-bounding-boxes-and-dimensions"></a>境界ボックスと寸法を作成する

モデルから出力されるデータには、画像内のオブジェクトの境界ボックスの座標と寸法が含まれています。 寸法の基底クラスを作成します。

1. **ソリューション エクスプローラー** で、"*YoloParser*" ディレクトリを右クリックし、 **[追加]**  >  **[新しい項目]** の順に選択します。
1. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを "*DimensionsBase.cs*" に変更します。 次に **[追加]** を選択します。

    コード エディターで "*DimensionsBase.cs*" ファイルが開きます。 すべての `using` ステートメントと既存のクラス定義を削除します。

    `DimensionsBase` クラスの次のコードを "*DimensionsBase.cs*" ファイルに追加します。

    [!code-csharp [DimensionsBaseClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/DimensionsBase.cs#L3-L9)]

    `DimensionsBase` には、次の `float` プロパティがあります。

    - `X` には、x 軸に沿ったオブジェクトの位置が格納されます。
    - `Y` には、y 軸に沿ったオブジェクトの位置が格納されます。
    - `Height` には、オブジェクトの高さが格納されます。
    - `Width` には、オブジェクトの幅が格納されます。

次に、境界ボックスのクラスを作成します。

1. **ソリューション エクスプローラー** で、"*YoloParser*" ディレクトリを右クリックし、 **[追加]**  >  **[新しい項目]** の順に選択します。
1. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを "*YoloBoundingBox.cs*" に変更します。 次に **[追加]** を選択します。

    コード エディターで "*YoloBoundingBox.cs*" ファイルが開きます。 "*YoloBoundingBox.cs*" の先頭に次の `using` ステートメントを追加します。

    [!code-csharp [YoloBoundingBoxUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L1)]

    既存のクラス定義のすぐ上に、`BoundingBoxDimensions` という新しいクラス定義を追加します。これは `DimensionsBase` クラスから継承して、それぞれの境界ボックスの寸法を格納します。

    [!code-csharp [BoundingBoxDimClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L5)]

    既存の `YoloBoundingBox` クラス定義を削除し、`YoloBoundingBox` クラスの次のコードを "*YoloBoundingBox.cs*" ファイルに追加します。

    [!code-csharp [YoloBoundingBoxClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L7-L21)]

    `YoloBoundingBox` には、次のプロパティがあります。

    - `Dimensions` には、境界ボックスの寸法が格納されます。
    - `Label` には、境界ボックス内で検出されるオブジェクトのクラスが格納されます。
    - `Confidence` には、クラスの信頼度が格納されます。
    - `Rect` には、境界ボックスの寸法の四角形の表現が格納されます。
    - `BoxColor` には、画像への描画に使用される各クラスに関連付けられた色が格納されます。

### <a name="create-the-parser"></a>パーサーを作成する

寸法と境界ボックスのクラスが作成されたので、次はパーサーを作成します。

1. **ソリューション エクスプローラー** で、"*YoloParser*" ディレクトリを右クリックし、 **[追加]**  >  **[新しい項目]** の順に選択します。
1. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを "*YoloOutputParser.cs*" に変更します。 次に **[追加]** を選択します。

    コード エディターで "*YoloOutputParser.cs*" ファイルが開きます。 "*YoloOutputParser.cs*" の先頭に次の `using` ステートメントを追加します。

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L1-L4)]

    既存の `YoloOutputParser` クラス定義内に、画像の各セルの寸法を格納する入れ子になったクラスを追加します。 `YoloOutputParser` クラス定義の先頭で `DimensionsBase` クラスから継承する `CellDimensions` クラスに次のコードを追加します。

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L10)]

1. `YoloOutputParser` クラス定義内に、次の定数とフィールドを追加します。

    [!code-csharp [ParserVarDefinitions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L12-L21)]

    - `ROW_COUNT` は、画像が分割されるグリッドの行数です。
    - `COL_COUNT` は、画像が分割されるグリッドの列数です。
    - `CHANNEL_COUNT` は、グリッドの 1 つのセルに含まれる値の合計数です。
    - `BOXES_PER_CELL` は、セル内の境界ボックスの数です。
    - `BOX_INFO_FEATURE_COUNT` は、ボックス内に含まれる特徴の数です (x、y、高さ、幅、信頼度)。
    - `CLASS_COUNT` は、各境界ボックスに含まれるクラス予測の数です。
    - `CELL_WIDTH` は、画像グリッド内の 1 つのセルの幅です。
    - `CELL_HEIGHT` は、画像グリッド内の 1 つのセルの高さです。
    - `channelStride` は、グリッド内の現在のセルの開始位置です。

    モデルで予測 (スコアリングとも呼ばれる) が行われると、`416px x 416px` の入力画像が `13 x 13` のサイズのセルから成るグリッドに分割されます。 各セルには `32px x 32px` が含まれます。 各セルには 5 個の境界ボックスがあり、それぞれに 5 つの特徴 (x、y、幅、高さ、信頼度) があります。 また、各境界ボックスには、各クラスの確率 (この例では 20) が格納されます。 したがって、各セルには 125 個の情報が含まれます (5 つの特徴 + 20 個のクラスの確率)。

5 個の境界ボックスすべてについて、`channelStride` の下にアンカーのリストを作成します。

[!code-csharp [ParserAnchors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L23-L26)]

アンカーは、境界ボックスの定義済みの高さと幅の比率です。 モデルによって検出されるほとんどのオブジェクトまたはクラスには、同様の比率があります。 これは、境界ボックスを作成するときに役立ちます。 境界ボックスの予測ではなく、事前に定義された寸法からのオフセットが計算されるため、境界ボックスを予測するために必要な計算が少なくなります。 通常、これらのアンカーの比率は、使用されるデータセットに基づいて計算されます。 この場合は、データセットは既知であり、値が事前に計算されているため、アンカーをハードコーディングすることができます。

次に、モデルで予測するラベルまたはクラスを定義します。 このモデルでは、元の YOLOv2 モデルによって予測されるクラスの合計数のサブセットである 20 個のクラスを予測します。

`anchors` の下にラベルのリストを追加します。

[!code-csharp [ParserLabels](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L28-L34)]

各クラスには色が関連付けられています。 `labels` の下にクラスの色を割り当てます。

[!code-csharp [ParserColors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L36-L59)]

### <a name="create-helper-functions"></a>ヘルパー関数を作成する

後処理フェーズには、関係する一連の手順が含まれています。 このためには、いくつかのヘルパー メソッドを使用できます。

パーサーによって使用されるヘルパー メソッドは次のとおりです。

- `Sigmoid` では、0 から 1 までの数値を出力するシグモイド関数が適用されます。
- `Softmax` では、入力ベクトルが確率分布に正規化されます。
- `GetOffset` では、1 次元モデルの出力の要素が、`125 x 13 x 13` テンソルの対応する位置にマップされます。
- `ExtractBoundingBoxes` では、モデル出力から `GetOffset` メソッドを使用して、境界ボックスの寸法が抽出されます。
- `GetConfidence` では、モデルで、オブジェクトが検出されたことをどのくらい確信しているかを示す信頼度が抽出され、`Sigmoid` 関数を使用してパーセンテージ値に変換されます。
- `MapBoundingBoxToCell` では、境界ボックスの寸法を使用して、画像内の各セルにマップされます。
- `ExtractClasses` では、`GetOffset` メソッドを使用してモデルの出力から境界ボックスのクラス予測が抽出され、`Softmax` メソッドを使用して確率分布に変換されます。
- `GetTopResult` では、確率が最も高い予測クラスのリストからクラスが選択されます。
- `IntersectionOverUnion` では、確率が低い境界ボックスの重なりがフィルター処理されます。

すべてのヘルパー メソッドのコードを `classColors` のリストの下に追加します。

[!code-csharp [ParserHelperMethods](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L61-L151)]

すべてのヘルパー メソッドを定義したら、次はそれらを使用してモデルの出力を処理します。

`IntersectionOverUnion` メソッドの下に、モデルによって生成される出力を処理する `ParseOutputs` メソッドを作成します。

```csharp
public IList<YoloBoundingBox> ParseOutputs(float[] yoloModelOutputs, float threshold = .3F)
{

}
```

境界ボックスを格納するリストを作成し、`ParseOutputs` メソッド内に変数を定義します。

[!code-csharp [BBoxList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L155)]

各画像は、`13 x 13` セルのグリッドに分割されます。 各セルには 5 個の境界ボックスがあります。 `boxes` 変数の下に、各セルのすべてのボックスを処理するコードを追加します。

```csharp
for (int row = 0; row < ROW_COUNT; row++)
{
    for (int column = 0; column < COL_COUNT; column++)
    {
        for (int box = 0; box < BOXES_PER_CELL; box++)
        {

        }
    }
}
```

最も内側のループ内で、1 次元モデルの出力に含まれる現在のボックスの開始位置を計算します。

[!code-csharp [ChannelDef](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L163)]

その直下で、`ExtractBoundingBoxDimensions` メソッドを使用して、現在の境界ボックスの寸法を取得します。

[!code-csharp [GetBBoxDims](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L165)]

次に、`GetConfidence` メソッドを使用して、現在の境界ボックスの信頼度を取得します。

[!code-csharp [GetConfidence](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L167)]

その後、`MapBoundingBoxToCell` メソッドを使用して、現在の境界ボックスを現在処理中のセルにマップします。

[!code-csharp [MapBoundingBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L169)]

さらに処理を進める前に、信頼度の値が、指定されたしきい値より大きいかどうかを確認します。 そうでない場合は、次の境界ボックスを処理します。

[!code-csharp [CheckThreshold1](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L171-L172)]

それ以外の場合は、出力の処理を続行します。 次の手順では、`ExtractClasses` メソッドを使用して、現在の境界ボックスの予測クラスの確率分布を取得します。

[!code-csharp [ExtractClasses](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L174)]

次に、`GetTopResult` メソッドを使用して、現在のボックスの確率が最も高いクラスの値とインデックスを取得し、スコアを計算します。

[!code-csharp [GetTopResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L176-L177)]

ここでも `topScore` を使用して、指定されたしきい値を超える境界ボックスのみを保持します。

[!code-csharp [CheckThreshold2](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L179-L180)]

最後に、現在の境界ボックスがしきい値を超えている場合は、新しい `BoundingBox` オブジェクトを作成して `boxes` リストに追加します。

[!code-csharp [AddBBoxToList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L182-L194)]

画像内のすべてのセルが処理されたら、`boxes` リストを返します。 `ParseOutputs` メソッドの最も外側の for-loop ループの下に、次の return ステートメントを追加します。

[!code-csharp [ReturnBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L198)]

### <a name="filter-overlapping-boxes"></a>重複するボックスをフィルター処理する

これで、信頼度の高いすべての境界ボックスがモデルの出力から抽出されたので、重複している画像を削除するために追加のフィルター処理を行う必要があります。 `ParseOutputs` メソッドの下に `FilterBoundingBoxes` というメソッドを追加します。

```csharp
public IList<YoloBoundingBox> FilterBoundingBoxes(IList<YoloBoundingBox> boxes, int limit, float threshold)
{

}
```

まずは、`FilterBoundingBoxes` メソッド内に、検出されたボックスのサイズと等しい配列を作成し、すべてのスロットをアクティブまたは処理の準備完了にマークします。

[!code-csharp [InitActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L203-L207)]

次に、境界ボックスが格納されたリストを信頼度に基づいて降順に並べ替えます。

[!code-csharp [SortBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L209-L211)]

その後、フィルター処理された結果を保持するリストを作成します。

[!code-csharp [InitFilterResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L213)]

各境界ボックスを反復処理して、各境界ボックスの処理を開始します。

```csharp
for (int i = 0; i < boxes.Count; i++)
{

}
```

この for ループ内で、現在の境界ボックスを処理できるかどうかを確認します。

```csharp
if (isActiveBoxes[i])
{

}
```

その場合は、境界ボックスを結果のリストに追加します。 結果が、抽出されるボックスの指定された制限を超えると、ループから抜け出します。 if ステートメント内に次のコードを追加します。

[!code-csharp [AddFirstBBoxToResults](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L219-L223)]

それ以外の場合は、隣接する境界ボックスを見ます。 ボックスの制限チェックの下に次のコードを追加します。

```csharp
for (var j = i + 1; j < boxes.Count; j++)
{

}
```

最初のボックスと同様に、隣接するボックスがアクティブまたは処理の準備ができている場合は、`IntersectionOverUnion` メソッドを使用して、最初のボックスと 2 番目のボックスが、指定されたしきい値を超えているかどうかを確認します。 次のコードを、最も内側の for ループに追加します。

[!code-csharp [IOUBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L227-L239)]

隣接する境界ボックスを確認する最も内側の for ループの外側で、処理する残りの境界ボックスがあるかどうかを確認します。 そうでない場合は、外側の for ループから抜け出します。

[!code-csharp [CheckActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L242-L243)]

最後に、`FilterBoundingBoxes` メソッドの最初の for ループの外側で、次の結果を返します。

[!code-csharp [ReturnFilteredBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L246)]

これでセットアップは終了です。 次は、このコードをモデルと共に使用して、スコアリングを行います。

## <a name="use-the-model-for-scoring"></a>スコアリングにモデルを使用する

後処理の場合と同様に、スコアリングの手順にはいくつかの手順があります。 このために、スコアリング ロジックを格納するクラスをプロジェクトに追加します。

1. **ソリューション エクスプローラー** で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。
1. **[新しい項目の追加]** ダイアログボックスで **[クラス]** を選択し、 **[名前]** フィールドを "*OnnxModelScorer.cs*" に変更します。 次に **[追加]** を選択します。

    コード エディターで "*OnnxModelScorer.cs*" ファイルが開きます。 "*OnnxModelScorer.cs*" の先頭に次の `using` ステートメントを追加します。

    [!code-csharp [ScorerUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L1-L7)]

    `OnnxModelScorer` クラス定義内に、次の変数を追加します。

    [!code-csharp [InitScorerVars](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L13-L17)]

    その直下で、以前に定義された変数を初期化する `OnnxModelScorer` クラスのコンストラクターを作成します。

    [!code-csharp [ScorerCtor](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L19-L24)]

    コンストラクターを作成したら、画像とモデルの設定に関連する変数を格納する 2 つの構造体を定義します。 モデルの入力として想定される高さと幅を格納するために、`ImageNetSettings` という名前の構造体を作成します。

    [!code-csharp [ImageNetSettingStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L26-L30)]

    その後、`TinyYoloModelSettings` という別の構造体を作成します。これには、モデルの入力レイヤーと出力レイヤーの名前を格納します。 モデルの入力レイヤーと出力レイヤーの名前を視覚化するには、[Netron](https://github.com/lutzroeder/netron) のようなツールを使用できます。

    [!code-csharp [YoloSettingsStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L32-L43)]

    次に、スコアリングに使用するメソッドの最初のセットを作成します。 `OnnxModelScorer` クラス内に `LoadModel` メソッドを作成します。

    ```csharp
    private ITransformer LoadModel(string modelLocation)
    {

    }
    ```

    `LoadModel` メソッド内に、ログ記録用に次のコードを追加します。

    [!code-csharp [LoadModelLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L47-L49)]

    [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit%2A) メソッドが呼び出されたときに操作されるデータ スキーマが ML.NET パイプラインで認識されている必要があります。 この場合、トレーニングに似たプロセスが使用されます。 ただし、実際のトレーニングは行われないため、空の [`IDataView`](xref:Microsoft.ML.IDataView) を使用することができます。 空のリストから、パイプラインの新しい [`IDataView`](xref:Microsoft.ML.IDataView) を作成します。

    [!code-csharp [LoadEmptyIDV](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L52)]

    その下にパイプラインを定義します。 パイプラインは、4 つの変換で構成されます。

    - [`LoadImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages%2A) で、画像がビットマップとして読み込まれます。
    - [`ResizeImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.ResizeImages%2A) で、指定されたサイズ (この場合は `416 x 416`) に画像の再スケーリングが行われます。
    - [`ExtractPixels`](xref:Microsoft.ML.ImageEstimatorsCatalog.ExtractPixels%2A) で、画像のピクセル表現がビットマップから数値ベクターに変更されます。
    - [`ApplyOnnxModel`](xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel%2A) で、ONNX モデルが読み込まれ、それを使用して、指定されたデータでスコアリングされます。

    `LoadModel` メソッドで、`data` 変数の下にパイプラインを定義します。

    [!code-csharp [ScoringPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L55-L58)]

    次に、スコアリングできるようにモデルをインスタンス化します。 パイプラインの [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit%2A) メソッドを呼び出し、後続の処理のためにそれを返します。

    [!code-csharp [FitReturnModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L61-L63)]

モデルが読み込まれたら、それを使用して予測を行うことができます。 このプロセスを容易にするために、`LoadModel` メソッドの下に `PredictDataUsingModel` というメソッドを作成します。

```csharp
private IEnumerable<float[]> PredictDataUsingModel(IDataView testData, ITransformer model)
{

}
```

`PredictDataUsingModel` 内に、ログを記録できるように、次のコードを追加します。

[!code-csharp [PredictDataLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L68-L71)]

次に、[`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを使用してデータをスコアリングします。

[!code-csharp [ScoreImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L73)]

予測される確率を抽出し、追加の処理が行えるように確率を返します。

[!code-csharp [ReturnModelOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L75-L77)]

これで両方の手順が設定されたので、結合して 1 つのメソッドにします。 `PredictDataUsingModel` メソッドの下に、`Score` という新しいメソッドを追加します。

[!code-csharp [ScoreMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L80-L85)]

もう一息です! これで、すべてを使用できるようになりました。

## <a name="detect-objects"></a>オブジェクトを検出する

すべての設定が完了したので、次はいくつかのオブジェクトを検出します。 まず、*Program.cs* クラスでスコアラーとパーサーの参照を追加します。

[!code-csharp [ReferenceScorerParser](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L8-L9)]

### <a name="score-and-parse-model-outputs"></a>モデル出力のスコア付けと解析

"*Program.cs*" クラスの `Main` メソッド内に、try-catch ステートメントを追加します。

```csharp
try
{

}
catch (Exception ex)
{
    Console.WriteLine(ex.ToString());
}
```

`try` ブロック内で、オブジェクト検出ロジックの実装を開始します。 まず、データを [`IDataView`](xref:Microsoft.ML.IDataView) に読み込みます。

[!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L29-L30)]

次に、`OnnxModelScorer` のインスタンスを作成し、読み込まれるデータのスコアリングに使用します。

[!code-csharp [ScoreData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L33-L36)]

次は、後処理の手順です。 `YoloOutputParser` のインスタンスを作成し、モデル出力の処理に使用します。

[!code-csharp [ParsePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L39-L44)]

モデル出力が処理されたら、次は画像上に境界ボックスを描画します。

### <a name="visualize-predictions"></a>予測の視覚化

モデルによって画像にスコアが付けられ、出力が処理された後は、境界ボックスが画像の上に描画される必要があります。 これを行うには、*Program.cs* の内部にある `GetAbsolutePath` メソッドの下に `DrawBoundingBox` というメソッドを追加します。

```csharp
private static void DrawBoundingBox(string inputImageLocation, string outputImageLocation, string imageName, IList<YoloBoundingBox> filteredBoundingBoxes)
{

}
```

まず、`DrawBoundingBox` メソッドで画像を読み込み、高さと幅の寸法を取得します。

[!code-csharp [LoadImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L78-L81)]

次に、for-each ループを作成し、モデルにより検出される各境界ボックスに反復処理を行います。

```csharp
foreach (var box in filteredBoundingBoxes)
{

}
```

for-each ループ内で、境界ボックスの寸法を取得します。

[!code-csharp [GetBBoxDimensions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L86-L89)]

境界ボックスの寸法は `416 x 416` のモデル入力に対応しているため、画像の実際のサイズに合わせて境界ボックスの寸法を拡大縮小します。

[!code-csharp [ScaleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L92-L95)]

次に、各境界ボックスの上に表示されるテキストのテンプレートを定義します。 テキストには、対応する境界ボックス内のオブジェクトのクラスのほか、信頼度を含めます。

[!code-csharp [DefineBBoxText](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L98)]

画像を描画するために、[`Graphics`](xref:System.Drawing.Graphics) オブジェクトに変換します。

```csharp
using (Graphics thumbnailGraphic = Graphics.FromImage(image))
{

}
```

`using` コード ブロック内で、グラフィックの [`Graphics`](xref:System.Drawing.Graphics) オブジェクト設定を調整します。

[!code-csharp [TuneGraphicSettings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L102-L104)]

その下で、テキストと境界ボックスのフォントと色のオプションを設定します。

[!code-csharp [SetColorOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L106-L114)]

[`FillRectangle`](xref:System.Drawing.Graphics.FillRectangle%2A) メソッドを使用してテキストを含めるために、境界ボックスの上に四角形を作成して塗りつぶします。 これにより、テキストにコントラストを付け、読みやすさを向上させることができます。

[!code-csharp [DrawTextBackground](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L117)]

次に、[`DrawString`](xref:System.Drawing.Graphics.DrawString%2A) メソッドと [`DrawRectangle`](xref:System.Drawing.Graphics.DrawRectangle%2A) メソッドを使用して、画像の上にテキストと境界ボックスを描画します。

[!code-csharp [DrawClassAndBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L118-L121)]

for-each ループの外側に、画像を `outputDirectory` に保存するコードを追加します。

[!code-csharp [SaveImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L125-L130)]

アプリケーションが実行時に想定どおりに予測を行っているという追加のフィードバックを取得するには、*Program.cs* ファイル内にある `DrawBoundingBox` メソッドの下に `LogDetectedObjects` というメソッドを追加して、検出されたオブジェクトをコンソールに出力します。

[!code-csharp [LogOutputs](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L133-L143)]

これで予測から視覚的なフィードバックを作成するヘルパー メソッドが用意できたので、スコア付けされた画像を反復処理する for ループを追加します。

```csharp
for (var i = 0; i < images.Count(); i++)
{

}
```

for ループ内で、画像ファイルの名前と、画像に関連付けられている境界ボックスを取得します。

[!code-csharp [GetImageFileName](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L49-L50)]

その後、`DrawBoundingBox` メソッドを使用して、画像の上に境界ボックスを描画します。

[!code-csharp [DrawBBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L52)]

最後に、`LogDetectedObjects` メソッドを使用し、予測をコンソールに出力します。

[!code-csharp [LogPredictionsOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L54)]

try-catch ステートメントの後に、プロセスの実行が完了したことを示すロジックを追加します。

[!code-csharp [EndProcessLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L62-L63)]

これで完了です。

## <a name="results"></a>結果

上記の手順を実行した後、コンソール アプリを実行します (Ctrl + F5 キー)。 結果は以下の出力のようになるはずです。 警告メッセージまたは処理中のメッセージが表示される場合がありますが、わかりやすくするため、これらのメッセージは結果から削除してあります。

```console
=====Identify the objects in the images=====

.....The objects in the image image1.jpg are detected as below....
car and its Confidence score: 0.9697262
car and its Confidence score: 0.6674225
person and its Confidence score: 0.5226039
car and its Confidence score: 0.5224892
car and its Confidence score: 0.4675332

.....The objects in the image image2.jpg are detected as below....
cat and its Confidence score: 0.6461141
cat and its Confidence score: 0.6400049

.....The objects in the image image3.jpg are detected as below....
chair and its Confidence score: 0.840578
chair and its Confidence score: 0.796363
diningtable and its Confidence score: 0.6056048
diningtable and its Confidence score: 0.3737402

.....The objects in the image image4.jpg are detected as below....
dog and its Confidence score: 0.7608147
person and its Confidence score: 0.6321323
dog and its Confidence score: 0.5967442
person and its Confidence score: 0.5730394
person and its Confidence score: 0.5551759

========= End of Process..Hit any Key ========
```

画像を境界ボックスと共に表示するには、`assets/images/output/` ディレクトリに移動します。 処理された画像の 1 つのサンプルを次に示します。

![ダイニング ルームの処理済み画像のサンプル](./media/object-detection-onnx/dinning-room-table-chairs.png)

おめでとうございます! これで、ML.NET でトレーニング済みの `ONNX` モデルを再利用してオブジェクト検出用の機械学習モデルを構築できました。

このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx) リポジトリにあります。

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> - 問題を把握する
> - ONNX の概要と ML.NET でどのように動作するかについて説明します。
> - モデルの概要
> - 事前トレーニング済みモデルを再利用する
> - 読み込まれたモデルを使用してオブジェクトを検出する

Machine Learning サンプルの GitHub リポジトリを確認し、拡張されたオブジェクト検出サンプルを探索してください。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub リポジトリ](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx)
