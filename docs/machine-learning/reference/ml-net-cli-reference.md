---
title: ML.NET CLI コマンド リファレンス
description: ML.NET CLI ツールの auto-train コマンドの概要、サンプル、およびリファレンス。
ms.date: 12/18/2019
ms.custom: mlnet-tooling
ms.openlocfilehash: bb161c596a76134876ee2bf0a6229bc551e0dad2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78848926"
---
# <a name="the-mlnet-cli-command-reference"></a>ML.NET CLI コマンド リファレンス

`auto-train` コマンドは、ML.NET CLI ツールが提供するメイン コマンドです。 このコマンドを使用すると、自動機械学習 (AutoML) を使用する高品質の ML.NET モデルと、そのモデルを実行/スコア付けするサンプル C# コードを生成できます。 さらに、そのモデルをトレーニングするための C# コードも生成され、そのモデルのアルゴリズムと設定を調べることができます。

> [!NOTE]
> このトピックは、現在プレビュー段階の ML.NET CLI と ML.NET AutoML について述べており、内容が変更される場合があります。

## <a name="overview"></a>概要

使用例:

```console
mlnet auto-train --task regression --dataset "cars.csv" --label-column-name price
```

`mlnet auto-train` コマンドで、以下の資産が生成されます。

- すぐに使用できるシリアル化されたモデル .zip ("最適なモデル")。
- その生成されたモデルを実行/スコア付けする C# コード。
- そのモデルの生成に使用されるトレーニング コードを含む C# コード。

最初の 2 つの資産は、モデルを使用して予測を行うために、エンド ユーザー アプリ (ASP.NET Core Web アプリ、サービス、デスクトップ アプリなど) で直接使用できます。

3 つ目の資産のトレーニング コードは、生成されたモデルをトレーニングするために CLI によって使用された ML.NET API コードを示しています。このため、そのモデルの特定のアルゴリズムと設定を調べることができます。

## <a name="examples"></a>使用例

二項分類問題に適した最も簡単な CLI コマンド (AutoML では、指定されたデータからほとんどの構成を推測します):

```console
mlnet auto-train --task binary-classification --dataset "customer-feedback.tsv" --label-column-name Sentiment
```

回帰問題に適したもう 1 つの簡単な CLI コマンド:

``` console
mlnet auto-train --task regression --dataset "cars.csv" --label-column-name Price
```

トレーニング データセット、テスト データセット、およびその他のカスタマイズの明示的な引数を使用して、二項分類モデルを作成し、トレーニングします。

```console
mlnet auto-train --task binary-classification --dataset "/MyDataSets/Population-Training.csv" --test-dataset "/MyDataSets/Population-Test.csv" --label-column-name "InsuranceRisk" --cache on --max-exploration-time 600
```

## <a name="command-options"></a>コマンド オプション

`mlnet auto-train` は、指定されたデータセットに基づいて複数のモデルをトレーニングし、最後に最適なモデルを選択し、シリアル化された .zip ファイルとして保存します。さらに、スコア付けとトレーニングに関連する C# コードを生成します。

```console
mlnet auto-train

--task | --mltask | -T <value>

--dataset | -d <value>

[
 [--validation-dataset | -v <value>]
  --test-dataset | -t <value>
]

--label-column-name | -n <value>
|
--label-column-index | -i <value>

[--ignore-columns | -I <value>]

[--has-header | -h <value>]

[--max-exploration-time | -x <value>]

[--verbosity | -V <value>]

[--cache | -c <value>]

[--name | -N <value>]

[--output-path | -o <value>]

[--help | -h]

```

無効な入力オプションを指定すると、CLI ツールによって、有効な入力の一覧とエラー メッセージが生成されます。

## <a name="task"></a>タスク

`--task | --mltask | -T` (文字列)

解決する ML の問題を示す 1 つの文字列。 たとえば、次のいずれかのタスクです (CLI は、最終的には AutoML でサポートされているすべてのタスクをサポートする予定です)。

