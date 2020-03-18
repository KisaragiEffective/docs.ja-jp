---
title: 'チュートリアル: ドラッグアンドドロップ操作の実行'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, drag and drop operations
- drag and drop [Windows Forms], Windows Forms
ms.assetid: eb66f6bf-4a7d-4c2d-b276-40fefb2d3b6c
ms.openlocfilehash: 265e6d4f9e3370d28a18b86dea983bb0b556be41
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746792"
---
# <a name="walkthrough-performing-a-drag-and-drop-operation-in-windows-forms"></a>チュートリアル : Windows フォームにおけるドラッグ アンド ドロップ操作の実行
Windows ベースのアプリケーション内でドラッグアンドドロップ操作を実行するには、一連のイベント (特に、<xref:System.Windows.Forms.Control.DragEnter>、<xref:System.Windows.Forms.Control.DragLeave>、および <xref:System.Windows.Forms.Control.DragDrop> イベント) を処理する必要があります。 これらのイベントのイベント引数で使用できる情報を操作することにより、ドラッグアンドドロップ操作を容易に行うことができます。  
  
## <a name="dragging-data"></a>データのドラッグ  
 ドラッグアンドドロップ操作はすべて、ドラッグで開始されます。 ドラッグの開始時に収集されるデータを有効にする機能は、<xref:System.Windows.Forms.Control.DoDragDrop%2A> メソッドに実装されます。  
  
 次の例では、<xref:System.Windows.Forms.Control.MouseDown> イベントを使用してドラッグ操作を開始しています。これは最も直観的です (ドラッグアンドドロップ操作のほとんどは、押されているマウスボタンで開始されます)。 ただし、すべてのイベントを使用してドラッグアンドドロッププロシージャを開始できることに注意してください。  
  
> [!NOTE]
> 特定のコントロールには、ドラッグ固有のカスタムイベントがあります。 たとえば、<xref:System.Windows.Forms.ListView> コントロールと <xref:System.Windows.Forms.TreeView> コントロールには、<xref:System.Windows.Forms.TreeView.ItemDrag> イベントがあります。  
  
#### <a name="to-start-a-drag-operation"></a>ドラッグ操作を開始するには  
  
1. ドラッグが開始されるコントロールの <xref:System.Windows.Forms.Control.MouseDown> イベントで、`DoDragDrop` メソッドを使用して、ドラッグするデータを設定します。ドラッグすると、許可される効果はになります。 詳細については、「 <xref:System.Windows.Forms.DragEventArgs.Data%2A> および <xref:System.Windows.Forms.DragEventArgs.AllowedEffect%2A>」を参照してください。  
  
     次の例は、ドラッグ操作を開始する方法を示しています。 ドラッグが開始するコントロールは <xref:System.Windows.Forms.Button> コントロールで、ドラッグされるデータは <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティを表す文字列です。また、許可される効果はコピーまたは移動します。  
  
    ```vb  
    Private Sub Button1_MouseDown(ByVal sender As Object, ByVal e As System.Windows.Forms.MouseEventArgs) Handles Button1.MouseDown  
       Button1.DoDragDrop(Button1.Text, DragDropEffects.Copy Or DragDropEffects.Move)  
    End Sub  
    ```  
  
    ```csharp  
    private void button1_MouseDown(object sender,   
    System.Windows.Forms.MouseEventArgs e)  
    {  
       button1.DoDragDrop(button1.Text, DragDropEffects.Copy |   
          DragDropEffects.Move);  
    }  
    ```  
  
    > [!NOTE]
    > `DoDragDrop` メソッドでは、任意のデータをパラメーターとして使用できます。上の例では、プロパティがドラッグ元 (<xref:System.Windows.Forms.Button> コントロール) に関連付けられていたため、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティが使用されていました (値をハードコーディングしたり、データセットからデータを取得したりするのではありません)。 ドラッグアンドドロップ操作を Windows ベースのアプリケーションに組み込むときは、この点に注意してください。  
  
 ドラッグ操作が有効になっている間に、ドラッグ操作を続行するためにシステムの "アクセス許可" を要求する <xref:System.Windows.Forms.Control.QueryContinueDrag> イベントを処理できます。 このメソッドを処理する場合は、ドラッグ操作に影響を与えるメソッドを呼び出すこともできます。たとえば、カーソルがカーソルを置いたときに <xref:System.Windows.Forms.TreeView> コントロールの <xref:System.Windows.Forms.TreeNode> を展開する場合などです。  
  
