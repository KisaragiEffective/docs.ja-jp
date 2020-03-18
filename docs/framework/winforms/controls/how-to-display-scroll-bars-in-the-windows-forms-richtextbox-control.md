---
title: RichTextBox コントロールにスクロールバーを表示する
ms.date: 03/30/2017
helpviewer_keywords:
- text boxes [Windows Forms], displaying scroll bars
- scroll bars [Windows Forms], displaying in controls
- RichTextBox control [Windows Forms], displaying scroll bars
ms.assetid: cdeb42e1-86e8-410c-ba46-18aec264ef5f
ms.openlocfilehash: 2185b572ef20765043d3df3dbfd8bf5b21cfac28
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745557"
---
# <a name="how-to-display-scroll-bars-in-the-windows-forms-richtextbox-control"></a>方法 : Windows フォームの RichTextBox コントロールにスクロール バーを表示する
既定では、必要に応じて、Windows フォーム <xref:System.Windows.Forms.RichTextBox> コントロールに水平スクロールバーと垂直スクロールバーが表示されます。 次の表に示すように、<xref:System.Windows.Forms.RichTextBox> コントロールの <xref:System.Windows.Forms.RichTextBox.ScrollBars%2A> プロパティに使用できる値は7つあります。  
  
### <a name="to-display-scroll-bars-in-a-richtextbox-control"></a>RichTextBox コントロールにスクロールバーを表示するには  
  
1. <xref:System.Windows.Forms.RichTextBox.Multiline%2A> プロパティを `true` に設定します。 <xref:System.Windows.Forms.RichTextBox.Multiline%2A> プロパティが `false`に設定されている場合、水平を含むスクロールバーの種類は表示されません。  
  
2. <xref:System.Windows.Forms.RichTextBox.ScrollBars%2A> プロパティを <xref:System.Windows.Forms.RichTextBoxScrollBars> 列挙の適切な値に設定します。  
  
    |値|[説明]|  
    |-----------|-----------------|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.Both> (規定値)|テキストがコントロールの幅または長さを超えた場合にのみ、水平または垂直のスクロールバー、またはその両方を表示します。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.None>|どの種類のスクロールバーも表示されません。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.Horizontal>|テキストがコントロールの幅を超えた場合にのみ、水平スクロールバーを表示します。 (これを行うには、<xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> プロパティを `false`に設定する必要があります)。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.Vertical>|テキストがコントロールの高さを超えた場合にのみ、垂直スクロールバーを表示します。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.ForcedHorizontal>|<xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> プロパティが `false`に設定されている場合、水平スクロールバーを表示します。 テキストがコントロールの幅を超えていない場合、スクロールバーは淡色表示されます。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.ForcedVertical>|常に垂直スクロールバーを表示します。 テキストがコントロールの長さを超えていない場合、スクロールバーは淡色表示されます。|  
    |<xref:System.Windows.Forms.RichTextBoxScrollBars.ForcedBoth>|常に垂直スクロールバーを表示します。 <xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> プロパティが `false`に設定されている場合、水平スクロールバーを表示します。 テキストがコントロールの幅または長さを超えていない場合、スクロールバーはグレー表示されます。|  
  
3. <xref:System.Windows.Forms.TextBoxBase.WordWrap%2A> プロパティに適切な値を設定します。  
  
    |値|[説明]|  
    |-----------|-----------------|  
    |`false`|コントロール内のテキストは、コントロールの幅に合わせて自動的に調整されないので、改行に達するまで右にスクロールします。 上にある水平スクロールバーまたは両方を選択した場合は、この値を使用します。|  
    |`true` (規定値)|コントロール内のテキストは、コントロールの幅に合わせて自動的に調整されます。 水平スクロールバーは表示されません。 1つ以上の段落を表示する場合は、この値を使用します。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RichTextBoxScrollBars>
- <xref:System.Windows.Forms.RichTextBox>
- [RichTextBox コントロール](richtextbox-control-windows-forms.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
