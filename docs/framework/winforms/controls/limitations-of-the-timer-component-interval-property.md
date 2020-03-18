---
title: Timer Component Interval プロパティの制限事項
ms.date: 03/30/2017
helpviewer_keywords:
- timers [Windows Forms], event intervals
- Interval property [Windows Forms], limitations
- timers [Windows Forms], Windows-based
- Timer component [Windows Forms], limitations of Interval property
ms.assetid: 7e5fb513-77e7-4046-a8e8-aab94e61ca0f
ms.openlocfilehash: 15626a53f0541ff79e2098377d9dfdb4626ac155
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745233"
---
# <a name="limitations-of-the-windows-forms-timer-components-interval-property"></a>Windows フォームの Timer コンポーネントの Interval プロパティの制限
Windows フォーム <xref:System.Windows.Forms.Timer> コンポーネントには、1つのタイマーイベントから次のタイマーイベントまでの経過時間をミリ秒単位で指定する <xref:System.Windows.Forms.Timer.Interval%2A> プロパティがあります。 コンポーネントが無効になっていない限り、タイマーはほぼ同じ間隔で <xref:System.Windows.Forms.Timer.Tick> イベントを受信し続けます。  
  
 このコンポーネントは、Windows フォームの環境用に設計されています。 サーバー環境に適したタイマーが必要な場合は、「[サーバー ベースのタイマーの概要](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/tb9yt5e6(v=vs.90))」を参照してください。  
  
## <a name="the-interval-property"></a>Interval プロパティ  
 <xref:System.Windows.Forms.Timer.Interval%2A> プロパティには、<xref:System.Windows.Forms.Timer> コンポーネントをプログラミングする際に考慮すべきいくつかの制限があります。  
  
- アプリケーションまたは他のアプリケーションがシステムに対して大量の要求 (長いループ、大量の計算、ドライブ、ネットワーク、ポートアクセスなど) を実行している場合、<xref:System.Windows.Forms.Timer.Interval%2A> プロパティで指定されているようにタイマーイベントを取得できないことがあります。  
  
- この間隔は、時間の経過と共に正確には保証されません。 正確さを確保するために、タイマーは、蓄積された時間を内部で追跡するのではなく、必要に応じてシステムクロックをチェックする必要があります。  
  
- <xref:System.Windows.Forms.Timer.Interval%2A> プロパティの有効桁数はミリ秒単位です。 コンピューターによっては、ミリ秒よりも高い解像度の高解像度カウンターが提供される場合があります。 このようなカウンターの可用性は、コンピューターのプロセッサハードウェアによって異なります。
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Timer>
- [Timer コンポーネント](timer-component-windows-forms.md)
- [Timer コンポーネントの概要](timer-component-overview-windows-forms.md)
