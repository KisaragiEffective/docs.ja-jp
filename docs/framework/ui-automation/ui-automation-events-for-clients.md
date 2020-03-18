---
title: クライアントの UI オートメーション イベント
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, events for clients
- events, UI Automation clients
ms.assetid: b909e388-3f24-4997-b6d4-bd9c35c2dc27
ms.openlocfilehash: 497c05740fd9e1c05d877cfdb72881168fff6e2e
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778761"
---
# <a name="ui-automation-events-for-clients"></a>クライアントの UI オートメーション イベント
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、UI オートメーション クライアントでの [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] イベントの使用方法について説明します。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] により、クライアントは対象とするイベントをサブスクライブできます。 この機能により、情報、構造体または状態が変更されていないか確認するためにシステム内のすべての [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 要素を常にポーリングする必要がなくなるため、パフォーマンスが向上します。  
  
 また、定義されたスコープ内のイベントだけをリッスンできるため、効率性も向上します。 たとえば、クライアントはツリー内のすべての [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素のフォーカス変更イベントをリッスンすることも、1 つの要素とその子孫のフォーカス変更イベントだけをリッスンすることもできます。  
  
> [!NOTE]
> あらゆるイベントが、[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] プロバイダーによって生成されるわけではないことに注意してください。 たとえば、すべてのプロパティの変更によって、Windows フォームおよび Win32 コントロールの標準プロキシプロバイダーによってイベントが発生するとは限りません。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントの詳細なビューについては、「 [UI Automation イベントの概要](ui-automation-events-overview.md)」を参照してください。  
  
<a name="Subscribing_to_Events"></a>   
## <a name="subscribing-to-events"></a>イベントのサブスクライブ  
 クライアント アプリケーションは特定の種類のイベントをサブスクライブするために、次のいずれかのメソッドを使用してイベント ハンドラーを登録します。  
  
|メソッド|イベントの種類|イベント引数の種類|デリゲート型|  
|------------|----------------|--------------------------|-------------------|  
|<xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A>|フォーカスの変更|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|<xref:System.Windows.Automation.AutomationFocusChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>|プロパティの変更|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|<xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddStructureChangedEventHandler%2A>|構造の変更|<xref:System.Windows.Automation.StructureChangedEventArgs>|<xref:System.Windows.Automation.StructureChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>|<xref:System.Windows.Automation.AutomationEvent> で識別されるその他すべてのイベント|<xref:System.Windows.Automation.AutomationEventArgs> または <xref:System.Windows.Automation.WindowClosedEventArgs>|<xref:System.Windows.Automation.AutomationEventHandler>|  
  
 メソッドを呼び出す前に、イベントを処理するデリゲート メソッドを作成する必要があります。 必要に応じて、単一のメソッドでさまざまな種類のイベントを処理し、そのメソッドを複数の呼び出しで表中のメソッドの 1 つに渡すことができます。 たとえば、単一の <xref:System.Windows.Automation.AutomationEventHandler> で、さまざまなメソッドを <xref:System.Windows.Automation.AutomationEventArgs.EventId%2A> に応じて異なる方法で処理するように設定できます。  
  
> [!NOTE]
> ウィンドウを閉じるイベントを処理するには、イベント ハンドラーに渡される引数の型を <xref:System.Windows.Automation.WindowClosedEventArgs> にキャストします。 ウィンドウの [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 要素は無効になるため、`sender` パラメーターを使用して情報を取得することはできないので、代わりに <xref:System.Windows.Automation.WindowClosedEventArgs.GetRuntimeId%2A> を使用します。  
  
> [!CAUTION]
> アプリケーションが自身の [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] からイベントを受け取る可能性がある場合、イベントのサブスクリプションまたはサブスクリプションの解除にアプリケーションの [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] スレッドを使用しないでください。 使用すると、予期しない動作を招く可能性があります。 詳細については、「 [UI オートメーション スレッド処理の問題点](ui-automation-threading-issues.md)」を参照してください。  
  
 シャットダウン時、またはアプリケーションで [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントに意味がなくなった場合、UI オートメーション クライアントが次のいずれかのメソッドを呼び出す必要があります。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationEventHandler%2A>|<xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A> を使用して登録されたイベント ハンドラーの登録を解除します。|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationFocusChangedEventHandler%2A>|<xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A> を使用して登録されたイベント ハンドラーの登録を解除します。|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationPropertyChangedEventHandler%2A>|<xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A> を使用して登録されたイベント ハンドラーの登録を解除します。|  
|<xref:System.Windows.Automation.Automation.RemoveAllEventHandlers%2A>|登録済みのすべてのイベント ハンドラーの登録を解除します。|  
  
 コード例については、「 [UI オートメーションイベントのサブスクライブ](subscribe-to-ui-automation-events.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [UI オートメーション イベントのサブスクライブ](subscribe-to-ui-automation-events.md)
- [UI オートメーション イベントの概要](ui-automation-events-overview.md)
- [UI オートメーション プロパティの概要](ui-automation-properties-overview.md)
- [TrackFocus サンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FocusTracker)
