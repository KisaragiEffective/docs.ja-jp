---
title: Button のクリックに応答する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- buttons [Windows Forms], responding to Click events
- events [Windows Forms], Click events
- Click event [Windows Forms], Button control
- MouseDown event
- Button control [Windows Forms], click response
- double-clicks
- examples [Windows Forms], controls
- Click event [Windows Forms], responding to
ms.assetid: 7a4951bd-369c-4662-b246-28ad83eda484
ms.openlocfilehash: dd6cf75a316257c86a23b44a818422336c12aa67
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735717"
---
# <a name="how-to-respond-to-windows-forms-button-clicks"></a>方法 : Windows フォームのボタンのクリックに応答する
Windows フォーム <xref:System.Windows.Forms.Button> コントロールの最も基本的な用途は、ボタンがクリックされたときにコードを実行することです。  
  
 <xref:System.Windows.Forms.Button> コントロールをクリックすると、<xref:System.Windows.Forms.Control.MouseEnter>、<xref:System.Windows.Forms.Control.MouseDown>、<xref:System.Windows.Forms.Control.MouseUp> イベントなど、その他の多数のイベントも生成されます。 これらの関連イベントにイベントハンドラーをアタッチする場合は、それらのアクションが競合していないことを確認してください。 たとえば、ボタンをクリックすると、ユーザーがテキストボックスに入力した情報がクリアされた場合、ボタンの上にマウスポインターを置くと、その時点で存在しない情報を示すツールヒントが表示されません。  
  
 ユーザーが <xref:System.Windows.Forms.Button> コントロールをダブルクリックすると、各クリックが個別に処理されます。つまり、コントロールはダブルクリックイベントをサポートしていません。  
  
### <a name="to-respond-to-a-button-click"></a>ボタンのクリックに応答するには  
  
- ボタンの `Click` で、実行するコードを記述 <xref:System.EventHandler> ます。 `Button1_Click` はコントロールにバインドされている必要があります。 詳細については、「[方法: Windows フォームの実行時にイベントハンドラーを作成](../how-to-create-event-handlers-at-run-time-for-windows-forms.md)する」を参照してください。  
  
    ```vb  
    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click  
       MessageBox.Show("Button1 was clicked")  
    End Sub  
    ```  
  
    ```csharp  
    private void button1_Click(object sender, System.EventArgs e)  
    {  
       MessageBox.Show("button1 was clicked");  
    }  
    ```  
  
    ```cpp  
    private:  
       void button1_Click(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          MessageBox::Show("button1 was clicked");  
       }  
    ```  
  
## <a name="see-also"></a>参照

- [Button コントロールの概要](button-control-overview-windows-forms.md)
- [Windows フォームの Button コントロールを選択する方法](ways-to-select-a-windows-forms-button-control.md)
- [Button コントロール](button-control-windows-forms.md)
