---
title: TextBox コントロールを使用してパスワードテキストボックスを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- TextBox control [Windows Forms], entering passwords
- password boxes [Windows Forms], creating
- PasswordChar property in text boxes
- passwords [Windows Forms], input mask
- passwords [Windows Forms], password text box
ms.assetid: d105d6b9-3d50-44cd-80d8-2c0e2f486727
ms.openlocfilehash: ff4706a736d15f14cf437c808219e9088773dc6d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731287"
---
# <a name="how-to-create-a-password-text-box-with-the-windows-forms-textbox-control"></a>方法 : Windows フォームの TextBox コントロールを使用してパスワード テキスト ボックスを作成する

パスワードボックスは、ユーザーが文字列を入力したときにプレースホルダー文字を表示する Windows フォームテキストボックスです。

### <a name="to-create-a-password-text-box"></a>パスワードテキストボックスを作成するには

1. <xref:System.Windows.Forms.TextBox> コントロールの <xref:System.Windows.Forms.TextBox.PasswordChar%2A> プロパティを特定の文字に設定します。

    <xref:System.Windows.Forms.TextBox.PasswordChar%2A> プロパティは、テキストボックスに表示される文字を指定します。 たとえば、[パスワード] ボックスにアスタリスクを表示する場合は、プロパティウィンドウの [<xref:System.Windows.Forms.TextBox.PasswordChar%2A>] プロパティに * を指定します。 その後、ユーザーがテキストボックスに入力した文字に関係なく、アスタリスクが表示されます。

2. Optional<xref:System.Windows.Forms.TextBoxBase.MaxLength%2A> プロパティを設定します。 プロパティは、テキストボックスに入力できる文字数を決定します。 最大長を超えた場合、システムはビープ音を出力し、テキストボックスはそれ以上文字を受け付けません。 パスワードを推測しようとしているハッカーにパスワードの最大長が使用される可能性があるため、この操作は行わないことに注意してください。

    次のコード例は、14文字までの文字列を受け取り、文字列の代わりにアスタリスクを表示するテキストボックスを初期化する方法を示しています。 `InitializeMyControl` プロシージャは自動的に実行されません。このメソッドを呼び出す必要があります。

    > [!IMPORTANT]
    > テキストボックスの <xref:System.Windows.Forms.TextBox.PasswordChar%2A> プロパティを使用すると、他のユーザーが入力したユーザーがユーザーのパスワードを確認できないようにすることができます。 このセキュリティ対策では、アプリケーションロジックによって発生する可能性のあるパスワードの格納や転送については説明しません。 入力したテキストは暗号化されていないため、他の機密データと同様に扱う必要があります。 このように表示されない場合でも、パスワードはまだプレーンテキスト文字列として扱われます (追加のセキュリティ対策を実装している場合を除く)。

    ```vb
    Private Sub InitializeMyControl()
       ' Set to no text.
       TextBox1.Text = ""
       ' The password character is an asterisk.
       TextBox1.PasswordChar = "*"
       ' The control will allow no more than 14 characters.
       TextBox1.MaxLength = 14
    End Sub
    ```

    ```csharp
    private void InitializeMyControl()
    {
       // Set to no text.
       textBox1.Text = "";
       // The password character is an asterisk.
       textBox1.PasswordChar = '*';
       // The control will allow no more than 14 characters.
       textBox1.MaxLength = 14;
    }
    ```

    ```cpp
    private:
       void InitializeMyControl()
       {
          // Set to no text.
          textBox1->Text = "";
          // The password character is an asterisk.
          textBox1->PasswordChar = '*';
          // The control will allow no more than 14 characters.
          textBox1->MaxLength = 14;
       }
    ```

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TextBox>
- [TextBox コントロールの概要](textbox-control-overview-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでのカーソル位置を制御する](how-to-control-the-insertion-point-in-a-windows-forms-textbox-control.md)
- [方法: 読み取り専用テキスト ボックスを作成する](how-to-create-a-read-only-text-box-windows-forms.md)
- [方法: 文字列に引用符を挿入する](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [方法: Windows フォーム TextBox コントロールでテキストを選択する](how-to-select-text-in-the-windows-forms-textbox-control.md)
- [方法: Windows フォーム TextBox コントロールで複数行を表示する](how-to-view-multiple-lines-in-the-windows-forms-textbox-control.md)
- [TextBox コントロール](textbox-control-windows-forms.md)
