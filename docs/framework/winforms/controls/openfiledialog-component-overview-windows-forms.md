---
title: OpenFileDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- OpenFileDialog
helpviewer_keywords:
- OpenFileDialog component [Windows Forms], about OpenFileDialog
- Open File dialog box [Windows Forms], displaying in Windows Forms
ms.assetid: cd717300-46b6-4f82-8207-b218fa7fa407
ms.openlocfilehash: c519b373150a49ee41dbb0c503f7d5a4862477d5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728437"
---
# <a name="openfiledialog-component-overview-windows-forms"></a>OpenFileDialog コンポーネントの概要 (Windows フォーム)

Windows フォームの <xref:System.Windows.Forms.OpenFileDialog> コンポーネントは、事前構成済みのダイアログ ボックスです。 これは、Windows オペレーティングシステムによって公開されているのと同じ **[ファイルを開く**] ダイアログボックスです。 これは、<xref:System.Windows.Forms.CommonDialog> クラスを継承しています。

## <a name="use-this-component"></a>このコンポーネントを使用する

このコンポーネントは、独自のダイアログボックスを構成する代わりに、ファイルを選択するための簡単なソリューションとして、Windows ベースのアプリケーション内で使用します。 Windows の標準のダイアログ ボックスを使用して、ユーザーがすぐに慣れる基本的な機能を持つアプリケーションを作成します。 ただし、<xref:System.Windows.Forms.OpenFileDialog> コンポーネントを使用する場合は、独自のファイルオープンロジックを作成する必要があることに注意してください。

実行時にダイアログを表示するには、<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用します。 ユーザーが <xref:System.Windows.Forms.OpenFileDialog.Multiselect%2A> プロパティを使用して開くファイルを複数選択できるようにすることができます。 また、<xref:System.Windows.Forms.OpenFileDialog.ShowReadOnly%2A> プロパティを使用して、ダイアログボックスに読み取り専用チェックボックスが表示されるかどうかを判断できます。 <xref:System.Windows.Forms.OpenFileDialog.ReadOnlyChecked%2A> プロパティは、[読み取り専用] チェックボックスがオンになっているかどうかを示します。 最後に、<xref:System.Windows.Forms.FileDialog.Filter%2A> プロパティは、ダイアログボックスの [ファイルの種類] ボックスに表示される選択を決定する、現在のファイル名のフィルター文字列を設定します。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Windows.Forms.OpenFileDialog> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.OpenFileDialog>
- [OpenFileDialog コンポーネント](openfiledialog-component-windows-forms.md)
