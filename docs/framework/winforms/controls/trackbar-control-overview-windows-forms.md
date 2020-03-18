---
title: TrackBar コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- TrackBar
helpviewer_keywords:
- sliders [Windows Forms], about sliders
- TrackBar control [Windows Forms], about TrackBar control
- slider controls [Windows Forms], about slider controls
ms.assetid: 95910ecb-8a4c-4776-89fa-206c89ed6973
ms.openlocfilehash: 6901405100df4633c84850757f55b756bc9a0199
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741462"
---
# <a name="trackbar-control-overview-windows-forms"></a>TrackBar コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.TrackBar> コントロール ("スライダー" コントロールとも呼ばれます) は、大量の情報を移動したり、数値設定を視覚的に調整したりするために使用されます。 <xref:System.Windows.Forms.TrackBar> コントロールには、つまみ (スライダーとも呼ばれます) と目盛りの2つの部分があります。 つまみは、調整できる部分です。 その位置は、<xref:System.Windows.Forms.TrackBar.Value%2A> プロパティに対応します。 目盛りは、一定の間隔で間隔が一定の視覚的インジケーターです。 Trackbar は、指定した単位で移動し、水平方向または垂直方向に揃えることができます。 たとえば、トラックバーを使用して、システムのカーソルの点滅速度またはマウス速度を制御できます。  
  
## <a name="key-properties"></a>キー プロパティ  
 <xref:System.Windows.Forms.TrackBar> コントロールの主要なプロパティは、<xref:System.Windows.Forms.TrackBar.Value%2A>、<xref:System.Windows.Forms.TrackBar.TickFrequency%2A>、<xref:System.Windows.Forms.TrackBar.Minimum%2A>、および <xref:System.Windows.Forms.TrackBar.Maximum%2A>です。 <xref:System.Windows.Forms.TrackBar.TickFrequency%2A> は、ティックの間隔です。 <xref:System.Windows.Forms.TrackBar.Minimum%2A> と <xref:System.Windows.Forms.TrackBar.Maximum%2A> は、トラックバーに表示できる最小値と最大値です。  
  
 もう1つの重要なプロパティは、<xref:System.Windows.Forms.TrackBar.SmallChange%2A> と <xref:System.Windows.Forms.TrackBar.LargeChange%2A>です。 <xref:System.Windows.Forms.TrackBar.SmallChange%2A> プロパティの値は、左または右方向キーが押されたときに、thumb が移動する位置の数です。 <xref:System.Windows.Forms.TrackBar.LargeChange%2A> プロパティの値は、pageup キーが押されたとき、またはつまみの両側のトラックバーにマウスクリックが反応したときに、thumb が移動する位置の数です。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.TrackBar>
- [TrackBar コントロール](trackbar-control-windows-forms.md)
