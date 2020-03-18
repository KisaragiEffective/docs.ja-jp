---
title: ML.NET の概要とそのしくみ
description: ML.NET を使用すると、オンラインまたはオフラインのどちらのシナリオでも、.NET アプリケーションに機械学習を追加できます。 この機能により、データを使った自動予測をアプリケーションで利用できるようになります。ML.NET を使うためにネットワークに接続する必要はありません。 この記事では、ML.NET の機械学習の基本について説明します。
ms.date: 11/5/2019
ms.topic: overview
ms.custom: mvc
ms.openlocfilehash: 169250adf81992ad0025e78eb9c8f151107bcf40
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79185862"
---
# <a name="what-is-mlnet-and-how-does-it-work"></a>ML.NET の概要とそのしくみ

ML.NET を使用すると、オンラインまたはオフラインのどちらのシナリオでも、.NET アプリケーションに機械学習を追加できます。 この機能により、データを使う自動予測をアプリケーションに利用できるようになります。 機械学習アプリケーションでは、明示的なプログラミングを必要とする代わりに、データ内のパターンを利用して予測を行います。

ML.NET の中心となるのは、機械学習**モデル**です。 このモデルでは、入力データを予測に変換するために必要な手順が指定されます。 .ML.NET を使用すると、アルゴリズムを指定してカスタム モデルをトレーニングすることができます。または、事前トレーニング済みの TensorFlow および ONNX モデルをインポートすることもできます。

モデルを用意したら、それをアプリケーションに追加して予測を行うことができます。

ML.NET は、.NET Core を使用して Windows、Linux、macOS 上で動作します。また、.NET Framework を使用して Windows 上で動作します。 64 ビットはすべてのプラットフォームでサポートされています。 TensorFlow、LightGBM、および ONNX 関連の機能を除き、32 ビットは Windows 上でサポートされています。

ML.NET で行うことができる予測の種類の例を次に示します。

|||
|-|-|
|分類/カテゴリ|顧客からのフィードバックを肯定的なカテゴリと否定的なカテゴリに自動的に分割します|
|回帰/予測の連続値|サイズと場所に基づいて住宅の価格を予測する|
|異常検出|不正な銀行取引を検出する |
|Recommendations|以前の購入に基づいて、オンラインの顧客が購入する商品を提案します|
|時系列/シーケンシャル データ|天気/製品売上を予測する|
|イメージ分類|病状を医療画像で分類する|

## <a name="hello-mlnet-world"></a>ML.NET の基本

次のスニペットのコードは、最も簡単な ML.NET アプリケーションの例です。 この例では、住宅のサイズと価格のデータを使用して住宅価格を予測する線形回帰モデルを構築します。

 ```csharp
    using System;
    using Microsoft.ML;
    using Microsoft.ML.Data;

    class Program
    {
        public class HouseData
        {
            public float Size { get; set; }
            public float Price { get; set; }
        }

        public class Prediction
        {
            [ColumnName("Score")]
            public float Price { get; set; }
        }

        static void Main(string[] args)
        {
            MLContext mlContext = new MLContext();

            // 1. Import or create training data
            HouseData[] houseData = {
                new HouseData() { Size = 1.1F, Price = 1.2F },
                new HouseData() { Size = 1.9F, Price = 2.3F },
                new HouseData() { Size = 2.8F, Price = 3.0F },
                new HouseData() { Size = 3.4F, Price = 3.7F } };
            IDataView trainingData = mlContext.Data.LoadFromEnumerable(houseData);

            // 2. Specify data preparation and model training pipeline
            var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
                .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));

            // 3. Train model
            var model = pipeline.Fit(trainingData);

            // 4. Make a prediction
            var size = new HouseData() { Size = 2.5F };
            var price = mlContext.Model.CreatePredictionEngine<HouseData, Prediction>(model).Predict(size);

            Console.WriteLine($"Predicted price for size: {size.Size*1000} sq ft= {price.Price*100:C}k");

            // Predicted price for size: 2500 sq ft= $261.98k
        }
    }
```

## <a name="code-workflow"></a>コードのワークフロー

次の図は、アプリケーション コードの構造とモデル開発の反復処理を表しています。