## <a name="dropping-data"></a>データの削除  
 Windows フォームまたはコントロール上の場所からデータのドラッグを開始した後は、どこかにデータをドロップすることをお勧めします。 カーソルは、データの削除用に正しく構成されているフォームまたはコントロールの領域と交差すると変更されます。 Windows フォームまたはコントロール内の領域は、<xref:System.Windows.Forms.Control.AllowDrop%2A> プロパティを設定し、<xref:System.Windows.Forms.Control.DragEnter> イベントと <xref:System.Windows.Forms.Control.DragDrop> イベントを処理することによって、削除されたデータを受け入れることができます。  
  
#### <a name="to-perform-a-drop"></a>削除を実行するには  
  
1. <xref:System.Windows.Forms.Control.AllowDrop%2A> プロパティを true に設定します。  
  
2. ドロップが発生するコントロールの `DragEnter` イベントで、ドラッグされるデータが許容される型 (この場合は <xref:System.Windows.Forms.Control.Text%2A>) であることを確認します。 次に、このコードでは、<xref:System.Windows.Forms.DragDropEffects> 列挙体の値にドロップすると発生する効果を設定します。 詳細については、<xref:System.Windows.Forms.DragEventArgs.Effect%2A> を参照してください。  
  
    ```vb  
    Private Sub TextBox1_DragEnter(ByVal sender As Object, ByVal e As System.Windows.Forms.DragEventArgs) Handles TextBox1.DragEnter  
       If (e.Data.GetDataPresent(DataFormats.Text)) Then  
         e.Effect = DragDropEffects.Copy  
       Else  
         e.Effect = DragDropEffects.None  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_DragEnter(object sender,   
    System.Windows.Forms.DragEventArgs e)  
    {  
       if (e.Data.GetDataPresent(DataFormats.Text))   
          e.Effect = DragDropEffects.Copy;  
       else  
          e.Effect = DragDropEffects.None;  
    }  
    ```  
  
    > [!NOTE]
    > 独自のオブジェクトを <xref:System.Windows.Forms.DataObject.SetData%2A> メソッドの <xref:System.Object> パラメーターとして指定することで、独自の <xref:System.Windows.Forms.DataFormats> を定義できます。 これを行うときは、指定されたオブジェクトがシリアル化可能であることを確認してください。 詳細については、<xref:System.Runtime.Serialization.ISerializable> を参照してください。  
  
3. ドロップが発生するコントロールの <xref:System.Windows.Forms.Control.DragDrop> イベントで、<xref:System.Windows.Forms.DataObject.GetData%2A> メソッドを使用して、ドラッグされているデータを取得します。 詳細については、<xref:System.Security.Cryptography.Xml.DataObject.Data%2A> を参照してください。  
  
     次の例では、<xref:System.Windows.Forms.TextBox> コントロールがドラッグされているコントロールです (ドロップが発生します)。 このコードは、ドラッグされているデータと同じ <xref:System.Windows.Forms.TextBox> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティを設定します。  
  
    ```vb  
    Private Sub TextBox1_DragDrop(ByVal sender As Object, ByVal e As System.Windows.Forms.DragEventArgs) Handles TextBox1.DragDrop  
       TextBox1.Text = e.Data.GetData(DataFormats.Text).ToString  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_DragDrop(object sender,   
    System.Windows.Forms.DragEventArgs e)  
    {  
       textBox1.Text = e.Data.GetData(DataFormats.Text).ToString();  
    }  
    ```  
  
    > [!NOTE]
    > また、<xref:System.Windows.Forms.DragEventArgs.KeyState%2A> プロパティを操作することもできます。これにより、ドラッグアンドドロップ操作中に押されたキーに応じて、特定の効果が発生します (たとえば、CTRL キーが押されたときにドラッグしたデータをコピーするのは標準です)。  
  
## <a name="see-also"></a>参照

- [方法: クリップボードにデータを追加する](how-to-add-data-to-the-clipboard.md)
- [方法: クリップボードからデータを取得する](how-to-retrieve-data-from-the-clipboard.md)
- [ドラッグ アンド ドロップ操作とクリップボードのサポート](drag-and-drop-operations-and-clipboard-support.md)
