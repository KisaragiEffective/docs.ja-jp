---
title: '[キャンセル] ボタンとしてボタンを指定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- buttons [Windows Forms], cancel buttons
- Button control [Windows Forms], designating as cancel button
ms.assetid: 252f0834-e54b-44d9-96f7-ee5f50e94f2c
ms.openlocfilehash: 123b3e275065efadd24815320ea7d855808e60b9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743260"
---
# <a name="how-to-designate-a-windows-forms-button-as-the-cancel-button"></a>方法 : Windows フォームの Button コントロールをキャンセル ボタンとして指定する
任意の Windows フォームで、[キャンセル] ボタンとして <xref:System.Windows.Forms.Button> コントロールを指定できます。 フォーム上の他のコントロールにフォーカスがあるかどうかに関係なく、ユーザーが ESC キーを押すたびに [キャンセル] ボタンがクリックされます。 このようなボタンは、通常、ユーザーがアクションをコミットせずに操作をすばやく終了できるようにプログラミングされています。  
  
### <a name="to-designate-the-cancel-button"></a>[キャンセル] ボタンを指定するには  
  
1. フォームの <xref:System.Windows.Forms.Form.CancelButton%2A> プロパティを適切な <xref:System.Windows.Forms.Button> コントロールに設定します。  
  
    ```vb  
    Private Sub SetCancelButton(ByVal myCancelBtn As Button)  
       Me.CancelButton = myCancelBtn  
    End Sub  
    ```  
  
    ```csharp  
    private void SetCancelButton(Button myCancelBtn)  
    {  
       this.CancelButton = myCancelBtn;  
    }  
    ```  
  
    ```cpp  
    private:  
       void SetCancelButton(Button ^ myCancelBtn)  
       {  
          this->CancelButton = myCancelBtn;  
       }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Form.CancelButton%2A>
- [Button コントロールの概要](button-control-overview-windows-forms.md)
- [Windows フォームの Button コントロールを選択する方法](ways-to-select-a-windows-forms-button-control.md)
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [方法: Windows フォームの Button コントロールを承認ボタンとして指定する](how-to-designate-a-windows-forms-button-as-the-accept-button.md)
- [Button コントロール](button-control-windows-forms.md)
