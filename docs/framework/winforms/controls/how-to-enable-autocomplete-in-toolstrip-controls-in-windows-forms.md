---
title: '方法 : ToolStrip コントロールの AutoComplete を有効にする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AutoComplete [Windows Forms], examples
- toolbars [Windows Forms], AutoComplete
- examples [Windows Forms], toolbars
- AutoComplete [Windows Forms], enabling in toolbars
- ToolStripComboBox class [Windows Forms], examples
- ToolStrip control [Windows Forms], AutoComplete
ms.assetid: fd66d085-1af1-45d4-930a-cde944da2e16
ms.openlocfilehash: db411023ad624e4c3d60b09bdbd588c85f8e22d1
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745507"
---
# <a name="how-to-enable-autocomplete-in-toolstrip-controls-in-windows-forms"></a>方法 : Windows フォームで ToolStrip コントロールの AutoComplete を有効にする
次の手順では、<xref:System.Windows.Forms.ToolStripLabel> と <xref:System.Windows.Forms.ToolStripComboBox> を結合して、最近アクセスした Web サイトなどの項目の一覧を表示することができます。 ユーザーがリスト内のいずれかの項目の最初の文字と一致する文字を入力すると、その項目がすぐに表示されます。  
  
> [!NOTE]
> オートコンプリートは、<xref:System.Windows.Forms.ComboBox> や <xref:System.Windows.Forms.TextBox>などの従来のコントロールと同じように、`ToolStrip` コントロールと連動します。  
  
### <a name="to-enable-autocomplete-in-a-toolstrip-control"></a>ToolStrip コントロールでオートコンプリートを有効にするには  
  
1. <xref:System.Windows.Forms.ToolStrip> コントロールを作成し、そのコントロールに項目を追加します。  
  
    ```vb  
    ToolStrip1 = New System.Windows.Forms.ToolStrip  
    ToolStrip1.Items.AddRange(New System.Windows.Forms.ToolStripItem()_  
        {ToolStripLabel1, ToolStripComboBox1})  
    ```  
  
    ```csharp  
    toolStrip1 = new System.Windows.Forms.ToolStrip();  
    toolStrip1.Items.AddRange(new System.Windows.Forms.ToolStripItem[]   
        {toolStripLabel1, toolStripComboBox1});  
    ```  
  
2. フォームのサイズに関係なく常に使用できるように、ラベルの [<xref:System.Windows.Forms.ToolStripItem.Overflow%2A>] プロパティとコンボボックスを [<xref:System.Windows.Forms.ToolStripItemOverflow.Never>] に設定します。  
  
    ```vb  
    ToolStripLabel1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    ToolStripComboBox1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    ```  
  
    ```csharp  
    toolStripLabel1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    toolStripComboBox1.Overflow = System.Windows.Forms.ToolStripItemOverflow.Never  
    ```  
  
3. <xref:System.Windows.Forms.ToolStripComboBox> コントロールの Items コレクションに単語を追加します。  
  
    ```vb  
    ToolStripComboBox1.Items.AddRange(New Object() {"First Item", _  
        "Second Item", "Third Item"})  
    ```  
  
    ```csharp  
    toolStripComboBox1.Items.AddRange(new object[] {"First item", "Second item", "Third item"});  
    ```  
  
4. コンボボックスの [<xref:System.Windows.Forms.ComboBox.AutoCompleteMode%2A>] プロパティを <xref:System.Windows.Forms.AutoCompleteMode.Append>に設定します。  
  
    ```vb  
    ToolStripComboBox1.AutoCompleteMode = _  
        System.Windows.Forms.AutoCompleteMode.Append  
    ```  
  
    ```csharp  
    toolStripComboBox1.AutoCompleteMode = System.Windows.Forms.AutoCompleteMode.Append;  
    ```  
  
5. コンボボックスの [<xref:System.Windows.Forms.ComboBox.AutoCompleteSource%2A>] プロパティを <xref:System.Windows.Forms.AutoCompleteSource.ListItems>に設定します。  
  
    ```vb  
    ToolStripComboBox1.AutoCompleteSource = _  
        System.Windows.Forms.AutoCompleteSource.ListItems  
    ```  
  
    ```csharp  
    toolStripComboBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.ListItems;  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStripLabel>
- <xref:System.Windows.Forms.ToolStripComboBox>
- <xref:System.Windows.Forms.ToolStripComboBox.AutoCompleteMode%2A>
- <xref:System.Windows.Forms.ToolStripComboBox.AutoCompleteSource%2A>
- [ToolStrip コントロールの概要](toolstrip-control-overview-windows-forms.md)
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [ToolStrip テクノロジの概要](toolstrip-technology-summary.md)
