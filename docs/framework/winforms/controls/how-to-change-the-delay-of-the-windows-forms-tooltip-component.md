---
title: ツールヒントコンポーネントの遅延を変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- ToolTip component [Windows Forms], delay values
- tooltips [Windows Forms], delay values
- examples [Windows Forms], tooltips
ms.assetid: 08979ba7-dd84-477b-ab17-8d06e759be99
ms.openlocfilehash: 8ab0b0760e8c82d752eaada19f14cae57fa63fdc
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746587"
---
# <a name="how-to-change-the-delay-of-the-windows-forms-tooltip-component"></a>方法 : Windows フォームの ToolTip コンポーネントの遅延時間を変更する
Windows フォーム <xref:System.Windows.Forms.ToolTip> コンポーネントに設定できる遅延値は複数あります。 これらのプロパティすべての測定単位はミリ秒です。 <xref:System.Windows.Forms.ToolTip.InitialDelay%2A> プロパティは、ツールヒント文字列を表示するために、関連付けられたコントロールをユーザーがポイントする必要がある期間を決定します。 <xref:System.Windows.Forms.ToolTip.ReshowDelay%2A> プロパティは、ツールヒントに関連付けられたコントロール間でマウスを移動したときに、後続のツールヒント文字列が表示されるまでにかかる時間をミリ秒単位で設定します。 <xref:System.Windows.Forms.ToolTip.AutoPopDelay%2A> プロパティは、ツールヒント文字列を表示する時間の長さを決定します。 これらの値は、個別に設定することも、<xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A> プロパティの値を設定することによって設定することもできます。その他の遅延プロパティは、<xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A> プロパティに割り当てられた値に基づいて設定されます。 たとえば、<xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A> が値 N に設定されている場合、<xref:System.Windows.Forms.ToolTip.InitialDelay%2A> は N に設定されます。 <xref:System.Windows.Forms.ToolTip.ReshowDelay%2A> は、5 (または N/5) で割った <xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A> 値に設定され、<xref:System.Windows.Forms.ToolTip.AutoPopDelay%2A> は <xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A> プロパティ (または 5N) の5倍の値に設定されます。  
  
### <a name="to-set-the-delay"></a>遅延を設定するには  
  
1. この例に示すように、次のプロパティを設定します。  
  
    ```vb  
    ToolTip1.InitialDelay = 500  
    ToolTip1.ReshowDelay = 100  
    ToolTip1.AutoPopDelay = 5000  
    ```  
  
    ```csharp  
    ToolTip1.InitialDelay = 500;  
    ToolTip1.ReshowDelay = 100;  
    ToolTip1.AutoPopDelay = 5000;  
    ```  
  
    ```cpp  
    toolTip1->InitialDelay = 500;  
    toolTip1->ReshowDelay = 100;  
    toolTip1->AutoPopDelay = 5000;  
    ```  
  
## <a name="see-also"></a>参照

- [ToolTip コンポーネントの概要](tooltip-component-overview-windows-forms.md)
- [方法: デザイン時に Windows フォームのコントロールにツールヒントを設定する](how-to-set-tooltips-for-controls-on-a-windows-form-at-design-time.md)
- [ToolTip コンポーネント](tooltip-component-windows-forms.md)
