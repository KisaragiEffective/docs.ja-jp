---
title: Azure Monitor
description: Azure Monitor を使用すると、システムが実行されていることを確認できます。
ms.date: 02/05/2020
ms.openlocfilehash: 87ffca186346c3356c0277809d1d67145d1dd17b
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628034"
---
# <a name="azure-monitor"></a>Azure Monitor

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

他のクラウドプロバイダーは、Azure でのクラウドアプリケーションの監視ソリューションの成熟度としては機能しません。 Azure Monitor は、システムの状態の可視性を提供するように設計されたツールの包括的な名前であり、問題を把握し、アプリケーションの最適化を行うことができます。

![Azure Monitor は、クラウドネイティブアプリケーションがどのように機能しているかについての洞察を提供するためのツールのコレクションです。](./media/azure-monitor.png)
**図 7-12**。 Azure Monitor、クラウドネイティブアプリケーションがどのように機能しているかについての洞察を提供するためのツールのコレクションです。

## <a name="gathering-logs-and-metrics"></a>ログとメトリックの収集

監視ソリューションの最初の手順は、可能な限り多くのデータを収集することです。 収集できるデータが多いほど、取得できる洞察が深くなります。 従来、システムのインストルメント化は困難でした。 Simple Network Management Protocol (SNMP) は、マシンレベルの情報を収集するための gold 標準プロトコルでしたが、多くの知識と構成を必要としていました。 幸いにも、最も一般的なメトリックは Azure Monitor によって自動的に収集されるため、このようなハード作業の大部分は排除されています。

アプリケーションレベルのメトリックとイベントは、デプロイされるアプリケーションに固有のものであるため、自動的にインストルメント化することはできません。 これらのメトリックを収集するために、顧客が注文にサインアップまたは完了したときなど、このような情報を直接報告するための[sdk と api](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics)が用意されています。 例外をキャプチャして、Application Insights 経由で Azure Monitor に返すこともできます。 Sdk は、ゴー、Python、JavaScript、.NET 言語などのクラウドネイティブアプリケーションで検出されたほとんどすべての言語をサポートしています。

アプリケーションの状態に関する情報を収集する最終的な目標は、エンドユーザーのエクスペリエンスを向上させることです。 ユーザーが[外部の web テストを](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability)実行する場合よりも問題が発生しているかどうかを確認するには、どうすればよいでしょうか。 これらのテストは、世界中の場所から web サイトに ping を実行したり、エージェントがサイトにログインしてユーザーの操作をシミュレートしたりするのと同じように簡単に行うことができます。

## <a name="reporting-data"></a>レポートデータ

データが収集されると、データの操作、要約、およびグラフへのプロットが可能になり、問題が発生したときにすぐにユーザーが表示できるようになります。 これらのグラフは、システムのいくつかの側面についてストーリーを示すために設計された複数ページのレポートである、ダッシュボードまたはブックに収集できます。

人工知能や機械学習を行わずに、最新のアプリケーションを完成させることはできません。 このため、Azure のさまざまな machine learning ツールにデータを[渡すことが](https://www.youtube.com/watch?v=Cuza-I1g9tw)できるため、表示されない傾向や情報を抽出できます。

Application Insights には Kusto と呼ばれる強力なクエリ言語が用意されており、これを使用してレコードの検索、集計、グラフのプロットを行うことができます。 たとえば、このクエリでは、2007年11月のすべてのレコードが検索され、州ごとにグループ化され、上位10が円グラフとしてプロットされます。

```kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| summarize count() by State
| top 10 by count_
| render piechart
```

Application Insights クエリの結果 ![、**図 7-13**](./media/azure-monitor.png)
ます。 Application Insights クエリの結果。

[Kusto クエリを試してみるためのプレイグラウンド](https://dataexplorer.azure.com/clusters/help/databases/Samples)は、1時間または2時間のすばらしい場所です。 [サンプルクエリ](https://docs.microsoft.com/azure/kusto/query/samples)の読み取りも参考になる場合があります。

## <a name="dashboards"></a>ダッシュボード

Azure Monitor から情報を表示するために使用できる、いくつかの異なるダッシュボードテクノロジがあります。 単純に、Application Insights でクエリを実行し、[データをグラフにプロット](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-app-dashboards)するだけです。

メインの Azure ダッシュボードに埋め込まれた Application Insights グラフの例 ![](./media/azure-monitor.png)
**図 7-14**。 メインの Azure ダッシュボードに埋め込まれている Application Insights グラフの例を示します。

これらのグラフは、ダッシュボード機能を使用して適切に Azure portal に埋め込むことができます。 複数の層のデータにドリルダウンできるなど、より厳しい要件を持つユーザーには、Azure Monitor データを[Power BI](https://powerbi.microsoft.com/)できます。 Power BI は、業界をリードするエンタープライズクラスのビジネスインテリジェンスツールであり、さまざまなデータソースからデータを集計できます。

Power BI ダッシュボードの例を ![](./media/azure-monitor.png)
**図 7-15**。 ダッシュボード Power BI 例。

## <a name="alerts"></a>アラート

データダッシュボードが不十分な場合もあります。 ダッシュボードの監視が開始されていない場合でも、問題が解決されるまで、または検出されるまでに数時間かかることがあります。 このため、Azure Monitor には、上位ノッチの[アラートソリューション](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)も用意されています。 アラートは、次のような幅広い条件によってトリガーできます。

- メトリックの値
- ログの検索クエリ
- アクティビティ ログのイベント
- 基になっている Azure プラットフォームの正常性
- Web サイトの可用性のテスト

トリガーされると、アラートはさまざまなタスクを実行できます。 単純な側では、アラートは電子メールによる通知をメーリングリストまたはテキストメッセージに送信するだけです。 さらに関連するアラートによって、PagerDuty などのツールでワークフローがトリガーされることがあります。これは、特定のアプリケーションの呼び出し元を認識します。 アラートは、ワークフローのほぼ無制限の可能性を[Microsoft Flow](https://flow.microsoft.com/)にロックを解除するアクションをトリガーできます。

アラートの一般的な原因を特定すると、アラートの原因と解決方法についての詳細情報を使用してアラートを強化できます。 クラウドネイティブアプリケーションの高成熟した展開では、障害が発生したノードをスケールセットから削除する、または自動スケールアクティビティをトリガーするなどの操作を実行する自己復旧タスクを開始することを選択できます。 最終的には、午前2時に勤務先の担当者をウェイクアップして、ライブサイトの問題を解決する必要がなくなる可能性があります。システムは、翌朝に仕事が到着するまで、補償または少なくとも limp を調整できるようにするためです。

Azure Monitor は、自動的に機械学習を活用して、デプロイされたアプリケーションの通常の動作パラメーターを把握します。 これにより、通常のパラメーターの外部で動作しているサービスを検出できます。 たとえば、サイトでの一般的な平日のトラフィックは、1分あたり1万の要求になることがあります。 その後、特定の週に、要求の数が突然、1分あたりの非常に異常な2万要求に達します。 [スマート検出](https://docs.microsoft.com/azure/azure-monitor/app/proactive-diagnostics)では、この偏差が基準からのものであることがわかり、アラートがトリガーされます。 同時に、傾向分析は、トラフィックの負荷が予想されるときに偽陽性が発生しないようにするのに十分なスマートです。

## <a name="references"></a>参照

- [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)
- [スマート Alert Management-MS Ignite-ビデオ](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)

>[!div class="step-by-step"]
>[前へ](monitoring-azure-kubernetes.md)
>[次へ](identity.md)
