---
title: '方法: TreeView コントロールまたは ListView コントロールにカスタム情報を追加する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- ListItem
helpviewer_keywords:
- examples [Windows Forms], TreeView control
- examples [Windows Forms], ListView control
- ListView control [Windows Forms], adding custom information
- TreeView control [Windows Forms], adding custom information
ms.assetid: 68be11de-1d5b-430e-901f-cfbe48d14b19
ms.openlocfilehash: fe507c41de97e9332f3f27e453a476d992f86627
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732215"
---
# <a name="how-to-add-custom-information-to-a-treeview-or-listview-control-windows-forms"></a>方法 : TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する
派生ノードは、<xref:System.Windows.Forms.ListView> コントロールの Windows フォーム <xref:System.Windows.Forms.TreeView> コントロールまたは派生した項目で作成できます。 派生により、必要なフィールドだけではなく、それらを処理するためのカスタム メソッドやコンストラクターも追加できます。 この機能を使用して、顧客オブジェクトを各ツリー ノードや各リスト項目にアタッチすることもできます。 ここでは、<xref:System.Windows.Forms.TreeView> コントロールの例を示していますが、<xref:System.Windows.Forms.ListView> コントロールには同じアプローチを使用できます。  
  
### <a name="to-derive-a-tree-node"></a>ツリー ノードを派生するには  
  
- ファイルパスを記録するカスタムフィールドを持つ <xref:System.Windows.Forms.TreeNode> クラスから派生した新しいノードクラスを作成します。  
  
    ```vb  
    Class myTreeNode  
       Inherits TreeNode  
  
       Public FilePath As String  
  
       Sub New(ByVal fp As String)  
          MyBase.New()  
          FilePath = fp  
          Me.Text = fp.Substring(fp.LastIndexOf("\"))  
       End Sub  
    End Class  
    ```  
  
    ```csharp  
    class myTreeNode : TreeNode  
    {  
       public string FilePath;  
  
       public myTreeNode(string fp)  
       {  
          FilePath = fp;  
          this.Text = fp.Substring(fp.LastIndexOf("\\"));  
       }  
    }  
    ```  
  
    ```cpp  
    ref class myTreeNode : public TreeNode  
    {  
    public:  
       System::String ^ FilePath;  
  
       myTreeNode(System::String ^ fp)  
       {  
          FilePath = fp;  
          this->Text = fp->Substring(fp->LastIndexOf("\\"));  
       }  
    };  
    ```  
  
### <a name="to-use-a-derived-tree-node"></a>派生されたツリー ノードを使用するには  
  
1. 新たに派生されたツリー ノードは、関数呼び出しに対するパラメーターとして使用できます。  
  
     次の例では、テキスト ファイルの場所に設定されているパスは My Documents フォルダーです。 このように設定できるのは、Windows オペレーティング システムを実行しているほとんどのコンピューターにこのディレクトリが含まれていると想定できるからです。 また、このようにすることで、最小限のシステム アクセス レベルしか持たないユーザーもアプリケーションを安全に実行できるようになります。  
  
    ```vb  
    ' You should replace the bold text file   
    ' in the sample below with a text file of your own choosing.  
    TreeView1.Nodes.Add(New myTreeNode (System.Environment.GetFolderPath _  
       (System.Environment.SpecialFolder.Personal) _  
       & "\ TextFile.txt ") )  
    ```  
  
    ```csharp  
    // You should replace the bold text file   
    // in the sample below with a text file of your own choosing.  
    // Note the escape character used (@) when specifying the path.  
    treeView1.Nodes.Add(new myTreeNode(System.Environment.GetFolderPath
       (System.Environment.SpecialFolder.Personal)
       + @"\TextFile.txt") );  
    ```  
  
    ```cpp  
    // You should replace the bold text file   
    // in the sample below with a text file of your own choosing.  
    treeView1->Nodes->Add(new myTreeNode(String::Concat(  
       System::Environment::GetFolderPath  
       (System::Environment::SpecialFolder::Personal),  
       "\\TextFile.txt")));  
    ```  
  
2. ツリーノードが渡され、それが <xref:System.Windows.Forms.TreeNode> クラスとして型指定されている場合は、派生クラスにキャストする必要があります。 キャストとは、ある型のオブジェクトから別の型のオブジェクトに明示的に変換することです。 キャストの詳細については、「[暗黙的および明示的な変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)(Visual Basic)」、「 C#[キャストと型変換](../../../csharp/programming-guide/types/casting-and-type-conversions.md)(ビジュアル)」、またC++は「[キャスト演算子: ()](/cpp/cpp/cast-operator-parens) (ビジュアル)」を参照してください。  
  
    ```vb  
    Public Sub TreeView1_AfterSelect(ByVal sender As Object, ByVal e As System.Windows.Forms.TreeViewEventArgs) Handles TreeView1.AfterSelect  
       Dim mynode As myTreeNode  
       mynode = CType(e.node, myTreeNode)  
       MessageBox.Show("Node selected is " & mynode.filepath)  
    End Sub  
    ```  
  
    ```csharp  
    protected void treeView1_AfterSelect (object sender,  
    System.Windows.Forms.TreeViewEventArgs e)  
    {  
       myTreeNode myNode = (myTreeNode)e.Node;  
       MessageBox.Show("Node selected is " + myNode.FilePath);  
    }  
    ```  
  
    ```cpp  
    private:  
       System::Void treeView1_AfterSelect(System::Object ^  sender,  
          System::Windows::Forms::TreeViewEventArgs ^  e)  
       {  
          myTreeNode ^ myNode = safe_cast<myTreeNode^>(e->Node);  
          MessageBox::Show(String::Concat("Node selected is ",   
             myNode->FilePath));  
       }  
    ```  
  
## <a name="see-also"></a>参照

- [TreeView コントロール](treeview-control-windows-forms.md)
- [ListView コントロール](listview-control-windows-forms.md)
