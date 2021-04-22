---
title: 'チュートリアル: Model Builder で画像内のオブジェクトを検出する'
description: このチュートリアルでは、ML.NET Model Builder と Azure ML を使用して、物体検出モデルを構築し、画像内の一時停止標識を検出する方法について説明します。
author: briacht
ms.author: brachtma
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mlnet-tooling
ms.openlocfilehash: f628be47bbef080505355fa99630c68b327f1a60
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514503"
---
# <a name="tutorial-detect-stop-signs-in-images-with-model-builder"></a>チュートリアル: Model Builder で画像内の一時停止標識を検出する

ML.NET Model Builder と Azure Machine Learning を使用して、物体検出モデルを構築し、画像内の一時停止標識を検出して特定する方法について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - データを準備して理解する
> - シナリオを選択する
> - トレーニング環境を選択する
> - データを読み込む
> - モデルをトレーニングする
> - モデルを評価する
> - モデルを使用して予測を行う

> [!NOTE]
> モデル ビルダーは現在のところ、プレビュー段階です。

## <a name="prerequisites"></a>前提条件

前提条件の一覧とインストール手順は、[モデル ビルダーのインストール ガイド](../how-to-guides/install-model-builder.md)を参照してください。

## <a name="model-builder-object-detection-overview"></a>Model Builder の物体検出の概要

オブジェクト検出はコンピューターのビジョンの問題です。 画像の分類に密接に関連していますが、オブジェクト検出では、より詳細なスケールで画像分類が実行されます。 オブジェクト検出では、画像内のエンティティの特定 "_と_" 分類の両方が行われます。 物体検出モデルは、一般的にディープ ラーニングとニューラル ネットワークを使用してトレーニングされます。 詳細については、[ディープ ラーニングと機械学習の違い](/azure/machine-learning/concept-deep-learning-vs-machine-learning)に関するページを参照してください。

オブジェクト検出は、画像に異なる種類のオブジェクトが複数含まれる場合に使用します。

![画像の分類とオブジェクトの分類を示すスクリーンショット。](./media/object-detection-onnx/img-classification-obj-detection.PNG)

オブジェクト検出のユース ケースには、次のようなものがあります。

- 自動運転車
- ロボティクス
- 顔検出
- 職場の安全
- オブジェクトのカウント
- アクティビティ認識

このサンプルでは、Model Builder で構築された機械学習モデルを使用して、画像内の一時停止標識を検出する C# .NET Core コンソール アプリケーションを作成します。 このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub リポジトリにあります。

## <a name="prepare-and-understand-the-data"></a>データを準備して理解する

