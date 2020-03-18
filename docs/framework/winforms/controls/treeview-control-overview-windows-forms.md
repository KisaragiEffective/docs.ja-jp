---
title: TreeView コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- TreeView
helpviewer_keywords:
- TreeView control [Windows Forms], about TreeView control
ms.assetid: 0ece823a-9508-478a-bbdb-7d7c3bae51d5
ms.openlocfilehash: 44b5e27273d122f38ea00e009302d427305977a3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743225"
---
# <a name="treeview-control-overview-windows-forms"></a>TreeView コントロールの概要 (Windows フォーム)

Windows フォームの <xref:System.Windows.Forms.TreeView> コントロールでは、Windows オペレーティング システムの Windows エクスプローラーの左側のウィンドウにファイルやフォルダーが表示されるのと同じように、ノードの階層をユーザーに表示することができます。 ツリービュー内の各ノードには、*子ノード*と呼ばれる他のノードが含まれている場合があります。 親ノード、または子ノードを含むノードを展開または折りたたんで表示できます。 また、ツリー ビューの <xref:System.Windows.Forms.TreeView.CheckBoxes%2A> プロパティを `true` に設定することで、ノードの横にチェック ボックスがあるツリー ビューを表示することもできます。 ノードの <xref:System.Windows.Forms.TreeNode.Checked%2A> プロパティを `true` または `false` に設定することにより、プログラムでノードをオンまたはオフにすることができます。

## <a name="key-properties"></a>キー プロパティ

<xref:System.Windows.Forms.TreeView> コントロールの主要プロパティは、<xref:System.Windows.Forms.TreeView.Nodes%2A> および <xref:System.Windows.Forms.TreeView.SelectedNode%2A> です。 <xref:System.Windows.Forms.TreeView.Nodes%2A> プロパティには、ツリー ビューの最上位のノードの一覧が含まれています。 <xref:System.Windows.Forms.TreeView.SelectedNode%2A> プロパティは、現在選択されているノードを設定します。 ノードの横にあるアイコンを表示することができます。 コントロールはツリー ビューの <xref:System.Windows.Forms.ImageList> プロパティで <xref:System.Windows.Forms.TreeView.ImageList%2A> という名前のイメージを使用します。 <xref:System.Windows.Forms.TreeView.ImageIndex%2A> プロパティは、ツリー ビューでノードの既定のイメージを設定します。 画像の表示の詳細については、「[方法: Windows フォーム TreeView コントロールのアイコンを設定する](how-to-set-icons-for-the-windows-forms-treeview-control.md)」を参照してください。 Visual Studio 2005 を使用している場合は、<xref:System.Windows.Forms.TreeView> コントロールで使用できる標準イメージの大規模なライブラリにアクセスできます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TreeView>
- [TreeView コントロール](treeview-control-windows-forms.md)
- [方法: Windows フォーム TreeView コントロールのアイコンを設定する](how-to-set-icons-for-the-windows-forms-treeview-control.md)
- [方法: Windows フォーム TreeView コントロールでノードを追加および削除する](how-to-add-and-remove-nodes-with-the-windows-forms-treeview-control.md)
- [方法: Windows フォーム TreeView コントロールのすべてのノードを反復処理する](how-to-iterate-through-all-nodes-of-a-windows-forms-treeview-control.md)
- [方法: クリックされた TreeView ノードを判別する](how-to-determine-which-treeview-node-was-clicked-windows-forms.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
