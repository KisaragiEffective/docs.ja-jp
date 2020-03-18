---
title: 移行のガイドライン
ms.date: 03/30/2017
ms.assetid: cb65c132-58c9-4028-b3d4-1efc71d5e60e
ms.openlocfilehash: f0c21d32b745a51bada9133230dd0c87be9c915e
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937963"
---
# <a name="migration-guidance"></a>移行のガイドライン

.NET Framework 4 では、Microsoft は、Windows Workflow Foundation (WF) の2番目のメジャーバージョンをリリースしています。 [!INCLUDE[wf1](../../../includes/wf1-md.md)] は、WinFX でリリースされました (これには、WF3 と呼ばれる\* 名前空間の型が含まれていました) と .NET Framework 3.5 で強化されました。 WF3 も .NET Framework 4 に含まれていますが、新しいワークフローテクノロジ (WF4 名前空間の\* 型、と呼ばれます) と共に存在します。 WF4 の導入時期を検討する場合は、最初にそのタイミングの管理を認識することが重要です。  
  
- WF3 は .NET Framework 4 の完全にサポートされている部分です。  
  
- WF3 アプリケーションは、変更せずに .NET Framework 4 で実行され、引き続き完全にサポートされます。  
  
- 新しい WF3 アプリケーションを作成し、既存のアプリケーションを Visual Studio 2012 で編集し、完全にサポートすることができます。  
  
 したがって、.NET Framework 4 を採用するかどうかの決定は、WF3\*(\*WF4) から () に移動するかどうかの決定とは分離されています。 このトピックでは、WF の移行のガイドラインへのリンクを提供し、WF3 および WF4 での作業に関する情報を提供します。  
  
## <a name="wf-migration-white-papers-and-cookbooks"></a>WF の移行に関するホワイトペーパーと料理

 [WF の移行の概要](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))に関するトピックでは、WF3 と WF4 の間の関係と移行戦略の概要について説明します。 関連トピックでは、特定のトピックを掘り下げて説明します。  
  
 [WF の移行の概要](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 と WF4 の関係、および .NET 4 のワークフロー テクノロジのユーザーまたは潜在的なユーザーとして使用できる選択肢について説明します。  
  
 [WF の移行: WF3 開発のベストプラクティス](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 より簡単に WF4 に移行できるように、WF3 の成果物を設計する方法について説明します。  
  
 [WF のガイダンス: ルール](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 .NET Framework 4 つのソリューションにルール関連の投資を進める方法について説明します。  
  
 [WF のガイダンス: ステートマシン](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 ステート マシンのアクティビティがない場合の WF4 の制御フロー モデリングについて説明します。  
  
 このガイダンスは、.NET Framework 4 を対象とするワークフロー プロジェクトにのみ該当することに注意してください。 ステート マシンのワークフローは、Platform Update 1 のリリースで .NET 4.0.1 に追加され、.NET Framework 4.5 の一部として含まれていました。 .NET 4.0.1-4.0.3 および .NET Framework 4.5 でのステートマシンワークフローの詳細については、「 [Update 4.0.1 for Microsoft .NET Framework 4 Features](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hh290669(v=vs.100)) And [state machine ワークフロー](state-machine-workflows.md)」を参照してください。  
  
 [WF の移行のクックブック: カスタムアクティビティ](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 のカスタム アクティビティを WF4 で再設計する場合のサンプルと手順について説明します。  
  
 [WF の移行のクックブック: 高度なカスタムアクティビティ](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 キューを使用して子アクティビティを WF4 カスタム アクティビティとしてスケジュールする高度な WF3 カスタム アクティビティを再設計するための指針を示します。  
  
 [WF の移行のクックブック: ワークフロー](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 のワークフローを WF4 で再設計する場合のサンプルと手順について説明します。  
  
 [WF の移行のクックブック: ワークフローホスティング](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 ホスティング コードを WF4 ホスティング コードとして再設計するための指針を示します。 この目的は、WF3 と WF4 のワークフロー ホスティングの主な違いを説明することです。  
  
 [WF の移行のクックブック: ワークフロー追跡](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 WF3 追跡コードを再設計するための指針と、同等の WF4 追跡コードと構成を使用した構成を示します。  
  
 [WF のガイダンス: ワークフローサービス](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))  
 事前定義アクティビティの一般的なシナリオ向けに、WF3 で作成した Windows Communication Foundation (WCF) Web サービス (一般にワークフロー サービスと呼ばれます) を実装するワークフローを WF4 を使用するように再設計するための詳細な手順を例を中心として示します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Activities.Statements.Interop>
