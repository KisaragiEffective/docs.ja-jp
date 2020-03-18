---
title: ML.NET アルゴリズムの選び方
description: 機械学習モデルに適した ML.NET アルゴリズムの選び方について説明します
ms.topic: overview
ms.date: 06/05/2019
ms.openlocfilehash: 0fed33203c02303e37e47f548e08ec131eeb1c77
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75739994"
---
# <a name="how-to-choose-an-mlnet-algorithm"></a>ML.NET アルゴリズムの選び方

[ML.NET タスク](resources/tasks.md) ごとに選択できるトレーニング アルゴリズムは複数あります。 どれを選択するかは、解決しようとしている問題、データの特性、利用できるコンピューティング リソースとストレージ リソースによって変わります。 機械学習モデルのトレーニングは反復処理である点に注意することが重要です。 場合によっては、最適なものを見つけるために複数のアルゴリズムを試す必要があります。

アルゴリズムは**特徴**に対して動作します。 特徴は、入力データから計算された数値です。 それらは機械学習アルゴリズムに最適な入力です。 生の入力データを 1 つ以上の[データ変換](resources/transforms.md)を使用して特徴に変換します。 たとえば、テキスト データは、単語数と単語の組み合わせ数のセットに変換されます。 データ変換を使用して生のデータの種類から特徴が抽出されると、**特徴付けされた**と呼ばれます。 たとえば、特徴付けされたテキスト、特徴付けされた画像データなどです。

## <a name="trainer--algorithm--task"></a>トレーナー = アルゴリズム + タスク

アルゴリズムは、**モデル**を生成するために実行される数学です。 アルゴリズムが異なると、特性が異なるモデルが生成されます。

ML.NET では、同じアルゴリズムを異なるタスクに適用できます。 たとえば、確率的双対座標上昇法は、二項分類、多クラス分類、回帰に使用できます。 違いは、タスクに合わせてアルゴリズムの出力が解釈される方法にあります。

ML.NET には、各アルゴリズムとタスクの組み合わせに対して、トレーニング アルゴリズムを実行し、解釈するコンポーネントが用意されています。 これらのコンポーネントはトレーナーと呼ばれます。 たとえば、<xref:Microsoft.ML.Trainers.SdcaRegressionTrainer> には、**回帰**タスクに適用される **StochasticDualCoordinatedAscent** アルゴリズムが使用されます。

## <a name="linear-algorithms"></a>線形アルゴリズム

線形アルゴリズムでは、入力データと一連の**重み**の線形結合から**スコア**を計算するモデルが生成されます。 重みは、トレーニング中に推定されるモデルのパラメーターです。

