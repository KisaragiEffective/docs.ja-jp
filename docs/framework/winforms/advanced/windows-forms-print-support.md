---
title: 印刷のサポート
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, printing
- printing [Windows Forms]
- forms [Windows Forms], printing (using designer)
- printing [Windows Forms], Windows Forms, support
- printing [Windows Forms], print support
ms.assetid: a4a2960c-eb70-48e2-b641-cfb222704e46
ms.openlocfilehash: d6e8f60db7afe2f1b04eaae6fe71aa93e5c22cae
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732493"
---
# <a name="windows-forms-print-support"></a>Windows フォームにおける印刷のサポート
Windows フォームでの印刷は、主に[PrintDocument コンポーネント](../controls/printdocument-component-windows-forms.md)コンポーネントを使用してユーザーを印刷できるようにし、 [printPageSetupDialog dialog Control](../controls/printpreviewdialog-control-windows-forms.md)コントロール、 [PrintDialog コンポーネント](../controls/printdialog-component-windows-forms.md)、および[コンポーネント](../controls/pagesetupdialog-component-windows-forms.md)コンポーネントを使用して、Windows オペレーティングシステムに慣れているユーザーに使い慣れたグラフィカルインターフェイスを提供します。  
  
 通常は、<xref:System.Drawing.Printing.PrintDocument> コンポーネントの新しいインスタンスを作成し、<xref:System.Drawing.Printing.PrinterSettings> クラスと <xref:System.Drawing.Printing.PageSettings> クラスを使用して、印刷する内容を示すプロパティを設定します。次に、<xref:System.Drawing.Printing.PrintDocument.Print%2A> メソッドを呼び出してドキュメントを実際に印刷します。  
  
 Windows ベースのアプリケーションから印刷する過程で、<xref:System.Drawing.Printing.PrintDocument> コンポーネントには、印刷が行われていること、および印刷ジョブをキャンセルできることをユーザーに通知する [中止印刷] ダイアログボックスが表示されます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法: 標準の Windows フォーム印刷ジョブを作成する](how-to-create-standard-windows-forms-print-jobs.md)  
 <xref:System.Drawing.Printing.PrintDocument> コンポーネントを使用して Windows フォームから印刷する方法について説明します。  
  
 [方法: 実行時に PrintDialog のユーザー入力をキャプチャする](how-to-capture-user-input-from-a-printdialog-at-run-time.md)  
 <xref:System.Windows.Forms.PrintDialog> コンポーネントを使用して、選択した印刷オプションをプログラムで変更する方法について説明します。  
  
 [方法: Windows フォームでユーザーのコンピューターに接続されたプリンターを選択する](how-to-choose-the-printers-attached-to-user-computer-in-windows-forms.md)  
 実行時に <xref:System.Windows.Forms.PrintDialog> コンポーネントを使用して印刷するようにプリンターを変更する方法について説明します。  
  
 [方法: Windows フォームでグラフィックスを印刷する](how-to-print-graphics-in-windows-forms.md)  
 プリンターへのグラフィックスの送信について説明します。  
  
 [方法: Windows フォームで複数ページのテキスト ファイルを印刷する](how-to-print-a-multi-page-text-file-in-windows-forms.md)  
 プリンターにテキストを送信する方法について説明します。  
  
 [方法: Windows フォームの印刷ジョブを完了する](how-to-complete-windows-forms-print-jobs.md)  
 印刷ジョブの完了をユーザーに通知する方法について説明します。  
  
 [方法: Windows フォームを印刷する](how-to-print-a-windows-form.md)  
 現在のフォームのコピーを印刷する方法について説明します。  
  
 [方法: Windows フォームで印刷プレビューを使用して印刷する](how-to-print-in-windows-forms-using-print-preview.md)  
 ドキュメントの印刷に <xref:System.Windows.Forms.PrintPreviewDialog> を使用する方法について説明します。  
  
## <a name="related-sections"></a>関連項目  
 [PrintDocument コンポーネント](../controls/printdocument-component-windows-forms.md)  
 <xref:System.Drawing.Printing.PrintDocument> コンポーネントの使用方法について説明します。  
  
 [PrintDialog コンポーネント](../controls/printdialog-component-windows-forms.md)  
 <xref:System.Windows.Forms.PrintDialog> コンポーネントの使用方法について説明します。  
  
 [PrintPreviewDialog コントロール](../controls/printpreviewdialog-control-windows-forms.md)  
 <xref:System.Windows.Forms.PrintPreviewDialog> コントロールの使用方法について説明します。  
  
 [PageSetupDialog コンポーネント](../controls/pagesetupdialog-component-windows-forms.md)  
 <xref:System.Windows.Forms.PageSetupDialog> コンポーネントの使用方法について説明します。  
  
 <xref:System.Drawing.Printing>  
 <xref:System.Drawing.Printing> 名前空間のクラスについて説明します。
