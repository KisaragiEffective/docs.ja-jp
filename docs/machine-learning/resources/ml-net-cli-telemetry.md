---
title: ML.NET CLI によるテレメトリ収集
description: 分析のために使用状況の情報を収集する ML.NET CLI テレメトリ機能、収集されるデータ、機能を無効にする方法について説明します。 また、.NET の使用許諾契約と Microsoft GDPR 準拠に関する情報のリンクも紹介します。
ms.topic: conceptual
ms.date: 09/03/2019
ms.custom: mlnet-tooling
ms.openlocfilehash: 99e11acba343cc689c3219ca9316144fc62cd618
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78849745"
---
# <a name="telemetry-collection-by-the-mlnet-cli"></a>ML.NET CLI によるテレメトリ収集

[ML.NET CLI](https://aka.ms/mlnet-cli) には、Microsoft が利用するために集計している匿名の使用状況データを収集するテレメトリ機能があります。

## <a name="how-microsoft-uses-the-data"></a>Microsoft によるデータの使用方法

製品チームは、ツールの改善方法を理解するために ML.NET CLI のテレメトリ データを使用しています。 たとえば、お客様が特定の機械学習タスクをあまり使用しない場合、製品チームはその理由を調査し、その結果を使用して機能開発の優先順位を決定します。 ML.NET CLI のテレメトリは、クラッシュやコードの異常などの問題のデバッグにも役立ちます。

製品チームはこの分析情報に感謝していますが、このデータを送信したくない方がいらっしゃることも理解しています。 [テレメトリを無効にする方法についてはこちらを参照してください。](#opt-out-of-data-collection)

## <a name="scope"></a>スコープ

`mlnet` コマンドを実行すると、ML.NET CLI が起動しますが、このコマンド自体ではテレメトリが収集されません。

他のコマンドを添付せずに `mlnet` コマンドを実行すると、テレメトリは "*有効になりません*"。 次に例を示します。

- `mlnet`
- `mlnet --help`

`mlnet auto-train` などの [ML.NET CLI コマンド](../reference/ml-net-cli-reference.md)を実行すると、テレメトリは "*有効です*"。

## <a name="opt-out-of-data-collection"></a>データ収集のオプトアウト

ML.NET CLI のテレメトリ機能は既定で有効です。

`MLDOTNET_CLI_TELEMETRY_OPTOUT` 環境変数を `1` または `true` に設定して、製品利用統計情報の機能をオプトアウトします。 この環境変数は、ML.NET CLI ツールにグローバルに適用されます。

## <a name="data-points-collected"></a>収集されるデータ ポイント

この機能は次のデータを収集します。

- 呼び出されたコマンド (`auto-train` など)
- 使用されたコマンド ライン パラメーターの名前 ("dataset-name、label-column-name、ml-task、output-path、max-exploration-time、verbosity")
- ハッシュされた MAC アドレス: マシンの暗号化された (SHA256) 匿名で一意の ID
- 呼び出しのタイムスタンプ
- 地理的位置を特定するためにのみ使用される 3 オクテットの IP アドレス (完全な IP アドレスではない)
- 使用されているすべての引数/パラメーターの名前。 文字列などのお客様の値ではありません
- ハッシュされたデータセット ファイル名
- データセット ファイルサイズ バケット
- オペレーティング システムとバージョン
- タスク パラメーターの値:カテゴリ値 (`regression`、`binary-classification`、`multiclass-classification` など)
- ML.NET CLI バージョン (0.3.27703.4)

データは [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) テクノロジを使用して Microsoft サーバーに安全に送信され、制限されたアクセスの下で保持され、厳格なセキュリティ コントロールの下で安全な [Azure Storage](https://azure.microsoft.com/services/storage/) システムから使用されます。

### <a name="data-points-not-collected"></a>収集されないデータ ポイント

テレメトリ機能では、以下は収集 "*されません*"。

- ユーザー名などの個人データ
- データ セットのファイル名
- データセット ファイルのデータ

ML.NET CLI テレメトリで機密データが収集されている、またはデータの処理が安全ではないか不適切であると思われる場合は、調査のために [ML.NET](https://github.com/dotnet/machinelearning) リポジトリで問題を報告してください。

## <a name="license"></a>ライセンス

Microsoft による ML.NET CLI の配布には、[マイクロソフト ソフトウェア ライセンス条項:Microsoft .NET ライブラリ](https://aka.ms/dotnet-core-eula)に基づいてライセンスが付与されます。 データの収集と処理の詳細については、「データ」というタイトルのセクションを参照してください。

## <a name="disclosure"></a>開示

`mlnet auto-train` などの [ML.NET CLI コマンド](../reference/ml-net-cli-reference.md)を初めて実行すると、ML.NET CLI ツールにテレメトリをオプトアウトする方法が説明された開示テキストが表示されます。 テキストは、実行している CLI のバージョンによって多少異なります。

## <a name="see-also"></a>関連項目

- [ML.NET CLI リファレンス](../reference/ml-net-cli-reference.md)
- [マイクロソフト ソフトウェア ライセンス条項:Microsoft .NET ライブラリ](https://aka.ms/dotnet-core-eula)
- [Microsoft におけるプライバシー](https://www.microsoft.com/trustcenter/privacy/)
- [Microsoft のプライバシーに関する声明](https://privacy.microsoft.com/privacystatement)