線形アルゴリズムは、[線形分離可能](https://en.wikipedia.org/wiki/Linear_separability)である特徴の場合に適しています。

線形アルゴリズムでトレーニングする前に、特徴を正規化しておくことをお勧めします。 こうすることで、ある特徴が他の特徴よりも結果に大きな影響を及ぼすことを防ぎます。

一般に、線形アルゴリズムはスケーラブルで高速であり、トレーニング コストが低コストで、予測も低コストです。 これらは、特徴の数と、おおよそのトレーニング データ セットのサイズによってスケールします。

線形アルゴリズムは、トレーニング データに対して複数回実行されます。 データセットがメモリに収まる場合は、トレーナーを追加する前に ML.NET パイプラインに[キャッシュ チェックポイント](xref:Microsoft.ML.LearningPipelineExtensions.AppendCacheCheckpoint*)を追加すると、トレーニングが高速になります。

**線形トレーナー**

|アルゴリズム|Properties|トレーナー|
|---------|----------|--------|
|Averaged perceptron (平均化パーセプトロン)|テキスト分類に最適です|<xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>|
|確率的双対座標上昇法|既定のパフォーマンスが適切な場合は、調整が不要です|<xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer> <xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer> <xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer> <xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer> <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>|
|L-BFGS|特徴数が多い場合に使用します。 ロジスティック回帰トレーニング統計が生成されますが、AveragedPerceptronTrainer ほどスケールしません|<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer> <xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer> <xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>|
|Symbolic stochastic gradient descent (シンボリック確率的勾配降下法)|最も高速で最も正確な線形二項分類トレーナー。 プロセッサの数に合わせてスケールします|<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>|

## <a name="decision-tree-algorithms"></a>デシジョン ツリー アルゴリズム

デシジョン ツリー アルゴリズムは、一連の決定を含むモデルを作成します。事実上、データ値を通るフロー チャートです。

この種類のアルゴリズムを使用するために、特徴が線形に分離可能である必要はありません。 また、特徴ベクター内の個々の値は決定プロセスで独立して使用されるため、特徴を正規化する必要はありません。

一般的に、デシジョン ツリー アルゴリズムは非常に正確です。

Generalized Additive Model (一般化加法モデル、GAM) を除き、特徴の数が多いと、説明可能性のないツリー モデルになる可能性があります。

デシジョン ツリー アルゴリズムに消費されるリソースは多く、線形アルゴリズムほどスケールしません。 メモリに収まるようなデータセットに対しては適切に動作します。

Boosted decision trees (ブースト デシジョン ツリー) は小さなツリーの集合であり、各ツリーで入力データにスコアが付けられ、そのスコアは次のツリーに渡され、より良いスコアが生成されるという処理が繰り返されます。集合内の各ツリーは、前のツリーに基づいて改善されます。

**デシジョン ツリー トレーナー**

|アルゴリズム|Properties|トレーナー|
|---------|----------|--------|
|Light gradient boosted machine (軽勾配ブースト マシン)|最も高速で最も正確な二項分類ツリー トレーナー。 高度に調整可能|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer> <xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer> <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer> <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>|
|Fast tree (高速ツリー)|特徴付けされた画像データに使用します。 不均衡なデータに対応できます。 高度に調整可能 | <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer> <xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer> <xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer> <xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>|
|Fast forest (高速フォレスト)|ノイズの多いデータに適しています|<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer> <xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>|
|Generalized Additive Model (一般化加法モデル、GAM)|ツリー アルゴリズムに適した問題で、説明可能性が優先される場合に最適です|<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer> <xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>|

## <a name="matrix-factorization"></a>行列因子分解

|Properties|トレーナー|
|----------|--------|
|データセットが大規模で、カテゴリ別のスパース データに最適です|<xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>|

## <a name="meta-algorithms"></a>メタ アルゴリズム

これらのトレーナーでは、二項トレーナーから多クラストレーナーを作成します。 <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>、<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer> と共に使用します。

|アルゴリズム|Properties|トレーナー|
|---------|----------|--------|
|One versus all (1 対すべて)|この多クラス分類子では、クラスごとに 1 つの二項分類子がトレーニングされます。これにより、そのクラスは他のすべてのクラスと区別されます。 分類するクラスの数によってスケールが制限されます|[OneVersusAllTrainer\<BinaryClassificationTrainer>](xref:Microsoft.ML.Trainers.OneVersusAllTrainer) |
|Pairwise coupling (ペアワイズ結合)|この多クラス分類子では、クラスの各ペアに対して二項分類アルゴリズムがトレーニングされます。 2 つのクラスの各組み合わせをトレーニングする必要があるので、クラスの数によってスケールが制限されます。|[PairwiseCouplingTrainer\<BinaryClassificationTrainer>](xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer)|

## <a name="k-means"></a>K-Means

|Properties|トレーナー|
|----------|--------|
|クラスタリングに使用します|<xref:Microsoft.ML.Trainers.KMeansTrainer>|

## <a name="principal-component-analysis"></a>主成分分析

|Properties|トレーナー|
|----------|--------|
|異常検出に使用されます|<xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>|

## <a name="naive-bayes"></a>Naive Bayes

|Properties|トレーナー|
|----------|--------|
|特徴が独立しており、トレーニング データセットが小さい場合は、この多クラス分類トレーナーを使用します。|<xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>|

## <a name="prior-trainer"></a>以前のトレーナー

|Properties|トレーナー|
|----------|--------|
|他のトレーナーのパフォーマンスを基準にするには、この二項分類トレーナーを使用します。 他のトレーナーのメトリックは以前のトレーナーよりも効果の点で優れています。 |<xref:Microsoft.ML.Trainers.PriorTrainer>|
