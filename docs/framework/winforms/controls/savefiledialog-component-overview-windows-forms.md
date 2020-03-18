---
title: SaveFileDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- SaveFileDialog
helpviewer_keywords:
- Save File dialog box [Windows Forms], displaying
- SaveFileDialog component [Windows Forms], about SaveFileDialog
ms.assetid: be7a625f-46fd-4d06-9985-b613dcbf9bd2
ms.openlocfilehash: 7609c29b7e932ecee7dc8a289617094bd8d480e2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743103"
---
# <a name="savefiledialog-component-overview-windows-forms"></a>SaveFileDialog コンポーネントの概要 (Windows フォーム)

Windows フォームの <xref:System.Windows.Forms.SaveFileDialog> コンポーネントは、事前構成済みのダイアログ ボックスです。 これは、Windows によって使用される標準の **[ファイルの保存]** ダイアログボックスと同じです。 これは、<xref:System.Windows.Forms.CommonDialog> クラスを継承しています。

## <a name="working-with-the-savefiledialog-component"></a>SaveFileDialog コンポーネントの操作

独自のダイアログボックスを構成する代わりに、ユーザーがファイルを保存できるようにするための簡単なソリューションとして使用します。 標準の Windows ダイアログボックスに依存しているため、ユーザーにとって作成するアプリケーションの基本的な機能はすぐにわかります。 ただし、<xref:System.Windows.Forms.SaveFileDialog> コンポーネントを使用する場合は、独自のファイル保存ロジックを作成する必要があることに注意してください。

<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用して、実行時にダイアログボックスを表示できます。 <xref:System.Windows.Forms.SaveFileDialog.OpenFile%2A> メソッドを使用して、読み取り/書き込みモードでファイルを開くことができます。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Windows.Forms.SaveFileDialog> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SaveFileDialog>
- [SaveFileDialog コンポーネント](savefiledialog-component-windows-forms.md)