- `regression` - 数値の予測に ML モデルを使用するかどうかを選択します
- `binary-classification` - ML モデルの結果に、2 つの可能なカテゴリ別のブール値 (0 または 1) があるかどうかを選択します。
- `multiclass-classification` - ML モデルの結果に、複数の可能なカテゴリ別の値があるかどうかを選択します。

この引数には、ML タスクを 1 つのみ指定する必要があります。

## <a name="dataset"></a>データセット

`--dataset | -d` (文字列)

この引数には、次のオプションのいずれかのファイルパスを指定します。

- *A:データセット ファイル全体:* このオプションを使用し、ユーザーが `--test-dataset` と `--validation-dataset` を指定していない場合は、モデルの検証にクロス検証 (k 分割など) または自動データ分割アプローチが内部的に使用されます。 その場合、ユーザーはデータセットのファイルパスを入力するだけで済みます。

- *B:トレーニング データセット ファイル:* ユーザーがモデル検証用のデータセットも指定している場合 (`--test-dataset` と、必要に応じて `--validation-dataset` を使用)、`--dataset` 引数は "トレーニング データセット" のみがあることを意味します。 たとえば、80% - 20% のアプローチを使用してモデルの品質を検証し、正確度のメトリックを取得する場合、"トレーニング データセット" には 80% のデータが含まれ、"テスト データセット" には 20% のデータが含まれます。

## <a name="test-dataset"></a>テスト データセット

`--test-dataset | -t` (文字列)

テスト データセット ファイルを指すファイル パス。たとえば、正確度のメトリックを取得するために定期的な検証を行うときに 80% - 20% のアプローチを使用する場合です。

`--test-dataset` を使用する場合は `--dataset` も必要です。

--validation-dataset が使用されていない限り、`--test-dataset` 引数は省略可能です。 その場合、ユーザーは 3 つの引数を使用する必要があります。

## <a name="validation-dataset"></a>検証データセット

`--validation-dataset | -v` (文字列)

検証データセット ファイルを指すファイル パス。 どのような場合でも、検証データセットは省略可能です。

`validation dataset` を使用している場合、動作は次のようになります。

- `test-dataset` と `--dataset` の引数も必須です。

- `validation-dataset` データセットは、モデル選択の予測誤差を推定するために使用されます。

- `test-dataset` は、最終的に選択されたモデルの一般化誤差の評価に使用されます。 テスト セットを "コンテナー" に保管し、データ分析の最後にのみ取り出すのが理想的です。

基本的に、`validation dataset` と `test dataset` を使用する場合、検証フェーズは 2 つの部分に分けられます。

1. 1 つ目の部分では、モデルを見て、検証データを使用して最適なパフォーマンスのアプローチを選択します (= 検証)。
2. 次に、選択したアプローチの正確度を見積もります (= テスト)。

そのため、データの分離は 80/10/10 または 75/15/10 になります。 次に例を示します。

- `training-dataset` ファイルには 75% のデータがあります。
- `validation-dataset` ファイルには 15% のデータがあります。
- `test-dataset` ファイルには 10% のデータがあります。

いずれの場合でも、これらのパーセンテージは、既に分割されているファイルを指定する CLI の使用ユーザーが決定します。

## <a name="label-column-name"></a>ラベル列の名前

`--label-column-name | -n` (文字列)

この引数を使用すると、データセットのヘッダーに設定されている列の名前を使って、特定の目的/ターゲット列 (予測する変数) を指定できます。

この引数は、*分類問題*など、教師ありの ML タスクにのみ使用されます。 *クラスタリング*など、教師なしの ML タスクには使用できません。

## <a name="label-column-index"></a>ラベル列のインデックス

`--label-column-index | -i` (int)

この引数を使用すると、データセットのファイル内の列の数値インデックスを使用して、特定の目的/ターゲット列 (予測する変数) を指定できます (列インデックス値は 1 から始まります)。

*注:* ユーザーが `--label-column-name` も使用している場合は、`--label-column-name` が使用されます。

この引数は、*分類問題*など、教師ありの ML タスクにのみ使用されます。 *クラスタリング*など、教師なしの ML タスクには使用できません。

## <a name="ignore-columns"></a>列を無視

`--ignore-columns | -I` (文字列)

