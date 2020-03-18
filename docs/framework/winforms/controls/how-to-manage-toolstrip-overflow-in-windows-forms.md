---
title: '方法 : ToolStrip オーバーフローを管理する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ToolStrip control [Windows Forms], managing overflow
- toolbars [Windows Forms], managing overflow
- examples [Windows Forms], toolbars
- CanOverflow property
ms.assetid: fa10e0ad-4cbf-4c0d-9082-359c2f855d4e
ms.openlocfilehash: 52cc02e626bee2d2457355028ecddc17e462d8fa
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736147"
---
# <a name="how-to-manage-toolstrip-overflow-in-windows-forms"></a>方法 : Windows フォームの ToolStrip オーバーフローを管理する

<xref:System.Windows.Forms.ToolStrip> コントロールのすべての項目が割り当てられた領域に収まらない場合は、<xref:System.Windows.Forms.ToolStrip> でオーバーフロー機能を有効にし、特定の <xref:System.Windows.Forms.ToolStripItem>s のオーバーフロー動作を決定できます。

フォームの現在のサイズに <xref:System.Windows.Forms.ToolStrip> 割り当てられているよりも多くの領域を必要とする <xref:System.Windows.Forms.ToolStripItem>を追加すると、<xref:System.Windows.Forms.ToolStrip>に <xref:System.Windows.Forms.ToolStripOverflowButton> が自動的に表示されます。 <xref:System.Windows.Forms.ToolStripOverflowButton> が表示され、オーバーフローに対応する項目がドロップダウンオーバーフローメニューに移動します。 これにより、<xref:System.Windows.Forms.ToolStrip> 項目がさまざまなフォームサイズに適切に調整される方法をカスタマイズし、優先順位を付けることができます。 また、<xref:System.Windows.Forms.ToolStripItem.Placement%2A> と <xref:System.Windows.Forms.ToolStripOverflow.DisplayedItems%2A?displayProperty=nameWithType> のプロパティと <xref:System.Windows.Forms.ToolStrip.LayoutCompleted> イベントを使用して、項目がオーバーフローになったときの外観を変更することもできます。 デザイン時または実行時にフォームを拡大すると、メイン <xref:System.Windows.Forms.ToolStrip> により多くの <xref:System.Windows.Forms.ToolStripItem>が表示され、フォームのサイズを小さくするまで <xref:System.Windows.Forms.ToolStripOverflowButton> が消えてしまう可能性があります。

## <a name="to-enable-overflow-on-a-toolstrip-control"></a>ToolStrip コントロールでオーバーフローを有効にするには

- <xref:System.Windows.Forms.ToolStrip.CanOverflow%2A> プロパティが <xref:System.Windows.Forms.ToolStrip>の `false` に設定されていないことを確認します。 既定では、 `True`です。

     <xref:System.Windows.Forms.ToolStrip.CanOverflow%2A> が `True` (既定値) の場合、<xref:System.Windows.Forms.ToolStripItem> のコンテンツが水平方向の <xref:System.Windows.Forms.ToolStrip> の幅または垂直 <xref:System.Windows.Forms.ToolStrip>の高さを超えたときに、<xref:System.Windows.Forms.ToolStripItem> がドロップダウンオーバーフローメニューに送信されます。

## <a name="to-specify-overflow-behavior-of-a-specific-toolstripitem"></a>特定の ToolStripItem のオーバーフロー動作を指定するには

- <xref:System.Windows.Forms.ToolStripItem> の <xref:System.Windows.Forms.ToolStripItem.Overflow%2A> プロパティを目的の値に設定します。 `Always`、`Never`、`AsNeeded`の可能性があります。 既定では、 `AsNeeded`です。

    ```vb
    toolStripTextBox1.Overflow = _
    System.Windows.Forms.ToolStripItemOverflow.Never
    ```

    ```csharp
    toolStripTextBox1.Overflow = _
    System.Windows.Forms.ToolStripItemOverflow.Never;
    ```

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStripOverflowButton>
- <xref:System.Windows.Forms.ToolStripItem.Overflow%2A>
- <xref:System.Windows.Forms.ToolStrip.CanOverflow%2A>
- [ToolStrip コントロールの概要](toolstrip-control-overview-windows-forms.md)
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [ToolStrip テクノロジの概要](toolstrip-technology-summary.md)
