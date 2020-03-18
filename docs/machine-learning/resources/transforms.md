---
title: データ変換
description: ML.NET でサポートされている機能エンジニアリングのコンポーネントについて検証します。
ms.date: 04/02/2019
ms.openlocfilehash: ca410b475c556db5ad4c3862fb79755b455d6830
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397753"
---
# <a name="data-transformations"></a>データ変換

データ変換は次の操作を行うために使用されます。

- モデルのトレーニング用にデータを準備する
- インポートされたモデルを TensorFlow または ONNX 形式に適用する
- モデルから渡されたデータを後処理する

このガイドの変換は、[IEstimator](xref:Microsoft.ML.IEstimator%601) インターフェイスを実装するクラスを返します。 データ変換はまとめて連結することができます。 各変換は、リンクされた参照ドキュメントで指定されている特定の種類および形式のデータを想定し、生成します。

一部のデータ変換には、そのパラメーターを計算するためにトレーニング データが必要です。 たとえば、<xref:Microsoft.ML.NormalizationCatalog.NormalizeMeanVariance%2A> トランスフォーマーは、`Fit()` の操作中にトレーニング データの平均と分散を計算し、`Transform()` 操作でそのパラメーターを使用します。

他のデータ変換はトレーニング データを必要としません。 たとえば、<xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToGrayscale%2A> の変換は、`Transform()` の操作中にトレーニング データを確認せずに `Fit()` の操作を実行できます。

## <a name="column-mapping-and-grouping"></a>列のマップとグループ化

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A> | 1 つ以上の入力列を新しい出力列に連結します |
| <xref:Microsoft.ML.TransformExtensionsCatalog.CopyColumns%2A> | 1 つ以上の入力列をコピーして名前を変更します |
| <xref:Microsoft.ML.TransformExtensionsCatalog.DropColumns%2A> | 1 つ以上の入力列をドロップします |
| <xref:Microsoft.ML.TransformExtensionsCatalog.SelectColumns%2A> | 入力データから保持する 1 つ以上の列を選択します |

