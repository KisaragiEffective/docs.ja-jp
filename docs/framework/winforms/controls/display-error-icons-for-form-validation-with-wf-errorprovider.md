---
title: ErrorProvider コンポーネントを使用したフォーム検証のエラーアイコンの表示
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- errors [Windows Forms], displaying to users
- error icons
- ErrorProvider component [Windows Forms], displaying error icons
- error messages [Windows Forms], displaying icons
ms.assetid: 3b681a32-9db4-497b-a34b-34980eabee46
ms.openlocfilehash: a1e346e332db489351f59c9a0c03ae731baf3dc3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745914"
---
# <a name="how-to-display-error-icons-for-form-validation-with-the-windows-forms-errorprovider-component"></a>方法 : Windows フォーム ErrorProvider コンポーネントを使用してフォーム妥当性検査でエラー アイコンを表示する
Windows フォーム <xref:System.Windows.Forms.ErrorProvider> コンポーネントを使用して、ユーザーが無効なデータを入力したときにエラーアイコンが表示されるようにすることができます。 フォームの間にタブを表示するには、フォーム上に少なくとも2つのコントロールが必要であるため、検証コードを呼び出す必要があります。  
  
### <a name="to-display-an-error-icon-when-a-controls-value-is-invalid"></a>コントロールの値が無効なときにエラーアイコンを表示するには  
  
1. 2つのコントロール (たとえば、テキストボックス) を Windows フォームに追加します。  
  
2. フォームに <xref:System.Windows.Forms.ErrorProvider> コンポーネントを追加します。  
  
3. 最初のコントロールを選択し、その <xref:System.Windows.Forms.Control.Validating> イベントハンドラーにコードを追加します。 このコードが正常に実行されるためには、プロシージャがイベントに接続されている必要があります。 詳細については、「[方法: Windows フォームの実行時にイベントハンドラーを作成](../how-to-create-event-handlers-at-run-time-for-windows-forms.md)する」を参照してください。  
  
     次のコードは、ユーザーが入力したデータの有効性をテストします。データが無効な場合は、<xref:System.Windows.Forms.ErrorProvider.SetError%2A> メソッドが呼び出されます。 <xref:System.Windows.Forms.ErrorProvider.SetError%2A> メソッドの最初の引数は、アイコンを表示するコントロールを指定します。 2番目の引数は、表示するエラーテキストです。  
  
    ```vb  
    Private Sub TextBox1_Validating(ByVal Sender As Object, _  
       ByVal e As System.ComponentModel.CancelEventArgs) Handles _  
       TextBox1.Validating  
          If Not IsNumeric(TextBox1.Text) Then  
             ErrorProvider1.SetError(TextBox1, "Not a numeric value.")  
          Else  
             ' Clear the error.  
             ErrorProvider1.SetError(TextBox1, "")  
          End If  
    End Sub  
    ```  
  
    ```csharp  
    protected void textBox1_Validating (object sender,  
       System.ComponentModel.CancelEventArgs e)  
    {  
       try  
       {  
          int x = Int32.Parse(textBox1.Text);  
          errorProvider1.SetError(textBox1, "");  
       }  
       catch (Exception ex)  
       {  
          errorProvider1.SetError(textBox1, "Not an integer value.");  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       System::Void textBox1_Validating(System::Object ^  sender,  
          System::ComponentModel::CancelEventArgs ^  e)  
       {  
          try  
          {  
             int x = Int32::Parse(textBox1->Text);  
             errorProvider1->SetError(textBox1, "");  
          }  
          catch (System::Exception ^ ex)  
          {  
             errorProvider1->SetError(textBox1, "Not an integer value.");  
          }  
       }  
    ```  
  
     (ビジュアルC#、ビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.textBox1.Validating += new  
    System.ComponentModel.CancelEventHandler(this.textBox1_Validating);  
    ```  
  
    ```cpp  
    this->textBox1->Validating += gcnew  
       System::ComponentModel::CancelEventHandler  
       (this, &Form1::textBox1_Validating);  
    ```  
  
4. プロジェクトを実行します。 1つ目のコントロールに無効な (この例では数値以外の) データを入力し、tab キーを押します。 エラーアイコンが表示されたら、マウスポインターをポイントしてエラーテキストを確認します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ErrorProvider.SetError%2A>
- [ErrorProvider コンポーネントの概要](errorprovider-component-overview-windows-forms.md)
- [方法: Windows フォーム ErrorProvider コンポーネントで DataSet 内にエラーを表示する](view-errors-within-a-dataset-with-wf-errorprovider-component.md)
