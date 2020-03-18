---
title: 'チュートリアル: あやめ花の分類 - K-means クラスタリング'
description: クラスタリングのシナリオで ML.NET を使用する方法について説明します
author: pkulikov
ms.date: 11/15/2019
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: 174907adac5741d5cc7d02cb134921debc586061
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241092"
---
# <a name="tutorial-categorize-iris-flowers-using-k-means-clustering-with-mlnet"></a>チュートリアル: ML.NET でクラスタリングを使用してあやめの花を分類する

このチュートリアルでは、ML.NET を使って[あやめのデータ セット](https://en.wikipedia.org/wiki/Iris_flower_data_set)の[クラスタリング モデル](../resources/tasks.md#clustering)を作成する方法を示します。

このチュートリアルでは、次の作業を行う方法について説明します。
> [!div class="checklist"]
>
> - 問題を把握する
> - 適切な機械学習タスクを選択する
> - データを準備する
> - データを読み込んで変換する
> - 学習アルゴリズムを選択する
> - モデルをトレーニングする
> - モデルを使用して予測を行う

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2017 バージョン 15.6 以降](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)が ".NET Core クロスプラットフォーム開発" ワークロードと共にインストールされている。

## <a name="understand-the-problem"></a>問題を把握する

この問題は、アヤメの花のセットを特徴に基づいて異なるグループに分類するというものです。 対象となる特徴は、がくの長さと幅および花弁の長さと幅です。 このチュートリアルでは、各花の種類は不明であるものとします。 特徴からデータ セットの構造を理解し、データのインスタンスがこの構造にどのように当てはまるかを予測します。

## <a name="select-the-appropriate-machine-learning-task"></a>適切な機械学習タスクを選択する

各花がどのグループに属するかわからないので、[教師なし機械学習](../resources/glossary.md#unsupervised-machine-learning)タスクを選択します。 同じグループ内の要素同士の方が他のグループの要素より似ているようにデータ セットをグループに分割するので、[クラスタリング](../resources/tasks.md#clustering)機械学習タスクを使用します。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. Visual Studio を開きます。 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** をメニュー バーから選択します。 **[新しいプロジェクト]** ダイアログで、 **[Visual C#]** ノードを選択し、 **[.NET Core]** ノードを選択します。 次に、 **[コンソール アプリ (.NET Core)]** プロジェクト テンプレートを選択します。 **[名前]** テキスト ボックスに「IrisFlowerClustering」と入力し、 **[OK]** ボタンを選びます。

1. プロジェクトに *Data* という名前のディレクトリを作成して、データ セットとモデルのファイルを保存します。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しいフォルダー]** を選択します。 「Data」と入力して Enter キーを押します。

1. **Microsoft.ML** NuGet パッケージをインストールします。

    **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 [パッケージ ソース] として [nuget.org] を選択します。 **[参照]** タブを選択し、「**Microsoft.ML**」を検索し、 **[インストール]** ボタンを選択します。 **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

## <a name="prepare-the-data"></a>データを準備する

