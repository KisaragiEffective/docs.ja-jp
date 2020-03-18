---
title: '方法: MDI ドロップダウン メニューに MenuStrip を挿入する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MenuStrip control [Windows Forms], inserting
- MenuStrip control [Windows Forms], merging
- MDI [Windows Forms], merging menu items
ms.assetid: 0fad444e-26d9-49af-8860-044d9c10d608
ms.openlocfilehash: 6e189dd159c48b5779679d0563fab85e9b848992
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736403"
---
# <a name="how-to-insert-a-menustrip-into-an-mdi-drop-down-menu-windows-forms"></a>方法 : MDI ドロップダウン メニューに MenuStrip を挿入する (Windows フォーム)
アプリケーションの中には、マルチ ドキュメント インターフェイス (MDI) 子ウィンドウの種類が MDI 親ウィンドウと異なるものがあります。 たとえば、MDI 親がスプレッドシートで、MDI 子がグラフの場合があります。 そのような場合は、異なる種類の MDI 子ウィンドウがアクティブになったときに、MDI 子メニューの内容で MDI 親メニューの内容を更新する必要があります。  
  
 次の手順では、<xref:System.Windows.Forms.Form.IsMdiContainer%2A>、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A>、<xref:System.Windows.Forms.MergeAction>、および <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> プロパティを使用して、mdi 子メニューから MDI 親メニューのドロップダウン部分にメニュー項目のグループを挿入します。 MDI 子ウィンドウを閉じると、挿入されたメニュー項目が MDI 親から削除されます。  
  
### <a name="to-insert-a-menustrip-into-an-mdi-drop-down-menu"></a>MDI ドロップダウンメニューに MenuStrip を挿入するには  
  
1. フォームを作成し、その <xref:System.Windows.Forms.Form.IsMdiContainer%2A> プロパティを `true` に設定します。  
  
2. <xref:System.Windows.Forms.MenuStrip> を `Form1` に追加し、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A> の <xref:System.Windows.Forms.MenuStrip> プロパティを `true` に設定します。  
  
3. トップレベル メニュー項目を `Form1` の <xref:System.Windows.Forms.MenuStrip> に追加し、その <xref:System.Windows.Forms.Control.Text%2A> プロパティを「`&File`」に設定しますす。  
  
4. `&File` メニュー項目に3つのサブメニュー項目を追加し、それらの <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを `&Open`、`&Import from`、および `E&xit`に設定します。  
  
5. `&Import from` サブメニュー項目に2つのサブメニュー項目を追加し、それらの <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを `&Word` および `&Excel`に設定します。  
  
6. プロジェクトにフォームを追加し、フォームに <xref:System.Windows.Forms.MenuStrip> を追加し、<xref:System.Windows.Forms.ToolStrip.AllowMerge%2A> の `Form2` の <xref:System.Windows.Forms.MenuStrip> のプロパティを `true` に設定します。  
  
7. トップレベル メニュー項目を `Form2` の <xref:System.Windows.Forms.MenuStrip> に追加し、その <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを「`&File`」に設定しますす。  
  
8. `Form2` の `&File` メニューにサブメニュー項目を追加するには、<xref:System.Windows.Forms.ToolStripSeparator>、`&Save`、`Save and &Close`、および別の <xref:System.Windows.Forms.ToolStripSeparator>の順にします。  
  
9. 次の表に示すように、`Form2` のメニュー項目の <xref:System.Windows.Forms.MergeAction> と <xref:System.Windows.Forms.ToolStripItem.MergeIndex%2A> のプロパティを設定します。  
  
    |Form2 のメニュー項目|MergeAction 値|MergeIndex 値|  
    |---------------------|-----------------------|----------------------|  
    |ファイル|MatchOnly|-1|  
    |区切り記号|挿入|2|  
    |保存|挿入|3|  
    |[保存して閉じる]|挿入|4|  
    |区切り記号|挿入|5|  
  
10. <xref:System.Windows.Forms.Control.Click> の `&Open` の <xref:System.Windows.Forms.ToolStripMenuItem> イベントにイベント ハンドラーを作成します。  
  
11. このイベント ハンドラー内に次のコード例のようなコードを挿入し、`Form2` の新規インスタンスを `Form1` の MDI 子フォームとして作成し、表示します。  
  
    ```vb  
    Private Sub openToolStripMenuItem_Click(ByVal sender As System.Object, _  
    ByVal e As System.EventArgs) Handles openToolStripMenuItem.Click  
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
    this.openToolStripMenuItem.Click += new System.EventHandler(this.openToolStripMenuItem_Click);  
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