この引数を使用すると、トレーニング プロセスで読み込まれたり使用されたりしないように、データセット ファイル内の既存の列を無視することができます。

無視する列名を指定します。 複数の列名を区切るには、", " (コンマとスペース) または " " (スペース) を使用します。 空白を含む列名には引用符を使用することができます (例: "logged in")。

例:

`--ignore-columns email, address, id, logged_in`

## <a name="has-header"></a>ヘッダーの有無

`--has-header | -h` (ブール値)

データセット ファイルにヘッダー行があるかどうかを指定します。
指定できる値は次のとおりです。

- `true`
- `false`

この引数がユーザーによって指定されていない場合、既定で値は `true` です。

`--label-column-name` 引数を使用するには、データセット ファイルにヘッダーを含め、`--has-header` を `true` に設定する必要があります (これが既定値です)。

## <a name="max-exploration-time"></a>最大探索時間

`--max-exploration-time | -x` (文字列)

既定では、最大探索時間は 30 分です。

この引数では、プロセスで複数のトレーナーと構成を探索できる最長時間 (秒単位) を設定します。 指定した時間が 1 回の反復処理に対して短すぎる (たとえば 2 秒) 場合、構成された時間を超えることがあります。 この場合、実際の時間は、1 回の反復処理で 1 つのモデル構成を作成するために必要な時間です。

反復処理に必要な時間は、データセットのサイズによって変わります。

## <a name="cache"></a>キャッシュ

`--cache | -c` (文字列)

キャッシングを使用すると、トレーニング データセット全体がメモリ内に読み込まれます。

中小規模のデータセットでは、キャッシュを使用するとトレーニングのパフォーマンスが大幅に向上します。つまり、キャッシュを使用しない場合よりもトレーニング時間が短くなる可能性があります。

ただし、大規模なデータセットの場合、メモリ内のすべてのデータを読み込むと、メモリ不足になる可能性があるので、悪影響が出る可能性があります。 大規模なデータセット ファイルを使用してトレーニングし、キャッシュを使用しない場合、ML.NET では、トレーニング中により多くのデータを読み込む必要があるときに、ドライブから大量のデータがストリーミングされます。

次の値を指定できます。

`on`:トレーニング時にキャッシュを強制的に使用します。
`off`:トレーニング時にキャッシュを使用しないように強制します。
`auto`:AutoML のヒューリスティックに応じて、キャッシュを使用するかどうかが決まります。 通常、`auto` を選択した場合、中小規模のデータセットではキャッシュが使用され、大規模なデータセットではキャッシュが使用されません。

`--cache` パラメーターを指定しない場合、既定でキャッシュの `auto` 構成が使用されます。

## <a name="name"></a>名前

`--name | -N` (文字列)

作成される出力プロジェクトまたはソリューションの名前。 名前が指定されていない場合は、名前 `sample-{mltask}` が使用されます。

ML.NET モデル ファイル (.ZIP ファイル) も同じ名前になります。

## <a name="output-path"></a>出力パス

`--output-path | -o` (文字列)

生成された出力を配置するルートの場所/フォルダー。 既定値は、現在のディレクトリです。

## <a name="verbosity"></a>詳細度

`--verbosity | -V` (文字列)

標準出力の詳細レベルを設定します。

次の値を指定できます。

- `q[uiet]`
- `m[inimal]` (既定値)
- `diag[nostic]` (ログ情報レベル)

既定で CLI ツールには、作業時に最小限のフィードバック (最小) が表示されます。たとえば、作業中かどうか、可能であれば残り時間、または完了した時間の割合などが示されます。

## <a name="help"></a>ヘルプ

`-h|--help`

各コマンドのパラメーターの説明と共に、コマンドのヘルプを出力します。

## <a name="see-also"></a>関連項目

- [ML.NET CLI ツールのインストール方法](../how-to-guides/install-ml-net-cli.md)
- [ML.NET CLI の概要](../automate-training-with-cli.md)
- [チュートリアル: ML.NET CLI を使用してセンチメントを分析する](../tutorials/sentiment-analysis-cli.md)
- [ML.NET CLI のテレメトリ](../resources/ml-net-cli-telemetry.md)