## <a name="normalization-and-scaling"></a>正規化とスケーリング

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeMeanVariance%2A> | (トレーニング データの) 平均を引き、(トレーニング データの) 分散で割ります |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeLogMeanVariance%2A> | トレーニング データの対数に基づいて正規化します |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeLpNorm%2A> | 入力ベクターを [lp-norm](https://en.wikipedia.org/wiki/Lp_space#The_p-norm_in_finite_dimensions) でスケーリングします。この p は 1、2、または無限大です。 既定値は l2 (ユークリッド距離) ノルムです |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeGlobalContrast%2A> | 行データの平均を減算して行の各値をスケーリングし、標準偏差または (行データの) l2-norm で除算し、構成可能なスケール係数 (既定値は 2) で乗算します |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeBinning%2A> | 入力値をビンのインデックスに割り当て、ビンの数で除算して 0 から 1 の間の float 値を生成します。 ビンの境界は、ビン全体にトレーニング データを均等に分散するように計算されます |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeSupervisedBinning%2A> | ラベル列との相関関係に基づいて入力値をビンに割り当てます |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax%2A> | トレーニング データの最小値と最大値の差で入力をスケーリングします |

## <a name="conversions-between-data-types"></a>データ型間の変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.ConvertType%2A> | 入力列の型を新しい型に変換します |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValue%2A> | 指定されたマッピングのディクショナリに基づいて、値をキー (カテゴリ) にマップします。 |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A> | 入力データからマッピングを作成して値をキー (カテゴリ) にマップします |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapKeyToValue%2A> | キーを変換して元の値に戻します |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapKeyToVector%2A> | キーを変換して元の値のベクターに戻します |
| <xref:Microsoft.ML.ConversionsCatalog.MapKeyToBinaryVector%2A> | キーを変換して元の値のバイナリ ベクターに戻します |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.Hash%2A> | 入力列の値をハッシュします |

## <a name="text-transformations"></a>テキスト変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.TextCatalog.FeaturizeText%2A> | テキスト列を正規化された ngram と char-gram のカウントの float 配列に変換します |
| <xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A> | 1 つ以上のテキスト列を個々の単語に分割します |
| <xref:Microsoft.ML.TextCatalog.TokenizeIntoCharactersAsKeys%2A> | 1 つ以上のテキスト列を一連のトピックに関する個々の文字 float に分割します |
| <xref:Microsoft.ML.TextCatalog.NormalizeText%2A> | 大文字と小文字の変更、分音記号、句読点、数字の削除を行います |
| <xref:Microsoft.ML.TextCatalog.ProduceNgrams%2A> | テキスト列を ngram のカウント (連続する単語のシーケンス) バッグに変換します|
| <xref:Microsoft.ML.TextCatalog.ProduceWordBags%2A> | テキスト列を ngram ベクターのバッグに変換します |
| <xref:Microsoft.ML.TextCatalog.ProduceHashedNgrams%2A> | テキスト列をハッシュされた ngram のカウントのベクターに変換します |
| <xref:Microsoft.ML.TextCatalog.ProduceHashedWordBags%2A> | テキスト列をハッシュされた ngram のカウントのバッグに変換します |
| <xref:Microsoft.ML.TextCatalog.RemoveDefaultStopWords%2A>  | 指定された言語の既定のストップ ワードを入力列から削除します |
| <xref:Microsoft.ML.TextCatalog.RemoveStopWords%2A> | 入力列から指定されたストップ ワードを削除します |
| <xref:Microsoft.ML.TextCatalog.LatentDirichletAllocation%2A> | 一連のトピックにわたって、ドキュメント (float のベクターとして表されます) を float のベクターに変換します |
| <xref:Microsoft.ML.TextCatalog.ApplyWordEmbedding%2A> | レーニング済みのモデルを使用して、テキスト トークンを文のベクターに変換します |

## <a name="image-transformations"></a>画像変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToGrayscale%2A> | 画像をグレースケールに変換します |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToImage%2A> | ピクセルのベクターを <xref:Microsoft.ML.Transforms.Image.ImageDataViewType> に変換します |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ExtractPixels%2A> | 入力画像のピクセルを数値のベクターに変換します |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages%2A> | フォルダーの画像をメモリに読み込みます |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ResizeImages%2A> | イメージのサイズを変更する |
| <xref:Microsoft.ML.OnnxCatalog.DnnFeaturizeImage%2A> | 事前トレーニング済みのディープ ニューラル ネットワーク (DNN) モデルを適用して、入力イメージを特徴ベクターに変換します |

## <a name="categorical-data-transformations"></a>分類データの変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.CategoricalCatalog.OneHotEncoding%2A> | 1 つ以上のテキスト列を [one-hot](https://en.wikipedia.org/wiki/One-hot) エンコード済みベクターに変換します |
| <xref:Microsoft.ML.CategoricalCatalog.OneHotHashEncoding%2A> | 1 つまたは複数のテキスト列をハッシュベースの one-hot エンコード済みベクターに変換します |

## <a name="time-series-data-transformations"></a>時系列データの変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectAnomalyBySrCnn%2A> | Spectral Residual (SR) アルゴリズムを使用して、時系列入力データの異常を検出します |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectChangePointBySsa%2A> | 単一のスペクトラム分析 (SSA) を使用して、時系列データの変化点を検出します |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectIidChangePoint%2A> | アダプティブ カーネル密度見積もりとマルチンゲール スコアを使用して、独立同分布 (IID) の時系列データ内の変化点を検出します |
| <xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa%2A> | 単一のスペクトラム分析 (SSA) を使用して、時系列データを予測します |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectSpikeBySsa%2A> | 単一のスペクトラム分析 (SSA) を使用して、時系列データのスパイクを検出します |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectIidSpike%2A> | アダプティブ カーネル密度見積もりとマルチンゲール スコアを使用して、独立同分布 (IID) の時系列データのスパイクを検出します |

## <a name="missing-values"></a>不足している値

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.ExtensionsCatalog.IndicateMissingValues%2A> | 新しいブール出力列を作成します。入力列の値が欠落している場合、その値は true です。 |
| <xref:Microsoft.ML.ExtensionsCatalog.ReplaceMissingValues%2A> | 新しい出力列を作成します。値が入力列にない場合は値が既定値に設定され、それ以外の場合は入力値が設定されます |

## <a name="feature-selection"></a>特徴選択

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.FeatureSelectionCatalog.SelectFeaturesBasedOnCount%2A> | 既定以外の値がしきい値より大きい特徴を選択します |
| <xref:Microsoft.ML.FeatureSelectionCatalog.SelectFeaturesBasedOnMutualInformation%2A> | ラベル列のデータが最も依存している特徴を選択します |

## <a name="feature-transformations"></a>特徴の変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.KernelExpansionCatalog.ApproximatedKernelMap%2A> | 各入力ベクターを下位次元の特徴空間にマップします。ここでは、線形アルゴリズムへの入力として特徴を使用できるように、内部製品でカーネル関数が概算されます |
| <xref:Microsoft.ML.PcaCatalog.ProjectToPrincipalComponents%2A> | プリンシパル コンポーネント分析アルゴリズムを適用して、入力特徴ベクトルの次元を減らします |

## <a name="explainability-transformations"></a>説明可能性の変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.ExplainabilityCatalog.CalculateFeatureContribution%2A> | 特徴ベクターの要素ごとにコントリビューション スコアを計算します |

## <a name="calibration-transformations"></a>調整の変換

| 変換 | 定義 |
| --- | --- |
|<xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Platt%28System.String%2CSystem.String%2CSystem.String%29> | トレーニング データを使用して予測されるパラメーターと共にロジスティック回帰を使用し、二項分類子の生スコアをクラスの確率に変換します |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Platt%28System.Double%2CSystem.Double%2CSystem.String%29> | 固定パラメーターと共にロジスティック回帰を使用して、二項分類子の生スコアをクラスの確率に変換します |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Naive%2A> | スコアをビンに割り当て、ビン間の分布に基づいて確率を計算して、二項分類子の生スコアをクラスの確率に変換します |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Isotonic%2A> | スコアをビンに割り当てて、二項分類子の生スコアをクラスの確率に変換します。このとき、境界の位置とビンのサイズは、トレーニング データを使用して推定されます  |

## <a name="deep-learning-transformations"></a>ディープ ラーニングの変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel%2A> | インポートされた ONNX モデルを使用して入力データを変換する |
| <xref:Microsoft.ML.TensorflowCatalog.LoadTensorFlowModel%2A> | インポートされた TensorFlow モデルを使用して入力データを変換する |

## <a name="custom-transformations"></a>カスタム変換

| 変換 | 定義 |
| --- | --- |
| <xref:Microsoft.ML.CustomMappingCatalog.CustomMapping%2A> | ユーザー定義マッピングを使用して既存の列を新しい列に変換します |
