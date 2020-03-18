---
title: ワークフローの追跡とトレース
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], tracking and tracing
ms.assetid: b965ded6-370a-483d-8790-f794f65b137e
ms.openlocfilehash: 9f887babec5c070eed2fb3c7e4d8d35cdfb3f488
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837481"
---
# <a name="workflow-tracking-and-tracing"></a>ワークフローの追跡とトレース
Windows ワークフロー追跡は、ワークフローの実行を確認できるように設計された [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] の機能です。 また、ワークフロー インスタンスの実行を追跡できる追跡インフラストラクチャが用意されています。 WF 追跡インフラストラクチャはワークフローを透過的にインストルメント化し、実行中の主要イベントを反映してレコードを出力します。 この機能はすべての [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] ワークフローから既定で使用できます。 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] ワークフローで追跡を実行するために変更は必要ありません。 必要なのは、受信する追跡データの量を決定することのみです。 ワークフロー インスタンスが開始または完了すると、追跡レコードの処理が出力されます。 また、追跡によって、ワークフロー変数に関連付けられたビジネス関連データを抽出することもできます。 たとえば、ワークフローが注文処理システムを表している場合、注文 ID と共に <xref:System.Activities.Tracking.TrackingRecord> オブジェクトを抽出できます。 一般的に、WF 追跡機能を有効にすると、ワークフローの実行から診断またはビジネス分析データにアクセスしやすくなります。  
  
 これらの追跡コンポーネントは、WinFX の追跡サービスに相当します。 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] では、WF 追跡機能のパフォーマンスが改善され、プログラミング モデルが簡略化されました。 追跡ランタイムがワークフロー インスタンスをインストルメント化し、ワークフローのライフ サイクル、ワークフロー アクティビティ、およびカスタム イベント関連のイベントを出力します。  
  
 Windows Server App Fabric は、WCF とワーク フロー サービスの実行を監視することもできます。 詳細については、「windows server [AppFabric を使用した](https://docs.microsoft.com/previous-versions/appfabric/ee677276(v=azure.10)) [windows Server アプリケーションの監視](https://docs.microsoft.com/previous-versions/appfabric/ee677251(v=azure.10))と監視」を参照してください。  
  
 ワークフロー ランタイムの問題を解決するには、診断ワークフロー追跡を有効にします。 詳細については、「[ワークフロートレース](workflow-tracing.md)」を参照してください。  
  
 プログラミング モデルを理解するには、追跡インフラストラクチャの主要コンポーネントに関するトピックの説明を参照してください。  
  
- <xref:System.Activities.Tracking.TrackingRecord> オブジェクトは、ワークフロー ランタイムから取得します。 詳細については、「[追跡レコード](tracking-records.md)」を参照してください。  
  
- <xref:System.Activities.Tracking.TrackingParticipant> オブジェクトは <xref:System.Activities.Tracking.TrackingRecord> オブジェクトに定期受信します。 追跡参加要素には、<xref:System.Activities.Tracking.TrackingRecord> オブジェクトのペイロードを処理するロジックがありません (たとえば、ファイルに書き込むことができるなど)。 詳細については、「[追跡参加要素](tracking-participants.md)」を参照してください。  
  
- <xref:System.Activities.Tracking.TrackingProfile> オブジェクトは、ワークフロー インスタンスから出力された追跡レコードをフィルター処理します。 詳細については、「[追跡プロファイル](tracking-profiles.md)」を参照してください。  
  
## <a name="workflow-tracking-infrastructure"></a>ワークフロー追跡のインフラストラクチャ  
 ワークフロー追跡インフラストラクチャは、パブリッシュおよび定期受信のパラダイムに従っています。 ワークフロー インスタンスは追跡レコードのパブリッシャーですが、追跡レコードのサブスクライバーはワークフローに対する拡張として登録されます。 <xref:System.Activities.Tracking.TrackingRecord> オブジェクトに定期受信するこれらの拡張は、追跡参加要素と呼ばれます。 追跡参加要素は、<xref:System.Activities.Tracking.TrackingRecord> オブジェクトにアクセスし、そのための書き込み方法にかかわらず処理する機能拡張ポイントです。 追跡インフラストラクチャを使用すると、送信の追跡レコードにフィルターを適用して、参加要素からレコードのサブセットに定期受信できるようになります。 このフィルター機構は追跡プロファイル ファイルによって実現します。  
  
 追跡インフラストラクチャの概要を次の図に示します。  
  
 ![ワークフロー追跡インフラストラクチャを示すスクリーンショット。](./media/workflow-tracking-and-tracing/workflow-tracking-infrastructure.gif "WV")  
  
## <a name="in-this-section"></a>このセクションの内容  
 [追跡レコード](tracking-records.md)  
 ワークフロー ランタイムが出力する追跡レコードについて説明します。  
  
 [追跡プロファイル](tracking-profiles.md)  
 追跡プロファイルを使用する方法について説明します。  
  
 [追跡参加要素](tracking-participants.md)  
 システム標準の追跡参加要素を使用する方法またはカスタムの追跡参加要素を作成する方法について説明します。  
  
 [ワークフローの追跡の構成](configuring-tracking-for-a-workflow.md)  
 ワークフローの追跡を構成する方法について説明します。  
  
 [ワークフロー トレース](workflow-tracing.md)  
 ワークフローのデバッグ追跡を有効にする 2 つの方法について説明します。  
  
## <a name="see-also"></a>参照

- [SQL 追跡](./samples/sql-tracking.md)
