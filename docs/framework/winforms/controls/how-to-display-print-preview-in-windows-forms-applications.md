---
title: Windows フォームアプリで印刷プレビューを表示する
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- print preview [Windows Forms], displaying
- printing [Windows Forms], print preview
- examples [Windows Forms], print preview
ms.assetid: e394134c-0886-4517-bd8d-edc4a3749eb5
ms.openlocfilehash: ac02339ad86e491cd047dcd4b0c8841374b3bb2e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745568"
---
# <a name="how-to-display-print-preview-in-windows-forms-applications"></a>方法 : Windows フォーム アプリケーションに印刷プレビューを表示する
<xref:System.Windows.Forms.PrintPreviewDialog> コントロールを使用して、ユーザーが文書を印刷する前に頻繁に表示できるようにすることができます。  
  
 これを行うには、<xref:System.Drawing.Printing.PrintDocument> クラスのインスタンスを指定する必要があります。これは、印刷するドキュメントです。 <xref:System.Drawing.Printing.PrintDocument> コンポーネントで印刷プレビューを使用する方法の詳細については、「印刷[プレビューを使用して Windows フォームを印刷する方法](../advanced/how-to-print-in-windows-forms-using-print-preview.md)」を参照してください。  
  
> [!NOTE]
> 実行時に <xref:System.Windows.Forms.PrintPreviewDialog> コントロールを使用するには、ローカルまたはネットワーク経由でコンピューターにプリンターがインストールされている必要があります。これは、印刷時のドキュメントの外観を <xref:System.Windows.Forms.PrintPreviewDialog> コンポーネントが決定する方法の一部であるためです。  
  
 <xref:System.Windows.Forms.PrintPreviewDialog> コントロールは、<xref:System.Drawing.Printing.PrinterSettings> クラスを使用します。 さらに、<xref:System.Windows.Forms.PrintPreviewDialog> コントロールは、<xref:System.Windows.Forms.PrintPreviewDialog> コンポーネントと同様に、<xref:System.Drawing.Printing.PageSettings> クラスを使用します。 <xref:System.Windows.Forms.PrintPreviewDialog> コントロールの <xref:System.Windows.Forms.PrintPreviewControl.Document%2A> プロパティで指定された印刷ドキュメントは、<xref:System.Drawing.Printing.PrinterSettings> クラスと <xref:System.Drawing.Printing.PageSettings> クラスの両方のインスタンスを参照します。これらは、ドキュメントをプレビューウィンドウに表示するために使用されます。  
  
### <a name="to-view-pages-using-the-printpreviewdialog-control"></a>Printプレビューダイアログコントロールを使用してページを表示するには  
  
- <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用してダイアログ ボックスを表示し、使用する <xref:System.Drawing.Printing.PrintDocument> を指定します。  
  
     次のコード例では、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントハンドラーが <xref:System.Windows.Forms.PrintPreviewDialog> コントロールのインスタンスを開きます。 印刷ドキュメントは、<xref:System.Windows.Forms.PrintDialog.Document%2A> プロパティで指定されます。 次の例では、印刷ドキュメントが指定されていません。  
  
     この例では、フォームに <xref:System.Windows.Forms.Button> コントロール、`myDocument`という名前の <xref:System.Drawing.Printing.PrintDocument> コンポーネント、および <xref:System.Windows.Forms.PrintPreviewDialog> コントロールが必要です。  
  
    ```vb  
    Private Sub Button1_Click(ByVal sender As System.Object, _  
       ByVal e As System.EventArgs) Handles Button1.Click  
       ' The print document 'myDocument' used below  
       ' is merely for an example.  
       ' You will have to specify your own print document.  
       PrintPreviewDialog1.Document = myDocument  
       PrintPreviewDialog1.ShowDialog()  
    End Sub  
    ```  
  
    ```csharp  
    private void button1_Click(object sender, System.EventArgs e)  
    {  
       // The print document 'myDocument' used below  
       // is merely for an example.  
       // You will have to specify your own print document.  
       printPreviewDialog1.Document = myDocument;  
       printPreviewDialog1.ShowDialog();  
    }  
    ```  
  
    ```cpp  
    private:  
       void button1_Click(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          // The print document 'myDocument' used below  
          // is merely for an example.  
          // You will have to specify your own print document.  
          printPreviewDialog1->Document = myDocument;  
          printPreviewDialog1->ShowDialog();  
       }  
    ```  
  
     (ビジュアルC#、ビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.button1.Click += new System.EventHandler(this.button1_Click);  
    ```  
  
    ```cpp  
    this->button1->Click += gcnew  
       System::EventHandler(this, &Form1::button1_Click);  
    ```  
  
## <a name="see-also"></a>参照

- [PrintDocument コンポーネント](printdocument-component-windows-forms.md)
- [PrintPreviewDialog コントロール](printpreviewdialog-control-windows-forms.md)
- [Windows フォームにおける印刷のサポート](../advanced/windows-forms-print-support.md)
- [Windows フォーム](../index.md)
