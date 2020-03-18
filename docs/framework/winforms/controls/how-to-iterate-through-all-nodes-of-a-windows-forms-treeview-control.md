---
title: TreeView コントロールのすべてのノードを反復処理する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], TreeView control
- TreeView control [Windows Forms], iterating through nodes
- tree nodes in TreeView control [Windows Forms], iterating through
ms.assetid: 427f8928-ebcf-4beb-887f-695b905d5134
ms.openlocfilehash: 010932fa3fdfaa907325b9934682dcbf889265c1
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736369"
---
# <a name="how-to-iterate-through-all-nodes-of-a-windows-forms-treeview-control"></a>方法 : Windows フォーム TreeView コントロールのすべてのノードを反復処理する
ノード値に対して計算を実行するために、Windows フォーム <xref:System.Windows.Forms.TreeView> コントロール内のすべてのノードを調べると便利な場合があります。 この操作は、ツリーの各コレクションの各ノードを反復処理する再帰プロシージャ (C# および C++ の場合は再帰メソッド) を使用して実行できます。  
  
 ツリービュー内の各 <xref:System.Windows.Forms.TreeNode> オブジェクトには、ツリービュー内を移動するために使用できるプロパティがあります。 <xref:System.Windows.Forms.TreeNode.FirstNode%2A>、<xref:System.Windows.Forms.TreeNode.LastNode%2A>、<xref:System.Windows.Forms.TreeNode.NextNode%2A>、<xref:System.Windows.Forms.TreeNode.PrevNode%2A>、および <xref:System.Windows.Forms.TreeNode.Parent%2A>です。 <xref:System.Windows.Forms.TreeNode.Parent%2A> プロパティの値は、現在のノードの親ノードです。 現在のノードの子ノード (存在する場合) は、[<xref:System.Windows.Forms.TreeNode.Nodes%2A>] プロパティに一覧表示されます。 <xref:System.Windows.Forms.TreeView> コントロール自体には、ツリービュー全体のルートノードである "<xref:System.Windows.Forms.TreeView.TopNode%2A>" プロパティがあります。  
  
### <a name="to-iterate-through-all-nodes-of-the-treeview-control"></a>TreeView コントロールのすべてのノードを反復処理するには  
  
1. 各ノードをテストする再帰プロシージャ (C# および C++ における再帰メソッド) を作成します。  
  
2. プロシージャを呼び出します。  
  
     次の例は、各 <xref:System.Windows.Forms.TreeNode> オブジェクトの <xref:System.Windows.Forms.TreeNode.Text%2A> プロパティを印刷する方法を示しています。  
  
    ```vb  
    Private Sub PrintRecursive(ByVal n As TreeNode)  
       System.Diagnostics.Debug.WriteLine(n.Text)  
       MessageBox.Show(n.Text)  
       Dim aNode As TreeNode  
       For Each aNode In n.Nodes  
          PrintRecursive(aNode)  
       Next  
    End Sub  
  
    ' Call the procedure using the top nodes of the treeview.  
    Private Sub CallRecursive(ByVal aTreeView As TreeView)  
       Dim n As TreeNode  
       For Each n In aTreeView.Nodes  
          PrintRecursive(n)  
       Next  
    End Sub  
    ```  
  
    ```csharp  
    private void PrintRecursive(TreeNode treeNode)  
    {  
       // Print the node.  
       System.Diagnostics.Debug.WriteLine(treeNode.Text);  
       MessageBox.Show(treeNode.Text);  
       // Print each node recursively.  
       foreach (TreeNode tn in treeNode.Nodes)  
       {  
          PrintRecursive(tn);  
       }  
    }  
  
    // Call the procedure using the TreeView.  
    private void CallRecursive(TreeView treeView)  
    {  
       // Print each node recursively.  
       TreeNodeCollection nodes = treeView.Nodes;  
       foreach (TreeNode n in nodes)  
       {  
          PrintRecursive(n);  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void PrintRecursive( TreeNode^ treeNode )  
       {  
          // Print the node.  
          System::Diagnostics::Debug::WriteLine( treeNode->Text );  
          MessageBox::Show( treeNode->Text );  
  
          // Print each node recursively.  
          System::Collections::IEnumerator^ myNodes = (safe_cast<System::Collections::IEnumerable^>(treeNode->Nodes))->GetEnumerator();  
          try  
          {  
             while ( myNodes->MoveNext() )  
             {  
                TreeNode^ tn = safe_cast<TreeNode^>(myNodes->Current);  
                PrintRecursive( tn );  
             }  
          }  
          finally  
          {  
             IDisposable^ disposable = dynamic_cast<System::IDisposable^>(myNodes);  
             if ( disposable != nullptr )  
                      disposable->Dispose();  
          }  
       }  
  
       // Call the procedure using the TreeView.  
       void CallRecursive( TreeView^ treeView )  
       {  
          // Print each node recursively.  
          TreeNodeCollection^ nodes = treeView->Nodes;  
          System::Collections::IEnumerator^ myNodes = (safe_cast<System::Collections::IEnumerable^>(nodes))->GetEnumerator();  
          try  
          {  
             while ( myNodes->MoveNext() )  
             {  
                TreeNode^ n = safe_cast<TreeNode^>(myNodes->Current);  
                PrintRecursive( n );  
             }  
          }  
          finally  
          {  
             IDisposable^ disposable = dynamic_cast<System::IDisposable^>(myNodes);  
             if ( disposable != nullptr )  
                      disposable->Dispose();  
          }  
       }  
    ```  
  
## <a name="see-also"></a>参照

- [TreeView コントロール](treeview-control-windows-forms.md)
- [再帰プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/recursive-procedures.md)
