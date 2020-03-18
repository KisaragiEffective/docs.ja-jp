---
title: ML.NET CLI を使用してセンチメントを分析する
description: サンプル データセットから ML モデルと関連する C# コードを自動的に生成する
author: cesardl
ms.author: cesardl
ms.date: 12/23/2019
ms.custom: mvc
ms.topic: tutorial,mlnet-tooling
ms.openlocfilehash: d817e173239d2848fb16e94cca8ead563bc900a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79187620"
---
# <a name="analyze-sentiment-using-the-mlnet-cli"></a>ML.NET CLI を使用してセンチメントを分析する

ML.NET CLI を使用して ML.NET モデルと基礎となる C# コードを自動生成する方法について説明します。 データセットと実装する機械学習タスクを指定すると、CLI では、AutoML エンジンを使用してモデルの生成と展開のソース コードと、二項モデルが作成されます。

このチュートリアルでは、次の手順を実行します。
> [!div class="checklist"]
>
> - 選択した機械学習タスク用にデータを準備する
> - CLI から "mlnet auto-train" コマンドを実行する
> - 品質メトリックの結果を確認する
> - アプリケーションでモデルを使用するために生成された C# コードを理解する
> - モデルのトレーニングに使用された生成済み C# コードを調べる

> [!NOTE]
> このトピックは、現在プレビュー段階の ML.NET CLI ツールについて述べており、内容が変更される場合があります。 詳細については、[ML.NET](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet) に関するページを参照してください。

ML.NET CLI は ML.NET の一部であり、その主な目的は、ML.NET を学習する .NET 開発者のために ML.NET を "民主化" することなので、始めるときにゼロからコードを書く必要はありません。

ML.NET CLI は任意のコマンドプロンプト (Windows、Mac、または Linux) で実行して、指定するトレーニング データセットに基づいて高品質の ML.NET モデルとソース コードを生成できます。

## <a name="pre-requisites"></a>前提条件