- トレーニング データを収集して **IDataView** オブジェクトに読み込む
- 特徴を抽出し、機械学習アルゴリズムを適用するように操作のパイプラインを指定する
- パイプラインに対して **Fit()** を呼び出してモデルをトレーニングする
- モデルを評価し、反復処理で改善する
- アプリケーションで使用できるようにモデルをバイナリ形式で保存する
- モデルを **ITransformer** オブジェクトに読み込む
- **CreatePredictionEngine.Predict()** を呼び出して予測を行う

![データ生成、パイプライン開発、モデル トレーニング、モデル評価、およびモデル使用のためのコンポーネントを含む ML.NET アプリケーション開発フロー](./media/mldotnet-annotated-workflow.png)

これらの概念についてもう少し詳しく掘り下げてみましょう。

## <a name="machine-learning-model"></a>Machine Learning モデル

ML.NET モデルは、予測される出力に到達するために入力データに対して実行される変換を含むオブジェクトです。

### <a name="basic"></a>Basic

最も基本的なモデルは、前述の住宅価格の例のように、ある連続的な数量が別の連続的な数量と比例する 2 次元の線形回帰です。

![バイアスと重みのパラメーターがある線形回帰モデル](./media/linear-regression-model.svg)

このモデルは $Price = b + Size * w$ のようにシンプルです。 パラメーター $b$ と $w$ は、(サイズ、価格) ペアのセットに直線を合わせることで推定されます。 モデルのパラメーターを見つけるために使用されるデータは、**トレーニング データ**と呼ばれます。 機械学習モデルの入力は**特徴**と呼ばれます。 この例では、$Size$ が唯一の特徴です。 機械学習モデルのトレーニングに使用される真値は、**ラベル**と呼ばれます。 ここでは、トレーニング データ セットの $Price$ 値がラベルです。

### <a name="more-complex"></a>より複雑

より複雑なモデルでは、取引の説明文を使用して金融取引をカテゴリに分類します。

各取引の説明を特徴セットに分類するには、冗長な単語と文字を削除し、単語と文字の組み合わせを数えます。 特徴セットは、トレーニング データ内の一連のカテゴリに基づいて線形モデルをトレーニングするために使用されます。 新しい説明がトレーニング セット内の説明に似ているほど、同じカテゴリに割り当てられる可能性が高くなります。

![テキスト分類モデル](./media/text-classification-model.svg)

住宅価格モデルとテキスト分類モデルはどちらも**線形**モデルです。 データの性質や解決対象の問題に応じて、**デシジョン ツリー** モデル、**一般化加法**モデルなどを使用することもできます。 モデルの詳細については、[タスク](./resources/tasks.md)に関する記事を参照してください。

## <a name="data-preparation"></a>データの準備

ほとんどの場合、利用できるデータは、機械学習モデルのトレーニングにそのまま使用するために適していません。 生データは使用前に準備するか前処理して、モデルのパラメーターを見つける必要があります。 文字列値から数値表現へのデータの変換が必要な場合があります。 入力データには冗長な情報が含まれる場合があります。 入力データの次元の縮小または拡大が必要な場合があります。 データの正規化またはスケールが必要な場合があります。

「[ML.NET のチュートリアル](./tutorials/index.md)」では、特定の機械学習タスクに使用されるテキスト、画像、数値、および時系列データ用のさまざまなデータ処理パイプラインが説明されています。

[データの準備方法](./how-to-guides/prepare-data-ml-net.md)に関する記事では、データ準備を適用する方法が全般的に説明されています。

すべての[使用できる変換](./resources/transforms.md)の付録については、リソース セクションを参照してください。

## <a name="model-evaluation"></a>モデル評価

トレーニングを完了したモデルが今後の予測にどのくらい役立つかは、どうすればわかるでしょうか。 ML.NET を使用すると、いくつかの新しいテスト データに対してモデルを評価できます。

機械学習タスクの種類ごとに、テスト データ セットに対するモデルの正確度と精度の評価に使用されるメトリックがあります。

住宅価格の例では、**回帰**タスクを使用しました。 このモデルを評価するには、元のサンプルに次のコードを追加します。

```csharp
        HouseData[] testHouseData =
        {
            new HouseData() { Size = 1.1F, Price = 0.98F },
            new HouseData() { Size = 1.9F, Price = 2.1F },
            new HouseData() { Size = 2.8F, Price = 2.9F },
            new HouseData() { Size = 3.4F, Price = 3.6F }
        };

        var testHouseDataView = mlContext.Data.LoadFromEnumerable(testHouseData);
        var testPriceDataView = model.Transform(testHouseDataView);

        var metrics = mlContext.Regression.Evaluate(testPriceDataView, labelColumnName: "Price");

        Console.WriteLine($"R^2: {metrics.RSquared:0.##}");
        Console.WriteLine($"RMS error: {metrics.RootMeanSquaredError:0.##}");

        // R^2: 0.96
        // RMS error: 0.19
```

