---
title: デザイナーを使用して TreeView コントロールでノードを追加および削除する
ms.date: 03/30/2017
helpviewer_keywords:
- examples [Windows Forms], TreeView control
- TreeView control [Windows Forms], removing nodes
- tree nodes in TreeView control
- TreeView control [Windows Forms], adding nodes
ms.assetid: 35bf1750-045e-4ec5-97cb-b47b0dbdaa2c
ms.openlocfilehash: 7edf09539719ec76fa3f650254c5c84ff0bc3af7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732242"
---
# <a name="how-to-add-and-remove-nodes-with-the-windows-forms-treeview-control-using-the-designer"></a>方法 : デザイナーで Windows フォーム TreeView コントロールを使ってノードを追加および削除する

Windows フォーム <xref:System.Windows.Forms.TreeView> コントロールではノードが階層的に表示されるので、ノードを追加するときは、その親ノードに注意する必要があります。

次の手順では、<xref:System.Windows.Forms.TreeView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

### <a name="to-add-or-remove-nodes-in-the-designer"></a>デザイナーでノードを追加または削除するには

1. <xref:System.Windows.Forms.TreeView> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.TreeView.Nodes%2A>] プロパティの横に**ある省略記号 (省略**記号ボタン ([...]) ボタンをクリックして、[Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] ボタンを![ます。

     **TreeNode エディター**が表示されます。

3. ノードを追加するには、ルートノードが存在している必要があります。存在しない場合は、まず **[ルートの追加]** ボタンをクリックしてルートを追加する必要があります。 その後、ルートまたは他の任意のノードを選択し、 **[子の追加]** ボタンをクリックして、子ノードを追加できます。

4. ノードを削除するには、削除するノードを選択し、 **[削除]** ボタンをクリックします。

## <a name="see-also"></a>参照

- [TreeView コントロール](treeview-control-windows-forms.md)
- [TreeView コントロールの概要](treeview-control-overview-windows-forms.md)
- [方法: Windows フォーム TreeView コントロールのアイコンを設定する](how-to-set-icons-for-the-windows-forms-treeview-control.md)
- [方法: Windows フォーム TreeView コントロールのすべてのノードを反復処理する](how-to-iterate-through-all-nodes-of-a-windows-forms-treeview-control.md)
- [方法: クリックされた TreeView ノードを判別する](how-to-determine-which-treeview-node-was-clicked-windows-forms.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
