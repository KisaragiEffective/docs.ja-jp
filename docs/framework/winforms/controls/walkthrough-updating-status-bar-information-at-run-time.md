---
title: 'チュートリアル : ステータス バー情報の実行時更新'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- status bars [Windows Forms], updating at run time
- status bars [Windows Forms], refreshing panels
- StatusBar control [Windows Forms], refreshing panels
- panels [Windows Forms], refreshing status bar
ms.assetid: cc2abb06-c082-49f7-a5a3-2fd1bbcb58d1
ms.openlocfilehash: f1d22079dfa3a6452efb74ef57530bb43e059a4a
ms.sourcegitcommit: 5a28f8eb071fcc09b045b0c4ae4b96898673192e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2019
ms.locfileid: "73197099"
---
# <a name="walkthrough-updating-status-bar-information-at-run-time"></a>チュートリアル : ステータス バー情報の実行時更新
> [!IMPORTANT]
> <xref:System.Windows.Forms.StatusStrip> コントロールと <xref:System.Windows.Forms.ToolStripStatusLabel> コントロールは、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> コントロールに対して機能を置き換え、追加します。ただし、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> のコントロールは、下位互換性と将来の使用の両方のために保持されます (選択した場合)。  
  
 多くの場合、アプリケーションの状態の変化や他のユーザー操作に基づいて、ステータス バー パネルの内容を実行時に動的に更新する必要があります。 これは、CapsLock、NumLock、ScrollLock などのキーが有効化されたことをユーザーに通知したり、便利な情報として日付や時刻を表示したりするための一般的な方法です。  
  
 次の例では、<xref:System.Windows.Forms.StatusBarPanel> クラスのインスタンスを使用して、クロックをホストします。  
  
### <a name="to-get-the-status-bar-ready-for-updating"></a>ステータス バーを更新できる状態にするには  
  
1. 新しい Windows フォームを作成します。  
  
2. フォームに <xref:System.Windows.Forms.StatusBar> コントロールを追加します。 詳細については、「[方法 : Windows フォームにコントロールを追加する](how-to-add-controls-to-windows-forms.md)」を参照してください。  
  
3. ステータスバーパネルを <xref:System.Windows.Forms.StatusBar> コントロールに追加します。 詳細については、「[方法 : StatusBar コントロールにパネルを追加する](how-to-add-panels-to-a-statusbar-control.md)」を参照してください。  
  
4. フォームに追加した <xref:System.Windows.Forms.StatusBar> コントロールについては、<xref:System.Windows.Forms.StatusBar.ShowPanels%2A> プロパティを `true`に設定します。  
  
5. フォームに Windows フォーム <xref:System.Windows.Forms.Timer> コンポーネントを追加します。  
  
    > [!NOTE]
    > Windows フォーム <xref:System.Windows.Forms.Timer?displayProperty=nameWithType> コンポーネントは Windows フォーム環境向けに設計されています。 サーバー環境に適したタイマーが必要な場合は、「[サーバー ベースのタイマーの概要](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/tb9yt5e6(v=vs.90))」を参照してください。  
  
6. <xref:System.Windows.Forms.Timer.Enabled%2A> プロパティを `true`に設定します。  
  
7. <xref:System.Windows.Forms.Timer> の <xref:System.Windows.Forms.Timer.Interval%2A> プロパティを3万に設定します。  
  
    > [!NOTE]
    > <xref:System.Windows.Forms.Timer> コンポーネントの [<xref:System.Windows.Forms.Timer.Interval%2A>] プロパティは、表示される時刻に正確な時刻が反映されるように、30秒 (3万ミリ秒) に設定されます。  
  
### <a name="to-implement-the-timer-to-update-the-status-bar"></a>タイマーを実装してステータス バーを更新するには  
  
1. <xref:System.Windows.Forms.Timer> コンポーネントのイベントハンドラーに次のコードを挿入して、<xref:System.Windows.Forms.StatusBar> コントロールのパネルを更新します。  
  
    ```vb  
    Private Sub Timer1_Tick(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Timer1.Tick  
       StatusBar1.Panels(0).Text = Now.ToShortTimeString  
    End Sub  
    ```  
  
    ```csharp  
    private void timer1_Tick(object sender, System.EventArgs e)  
    {  
       statusBar1.Panels[0].Text = DateTime.Now.ToShortTimeString();  
    }  
    ```  
  
    ```cpp  
    private:  
      System::Void timer1_Tick(System::Object ^ sender,  
        System::EventArgs ^ e)  
      {  
        statusBar1->Panels[0]->Text =  
          DateTime::Now.ToShortTimeString();  
      }  
    ```  
  
     この時点で、アプリケーションを実行して、ステータス バー パネルで実行される時計を確認できる状態になります。  
  
### <a name="to-test-the-application"></a>アプリケーションをテストするには  
  
1. アプリケーションをデバッグし、F5 キーを押してアプリケーションを実行します。 デバッグの詳細については、「[Visual Studio でのデバッグ](/visualstudio/debugger/debugger-feature-tour)」を参照してください。  
  
    > [!NOTE]
    > ステータス バーに時計が表示されるまでに約 30 秒かかります。 これは、できるだけ正確な時刻を取得するためです。 反対に、時計がすぐに表示されるようにするには、前の手順の手順 7. で設定した <xref:System.Windows.Forms.Timer.Interval%2A> プロパティの値を小さくします。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.StatusBar>
- <xref:System.Windows.Forms.ToolStripStatusLabel>
- [方法: StatusBar コントロールにパネルを追加する](how-to-add-panels-to-a-statusbar-control.md)
- [方法: Windows フォームの StatusBar コントロールでクリックされたパネルを確認する](determine-which-panel-wf-statusbar-control-was-clicked.md)
- [StatusBar コントロールの概要](statusbar-control-overview-windows-forms.md)