評価メトリックから、誤差が少ないことと、予測される出力とテスト出力の間の相関度が高いことがわかります。 簡単でしたね。 実際の例で優れたモデル メトリックを実現するには、さらに調整が必要です。

## <a name="mlnet-architecture"></a>ML.NET のアーキテクチャ

このセクションでは、ML.NET のアーキテクチャ パターンについて説明します。 経験豊富な .NET 開発者であれば、なじみのあるパターンと、あまりなじみのないパターンがあるでしょう。 詳しく説明しますので、ついてきてください。

ML.NET アプリケーションは <xref:Microsoft.ML.MLContext> オブジェクトから始まります。 このシングルトン オブジェクトには**カタログ**が含まれます。 カタログは、データの読み込みと保存、変換、トレーナー、およびモデル運用コンポーネントのためのファクトリです。 各カタログ オブジェクトには、さまざまな種類のコンポーネントを作成するメソッドがあります。

|||||
|-|-|-|-|
|データの読み込みと保存||<xref:Microsoft.ML.DataOperationsCatalog>||
|データの準備||<xref:Microsoft.ML.TransformsCatalog>||
|トレーニングのアルゴリズム|二項分類|<xref:Microsoft.ML.BinaryClassificationCatalog>||
||多クラス分類|<xref:Microsoft.ML.MulticlassClassificationCatalog>||
||異常検出|<xref:Microsoft.ML.AnomalyDetectionCatalog>||
||クラスタリング|<xref:Microsoft.ML.ClusteringCatalog>||
||予測|<xref:Microsoft.ML.ForecastingCatalog>||
||ランキング|<xref:Microsoft.ML.RankingCatalog>||
||回帰|<xref:Microsoft.ML.RegressionCatalog>||
||推奨|<xref:Microsoft.ML.RecommendationCatalog>|`Microsoft.ML.Recommender` NuGet パッケージを取得する|
||TimeSeries|<xref:Microsoft.ML.TimeSeriesCatalog>|`Microsoft.ML.TimeSeries` NuGet パッケージを取得する|
|モデルの使用法 ||<xref:Microsoft.ML.ModelOperationsCatalog>||

上記の各カテゴリの作成方法に移動できます。 Visual Studio を使用すると、IntelliSense を介してカタログが表示されます。

   ![回帰トレーナー用の IntelliSense](./media/catalog-intellisense.png)

### <a name="build-the-pipeline"></a>パイプラインを構築する

各カタログ内には、一連の拡張メソッドがあります。 トレーニング パイプラインを作成するために拡張メソッドがどのように使用されるかを見てみましょう。

```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
        .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));
```

このスニペットで、`Concatenate` と `Sdca` はどちらもカタログ内のメソッドです。 それぞれ、パイプラインに追加される [IEstimator](xref:Microsoft.ML.IEstimator%601) オブジェクトが作成されます。

この時点では、オブジェクトが作成されるだけです。 実行は行われません。

### <a name="train-the-model"></a>モデルをトレーニングする

パイプライン内にオブジェクトが作成されたら、データを使用してモデルをトレーニングできます。

```csharp
    var model = pipeline.Fit(trainingData);
```

`Fit()` を呼び出すと、入力トレーニング データを使用してモデルのパラメーターが推定されます。 これはモデルのトレーニングと呼ばれます。 前述の線形回帰モデルには、**バイアス**と**重み**という 2 つのモデル パラメーターがあったことを思い出してください。 `Fit()` の呼び出しの後は、パラメーターの値がわかっています。 ほとんどのモデルには、これよりもさらに多くのパラメーターがあります。

モデルのトレーニングの詳細については、[モデルのトレーニング方法](./how-to-guides/train-machine-learning-model-ml-net.md)に関するページをご覧ください。

結果のモデル オブジェクトには <xref:Microsoft.ML.ITransformer> インターフェイスが実装されます。 つまり、このモデルによって入力データは予測に変換されます。

```csharp
   IDataView predictions = model.Transform(inputData);
```

