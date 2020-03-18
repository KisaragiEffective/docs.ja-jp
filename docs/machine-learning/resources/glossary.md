---
title: 機械学習の用語集
description: ML.NET でカスタム モデルをビルドする際に役立つ機械学習の重要な用語の用語集。
ms.topic: reference
ms.date: 07/31/2019
ms.openlocfilehash: 32ccb6df1cb08db45ebd25a0d1c0ea4396a6c50b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397765"
---
# <a name="machine-learning-glossary-of-important-terms"></a>機械学習の重要な用語の用語集

ML.NET でカスタム モデルをビルドする際に役立つ機械学習の重要な用語を次に示します。

## <a name="accuracy"></a>精度

[分類](#classification)における正確度は、正しく分類された項目の数をテスト セット内の項目の総数で割ったものです。 0 (正確度が最も低い) ～ 1 (正確度が最も高い) の値になります。 正確度は、モデル パフォーマンスの評価メトリックの 1 つです。 [精度](#precision)、[再現率](#recall)、および [F 値](#f-score)と併せて考慮してください。

## <a name="area-under-the-curve-auc"></a>曲線下面積 (AUC)

[二項分類](#binary-classification)における評価メトリックであり、偽陽性率 (x 軸上) に対する真陽性率 (y 軸上) を描画する曲線下面積の値です。 0\.5 (最低) ～ 1 (最高) の値になります。 ROC 曲線 (受信者操作特性曲線) 下面積とも呼ばれます。 詳しくは、Wikipedia の[受信者操作特性](https://en.wikipedia.org/wiki/Receiver_operating_characteristic)の記事を参照してください。

## <a name="binary-classification"></a>二項分類

[ラベル](#classification)が 2 つのクラスのうちの 1 つである[分類](#label)です。 詳細については、トピック「[機械学習のタスク](tasks.md#binary-classification)」のセクションの「[二項分類](tasks.md)」を参照してください。

## <a name="calibration"></a>較正

調整は、二項分類と多クラス分類のために、生のスコアをクラスのメンバーシップにマップするプロセスです。 一部 ML.NET トレーナーには `NonCalibrated` サフィックスがあります。 これらのアルゴリズムからは、後でクラスの確率にマップする必要がある生のスコアが生成されます。

## <a name="catalog"></a>Catalog

ML.NET では、カタログは、共通の目的でグループ化された拡張機能のコレクションです。

たとえば、各機械学習タスク (二項分類、回帰、ランキングなど) には、利用できる機械学習アルゴリズム (トレーナー) のカタログがあります。 二項分類トレーナー用のカタログは <xref:Microsoft.ML.BinaryClassificationCatalog.BinaryClassificationTrainers> です。

## <a name="classification"></a>分類

データを使用してカテゴリを予測する際、[教師あり機械学習](#supervised-machine-learning)タスクが分類と呼ばれます。 [二項分類](#binary-classification)とは、2 つのカテゴリだけを予測する (たとえば、画像を猫または犬の写真として分類する) ことです。 [多クラス分類](#multiclass-classification)とは、複数のカテゴリを予測する (たとえば、画像を特定の犬種の写真として分類する) ことです。

## <a name="coefficient-of-determination"></a>決定係数

[回帰](#regression)における評価メトリックであり、データがモデルにどの程度適合するかを示します。 0 ～ 1 の値になります。 値 0 は、データがランダムであるか、モデルに適合できないことを意味します。 値 1 は、モデルがデータと完全に一致していることを意味します。 多くの場合、これは r<sup>2</sup>、R<sup>2</sup>、または r の 2 乗と呼ばれます。

## <a name="data"></a>Data

データはあらゆる機械学習アプリケーションの中心です。 ML.NET では、データは <xref:Microsoft.ML.IDataView> オブジェクトで表されます。 データ ビュー オブジェクト:

- 列と行で構成されています
- 遅延評価されます (つまり、操作によって要求されたときにのみデータが読み込まれます)
- 各列の型、形式、長さを定義するスキーマが含まれます

## <a name="estimator"></a>エスティメーター

<xref:Microsoft.ML.IEstimator%601> インターフェイスを実装する ML.NET のクラス。

エスティメーターは、変換 (データ準備変換と機械学習モデル トレーニング変換の両方) の仕様です。 エスティメーターを連結して、変換のパイプラインにすることができます。 エスティメーターまたはエスティメーターのパイプラインのパラメーターは、<xref:Microsoft.ML.IEstimator%601.Fit%2A> が呼び出されたときに学習されます。 <xref:Microsoft.ML.IEstimator%601.Fit%2A> の結果は[トランスフォーマー](#transformer)です。

## <a name="extension-method"></a>拡張メソッド

クラスの一部ですが、クラスの外部で定義されている .NET メソッドです。 拡張メソッドの最初のパラメーターは、その拡張メソッドが属するクラスへの静的な `this` 参照です。

拡張メソッドは、ML.NET で[エスティメーター](#estimator)のインスタンスを構築するために幅広く使用されています。

## <a name="feature"></a>機能

測定対象となる事象の測定可能なプロパティです。通常は数 (倍精度) 値になります。 複数の特徴は**特徴ベクトル**と呼ばれ、通常は `double[]` として格納されます。 特徴では、測定対象となる事象の重要な特性を定義します。 詳しくは、Wikipedia の[特徴](https://en.wikipedia.org/wiki/Feature_(machine_learning))の記事を参照してください。

## <a name="feature-engineering"></a>特徴エンジニアリング

特徴エンジニアリングは、一連の[特徴](#feature)の定義、および使用可能な事象データから特徴ベクトルを生成する (特徴抽出) ソフトウェアの開発を含むプロセスです。 詳しくは、Wikipedia の[特徴エンジニアリング](https://en.wikipedia.org/wiki/Feature_engineering)の記事を参照してください。

## <a name="f-score"></a>F 値

[分類](#classification)における評価メトリックであり、[精度](#precision)と[再現率](#recall)の調和平均を取ります。

## <a name="hyperparameter"></a>ハイパーパラメーター

機械学習アルゴリズムのパラメーターです。 例として、デシジョン フォレストにおける学習するツリー数や勾配降下アルゴリズムにおけるステップ サイズなどがあります。 *ハイパーパラメーター*の値は、モデルのトレーニング前に設定され、予測関数のパラメーターを検出するプロセスを管理します。例として、デシジョン ツリーにおける比較ポイントや線形回帰モデルにおける重みなどがあります。 詳しくは、Wikipedia の[ハイパーパラメーター](https://en.wikipedia.org/wiki/Hyperparameter_(machine_learning))の記事を参照してください。

## <a name="label"></a>Label

機械学習モデルで予測される要素です。 たとえば、犬種や将来の株価などです。

## <a name="log-loss"></a>対数損失

[分類](#classification)における評価メトリックであり、分類子の正確度を示します。 対数損失が小さいほど、分類子の正確度が高くなります。

## <a name="loss-function"></a>損失関数

損失関数は、トレーニング ラベル値とモデルによって行われた予測との差です。 モデルのパラメーターは、損失関数を最小化することで推定されます。

さまざまな損失関数を使用してさまざまなトレーナーを構成できます。

## <a name="mean-absolute-error-mae"></a>平均絶対誤差 (MAE)

[回帰](#regression)における評価メトリックであり、すべてのモデルの誤差の平均です。モデルの誤差とは、予測された[ラベル](#label)値と正確なラベル値の間の距離です。

## <a name="model"></a>モデル

従来的に予測関数のパラメーターです。 たとえば、線形回帰モデルにおける重みやデシジョン ツリーにおける分割ポイントなどがあります。 ML.NET では、ドメイン オブジェクト (画像、テキストなど) の[ラベル](#label)の予測に必要なすべての情報がモデルに含まれます。 つまり、ML.NET モデルには、必要な特徴付けのステップと予測関数のパラメーターが含まれます。

## <a name="multiclass-classification"></a>多クラス分類

[ラベル](#classification)が 3 つ以上のクラスのうちの 1 つである[分類](#label)です。 詳細については、トピック「[機械学習のタスク](tasks.md#multiclass-classification)」のセクション「[多クラス分類](tasks.md)」を参照してください。

## <a name="n-gram"></a>N グラム

テキスト データの特徴抽出スキームです。N 個の単語のシーケンスが[特徴](#feature)値になります。

## <a name="normalization"></a>正規化

正規化とは、浮動小数点データを 0 から 1 の間の値にスケーリングするプロセスです。 ML.NET で使用されるトレーニング アルゴリズムの多くで、入力機能データを正規化する必要があります。 ML.NET では、[正規化のために一連の変換](transforms.md#normalization-and-scaling)を提供します。

## <a name="numerical-feature-vector"></a>数値特徴ベクトル

数値でのみ構成される[特徴](#feature)ベクトルです。 これは `double[]` に似ています。

## <a name="pipeline"></a>パイプライン

モデルをデータ セットに適合させるために必要なすべての操作です。 パイプラインは、データのインポート、変換、特徴付け、および学習の各ステップで構成されます。 トレーニングが完了したパイプラインがモデルになります。

## <a name="precision"></a>Precision

[分類](#classification)におけるクラスの精度は、そのクラスに属していると正確に予測された項目の数を、クラスに属していると予測された項目の総数で割ったものです。

## <a name="recall"></a>再現率

[分類](#classification)におけるクラスの再現率は、そのクラスに属していると正確に予測された項目の数を、実際にクラスに属している項目の総数で割ったものです。

## <a name="regularization"></a>正則化

 正則化は、複雑すぎるため、線形モデルには適していません。 正則化には 2 つの種類があります。

- $L_1$ の正則化では、重要でない特徴の重みが 0 になります。 保存されるモデルのサイズは、この種類の正則化の後に小さくなる可能性があります。
- $L_2$ の正則化では、重要でない特徴の重みの範囲を最小化します。 これは、より一般的なプロセスであり、外れ値の影響を受けにくくなります。

## <a name="regression"></a>回帰

出力が実際の値 (たとえば、倍精度) である[教師あり機械学習](#supervised-machine-learning)タスクです。 例として、株価の予測などがあります。 詳細については、トピック「[機械学習のタスク](tasks.md#regression)」のセクション「[回帰](tasks.md)」を参照してください。

## <a name="relative-absolute-error"></a>相対絶対誤差

[回帰](#regression)における評価メトリックであり、すべての絶対誤差の合計を、正確な[ラベル](#label)値とすべての正確なラベル値の平均との間の距離の合計で割ったものです。

## <a name="relative-squared-error"></a>相対平方誤差

[回帰](#regression)における評価メトリックであり、すべての平方絶対誤差の合計を、正確な[ラベル](#label)値とすべての正確なラベル値の平均との間の平方距離の合計で割ったものです。

## <a name="root-of-mean-squared-error-rmse"></a>平均平方誤差の平方根 (RMSE)

[回帰](#regression)における評価メトリックであり、誤差を 2 乗した値の平均値の平方根です。

## <a name="scoring"></a>スコアリング

スコアリングは、トレーニング済みの機械学習モデルに新しいデータを適用し、予測を生成するプロセスです。 スコアリングは推論とも呼ばれます。 モデルの種類に応じて、スコアは場合によって、未処理の値、確率、またはカテゴリになります。

## <a name="supervised-machine-learning"></a>教師あり機械学習

機械学習の 1 つの手法であり、目的となるモデルが未知のデータのラベルを予測します。 例として、分類、回帰、構造化予測などがあります。 詳しくは、Wikipedia の[教師あり学習](https://en.wikipedia.org/wiki/Supervised_learning)の記事を参照してください。

## <a name="training"></a>トレーニング

特定のトレーニング データ セットの[モデル](#model)を識別するプロセスです。 線形モデルの場合、重みの検出を意味します。 ツリーの場合、分割ポイントの識別が含まれます。

## <a name="transformer"></a>トランスフォーマー

<xref:Microsoft.ML.ITransformer> インターフェイスを実装する ML.NET クラス。

トランスフォーマーでは、ある <xref:Microsoft.ML.IDataView> が別のものに変換されます。 トランスフォーマーを作成するには、[エスティメーター](#estimator)またはエスティメーター パイプラインをトレーニングします。

## <a name="unsupervised-machine-learning"></a>教師なし機械学習

機械学習の 1 つの手法であり、目的となるモデルがデータの隠された (潜在的な) 構造を検出します。 例として、クラスタリング、トピック モデリング、次元削減などがあります。 詳しくは、Wikipedia の[教師なし学習](https://en.wikipedia.org/wiki/Unsupervised_learning)の記事を参照してください。
