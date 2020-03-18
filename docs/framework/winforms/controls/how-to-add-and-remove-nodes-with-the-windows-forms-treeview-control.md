---
title: TreeView コントロールを使用してノードを追加および削除する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], TreeView control
- TreeView control [Windows Forms], removing nodes
- tree nodes in TreeView control
- TreeView control [Windows Forms], adding nodes
ms.assetid: de1b82db-4905-449a-9f59-af271a6b6673
ms.openlocfilehash: 02b3a7286798c6f2a6426e09c8fc6c18b74a6bf0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731965"
---
# <a name="how-to-add-and-remove-nodes-with-the-windows-forms-treeview-control"></a>方法 : Windows フォーム TreeView コントロールでノードを追加および削除する
Windows フォーム <xref:System.Windows.Forms.TreeView> コントロールは、最上位レベルのノードを <xref:System.Windows.Forms.TreeView.Nodes%2A> コレクションに格納します。 各 <xref:System.Windows.Forms.TreeNode> には、その子ノードを格納するための独自の <xref:System.Windows.Forms.TreeNode.Nodes%2A> コレクションもあります。 どちらのコレクションプロパティも <xref:System.Windows.Forms.TreeNodeCollection>型です。これにより、ノード階層の1つのレベルでノードを追加、削除、および再配置できる標準のコレクションメンバーが提供されます。  
  
### <a name="to-add-nodes-programmatically"></a>プログラムによってノードを追加するには  
  
1. ツリービューの <xref:System.Windows.Forms.TreeView.Nodes%2A> プロパティの <xref:System.Windows.Forms.TreeNodeCollection.Add%2A> メソッドを使用します。  
  
    ```vb  
    ' Adds new node as a child node of the currently selected node.  
    Dim newNode As TreeNode = New TreeNode("Text for new node")  
    TreeView1.SelectedNode.Nodes.Add(newNode)  
    ```  
  
    ```csharp  
    // Adds new node as a child node of the currently selected node.  
    TreeNode newNode = new TreeNode("Text for new node");  
    treeView1.SelectedNode.Nodes.Add(newNode);  
    ```  
  
    ```cpp  
    // Adds new node as a child node of the currently selected node.  
    TreeNode ^ newNode = new TreeNode("Text for new node");  
    treeView1->SelectedNode->Nodes->Add(newNode);  
    ```  
  
### <a name="to-remove-nodes-programmatically"></a>プログラムによってノードを削除するには  
  
1. 1つのノードを削除するには、ツリービューの <xref:System.Windows.Forms.TreeView.Nodes%2A> プロパティの <xref:System.Windows.Forms.TreeNodeCollection.Remove%2A> メソッドを使用します。すべてのノードをクリアするには、<xref:System.Windows.Forms.TreeNodeCollection.Clear%2A> メソッドを使用します。  
  
    ```vb  
    ' Removes currently selected node, or root if nothing is selected.  
    TreeView1.Nodes.Remove(TreeView1.SelectedNode)  
    ' Clears all nodes.  
    TreeView1.Nodes.Clear()  
    ```  
  
    ```csharp  
    // Removes currently selected node, or root if nothing   
    // is selected.  
    treeView1.Nodes.Remove(treeView1.SelectedNode);  
    // Clears all nodes.  
    TreeView1.Nodes.Clear();  
    ```  
  
    ```cpp  
    // Removes currently selected node, or root if nothing  
    // is selected.  
    treeView1->Nodes->Remove(treeView1->SelectedNode);  
    // Clears all nodes.  
    treeView1->Nodes->Clear();  
    ```  
  
## <a name="see-also"></a>参照

- [TreeView コントロール](treeview-control-windows-forms.md)
- [TreeView コントロールの概要](treeview-control-overview-windows-forms.md)
- [方法: Windows フォーム TreeView コントロールのアイコンを設定する](how-to-set-icons-for-the-windows-forms-treeview-control.md)
- [方法: Windows フォーム TreeView コントロールのすべてのノードを反復処理する](how-to-iterate-through-all-nodes-of-a-windows-forms-treeview-control.md)
- [方法: クリックされた TreeView ノードを判別する](how-to-determine-which-treeview-node-was-clicked-windows-forms.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
