---
title: 印刷プレビューを使用して印刷する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- printing [Windows Forms], using print preview
- printing [Windows Forms], with print preview
- print preview
ms.assetid: 4a16f7e2-ae10-4485-b0ae-3d558334d0fe
ms.openlocfilehash: 1975c902fdb56326c763f2e2fc11e381ffc7fbd3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740609"
---
# <a name="how-to-print-in-windows-forms-using-print-preview"></a>方法 : Windows フォームで印刷プレビューを使用して印刷する
Windows フォームのプログラミングでは、印刷サービスに加えて印刷プレビューを提供することは非常に一般的です。 印刷プレビューのサービスをアプリケーションに追加する簡単な方法は、ファイルの印刷に <xref:System.Windows.Forms.PrintPreviewDialog> コントロールを <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベント処理ロジックと組み合わせて使用することです。  
  
### <a name="to-preview-a-text-document-with-a-printpreviewdialog-control"></a>PrintPreviewDialog コントロールのテキスト ドキュメントをプレビューするには  
  
1. フォームに <xref:System.Windows.Forms.PrintPreviewDialog>、 <xref:System.Drawing.Printing.PrintDocument>、および 2 つの文字列を追加します。  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#1)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#1)]  
  
2. <xref:System.Drawing.Printing.PrintDocument.DocumentName%2A> プロパティを印刷するドキュメントに設定し、ドキュメントの内容を開いて前に追加した文字列に読み取ります。  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#2)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#2)]  
  
3. ドキュメントの印刷の場合と同様、 <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベント ハンドラーで <xref:System.Drawing.Printing.PrintPageEventArgs.Graphics%2A> クラスの <xref:System.Drawing.Printing.PrintPageEventArgs> プロパティと、ファイルの内容を使用して、1 ページあたりの行数を計算し、ドキュメントの内容を表示します。 各ページを描画した後で、最後のページかどうかを確認し、 <xref:System.Drawing.Printing.PrintPageEventArgs.HasMorePages%2A> の <xref:System.Drawing.Printing.PrintPageEventArgs> プロパティを適切に設定します。 <xref:System.Drawing.Printing.PrintDocument.PrintPage> が <xref:System.Drawing.Printing.PrintPageEventArgs.HasMorePages%2A> になるまで `false`イベントが発生します。 ドキュメントの表示が完了したら、表示される文字列をリセットします。 また、 <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントがイベント処理メソッドに関連付けられていることを確認します。  
  
    > [!NOTE]
    > アプリケーションに印刷を実装した場合は、既に手順 2 および 3 を完了している可能性があります。  
  
     次のコード例では、イベント ハンドラーが、フォームで使用されているものと同じフォントで "testPage.txt" ファイルの内容を印刷するために使用されます。  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#3)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#3)]  
  
4. <xref:System.Windows.Forms.PrintPreviewDialog.Document%2A> コントロールの <xref:System.Windows.Forms.PrintPreviewDialog> のプロパティをフォームの <xref:System.Drawing.Printing.PrintDocument> コンポーネントに設定します。  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#5)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#5)]  
  
5. <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> コントロールの <xref:System.Windows.Forms.PrintPreviewDialog> メソッドを呼び出します。 通常はボタンの <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> イベント処理メソッドから <xref:System.Windows.Forms.Control.Click> を呼び出します。 <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> を呼び出すことで <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントが発生し、 <xref:System.Windows.Forms.PrintPreviewDialog> コントロールに出力を表示します。 ユーザーが、ダイアログの印刷アイコンをクリックすると、 <xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントがもう一度発生し、プレビュー ダイアログではなく、プリンターに出力を送信します。 これが理由で、手順 3 のレンダリング プロセスの最後に文字列がリセットされます。  
  
     次のコード例は、フォームのボタンの <xref:System.Windows.Forms.Control.Click> イベント処理メソッドを示します。 このイベント処理メソッドは、ドキュメントを読み込んで、印刷プレビューのダイアログを表示するメソッドを呼び出します。  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#4)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#4)]  
  
## <a name="example"></a>例  
 [!code-csharp[System.Drawing.Printing.PrintPreviewExample#0](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#0)]
 [!code-vb[System.Drawing.Printing.PrintPreviewExample#0](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#0)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System、System.Windows.Forms、System.Drawing の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- [方法: Windows フォームで複数ページのテキスト ファイルを印刷する](how-to-print-a-multi-page-text-file-in-windows-forms.md)
- [Windows フォームにおける印刷のサポート](windows-forms-print-support.md)
- [Windows フォームでのより安全な印刷](../more-secure-printing-in-windows-forms.md)
