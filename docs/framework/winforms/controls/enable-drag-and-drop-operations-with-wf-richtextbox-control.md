---
title: RichTextBox コントロールでドラッグアンドドロップ操作を有効にする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], text boxes
- drag and drop [Windows Forms], richTextBox control
- text boxes [Windows Forms], drag-and-drop operations
- RichTextBox control [Windows Forms], drag-and-drop operations
ms.assetid: ca167d1c-2014-4cf0-96a0-20598470be3b
ms.openlocfilehash: 3c17560dee012912aea2938654f1dc4dc56e0725
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745825"
---
# <a name="how-to-enable-drag-and-drop-operations-with-the-windows-forms-richtextbox-control"></a>方法 : Windows フォームの RichTextBox コントロールにおけるドラッグ アンド ドロップ操作を有効にする
Windows フォームでのドラッグ アンド ドロップ操作の <xref:System.Windows.Forms.RichTextBox> コントロールは、 <xref:System.Windows.Forms.RichTextBox.DragEnter> および <xref:System.Windows.Forms.RichTextBox.DragDrop> イベントを処理すると実行されます。 そのため、ドラッグ アンド ドロップの操作は <xref:System.Windows.Forms.RichTextBox> コントロールを使用すると非常にシンプルです。  
  
### <a name="to-enable-drag-operations-in-a-richtextbox-control"></a>RichTextBox コントロールでドラッグ操作を有効にするには  
  
1. <xref:System.Windows.Forms.RichTextBox.AllowDrop%2A> プロパティの <xref:System.Windows.Forms.RichTextBox> コントロールを `true`に設定します。  
  
