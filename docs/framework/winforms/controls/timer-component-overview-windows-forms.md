---
title: Timer コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- Timer
helpviewer_keywords:
- Timer component [Windows Forms], about Timer components
- timers [Windows Forms], about timers
ms.assetid: e672c05b-a8b6-4b26-9e4d-9223aa9e3873
ms.openlocfilehash: 83b52d395cdc3e3357bcf83517eab258a5c8ae08
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742775"
---
# <a name="timer-component-overview-windows-forms"></a>Timer コンポーネントの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.Timer> は、一定の間隔でイベントを発生させるコンポーネントです。 このコンポーネントは、Windows フォームの環境用に設計されています。 サーバー環境に適したタイマーが必要な場合は、「[サーバー ベースのタイマーの概要](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/tb9yt5e6(v=vs.90))」を参照してください。  
  
## <a name="key-properties-methods-and-events"></a>主要なプロパティ、メソッド、およびイベント  
 間隔の長さは、<xref:System.Windows.Forms.Timer.Interval%2A> プロパティによって定義され、その値はミリ秒単位です。 コンポーネントが有効になると、<xref:System.Windows.Forms.Timer.Tick> イベントがすべての間隔で発生します。 ここで、実行するコードを追加します。 詳細については、「[方法: Windows フォーム Timer コンポーネントを使用して一定間隔でプロシージャを実行する](run-procedures-at-set-intervals-with-wf-timer-component.md)」を参照してください。 <xref:System.Windows.Forms.Timer> コンポーネントの主要なメソッドは <xref:System.Windows.Forms.Timer.Start%2A> と <xref:System.Windows.Forms.Timer.Stop%2A>であり、タイマーをオンまたはオフにします。 タイマーがオフに切り替わると、リセットされます。<xref:System.Windows.Forms.Timer> コンポーネントを一時停止する方法はありません。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Timer>
- [Timer コンポーネント](timer-component-windows-forms.md)
- [Windows フォームの Timer コンポーネントの Interval プロパティの制限](limitations-of-the-timer-component-interval-property.md)
