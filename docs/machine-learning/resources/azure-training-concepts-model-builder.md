---
title: モデル ビルダーの Azure トレーニング リソース
description: Azure Machine Learning のリソース ガイド
ms.topic: reference
ms.date: 02/27/2020
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: a19e13955d0eaea344109eb817f3a3959c3dd883
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79185822"
---
# <a name="model-builder-azure-training-resources"></a>モデル ビルダーの Azure トレーニング リソース

次に、Azure でモデル ビルダーを使用してモデルをトレーニングするときに、使用できるリソースを説明します。

## <a name="what-is-an-azure-machine-learning-experiment"></a>Azure Machine Learning の実験とは

Azure Machine Learning の実験とは、Azure でモデル ビルダーのトレーニングを実行する前に作成する必要があるリソースです。

実験には、1 回以上の機械学習のトレーニングの実行の構成と結果がカプセル化されます。 実験は特定のワークスペースに属します。 ワークスペースの名前は、実験の初回作成時に登録されます。 それ以降同じ実験名を使用した場合、実行は同じ実験の一部としてログに記録されます。 そうでない場合は、新しい実験が作成されます。

## <a name="what-is-an-azure-machine-learning-workspace"></a>Azure Machine Learning ワークスペースとは

ワークスペースとは、トレーニングの実行の一貫で作成された Azure Machine Learning のすべてのリソースと成果物の一元的な場所である Azure Machine Learning のリソースです。

Azure Machine Learning ワークスペースの作成には、次が必要です。

- 名前:3 から 33 文字のお使いのワークスペースの名前。 名前には、英数字とハイフンのみを使用できます。
- リージョン:お使いのワークスペースとリソースがデプロイされているデータ センターの地理的な場所。 ご自分またはお客様に近い場所を選択することをお勧めします。
- リソース グループ: Azure ソリューションの関連するリソースをすべて保持するコンテナー。

## <a name="what-is-an-azure-machine-learning-compute"></a>Azure Machine Learning コンピューティングとは

Azure Machine Learning コンピューティングとは、トレーニングに使用するクラウドベースの Linux VM です。

Azure Machine Learning ワークスペースの作成には、次が必要です。

- 名前:2 から 16 文字のお使いのワークスペースの名前。 名前には、英数字とハイフンのみを使用できます。
- コンピューティング サイズ

    モデル ビルダーでは、GPU に最適化された次のいずれかのコンピューティングの種類を使用できます。

    | サイズ | vCPU | メモリ:GiB | 一時ストレージ (SSD) GiB | GPU | GPU メモリ: GiB | 最大データ ディスク数 | 最大 NIC 数 |
    |---|---|---|---|---|---|---|---|
    | Standard_NC12   | 12 | 112 | 680  | 2 | 24 | 48 | 2 |
    | Standard_NC24   | 24 | 224 | 1440 | 4 | 48 | 64 | 4 |

    GPU に最適化されたコンピューティングの種類の詳細については、[NC シリーズの Linux VM のドキュメント](https://docs.microsoft.com/azure/virtual-machines/nc-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json)に関するページを参照してください。

## <a name="training"></a>トレーニング

Azure でのトレーニングは、モデル ビルダーの画像分類のシナリオのみを用意しています。 これらのモデルのトレーニングに使用されるアルゴリズムは、ResNet50 アーキテクチャに基づくディープ ニューラル ネットワークです。 トレーニングには時間がかかり、この時間は選択したコンピューティングのサイズやデータ量によって変わります。 モデルの初回トレーニング時は、リソースのプロビジョニングのためにトレーニング時間が若干長くなります。 Visual Studio で [Monitor current run in Azure portal]\(Azure portal で現在の実行の監視\) リンクを選択すると、ご自分の実行の進行状況を追跡できます。

## <a name="results"></a>結果

トレーニングが完了すると、次のサフィックスが使用された 2 つのプロジェクトがお使いのソリューションに追加されます。

- *ConsoleApp*:予測パイプラインの構築と予測を行うスタート コードの提供を行う C# の .NET Core コンソール アプリケーション。
- *モデル*:入力および出力モデル データのスキーマと、次の資産を定義するデータ モデルを含む C# .NET Standard アプリケーション。

  - bestModel.onnx:モデルの Open Neural Network Exchange (ONNX) 形式でのシリアル化されたバージョン。 ONNX は、ML.NET、PyTorch、および TensorFlow などのフレームワーク間で相互運用を可能にする、オープンソースの AI モデル用の形式です。
  - bestModelMap.json:モデルの出力をテキストのカテゴリにマップする、予測に使用するカテゴリの一覧。
  - MLModel.zip:`bestModelMap.json` ファイルを使用して予測を行い、出力をマップする、モデル *bestModel.onnx* のシリアル化されたバージョンを使用する ML.NET 予測パイプラインのシリアル化されたバージョン。

## <a name="use-the-machine-learning-model"></a>機械学習モデルの使用

*モデル* プロジェクトの `ModelInput` クラスと `ModelOutput` クラスでは、モデルで期待される入力と出力のスキーマをそれぞれ定義します。

`ModelInput` には、画像の分類シナリオの場合、2 つ列があります。

- `ImageSource`:文字列での画像の場所のパス。
- `Label`:画像が属する実際のカテゴリです。 `Label` は、トレーニング時に入力としてのみ使用され、予測を行うときに指定する必要はありません。

`ModelOutput` には、列が 2 つあります。

- `Prediction`:画像の予想されるカテゴリ。
- `Score`:すべてのカテゴリの可能性の一覧 (最高値は `Prediction`)。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="cannot-create-compute"></a>コンピューティングを作成できない

Azure Machine Learning でコンピューティングの作成中にエラーが発生した場合、エラー状態でコンピューティング リソースが存在し続ける場合があります。 同じ名前でコンピューティング リソースを再作成しようとすると、その操作は失敗します。 このエラーを修正するには、次のいずれかを実行します。

- 新しいコンピューティングを別名で作成します
- Azure portal に移動し、元のコンピューティング リソースを削除します
