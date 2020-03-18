---
title: '方法: ユーザーのコンピューターに接続されているプリンターを選択する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- printing [Windows Forms], choosing printers
- printers [Windows Forms], choosing
ms.assetid: 63c1172b-2931-4ac0-953f-37f629494bbf
ms.openlocfilehash: 7fc2427468540ac0a1480f6140cbb34c3a0f1ab3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746511"
---
# <a name="how-to-choose-the-printers-attached-to-a-users-computer-in-windows-forms"></a>方法 : Windows フォームでユーザーのコンピューターに接続されたプリンターを選択する
既定のプリンター以外のプリンターに印刷することがよくあります。 <xref:System.Windows.Forms.PrintDialog> コンポーネントを使用すると、現在インストールされているプリンターからユーザーに選択させることができます。 <xref:System.Windows.Forms.PrintDialog> コンポーネントでは、 <xref:System.Windows.Forms.DialogResult> コンポーネントの <xref:System.Windows.Forms.PrintDialog> がキャプチャされ、プリンターの選択に使用されます。  
  
 次の手順では、既定のプリンターに印刷するテキスト ファイルを選択します。 <xref:System.Windows.Forms.PrintDialog> クラスがインスタンス化されます。  
  
### <a name="to-choose-a-printer-and-then-print-a-file"></a>プリンターを選択してファイルを印刷するには  
  
1. <xref:System.Windows.Forms.PrintDialog> コンポーネントを使用して、使用するプリンターを選択します。  
  
     次のコード例では、2 つのイベントを処理しています。 最初の <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントでは、<xref:System.Windows.Forms.PrintDialog> クラスがインスタンス化され、ユーザーが選択したプリンターが <xref:System.Windows.Forms.DialogResult> プロパティにキャプチャされます。  
  
     2番目のイベントでは、<xref:System.Drawing.Printing.PrintDocument> コンポーネントの <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントで、指定されたプリンターにサンプルドキュメントが出力されます。  
  
    ```vb  
    Private Sub Button1_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles Button1.Click  
       Dim PrintDialog1 As New PrintDialog()  
       PrintDialog1.Document = PrintDocument1  
       Dim result As DialogResult = PrintDialog1.ShowDialog()  
  
       If (result = DialogResult.OK) Then  
         PrintDocument1.Print()  
       End If   
  
    End Sub  
  
    Private Sub PrintDocument1_PrintPage(ByVal sender As Object, ByVal e As System.Drawing.Printing.PrintPageEventArgs) Handles PrintDocument1.PrintPage  
       e.Graphics.FillRectangle(Brushes.Red, New Rectangle(500, 500, 500, 500))          
    End Sub  
    ```  
  
    ```csharp  
    private void button1_Click(object sender, System.EventArgs e)  
    {  
       PrintDialog printDialog1 = new PrintDialog();  
       printDialog1.Document = printDocument1;  
       DialogResult result = printDialog1.ShowDialog();  
       if (result == DialogResult.OK)  
       {  
          printDocument1.Print();  
       }  
    }  
  
    private void printDocument1_PrintPage(object sender,   
    System.Drawing.Printing.PrintPageEventArgs e)  
    {  
       e.Graphics.FillRectangle(Brushes.Red,   
         new Rectangle(500, 500, 500, 500));  
    }  
    ```  
  
    ```cpp  
    private:  
       void button1_Click(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          PrintDialog ^ printDialog1 = gcnew PrintDialog();  
          printDialog1->Document = printDocument1;  
          System::Windows::Forms::DialogResult result =   
             printDialog1->ShowDialog();  
          if (result == DialogResult::OK)  
          {  
             printDocument1->Print();  
          }  
       }  
    private:  
       void printDocument1_PrintPage(System::Object ^ sender,  
          System::Drawing::Printing::PrintPageEventArgs ^ e)  
       {  
          e->Graphics->FillRectangle(Brushes::Red,  
             Rectangle(500, 500, 500, 500));  
       }  
    ```  
  
     (ビジュアルC#とビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.printDocument1.PrintPage += new  
       System.Drawing.Printing.PrintPageEventHandler  
       (this.printDocument1_PrintPage);  
    this.button1.Click += new System.EventHandler(this.button1_Click);  
    ```  
  
    ```cpp  
    this->printDocument1->PrintPage += gcnew  
       System::Drawing::Printing::PrintPageEventHandler  
       (this, &Form1::printDocument1_PrintPage);  
    this->button1->Click += gcnew  
       System::EventHandler(this, &Form1::button1_Click);  
    ```  
  
## <a name="see-also"></a>参照

- [Windows フォームにおける印刷のサポート](windows-forms-print-support.md)
