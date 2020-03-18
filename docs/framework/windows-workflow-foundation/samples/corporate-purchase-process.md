---
title: 企業の購買プロセス
ms.date: 03/30/2017
ms.assetid: a5e57336-4290-41ea-936d-435593d97055
ms.openlocfilehash: 95fa421ed44cf2d930fb4b80979d1b8bd9fda5ed
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715209"
---
# <a name="corporate-purchase-process"></a>企業の購買プロセス
このサンプルは、Request for Proposals (RFP: 提案依頼書) に基づくごく基本的な購買プロセスを作成する方法を示しています。この購買プロセスでは最良の提案が自動的に選択されます。 このサンプルでは、<xref:System.Activities.Statements.Parallel>、<xref:System.Activities.Statements.ParallelForEach%601>、および <xref:System.Activities.Statements.ForEach%601> と、プロセスを表すワークフローを作成するカスタム アクティビティが組み合わされています。

 このサンプルには、(元の要求元または特定のベンダーとして) 別の参加者としてプロセスと対話できる ASP.NET クライアントアプリケーションが含まれています。

## <a name="requirements"></a>要件

- Visual Studio 2012。

- [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)].

## <a name="demonstrates"></a>例

- カスタム アクティビティ。

- アクティビティの構成。

- ブックマーク。

- 永続性。

- スキーマ化された永続化。

- トレース。

- [追跡] :

