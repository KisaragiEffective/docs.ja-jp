---
title: Windows のイベント トレースへの追跡イベント
ms.date: 03/30/2017
ms.assetid: f812659b-0943-45ff-9430-4defa733182b
ms.openlocfilehash: 2c397bcfa809a1306e9c31bf3f652b055d997f38
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094580"
---
# <a name="tracking-events-into-event-tracing-in-windows"></a>Windows のイベント トレースへの追跡イベント

このサンプルでは、ワークフローサービスで Windows Workflow Foundation (WF) 追跡を有効にし、Windows イベントトレーシング (ETW) で追跡イベントを出力する方法を示します。 ワークフロー追跡レコードを ETW に出力するために、このサンプルでは ETW 追跡参加要素 (<xref:System.Activities.Tracking.EtwTrackingParticipant>) を使用します。

このサンプルのワークフローでは、要求を受け取り、入力データの逆数を入力変数に割り当てて、クライアントに逆数を返します。 入力データが 0 の場合、処理されない 0 による除算の例外が発生し、ワークフローが中止されます。 追跡を有効にすると、エラー追跡レコードが ETW に出力され、後でエラーをトラブルシューティングする際に役立ちます。 ETW 追跡参加要素は、追跡レコードを定期受信するように追跡プロファイルで構成されています。 追跡プロファイルは、Web.config ファイルで定義され、構成パラメーターとして ETW 追跡参加要素に渡されます。 ETW 追跡参加要素は、ワークフロー サービスの Web.config ファイルで構成され、サービス動作としてサービスに適用されます。 このサンプルでは、イベント ログの追跡イベントをイベント ビューアーを使用して確認します。

## <a name="workflow-tracking-details"></a>ワークフロー追跡の詳細

Windows Workflow Foundation は、ワークフローインスタンスの実行を追跡するための追跡インフラストラクチャを提供します。 追跡ランタイムは、ワークフロー ライフサイクルに関連するイベント、ワークフロー アクティビティのイベント、およびカスタム イベントを出力するワークフロー インスタンスを作成します。 次の表で、追跡インフラストラクチャの主要コンポーネントの詳細を説明します。

|コンポーネント|[説明]|
|---------------|-----------------|
|追跡ランタイム|追跡レコードを出力するためのインフラストラクチャを提供します。|
|追跡参加要素|追跡レコードにアクセスします。 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] には、追跡レコードを Event Tracing for Windows (ETW) イベントとして書き込む追跡参加要素が用意されています。|
|追跡プロファイル|ワークフロー インスタンスから出力された追跡レコードのサブセットを追跡参加要素から定期受信するためのフィルター機構。|

次の表で、ワークフロー ランタイムが出力する追跡レコードの詳細を説明します。

|追跡レコード|[説明]|
|---------------------|-----------------|
|ワークフロー インスタンスの追跡レコード|ワークフロー インスタンスのライフサイクルを表します。 たとえば、ワークフローの開始時または完了時にインスタンス レコードが出力されます。|
|アクティビティ状態の追跡レコード|アクティビティの実行状況を詳しく記録します。 これらのレコードは、アクティビティをスケジュールしたとき、アクティビティが完了したとき、エラーがスローされたときなど、ワークフロー アクティビティの状態を示します。|
|ブックマーク再開レコード|ワークフロー インスタンス内のブックマークが再開されたときに出力されます。|
|カスタム追跡レコード|ワークフロー作成者はカスタム追跡レコードを作成し、カスタム アクティビティ内で出力できます。|
|<xref:System.Activities.Tracking.ActivityScheduledRecord>|このレコードは、アクティビティが別のアクティビティをスケジュールするときに出力されます。|
|<xref:System.Activities.Tracking.FaultPropagationRecord>|このレコードは、アクティビティからエラーが伝達されたときに出力されます。|
|<xref:System.Activities.Tracking.CancelRequestedRecord>|このレコードは、アクティビティが別のアクティビティによって取り消されたときに出力されます。|

追跡参加要素は、追跡プロファイルを使用して、出力された追跡レコードのサブセットを定期受信します。 追跡プロファイルには、特定の追跡レコード タイプを定期受信するための追跡クエリが含まれています。 追跡プロファイルは、コードで指定したり、構成で指定したりすることができます。

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 を使用して、EtwTrackingParticipantSample ソリューションファイルを開きます。

2. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。

