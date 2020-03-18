---
title: TreeView コントロールのアイコンを設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], TreeView control
- TreeView control [Windows Forms], node icons
- ImageList component [Windows Forms], adding images
- icons [Windows Forms], setting for TreeView control
- tree nodes in TreeView control [Windows Forms], icons
ms.assetid: c14ddcc0-e5a6-4c21-a2d5-6799fd491781
ms.openlocfilehash: e3d7fc35c6d9f70822cdd0b69dd12f3d469f4ffa
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737266"
---
# <a name="how-to-set-icons-for-the-windows-forms-treeview-control"></a>方法 : Windows フォーム TreeView コントロールのアイコンを設定する
Windows フォーム <xref:System.Windows.Forms.TreeView> コントロールでは、各ノードの横にアイコンを表示できます。 アイコンがノードテキストのすぐ左に配置されます。 これらのアイコンを表示するには、ツリービューを <xref:System.Windows.Forms.ImageList> コントロールに関連付ける必要があります。 イメージリストの詳細については、「 [Imagelist コンポーネント](imagelist-component-windows-forms.md)」および「[方法: Windows フォーム imagelist コンポーネントを使用してイメージを追加または削除する](how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)」を参照してください。  
  
> [!NOTE]
> Microsoft .NET Framework バージョン1.1 のバグにより、アプリケーションが <xref:System.Windows.Forms.Application.EnableVisualStyles%2A?displayProperty=nameWithType>を呼び出すと、<xref:System.Windows.Forms.TreeView> ノードにイメージが表示されなくなります。 このバグを回避するには、<xref:System.Windows.Forms.Application.EnableVisualStyles%2A>を呼び出した直後に `Main` メソッドで <xref:System.Windows.Forms.Application.DoEvents%2A?displayProperty=nameWithType> を呼び出します。 このバグは .NET Framework 2.0 で修正されています。  
  
### <a name="to-display-images-in-a-tree-view"></a>ツリービューでイメージを表示するには  
  
1. <xref:System.Windows.Forms.TreeView> コントロールの <xref:System.Windows.Forms.TreeView.ImageList%2A> プロパティを、使用する既存の <xref:System.Windows.Forms.ImageList> コントロールに設定します。  
  
     これらのプロパティは、デザイナーでプロパティウィンドウまたはコードで設定できます。  
  
    ```vb  
    TreeView1.ImageList = ImageList1  
    ```  
  
    ```csharp  
    treeView1.ImageList = imageList1;  
    ```  
  
    ```cpp  
    treeView1->ImageList = imageList1;  
    ```  
  
2. ノードの <xref:System.Windows.Forms.TreeNode.ImageIndex%2A> と <xref:System.Windows.Forms.TreeNode.SelectedImageIndex%2A> のプロパティを設定します。 <xref:System.Windows.Forms.TreeNode.ImageIndex%2A> プロパティによって、ノードの通常の状態と展開された状態に表示されるイメージが決まり、<xref:System.Windows.Forms.TreeNode.SelectedImageIndex%2A> プロパティによって、ノードの選択された状態に表示されるイメージが決まります。  
  
     これらのプロパティは、コードで設定することも、TreeNode エディター内で設定することもできます。 TreeNode エディターを開くには、プロパティウィンドウの <xref:System.Windows.Forms.TreeView.Nodes%2A> プロパティの横にある省略記号ボタン (![参照ボタン ([...]) をクリックします (](./media/visual-studio-ellipsis-button.png)プロパティウィンドウ)。  
  
    ```vb  
    ' (Assumes that ImageList1 contains at least two images and  
    ' the TreeView control contains a selected image.)  
    TreeView1.SelectedNode.ImageIndex = 0  
    TreeView1.SelectedNode.SelectedImageIndex = 1  
    ```  
  
    ```csharp  
    // (Assumes that imageList1 contains at least two images and  
    // the TreeView control contains a selected image.)  
    treeView1.SelectedNode.ImageIndex = 0;  
    treeView1.SelectedNode.SelectedImageIndex = 1;  
    ```  
  
    ```cpp  
    // (Assumes that imageList1 contains at least two images and  
    // the TreeView control contains a selected image.)  
    treeView1->SelectedNode->ImageIndex = 0;  
    treeView1->SelectedNode->SelectedImageIndex = 1;  
    ```  
  
## <a name="see-also"></a>参照

- [TreeView コントロールの概要](treeview-control-overview-windows-forms.md)
- [方法: Windows フォーム TreeView コントロールでノードを追加および削除する](how-to-add-and-remove-nodes-with-the-windows-forms-treeview-control.md)
- [方法: Windows フォーム TreeView コントロールのすべてのノードを反復処理する](how-to-iterate-through-all-nodes-of-a-windows-forms-treeview-control.md)
- [方法: クリックされた TreeView ノードを判別する](how-to-determine-which-treeview-node-was-clicked-windows-forms.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
