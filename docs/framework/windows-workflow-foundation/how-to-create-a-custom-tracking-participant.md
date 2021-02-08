---
description: '詳細については、「方法: カスタム追跡参加要素を作成する」を参照してください。'
title: '方法: カスタム追跡参加要素を作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1b612c7e-2381-4a7c-b07a-77030415f2a3
ms.openlocfilehash: 8b435ac110d286d0582700289079459cca3cf979
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99777841"
---
# <a name="how-to-create-a-custom-tracking-participant"></a>方法: カスタム追跡参加要素を作成する

ワークフロー追跡により、ワークフロー実行の状態が視覚的に示されます。 ワークフロー ランタイムによって、ワークフローのライフサイクル イベント、アクティビティのライフサイクル イベント、ブックマークの再開、およびエラーについて説明する追跡レコードが出力されます。 これらの追跡レコードは、追跡参加要素によって使用されます。 Windows Workflow Foundation (WF) には、追跡レコードを Windows イベントトレーシング (ETW) イベントとして書き込む標準の追跡参加要素が含まれています。 これで要件が満たされない場合は、カスタムの追跡参加要素を作成することもできます。 チュートリアルのこの手順では、`WriteLine` アクティビティの出力をキャプチャするカスタム追跡参加要素と追跡プロファイルを作成して、ユーザーに表示できるようにする方法について説明します。  
  
## <a name="to-create-the-custom-tracking-participant"></a>カスタム追跡参加要素を作成するには  
  
1. **ソリューションエクスプローラー** で [ **NumberGuessWorkflowHost** ] を右クリックし、[**追加**]、[**クラス**] の順に選択します。 `StatusTrackingParticipant`[**名前**] ボックスに「」と入力し、[**追加**] をクリックします。  
  
2. 次の `using` (または `Imports`) ステートメントを、他の `using` (または `Imports`) ステートメントを含むファイルの先頭に追加します。  
  
    ```vb  
    Imports System.Activities.Tracking  
    Imports System.IO  
    ```  
  
    ```csharp  
    using System.Activities.Tracking;  
    using System.IO;  
    ```  
  
3. `StatusTrackingParticipant` クラスを変更して、`TrackingParticipant` から継承されるようにします。  
  
    ```vb  
    Public Class StatusTrackingParticipant  
        Inherits TrackingParticipant  
  
    End Class  
    ```  
  
    ```csharp  
    class StatusTrackingParticipant : TrackingParticipant  
    {  
    }  
    ```  
  
