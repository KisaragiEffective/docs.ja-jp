---
title: '方法 : 印刷ダイアログ ボックスを呼び出す'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- invoking print dialogs [WPF]
- print dialogs [WPF], invoking
ms.assetid: e3a2c84c-74fe-45a4-8501-5813f9dbfed2
ms.openlocfilehash: f7ef3312d876324b44b055d70fd22e3fba075ec6
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095178"
---
# <a name="how-to-invoke-a-print-dialog"></a>方法 : 印刷ダイアログ ボックスを呼び出す
アプリケーションから印刷する機能を提供するために、単に <xref:System.Windows.Controls.PrintDialog> オブジェクトを作成して開くことができます。  
  
## <a name="example"></a>例  
 <xref:System.Windows.Controls.PrintDialog> コントロールは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]、構成、および XPS ジョブを送信するための1つのエントリポイントを提供します。 コントロールは使いやすく、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] マークアップまたはコードを使用してインスタンス化できます。 次の例では、コントロールをインスタンス化してコードで開く方法と、コントロールから出力する方法を示します。 また、ダイアログで特定の範囲のページを設定するオプションをユーザーに与える方法についても説明します。 このコード例では、C: ドライブのルートに FixedDocumentSequence ファイルがあることを前提としています。  
  
 [!code-csharp[printdialog#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PrintDialog/CSharp/Window1.xaml.cs#1)]
 [!code-vb[printdialog#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PrintDialog/visualbasic/window1.xaml.vb#1)]  
  
 ダイアログが開いたら、ユーザーは自分のコンピューターにインストールされているプリンターから選択できるようになります。 また、 [MICROSOFT Xps ドキュメントライター](/windows/win32/printdocs/microsoft-xps-document-writer)を選択して、印刷ではなく XML Paper SPECIFICATION (XPS) ファイルを作成することもできます。  
  
> [!NOTE]
> このトピックで説明する WPF の <xref:System.Windows.Controls.PrintDialog?displayProperty=nameWithType> コントロールは Windows フォームの <xref:System.Windows.Forms.PrintDialog?displayProperty=nameWithType> コンポーネントと混同しないようにしてください。  
  
 厳密に言うと、ダイアログを開くことなく <xref:System.Windows.Controls.PrintDialog.PrintDocument%2A> メソッドを使用できます。 この意味では、コントロールは見えない印刷コンポーネントとして使用できます。 ただし、パフォーマンス上の理由から、<xref:System.Printing.PrintQueue.AddJob%2A> メソッドまたは <xref:System.Windows.Xps.XpsDocumentWriter>の多くの <xref:System.Windows.Xps.XpsDocumentWriter.Write%2A> と <xref:System.Windows.Xps.XpsDocumentWriter.WriteAsync%2A> メソッドのいずれかを使用することをお勧めします。 詳細については、「[プログラムによる XPS ファイルの印刷](how-to-programmatically-print-xps-files.md)」と「」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Controls.PrintDialog>
- [WPF のドキュメント](documents-in-wpf.md)
- [印刷の概要](printing-overview.md)
- [Microsoft XPS ドキュメントライター](/windows/win32/printdocs/microsoft-xps-document-writer)
