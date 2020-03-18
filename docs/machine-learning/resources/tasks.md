---
title: 機械学習のタスク
description: ML.NET でサポートされる機械学習のさまざまなタスクと、それらに関連するタスクについて説明します。
ms.date: 12/23/2019
ms.openlocfilehash: 6cd41065e668375537b9816ef7a208a65e0a523b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397789"
---
# <a name="machine-learning-tasks-in-mlnet"></a>ML.NET での機械学習のタスク

機械学習タスクとは、問題や質問、および利用可能なデータに基づいて行われる予測または推論の種類です。 たとえば、分類タスクはデータをカテゴリに割り当て、クラスター化タスクは類似性に従ってデータをグループ化します。

機械学習タスクはデータのパターンに依存します。明示的にプログラミングされるのでありません。

この記事では、ML.NET から選択できる機械学習のさまざまなタスクと、一般的なユース ケースをいくつか取り上げ説明します。

どのタスクが自分のシナリオにとって適切かを決定したら、モデルをトレーニングするのに最適なアルゴリズムを選択する必要があります。 使用可能なアルゴリズムは、タスクごとのセクションに一覧表示されています。

## <a name="binary-classification"></a>二項分類

データのインスタンスが 2 つのうちのどちらのクラス (カテゴリ) に属しているかを予測するために使用する[教師あり機械学習](glossary.md#supervised-machine-learning)タスクです。 分類アルゴリズムの入力はラベル付けされた一連のサンプルであり、各ラベルは整数 0 または 1 です。 二項分類アルゴリズムの出力は分類子であり、ラベルのない新しいインスタンスのクラスを予測するために使用できます。 二項分類のシナリオの例を次に示します。

* [Twitter コメントのセンチメントが "肯定的" か "否定的" かを理解する](../tutorials/sentiment-analysis.md)。
* 患者が特定の病気であるかどうかを診断する。
* 電子メールを "スパム" としてマークするかどうかを決定する。
* 写真にイヌや果物などの特定のアイテムが含まれているかどうかを判断します。

詳しくは、Wikipedia の[二項分類](https://en.wikipedia.org/wiki/Binary_classification)の記事を参照してください。

### <a name="binary-classification-trainers"></a>二項分類トレーナー

次のアルゴリズムを使用して二項分類モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>
* <xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>
* <xref:Microsoft.ML.Trainers.PriorTrainer>
* <xref:Microsoft.ML.Trainers.LinearSvmTrainer>

### <a name="binary-classification-inputs-and-outputs"></a>二項分類の入力と出力

二値分類で最適な結果を得るには、バランスの取れた (すなわち、正と負のトレーニング データの数が等しい) トレーニング データを使用します。 欠損値はトレーニングの前に処理しておきます。

入力ラベル列データは <xref:System.Boolean> にする必要があります。
入力特徴列データは、<xref:System.Single> の固定サイズ ベクターにする必要があります。

これらのトレーナーから、以下の列が出力されます。

| [出力列の名前] | 列の型 | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> | モデルによって計算された生のスコア|
| `PredictedLabel` | <xref:System.Boolean> | スコアの符号に基づく予測ラベル。 負のスコアは `false` にマップされ、正のスコアは `true` にマップされます。|

## <a name="multiclass-classification"></a>多クラス分類

データのインスタンスのクラス (カテゴリ) を予測するために使用する[教師あり機械学習](glossary.md#supervised-machine-learning)タスクです。 分類アルゴリズムの入力は、ラベル付けされた一連のサンプルです。 各ラベルは、通常、テキストとして開始されます。 その後、TermTransform を経由してキー (数値) 型に変換されます。 分類アルゴリズムの出力は分類子であり、ラベルのない新しいインスタンスのクラスを予測するために使用できます。 多クラス分類のシナリオの例を次に示します。

* "シベリアン ハスキー"、"ゴールデン レトリバー"、"プードル" などの犬種を特定する。
* 映画のレビューが "肯定的"、"中立"、"否定的" のどれかを理解する。
* ホテルのレビューを "立地"、"価格"、"清潔さ" などに分類する。

詳しくは、Wikipedia の[多クラス分類](https://en.wikipedia.org/wiki/Multiclass_classification)の記事を参照してください。

>[!NOTE]
>一対全では、多クラス データセットに対して機能するように[二項分類学習器](#binary-classification)がアップグレードされます。 詳細については、[Wikipedia] (https://en.wikipedia.org/wiki/Multiclass_classification#One-vs.-rest) ) を参照してください。

### <a name="multiclass-classification-trainers"></a>多クラス分類トレーナー

次のトレーニング アルゴリズムを使用して多クラス分類モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.OneVersusAllTrainer>
* <xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>
* <xref:Microsoft.ML.Vision.ImageClassificationTrainer>

### <a name="multiclass-classification-inputs-and-outputs"></a>多クラス分類の入力と出力

入力ラベル列データは[キー](xref:Microsoft.ML.Data.KeyDataViewType)型にする必要があります。
特徴列は、<xref:System.Single> の固定サイズ ベクターにする必要があります。

このトレーナーの出力は以下のとおりです。

| 出力の名前 | [種類] | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> のベクター | すべてのクラスのスコア。 値が大きいほど、関連するクラスに分類される可能性が高くなります。 i 番目の要素が最大値の場合、予測ラベル インデックスは i になります。 i はゼロベースのインデックスです。 |
| `PredictedLabel` | [キー](xref:Microsoft.ML.Data.KeyDataViewType)型 | 予測ラベルのインデックス。 その値が i の場合、実際のラベルはキーと値の入力ラベルの型の i 番目のカテゴリになります。 |

## <a name="regression"></a>回帰

関連する一連の特徴からラベルの値を予測するために使用する[教師あり機械学習](glossary.md#supervised-machine-learning)タスクです。 ラベルには任意の実際の値を使用できます。分類タスクのように有限の値のセットから取得するものではありません。 回帰アルゴリズムでは、ラベルに関連する特徴におけるラベルの依存関係をモデル化して、特徴の値が変化するとラベルがどのように変化するかを判断します。 回帰アルゴリズムの入力は、既知の値のラベルが付いた一連のサンプルです。 回帰アルゴリズムの出力は関数であり、新しい入力特徴のセットのラベル値を予測するために使用できます。 回帰のシナリオの例を次に示します。

* 住宅の属性 (寝室数、立地、規模など) に基づいて住宅価格を予測する。
* 履歴データと現在の市場動向に基づいて将来の株価を予測する。
* 広告予算に基づいて製品の売り上げを予測する。

### <a name="regression-trainers"></a>回帰トレーナー

次のアルゴリズムを使用して回帰モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>
* <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>
* <xref:Microsoft.ML.Trainers.OlsTrainer>
* <xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>

### <a name="regression-inputs-and-outputs"></a>回帰の入力と出力

入力ラベル列データは <xref:System.Single> にする必要があります。

このタスクのトレーナーの出力は以下のとおりです。

| 出力の名前 | [種類] | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> | モデルによって予測された生のスコア |

## <a name="clustering"></a>クラスタリング

データのインスタンスを、同様の特性を持つクラスタにグループ化するために使用する[教師なし機械学習](glossary.md#unsupervised-machine-learning)タスクです。 また、閲覧や単純な観測では論理的に導き出せない可能性のあるデータ セットにおける関係をクラスタリングを使用して識別することもできます。 クラスタリング アルゴリズムの入力と出力は、選択した方法によって異なります。 分布、重心、結合性、または密度に基づくアプローチを利用できます。 現在、ML.NET では、K 平均法によるクラスタリングを使用した重心に基づくアプローチをサポートしています。 クラスタリングのシナリオの例を次に示します。

* ホテル選択の傾向と特性に基づいてホテルの宿泊客のセグメントを理解する。
* 対象を絞った広告キャンペーンの構築に役立つ顧客セグメントと統計データを特定する。
* 製造メトリックに基づいてインベントリを分類する。

### <a name="clustering-trainer"></a>クラスタリング トレーナー

次のアルゴリズムを使用してクラスタリング モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.KMeansTrainer>

### <a name="clustering-inputs-and-outputs"></a>入力と出力のクラスタリング

入力特徴データは <xref:System.Single> である必要があります。 ラベルは必要ありません。

このトレーナーの出力は以下のとおりです。

| 出力の名前 | [種類] | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> のベクター | 与えられたデータ ポイントからすべてのクラスターの重心までの距離 |
| `PredictedLabel` | [キー](xref:Microsoft.ML.Data.KeyDataViewType)型 | モデルによって予測された最も近いクラスターのインデックス。 |

## <a name="anomaly-detection"></a>異常検出

このタスクでは、主成分分析 (PCA) を使用して、異常検出モデルを作成します。 PCA に基づく異常分析は、有効なトランザクションなどの 1 つのクラスからトレーニング データを簡単に取得できるが、対象とする異常の十分なサンプルを取得するのが難しいシナリオでモデルを構築するのに役立ちます。

機械学習における確立された手法である PCA は、データの内部構造を明らかにし、データの分散を説明するために、調査データ分析で頻繁に使用されます。 PCA は、複数の変数が含まれるデータを分析することで機能します。 変数の相関を探し、結果内の差異を最も捕らえている値の合成を決定します。 これらの合成されたフィーチャー値を使用して、主成分と呼ばれる、よりコンパクトなフィーチャー空間が作成されます。

異常検出には、機械学習における多くの重要なタスクが含まれています。

* 不正な可能性があるトランザクションを識別します。
* ネットワークへの侵入が発生していることを示唆するパターンを学習します。
* 患者の異常なクラスターを検出します。
* システムに入力された値をチェックします。

異常とは定義上はまれに発生するイベントであるため、モデル化するために使用する代表的なデータ サンプルを収集するのは困難です。 このカテゴリに含まれるアルゴリズムは、不均衡なデータセットの使用によるモデルの構築とトレーニングという主要な課題に対処するように特別に設計されています。

### <a name="anomaly-detection-trainer"></a>異常検出トレーナー

次のアルゴリズムを使用して異常検出モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>

### <a name="anomaly-detection-inputs-and-outputs"></a>異常検出の入力と出力

入力特徴は、<xref:System.Single> の固定サイズのベクターにする必要があります。

このトレーナーの出力は以下のとおりです。

| 出力の名前 | [種類] | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 異常検出モデルによって計算された、負ではない無制限のスコア |
| `PredictedLabel` | <xref:System.Boolean> | 入力が異常 (PredictedLabel = true) か異常でないか (PredictedLabel = false) を表す true/false 値 |

## <a name="ranking"></a>ランキング

ランキング タスクでは、ラベル付きのサンプルのセットからランカーが構築されます。 この例のセットは、特定の条件を使用してスコア付けできるインスタンス グループで構成されています。 順位付けのラベルは、インスタンスごとに {0、1、2、3, 4} です。  各インスタンスのスコアが不明である新しいインスタンス グループをランク付けするように、ランカーがトレーニングされます。 ML.NET のランキング学習器は、[機械が学習したランキング](https://en.wikipedia.org/wiki/Learning_to_rank)に基づいています。

### <a name="ranking-training-algorithms"></a>ランキング トレーニング アルゴリズム

次のアルゴリズムを使ってランキング モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>

### <a name="ranking-input-and-outputs"></a>入力と出力のランキング

入力ラベル データ型は[キー](xref:Microsoft.ML.Data.KeyDataViewType)型または <xref:System.Single> である必要があります。 ラベルの値によって関連性が決まります。値が高いほど関連性が高くなります。 ラベルが[キー](xref:Microsoft.ML.Data.KeyDataViewType)型の場合、キーのインデックスは関連性の値です。ここで、最小のインデックスは最も低い関連性です。 ラベルが <xref:System.Single> の場合、値が大きいほど関連性が高いことを示します。

特徴データは <xref:System.Single> の固定サイズのベクターにする必要があります。また、入力行グループ列は[キー](xref:Microsoft.ML.Data.KeyDataViewType)型にする必要があります。

このトレーナーの出力は以下のとおりです。

| 出力の名前 | [種類] | [説明]|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 予測を決定するためにモデルによって計算された無制限のスコア |

## <a name="recommendation"></a>推奨

レコメンデーション タスクによって、推奨される製品やサービスの一覧を作成できます。 ML.NET では、カタログ内に製品のレーティングの履歴データがある場合は、レコメンデーション用の[協調フィルタリング](https://en.wikipedia.org/wiki/Matrix_factorization_%28recommender_systems%29) アルゴリズムである[行列因子分解 (MF)](https://en.wikipedia.org/wiki/Collaborative_filtering) が使用されます。 たとえば、ユーザーによる映画の採点の履歴データがあるときに、そのユーザーが次に見たいと思う映画を推薦できます。

### <a name="recommendation-training-algorithms"></a>レコメンデーション トレーニング アルゴリズム

次のアルゴリズムを使ってレコメンデーション モデルをトレーニングすることができます。

* <xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>

## <a name="forecasting"></a>予測

予測タスクでは過去の時系列データを使用し、将来の行動を予測します。 予測に適したシナリオには、気象予測、季節別の売上予測、予防保守管理などがあります。

### <a name="forecasting-trainers"></a>予測トレーナー

次のアルゴリズムを使って予測モデルをトレーニングすることができます。

<xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa*>