4. 次の `Track` メソッド オーバーライドを追加します。 追跡レコードには、いくつかの種類があります。 ここでは、アクティビティ追跡レコードに含まれる `WriteLine` アクティビティの出力に注目します。 `TrackingRecord` が `ActivityTrackingRecord` アクティビティの `WriteLine` である場合は、ワークフローの `Text` に基づいた名前の付いたファイルに `WriteLine` の `InstanceId` が追加されます。 このチュートリアルでは、ファイルはホスト アプリケーションの現在のフォルダーに保存されます。  
  
    ```vb  
    Protected Overrides Sub Track(record As TrackingRecord, timeout As TimeSpan)  
        Dim asr As ActivityStateRecord = TryCast(record, ActivityStateRecord)  
  
        If Not asr Is Nothing Then  
            If asr.State = ActivityStates.Executing And _  
            asr.Activity.TypeName = "System.Activities.Statements.WriteLine" Then  
  
                'Append the WriteLine output to the tracking  
                'file for this instance.  
                Using writer As StreamWriter = File.AppendText(record.InstanceId.ToString())  
                    writer.WriteLine(asr.Arguments("Text"))  
                    writer.Close()  
                End Using  
            End If  
        End If  
    End Sub  
    ```  
  
    ```csharp  
    protected override void Track(TrackingRecord record, TimeSpan timeout)  
    {  
        ActivityStateRecord asr = record as ActivityStateRecord;  
  
        if (asr != null)  
        {  
            if (asr.State == ActivityStates.Executing &&  
                asr.Activity.TypeName == "System.Activities.Statements.WriteLine")  
            {  
                // Append the WriteLine output to the tracking  
                // file for this instance  
                using (StreamWriter writer = File.AppendText(record.InstanceId.ToString()))  
                {  
                    writer.WriteLine(asr.Arguments["Text"]);  
                    writer.Close();  
                }  
            }  
        }  
    }  
    ```  
  
     追跡プロファイルを指定しない場合は、既定の追跡プロファイルが使用されます。 既定の追跡プロファイルを使用する場合は、すべての `ActivityStates` に関する追跡レコードが出力されます。 ここでは、`WriteLine` アクティビティのライフサイクル中に 1 回だけテキストをキャプチャする必要があるため、`ActivityStates.Executing` 状態からテキストを抽出するだけです。 追跡 [プロファイルを作成して追跡参加要素を登録するに](#to-create-the-tracking-profile-and-register-the-tracking-participant)は、追跡 `WriteLine` レコードのみが出力されることを指定する追跡プロファイルを作成し `ActivityStates.Executing` ます。  
  
## <a name="to-create-the-tracking-profile-and-register-the-tracking-participant"></a>追跡プロファイルを作成して追跡参加要素を登録するには  
  
1. **ソリューションエクスプローラー** で [ **WorkflowHostForm** ] を右クリックし、[**コードの表示**] を選択します。  
  
2. 次の `using` (または `Imports`) ステートメントを、他の `using` (または `Imports`) ステートメントを含むファイルの先頭に追加します。  
  
    ```vb  
    Imports System.Activities.Tracking  
    ```  
  
    ```csharp  
    using System.Activities.Tracking;  
    ```  
  
3. ワークフロー拡張機能に `ConfigureWorkflowApplication` を追加するコードの直後で、ワークフロー ライフサイクル ハンドラーの前に、`StringWriter` に次のコードを追加します。  
  
    ```vb  
    'Add the custom tracking participant with a tracking profile  
    'that only emits tracking records for WriteLine activities.  
    Dim query As New ActivityStateQuery()  
    query.ActivityName = "WriteLine"  
    query.States.Add(ActivityStates.Executing)  
    query.Arguments.Add("Text")  
  
    Dim profile As New TrackingProfile()  
    profile.Queries.Add(query)  
  
    Dim stp As New StatusTrackingParticipant()  
    stp.TrackingProfile = profile  
  
    wfApp.Extensions.Add(stp)  
    ```  
  
    ```csharp  
    // Add the custom tracking participant with a tracking profile  
    // that only emits tracking records for WriteLine activities.  
    StatusTrackingParticipant stp = new StatusTrackingParticipant  
    {  
        TrackingProfile = new TrackingProfile  
        {  
            Queries =
            {  
                new ActivityStateQuery  
                {  
                    ActivityName = "WriteLine",  
                    States = { ActivityStates.Executing },  
                    Arguments = { "Text" }  
                }  
            }  
        }  
    };  
  
    wfApp.Extensions.Add(stp);  
    ```  
  
     この追跡プロファイルでは、`WriteLine` 状態の `Executing` アクティビティのアクティビティ状態レコードのみがカスタム追跡参加要素に出力されるように指定します。  
  
     このコードを追加した後、`ConfigureWorkflowApplication` の先頭は次の例のようになります。  
  
    ```vb  
    Private Sub ConfigureWorkflowApplication(wfApp As WorkflowApplication)  
        'Configure the persistence store.  
        wfApp.InstanceStore = store  
  
        'Add a StringWriter to the extensions. This captures the output  
        'from the WriteLine activities so we can display it in the form.  
        Dim sw As New StringWriter()  
        wfApp.Extensions.Add(sw)  
  
        'Add the custom tracking participant with a tracking profile  
        'that only emits tracking records for WriteLine activities.  
        Dim query As New ActivityStateQuery()  
        query.ActivityName = "WriteLine"  
        query.States.Add(ActivityStates.Executing)  
        query.Arguments.Add("Text")  
  
        Dim profile As New TrackingProfile()  
        profile.Queries.Add(query)  
  
        Dim stp As New StatusTrackingParticipant()  
        stp.TrackingProfile = profile  
  
        wfApp.Extensions.Add(stp)  
  
        'Workflow lifecycle handlers...  
    ```  
  
    ```csharp  
    private void ConfigureWorkflowApplication(WorkflowApplication wfApp)  
    {  
        // Configure the persistence store.  
        wfApp.InstanceStore = store;  
  
        // Add a StringWriter to the extensions. This captures the output  
        // from the WriteLine activities so we can display it in the form.  
        StringWriter sw = new StringWriter();  
        wfApp.Extensions.Add(sw);  
  
        // Add the custom tracking participant with a tracking profile  
        // that only emits tracking records for WriteLine activities.  
        StatusTrackingParticipant stp = new StatusTrackingParticipant  
        {  
            TrackingProfile = new TrackingProfile  
            {  
                Queries =
                {  
                    new ActivityStateQuery  
                    {  
                        ActivityName = "WriteLine",  
                        States = { ActivityStates.Executing },  
                        Arguments = { "Text" }  
                    }  
                }  
            }  
        };  
  
        wfApp.Extensions.Add(stp);  
  
        // Workflow lifecycle handlers...  
    ```  
  
## <a name="to-display-the-tracking-information"></a>追跡情報を表示するには  
  
1. **ソリューションエクスプローラー** で [ **WorkflowHostForm** ] を右クリックし、[**コードの表示**] を選択します。  
  
2. `InstanceId_SelectedIndexChanged` ハンドラーで、ステータス ウィンドウをクリアするコードの直後に次のコードを追加します。  
  
    ```vb  
    'If there is tracking data for this workflow, display it  
    'in the status window.  
    If File.Exists(WorkflowInstanceId.ToString()) Then  
        Dim status As String = File.ReadAllText(WorkflowInstanceId.ToString())  
        UpdateStatus(status)  
    End If  
    ```  
  
    ```csharp  
    // If there is tracking data for this workflow, display it  
    // in the status window.  
    if (File.Exists(WorkflowInstanceId.ToString()))  
    {  
        string status = File.ReadAllText(WorkflowInstanceId.ToString());  
        UpdateStatus(status);  
    }  
    ```  
  
     ワークフローの一覧で新しいワークフローを選択すると、そのワークフローの追跡レコードがステータス ウィンドウに読み込まれて表示されます。 完成した `InstanceId_SelectedIndexChanged` ハンドラーは次のようになります。  
  
    ```vb  
    Private Sub InstanceId_SelectedIndexChanged(sender As Object, e As EventArgs) Handles InstanceId.SelectedIndexChanged  
        If InstanceId.SelectedIndex = -1 Then  
            Return  
        End If  
  
        'Clear the status window.  
        WorkflowStatus.Clear()  
  
        'If there is tracking data for this workflow, display it  
        'in the status window.  
        If File.Exists(WorkflowInstanceId.ToString()) Then  
            Dim status As String = File.ReadAllText(WorkflowInstanceId.ToString())  
            UpdateStatus(status)  
        End If  
  
        'Get the workflow version and display it.  
        'If the workflow is just starting then this info will not  
        'be available in the persistence store so do not try and retrieve it.  
        If Not WorkflowStarting Then  
            Dim instance As WorkflowApplicationInstance = _  
                WorkflowApplication.GetInstance(WorkflowInstanceId, store)  
  
            WorkflowVersion.Text = _  
                WorkflowVersionMap.GetIdentityDescription(instance.DefinitionIdentity)  
  
            'Unload the instance.  
            instance.Abandon()  
        End If  
    End Sub  
    ```  
  
    ```csharp  
    private void InstanceId_SelectedIndexChanged(object sender, EventArgs e)  
    {  
        if (InstanceId.SelectedIndex == -1)  
        {  
            return;  
        }  
  
        // Clear the status window.  
        WorkflowStatus.Clear();  
  
        // If there is tracking data for this workflow, display it  
        // in the status window.  
        if (File.Exists(WorkflowInstanceId.ToString()))  
        {  
            string status = File.ReadAllText(WorkflowInstanceId.ToString());  
            UpdateStatus(status);  
        }  
  
        // Get the workflow version and display it.  
        // If the workflow is just starting then this info will not  
        // be available in the persistence store so do not try and retrieve it.  
        if (!WorkflowStarting)  
        {  
            WorkflowApplicationInstance instance =  
                WorkflowApplication.GetInstance(this.WorkflowInstanceId, store);  
  
            WorkflowVersion.Text =  
                WorkflowVersionMap.GetIdentityDescription(instance.DefinitionIdentity);  
  
            // Unload the instance.  
            instance.Abandon();  
        }  
    }  
    ```  
  
## <a name="to-build-and-run-the-application"></a>アプリケーションをビルドして実行するには  
  
1. Ctrl キーと Shift キーを押しながら B キーを押してアプリケーションをビルドします。  
  
2. Ctrl キーを押しながら F5 キーを押してアプリケーションを起動します。  
  
3. 推測ゲームの範囲と開始するワークフローの種類を選択し、[ **新しいゲーム**] をクリックします。 [ **推定** ] ボックスに推測を入力し、[ **ジャンプ** ] をクリックして推測を送信します。 ワークフローの状態がステータス ウィンドウに表示されます。 この出力は、`WriteLine` アクティビティからキャプチャされます。 [ **ワークフローインスタンス Id** ] コンボボックスから別のワークフローに切り替え、現在のワークフローの状態が削除されていることを確認します。 以前のワークフローに戻して、次の例のように、状態が復元されることを確認します。  
  
    > [!NOTE]
    > 追跡が有効になる前に開始されたワークフローに切り替えた場合、状態は表示されません。 ただし、推定値を追加すると、この時点では追跡が有効になっているため、その状態が保存されます。  
  
    ```output
    Please enter a number between 1 and 10
    Your guess is too high.
    Please enter a number between 1 and 10
    ```

    > [!NOTE]
    > この情報は、乱数の範囲を決定する場合に役立ちますが、これまでに作成された推定値に関する情報を含んでいません。 この情報については、次の手順「 [方法: ワークフローの複数のバージョンを side-by-side でホストする](how-to-host-multiple-versions-of-a-workflow-side-by-side.md)」を参照してください。

    ワークフロー インスタンス ID を書き留め、ゲームを最後まで実行します。
  
4. エクスプローラーを開き、 **NumberGuessWorkflowHost\bin\debug** フォルダー (またはプロジェクトの設定によっては **bin\release** ) に移動します。 プロジェクトの実行可能ファイルに加え、GUID ファイル名が付いたファイルがあることに注意します。 前の手順で完了したワークフローのワークフロー インスタンス ID に対応するファイルを特定してメモ帳で開きます。 追跡情報は、次のような情報が含まれています。  
  
    ```output
    Please enter a number between 1 and 10
    Your guess is too high.
    Please enter a number between 1 and 10
    Your guess is too high.
    Please enter a number between 1 and 10
    ```

    この追跡データには、ユーザーの推定値だけでなく、ワークフローの最後の推定値に関する情報も含まれていません。 これは、追跡情報がワークフローからの `WriteLine` 出力のみで構成されており、表示される最後のメッセージがワークフロー完了後に `Completed` ハンドラーから表示されるためです。 チュートリアルの次の手順「 [方法: ワークフローの複数のバージョンを side-by-side にホスト](how-to-host-multiple-versions-of-a-workflow-side-by-side.md)する」では、 `WriteLine` ユーザーの推定を表示するように既存のアクティビティを変更し、 `WriteLine` 最終結果を表示する追加のアクティビティを追加します。 これらの変更を統合した後は、「 [方法: ワークフローの複数のバージョンをホスト](how-to-host-multiple-versions-of-a-workflow-side-by-side.md) する」では、ワークフローの複数のバージョンを同時にホストする方法を示します。
