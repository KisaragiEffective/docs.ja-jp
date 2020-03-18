---
title: '方法: ToolStrip 項目の間隔と配置を変更する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ToolStrip control [Windows Forms], aligning items
- examples [Windows Forms], toolbars
- toolbars [Windows Forms], aligning items
ms.assetid: cd483466-0f49-43df-addf-e2b5fcd64027
ms.openlocfilehash: 805fbac5fe33071006f29692d503e5c57eacd765
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746564"
---
# <a name="how-to-change-the-spacing-and-alignment-of-toolstrip-items-in-windows-forms"></a>方法 : Windows フォーム内の ToolStrip 項目の間隔と配置を変更する
<xref:System.Windows.Forms.ToolStrip> コントロールは、サイズ変更、相互に関連する <xref:System.Windows.Forms.ToolStripItem> コントロールの間隔、<xref:System.Windows.Forms.ToolStrip>上のコントロールの配置、<xref:System.Windows.Forms.ToolStrip>を基準としたコントロールの間隔などのレイアウト機能を完全にサポートします。  
  
 <xref:System.Windows.Forms.ToolStripItem.AutoSize%2A> プロパティの既定値は `true`ので、<xref:System.Windows.Forms.ToolStripItem.AutoSize%2A> プロパティを `false`に設定しない限り、コントロールは自動的にサイズ変更されます。  
  
### <a name="to-manually-size-a-toolstripitem"></a>ToolStripItem のサイズを手動で変更するには  
  
1. 関連付けられたコントロールの <xref:System.Windows.Forms.ToolStripItem.AutoSize%2A> プロパティを `false` に設定します。  
  
    ```vb  
    ToolStripButton1.AutoSize = False  
    ```  
  
    ```csharp  
    toolStripButton1.AutoSize = false;  
    ```  
  
2. 関連する <xref:System.Windows.Forms.ToolStripItem>に対して、<xref:System.Windows.Forms.ToolStripItem.Size%2A> プロパティを必要に応じて設定します。  
  
### <a name="to-set-the-spacing-of-a-toolstripitem"></a>ToolStripItem の間隔を設定するには  
  
1. 目的の値をピクセル単位で、関連付けられているコントロールの <xref:System.Windows.Forms.ToolStripItem.Margin%2A> プロパティに挿入します。  
  
     <xref:System.Windows.Forms.ToolStripItem.Margin%2A> プロパティの値は、項目と隣接する項目の間の間隔を、左、上、右、下の順に指定します。  
  
    ```vb  
    ToolStripTextBox1.Margin = New System.Windows.Forms.Padding _  
        (3, 0, 3, 0)  
    ```  
  
    ```csharp  
    toolStripTextBox1.Margin = new System.Windows.Forms.Padding   
        (3, 0, 3, 0);  
    ```  
  
### <a name="to-align-a-toolstripitem-to-the-right-side-of-the-toolstrip"></a>ToolStripItem を ToolStrip の右側に配置するには  
  
1. 関連付けられたコントロールの <xref:System.Windows.Forms.ToolStripItem.Alignment%2A> プロパティを <xref:System.Windows.Forms.ToolStripItemAlignment.Right> に設定します。 既定では、<xref:System.Windows.Forms.ToolStripItem.Alignment%2A> は <xref:System.Windows.Forms.ToolStripItemAlignment.Left>に設定されます。これにより、コントロールが <xref:System.Windows.Forms.ToolStrip>の左側に配置されます。  
  
    ```vb  
    ToolStripSplitButton1.Alignment = _  
        System.Windows.Forms.ToolStripItemAlignment.Right  
    ```  
  
    ```csharp  
    toolStripSplitButton1.Alignment =   
        System.Windows.Forms.ToolStripItemAlignment.Right;  
    ```  
  
### <a name="to-arrange-toolstrip-items-on-the-toolstrip"></a>Toolstrip に ToolStrip 項目を配置するには  
  
- <xref:System.Windows.Forms.ToolStrip.LayoutStyle%2A> プロパティに、必要な <xref:System.Windows.Forms.ToolStripLayoutStyle> の値を設定します。  
  
    ```vb  
    ToolStripDropDown1.LayoutStyle = _  
        System.Windows.Forms.ToolStripLayoutStyle.Flow  
    ```  
  
    ```csharp  
    toolStripDropDown1.LayoutStyle =   
        System.Windows.Forms.ToolStripLayoutStyle.Flow;  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.Control.Layout>
- <xref:System.Windows.Forms.ToolStrip.LayoutCompleted>
- <xref:System.Windows.Forms.ToolStrip.LayoutSettings%2A>
- <xref:System.Windows.Forms.ToolStripItem.TextImageRelation%2A>
- <xref:System.Windows.Forms.ToolStripItem.Placement%2A>
- <xref:System.Windows.Forms.ToolStrip.CanOverflow%2A>
- [ToolStrip コントロールの概要](toolstrip-control-overview-windows-forms.md)
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [ToolStrip テクノロジの概要](toolstrip-technology-summary.md)
