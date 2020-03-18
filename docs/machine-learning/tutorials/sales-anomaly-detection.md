---
title: 'チュートリアル: 製品売上の異常を検出する'
description: 製品売上データの異常検出アプリケーションを構築する方法について説明します。 このチュートリアルでは、Visual Studio 2019 の C# を使って .NET Core コンソール アプリケーションを作成します。
ms.date: 11/15/2019
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: fe2904dee349f32feb115ea533adbb4b1d8b7140
ms.sourcegitcommit: 81ad1f09b93f3b3e6706a7f2e4ddf50ef229ea3d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74204941"
---
# <a name="tutorial-detect-anomalies-in-product-sales-with-mlnet"></a>チュートリアル: ML.NET で製品売上の異常を検出する

製品売上データの異常検出アプリケーションを構築する方法について説明します。 このチュートリアルでは、Visual Studio の C# を使って .NET Core コンソール アプリケーションを作成します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> * データを読み込む
> * スパイクの異常検出のために変換を作成する
> * 変換を使用してスパイクの異常を検出する
> * 変化点の異常検出のために変換を作成する
> * 変換を使用して変化点の異常を検出する

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection) リポジトリで確認できます。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

* [product-sales.csv データセット](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)

>[!NOTE]
> `product-sales.csv` のデータ形式は、DataMarket が出典であるデータセット "Shampoo Sales Over a Three Year Period" (過去 3 年間のシャンプーの売上) に基づいており、Rob Hyndman が作成した Time Series Data Library (TSDL) で提供されています。
> "Shampoo Sales Over a Three Year Period" データセットは、DataMarket Default Open License の下でライセンスを受けています。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. "ProductSalesAnomalyDetection" という **.NET Core コンソール アプリケーション**を作成します。

2. データ セット ファイルを保存するために、プロジェクトに *Data* という名前のディレクトリを作成します。

3. **Microsoft.ML NuGet パッケージ**をインストールします。

    ソリューション エクスプローラーで、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。[参照] タブを選択し、「**Microsoft.ML**」を検索し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。 **Microsoft.ML.TimeSeries** についてもこれらの手順を繰り返します。

