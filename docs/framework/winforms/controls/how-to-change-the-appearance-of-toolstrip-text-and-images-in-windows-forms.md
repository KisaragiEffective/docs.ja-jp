---
title: '方法: ToolStrip のテキストとイメージの外観を変更する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ToolStrip control [Windows Forms], appearance
- toolbars [Windows Forms], images
- examples [Windows Forms], toolbars
- toolbars [Windows Forms], appearance
- ToolStrip control [Windows Forms], images
- ToolStrip control [Windows Forms], text
- toolbars [Windows Forms], text
ms.assetid: d62dc9d1-2edd-4dfa-aed7-1335d6e13d86
ms.openlocfilehash: 7816e138e44554683c201895ece1f886ace8bfa6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746601"
---
# <a name="how-to-change-the-appearance-of-toolstrip-text-and-images-in-windows-forms"></a>方法 : Windows フォーム内の ToolStrip テキストとイメージの外観を変更する
<xref:System.Windows.Forms.ToolStripItem> にテキストと画像を表示するかどうか、およびそれら <xref:System.Windows.Forms.ToolStrip>を互いに相対的にどのように調整するかを制御できます。  
  
### <a name="to-define-what-is-displayed-on-a-toolstripitem"></a>ToolStripItem に表示される内容を定義するには  
  
- <xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A> プロパティを目的の値に設定します。 `Image`、`ImageAndText`、`None`、および `Text`の可能性があります。 既定では、 `ImageAndText`です。  
  
    ```vb  
    ToolStripButton2.DisplayStyle = _  
        System.Windows.Forms.ToolStripItemDisplayStyle.Image  
    ```  
  
    ```csharp  
    toolStripButton2.DisplayStyle = System.Windows.Forms.ToolStripItemDisplayStyle.Image;  
    ```  
  
### <a name="to-align-text-on-a-toolstripitem"></a>ToolStripItem のテキストを揃えるには  
  
- <xref:System.Windows.Forms.ToolStripItem.TextAlign%2A> プロパティを目的の値に設定します。 Left、center、right を使用すると、上下左右の任意の組み合わせを使用できます。 既定では、 `MiddleCenter`です。  
  
    ```vb  
    ToolStripSplitButton1.TextAlign = _  
        System.Drawing.ContentAlignment.MiddleRight  
    ```  
  
    ```csharp  
    toolStripSplitButton1.TextAlign = System.Drawing.ContentAlignment.MiddleRight;  
    ```  
  
### <a name="to-align-an-image-on-a-toolstripitem"></a>ToolStripItem 上のイメージを整列させるには  
  
- <xref:System.Windows.Forms.ToolStripItem.ImageAlign%2A> プロパティを目的の値に設定します。 Left、center、right を使用すると、上下左右の任意の組み合わせを使用できます。 既定では、 `MiddleLeft`です。  
  
    ```vb  
    ToolStripSplitButton1.ImageAlign = _  
        System.Drawing.ContentAlignment.MiddleRight  
    ```  
  
    ```csharp  
    toolStripSplitButton1.ImageAlign = System.Drawing.ContentAlignment.MiddleRight;  
    ```  
  
### <a name="to-define-how-toolstripitem-text-and-images-are-displayed-relative-to-each-other"></a>ToolStripItem テキストと画像を相互に相対的に表示する方法を定義するには  
  
- <xref:System.Windows.Forms.ToolStripItem.TextImageRelation%2A> プロパティを目的の値に設定します。 設定できる値は、`ImageAboveText`、`ImageBeforeText`、`Overlay`、`TextAboveImage`、および `TextBeforeImage` です。 既定では、 `ImageBeforeText`です。  
  
    ```vb  
    ToolStripButton1.TextImageRelation = _  
        System.Windows.Forms.TextImageRelation.ImageAboveText  
    ```  
  
    ```csharp  
    toolStripButton1.TextImageRelation = System.Windows.Forms.TextImageRelation.ImageAboveText;  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- [ToolStrip コントロールの概要](toolstrip-control-overview-windows-forms.md)
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [ToolStrip テクノロジの概要](toolstrip-technology-summary.md)
