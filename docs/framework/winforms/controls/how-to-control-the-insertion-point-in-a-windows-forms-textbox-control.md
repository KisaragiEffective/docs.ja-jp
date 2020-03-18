---
title: TextBox コントロールの挿入ポイントを制御する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- TextBox control [Windows Forms], insertion point
- insertion points [Windows Forms], TextBox controls
- text boxes [Windows Forms], controlling insertion point
ms.assetid: 5bee7d34-5121-429e-ab1f-d8ff67bc74c1
ms.openlocfilehash: fd4803820921f0c922a4ce885f809abee8fd4c6c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742206"
---
# <a name="how-to-control-the-insertion-point-in-a-windows-forms-textbox-control"></a>方法 : Windows フォーム TextBox コントロールでのカーソル位置を制御する
Windows フォーム <xref:System.Windows.Forms.TextBox> コントロールがまずフォーカスを受け取ると、テキストボックス内の既定の挿入は既存のテキストの左側に挿入されます。 ユーザーは、キーボードまたはマウスを使用して、挿入ポイントを移動できます。 テキストボックスでフォーカスが失われた場合は、ユーザーが最後に配置した場所に挿入ポイントが配置されます。  
  
 場合によっては、この動作をユーザーに混乱ことがあります。 ワードプロセッシングアプリケーションでは、既存のテキストの後に新しい文字が表示されることがあります。 データ入力アプリケーションでは、ユーザーは既存のエントリを置き換えるために新しい文字が必要になる場合があります。 <xref:System.Windows.Forms.TextBoxBase.SelectionStart%2A> プロパティと <xref:System.Windows.Forms.TextBoxBase.SelectionLength%2A> プロパティを使用すると、目的に合わせて動作を変更できます。  
  
### <a name="to-control-the-insertion-point-in-a-textbox-control"></a>TextBox コントロールの挿入位置を制御するには  
  
1. <xref:System.Windows.Forms.TextBoxBase.SelectionStart%2A> プロパティに適切な値を設定します。 0の場合、挿入ポイントは最初の文字のすぐ左に配置されます。  
  
2. Optional<xref:System.Windows.Forms.TextBoxBase.SelectionLength%2A> プロパティを、選択するテキストの長さに設定します。  
  
     次のコードは、常に0への挿入ポイントを返します。 `TextBox1_Enter` イベントハンドラーはコントロールにバインドされている必要があります。詳細については、「 [Windows フォームでのイベントハンドラーの作成](../creating-event-handlers-in-windows-forms.md)」を参照してください。  
  
    ```vb  
    Private Sub TextBox1_Enter(ByVal sender As Object, ByVal e As System.EventArgs) Handles TextBox1.Enter  
       TextBox1.SelectionStart = 0  
       TextBox1.SelectionLength = 0  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_Enter(Object sender, System.EventArgs e) {  
       textBox1.SelectionStart = 0;  
       textBox1.SelectionLength = 0;  
    }  
    ```  
  
    ```cpp  
    private:  
       void textBox1_Enter(System::Object ^  sender,  
          System::EventArgs ^  e)  
       {  
          textBox1->SelectionStart = 0;  
          textBox1->SelectionLength = 0;  
       }  
    ```  
  
## <a name="making-the-insertion-point-visible-by-default"></a>既定で挿入ポイントを表示する  
 <xref:System.Windows.Forms.TextBox> の挿入ポイントは、既定では、<xref:System.Windows.Forms.TextBox> コントロールがタブオーダーの最初の位置にある場合にのみ、新しいフォームに表示されます。 そうしないと、キーボードまたはマウスを使用して <xref:System.Windows.Forms.TextBox> にフォーカスを移す場合にのみ、挿入ポイントが表示されます。  
  
#### <a name="to-make-the-text-box-insertion-point-visible-by-default-on-a-new-form"></a>テキストボックスの挿入ポイントが既定で新しいフォームに表示されるようにするには  
  
- <xref:System.Windows.Forms.TextBox> コントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> プロパティを `0`に設定します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TextBox>
- [TextBox コントロールの概要](textbox-control-overview-windows-forms.md)
- [方法: Windows フォームの TextBox コントロールを使用してパスワード テキスト ボックスを作成する](how-to-create-a-password-text-box-with-the-windows-forms-textbox-control.md)
- [方法: 読み取り専用テキスト ボックスを作成する](how-to-create-a-read-only-text-box-windows-forms.md)
- [方法: 文字列に引用符を挿入する](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでテキストを選択する](how-to-select-text-in-the-windows-forms-textbox-control.md)
- [方法: Windows フォーム TextBox コントロールで複数行を表示する](how-to-view-multiple-lines-in-the-windows-forms-textbox-control.md)
- [TextBox コントロール](textbox-control-windows-forms.md)
