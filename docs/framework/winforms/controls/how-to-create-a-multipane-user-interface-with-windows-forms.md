---
title: マルチペインユーザーインターフェイスを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SplitContainer control [Windows Forms], examples
- ListView control [Windows Forms], examples
- RichTextBox control [Windows Forms], examples
- Panel control [Windows Forms], examples
- TreeView control [Windows Forms], examples
- Splitter control [Windows Forms], examples
ms.assetid: e79f6bcc-3740-4d1e-b46a-c5594d9b7327
ms.openlocfilehash: 4b168a6d566e20814d4403f90e157d80efe3bf12
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731339"
---
# <a name="how-to-create-a-multipane-user-interface-with-windows-forms"></a>方法 : Windows フォームでマルチペイン ユーザー インターフェイスを作成する
次の手順では、Microsoft Outlook で使用されているものと同様のマルチペインユーザーインターフェイスを作成します。これは、**フォルダー**リスト、**メッセージ**ペイン、および**プレビュー**ウィンドウで使用します。 この配置は、フォームを使用してドッキングコントロールを主することで実現されます。  
  
 コントロールをドッキングすると、コントロールが固定されている親コンテナーの端が決まります。 したがって、[<xref:System.Windows.Forms.SplitContainer.Dock%2A>] プロパティを [<xref:System.Windows.Forms.DockStyle.Right>] に設定すると、コントロールの右端が親コントロールの右端にドッキングされます。 さらに、ドッキングしたコントロールの端は、コンテナーコントロールの端に合わせて変更されます。 <xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティの動作の詳細については、「[方法: Windows フォームにコントロールをドッキング](how-to-dock-controls-on-windows-forms.md)する」を参照してください。  
  
 この手順では、アプリケーションが Microsoft Outlook を模倣するように機能を追加するのではなく、フォーム上の <xref:System.Windows.Forms.SplitContainer> とその他のコントロールを配置する方法に焦点を当てます。  
  
 このユーザーインターフェイスを作成するには、すべてのコントロールを <xref:System.Windows.Forms.SplitContainer> コントロール内に配置します。このコントロールには、左側のパネルに <xref:System.Windows.Forms.TreeView> コントロールが含まれています。 <xref:System.Windows.Forms.SplitContainer> コントロールの右側のパネルには、<xref:System.Windows.Forms.RichTextBox> コントロールの上に <xref:System.Windows.Forms.ListView> コントロールを持つ2つ目の <xref:System.Windows.Forms.SplitContainer> コントロールが含まれています。 これらの <xref:System.Windows.Forms.SplitContainer> コントロールを使用すると、フォーム上の他のコントロールのサイズを個別に変更できます。 この手順の手法を調整して、独自のカスタムユーザーインターフェイスを作成することができます。  
  
### <a name="to-create-an-outlook-style-user-interface-programmatically"></a>プログラムによって Outlook スタイルのユーザーインターフェイスを作成するには  
  
1. フォーム内で、ユーザーインターフェイスを構成する各コントロールを宣言します。 この例では、<xref:System.Windows.Forms.TreeView>、<xref:System.Windows.Forms.ListView>、<xref:System.Windows.Forms.SplitContainer>、および <xref:System.Windows.Forms.RichTextBox> の各コントロールを使用して、Microsoft Outlook のユーザーインターフェイスを模倣します。  
  
    ```vb  
    Private WithEvents treeView1 As System.Windows.Forms.TreeView  
    Private WithEvents listView1 As System.Windows.Forms.ListView  
    Private WithEvents richTextBox1 As System.Windows.Forms.RichTextBox  
    Private WithEvents splitContainer1 As _  
        System.Windows.Forms.SplitContainer  
    Private WithEvents splitContainer2 As _  
        System.Windows.Forms.SplitContainer  
    ```  
  
    ```csharp  
    private System.Windows.Forms.TreeView treeView1;  
    private System.Windows.Forms.ListView listView1;  
    private System.Windows.Forms.RichTextBox richTextBox1;  
    private System.Windows.Forms. SplitContainer splitContainer2;  
    private System.Windows.Forms. SplitContainer splitContainer1;  
    ```  
  
