---
title: CheckBox のクリックに応答する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Click event [Windows Forms], CheckBox control
- CheckBox control [Windows Forms], Click events
- events [Windows Forms], Click events
- double-clicks
- check boxes [Windows Forms], responding to events
ms.assetid: c39f901e-8899-43b6-aa31-939cbf7089fb
ms.openlocfilehash: ba2afb52939a6274978ce725dac19b5622419b99
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735670"
---
# <a name="how-to-respond-to-windows-forms-checkbox-clicks"></a>方法 : Windows フォームの CheckBox のクリックに応答する
ユーザーが Windows フォーム <xref:System.Windows.Forms.CheckBox> コントロールをクリックするたびに、<xref:System.Windows.Forms.Control.Click> イベントが発生します。 チェックボックスの状態に応じて、何らかのアクションを実行するようにアプリケーションをプログラミングできます。  
  
### <a name="to-respond-to-checkbox-clicks"></a>チェックボックスのクリックに応答するには  
  
1. <xref:System.Windows.Forms.Control.Click> イベントハンドラーで、<xref:System.Windows.Forms.CheckBox.Checked%2A> プロパティを使用してコントロールの状態を確認し、必要な操作を実行します。  
  
    ```vb  
    Private Sub CheckBox1_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles CheckBox1.Click  
       ' The CheckBox control's Text property is changed each time the   
       ' control is clicked, indicating a checked or unchecked state.  
       If CheckBox1.Checked = True Then  
          CheckBox1.Text = "Checked"  
       Else  
          CheckBox1.Text = "Unchecked"  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void checkBox1_Click(object sender, System.EventArgs e)  
    {  
       // The CheckBox control's Text property is changed each time the   
       // control is clicked, indicating a checked or unchecked state.  
       if (checkBox1.Checked)  
       {  
          checkBox1.Text = "Checked";  
       }  
       else  
       {  
          checkBox1.Text = "Unchecked";  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void checkBox1_CheckedChanged(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          if (checkBox1->Checked)  
          {  
             checkBox1->Text = "Checked";  
          }  
          else  
          {  
             checkBox1->Text = "Unchecked";  
          }  
       }  
    ```  
  
    > [!NOTE]
    > ユーザーが <xref:System.Windows.Forms.CheckBox> コントロールをダブルクリックすると、各クリックが個別に処理されます。つまり、<xref:System.Windows.Forms.CheckBox> コントロールでは、ダブルクリックイベントはサポートされません。  
  
    > [!NOTE]
    > <xref:System.Windows.Forms.CheckBox.AutoCheck%2A> プロパティが `true` (既定値) の場合、クリックされると <xref:System.Windows.Forms.CheckBox> が自動的に選択またはクリアされます。 それ以外の場合は、<xref:System.Windows.Forms.Control.Click> イベントが発生したときに、<xref:System.Windows.Forms.CheckBox.Checked%2A> プロパティを手動で設定する必要があります。  
  
     <xref:System.Windows.Forms.CheckBox> コントロールを使用して、一連の操作を決定することもできます。  
  
### <a name="to-determine-a-course-of-action-when-a-check-box-is-clicked"></a>チェックボックスがクリックされたときの動作を確認するには  
  
1. Case ステートメントを使用して、<xref:System.Windows.Forms.CheckBox.CheckState%2A> プロパティの値を照会し、アクションの実行を決定します。 <xref:System.Windows.Forms.CheckBox.ThreeState%2A> プロパティが `true`に設定されている場合、<xref:System.Windows.Forms.CheckBox.CheckState%2A> プロパティは、チェックボックスを表す3つの値、チェックボックスをオフにする値、またはチェックボックスが淡色表示され、オプションが使用できないことを示すために淡色表示された状態で表示される3つの不確定状態を返すことができます。  
  
    ```vb  
    Private Sub CheckBox1_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles CheckBox1.Click  
       Select Case CheckBox1.CheckState  
          Case CheckState.Checked  
             ' Code for checked state.  
          Case CheckState.Unchecked  
             ' Code for unchecked state.  
          Case CheckState.Indeterminate  
             ' Code for indeterminate state.  
       End Select   
    End Sub  
    ```  
  
    ```csharp  
    private void checkBox1_Click(object sender, System.EventArgs e)  
    {  
       switch(checkBox1.CheckState)  
       {  
          case CheckState.Checked:  
             // Code for checked state.  
             break;  
          case CheckState.Unchecked:  
             // Code for unchecked state.  
             break;  
          case CheckState.Indeterminate:  
             // Code for indeterminate state.  
             break;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void checkBox1_CheckedChanged(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          switch(checkBox1->CheckState) {  
             case CheckState::Checked:  
                // Code for checked state.  
                break;  
             case CheckState::Unchecked:  
                // Code for unchecked state.  
                break;  
             case CheckState::Indeterminate:  
                // Code for indeterminate state.  
                break;  
          }  
       }  
    ```  
  
    > [!NOTE]
    > <xref:System.Windows.Forms.CheckBox.ThreeState%2A> プロパティが `true`に設定されている場合、<xref:System.Windows.Forms.CheckBox.Checked%2A> プロパティは <xref:System.Windows.Forms.CheckState.Checked> と <xref:System.Windows.Forms.CheckState.Indeterminate>の両方に対して `true` を返します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.CheckBox>
- [CheckBox コントロールの概要](checkbox-control-overview-windows-forms.md)
- [方法: Windows フォームの CheckBox コントロールでオプションを設定する](how-to-set-options-with-windows-forms-checkbox-controls.md)
- [CheckBox コントロール](checkbox-control-windows-forms.md)