### <a name="use-the-model"></a>モデルを使用する

入力データを一括して予測に変換するか、入力を 1 つずつ変換することができます。 住宅価格の例では、その両方を行いました。つまり、モデルの評価を目的とした一括処理と、新しい予測を行うための個別処理です。 1 つの予測を見てみましょう。

```csharp
    var size = new HouseData() { Size = 2.5F };
    var predEngine = mlContext.CreatePredictionEngine<HouseData, Prediction>(model);
    var price = predEngine.Predict(size);
```

`CreatePredictionEngine()` メソッドは、入力クラスと出力クラスを受け取ります。 このフィールドの名前やコード属性によって、モデルのトレーニングと予測中に使用されるデータ列の名前が決まります。 方法のセクションで [1 つの予測方法](./how-to-guides/single-predict-model-ml-net.md)に関する記事を参照してください。

### <a name="data-models-and-schema"></a>データ モデルとスキーマ

ML.NET 機械学習パイプラインの中心には [DataView](xref:Microsoft.ML.IDataView) オブジェクトがあります。

パイプライン内の各変換には、入力スキーマ (変換で入力にあると想定されているデータの名前、型、およびサイズ) と、出力スキーマ (変換によって変換後に生成されるデータの名前、型、およびサイズ) があります。 次のドキュメントでは、[IDataView インターフェイスとその型システム](https://xadupre.github.io/machinelearningext/mlnetdocs/idataviewtypesystem.html)について詳しく説明しています。

パイプライン内の 1 つの変換からの出力スキーマが次の変換の入力スキーマと一致しない場合、ML.NET から例外がスローされます。

データ ビュー オブジェクトには列と行があります。 各列には、名前、型、および長さがあります。 たとえば、住宅価格例の入力列は **Size** と **Price** です。 これらはどちらも型であり、ベクターではなくスカラーの数量です。

   ![住宅価格の予測データを含む ML.NET データ ビューの例](./media/ml-net-dataview.png)

すべての ML.NET アルゴリズムは、ベクターである入力列を探します。 既定では、このベクター列は **Features** という名前です。 住宅価格の例で、**Size** 列を **Features** という新しい列に連結したのはこのためです。

 ```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
 ```

予測を実行した後は、いずれのアルゴリズムでも新しい列が作成されます。 このような新しい列の固定名は、機械学習アルゴリズムの種類によって異なります。 回帰タスクでは、新しい列の 1 つは **Score** という名前です。 価格データにこの名前を付けたのはこのためです。

```csharp
    public class Prediction
    {
        [ColumnName("Score")]
        public float Price { get; set; }
    }
```

さまざまな機械学習タスクの出力列の詳細については、[機械学習のタスク](resources/tasks.md)に関するガイドを参照してください。

DataView オブジェクトの重要なプロパティは、**遅延**評価されることです。 データ ビューは、モデルのトレーニングと評価、およびデータの予測中にのみ読み込まれ、操作されます。 ML.NET アプリケーションの作成とテストを行っている間は、[Preview](xref:Microsoft.ML.DebuggerExtensions.Preview*) メソッドを呼び出すことで、Visual Studio デバッガーを使用して任意のデータ ビュー オブジェクトを確認することができます。

```csharp
    var debug = testPriceDataView.Preview();
```

`debug` 変数はデバッガーで監視し、その内容を調べることができます。 運用環境のコードでは、パフォーマンスが大幅に低下するので Preview メソッドを使用しないでください。

### <a name="model-deployment"></a>モデル デプロイ

実際のアプリケーションでは、モデル トレーニングと評価のコードは予測とは別になります。 実際、これら 2 つの活動は、別のチームによって行われることがよくあります。 モデル開発チームは、予測アプリケーションで使用するためにモデルを保存することができます。

```csharp
   mlContext.Model.Save(model, trainingData.Schema,"model.zip");
```

## <a name="next-steps"></a>次のステップ

* [チュートリアル](./tutorials/index.md)で、より現実的なデータ セットと共にさまざまな機械学習タスクを使用してアプリケーションを構築する方法を学習します。

* [ハウツー ガイド](./how-to-guides/index.md)で、特定のトピックについてさらに詳しく学習します。

* さらに詳しく学習するには、[API リファレンスのドキュメント](https://docs.microsoft.com/dotnet/api/?view=ml-dotnet)を参照してください。
