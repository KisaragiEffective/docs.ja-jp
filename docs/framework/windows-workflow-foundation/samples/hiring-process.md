---
title: 雇用プロセス
ms.date: 03/30/2017
ms.assetid: d5fcacbb-c884-4b37-a5d6-02b1b8eec7b4
ms.openlocfilehash: 02968acfc762550c9010dd0ed29acbca845e08bb
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715979"
---
# <a name="hiring-process"></a>雇用プロセス
このサンプルでは、メッセージング アクティビティ、およびワークフロー サービスとしてホストされる 2 つのワークフローを使用して、ビジネス プロセスを実装する方法を示します。 この 2 つのワークフローは、Contoso, Inc という架空の会社の IT インフラストラクチャの一部です。  
  
 `HiringRequest` ワークフロー プロセス (<xref:System.Activities.Statements.Flowchart> として実装) は、組織の複数のマネージャーの承認が必要です。 この目標を達成するために、ワークフローは組織内の他の既存のサービス (ここでは、受信トレイサービスと、plain Windows Communication Foundation (WCF) サービスとして実装された組織のデータサービス) を使用します。  
  
 `ResumeRequest` ワークフロー (<xref:System.Activities.Statements.Sequence> として実装) は、Contoso 社の外部 Careers Web サイトに求人情報を発行し、履歴書の受け取りを管理します。 求人情報は、指定した期間 (タイムアウトになるまで) または Contoso の従業員が削除を決定するまで、外部 Web サイトに掲載されます。  
  
 このサンプルでは、次の [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] の機能について説明します。  
  
- ビジネス プロセスをモデル化する <xref:System.Activities.Statements.Flowchart> および <xref:System.Activities.Statements.Sequence> ワークフロー。  
  
- ワークフロー サービス  
  
- メッセージング アクティビティ。  
  
- コンテンツ ベースの相関関係。  
  
- カスタム アクティビティ (宣言型およびコードベース)。  
  
- システムで指定された SQL サーバー永続性。  
  
- カスタム <xref:System.Activities.Persistence.PersistenceParticipant>。  
  
- カスタム追跡  
  
- Event Tracking for Windows (ETW) 追跡。  
  
- アクティビティの構成。  
  
- <xref:System.Activities.Statements.Parallel> アクティビティ。  
  
- <xref:System.Activities.Statements.CancellationScope> アクティビティ。  
  
- 永続的なタイマー (<xref:System.Activities.Statements.Delay> アクティビティ)。  
  
- トランザクション。  
  
