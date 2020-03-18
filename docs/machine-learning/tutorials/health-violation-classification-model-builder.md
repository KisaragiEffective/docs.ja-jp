---
title: 'チュートリアル: モデル ビルダーを使用した保健衛生違反の分類'
description: このチュートリアルでは、サンフランシスコでのレストランの保健衛生違反の重大度を分類するために、ML.NET モデル ビルダーを使用して多クラス分類モデルを構築する方法について説明します。
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.topic: tutorial
ms.custom: mvc,mlnet-tooling
ms.openlocfilehash: e94313277a025d482105a6d78b6608d4df442c43
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79185828"
---
# <a name="tutorial-classify-the-severity-of-restaurant-health-violations-with-model-builder"></a>チュートリアル: モデル ビルダーを使用してレストランの保健衛生の重大度を分類する

保健衛生検査中に検出されたレストラン違反のリスク レベルを分類するために、モデル ビルダーを使用して多クラス分類モデルを構築する方法について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - データを準備して理解する
> - シナリオを選択する
> - データベースからのデータの読み込み
> - モデルをトレーニングする
> - モデルを評価する
> - モデルを使用して予測を行う

> [!NOTE]
> モデル ビルダーは現在のところ、プレビュー段階です。

## <a name="prerequisites"></a>必須コンポーネント

前提条件の一覧とインストール手順は、[モデル ビルダーのインストール ガイド](../how-to-guides/install-model-builder.md)を参照してください。

## <a name="model-builder-multiclass-classification-overview"></a>モデル ビルダー多クラス分類の概要

このサンプルでは、モデル ビルダーでビルドされた機械学習モデルを使用して、保健衛生違反のリスクを分類する C# .NET Core コンソール アプリケーションを作成します。 このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub リポジトリにあります。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

1. "RestaurantViolations" という名前の **C# .NET Core コンソール アプリケーション** を作成します。 **[ソリューションとプロジェクトを同じディレクトリに配置する]** を**オフ** (VS 2019) にします。または、 **[ソリューションのディレクトリの作成]** を**オン**にします (VS 2017)。

## <a name="prepare-and-understand-the-data"></a>データを準備して理解する