- さまざまなクライアント (ASP.NET Web applications と WinForms applications) で [!INCLUDE[wf1](../../../../includes/wf1-md.md)] をホストする。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Application\PurchaseProcess`  
  
## <a name="description-of-the-process"></a>プロセスの説明  
 このサンプルでは、汎用会社の仕入先からの提案を収集するための Windows Workflow Foundation (WF) プログラムの実装を示します。  
  
1. Company X の従業員が Request for Proposal (RFP) を作成します。  
  
    1. RFP のタイトルと説明を入力します。  
  
    2. 提案の送信を依頼するベンダーを選択します。  
  
2. 従業員が提案を送信します。  
  
    1. ワークフローのインスタンスが作成されます。  
  
    2. ワークフローは、すべてのベンダーから提案が送信されるまで待機します。  
  
3. すべての提案が受信されると、それらがワークフローによって反復処理されて、最良の提案が選択されます。  
  
    1. ベンダーにはそれぞれ評価があります (評価リストは VendorRepository.cs に格納されています)。  
  
    2. 提案の合計金額は、(ベンダーによって入力された金額) * (記録されているベンダーの評価) / 100 という式によって決定されます。  
  
4. 元の要求者は、送信されたすべての提案を表示できます。 最良の提案はレポートの特別なセクションに表示されます。  
  
## <a name="process-definition"></a>プロセスの定義  
 このサンプルのコア ロジックでは <xref:System.Activities.Statements.ParallelForEach%601> アクティビティが使用されています。このアクティビティは、各ベンダーからの提案を待機して (ブックマークを作成するカスタム アクティビティを使用)、ベンダーの提案を RFP として登録します (<xref:System.Activities.Statements.InvokeMethod> アクティビティを使用)。  
  
 その後、`RfpRepository` に格納されている受信したすべての提案が反復処理されて、調整金額が計算されます (<xref:System.Activities.Statements.Assign> アクティビティと <xref:System.Activities.Expressions> アクティビティを使用)。調整金額が前の最良の提案より優れている場合は、その新しい金額が最良の提案に割り当てられます (<xref:System.Activities.Statements.If> アクティビティと <xref:System.Activities.Statements.Assign> アクティビティを使用)。  
  
## <a name="projects-in-this-sample"></a>このサンプルのプロジェクト  
 このサンプルには次のプロジェクトが含まれています。  
  
|プロジェクト|説明|  
|-------------|-----------------|  
|共通|プロセス内で使用されるエンティティ オブジェクト (Request for Proposal、Vendor、および Vendor Proposal)。|  
|WfDefinition|購買プロセス ワークフローのインスタンスの作成および使用のためにクライアント アプリケーションによって使用されるプロセス ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] プログラムとしてのプロセス) とホスト (`PurchaseProcessHost`) の定義。|  
|WebClient|購入プロセスのインスタンスの作成と参加をユーザーに許可する ASP.NET クライアントアプリケーション。 独自に作成したホストを使用してワークフロー エンジンとやり取りします。|  
|WinFormsClient|購買プロセスのインスタンスを作成したりそれに参加したりできる Windows フォーム クライアント アプリケーション。 独自に作成したホストを使用してワークフロー エンジンとやり取りします。|  
  
### <a name="wfdefinition"></a>WfDefinition  
 次の表には、WfDefinition プロジェクトの最も重要なファイルの説明が含まれています。  
  
|File|説明|  
|----------|-----------------|  
|IPurchaseProcessHost.cs|ワークフローのホストのインターフェイス。|  
|PurchaseProcessHost.cs|ワークフローのホストの実装。 ホストは、ワークフロー ランタイムの詳細を抽象化します。`PurchaseProcess` ワークフローのインスタンスの読み込み、実行、およびインスタンスとのやり取りのために、すべてのクライアント アプリケーションで使用されます。|  
|PurchaseProcessWorkflow.cs|Purchase Process ワークフローの定義を含むアクティビティ (<xref:System.Activities.Activity> から派生します)。<br /><br /> <xref:System.Activities.Activity> から派生するアクティビティは、既存のカスタム アクティビティと [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] アクティビティ ライブラリのアクティビティをまとめることで機能を構成します。 これらのアクティビティをまとめることは、カスタム機能を作成するための最も基本的な方法です。|  
|WaitForVendorProposal.cs|このカスタム アクティビティは <xref:System.Activities.NativeActivity> から派生し、後にベンダーが提案を送信するときに再開する必要がある名前付きブックマークを作成します。<br /><br /> <xref:System.Activities.NativeActivity> から派生するアクティビティは、<xref:System.Activities.CodeActivity> から派生するアクティビティと同様に、<xref:System.Activities.NativeActivity.Execute%2A> をオーバーライドすることで、命令型機能を作成します。ただし、<xref:System.Activities.ActivityContext> メソッドに渡される `Execute` を介して、ワークフロー ランタイムのすべての機能にもアクセスできます。 このコンテキストは、子アクティビティのスケジュールと取り消し、非永続化ゾーン (アトミック トランザクションの中など、ランタイムによってワークフローのデータが永続化されない実行ブロック) の設定、および <xref:System.Activities.Bookmark> オブジェクト (一時停止したワークフローの再開を処理) をサポートします。|  
|TrackingParticipant.cs|すべての追跡イベントを受信してテキスト ファイルに保存する <xref:System.Activities.Tracking.TrackingParticipant>。<br /><br /> 追跡参加要素は拡張としてワークフロー インスタンスに追加されます。|  
|XmlWorkflowInstanceStore.cs|ワークフロー アプリケーションを XML ファイルに保存するカスタムの <xref:System.Runtime.DurableInstancing.InstanceStore>。|  
|XmlPersistenceParticipant.cs|Request for Proposal のインスタンスを XML ファイルに保存するカスタムの <xref:System.Activities.Persistence.PersistenceParticipant>。|  
|AsyncResult.cs / CompletedAsyncResult.cs|永続化コンポーネントに非同期パターンを実装するためのヘルパー クラス。|  
  
### <a name="common"></a>共通  
 次の表には、Common プロジェクトの最も重要なクラスの説明が含まれています。  
  
|&lt;クラス&gt; のすべてのオブジェクト|説明|  
|-----------|-----------------|  
|ベンダー|Request for Proposals で提案を送信するベンダー。|  
|RequestForProposal|Request for Proposals (RFP) は、ベンダーに特定の商品またはサービスについての提案の送信を求める依頼書です。|  
|VendorProposal|ベンダーによって具象 RFP に送信された提案。|  
|VendorRepository|Vendor のリポジトリ。 この実装には、Vendor のインスタンスのメモリ内コレクションと、それらのインスタンスを公開するためのメソッドが含まれています。|  
|RfpRepository|Request for Proposals のリポジトリ。 この実装は、Linq to XML を使用して、スキーマ化された永続化によって生成された Request for Proposal の XML ファイルをクエリします。 |  
|IOHelper|このクラスは、I/O 関連のすべての問題を処理します (フォルダー、パスなど)。|  
  
### <a name="web-client"></a>Web クライアント  
 次の表には、Web Client プロジェクトの最も重要な Web ページの説明が含まれています。  
  
|File|説明|  
|-|-|  
|CreateRfp.aspx|新しい Request for Proposals を作成して送信します。|  
|Default.aspx|アクティブな Request for Proposals と完了した Request for Proposals をすべて表示します。|  
|GetVendorProposal.aspx|具象 Request for Proposals 内のベンダーからの提案を取得します。 このページを使用するのはベンダーだけです。|  
|ShowRfp.aspx|Request for Proposals に関するすべての情報 (受信した提案、日付、金額、およびその他の情報) を表示します。 このページを使用するのは Request for Proposal の作成者だけです。|  
  
### <a name="winforms-client"></a>WinForms Client  
 次の表には、WinForms Client プロジェクトの最も重要なフォームの説明が含まれています。  
  
|フォーム|説明|  
|-|-|  
|NewRfp|新しい Request for Proposals を作成して送信します。|  
|ShowProposals|アクティブな Request for Proposals と完了した Request for Proposals をすべて表示します。 **注:** 提案の要求を作成または変更した後で、その画面の変更を表示するには、UI の **[更新]** ボタンをクリックする必要があります。|  
|SubmitProposal|具象 Request for Proposals 内のベンダーからの提案を取得します。 このウィンドウを使用するのはベンダーだけです。|  
|ViewRfp|Request for Proposals に関するすべての情報 (受信した提案、日付、金額、およびその他の情報) を表示します。 このウィンドウを使用するのは Request for Proposals の作成者だけです。|  
  
### <a name="persistence-files"></a>永続化ファイル  
 次の表は、永続化プロバイダー (`XmlPersistenceProvider`) によって生成されるファイルを示しています。これらのファイルは、現在のシステムの一時フォルダーのパスに配置されます (<xref:System.IO.Path.GetTempPath%2A> を使用)。 トレース ファイルは現在の実行パスに作成されます。  
  
|[ファイル名]|説明|Path|  
|-|-|-|  
|rfps.xml|アクティブな Request for Proposals と完了した Request for Proposals をすべて含む XML ファイル。|<xref:System.IO.Path.GetTempPath%2A>|  
|[instanceid]|このファイルには、ワークフロー インスタンスに関するすべての情報が含まれています。<br /><br /> このファイルは、スキーマ化された永続化の実装 (XmlPersistenceProvider の PersistenceParticipant) によって生成されます。|<xref:System.IO.Path.GetTempPath%2A>|  
|[instanceId].tracking|具象インスタンス内で発生したすべてのイベントを含むテキスト ファイル。<br /><br /> このファイルは TrackingParticipant によって生成されます。|<xref:System.IO.Path.GetTempPath%2A>|  
|PurchaseProcess.Tracing.TraceLog.txt|App.config ファイルまたは Web.config ファイルの構成パラメーターに基づいてワークフローによって生成されるトレース ファイル。|現在の実行パス|  
  
#### <a name="to-use-this-sample"></a>このサンプルを使用するには  
  
1. Visual Studio 2010 を使用して、PurchaseProcess ソリューションファイルを開きます。  
  
2. Web クライアントプロジェクトを実行するには、**ソリューションエクスプローラー**を開き、 **web クライアント**プロジェクトを右クリックします。 **[スタートアッププロジェクトに設定]** を選択します。  
  
3. WinForms Client プロジェクトを実行するには、**ソリューションエクスプローラー**を開き、 **WinForms client**プロジェクトを右クリックします。 **[スタートアッププロジェクトに設定]** を選択します。  
  
4. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。  
  
5. ソリューションを実行するには、Ctrl キーを押しながら F5 キーを押します。  
  
### <a name="web-client-options"></a>Web Client のオプション  
  
- **新しい rfp を作成**する: 新しい提案申請書 (rfp) を作成し、購入プロセスワークフローを開始します。  
  
- **更新**: メインウィンドウで、アクティブな Rfp と完了した rfp の一覧を更新します。  
  
- **View**: 既存の RFP の内容を表示します。 ベンダーは自身の提案を送信できます (依頼されている場合。依頼されていない場合は RFP が完了していません)。  
  
- ビューの種類: ユーザーは、アクティブな Rfp グリッドの **[ビューとして表示]** コンボボックスで目的の参加者を選択することで、さまざまな id を使用して RFP にアクセスできます。  
  
### <a name="winforms-client-options"></a>WinForms Client のオプション  
  
- **Rfp の作成**: 新しい提案申請書 (RFP) を作成し、購入プロセスワークフローを開始します。  
  
- **更新**: メインウィンドウで、アクティブな Rfp と完了した rfp の一覧を更新します。  
  
- **[Rfp の表示]** : 既存の rfp の内容を表示します。 ベンダーは自身の提案を送信できます (依頼されている場合。依頼されていない場合は RFP が完了していません)。  
  
- **[接続]** するユーザー: アクティブな rfp のグリッドの **[View As]** コンボボックスで目的の参加者を選択することにより、さまざまな id を使用して RFP にアクセスできます。
