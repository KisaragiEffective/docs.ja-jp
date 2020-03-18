---
title: FolderBrowserDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- FolderBrowserDialog
helpviewer_keywords:
- FolderBrowserDialog component [Windows Forms], about FolderBrowserDialog
- directories [Windows Forms], enabling browsing in applications
- folders [Windows Forms], enabling browsing in applications
ms.assetid: 796b622c-3ba9-4356-93bb-e217fc52f2c7
ms.openlocfilehash: 8b017ba08ae4205e930ac00177c89a89fde17d3b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745730"
---
# <a name="folderbrowserdialog-component-overview-windows-forms"></a>FolderBrowserDialog コンポーネントの概要 (Windows フォーム)

Windows フォーム <xref:System.Windows.Forms.FolderBrowserDialog> コンポーネントは、フォルダーの参照および選択に使用されるモーダルダイアログボックスです。 <xref:System.Windows.Forms.FolderBrowserDialog> コンポーネント内から新しいフォルダーを作成することもできます。

> [!NOTE]
> フォルダーではなくファイルを選択するには、 [OpenFileDialog](openfiledialog-component-windows-forms.md)コンポーネントを使用します。

<xref:System.Windows.Forms.FolderBrowserDialog> コンポーネントは、実行時に <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用して表示されます。 [<xref:System.Windows.Forms.FolderBrowserDialog.RootFolder%2A>] プロパティを設定して、ダイアログボックスのツリービュー内に表示される最上位のフォルダーとサブフォルダーを決定します。 ダイアログボックスが表示されたら、<xref:System.Windows.Forms.FolderBrowserDialog.SelectedPath%2A> プロパティを使用して、選択されたフォルダーのパスを取得できます。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Windows.Forms.FolderBrowserDialog> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.FolderBrowserDialog>
- [方法: Windows フォーム FolderBrowserDialog コンポーネントを使用してフォルダーを選択する](how-to-choose-folders-with-the-windows-forms-folderbrowserdialog-component.md)
- [FolderBrowserDialog コンポーネント](folderbrowserdialog-component-windows-forms.md)
