---
title: RichTextBox コントロールでの書式設定属性の変更を確認する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], text boxes
- RichTextBox control [Windows Forms], determining font changes
- text boxes [Windows Forms], determining font changes
- SelChange event
ms.assetid: bdfed015-f77a-41e5-b38f-f8629b2fa166
ms.openlocfilehash: f9b2a1028f79059ec7d4d6bf3683100455bb5dea
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746038"
---
# <a name="how-to-determine-when-formatting-attributes-change-in-the-windows-forms-richtextbox-control"></a>方法 : Windows フォームの RichTextBox コントロールにおける書式属性の変更を確認する
Windows フォーム <xref:System.Windows.Forms.RichTextBox> コントロールの一般的な使用方法としては、フォントオプションや段落スタイルなどの属性を使用したテキストの書式設定があります。 多くのワードプロセッシングアプリケーションのように、アプリケーションでは、ツールバーを表示するためにテキストの書式設定の変更を追跡する必要がある場合があります。  
  
### <a name="to-respond-to-changes-in-formatting-attributes"></a>属性の書式設定の変更に応答するには  
  
1. 属性の値に応じて適切なアクションを実行するには、<xref:System.Windows.Forms.RichTextBox.SelectionChanged> イベントハンドラーにコードを記述します。 次の例では、<xref:System.Windows.Forms.RichTextBox.SelectionBullet%2A> プロパティの値に応じて、ツールバーボタンの外観を変更します。 ツールバーボタンは、カーソルがコントロール内で移動したときにのみ更新されます。  
  
     次の例では、フォームに <xref:System.Windows.Forms.RichTextBox> コントロールと、ツールバーボタンを含む <xref:System.Windows.Forms.ToolBar> コントロールがあることを前提としています。 ツールバーとツールバーボタンの詳細については、「[方法: ツールバーコントロールにボタンを追加](how-to-add-buttons-to-a-toolbar-control.md)する」を参照してください。  
  
    ```vb  
    ' The following code assumes the existence of a toolbar control  
    ' with at least one toolbar button.  
    Private Sub RichTextBox1_SelectionChanged(ByVal sender As Object, ByVal e As System.EventArgs) Handles RichTextBox1.SelectionChanged  
       If RichTextBox1.SelectionBullet = True Then  
          ' Bullet button on toolbar should appear pressed  
          ToolBarButton1.Pushed = True  
       Else  
           ' Bullet button on toolbar should appear unpressed  
           ToolBarButton1.Pushed = False  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    // The following code assumes the existence of a toolbar control  
    // with at least one toolbar button.  
    private void richTextBox1_SelectionChanged(object sender,  
    System.EventArgs e)  
    {  
       if (richTextBox1.SelectionBullet == true)   
       {  
          // Bullet button on toolbar should appear pressed  
          toolBarButton1.Pushed = true;  
       }  
       else   
       {  
          // Bullet button on toolbar should appear unpressed  
          toolBarButton1.Pushed = false;  
       }  
    }  
    ```  
  
    ```cpp  
    // The following code assumes the existence of a toolbar control  
    // with at least one toolbar button.  
    private:  
       System::Void richTextBox1_SelectionChanged(  
          System::Object ^  sender, System::EventArgs ^  e)  
       {  
          if (richTextBox1->SelectionBullet == true)  
          {  
             // Bullet button on toolbar should appear pressed  
             toolBarButton1->Pushed = true;  
          }  
          else  
          {  
             // Bullet button on toolbar should appear unpressed  
             toolBarButton1->Pushed = false;  
          }  
       }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBox.SelectionChanged>
- <xref:System.Windows.Forms.RichTextBox>
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