3. ソリューションを実行するには、F5 キーを押します。

    既定では、サービスはポート 53797 (http://localhost:53797/SampleWorkflowService.xamlx)でリッスンしています。

4. ファイルエクスプローラーを使用して、WCF テストクライアントを開きます。

    WCF テストクライアント (Wcftestclient.exe) は、Visual Studio 2010 のインストールフォルダー > \Common7\IDE\ フォルダー \<にあります。

    既定の Visual Studio 2010 インストールフォルダーは、C:\Program の Visual Studio 10.0 です。

5. WCF テストクライアントで、 **[ファイル]** メニューの **[サービスの追加]** を選択します。

    入力ボックスにエンドポイントのアドレスを追加します。 既定では、 `http://localhost:53797/SampleWorkflowService.xamlx`です。

6. イベント ビューアー アプリケーションを開きます。

    サービスを呼び出す前に、 **[スタート]** メニューからイベントビューアーを開始し、 **[実行]** を選択して `eventvwr.exe`を入力します。 ワークフロー サービスから生成された追跡イベントをイベント ログでリッスンしていることを確認します。

7. イベントビューアーのツリービューで、 **[イベントビューアー]** 、 **[アプリケーションとサービスログ]** 、 **[Microsoft]** の順に移動します。 **[Microsoft]** を右クリックして **[表示]** を選択し、 **[分析およびデバッグログの表示]** をクリックして、分析ログとデバッグログを有効にします。

    [**分析とデバッグログを表示**する] オプションがオンになっていることを確認します。

8. イベントビューアーのツリービューで、 **[イベントビューアー]** 、 **[アプリケーションとサービスログ]** 、 **[Microsoft]** 、 **[Windows]** 、 **[アプリケーションサーバー-アプリケーション]** の順に移動します。 **[分析]** を右クリックし、 **[ログを有効]** にする を選択して、**分析**ログを有効にします。

9. WCF テスト クライアントで、`GetData` をダブルクリックしてサービスをテストします。

    `GetData` メソッドが開きます。 この要求はパラメーターを 1 つ受け取ります。値が 0 (既定値) になっていることを確認します。

     **[呼び出し]** をクリックします。

10. ワークフローから出力されたイベントを確認します。

    イベントビューアーに戻り、 **[イベントビューアー]** 、 **[アプリケーションとサービスログ]** 、 **[Microsoft]** 、 **[Windows]** 、 **[アプリケーションサーバー-アプリケーション]** の順に移動します。 **[分析]** を右クリックし、 **[更新]** を選択します。

    イベント ビューアーにワークフロー イベントが表示されます。 ワークフロー実行イベントが表示され、その中にワークフローのエラーに対応する未処理の例外が含まれていることに注意してください。 また、アクティビティがエラーをスローしたことを示す警告イベントもワークフロー アクティビティから出力されます。

11. エラーがスローされないように入力データを 0 以外にして、手順 9. と 10. を繰り返します。

追跡プロファイルを使用すると、実行時にワークフロー インスタンスの状態が変化したときに生成されるイベントを定期受信できます。 監視の要件に応じて、ワークフローの主な状態変化の少数のセットを定期受信する、大まかなプロファイルを作成できます。 それとは反対に、後で実行を十分に再構築できるほど出力が豊富な、詳細なプロファイルを作成することもできます。 このサンプルでは、イベントの少数のセットを出力する `HealthMonitoring Tracking Profile` を使用して、ワークフロー ランタイムから ETW に出力されるイベントを示します。 Web.config には、それよりも多くのワークフロー追跡イベントを出力する `Troubleshooting Tracking Profile` という名前の別のプロファイルも用意されています。 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] をインストールすると、Machine.config ファイルでは、空の名前の既定のプロファイルが構成されています。 このプロファイルは、プロファイル名が指定されなかった場合や空のプロファイル名が指定された場合に、ETW 追跡動作の構成で使用されます。

状態監視追跡プロファイルでは、ワークフロー インスタンス レコードとアクティビティ エラー伝達レコードが出力されます。 このプロファイルは、Web.config 構成ファイルに次の追跡プロファイルを追加して作成されます。

```xml
<tracking>
  <profiles>
    <trackingProfile name="HealthMonitoring Tracking Profile">
      <workflow activityDefinitionId="*">
        <workflowInstanceQueries>
          <workflowInstanceQuery>
            <states>
              <state name="Started"/>
              <state name="Completed"/>
              <state name="Aborted"/>
              <state name="UnhandledException"/>
            </states>
          </workflowInstanceQuery>
        </workflowInstanceQueries>
        <faultPropagationQueries>
          <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>
        </faultPropagationQueries>
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```

 プロファイルを変更するには、`EtwTrackingParticipant` 構成を次のように変更します。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior>
      <etwTracking profileName="HealthMonitoring Tracking Profile"/>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

#### <a name="to-clean-up-optional"></a>クリーンアップするには (省略可能)

1. イベント ビューアーを開きます。

2. **[イベントビューアー]** 、 **[アプリケーションとサービスログ]** 、 **[Microsoft]** 、 **[Windows]** 、 **[アプリケーションサーバー-アプリケーション]** の順に移動します。 **[分析]** を右クリックし、 **[ログを無効にする]** を選択します。

3. **[イベントビューアー]** 、 **[アプリケーションとサービスログ]** 、 **[Microsoft]** 、 **[Windows]** 、 **[アプリケーションサーバー-アプリケーション]** の順に移動します。 **[分析]** を右クリックし、 **[ログの消去]** を選択します。

4. **[クリア]** オプションを選択して、イベントを消去します。

## <a name="known-issue"></a>既知の問題

> [!NOTE]
> イベント ビューアーの既知の問題により、ETW イベントをデコードできない場合があります。 その場合、次のようなエラー メッセージが表示されます。
>
> ソース Microsoft-Windows-アプリケーションサーバーからのイベント ID \<id > の説明が見つかりません。 このイベントを発生させるコンポーネントがローカル コンピューターにインストールされていないか、インストールが壊れています。 ローカル コンピューターにコンポーネントをインストールするか、コンポーネントを修復してください。
>
> このエラーが発生した場合は、操作ウィンドウで [最新の情報に更新] をクリックしてください。 これにより、イベントが正常にデコードされます。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\EtwTracking`

## <a name="see-also"></a>参照

- [AppFabric の監視のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
