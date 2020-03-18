---
title: '方法 : 実行時に ToolStrip 項目を並べ替えできるようにする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ToolStrip control [Windows Forms], examples
- examples [Windows Forms], toolbars
- toolbars [Windows Forms], rearranging controls
- ToolStrip control [Windows Forms], reordering items
ms.assetid: 8480b69a-379f-4dc2-8dcf-365ed93692b2
ms.openlocfilehash: 44b52bf997819f090569d08eb395d8af18f61370
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745479"
---
# <a name="how-to-enable-reordering-of-toolstrip-items-at-run-time-in-windows-forms"></a>方法 : Windows フォーム内で実行時に ToolStrip 項目を並べ替えできるようにする
ユーザーが <xref:System.Windows.Forms.ToolStrip>上で <xref:System.Windows.Forms.ToolStripItem> コントロールを再配置できるようにすることができます。  
  
### <a name="to-enable-toolstripitem-rearrangement-at-run-time"></a>実行時に ToolStripItem の再配置を有効にするには  
  
- <xref:System.Windows.Forms.ToolStrip.AllowItemReorder%2A> プロパティを `true` に設定します。 既定では、<xref:System.Windows.Forms.ToolStrip.AllowItemReorder%2A> は `false`です。  
  
     実行時には、ユーザーは ALT キーを押したままマウスの左ボタンを押すと、<xref:System.Windows.Forms.ToolStripItem> が <xref:System.Windows.Forms.ToolStrip>上の別の場所にドラッグされます。  
  
    ```vb  
    toolStrip1.AllowItemReorder = True  
    ```  
  
    ```csharp  
    toolStrip1.AllowItemReorder = true;  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStrip.AllowItemReorder%2A>
- [ToolStrip コントロールの概要](toolstrip-control-overview-windows-forms.md)
- [ToolStrip コントロールのアーキテクチャ](toolstrip-control-architecture.md)
- [ToolStrip テクノロジの概要](toolstrip-technology-summary.md)
