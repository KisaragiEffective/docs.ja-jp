---
title: 標準の印刷ジョブを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- printing [Windows Forms]
- printing [Windows Forms], creating print jobs
- printing [Visual Basic], in Windows applications
ms.assetid: 03342b90-9cfe-40b2-838b-b479a13c5dea
ms.openlocfilehash: 4850dc901630179cc44fefda7e25bbabcfb4725f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741521"
---
# <a name="how-to-create-standard-windows-forms-print-jobs"></a>方法 : 標準の Windows フォーム印刷ジョブを作成する
Windows フォームでの印刷の基礎は、<xref:System.Drawing.Printing.PrintDocument> コンポーネント (具体的には <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベント) です。 <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントを処理するコードを記述することによって、印刷する内容と印刷方法を指定できます。  
  
### <a name="to-create-a-print-job"></a>印刷ジョブを作成するには  
  
1. フォームに <xref:System.Drawing.Printing.PrintDocument> コンポーネントを追加します。  
  
2. <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントを処理するコードを記述します。  
  
     独自の印刷ロジックをコーディングする必要があります。 また、印刷する素材を指定する必要があります。  
  
     次のコード例では、<xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントハンドラーに赤い四角形の形状のサンプルグラフィックが作成され、印刷する素材として機能します。  
  
    ```vb  
    Private Sub PrintDocument1_PrintPage(ByVal sender As Object, ByVal e As System.Drawing.Printing.PrintPageEventArgs) Handles PrintDocument1.PrintPage  
       e.Graphics.FillRectangle(Brushes.Red, New Rectangle(500, 500, 500, 500))  
    End Sub  
    ```  
  
    ```csharp  
    private void printDocument1_PrintPage(object sender,   
    System.Drawing.Printing.PrintPageEventArgs e)  
    {  
       e.Graphics.FillRectangle(Brushes.Red,   
         new Rectangle(500, 500, 500, 500));  
    }  
    ```  
  
    ```cpp  
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
    ```  
  
    ```cpp  
    printDocument1->PrintPage += gcnew  
       System::Drawing::Printing::PrintPageEventHandler  
       (this, &Form1::printDocument1_PrintPage);  
    ```  
  
     また、<xref:System.Drawing.Printing.PrintDocument.BeginPrint> イベントと <xref:System.Drawing.Printing.PrintDocument.EndPrint> イベントのコードを記述することもできます。たとえば、印刷するページの合計数を表す整数を含めて、各ページが印刷するたびにデクリメントします。  
  
    > [!NOTE]
    > <xref:System.Windows.Forms.PrintDialog> コンポーネントをフォームに追加して、ユーザーに対してクリーンで効率的なユーザーインターフェイス (UI) を提供できます。 <xref:System.Windows.Forms.PrintDialog> コンポーネントの [<xref:System.Windows.Forms.PrintDialog.Document%2A>] プロパティを設定すると、フォーム上で作業している印刷ドキュメントに関連するプロパティを設定できます。 <xref:System.Windows.Forms.PrintDialog> コンポーネントの詳細については、「 [PrintDialog コンポーネント](../controls/printdialog-component-windows-forms.md)」を参照してください。  
  
     プログラムで印刷ジョブを作成する方法など、Windows フォームの印刷ジョブの詳細については、「<xref:System.Drawing.Printing.PrintPageEventArgs>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Drawing.Printing.PrintDocument>
- [Windows フォームにおける印刷のサポート](windows-forms-print-support.md)
