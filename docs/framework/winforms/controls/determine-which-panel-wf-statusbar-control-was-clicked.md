---
title: StatusBar コントロールでクリックされたパネルを確認する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- status bars [Windows Forms], determining panel clicked
- panels [Windows Forms], determining clicked
- StatusBar control [Windows Forms], coding panel click events
- StatusBar control [Windows Forms], determining panel clicked
- PanelClick event [Windows Forms], determining panel clicked
- Panel control [Windows Forms], determining click
ms.assetid: d14c6092-04b2-4a07-8ddf-0dd11277ff5f
ms.openlocfilehash: 94619f8bd426a42e5dafa0db99880e20d24f9963
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746012"
---
# <a name="how-to-determine-which-panel-in-the-windows-forms-statusbar-control-was-clicked"></a>方法 : Windows フォームの StatusBar コントロールでクリックされたパネルを確認する
> [!IMPORTANT]
> <xref:System.Windows.Forms.StatusStrip> コントロールと <xref:System.Windows.Forms.ToolStripStatusLabel> コントロールは、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> コントロールに対して機能を置き換え、追加します。ただし、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> のコントロールは、下位互換性と将来の使用の両方のために保持されます (選択した場合)。  
  
 ユーザーのクリックに応答するように[StatusBar コントロール](statusbar-control-windows-forms.md)コントロールをプログラミングするには、<xref:System.Windows.Forms.StatusBar.PanelClick> イベント内で case ステートメントを使用します。 イベントには、クリックされた <xref:System.Windows.Forms.StatusBarPanel>への参照を含む引数 (panel 引数) が含まれています。 この参照を使用して、クリックされたパネルのインデックスを特定し、それに応じてプログラムを作成できます。  
  
> [!NOTE]
> <xref:System.Windows.Forms.StatusBar> コントロールの <xref:System.Windows.Forms.StatusBar.ShowPanels%2A> プロパティが `true`に設定されていることを確認します。  
  
### <a name="to-determine-which-panel-was-clicked"></a>クリックされたパネルを確認するには  
  
1. <xref:System.Windows.Forms.StatusBar.PanelClick> イベントハンドラーで、`Select Case` (Visual Basic) または `switch case` (Visual C#または visual C++) ステートメントを使用して、クリックされたパネルを判別します。これを行うには、イベント引数でクリックしたパネルのインデックスを調べます。  
  
     次のコード例では、<xref:System.Windows.Forms.StatusBar> コントロール、`StatusBar1`、および2つの <xref:System.Windows.Forms.StatusBarPanel> オブジェクト、`StatusBarPanel1` および `StatusBarPanel2`の存在をフォーム上に指定する必要があります。  
  
    ```vb  
    Private Sub StatusBar1_PanelClick(ByVal sender As System.Object, ByVal e As System.Windows.Forms.StatusBarPanelClickEventArgs) Handles StatusBar1.PanelClick  
       Select Case StatusBar1.Panels.IndexOf(e.StatusBarPanel)  
         Case 0  
           MessageBox.Show("You have clicked Panel One.")  
         Case 1  
           MessageBox.Show("You have clicked Panel Two.")  
       End Select  
    End Sub  
    ```  
  
    ```csharp  
    private void statusBar1_PanelClick(object sender,   
    System.Windows.Forms.StatusBarPanelClickEventArgs e)  
    {  
       switch (statusBar1.Panels.IndexOf(e.StatusBarPanel))  
       {  
          case 0 :  
             MessageBox.Show("You have clicked Panel One.");  
             break;  
          case 1 :  
             MessageBox.Show("You have clicked Panel Two.");  
             break;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void statusBar1_PanelClick(System::Object ^  sender,  
          System::Windows::Forms::StatusBarPanelClickEventArgs ^  e)  
       {  
          switch (statusBar1->Panels->IndexOf(e->StatusBarPanel))  
          {  
             case 0 :  
                MessageBox::Show("You have clicked Panel One.");  
                break;  
             case 1 :  
                MessageBox::Show("You have clicked Panel Two.");  
                break;  
          }  
       }  
    ```  
  
     (ビジュアルC#、ビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.statusBar1.PanelClick += new   
       System.Windows.Forms.StatusBarPanelClickEventHandler   
       (this.statusBar1_PanelClick);  
    ```  
  
    ```cpp  
    this->statusBar1->PanelClick += gcnew  
       System::Windows::Forms::StatusBarPanelClickEventHandler  
       (this, &Form1::statusBar1_PanelClick);  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.StatusBar>
- <xref:System.Windows.Forms.ToolStripStatusLabel>
- [方法: ステータス バー パネルのサイズを設定する](how-to-set-the-size-of-status-bar-panels.md)
- [チュートリアル: ステータス バー情報の実行時更新](walkthrough-updating-status-bar-information-at-run-time.md)
- [StatusBar コントロールの概要](statusbar-control-overview-windows-forms.md)