2. ユーザーインターフェイスを定義するプロシージャを作成します。 次のコードは、フォームが Microsoft Outlook のユーザーインターフェイスに似ているように、プロパティを設定します。 ただし、他のコントロールを使用したり、別の方法でドッキングしたりすると、同様に柔軟な他のユーザーインターフェイスを簡単に作成できます。  
  
    ```vb  
    Public Sub CreateOutlookUI()  
        ' Create an instance of each control being used.  
        Me.components = New System.ComponentModel.Container()  
        Me.treeView1 = New System.Windows.Forms.TreeView()  
        Me.listView1 = New System.Windows.Forms.ListView()  
        Me.richTextBox1 = New System.Windows.Forms.RichTextBox()  
        Me.splitContainer1 = New System.Windows.Forms.SplitContainer()  
        Me.splitContainer2= New System.Windows.Forms. SplitContainer()  
  
        ' Should you develop this into a working application,  
        ' use the AddHandler method to hook up event procedures here.  
  
        ' Set properties of TreeView control.  
        ' Use the With statement to avoid repetitive code.  
        With Me.treeView1  
            .Dock = System.Windows.Forms.DockStyle.Fill  
            .TabIndex = 0  
            .Nodes.Add("treeView")  
        End With  
  
    ' Set properties of ListView control.  
       With Me.listView1  
          .Dock = System.Windows.Forms.DockStyle.Top  
          .TabIndex = 2  
          .Items.Add("listView")  
       End With  
  
    ' Set properties of RichTextBox control.  
       With Me.richTextBox1  
          .Dock = System.Windows.Forms.DockStyle.Fill  
          .TabIndex = 3  
          .Text = "richTextBox1"  
       End With  
  
        ' Set properties of the first SplitContainer control.  
        With Me.splitContainer1  
            .Dock = System.Windows.Forms.DockStyle.Fill  
            .TabIndex = 1  
            .SplitterWidth = 4  
            .SplitterDistance = 150  
            .Orientation = Orientation.Horizontal  
            .Panel1.Controls.Add(Me.listView1)  
            .Panel2.Controls.Add(Me.richTextBox1)  
    End With  
  
        ' Set properties of the second SplitContainer control.  
        With Me.splitContainer2  
            .Dock = System.Windows.Forms.DockStyle.Fill  
            .TabIndex = 4  
            .SplitterWidth = 4  
            .SplitterDistance = 100  
            .Panel1.Controls.Add(Me.treeView1)  
            .Panel2.Controls.Add(Me.SplitContainer1)  
    End With  
  
    ' Add the main SplitContainer control to the form.  
        Me.Controls.Add(Me.splitContainer2)  
        Me.Text = "Intricate UI Example"  
    End Sub  
    ```  
  
    ```csharp  
    public void createOutlookUI()  
    {  
        // Create an instance of each control being used.  
        treeView1 = new System.Windows.Forms.TreeView();  
        listView1 = new System.Windows.Forms.ListView();  
        richTextBox1 = new System.Windows.Forms.RichTextBox();  
        splitContainer2 = new System.Windows.Forms.SplitContainer();  
        splitContainer1 = new System.Windows.Forms.SplitContainer();  
  
        // Insert code here to hook up event methods.  
  
        // Set properties of TreeView control.  
        treeView1.Dock = System.Windows.Forms.DockStyle.Fill;  
        treeView1.TabIndex = 0;  
        treeView1.Nodes.Add("treeView");  
  
        // Set properties of ListView control.  
        listView1.Dock = System.Windows.Forms.DockStyle.Top;  
        listView1.TabIndex = 2;  
        listView1.Items.Add("listView");  
  
        // Set properties of RichTextBox control.  
        richTextBox1.Dock = System.Windows.Forms.DockStyle.Fill;  
        richTextBox1.TabIndex = 3;  
        richTextBox1.Text = "richTextBox1";  
  
        // Set properties of first SplitContainer control.  
        splitContainer1.Dock = System.Windows.Forms.DockStyle.Fill;  
        splitContainer2.TabIndex = 1;  
        splitContainer2.SplitterWidth = 4;  
        splitContainer2.SplitterDistance = 150;  
        splitContainer2.Orientation = Orientation.Horizontal;  
        splitContainer2.Panel1.Controls.Add(this.listView1);  
        splitContainer2.Panel1.Controls.Add(this.richTextBox1);  
  
        // Set properties of second SplitContainer control.  
        splitContainer2.Dock = System.Windows.Forms.DockStyle.Fill;  
        splitContainer2.TabIndex = 4;  
        splitContainer2.SplitterWidth = 4;  
        splitContainer2.SplitterDistance = 100;  
        splitContainer2.Panel1.Controls.Add(this.treeView1);  
        splitContainer2.Panel1.Controls.Add(this.splitContainer1);  
  
        // Add the main SplitContainer control to the form.  
        this.Controls.Add(this.splitContainer2);  
        this.Text = "Intricate UI Example";  
    }  
    ```  
  
3. Visual Basic で、先ほど作成したプロシージャの呼び出しを `New()` プロシージャに追加します。 ビジュアルC#で、form クラスのコンストラクターに次のコード行を追加します。  
  
    ```vb  
    ' Add this to the New procedure.  
    CreateOutlookUI()  
    ```  
  
    ```csharp  
    // Add this to the form class's constructor.  
    createOutlookUI();  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SplitContainer>
- [SplitContainer コントロール](splitcontainer-control-windows-forms.md)
- [方法: デザイナーを使用して Windows フォームでマルチペイン ユーザー インターフェイスを作成する](create-a-multipane-user-interface-with-wf-using-the-designer.md)