1. [iris.data](https://github.com/dotnet/machinelearning/blob/master/test/data/iris.data) データ セットをダウンロードし、前の手順で作成した *Data* フォルダーに保存します。 あやめのデータ セットについて詳しくは、Wikipedia の「[Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)」(あやめのデータ セット) のページと、データ セットのソースである「[Iris Data Set](https://archive.ics.uci.edu/ml/datasets/Iris)」(あやめのデータ セット) のページをご覧ください。

1. **ソリューション エクスプローラー**で、*iris.data* ファイルを右クリックして、 **[プロパティ]** を選択します。 **[詳細設定]** で、 **[出力ディレクトリにコピー]** の値を **[新しい場合はコピーする]** に変更します。

*Iris.data* ファイルには、次の値を表す 5 つの列が含まれます。

- がくの長さ (センチメートル単位)
- がくの幅 (センチメートル単位)
- 花弁の長さ (センチメートル単位)
- 花弁の幅 (センチメートル単位)
- アヤメの種類

クラスタリングの例が目的なので、このチュートリアルでは最後の列を無視します。

## <a name="create-data-classes"></a>データ クラスを作成する

入力データと予測のためのクラスを作成します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを「*IrisData.cs*」に変更します。 次に **[追加]** を選択します。
1. 以下の `using` ディレクティブを新しいファイルに追加します。

   [!code-csharp[Add necessary usings](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/IrisData.cs#Usings)]

既存のクラス定義を削除し、`IrisData` クラスと `ClusterPrediction` クラスを定義する次のコードを *IrisData.cs* ファイルに追加します。

[!code-csharp[Define data classes](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/IrisData.cs#ClassDefinitions)]

`IrisData` は入力データ クラスであり、データ セットの各特徴の定義が含まれます。 データ セット ファイル内のソース列のインデックスを指定するには、[LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute) 属性を使用します。

`ClusterPrediction` クラスは、`IrisData` インスタンスに適用されたクラスタリング モデルの出力を表します。 [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute) 属性を使って、`PredictedClusterId` フィールドを **PredictedLabel** 列に、`Distances` フィールドを **Score** 列に、それぞれバインドします。 クラスタリング タスクの場合、これらの列には次の意味があります。

- **PredictedLabel** 列には、予測されたクラスターの ID が含まれます。
- **Score** 列には、クラスターの重心へのユークリッド距離を二乗した値の配列が含まれます。 配列の長さは、クラスターの数と同じです。

> [!NOTE]
> 入力データと予測データのクラス内の浮動小数点値を表すには、`float` 型を使います。

## <a name="define-data-and-model-paths"></a>データおよびモデルのパスを定義する

*Program.cs* ファイルに戻り、データ セット ファイルと、モデルを保存するファイルへのパスを保持するために、2 つのフィールドを追加します。

- `_dataPath` には、モデルのトレーニングに使用するデータ セットがあるファイルへのパスが含まれます。
- `_modelPath` には、トレーニング済みモデルが格納されるファイルへのパスが含まれます。

`Main` メソッドのすぐ上に次のコードを追加して、それらのパスを指定します。

[!code-csharp[Initialize paths](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#Paths)]

上記のコードをコンパイルするには、*Program.cs* ファイルの先頭に次の `using` ディレクティブを追加します。

[!code-csharp[Add usings for paths](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#UsingsForPaths)]

## <a name="create-ml-context"></a>ML のコンテキストの作成

以下の追加の `using` ディレクティブを、*Program.cs* ファイルの先頭に追加します。

[!code-csharp[Add Microsoft.ML usings](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#MLUsings)]

`Main` メソッドの `Console.WriteLine("Hello World!");` 行を次のコードで置き換えます。

[!code-csharp[Create ML context](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#CreateContext)]

<xref:Microsoft.ML.MLContext?displayProperty=nameWithType> クラスは機械学習環境を表し、ログ記録のメカニズムとデータの読み込みのエントリ ポイント、モデルのトレーニング、予測、およびその他のタスクを提供します。 これは、概念的には Entity Framework での `DbContext` の使用に相当します。

## <a name="set-up-data-loading"></a>データの読み込みの設定

`Main` メソッドに次のコードを追加して、データを読み込む方法を設定します。

[!code-csharp[Create text loader](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#CreateDataView)]

ジェネリック [`MLContext.Data.LoadFromTextFile` 拡張メソッド](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29)は、指定された `IrisData` 型からデータ セット スキーマを推測し、<xref:Microsoft.ML.IDataView> を返します。これはトランスフォーマーの入力として使用できます。

## <a name="create-a-learning-pipeline"></a>学習パイプラインを作成する

このチュートリアルでは、クラスタリング タスクの学習パイプラインは、次の 2 つの手順で構成されています。

- 読み込まれた列を、クラスタリング トレーナーによって使用される 1 つの **Features** 列に連結します。
- <xref:Microsoft.ML.Trainers.KMeansTrainer> トレーナーを使用して、K- 平均法++ クラスタリング アルゴリズムを使用するモデルをトレーニングします。

`Main` メソッドに次のコードを追加します。

[!code-csharp[Create pipeline](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#CreatePipeline)]

このコードは、データ セットを 3 つのクラスターに分割することを指定します。

## <a name="train-the-model"></a>モデルをトレーニングする

前のセクションで追加した手順では、トレーニング用にパイプラインを準備しましたが、トレーニングは実行されていません。 次の行を `Main` メソッドに追加して、データの読み込みとモデルのトレーニングを実行します。

[!code-csharp[Train the model](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#TrainModel)]

### <a name="save-the-model"></a>モデルを保存する

この時点で、既存または新規のどの .NET アプリケーションにも統合できるモデルができました。 モデルを .zip ファイルに保存するには、次のコードを `Main` メソッドに追加します。

[!code-csharp[Save the model](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#SaveModel)]

## <a name="use-the-model-for-predictions"></a>モデルを使用して予測を行う

予測を行うには、トランスフォーマー パイプラインを介して入力の型のインスタンスを受け取り、出力の型のインスタンスを生成する <xref:Microsoft.ML.PredictionEngine%602> クラスを使用します。 次の行を `Main` メソッドに追加して、そのクラスのインスタンスを作成します。

[!code-csharp[Create predictor](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#Predictor)]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) は、データの 1 つのインスタンスに対して予測を実行できる便利な API です。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) はスレッド セーフではありません。 シングル スレッド環境またはプロトタイプ環境で使用できます。 運用環境でパフォーマンスとスレッド セーフを向上させるには、`PredictionEnginePool` サービスを使用します。これにより、アプリケーション全体で使用するできる [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) オブジェクトの [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) が作成されます。 [ASP.NET Core Web API で `PredictionEnginePool` を使用する](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)方法については、こちらのガイドを参照してください。

> [!NOTE]
> `PredictionEnginePool` サービスの拡張機能は、現在プレビュー段階です。

テスト データのインスタンスを格納するための `TestIrisData` クラスを作成します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。
1. **[新しい項目の追加]** ダイアログ ボックスで、 **[クラス]** を選択し、 **[名前]** フィールドを「*TestIrisData.cs*」に変更します。 次に **[追加]** を選択します。
1. 次の例に示すように、静的になるようにクラスを変更します。

   [!code-csharp[Make class static](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/TestIrisData.cs#Static)]

このチュートリアルでは、このクラスで 1 つのあやめのデータ インスタンスを示します。 他のシナリオを追加してモデルで実験できます。 `TestIrisData` クラスに次のコードを追加します。

[!code-csharp[Test data](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/TestIrisData.cs#TestData)]

指定した項目が属するクラスターを発見するには、*Program.cs* ファイルに戻り、次のコードを `Main` メソッドに追加します。

[!code-csharp[Predict and output results](~/samples/snippets/machine-learning/IrisFlowerClustering/csharp/Program.cs#PredictionExample)]

プログラムを実行し、指定したデータ インスタンスが含まれるクラスターと、そのインスタンスからクラスターの重心までの平方距離を確認します。 結果は以下のようになるはずです。

```text
Cluster: 2
Distances: 11.69127 0.02159119 25.59896
```

おめでとうございます! これで、アヤメのクラスタリングの機械学習モデルを作成し、それを使用して予測を行うことができました。 このチュートリアルのソース コードは、[dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/IrisFlowerClustering) GitHub リポジトリで確認できます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> - 問題を把握する
> - 適切な機械学習タスクを選択する
> - データを準備する
> - データを読み込んで変換する
> - 学習アルゴリズムを選択する
> - モデルをトレーニングする
> - モデルを使用して予測を行う

学習を続けてその他のサンプルを確認するには、GitHub リポジトリをご覧ください。
> [!div class="nextstepaction"]
> [dotnet/machinelearning GitHub リポジトリ](https://github.com/dotnet/machinelearning/)