> 機械学習モデルのトレーニングと評価に使用するデータ セットは、[サンフランシスコ公衆衛生局の安全性スコア](https://www.sfdph.org/dph/EH/Food/score/default.asp) から取得したものです。 便宜上、データ セットは、モデルのトレーニングと予測の作成に関連する列のみを含むように圧縮されています。 [データセット](https://data.sfgov.org/Health-and-Social-Services/Restaurant-Scores-LIVES-Standard/pyih-qa8i?row_index=0)の詳細については、次の Web サイトを参照してください。

[Restaurant Safety Scores データセット](https://github.com/luisquintanilla/machinelearning-samples/raw/AB1608219/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantScores.zip)をダウンロードして解凍します。

データセットの各行には、保健局の検査中に観察された違反に関する情報と、それらの違反が公衆衛生と安全性に与える脅威のリスク評価が含まれています。

| InspectionType | ViolationDescription | RiskCategory |
| --- | --- | --- |
| 定期 - スケジュールなし | 食品接触面の洗浄または衛生状態が不適切 | リスク - 中 |
| 新しい所有権 | リスクの高い害虫の侵入 | リスク - 高 |
| 定期 - スケジュールなし | 清潔でない、適切に保管されていない、または消毒が十分でない布巾 | リスク - 低 |

- **InspectionType**: 検査の種類。 これは、新しい施設の最初の検査、定期検査、苦情検査など、さまざまな種類の検査とすることができます。
- **ViolationDescription**: 検査中に検出された違反の説明。
- **RiskCategory**: 違反が公衆衛生と安全性に及ぼすリスクの重大度。

`label` は予測する列です。 分類タスクを実行する場合の目標は、カテゴリ (テキストまたは数値) を割り当てることです。 この分類シナリオで、違反の重大度として割り当てられる値は、リスク - 低、リスク - 中、またはリスク - 高です。 したがって、**RiskCategory** はラベルです。 `features` は、`label` を予測するためのモデルを指定する入力です。 この場合は、**RiskCategory** を予測するための機能または入力として **InspectionType** および **ViolationDescription** を使用します。

## <a name="choose-a-scenario"></a>シナリオを選択する

![Visual Studio のモデル ビルダー ウィザード](./media/sentiment-analysis-model-builder/model-builder-screen.png)

モデルをトレーニングするには、モデル ビルダーによって提供される機械学習シナリオの一覧から選択します。 この場合、シナリオは "*問題の分類*" です。

1. **ソリューション エクスプローラー**で、 *[RestaurantViolations]* プロジェクトを右クリックし、 **[追加]**  >  **[機械学習]** の順に選択します。
1. このサンプルでは、シナリオは多クラス分類です。 モデル ビルダーの "*シナリオ*" の手順で、 **[問題の分類]** シナリオを選択します。

## <a name="load-the-data"></a>データを読み込む

モデル ビルダーは、SQL Server データベースから、あるいは `csv` 形式または `tsv` 形式のローカル ファイルからデータを受け取ります。

1. モデル ビルダー ツールのデータの手順で、データ ソースのドロップダウンから **[SQL Server]** を選択します。
1. **[SQL Server データベースに接続]** テキスト ボックスの横にあるボタンを選択します。
    1. **[データの選択]** ダイアログで、 **[Microsoft SQL Server データベース ファイル]** を選択します。
    1. **[常にこれを選択する]** チェックボックスをオフにして、 **[続行]** を選択します。
    1. **[接続プロパティ]** ダイアログで、 **[参照]** を選択し、ダウンロードした *RestaurantScores.mdf* ファイルを選択します。
    1. **[OK]** を選択します。
1. **[テーブル名]** ドロップダウンから **[違反]** を選択します。
1. **[予測する列 (ラベル)]** ドロップダウンで **[RiskCategory]** を選択します。
1. **[入力列 (機能)]** ドロップダウンで選択されている既定の列の選択 ( **[InspectionType]** および **[ViolationDescription]** ) をそのまま使用します。
1. **[トレーニング]** リンクを選択して、モデル ビルダーの次の手順に進みます。

## <a name="train-the-model"></a>モデルをトレーニングする

このチュートリアルで、問題の分類モデルをトレーニングするのに使用する機械学習のタスクは、多クラス分類です。 モデルのトレーニング プロセス中、モデル ビルダーは、データセットに対して最もパフォーマンスの優れたモデルを見つけるために、さまざまな多クラス分類アルゴリズムと設定を使用して個々のモデルをトレーニングします。

モデルのトレーニングに必要な時間は、データの量に比例します。 モデル ビルダーにより、 **[Time to train (seconds)]\(トレーニング時間 (秒)\)** の既定値が、データ ソースのサイズに基づいて自動的に選択されます。

1. モデル ビルダーによって **[トレーニング時間 (秒)]** の値が 10 秒に設定しますが、30 秒に増加します。 トレーニング時間が長いほど、モデル ビルダーは最良のモデルの検索の中で、より多くのアルゴリズムやパラメーターの組み合わせを調べることができます。
1. **[Start Training]\(トレーニング開始\)** を選択します。

    トレーニング プロセスを通して、進捗データがトレーニングの手順の `Progress` セクションに表示されます。

    - **[状態]** には、トレーニング プロセスの完了ステータスが表示されます。
    - **[Best accuracy]\(最良の精度\)**   には、これまでにモデル ビルダーで見つかった最良のパフォーマンスのモデルの精度が表示されます。 精度が高くなるほど、テスト データでモデルが正確に予測されたことになります。
    - **[Best algorithm]\(最良のアルゴリズム\)**   には、これまでにモデル ビルダーで見つかった最良のパフォーマンスのアルゴリズムの名前が表示されます。
    - **[Last algorithm]\(最後のアルゴリズム\)**   には、モデル ビルダーがモデルのトレーニングに最近使用したアルゴリズムの名前が表示されます。

1. トレーニングが完了したら、 **[評価]** リンクを選択して、次の手順に進みます。

## <a name="evaluate-the-model"></a>モデルを評価する

トレーニングの手順の結果は、最良のパフォーマンスを示した 1 つのモデルになります。 モデル ビルダーの評価の手順の出力セクションには、 **[Best Model]\(最良のモデル\)** エントリの最良のパフォーマンスのモデルで使用されたアルゴリズムと、 **[Best Model Accuracy]\(最良のモデル精度\)** のメトリックが含まれます。 さらに、探索された最大 5 つのモデルとそのメトリックが含まれている概要テーブルが表示されます。

精度のメトリックに不満がある場合、モデルのトレーニング時間を増やすか、さらに多くのデータを使用すると、モデルの精度を簡単に高めることができます。 それ以外の場合、 **[コード]** リンクを選択して、モデル ビルダーの最後の手順に進みます。

## <a name="add-the-code-to-make-predictions"></a>予測を行うコードを追加する

トレーニング プロセスの結果として 2 つのプロジェクトが作成されます。

- RestaurantViolationsML.ConsoleApp: モデルのトレーニングとサンプルの使用に関するコードが含まれる C# .NET Core コンソール アプリケーション。
- RestaurantViolationsML.Model: 入力および出力モデル データのスキーマを定義するデータ モデル、トレーニング時にパフォーマンスが最高のモデルの保存バージョン、および予測を行う `ConsumeModel` というヘルパー クラスを含む .NET Standard クラス ライブラリ。

1. モデル ビルダーのコードの手順で、 **[プロジェクトの追加]** を選択して、自動生成されたプロジェクトをソリューションに追加します。
1. *RestaurantViolations* プロジェクトで *Program.cs* ファイルを開きます。
1. *RestaurantViolationsML.Model* プロジェクトを参照する次の using ステートメントを追加します。

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L2)]

1. 新しいデータに対してモデルを使用して予測を行うには、アプリケーションの `ModelInput` メソッド内に `Main` クラスの新しいインスタンスを作成します。 リスク カテゴリが入力に含まれていないことに注意してください。 これは、モデルによってその予測が生成されるためです。

    [!code-csharp [TestData](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L11-L15)]

1. `ConsumeModel` クラスの `Predict` メソッドを使用します。 `Predict` メソッドでは、トレーニング済みモデルを読み込み、モデルに対して [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) を作成し、新しいデータに対してそれを使用して予測を行います。

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L17-L24)]

1. アプリケーションを実行します。

    プログラムによって生成される出力は次のスニペットのようになります。

    ```bash
    Inspection Type: Complaint
    Violation Description: Inadequate sewage or wastewater disposal
    Risk Category: Moderate Risk
    ```

別のソリューション内で後で生成されたプロジェクトを参照する必要がある場合は、`C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` ディレクトリを検索することができます。

おめでとうございます! これで、モデル ビルダーを使用して保健衛生違反のリスクを分類するための機械学習モデルを正常に作成できました。 このチュートリアルのソース コードは、[dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub リポジトリにあります。

## <a name="additional-resources"></a>その他の技術情報

このチュートリアルで説明しているトピックについて詳しくは、次のリソースを参照してください。

- [モデル ビルダーのシナリオ](../automate-training-with-model-builder.md#scenarios)
- [多クラス分類](../resources/glossary.md#multiclass-classification)
- [多クラス分類モデル メトリック](../resources/metrics.md#evaluation-metrics-for-multi-class-classification)