- [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) 以降
- (省略可能) [Visual Studio 2017 または 2019](https://visualstudio.microsoft.com/vs/)
- [ML.NET CLI](../how-to-guides/install-ml-net-cli.md)

生成された C# コード プロジェクトは、Visual Studio から、または `dotnet run` (.NET Core CLI) を使用して実行できます。

## <a name="prepare-your-data"></a>データを準備する

ここでは、二項分類機械学習タスクである "感情分析" シナリオに使用されている既存のデータセットを使用します。 お持ちのデータセットを同様の方法で使用して、モデルとコードを自動的に生成することができます。

1. [UCI Sentiment Labeled Sentences データセット zip ファイル (次の注の引用を参照してください)](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) をダウンロードし、任意のフォルダーに展開します。

    > [!NOTE]
    > このチュートリアルで使用されるデータセットでは、「From Group to Individual Labels using Deep Features」 (Kotzias 他 KDD 2015) のものであり、UCI 機械学習リポジトリ (Dua, D. and Karra Taniskidou, E.(2017)) でホストされています。 UCI 機械学習リポジトリ [http://archive.ics.uci.edu/ml ]。 カリフォルニア州アーバイン: カリフォルニア大学情報コンピュータサイエンス学部。

2. `yelp_labelled.txt` ファイルを以前に作成した任意のフォルダー (`/cli-test` など) にコピーします。

3. 任意のコマンド プロンプトを開き、データセット ファイルをコピーしたフォルダーに移動します。 次に例を示します。

    ```console
    cd /cli-test
    ```

    Visual Studio Code などの任意のテキスト エディターを使用して、`yelp_labelled.txt` データセット ファイルを開き、調べることができます。 次のような構造であることがわかります。

    - このファイルにヘッダーはありません。 列のインデックスを使用します。

    - 列は 2 つしかありません。

        | テキスト (列インデックス 0) | ラベル (列インデックス 1)|
        |--------------------------|-------|
        | Wow...Loved this place. | 1 |
        | Crust is not good. | 0 |
        | Not tasty and the texture was just nasty. | 0 |
        | ...その他のテキスト行... | ...(1 または 0)... |

    データセット ファイルはエディターから必ず閉じてください。

    これで、この "感情分析" シナリオに CLI を使用する準備が整いました。

    > [!NOTE]
    > このチュートリアルを完了した後は、現在 ML.NET CLI プレビューでサポートされているいずれかの ML タスク ( *"二項分類"、"多クラス分類"、"回帰"* ) に使用する準備ができていれば、お持ちのデータセットで試すこともできます。

## <a name="run-the-mlnet-auto-train-command"></a>"mlnet auto-train" コマンドを実行する

1. 次の ML.NET CLI コマンドを実行します。

    ```console
    mlnet auto-train --task binary-classification --dataset "yelp_labelled.txt" --label-column-index 1 --has-header false --max-exploration-time 10
    ```

    このコマンドによって **`mlnet auto-train` コマンド** が次のように実行されます。
    - 種類が **`binary-classification`** の **ML タスク**に対して
    - **データセット ファイル `yelp_labelled.txt`** をトレーニングおよびテスト データセットとして使用します (内部的には、CLI によってクロス検証が使用されるか、トレーニング用に 1 つとテスト用に 1 つという 2 つのデータセットに分割されます)。
    - この予測する**目的列/ターゲット列** (一般的に **"ラベル"** と呼ばれます) は、**インデックス 1 の列**です (インデックスは 0 ベースなので、これは 2 列目です)。
    - この特定のデータセット ファイルにはヘッダーがないため、列名を含む**ファイル ヘッダーは使用しません**。
    - 実験の**目標探索時間**は **10 秒**です

    CLI からの出力が次のように表示されます。

    <!-- markdownlint-disable MD023 MD025 -->

    # <a name="windows"></a>[Windows](#tab/windows)

    ![PowerShell 上の ML.NET CLI の auto-train](./media/mlnet-cli/mlnet-auto-train-binary-classification-powershell.gif)

    # <a name="macos-bash"></a>[macOS Bash](#tab/macosbash)

    ![PowerShell 上の ML.NET CLI の auto-train](./media/mlnet-cli/mlnet-auto-train-binary-classification-bash.gif)

    この特定のケースでは、CLI ツールでは、わずか 10 秒の間に指定した小さなデータセットに対してかなりの回数の反復処理を実行できました。つまり、内部データ変換とアルゴリズムのハイパーパラメーターが異なるさまざまなアルゴリズム/構成の組み合わせに基づいて、複数回のトレーニングを行いました。

    最後に、10 秒間で検出された "最高品質" のモデルは、何らかの特定の構成を持つ特定のトレーナー/アルゴリズムを使用するモデルです。 探索時間によっては、コマンドから異なる結果が生成される可能性があります。 選択は、`Accuracy` など、表示されている複数のメトリックに基づいています。

    **モデルの品質メトリックを理解する**

    二項分類モデルを評価する最初で最も簡単なメトリックは正確度です。これは簡単に理解できます。 "正確度は、テスト データ セットでの正しい予測の割合です"。 100% (1.00) に近いほど優れています。

    ただし、正確度メトリックを使用して測定するだけでは不十分な場合があります。特に、テスト データセットでラベル (この場合は 0 と 1) のバランスが取れていない場合です。

    追加のメトリックや、さまざまなモデルの評価に使用される**メトリックに関するより詳細な情報** (正確度、AUC、AUCPR、F1 スコアなど) については、[ML.NET のメトリックの概要](../resources/metrics.md)に関する記事をご覧ください。

    > [!NOTE]
    > これと同じデータセットを試し、`--max-exploration-time` に数分間 (たとえば 180 秒を指定するので 3 分間) を指定すると、このデータセット (かなり小規模で 1,000 行) に対して、トレーニング パイプライン構成が異なる優れた "最適なモデル" が自動的に検出されます。

    大規模なデータセットを対象とした "運用環境対応モデル" である "最高/高品質" なモデルを見つけるには、通常、データセットのサイズに応じて、はるかに長い探索時間を指定して、CLI を使用した実験を行う必要があります。 実際のところ、多くの場合、特に行と列のデータセットが大きいと、数時間の探索時間が必要になることがあります。

1. 前のコマンド実行により、以下の資産が生成されました。

    - すぐに使用できるシリアル化されたモデル .zip ("最適なモデル")。
    - その生成されたモデルを実行/スコア付けする C# コード (そのモデルを使用してエンドユーザー アプリで予測を行うため)。
    - そのモデルを生成に使用される C# トレーニング コード (学習目的)。
    - ハイパーパラメーターとデータ変換の組み合わせを使用して各アルゴリズムに関する具体的な詳細情報を試し、すべての反復処理が探索されたログ ファイル。

    最初の 2 つの資産 (.ZIP ファイル モデルと、そのモデルを実行する C# コード) は、その生成された ML モデルを使用して予測を行うために、エンドユーザー アプリ (ASP.NET Core Web アプリ、サービス、デスクトップ アプリなど) で直接使用できます。

    3 つ目の資産のトレーニング コードは、生成されたモデルをトレーニングするために CLI によって使用された ML.NET API コードを示しています。そのため、CLI によって選択された特定のトレーナー/アルゴリズムとハイパーパラメーターを調べることができます。

これらの列挙した資産については、チュートリアルの次の手順で説明します。

## <a name="explore-the-generated-c-code-to-use-for-running-the-model-to-make-predictions"></a>生成された C# コードを調べて予測するモデルの実行に使用する

1. Visual Studio (2017 または 2019) で、元の保存先フォルダー内の `SampleBinaryClassification` という名前のフォルダーに生成されたソリューションを開きます (このチュートリアルでは `/cli-test` という名前でした)。 次のようなソリューションが表示されます。

    > [!NOTE]
    > このチュートリアルでは Visual Studio を使用することをお勧めしますが、生成された C# コード (2 つのプロジェクト) を任意のテキスト エディターで調べ、生成されたコンソール アプリを macOS、Linux または Windows マシン上で `dotnet CLI` を使用して実行することもできます。

    ![CLI によって生成された VS ソリューション](./media/mlnet-cli/generated-csharp-solution-detailed.png)

    - シリアル化された ML モデル (.zip ファイル) とデータ クラス (データ モデル) を含む、生成された**クラス ライブラリ**は、エンドユーザー アプリケーションで直接使用できます。さらに、そのクラス ライブラリを直接参照する (または必要に応じてコードを移動する) こともできます。
    - 生成された**コンソール アプリ**には、確認する必要がある実行コードが含まれているので、通常、単純なコード (わずか数行) を予測を行うエンドユーザー アプリケーションに移動することで、"スコア付けコード" (予測する ML モデルを実行するコード) を再利用します。

1. クラス ライブラリ プロジェクト内の **ModelInput.cs** および **ModelOutput.cs** クラス ファイルを開きます。 これらのクラスは、データの保持に使用される "データ クラス" つまり POCO クラスであることがわかります。 これは "定型コード" ですが、データセットに数十列から数百列がある場合に生成すると便利です。
    - `ModelInput` クラスはデータセットからデータを読み取るときに使用されます。
    - `ModelOutput` クラスは、予測結果 (予測データ) を取得するために使用します。

1. Program.cs ファイルを開き、コードを調べます。 わずか数行で、モデルを実行し、サンプル予測を行うことができます。

    ```csharp
    static void Main(string[] args)
    {
        MLContext mlContext = new MLContext();

        // Training code used by ML.NET CLI and AutoML to generate the model
        //ModelBuilder.CreateModel();

        ITransformer mlModel = mlContext.Model.Load(MODEL_FILEPATH, out DataViewSchema inputSchema);
        var predEngine = mlContext.Model.CreatePredictionEngine<ModelInput, ModelOutput>(mlModel);

        // Create sample data to do a single prediction with it
        ModelInput sampleData = CreateSingleDataSample(mlContext, DATA_FILEPATH);

        // Try a single prediction
        ModelOutput predictionResult = predEngine.Predict(sampleData);

        Console.WriteLine($"Single Prediction --> Actual value: {sampleData.Label} | Predicted value: {predictionResult.Prediction}");
    }
    ```

    - コードの最初の行では、ML.NET コードを実行するたびに、必要な `MLContext` オブジェクトを作成するだけです。

    - 2 行目のコードはコメント化されています。なぜなら、モデルは既に CLI ツールによってトレーニングされ、モデルのシリアル化された .ZIP ファイルに保存されているので、モデルをトレーニングする必要がないからです。 ただし、CLI による *"モデルのトレーニング方法"* を確認するには、その行をコメント解除し、その特定の ML モデルに使用されるトレーニング コードを実行/デバッグすることができます。

    - 3 行目のコードでは、シリアル化されたモデルの .ZIP ファイルからそのモデルを読むために、そのモデルの .ZIP ファイルのパスを指定して `mlContext.Model.Load()` API を使用します。

    - 4 行目のコードでは、`mlContext.Model.CreatePredictionEngine<TSrc,TDst>(ITransformer mlModel)` API を使用して `PredictionEngine` オブジェクトを作成します。 1 つのデータ サンプル (この場合は、センチメントを予測するための 1 つのテキスト) をターゲットにして予測する場合は、常に `PredictionEngine` オブジェクトが必要です。

    - コードの 5 行目では、関数 `CreateSingleDataSample()` を呼び出して、予測に使用するその *1 つのサンプル データ*を作成します。 CLI ツールでは、使用するサンプル データの種類が認識されないので、この関数内ではデータセットの最初の行が読み込まれます。 ただし、この場合は、その関数を実装してこの簡単なコードを更新することで、現在の `CreateSingleDataSample()` 関数の実装ではなく独自の "ハードコーディングされた" データを作成することもできます。

        ```csharp
        private static ModelInput CreateSingleDataSample()
        {
            ModelInput sampleForPrediction = new ModelInput() { Col0 = "The ML.NET CLI is great for getting started. Very cool!", Label = true };
            return sampleForPrediction;
        }
        ```

1. プロジェクトを実行するには、データセットの最初の行から読み込まれた元のサンプル データを使用するか、ハードコーディングされた独自のカスタム サンプル データを指定します。 次のような予測が得られます。

    # <a name="windows"></a>[Windows](#tab/windows)

    Visual Studio から F5 キー ([再生] ボタン) を押してコンソール アプリを実行します。

    ![PowerShell 上の ML.NET CLI の auto-train](./media/mlnet-cli/sample-cli-prediction-execution.png))

    # <a name="macos-bash"></a>[macOS Bash](#tab/macosbash)

    次のコマンドを入力して、コマンドプロンプトからコンソール アプリを実行します。

    ```dotnetcli
    cd SampleBinaryClassification
    cd SampleBinaryClassification.ConsoleApp

    dotnet run
    ```

    ![PowerShell 上の ML.NET CLI の auto-train](./media/mlnet-cli/sample-cli-prediction-execution-bash.png))

    ---

1. ハードコーディングされたサンプル データをセンチメントが異なる他の文に変更して、モデルがどのように肯定的センチメントまたは否定的センチメントを予測するかを確認してみてください。

## <a name="infuse-your-end-user-applications-with-ml-model-predictions"></a>エンドユーザー アプリケーションに ML モデルの予測を組み込む

同様の "ML モデル スコア付けコード" を使用して、エンドユーザー アプリケーションでモデルを実行し、予測することができます。

たとえば、そのコードを **WPF** や **WinForms** などの任意の Windows デスクトップ アプリケーションに直接移動して、コンソール アプリで実行したときと同じ方法でモデルを実行できます。

ただし、ML モデルを実行するこれらのコード行を実装する方法は、最適化して (つまり、モデルの .zip ファイルをキャッシュし、読み込みが 1 回になるようにして)、要求ごとに作成するのではなくシングルトン オブジェクトを持つようにします。次のセクションで説明するように、Web アプリケーションや分散サービスなど、アプリケーションをスケーラブルにする必要がある場合は特にそうです。

### <a name="running-mlnet-models-in-scalable-aspnet-core-web-apps-and-services-multi-threaded-apps"></a>スケーラブルな ASP.NET Core Web アプリケーションおよびサービス (マルチスレッド アプリ) で ML.NET モデルを実行する

モデル オブジェクト (モデルの .zip ファイルから読み込まれた `ITransformer`) と `PredictionEngine` オブジェクトの作成は、最適化するようにします。特に、スケーラブルな Web アプリと分散サービスに対して実行する場合は特にそうです。 最初のケースでは、モデル オブジェクト (`ITransformer`) の最適化は簡単です。 `ITransformer` オブジェクトはスレッドセーフなので、オブジェクトをシングルトン オブジェクトまたは静的オブジェクトとしてキャッシュして、モデルの読み込みを 1 回にすることができます。

2 つ目のオブジェクトの `PredictionEngine` オブジェクトはそれほど簡単ではありません。`PredictionEngine` オブジェクトはスレッドセーフではないので、ASP.NET Core アプリでこのオブジェクトをシングルトン オブジェクトまたは静的オブジェクトとしてインスタンス化することができないからです。 このスレッド セーフとスケーラビリティの問題については、この[ブログ記事](https://devblogs.microsoft.com/cesardelatorre/how-to-optimize-and-run-ml-net-models-on-scalable-asp-net-core-webapis-or-web-apps/)で詳しく説明されています。

ただし、状況はこのブログ記事で説明されているよりもはるかに簡単になりました。 Microsoft はより簡単なアプローチに取り組み、優れた **".NET Core 統合パッケージ"** を作成しました。アプリケーションの DI サービス (Dependency Injection サービス) に登録することで ASP.NET Core アプリおよびサービスで簡単に使用できます。また、その後はコードから直接使用できます。 その実行方法については、次のチュートリアルと例を参照してください。

- [チュートリアル: スケーラブルな ASP.NET Core Web アプリと WebAPI 上の ML.NET モデルの実行](https://aka.ms/mlnet-tutorial-netcoreintegrationpkg)
- [サンプル: ASP.NET Core WebAPI 上のスケーラブルな ML.NET モデル](https://aka.ms/mlnet-sample-netcoreintegrationpkg)

## <a name="explore-the-generated-c-code-that-was-used-to-train-the-best-quality-model"></a>"最高品質" のモデルのトレーニングに使用された生成済み C# コードを調べる

より高度な学習目的のために、生成済みモデルのトレーニングに CLI ツールによって使用された生成済み C# コードを調べることもできます。

その "トレーニング モデル コード" は、現在、`ModelBuilder` という名前で生成されたカスタム クラスに生成されているので、そのトレーニング コードを調べることができます。

さらに重要な点は、この特定のシナリオ (感情分析モデル) の場合、その生成済みトレーニング コードを次のチュートリアルで説明されているコードと比較することもできることです。

- 比較:[チュートリアル: センチメント分析のバイナリ分類のシナリオで ML.NET を使用する](sentiment-analysis.md)

チュートリアルで選択したアルゴリズムとパイプラインの構成を CLI ツールによって生成されたコードと比較するのは興味深いことです。 より良いモデルのために反復処理と検索に使う時間に応じて、選択されるアルゴリズムと、その特定のハイパーパラメーターとパイプラインの構成は変わることがあります。

このチュートリアルでは、次の作業を行う方法を学びました。
> [!div class="checklist"]
>
> - 選択した ML タスク (解決する問題) 用のデータを準備する
> - CLI ツールで "mlnet auto-train" コマンドを実行する
> - 品質メトリックの結果を確認する
> - モデルを実行するために生成された C# コード (エンドユーザー アプリで使用するコード) を理解する
> - "最高品質" のモデルのトレーニングに使用された生成済み C# コードを調べる (学習目的)

## <a name="see-also"></a>関連項目

- [ML.NET CLI を使用してモデルのトレーニングを自動化する](../automate-training-with-cli.md)
- [チュートリアル: スケーラブルな ASP.NET Core Web アプリと WebAPI 上の ML.NET モデルの実行](https://aka.ms/mlnet-tutorial-netcoreintegrationpkg)
- [サンプル: ASP.NET Core WebAPI 上のスケーラブルな ML.NET モデル](https://aka.ms/mlnet-sample-netcoreintegrationpkg)
- [ML.NET CLI auto-train コマンド リファレンス ガイド](../reference/ml-net-cli-reference.md)
- [ML.NET CLI のテレメトリ](../resources/ml-net-cli-telemetry.md)
