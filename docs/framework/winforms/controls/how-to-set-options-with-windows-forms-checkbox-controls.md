---
title: CheckBox コントロールでオプションを設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- checked
helpviewer_keywords:
- CheckBox control [Windows Forms], checked state
- check boxes [Windows Forms], using to set options
- CheckBox control [Windows Forms], using to set options
ms.assetid: 2ac70498-7e3e-4e07-8901-ccabaeb5fd3e
ms.openlocfilehash: 84198eab42aa02b1bb37fa16a3c4247a37f58a10
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746770"
---
# <a name="how-to-set-options-with-windows-forms-checkbox-controls"></a>方法 : Windows フォームの CheckBox コントロールでオプションを設定する
Windows フォーム <xref:System.Windows.Forms.CheckBox> コントロールは、ユーザーに True/False または Yes/No オプションを提供するために使用されます。 コントロールが選択されると、そのコントロールにチェックマークが表示されます。  
  
### <a name="to-set-options-with-checkbox-controls"></a>CheckBox コントロールを使用してオプションを設定するには  
  
1. <xref:System.Windows.Forms.CheckBox.Checked%2A> プロパティの値を調べてその状態を確認し、その値を使用してオプションを設定します。  
  
     次のコードサンプルでは、<xref:System.Windows.Forms.CheckBox> コントロールの <xref:System.Windows.Forms.CheckBox.CheckedChanged> イベントが発生すると、このチェックボックスがオンになっている場合、フォームの <xref:System.Windows.Forms.Control.AllowDrop%2A> プロパティが `false` に設定されます。 これは、ユーザーの操作を制限する必要がある場合に便利です。  
  
    ```vb  
    Private Sub CheckBox1_CheckedChanged(ByVal sender As System.Object, _  
       ByVal e As System.EventArgs) Handles CheckBox1.CheckedChanged  
       ' Determine the CheckState of the check box.  
       If CheckBox1.CheckState = CheckState.Checked Then  
          ' If checked, do not allow items to be dragged onto the form.  
          Me.AllowDrop = False  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void checkBox1_CheckedChanged(object sender, System.EventArgs e)  
    {  
       // Determine the CheckState of the check box.  
       if (checkBox1.CheckState == CheckState.Checked)   
       {  
          // If checked, do not allow items to be dragged onto the form.  
          this.AllowDrop = false;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void checkBox1_CheckedChanged(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          // Determine the CheckState of the check box.  
          if (checkBox1->CheckState == CheckState::Checked)   
          {  
             // If checked, do not allow items to be dragged onto the form.  
             this->AllowDrop = false;  
          }  
       }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.CheckBox>
- [CheckBox コントロールの概要](checkbox-control-overview-windows-forms.md)
- [方法: Windows フォームの CheckBox のクリックに応答する](how-to-respond-to-windows-forms-checkbox-clicks.md)
- [CheckBox コントロール](checkbox-control-windows-forms.md)
