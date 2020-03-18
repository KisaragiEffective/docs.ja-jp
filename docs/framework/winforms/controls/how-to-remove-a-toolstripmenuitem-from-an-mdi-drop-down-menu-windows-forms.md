---
title: '方法: ToolStripMenuItem を MDI ドロップダウン メニューから削除する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- menu items [Windows Forms], removing from MDI drop-down menus
- MenuStrip control [Windows Forms], merging
- MenuStrip control [Windows Forms], removing
- MDI [Windows Forms], merging menu items
ms.assetid: bdafe60d-82ee-45bc-97fe-eeefca6e54c1
ms.openlocfilehash: 3198195cf0991734826508aa65818505bf2038c8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735844"
---
# <a name="how-to-remove-a-toolstripmenuitem-from-an-mdi-drop-down-menu-windows-forms"></a>方法 : ToolStripMenuItem を MDI ドロップダウン メニューから削除する (Windows フォーム)
アプリケーションの中には、マルチ ドキュメント インターフェイス (MDI) 子ウィンドウの種類が MDI 親ウィンドウと異なるものがあります。 たとえば、MDI 親がスプレッドシートで、MDI 子がグラフの場合があります。 そのような場合は、異なる種類の MDI 子ウィンドウがアクティブになったときに、MDI 子メニューの内容で MDI 親メニューの内容を更新する必要があります。  
  
 次の手順では、<xref:System.Windows.Forms.Form.IsMdiContainer%2A>、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A>、<xref:System.Windows.Forms.MergeAction>、および <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> プロパティを使用して、MDI 親メニューのドロップダウン部分からメニュー項目を削除します。 MDI 子ウィンドウを閉じると、削除されたメニュー項目が MDI 親メニューに復元されます。  
  
### <a name="to-remove-a-menustrip-from-an-mdi-drop-down-menu"></a>MDI ドロップダウンメニューから MenuStrip を削除するには  
  
1. フォームを作成し、その <xref:System.Windows.Forms.Form.IsMdiContainer%2A> プロパティを `true` に設定します。  
  
2. <xref:System.Windows.Forms.MenuStrip> を `Form1` に追加し、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A> の <xref:System.Windows.Forms.MenuStrip> プロパティを `true` に設定します。  
  
3. トップレベル メニュー項目を `Form1` の <xref:System.Windows.Forms.MenuStrip> に追加し、その <xref:System.Windows.Forms.Control.Text%2A> プロパティを「`&File`」に設定しますす。  
  
4. `&File` メニュー項目に3つのサブメニュー項目を追加し、それらの <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを `&Open`、`&Import from`、および `E&xit`に設定します。  
  
5. `&Import from` サブメニュー項目に2つのサブメニュー項目を追加し、それらの <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを `&Word` および `&Excel`に設定します。  
  
6. プロジェクトにフォームを追加し、フォームに <xref:System.Windows.Forms.MenuStrip> を追加し、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A> の `Form2` の <xref:System.Windows.Forms.MenuStrip> のプロパティを `true` に設定します。  
  
7. トップレベル メニュー項目を `Form2` の <xref:System.Windows.Forms.MenuStrip> に追加し、その <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを「`&File`」に設定しますす。  
  
8. `Form2`の `&File` メニューに `&Import from` サブメニュー項目を追加し、[`&File`] メニューに [`&Word`] サブメニュー項目を追加します。  
  
9. 次の表に示すように、`Form2` のメニュー項目の <xref:System.Windows.Forms.MergeAction> と <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> のプロパティを設定します。  
  
    |Form2 のメニュー項目|MergeAction 値|MergeIndex 値|  
    |---------------------|-----------------------|----------------------|  
    |ファイル|MatchOnly|-1|  
    |インポート:|MatchOnly|-1|  
    |Word|[削除]|-1|  
  
10. `Form1`で、`&Open`<xref:System.Windows.Forms.ToolStripMenuItem>の <xref:System.Windows.Forms.Control.Click> イベントのイベントハンドラーを作成します。  
  
11. イベントハンドラー内で、次のコード例のようなコードを挿入し、`Form1`の MDI 子として `Form2` の新しいインスタンスを作成して表示します。  
  
    ```vb  
    Private Sub openToolStripMenuItem_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles openToolStripMenuItem.Click  
        Dim NewMDIChild As New Form2()  
        'Set the parent form of the child window.  
            NewMDIChild.MdiParent = Me  
        'Display the new form.  
            NewMDIChild.Show()  
    End Sub  
    ```  
  
    ```csharp  
    private void openToolStripMenuItem_Click(object sender, EventArgs e)  
    {  
        Form2 newMDIChild = new Form2();  
        // Set the parent form of the child window.  
            newMDIChild.MdiParent = this;  
        // Display the new form.  
            newMDIChild.Show();  
    }  
    ```  
  
12. `&Open`<xref:System.Windows.Forms.ToolStripMenuItem> に次のコード例のようなコードを配置し、イベント ハンドラーを登録します。  
  
    ```vb  
    Private Sub openToolStripMenuItem_Click(sender As Object, e As _  
    EventArgs) Handles openToolStripMenuItem.Click  
    ```  
  
    ```csharp  
    this.openToolStripMenuItem.Click += new _  
    System.EventHandler(this.openToolStripMenuItem_Click);  
    ```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.Form> と `Form1` という名前の 2 つの `Form2` コントロール。  
  
- <xref:System.Windows.Forms.MenuStrip> 上の `Form1` という名前の `menuStrip1` コントロールと、<xref:System.Windows.Forms.MenuStrip> 上の `Form2` という名前の `menuStrip2` コントロール。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- [方法: MDI 親フォームを作成する](../advanced/how-to-create-mdi-parent-forms.md)
- [方法: MDI 子フォームを作成する](../advanced/how-to-create-mdi-child-forms.md)
- [MenuStrip コントロールの概要](menustrip-control-overview-windows-forms.md)