- 同一ソリューションの複数のワークフロー。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Application\HiringProcess`  
  
## <a name="description-of-the-process"></a>プロセスの説明  
 Contoso 社は、各部門の人数を綿密に管理しようとしています。 このため、従業員が新しい雇用プロセスを開始する場合、求人を実際に開始する前に雇用要件プロセスの承認を受ける必要があります。 このプロセスは、雇用プロセス要求 (HiringRequestService プロジェクトで定義) と呼ばれ、次の手順で構成されます。  
  
1. 従業員 (要求者) は雇用プロセス要求を開始します。  
  
2. 要求者のマネージャーは、要求を承認する必要があります。  
  
    1. マネージャーは要求を拒否できます。  
  
    2. マネージャーは、追加情報が必要な場合、要求者に要求を戻すことができます。  
  
        1. 要求者は要求をレビューし、マネージャーに再承認を求めることができます。  
  
    3. マネージャーはこれを承認します。  
  
3. 要求者のマネージャーの承認後は、その部門の責任者が要求を承認する必要があります。  
  
    1. 部門の責任者は要求を拒否できます。  
  
    2. 部門の責任者は要求を承認できます。  
  
4. 部門の責任者の承認後は、HR (人事部) の 2名のマネージャーまたは CEO (最高経営責任者) の承認を受ける必要があります。  
  
    1. プロセスは承認状態または拒否状態のいずれかになります。  
  
    2. プロセスが [承諾済み] になると、`ResumeRequest` ワークフローの新しいインスタンスが開始されます (`ResumeRequest` はサービス参照を使用して HiringRequest.csproj にリンクされます)。  
  
 マネージャーが新しい従業員の雇用を承認すると、HR は適切な応募者を採用する必要があります。 このプロセスは、2 番目のワークフロー (`ResumeRequest`。ResumeRequestService.csproj で定義。) で実行されます。 このワークフローによって、Contoso 社の外部 Careers Web サイトに求人情報を送信するプロセスが定義され、応募者から履歴書を受信し、募集の状況が監視されます。 求人は、指定した期間 (有効期間が過ぎるまで) または Contoso の従業員が削除を指定するまで有効です。 `ResumeRequest` ワークフローは、次のステップから構成されています。  
  
1. Contoso 社の従業員は、求人およびタイムアウト期間に関する情報を入力します。 従業員がこの情報を入力すると、Careers Web サイトに求人情報が掲載されます。  
  
2. この情報が発行されると、この情報に興味を持った人が自分の履歴書を送信します。 履歴書は、送信後、求人にリンクされたレコードに保存されます。  
  
3. 応募者は、タイムアウトになるまで、または Contoso 社の HR 部でプロセスを停止して求人を削除することを明示的に決定するまでは履歴書を送信することができます。  
  
## <a name="projects-in-the-sample"></a>サンプルのプロジェクト  
 サンプル ソリューションのプロジェクトを次の表に示します。  
  
|プロジェクト|説明|  
|-------------|-----------------|  
|ContosoHR|データ コントラクト、ビジネス オブジェクト、およびリポジトリ クラスを含んでいます。|  
|HiringRequestService|Hiring Request Process ワークフローの定義を含んでいます。<br /><br /> このプロジェクトは、サービスとしてワークフロー (xaml ファイル) を自己ホストするコンソール アプリケーションとして実装されます。|  
|ResumeRequestService|タイムアウトとなるかプロセス終了が決定するまで応募者からの履歴書を収集するワークフロー サービス。<br /><br /> このプロジェクトは、宣言型ワークフロー サービス (xamlx) として実装されます。|  
|OrgService|組織情報 (Employees、Positions、PositionTypes、および Departments) を公開するサービス。 このサービスは、Enterprise Resource Plan (ERP) の Company Organization モジュールと考えることができます。<br /><br /> このプロジェクトは、Windows Communication Foundation (WCF) サービスを公開するコンソールアプリケーションとして実装されます。|  
|InboxService|従業員のアクション可能なタスクを含んでいる受信トレイです。<br /><br /> このプロジェクトは、WCF サービスを公開するコンソール アプリケーションとして実装されます。|  
|InternalClient|プロセスと対話するための Web アプリケーション。 ユーザーは、HiringProcess ワークフローを開始して、参加し、表示できます。 このアプリケーションを使用すると、ResumeRequest プロセスを開始および監視することもできます。<br /><br /> このサイトは、Contoso のイントラネット内のサイトとして実装されます。 このプロジェクトは ASP.NET Web サイトとして実装されます。|  
|CareersWebSite|Contoso 社で募集中の求人を公開する外部 Web サイト。 興味を持った応募者は、このサイトに移動して、履歴書を送信します。|  
  
## <a name="feature-summary"></a>機能の概要  
 このサンプルでの各機能の使用方法を次の表に示します。  
  
|特性|説明|プロジェクト|  
|-------------|-----------------|-------------|  
|フローチャート|ビジネス プロセスがフローチャートとして表されます。 このフローチャートの説明では、業務時にプロセスがホワイトボードに描画されるのと同じ方法でプロセスが表されます。|HiringRequestService|  
|ワークフロー サービス|プロセスの定義を持つフローチャートは、サービスでホストされます (この例では、サービスはコンソール アプリケーションでホストされます)。|HiringRequestService|  
|メッセージング アクティビティ|フローチャートでは、次の 2 つの方法でメッセージング アクティビティが使用されます。<br /><br /> -ユーザーから情報を取得する場合 (各承認手順で意思決定と関連情報を受け取る場合)。<br />-サービス参照によって使用される、他の既存のサービス (受信トレイサービスおよび OrgDataService) と対話する場合。|HiringRequestService|  
|コンテンツ ベースの相関関係|雇用要求の ID プロパティにおける承認メッセージ相関関係は次のとおりです。<br /><br /> -プロセスが開始されると、関連付けハンドルは要求の ID で初期化されます。<br />-受信承認メッセージはその ID に関連付けられます (各承認メッセージの最初のパラメーターは要求の ID です)。|HiringRequestService / ResumeRequestService|  
|カスタム アクティビティ (宣言型およびコード ベース)|このサンプルには、次の複数のカスタム アクティビティがあります。<br /><br /> -   `SaveActionTracking`: このアクティビティは、(<xref:System.Activities.NativeActivityContext.Track%2A>を使用して) カスタム <xref:System.Activities.Tracking.TrackingRecord> を出力します。 このアクティビティは、<xref:System.Activities.NativeActivity> を拡張する命令型コードを使用して作成されています。<br />-   `GetEmployeesByPositionTypes`: このアクティビティは、位置の種類の Id のリストを受け取り、Contoso にその位置を持つ people の一覧を返します。 このアクティビティは、宣言的に作成されています (アクティビティ デザイナーを使用)。<br />-   `SaveHiringRequestInfo`: このアクティビティでは、(`HiringRequestRepository.Save`を使用して) `HiringRequest` の情報が保存されます。 このアクティビティは、<xref:System.Activities.CodeActivity> を拡張する命令型コードを使用して作成されています。|HiringRequestService|  
|システムで指定された SQL サーバー永続性|Flowchart プロセス定義をホストする<xref:System.ServiceModel.Activities.WorkflowServiceHost> インスタンスは、システムで指定された SQL サーバー永続性を使用するように構成されています。|HiringRequestService / ResumeRequestService|  
|カスタム追跡|このサンプルには、`HiringRequestProcess` の履歴を保存するカスタム追跡参加要素が含まれています (これによって、アクションの内容、実行者、および時期が記録されます)。 ソース コードは、HiringRequestService の Tracking フォルダーにあります。|HiringRequestService|  
|ETW 追跡|システムで指定された ETW 追跡は、HiringRequestService サービスの App.config ファイルで設定されます。|HiringRequestService|  
|アクティビティの構成|プロセス定義では、<xref:System.Activities.Activity> のフリー コンポジションが使用されます。 Flowchart には、その他のアクティビティなどを同時に含んでいる複数の Sequence アクティビティと Parallel アクティビティが含まれています。|HiringRequestService|  
|並行活動|-   <xref:System.Activities.Statements.ParallelForEach%601> は、CEO および人事管理者の受信トレイに並列で登録するために使用されます (2 人の HR マネージャーの承認手順を待機しています)。<br />-   <xref:System.Activities.Statements.Parallel> は、完了して却下された手順でクリーンアップタスクを実行するために使用されます。|HiringRequestService|  
|モデルの取り消し|フローチャートでは、<xref:System.Activities.Statements.CancellationScope> を使用して、取り消し動作を作成します (この場合、一部のクリーンアップが実行されます)。|HiringRequestService|  
|カスタマー永続参加要素|`HiringRequestPersistenceParticipant` は、ワークフロー変数のデータを Contoso HR データベースに保存されているテーブルに保存します。|HiringRequestService|  
|ワークフロー サービス|`ResumeRequestService` は、ワークフロー サービスを使用して実装されます。 ワークフロー定義およびサービス情報は、ResumeRequestService.xamlx に含まれています。 サービスは、永続性と追跡を使用するように構成されます。|ResumeRequestService|  
|持続的なタイマー|`ResumeRequestService` は、永続的なタイマーを使用して、求人の期間を定義します (タイムアウトになると、求人は終了します)。|ResumeRequestService|  
|トランザクション|<xref:System.Activities.Statements.TransactionScope> は、複数のアクティビティの実行時にデータの一貫性を保つために使用されます (新しい履歴書の受信時)。|ResumeRequestService|  
|トランザクション|カスタムの永続参加要素 (`HiringRequestPersistenceParticipant`) とカスタムの追跡参加要素 (`HistoryFileTrackingParticipant`) では、同じトランザクションを使用します。|HiringRequestService|  
|ASP.NET アプリケーションでの [!INCLUDE[wf1](../../../../includes/wf1-md.md)] の使用。|ワークフローは、2つの ASP.NET アプリケーションからアクセスされます。|InternalClient / CareersWebSite|  
  
## <a name="data-storage"></a>データ ストレージ  
 データは、`ContosoHR` という名前の SQL Server データベースに保存されます (このデータベースを作成するためのスクリプトは `DbSetup` フォルダーにあります)。 ワークフロー インスタンスは `InstanceStore` という名前の SQL Server データベースに保存されます (インスタンス ストアを作成するためのスクリプトは [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] の配布に含まれています)。  
  
 両方のデータベースは、Visual Studio の開発者コマンドプロンプトからセットアップを実行して作成されます。  
  
## <a name="running-the-sample"></a>サンプルの実行  
  
#### <a name="to-create-the-databases"></a>データベースを作成するには  
  
1. Visual Studio の開発者コマンドプロンプトを開きます。  
  
2. サンプル フォルダーに移動します。  
  
3. Setup.cmd を実行します。  
  
4. 2 つのデータベース (`ContosoHR` と `InstanceStore`) が SQL Express に作成されたことを確認します。  
  
#### <a name="to-set-up-the-solution-for-execution"></a>ソリューションの実行を設定するには  
  
1. Visual Studio を管理者として実行します。 HiringRequest.sln を開きます。  
  
2. **ソリューションエクスプローラー**でソリューションを右クリックし、 **[プロパティ]** を選択します。  
  
3. **[マルチスタートアッププロジェクト]** オプションを選択し、 **[careerswebsite]** 、 **Internalclient**、 **HiringRequestService**、 **ResumeRequestService**を**開始**するように設定します。 **ContosoHR**、受信**トレイサービス**、および**Orgservice**は、"なし" のままにします。  
  
4. Ctrl キーと Shift キーを押しながら B キーを押して、ソリューションをビルドします。 ビルドが成功したことを確認します。  
  
#### <a name="to-run-the-solution"></a>ソリューションを実行するには  
  
1. デバッグを行わない場合は、ソリューションのビルド後、Ctrl キーを押しながら F5 キーを押してソリューションを実行します。 すべてのサービスが開始されたことを確認します。  
  
2. ソリューションで **[Internalclient]** を右クリックし、 **[ブラウザーで表示]** を選択します。 `InternalClient` の既定のページが表示されます。 サービスが実行中であることを確認し、リンクをクリックします。  
  
3. **HiringRequest**モジュールが表示されます。 詳細については、以下のシナリオを参照してください。  
  
4. `HiringRequest` が完了すると、`ResumeRequest` を開始できます。 詳細については、以下のシナリオを参照してください。  
  
5. `ResumeRequest` は、投稿されると、パブリック Web サイト (Contoso Careers Web サイト) で使用可能になります。 求人を確認するには (さらに、応募するには)、Careers Web サイトに移動します。  
  
6. ソリューションの **[careerswebsite]** を右クリックし、 **[ブラウザーで表示]** を選択します。  
  
7. ソリューションで **[Internalclient]** を右クリックし、 **[ブラウザーで表示]** を選択して `InternalClient` に戻ります。  
  
8. 受信トレイ トップメニューの **ジョブの投稿** リンクをクリックして、**jobpostings** セクションにアクセスします。 詳細については、以下のシナリオを参照してください。  
  
## <a name="scenarios"></a>監視プロセス  
  
### <a name="hiring-request"></a>雇用要求  
  
1. Michael Alexander (ソフトウェア エンジニア) は、エンジニアリング部において、C# の分野で 3 年以上の実務経験を持つ SDET (Software Engineer in Test) を採用するための新たな求人を要求します。  
  
2. 作成されると、要求は Michael の受信トレイに表示されます (要求が表示されない場合は **[更新]** をクリックします)。これは、michael のマネージャーである Peter Brehm の承認待ちです。  
  
3. Peter は、Michael の要求に対応します。 Peter は、この職種で要求される C# の実務経験は 3 年ではなく 5 年であると考え、その旨を知らせるコメントを送信します。  
  
4. Michael は上司からメッセージを受信トレイに表示し、操作を希望しています。Michael は、位置要求の履歴を確認し、Peter に同意します。 Michael は、求人要求の C# の実務経験に関する説明を 5 年に変更し、その変更を受け入れます。  
  
5. Peter は、Michael の変更済みの要求を確認し、これを受け入れます。 ここで、要求は、エンジニアリング部長 Tsvi Reiter の承認が必要になります。  
  
6. Tsvi Reiter は、要求に多くの時間をかけることができないため、要求が緊急であることと、この要求を受け入れることをコメントします。  
  
7. ここで、要求は、2 人の HR マネージャーまたは CEO の承認が必要になります。 CEO である Brian Richard Goldstein は Tsvi による緊急要求を確認します。 Brian Richard Goldstein は、これを受け入れ、2 人の HR マネージャーによる承認を省略して、要求に対応します。  
  
8. 要求は Michael の受信トレイから削除され、SDET を雇用するためのプロセスが開始されます。  
  
### <a name="start-resume-request"></a>履歴書の要求の開始  
  
1. これで、ジョブの位置は、ユーザーが適用できる外部 Web サイトへの投稿を待機しています ( **[ジョブの投稿]** リンクをクリックすると表示されます)。 現在、この求人は、求人を決定して掲載する HR 担当者の最終判断を待っている状態です。  
  
2. HR は、( **[編集]** リンクをクリックして) このジョブ60の位置を編集したいと考えています (実際には、数日または数週間)。 タイムアウトによって、求人は指定時間に従い外部 Web サイトに掲載されます。  
  
3. 編集したジョブの位置を保存すると、[**再開**しています] タブに表示されます (新しいジョブの位置を確認するには、Web ページを更新してください)。  
  
### <a name="collecting-resumes"></a>履歴書の収集  
  
1. 求人は外部の Web サイトに提示されています。 求人の応募に興味がある人は、この求人に応募するために履歴書を送信することができます。  
  
2. ジョブの投稿リストサービスに戻ると、これまでに収集された "表示の再開" を行うことができます。  
  
3. また、HR は履歴書の募集を停止することもできます (適切な応募者が見つかった場合など)。  
  
## <a name="troubleshooting"></a>トラブルシューティング  
  
1. 管理者特権で Visual Studio を実行していることを確認します。  
  
2. ソリューションをビルドできない場合は、次の項目を確認してください。  
  
    - `ContosoHR` への参照が、`InternalClient` プロジェクトまたは `CareersWebSite` プロジェクトにありません。  
  
3. ソフトウェアを実行できない場合は、次の項目を確認してください。  
  
    1. すべてのサービスが実行中である。  
  
    2. サービス参照が更新されている。  
  
        1. App_WebReferences フォルダーを開きます。  
  
        2. **[Contoso]** を右クリックし、 **[Web/サービス参照の更新]** を選択します。  
  
        3. Visual Studio で CTRL + SHIFT + B キーを押して、ソリューションをリビルドします。  
  
## <a name="uninstalling"></a>のアンインストール  
  
1. DbSetup フォルダーにある Cleanup.bat を実行して SQL Server インスタンス ストアを削除します。  
  
2. ハード ドライブからソース コードを削除します。