4. *Program.cs* の先頭に次の `using` ステートメントを追加します。

    [!code-csharp[AddUsings](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a>データをダウンロードする

1. データセットをダウンロードし、以前に作成した *Data* フォルダーに保存します。

   * [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) を右クリックし、[名前を付けてリンクを保存 (またはターゲット)] を選択します。

     \*.csv ファイルを *Data* フォルダーに保存したことを確認します。または他の場所に保存した後に、\*.csv ファイルを *Data* フォルダーに移動します。

2. ソリューション エクスプローラーで、\*.csv ファイルを右クリックし、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

次の表は、\*.csv ファイルのデータのプレビューです。

|月  |ProductSales |
|-------|-------------|
|1-Jan  |    271      |
|2-Jan  |    150.9    |
|.....  |    .....    |
|1-Feb  |    199.3    |
|.....  |    .....    |

### <a name="create-classes-and-define-paths"></a>クラスを作成してパスを定義する

次に、入力クラスと予測クラスのデータ構造を定義します。

プロジェクトに新しいクラスを追加します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックして、 **[追加]、[新しいアイテム]** の順に選びます。

2. **[新しい項目の追加] ダイアログ ボックス**で、 **[クラス]** を選択し、 **[名前]** フィールドを「*ProductSalesData.cs*」に変更します。 次に **[追加]** を選択します。

   コード エディターで *ProductSalesData.cs* ファイルが開きます。

3. 次の `using` ステートメントを *ProductSalesData.cs* の先頭に追加します。

   ```csharp
   using Microsoft.ML.Data;
   ```

4. 既存のクラス定義を削除し、`ProductSalesData` と `ProductSalesPrediction` の 2 つのクラスを含む次のコードを *ProductSalesData.cs* ファイルに追加します。

    [!code-csharp[DeclareTypes](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/ProductSalesData.cs#DeclareTypes "Declare data record types")]

    `ProductSalesData` は入力データ クラスを指定します。 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性は、データセット内のどの列 (列インデックス) を読み込むかを指定します。

    `ProductSalesPrediction` では予測データ クラスを指定します。 異常検出の場合、予測は異常、生スコア、p 値があるかどうかを示すアラートで構成されます。 p 値が 0 に近いほど、異常が発生する可能性が高くなります。

5. 最近ダウンロードしたデータセット ファイルのパスと保存したモデル ファイルのパスを保持するために、2 つのグローバル フィールドを作成します。

    * `_dataPath` には、モデルのトレーニングに使用するデータ セットのパスが含まれます。
    * `_docsize` はデータセット ファイル内のレコード数です。 `_docSize` を `pvalueHistoryLength` の計算に使用します。

6. `Main` メソッドのすぐ上にある行に次のコードを追加して、それらのパスを指定します。

    [!code-csharp[Declare global variables](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DeclareGlobalVariables "Declare global variables")]

### <a name="initialize-variables-in-main"></a>Main で変数を初期化する

1. `Main` メソッドの `Console.WriteLine("Hello World!")` の行は、`mlContext` 変数を宣言して初期化する次のコードに置き換えます。

    [!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateMLContext "Create the ML Context")]

    [MLContext クラス](xref:Microsoft.ML.MLContext)は、すべての ML.NET 操作の開始点で、`mlContext` を初期化することで、モデル作成ワークフローのオブジェクト間で共有できる新しい ML.NET 環境が作成されます。 これは Entity Framework における `DBContext` と概念的には同じです。

### <a name="load-the-data"></a>データを読み込む

ML.NET 内のデータは、[IDataView クラス](xref:Microsoft.ML.IDataView)として表されます。 `IDataView` は、表形式のデータ (数値とテキスト) を表すための柔軟で効率的な方法です。 データはテキスト ファイルから、または他のソース (SQL データベースやログ ファイルなど) から `IDataView` オブジェクトに読み込むことができます。

1. `Main()` メソッドの次の行として次のコードを追加します。

    [!code-csharp[LoadData](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#LoadData "loading dataset")]

    [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) は、データ スキーマを定義し、ファイルを読み取ります。 データ パス変数を取得して、`IDataView` を返します。

## <a name="time-series-anomaly-detection"></a>時系列の異常検出

異常検出によって、予期しない、または通常とは異なるイベントや動作にフラグが立てられます。 これは、問題を探す場所の手がかりとなり、"これはおかしいだろうか" という問いの答えを見つけるために役立ちます。

!["これはおかしいだろうか" 異常検出の例。](./media/sales-anomaly-detection/time-series-anomaly-detection.png)

異常検出は、時系列データの異常値 (指定された入力の時系列上で、動作が予期されたものではない点、つまり "おかしい" 点) を検出するプロセスです。

異常検出は、さまざまな場面で役立ちます。 たとえば、次のようになります。

車を持っている場合に知りたいこと:このオイル ゲージの表示値は正常か。それとも漏れがあるか。
電力消費量を監視している場合に知りたいこと:停電はあるか。

検出できる時系列の異常には 2 つの種類があります。

* **スパイク**は、システム内の異常動作の一時的なバーストを示します。

* **変化点**は、システム内での長期にわたる永続的な変化の始まりを示します。

ML.NET では、[独立した同一分散のデータセット](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)には IID Spike Detection (IID スパイク検出) アルゴリズムまたは IID Change point Detection (IID 変化点検出) アルゴリズムが適しています。

他のチュートリアルのモデルとは異なり、時系列の異常検出器の変換は入力データで直接操作されます。 `IEstimator.Fit()` メソッドでは、変換を生成するためにトレーニング データを必要としません。 データ スキーマは必要ですが、`ProductSalesData` の空のリストから生成されたデータ ビューによって提供されます。

スパイクと変化点の検出には、同じ製品売上データを分析します。 構築とトレーニング モデル プロセスは、スパイク検出と変化点検出で同じです。主な違いは、使用される具体的な検出アルゴリズムです。

## <a name="spike-detection"></a>スパイク検出

スパイク検出の目的は、時系列データ値の大部分と大きく異なる突然の一時的なバーストを特定することです。 このような疑わしいまれな項目、イベント、または観測値を適時に検出して最小限に抑えることが重要です。 次のようなアプローチを利用すると、停電、サイバー攻撃、バイラル Web コンテンツなど、さまざまな異常を検出できます。 次の図は、時系列データセットのスパイクの例です。

![2 つのスパイク検出を示すスクリーンショット。](./media/sales-anomaly-detection/two-spike-detections.png)

### <a name="add-the-createemptydataview-method"></a>CreateEmptyDataView () メソッドを追加する

次のメソッドを `Program.cs` に追加します。

[!code-csharp[CreateEmptyDataView](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateEmptyDataView)]

`CreateEmptyDataView()` では、`IEstimator.Fit()` メソッドへの入力として使用される正しいスキーマで、空のデータ ビュー オブジェクトを生成します。

### <a name="create-the-detectspike-method"></a>DetectSpike() メソッドを作成します。

`DetectSpike()` メソッド:

* エスティメーターから変換を作成します。
* 売上データ履歴に基づいてスパイクを検出します。
* 結果を表示します。

1. `Main()` メソッドの直後に、次のコードを使用して `DetectSpike()` メソッドを作成します。

    ```csharp
    static void DetectSpike(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. スパイク検出のために、[IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator) を使用してモデルをトレーニングします。 それを次のコードを使用して `DetectSpike()` メソッドに追加します。

    [!code-csharp[AddSpikeTrainer](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddSpikeTrainer)]

1. `DetectSpike()` メソッドの次のコード行として以下を追加して、スパイク検出の変換を作成します。

    [!code-csharp[TrainModel1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TrainModel1)]

1. 次のコード行を追加して、`productSales` データを `DetectSpike()` メソッドの次の行に変換します。

    [!code-csharp[TransformData1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TransformData1)]

    前のコードでは、[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) メソッドを使用して、データセットの複数の入力行の予測を行います。

1. 表示を簡単にするために、次のコードを使用し、[CreateEnumerable()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) メソッドを使って `transformedData` を厳密に型指定された `IEnumerable` に変換します。

    [!code-csharp[CreateEnumerable1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateEnumerable1)]

1. 次の <xref:System.Console.WriteLine?displayProperty=nameWithType> コードを使用して表示ヘッダー行を作成します。

    [!code-csharp[DisplayHeader1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayHeader1)]

    スパイクの検出結果には、次の情報が表示されます。

    * `Alert` は、特定のデータ ポイントに対するスパイク アラートを示します。
    * `Score`は、データセット内の特定のデータ ポイントに対する `ProductSales` 値です。
    * `P-Value` "P" は確率を表します。 p 値が 0 に近いほど、データ ポイントが異常になる可能性が高くなります。

1. 次のコードを使用して `predictions` `IEnumerable` を反復処理し、結果を表示します。

    [!code-csharp[DisplayResults1](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayResults1)]

1. `DetectSpike()` メソッドに対する呼び出しを `Main()` メソッドに追加します。

    [!code-csharp[CallDetectSpike](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CallDetectSpike)]

## <a name="spike-detection-results"></a>スパイクの検出結果

結果は以下のようになるはずです。 処理中にメッセージが表示されます。 警告または処理メッセージが表示されることがありますが、 わかりやすくするために、一部のメッセージは次の結果から削除してあります。

```console
Detect temporary changes in pattern
=============== Training the model ===============
=============== End of training process ===============
Alert   Score   P-Value
0       271.00  0.50
0       150.90  0.00
0       188.10  0.41
0       124.30  0.13
0       185.30  0.47
0       173.50  0.47
0       236.80  0.19
0       229.50  0.27
0       197.80  0.48
0       127.90  0.13
1       341.50  0.00 <-- Spike detected
0       190.90  0.48
0       199.30  0.48
0       154.50  0.24
0       215.10  0.42
0       278.30  0.19
0       196.40  0.43
0       292.00  0.17
0       231.00  0.45
0       308.60  0.18
0       294.90  0.19
1       426.60  0.00 <-- Spike detected
0       269.50  0.47
0       347.30  0.21
0       344.70  0.27
0       445.40  0.06
0       320.90  0.49
0       444.30  0.12
0       406.30  0.29
0       442.40  0.21
1       580.50  0.00 <-- Spike detected
0       412.60  0.45
1       687.00  0.01 <-- Spike detected
0       480.30  0.40
0       586.30  0.20
0       651.90  0.14
```

## <a name="change-point-detection"></a>変化点検出

`Change points` は、レベルの変化や傾向など、時系列のイベント ストリームの値分布の永続的な変化です。 こうした永続的な変化は、`spikes` よりもはるかに長く続き、壊滅的なイベントを示している可能性があります。 通常、`Change points` は目視ではわかりませんが、次のような方法を使用してデータから検出できます。  次の図は、変化点検出の例です。

![2 つの変更点検出を示すスクリーンショット。](./media/sales-anomaly-detection/change-point-detection.png)

### <a name="create-the-detectchangepoint-method"></a>DetectChangepoint() メソッドを作成します。

`DetectChangepoint()` メソッドは次のタスクを実行します。

* エスティメーターから変換を作成します。
* 売上データ履歴に基づいて変更点を検出します。
* 結果を表示します。

1. `Main()` メソッドの直後に、次のコードを使用して `DetectChangepoint()` メソッドを作成します。

    ```csharp
    static void DetectChangepoint(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. 次のコードを使って `DetectChangepoint()` メソッドで [iidChangePointEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator) を作成します。

    [!code-csharp[AddChangepointTrainer](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#AddChangepointTrainer)]

1. 前に行ったように、`DetectChangePoint()` メソッドに次のコード行を追加して、エスティメーターから変換を作成します。

    [!code-csharp[TrainModel2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TrainModel2)]

1. `Transform()` メソッドを使用して、次のコードを `DetectChangePoint()` に追加してデータを変換します。

    [!code-csharp[TransformData2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#TransformData2)]

1. 以前と同様に、表示を簡単にするために、次のコードを使用し、`CreateEnumerable()` メソッドを使って `transformedData` を厳密に型指定された `IEnumerable` に変換します。

    [!code-csharp[CreateEnumerable2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CreateEnumerable2)]

1. `DetectChangePoint()` メソッドの次の行として次のコードを使用して、表示ヘッダーを作成します。

    [!code-csharp[DisplayHeader2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayHeader2)]

    変化点の検出結果には、次の情報が表示されます。

    * `Alert` は、特定のデータ ポイントに対する変化点アラートを示します。
    * `Score`は、データセット内の特定のデータ ポイントに対する `ProductSales` 値です。
    * `P-Value` "P" は確率を表します。 P 値が 0 に近いほど、データ ポイントが異常になる可能性が高くなります。
    * `Martingale value` は、一連の P 値に基づいて、データ ポイントがどの程度 "おかしい" かを特定するために使用されます。

1. 次のコードを使用して `predictions` `IEnumerable` を反復処理し、結果を表示します。

    [!code-csharp[DisplayResults2](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#DisplayResults2)]

1. 次の `DetectChangepoint()` メソッドに対する呼び出しを `Main()` メソッドに追加します。

    [!code-csharp[CallDetectChangepoint](~/samples/machine-learning/tutorials/ProductSalesAnomalyDetection/Program.cs#CallDetectChangepoint)]

## <a name="change-point-detection-results"></a>変化点の検出結果

結果は以下のようになるはずです。 処理中にメッセージが表示されます。 警告または処理メッセージが表示されることがありますが、 わかりやすくするために、一部のメッセージは次の結果から削除してあります。

```console
Detect Persistent changes in pattern
=============== Training the model Using Change Point Detection Algorithm===============
=============== End of training process ===============
Alert   Score   P-Value Martingale value
0       271.00  0.50    0.00
0       150.90  0.00    2.33
0       188.10  0.41    2.80
0       124.30  0.13    9.16
0       185.30  0.47    9.77
0       173.50  0.47    10.41
0       236.80  0.19    24.46
0       229.50  0.27    42.38
1       197.80  0.48    44.23 <-- alert is on, predicted changepoint
0       127.90  0.13    145.25
0       341.50  0.00    0.01
0       190.90  0.48    0.01
0       199.30  0.48    0.00
0       154.50  0.24    0.00
0       215.10  0.42    0.00
0       278.30  0.19    0.00
0       196.40  0.43    0.00
0       292.00  0.17    0.01
0       231.00  0.45    0.00
0       308.60  0.18    0.00
0       294.90  0.19    0.00
0       426.60  0.00    0.00
0       269.50  0.47    0.00
0       347.30  0.21    0.00
0       344.70  0.27    0.00
0       445.40  0.06    0.02
0       320.90  0.49    0.01
0       444.30  0.12    0.02
0       406.30  0.29    0.01
0       442.40  0.21    0.01
0       580.50  0.00    0.01
0       412.60  0.45    0.01
0       687.00  0.01    0.12
0       480.30  0.40    0.08
0       586.30  0.20    0.03
0       651.90  0.14    0.09
```

おめでとうございます! これで、売上データのスパイクや変化点の異常を検出するための機械学習モデルを適切に構築できました。

このチュートリアルのソース コードは [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/ProductSalesAnomalyDetection) リポジトリで確認できます。

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> * データを読み込む
> * スパイクの異常検出のためにモデルをトレーニングする
> * トレーニング済みモデルを使用してスパイクの異常を検出する
> * 変化点の異常検出のためにモデルをトレーニングする
> * トレーニング済みモデルを使用して変化点の異常を検出する

## <a name="next-steps"></a>次の手順

Machine Learning サンプルの GitHub リポジトリを確認し、Power Consumption Anomaly Detection サンプルを調べてください。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub リポジトリ](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/AnomalyDetection_PowerMeterReadings)
