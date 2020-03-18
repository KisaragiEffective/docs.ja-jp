---
title: 電源管理
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- battery states
- power states
ms.assetid: ad04a801-5682-4d88-92c5-26eb9cdb209a
ms.openlocfilehash: 9ac39df43a08f62e50116c61c4bdeda4cd1c8281
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746854"
---
# <a name="power-management-in-windows-forms"></a>Windows フォームでの電源管理
Windows フォームアプリケーションは、Windows オペレーティングシステムの電源管理機能を利用できます。 アプリケーションでは、コンピューターの電源状態を監視し、状態が変化したときにアクションを実行できます。 たとえば、アプリケーションがポータブルコンピューター上で実行されている場合、コンピューターのバッテリ残量が一定のレベルに達したときに、アプリケーションの特定の機能を無効にすることができます。  
  
 .NET Framework は、ユーザーがオペレーティングシステムを中断または再開したときや、AC 電源の状態またはバッテリの状態が変化したときなど、電源の状態が変化したときに発生する <xref:Microsoft.Win32.SystemEvents.PowerModeChanged> イベントを提供します。 次のコード例に示すように、<xref:System.Windows.Forms.SystemInformation> クラスの <xref:System.Windows.Forms.SystemInformation.PowerStatus%2A> プロパティを使用して、現在の状態を照会できます。  
  
 [!code-csharp[PowerMode#1](~/samples/snippets/csharp/VS_Snippets_Winforms/powermode/cs/form1.cs#1)]
 [!code-vb[PowerMode#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/powermode/vb/form1.vb#1)]  
  
 <xref:System.Windows.Forms.SystemInformation.PowerStatus%2A> プロパティには、<xref:System.Windows.Forms.BatteryChargeStatus> 列挙体以外にも、バッテリ容量 (<xref:System.Windows.Forms.PowerStatus.BatteryFullLifetime%2A>) とバッテリ充電率 (<xref:System.Windows.Forms.PowerStatus.BatteryLifePercent%2A>、<xref:System.Windows.Forms.PowerStatus.BatteryLifeRemaining%2A>) を決定するための列挙が含まれています。  
  
 <xref:System.Windows.Forms.Application> の <xref:System.Windows.Forms.Application.SetSuspendState%2A> 方法を使用して、コンピューターを休止状態または中断モードにすることができます。 `force` 引数が `false`に設定されている場合、オペレーティングシステムは、中断の許可を要求しているすべてのアプリケーションにイベントをブロードキャストします。 `disableWakeEvent` 引数が `true`に設定されている場合、オペレーティングシステムはすべてのウェイクイベントを無効にします。  
  
 次のコード例は、コンピューターを休止状態にする方法を示しています。  
  
 [!code-csharp[PowerMode#2](~/samples/snippets/csharp/VS_Snippets_Winforms/powermode/cs/form1.cs#2)]
 [!code-vb[PowerMode#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/powermode/vb/form1.vb#2)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.Win32.SystemEvents.PowerModeChanged>
- <xref:System.Windows.Forms.SystemInformation.PowerStatus%2A>
- <xref:System.Windows.Forms.Application.SetSuspendState%2A>
- <xref:Microsoft.Win32.SystemEvents.SessionSwitch>
