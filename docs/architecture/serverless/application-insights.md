---
title: Application Insights - サーバーレス アプリ
description: Application Insights は、開発者が Web アプリ、モバイル アプリ、デスクトップ アプリ、マイクロサービスの問題を検出、トリアージ、診断できるサーバーレス診断プラットフォームです。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 7c1013ac029645a2da44aaf1c3b6ba74ca3f3dde
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72522737"
---
# <a name="telemetry-with-application-insights"></a>Application Insights のテレメトリ

[Application Insights](https://docs.microsoft.com/azure/application-insights) は、開発者が Web アプリ、モバイル アプリ、デスクトップ アプリ、マイクロサービスの問題を検出、トリアージ、診断できるサーバーレス診断プラットフォームです。 ポータルでスイッチを切り換えるだけで、関数アプリに対して Application Insights を有効にすることができます。 Application Insights を使用すると、サーバーを構成したり、独自のデータベースを設定したりする必要なく、これらの機能すべてが提供されます。 Application Insights のすべての機能は、アプリと自動的に統合されるサービスとして提供されます。

![Application Insights のロゴ](./media/application-insights-logo.png)

既存のアプリに Application Insights を追加することは、アプリケーションの設定にインストルメンテーション キーを追加するのと同じくらい簡単です。 Application Insights を使用して次のことができます。

- 関数呼び出しの回数、関数の実行にかかる時間、例外などのメトリクスに基づいて、カスタム グラフとカスタム アラートを作成する
- エラーとサーバーの例外を分析する
- 操作別にパフォーマンスを掘り下げ、サードパーティの依存関係の呼び出しにかかる時間を測定する
- 関数アプリをホストするすべてのサーバーの CPU 使用率、メモリ、速度を監視する
- 関数アプリの要求数や待機時間といったメトリクスのライブ ストリームを表示する
- [Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics) を使用して関数データの検索、クエリ、カスタム グラフの作成を行う

![メトリックス エクスプローラー](./media/metrics-explorer.png)

組み込みのテレメトリに加えて、カスタム テレメトリを生成することもできます。 次のコード スニペットは、関数アプリ用に設定したインストルメンテーション キーを使用して、カスタム テレメトリ クライアントを作成します。

```csharp
public static TelemetryClient telemetry = new TelemetryClient()
{
    InstrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY")
};
```

次のコードは、[Azure Table Storage](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview) インスタンスに新しい行を挿入するのにかかる時間を測定します。

```csharp
var operation = TableOperation.Insert(entry);
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
await table.ExecuteAsync(operation);
telemetry.TrackDependency("AzureTableStorageInsert", "Insert", startTime, timer.Elapsed, true);
```

結果のパフォーマンス グラフは次のようになります。

![カスタムのテレメトリ](./media/custom-telemetry.png)

カスタム テレメトリは、新しい行の挿入にかかる平均時間が 32.6 ミリ秒であることを示しています。

Application Insights には、サーバーレス アプリケーションに関する詳細なテレメトリをログに記録するための強力で便利な方法が用意されています。 提供されるトレースとログ記録のレベルを完全に制御できます。 イベント、依存関係、ページ ビューなどのカスタム統計情報を追跡できます。 最後に、高機能の分析によって、重要な質問をしてグラフや高度な分析情報を生成するクエリを作成できます。

詳しくは、「[Azure Functions を監視する](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)」をご覧ください。

>[!div class="step-by-step"]
>[前へ](azure-functions.md)
>[次へ](logic-apps.md)