2. <xref:System.Windows.Forms.RichTextBox.DragEnter> イベントのイベント ハンドラーで適切なコードを作成します。 `if` ステートメントを使用して、ドラッグされるデータが適切な型 (この場合はテキスト) であることを確認します。 <xref:System.Windows.Forms.DragEventArgs.Effect%2A?displayProperty=nameWithType> プロパティは、いずれかの <xref:System.Windows.Forms.DragDropEffects> 列挙値に設定できます。  
  
    ```vb  
    Private Sub RichTextBox1_DragEnter(ByVal sender As Object, _   
       ByVal e As System.Windows.Forms.DragEventArgs) _   
       Handles RichTextBox1.DragEnter  
       If (e.Data.GetDataPresent(DataFormats.Text)) Then  
          e.Effect = DragDropEffects.Copy  
       Else  
          e.Effect = DragDropEffects.None  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void richTextBox1_DragEnter(object sender,   
    System.Windows.Forms.DragEventArgs e)  
    {  
       if (e.Data.GetDataPresent(DataFormats.Text))   
          e.Effect = DragDropEffects.Copy;  
       else  
          e.Effect = DragDropEffects.None;  
    }  
    ```  
  
    ```cpp  
    private:  
       void richTextBox1_DragEnter(System::Object ^  sender,  
          System::Windows::Forms::DragEventArgs ^  e)  
       {  
          if (e->Data->GetDataPresent(DataFormats::Text))  
             e->Effect = DragDropEffects::Copy;  
          else  
             e->Effect = DragDropEffects::None;  
       }  
    ```  
  
     (ビジュアルC#とビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.richTextBox1.DragEnter += new  
        System.Windows.Forms.DragEventHandler  
        (this.richTextBox1_DragEnter);  
    ```  
  
    ```cpp  
    this->richTextBox1->DragEnter += gcnew  
       System::Windows::Forms::DragEventHandler  
       (this, &Form1::richTextBox1_DragEnter);  
    ```  
  
3. <xref:System.Windows.Forms.RichTextBox.DragDrop> イベントを処理するコードを記述します。 <xref:System.Windows.Forms.DataObject.GetData%2A?displayProperty=nameWithType> メソッドを使用してドラッグされるデータを取得します。  
  
     次の例では、コードは、ドラッグされるデータに相当する <xref:System.Windows.Forms.RichTextBox.Text%2A> コントロールの <xref:System.Windows.Forms.RichTextBox> のプロパティを設定します。 <xref:System.Windows.Forms.RichTextBox> コントロール内にすでにテキストがある場合、ドラッグされたデータが挿入ポイントに挿入されます。  
  
    ```vb  
    Private Sub RichTextBox1_DragDrop(ByVal sender As Object, _   
       ByVal e As System.Windows.Forms.DragEventArgs) _   
       Handles RichTextBox1.DragDrop  
       Dim i As Int16   
       Dim s As String  
  
       ' Get start position to drop the text.  
       i = RichTextBox1.SelectionStart  
       s = RichTextBox1.Text.Substring(i)  
       RichTextBox1.Text = RichTextBox1.Text.Substring(0, i)  
  
       ' Drop the text on to the RichTextBox.  
       RichTextBox1.Text = RichTextBox1.Text + _  
          e.Data.GetData(DataFormats.Text).ToString()  
       RichTextBox1.Text = RichTextBox1.Text + s  
    End Sub  
    ```  
  
    ```csharp  
    private void richTextBox1_DragDrop(object sender,   
    System.Windows.Forms.DragEventArgs e)  
    {  
       int i;  
       String s;  
  
       // Get start position to drop the text.  
       i = richTextBox1.SelectionStart;  
       s = richTextBox1.Text.Substring(i);  
       richTextBox1.Text = richTextBox1.Text.Substring(0,i);  
  
       // Drop the text on to the RichTextBox.  
       richTextBox1.Text = richTextBox1.Text +   
          e.Data.GetData(DataFormats.Text).ToString();  
       richTextBox1.Text = richTextBox1.Text + s;  
    }  
    ```  
  
    ```cpp  
    private:  
       System::Void richTextBox1_DragDrop(System::Object ^  sender,  
          System::Windows::Forms::DragEventArgs ^  e)  
       {  
          int i;  
          String ^s;  
  
       // Get start position to drop the text.  
       i = richTextBox1->SelectionStart;  
       s = richTextBox1->Text->Substring(i);  
       richTextBox1->Text = richTextBox1->Text->Substring(0,i);  
  
       // Drop the text on to the RichTextBox.  
       String ^str = String::Concat(richTextBox1->Text, e->Data  
       ->GetData(DataFormats->Text)->ToString());   
       richTextBox1->Text = String::Concat(str, s);  
       }  
    ```  
  
     (ビジュアルC#とビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.richTextBox1.DragDrop += new  
        System.Windows.Forms.DragEventHandler  
        (this.richTextBox1_DragDrop);  
    ```  
  
    ```cpp  
    this->richTextBox1->DragDrop += gcnew   
       System::Windows::Forms::DragEventHandler  
       (this, &Form1::richTextBox1_DragDrop);  
    ```  
  
### <a name="to-test-the-drag-and-drop-functionality-in-your-application"></a>アプリケーションでドラッグ アンド ドロップ機能をテストするには  
  
1. アプリケーションを保存し、ビルドします。 実行中にワードパッドを起動します。  
  
     ワードパッドは、Windows によってインストールされるテキスト エディターであり、ドラッグ アンド ドロップ操作をサポートします。 ワードパッドを起動するには、 **[スタート]** ボタンをクリックし、 **[ファイル名を指定して実行]** を選択し、 `WordPad` [ファイル名を指定して実行] **ダイアログ ボックスのテキスト ボックスに「** 」と入力し、 **[OK]** をクリックします。  
  
2. ワードパッドが起動したら、テキストの文字列を入力します。 マウスを使用してテキストを選択し、選択したテキストを Windows アプリケーションの <xref:System.Windows.Forms.RichTextBox> コントロールにドラッグします。  
  
     マウスを <xref:System.Windows.Forms.RichTextBox> コントロールに移動すると (そして、その結果 <xref:System.Windows.Forms.RichTextBox.DragEnter> イベントが発生すると)、マウス ポインターの形が変化し、選択したテキストを <xref:System.Windows.Forms.RichTextBox> コントロールにドロップできます。  
  
     マウス ボタンを放すと、選択したテキストがドロップ (つまり <xref:System.Windows.Forms.RichTextBox.DragDrop> イベントが発生) し、 <xref:System.Windows.Forms.RichTextBox> コントロール内に挿入されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBox>
- [方法: アプリケーション間でドラッグ アンド ドロップ操作を実行する](../advanced/how-to-perform-drag-and-drop-operations-between-applications.md)
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
