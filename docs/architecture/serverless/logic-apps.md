---
title: Azure Logic Apps - サーバーレス アプリ
description: Azure Logic Apps では、クラウド サービスとオンプレミス システムにまたがったアプリとデータを統合する、自動化されたスケーラブルなワークフローを構築できます。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 7ece3d30209713d42ee44ef9c1be1cf0fe82464a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69577455"
---
# <a name="azure-logic-apps"></a>Azure Logic Apps

[Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps) はサーバーレス エンジンを備えており、クラウド サービスとオンプレミス システムの間でアプリやデータを統合する、自動化されたワークフローを構築できます。 ワークフローはビジュアル デザイナーを使用して作成します。 イベントまたはタイマーに基づいてワークフローをトリガーし、統合アプリケーションへのコネクタを活用し、企業間 (B2B) 通信を容易にすることができます。 Logic Apps は Azure Functions とシームレスに統合されます。

![Azure Logic Apps ロゴ](./media/logic-apps-logo.png)

Logic Apps では、クラウド サービス (関数など) をクラウド リソース (キューやデータベースなど) と接続する以上のことができます。 オンプレミス ゲートウェイでオンプレミス ワークフローを調整することもできます。 たとえば、ワークフロー内のラウドベース イベントや条件付きロジックに対する応答として、Logic App を使用し、オンプレミス SQL ストアド プロシージャをトリガーできます。 Azure のオンプレミス データ ゲートウェイを使用してオンプレミスのデータ ソースに接続する方法の詳細は[こちら](https://docs.microsoft.com/azure/analysis-services/analysis-services-gateway)をご覧ください。

![Logic Apps アーキテクチャ](./media/logic-apps-architecture.png)

Azure Functions と同様に、Logic App ワークフローはトリガーで開始できます。 さまざまなトリガーから選択できます。 次のキャプチャでは、利用できるたくさんのトリガーの中から人気のトリガーのほんの一部が選ばれています。

![Logic Apps トリガー](./media/logic-app-triggers.png)

アプリがトリガーされたら、ビジュアル デザイナーを使用し、ステップ、ループ、条件、アクションを構築できます。 前の手順で取り込まれたデータはすべて、後続の手順で利用できます。 次のワークフローでは、CosmosDB データベースから URL が読み込まれます。 ホストが `t.co` の URL が見つけられ、Twitter 上のものが検索されます。 該当するツイートが見つかった場合、関数を呼び出すことで、関連するツイートでドキュメントが更新されます。

![Logic App ワークフロー](./media/logic-app-workflow.png)

Logic Apps ダッシュボードには、ワークフローの実行履歴と各実行の成功/失敗が表示されます。 特定の実行を表示したり、各手順で使用されたデータを検査し、問題を解決したりできます。 Logic Apps には自分で編集できるテンプレートがあり、複雑な企業ワークフローに適しています。

詳細については、[Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps)に関するページを参照してください。

>[!div class="step-by-step"]
>[前へ](application-insights.md)
>[次へ](event-grid.md)
