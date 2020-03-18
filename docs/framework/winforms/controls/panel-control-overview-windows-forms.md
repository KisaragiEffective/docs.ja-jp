---
title: Panel コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- Panel
helpviewer_keywords:
- grouping controls [Windows Forms], Panel control
- Panel control [Windows Forms], about Panel control
ms.assetid: b6b83636-2c39-4dad-89d6-f0fa41049a74
ms.openlocfilehash: 3ea86e898e8a3e1ca90ba8e0a6240414702a1a73
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744306"
---
# <a name="panel-control-overview-windows-forms"></a>Panel コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.Panel> コントロールを使用して、他のコントロールの識別可能なグループ化を提供します。 通常、パネルを使用して、関数によってフォームを分割します。 たとえば、使用する夜間配送業者などのメーリングオプションを指定する注文フォームがあるとします。 パネル内のすべてのオプションをグループ化すると、ユーザーは論理的な視覚的な手掛かりを得ることができます。 デザイン時には、すべてのコントロールを簡単に移動できます。 <xref:System.Windows.Forms.Panel> コントロールを移動すると、それに含まれるすべてのコントロールも移動します。 パネルにグループ化されたコントロールには、<xref:System.Windows.Forms.Control.Controls%2A> プロパティを使用してアクセスできます。 このプロパティは <xref:System.Windows.Forms.Control> インスタンスのコレクションを返します。そのため、通常は、この方法で取得したコントロールを特定の型にキャストする必要があります。  
  
## <a name="panel-versus-groupbox"></a>パネルと GroupBox  
 <xref:System.Windows.Forms.Panel> コントロールは、<xref:System.Windows.Forms.GroupBox> コントロールに似ています。ただし、スクロールバーを持つことができるのは <xref:System.Windows.Forms.Panel> コントロールだけで、<xref:System.Windows.Forms.GroupBox> コントロールのみがキャプションを表示します。  
  
## <a name="key-properties"></a>キー プロパティ  
 スクロールバーを表示するには、<xref:System.Windows.Forms.ScrollableControl.AutoScroll%2A> プロパティを `true`に設定します。 <xref:System.Windows.Forms.Control.BackColor%2A>、<xref:System.Windows.Forms.Control.BackgroundImage%2A>、および <xref:System.Windows.Forms.Panel.BorderStyle%2A> の各プロパティを設定して、パネルの外観をカスタマイズすることもできます。 <xref:System.Windows.Forms.Control.BackColor%2A> と <xref:System.Windows.Forms.Control.BackgroundImage%2A> のプロパティの詳細については、「[方法: パネルの背景を設定する](how-to-set-the-background-of-a-windows-forms-panel.md)」を参照してください。 <xref:System.Windows.Forms.Panel.BorderStyle%2A> プロパティは、表示されている境界線 (<xref:System.Windows.Forms.BorderStyle.None>)、線 (<xref:System.Windows.Forms.BorderStyle.FixedSingle>)、または影付きの線 (<xref:System.Windows.Forms.BorderStyle.Fixed3D>) を含まないパネルを表示するかどうかを決定します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Panel>
- [GroupBox コントロール](groupbox-control-windows-forms.md)
- [方法: デザイナーを使用して Windows フォーム Panel コントロールでコントロールをグループ化する](group-controls-with-wf-panel-control-using-the-designer.md)
- [方法: デザイナーを使って Windows フォーム パネルの背景を設定する](how-to-set-the-background-of-a-windows-forms-panel-using-the-designer.md)