一時停止標識データセットは、[Unsplash](https://unsplash.com/) からダウンロードされ、それぞれに少なくとも 1 つの一時停止標識が含まれている 50 個の画像で構成されています。

最初に実行する必要があることは、画像に注釈を付けるか、各画像の一時停止標識の周囲に境界ボックスを描画することです。 このチュートリアルでは、[VoTT](https://github.com/microsoft/VoTT) というツールで画像に注釈を付けます。

> 以下のデータのラベル付けの手順をスキップする場合は、[このデータセットのバージョンをダウンロード](https://aka.ms/mlnet-object-detection-tutorial-assets)して、「[コンソール アプリケーションを作成する](object-detection-model-builder.md#create-a-console-application)」に進みます。

### <a name="create-a-new-vott-project"></a>新しい VoTT プロジェクトを作成する

1. 50 個の一時停止標識画像の[データセットをダウンロード](https://aka.ms/mlnet-object-detection-tutorial-dataset)して、解凍します。
1. [VoTT (Visual Object Tagging Tool) をダウンロード](https://github.com/Microsoft/VoTT/releases)します。
1. VoTT を開いて、 **[新しいプロジェクト]** を選択します。

    ![VoTT ホーム画面](./media/object-detection-model-builder/vott.png)

1. **[プロジェクトの設定]** で、 **[表示名]** を "StopSignObjDetection" に変更します。
1. **[セキュリティ トークン]** を *[新しいセキュリティ トークンの生成]* に変更します。
1. **[ソース接続]** の横にある **[接続の追加]** を選択します。
1. **[接続の設定]** で、ソース接続の **[表示名]** を "StopSignImages" に変更し、 *[ローカル ファイル システム]* を **[プロバイダー]** として選択します。 **[フォルダー パス]** で、50 個のトレーニング画像を格納する *Stop-Signs* フォルダーを選択し、 **[接続の保存]** を選択します。

    ![VoTT の [新しい接続] ダイアログ](./media/object-detection-model-builder/vott-new-connection.png)

1. **[プロジェクトの設定]** で、 **[ソース接続]** を *[StopSignImages]* (先ほど作成した接続) に変更します。
1. **[ターゲット接続]** も *[StopSignImages]* に変更します。 **[プロジェクトの設定]** は、次のスクリーンショットのようになっているはずです。

    ![VoTT の [プロジェクトの設定] ダイアログ](./media/object-detection-model-builder/vott-new-project.png)

1. **[プロジェクトの保存]** を選択します。

### <a name="add-tag-and-label-images"></a>タグとラベルの画像を追加する

これで、左側にすべてのトレーニング画像のプレビュー画像、中央に選択した画像のプレビュー、右側に **[タグ]** 列があるウィンドウが表示されるはずです。 この画面は、**タグ エディター** です。

1. 新しいタグを追加するには、 **[タグ]** ツールバーの最初 (プラス形) のアイコンを選択します。

    ![VoTT の [新しいタグ] アイコン](./media/object-detection-model-builder/vott-new-tag-icon.png)

1. タグに "Stop-Sign" という名前を付け、キーボードの <kbd>Enter</kbd> を押します。

    ![VoTT の [新しいタグ]](./media/object-detection-model-builder/vott-new-tag.png)

1. クリックしてドラッグし、画像内の各一時停止標識の周囲に四角形を描画します。 カーソルで四角形を描画できない場合は、上部のツールバーから **[四角形の描画]** ツールを選択するか、キーボード ショートカット <kbd>R</kbd> を使用してください。
1. 四角形の描画後、前の手順で作成した **[Stop-Sign]** タグを選択して、境界ボックスにタグを追加します。
1. データセット内の次の画像のプレビュー画像をクリックし、このプロセスを繰り返します。
1. すべての画像のすべての一時停止標識に対して、手順 3 から 4 を繰り返します。

    ![VoTT の画像の注釈付け](./media/object-detection-model-builder/vott-annotating.gif)

### <a name="export-your-vott-json"></a>VoTT JSON をエクスポートする

すべてのトレーニング画像をラベル付けしたら、Model Builder でトレーニングに使用されるファイルをエクスポートできます。

1. 左側のツールバーの 4 番目のアイコン (ボックスに斜めの矢印があるもの) を選択し、 **[エクスポート設定]** に移動します。
1. **[プロバイダー]** は *[VoTT JSON]* のままにします。
1. **[アセットの状態]** を、 *[タグ付きアセットのみ]* に変更します。
1. **[画像を含める]** をオフにします。 画像を含める場合は、生成されるエクスポート フォルダーにトレーニング画像がコピーされますが、これは必要ではありません。
1. **[エクスポート設定の保存]** を選択します。

    ![VoTT の [エクスポート設定]](./media/object-detection-model-builder/vott-export.png)

1. **タグ エディター** (リボンのような形をした、左側のツールバーの 2 番目のアイコン) に戻ります。 上部のツールバーで、 **[プロジェクトのエクスポート]** アイコン (ボックス内の矢印のような形をした最後のアイコン) を選択するか、キーボード ショートカットの <kbd>Ctrl</kbd>+<kbd>E</kbd> を使用します。

    ![VoTT の [エクスポート] ボタン](./media/object-detection-model-builder/vott-export-button.png)

このエクスポートによって、*Stop-Sign-Images* フォルダーに *vott-json-export* という新しいフォルダーが作成され、その新しいフォルダーに *StopSignObjDetection-export* という名前の JSON ファイルが生成されます。 この JSON ファイルは、次の手順で、Model Builder で物体検出モデルをトレーニングするために使用します。

## <a name="create-a-console-application"></a>コンソール アプリケーションの作成

1. Visual Studio で、*StopSignDetection* という **C# .NET Core コンソール アプリケーション** を作成します。

## <a name="choose-a-scenario"></a>シナリオを選択する

1. **ソリューション エクスプローラー** で、*StopSignDetection* プロジェクトを右クリックし、 **[追加]**  >  **[機械学習]** を選択し、Model Builder UI を開きます。
1. このサンプルの場合、シナリオは物体検出です。 Model Builder の **[シナリオ]** 手順で、 **[物体検出]** シナリオを選択します。

![Visual Studio のモデル ビルダー ウィザード](./media/object-detection-model-builder/obj-det-scenario.png)

> シナリオの一覧に *[物体検出]* が表示されない場合は、[Model Builder のバージョンの更新](https://marketplace.visualstudio.com/items?itemName=MLNET.07)が必要になることがあります。

## <a name="choose-the-training-environment"></a>トレーニング環境を選択する

現時点で、Model Builder では Azure Machine Learning による物体検出モデルのトレーニングのみがサポートされているため、既定で Azure トレーニング環境が選択されています。

![Azure トレーニング環境の選択](./media/object-detection-model-builder/obj-det-environment.png)

Azure ML を使用してモデルをトレーニングするには、Model Builder から Azure ML 実験を作成する必要があります。

**Azure ML 実験** は、1 回以上の機械学習トレーニングの実行の構成と結果をカプセル化するリソースです。

Azure ML 実験を作成するには、まず Azure で環境を構成する必要があります。 実験を実行するには、次のものが必要です。

- Azure サブスクリプション
- **ワークスペース**: トレーニング実行の一部として作成される Azure ML のすべてのリソースと成果物の一元的な場所を提供する Azure ML リソースです。
- **コンピューティング**: Azure Machine Learning コンピューティングとは、トレーニングに使用するクラウドベースの Linux VM です。 [Model Builder でサポートされているコンピューティングの種類](../resources/azure-training-concepts-model-builder.md#what-is-an-azure-machine-learning-compute)の詳細を確認してください。

### <a name="set-up-an-azure-ml-workspace"></a>Azure ML ワークスペースを設定する

環境を構成するには:

1. **[ワークスペースの設定]** ボタンを選択します。
1. **[新しい実験の作成]** ダイアログで、Azure サブスクリプションを選択します。
1. 既存のワークスペースを選択するか、新しい Azure ML ワークスペースを作成します。

    新しいワークスペースを作成する場合、次のリソースがプロビジョニングされます。

    - Azure Machine Learning ワークスペース
    - Azure Storage
    - Azure Application Insights
    - Azure Container Registry
    - Azure Key Vault

    そのため、このプロセスには数分かかることがあります。

1. 既存のコンピューティングを選択するか、新しい Azure ML コンピューティングを作成します。 このプロセスは数分かかることがあります。
1. 既定の実験名のままにして、 **[作成]** を選択します。

    ![Azure ワークスペース セットアップ ダイアログ](./media/object-detection-model-builder/azure-dialog.png)

最初の実験が作成され、実験名がワークスペースに登録されます。 それ以降の実行は (同じ実験名を使用した場合)、同じ実験の一部としてログに記録されます。 そうでない場合は、新しい実験が作成されます。

構成に満足したら、Model Builder の **[次のステップ]** ボタンを選択して、 **[データ]** ステップに移動します。

## <a name="load-the-data"></a>データを読み込む

Model Builder の **[データ]** ステップで、トレーニング データセットを選択します。

>現在、Model Builder では、VoTT によって生成される JSON の形式のみが受け入れられますが、チームでは今後、サポートする形式を増やしていく予定です。 Model Builder でサポートされることを期待する物体検出用のデータセット形式がある場合は、[GitHub](https://github.com/dotnet/machinelearning-modelbuilder/issues/new?assignees=&labels=&template=data_suggestion.md&title=) でフィードバックをお寄せください。

1. **[フォルダーの選択]** テキスト ボックスの横にあるボタンを選択し、エクスプローラーを使用して、*Stop-Signs/vott-json-export* ディレクトリにある `StopSignObjDetection-export.json` を見つけます。

    ![Model Builder のデータ ステップ](./media/object-detection-model-builder/obj-det-data.png)

1. **データ プレビュー** でデータが正しく表示されている場合は、 **[次のステップ]** を選択して、 **[トレーニング]** ステップに進みます。

## <a name="train-the-model"></a>モデルをトレーニングする

次のステップは、モデルをトレーニングすることです。

Model Builder の **[トレーニング]** 画面で、 **[トレーニングの開始]** ボタンを選択します。

この時点で、データは Azure Storage にアップロードされ、Azure ML でトレーニング プロセスが開始されます。

>トレーニング プロセスには時間がかかり、この時間は選択したコンピューティングのサイズやデータ量によって変わります。 Azure でのモデルの初回トレーニング時は、リソースをプロビジョニングする必要があるため、トレーニング時間が若干長くなることが予想されます。 この 50 個の画像のサンプルでは、トレーニングに約 16 分かかりました。

Visual Studio で **[Azure portal で現在の実行を監視する]** リンクを選択すると、Azure Machine Learning ポータルで実行の進行状況を追跡できます。

トレーニングが完了したら、 **[次のステップ]** ボタンを選択して、 **[評価]** ステップに進みます。

## <a name="evaluate-the-model"></a>モデルを評価する

[評価] 画面には、モデルの精度など、トレーニング プロセスの結果の概要が表示されます。

![Model Builder の評価ステップ](./media/object-detection-model-builder/obj-det-evaluate.png)

この例では、精度が 100% と表示されていますが、これは、データセット内の画像が少なすぎるため、モデルが *オーバーフィット* している可能性がかなり高いことを意味します。

**[独自のモデルを試す]** 実験を使用して、モデルが想定どおりに実行されているかどうかをすばやく確認できます。

**[画像の参照]** を選択し、テスト画像 (できれば、モデルでトレーニングの一部として使用されていないもの) を提供します。

![Model Builder の独自のモデルを試す](./media/object-detection-model-builder/obj-det-try-model.png)

検出された各境界ボックスに表示されるスコアは、検出されたオブジェクトの **信頼度** を示します。 たとえば、上のスクリーンショットでは、一時停止標識を囲む境界ボックス上のスコアは、モデルで、検出されたオブジェクトは一時停止標識であることが 99% 確実であるとされていることを示しています。

**スコアしきい値** は、しきい値スライダーで増減でき、それらのスコアに基づいて検出されたオブジェクトが追加および削除されます。 たとえば、しきい値が 0.51 の場合、モデルには、信頼スコアが 0.51 以上のオブジェクトのみが表示されます。 しきい値を大きくするほど、検出されるオブジェクトが少なくなり、しきい値を小さくするほど、検出されるオブジェクトが多くなります。

精度のメトリックに不満がある場合、モデルの精度を高めるために試してみる簡単な 1 つの方法は、使用するデータを増やすことです。 そうでない場合は、 **[次のステップ]** リンクを選択して、Model Builder の **[コード]** ステップに進みます。

## <a name="add-the-code-to-make-predictions"></a>予測を行うコードを追加する

トレーニング プロセスの結果として 2 つのプロジェクトが作成されます。

- *StopSignDetectionML.ConsoleApp*: モデルのトレーニング コードとサンプルの使用に関するコードが含まれる C# .NET Core コンソール アプリケーション。
- *StopSignDetectionML.Model*: 入力および出力モデル データのスキーマを定義するデータ モデル、トレーニング時にパフォーマンスが最高のモデルの保存バージョン、および予測を行う `ConsumeModel` というヘルパー クラスを含む .NET Standard クラス ライブラリ。

1. Model Builder のコード ステップで、 **[プロジェクトの追加]** を選択して、自動生成されたプロジェクトをソリューションに追加します。
1. *StopSignDetection* プロジェクトで *Program.cs* ファイルを開き、ファイルの先頭に次の using ステートメントを追加して *StopSignDetectionML.Model* プロジェクトを参照します。

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L2)]

1. 次の[テスト画像](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/test-image1.jpeg)をダウンロードします。
1. `ImageSource` プロパティを、アプリケーションの `Main` メソッド内のテスト画像のファイルパスに設定して、`ModelInput` クラスの新しいインスタンスを作成します。 "Hello World" ステートメントを、次のコードに置き換えます。

    [!code-csharp [InputData](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L10-L15)]

1. `ConsumeModel` クラスの `Predict` メソッドを使用して、トレーニング済みモデルを読み込み、モデルの [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成して、新しいデータに対して予測を行います。 `ModelInput` ステートメントの下に次のコードを追加します。

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L15)]

1. 次のコードを追加して、ラベル、座標、スコアを含む予測の出力を生成します。

    [!code-csharp [PrintResults](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L17-L18)]

1. アプリケーションを実行します。

    プログラムによって生成される出力は次のスニペットのようになります。

    ```bash
    Predicted Boxes:

    Top: 89.453415, Left: 481.95343, Right: 724.8073, Bottom: 388.32385, Label: Stop-Sign, Score: 0.99539465
    ```

お疲れさまでした。 Model Builder を使用して、画像内の一時停止標識を検出する機械学習モデルを正常に構築しました。 このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub リポジトリにあります。

## <a name="additional-resources"></a>その他のリソース

このチュートリアルで説明しているトピックについて詳しくは、次のリソースを参照してください。

- [モデル ビルダーのシナリオ](../automate-training-with-model-builder.md#scenario)
- [ML.NET で ONNX を使用した物体検出](object-detection-onnx.md)
