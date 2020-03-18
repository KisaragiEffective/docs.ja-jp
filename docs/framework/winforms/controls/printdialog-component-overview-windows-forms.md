---
title: PrintDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- PrintDialog
helpviewer_keywords:
- Print dialog box [Windows Forms], displaying
- PrintDialog component [Windows Forms], about PrintDialog component
ms.assetid: 8327b8ac-1017-4b5e-a88b-fea9dd56999c
ms.openlocfilehash: 0ed7f7a1f9770f71b75ae744a056b6943335c852
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728670"
---
# <a name="printdialog-component-overview-windows-forms"></a>PrintDialog コンポーネントの概要 (Windows フォーム)

Windows フォーム[PrintDialog](printdialog-component-windows-forms.md)コンポーネントは、プリンターの選択、印刷するページの選択、および Windows ベースのアプリケーションでのその他の印刷関連設定の決定に使用される、事前に構成されたダイアログボックスです。 独自のダイアログ ボックスを構成する代わりに、プリンターと印刷関連の設定の選択のための簡単なソリューションとして使用します。 ユーザーがドキュメントのさまざまな部分を印刷できるようにするには、[すべて印刷]、選択したページ範囲を印刷する、または選択内容を印刷します。 Windows の標準のダイアログ ボックスを使用して、ユーザーがすぐに慣れる基本的な機能を持つアプリケーションを作成します。 <xref:System.Windows.Forms.PrintDialog> コンポーネントは <xref:System.Windows.Forms.CommonDialog> クラスから継承されます。

## <a name="working-with-the-component"></a>コンポーネントの操作

実行時にダイアログボックスを表示するには、<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用します。 このコンポーネントには、1つの印刷ジョブ (<xref:System.Drawing.Printing.PrintDocument> クラス) または個々のプリンター (<xref:System.Drawing.Printing.PrinterSettings> クラス) の設定に関連するプロパティがあります。 これらはいずれも、複数のプリンターで共有できます。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Windows.Forms.PrintDialog> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.PrintDialog>
- [PrintDialog コンポーネント](printdialog-component-windows-forms.md)
