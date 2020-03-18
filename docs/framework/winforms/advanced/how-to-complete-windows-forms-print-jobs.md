---
title: 印刷ジョブの完了
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- print jobs [Windows Forms], completing in Windows Forms
- printing [Windows Forms], print jobs
ms.assetid: 23ec74f7-34c5-4710-82a0-ee2914518548
ms.openlocfilehash: b8ef4fa05b2107247181e82b72389f9503507135
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746498"
---
# <a name="how-to-complete-windows-forms-print-jobs"></a>方法 : Windows フォームの印刷ジョブを完了する
多くの場合、印刷を伴うワードプロセッサやその他のアプリケーションには、印刷ジョブが完了したことをユーザーに示すオプションが用意されています。 <xref:System.Drawing.Printing.PrintDocument> コンポーネントの <xref:System.Drawing.Printing.PrintDocument.EndPrint> イベントを処理することによって、この機能を Windows フォームに提供できます。  
  
 次の手順では、<xref:System.Drawing.Printing.PrintDocument> コンポーネントを含む Windows ベースのアプリケーションを作成しておく必要があります。これは、Windows ベースのアプリケーションからの印刷を有効にするための標準的な方法です。 <xref:System.Drawing.Printing.PrintDocument> コンポーネントを使用した Windows フォームからの印刷の詳細については、「[方法: 標準 Windows フォーム印刷ジョブを作成する](how-to-create-standard-windows-forms-print-jobs.md)」を参照してください。  
  
### <a name="to-complete-a-print-job"></a>印刷ジョブを完了するには  
  
1. <xref:System.Drawing.Printing.PrintDocument> コンポーネントの <xref:System.Drawing.Printing.PrintDocument.DocumentName%2A> プロパティを設定します。  
  
    ```vb  
    PrintDocument1.DocumentName = "MyTextFile"  
    ```  
  
    ```csharp  
    printDocument1.DocumentName = "MyTextFile";  
    ```  
  
    ```cpp  
    printDocument1->DocumentName = "MyTextFile";  
    ```  
  
2. <xref:System.Drawing.Printing.PrintDocument.EndPrint> イベントを処理するコードを記述します。  
  
     次のコード例では、ドキュメントの印刷が完了したことを示すメッセージボックスが表示されます。  
  
    ```vb  
    Private Sub PrintDocument1_EndPrint(ByVal sender As Object, ByVal e As System.Drawing.Printing.PrintEventArgs) Handles PrintDocument1.EndPrint  
       MessageBox.Show(PrintDocument1.DocumentName + " has finished printing.")  
    End Sub  
    ```  
  
    ```csharp  
    private void printDocument1_EndPrint(object sender,   
    System.Drawing.Printing.PrintEventArgs e)  
    {  
       MessageBox.Show(printDocument1.DocumentName +   
          " has finished printing.");  
    }  
    ```  
  
    ```cpp  
    private:  
       void printDocument1_EndPrint(System::Object ^ sender,  
          System::Drawing::Printing::PrintEventArgs ^ e)  
       {  
          MessageBox::Show(String::Concat(printDocument1->DocumentName,  
             " has finished printing."));  
       }  
    ```  
  
     (ビジュアルC#とビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.printDocument1.EndPrint += new  
       System.Drawing.Printing.PrintEventHandler  
       (this.printDocument1_EndPrint);  
    ```  
  
    ```cpp  
    this->printDocument1->EndPrint += gcnew  
       System::Drawing::Printing::PrintEventHandler  
       (this, &Form1::printDocument1_EndPrint);  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Drawing.Printing.PrintDocument>
- [Windows フォームにおける印刷のサポート](windows-forms-print-support.md)
