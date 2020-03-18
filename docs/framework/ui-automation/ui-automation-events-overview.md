---
title: UI オートメーション イベントの概要
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- UI Automation, events
- clients, UI Automation
- events, UI Automation
- providers, UI Automation
- UI Automation, clients
ms.assetid: 69eebd8b-39ed-40e7-93cc-4457c4caf746
ms.openlocfilehash: 5f9362814eb671a6d7a111cadb96be6d06f5cb3d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441496"
---
# <a name="ui-automation-events-overview"></a>UI オートメーション イベントの概要
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] イベント通知は、スクリーン リーダーやスクリーン拡大鏡などの支援技術にとっての重要な機能です。 これらの UI オートメーション クライアントは、 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] で何かが発生したときに UI オートメーション プロバイダーが発生させるイベントを追跡し、その情報を使用してエンド ユーザーに通知します。  
  
 これらのイベントにサブスクライブしているクライアントがあるか、それともイベントをリッスンするクライアントがなく、サブスクライブしているクライアントがまったくないかに応じて、プロバイダー アプリケーションが選択的にイベントを発生させることで効率が向上します。  
  
<a name="Types_of_Events"></a>   
## <a name="types-of-events"></a>イベントの種類  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントは次のカテゴリに分けられます。  
  
|イベント|説明|  
|-----------|-----------------|  
|プロパティの変更|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素のプロパティまたはコントロール パターンが変更された場合に発生します。 たとえば、クライアントがアプリケーションのチェック ボックス コントロールの監視を必要とする場合に、 <xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> プロパティ上のプロパティの変更イベントをリッスンするよう登録できます。 チェック ボックス コントロールがオンまたはオフになったときに、プロバイダーがイベントを発生させ、クライアントが必要なアクションを実行できます。|  
|要素のアクション|[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] の変更がエンド ユーザーまたはプログラムによるアクティビティに起因する場合に発生します。たとえば、ボタンがクリックされたり、 <xref:System.Windows.Automation.InvokePattern>を通じて呼び出される場合です。|  
|構造の変更|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの構造が変更された場合に発生します。 構造が変更されるのは、デスクトップ上で新しい [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 項目が表示または非表示にされるか、削除された場合です。|  
|グローバル デスクトップの変更|ある要素から別の要素にフォーカスが移った場合やウィンドウが閉じられた場合など、クライアントにグローバルに関連するアクションが起こった場合に発生します。|  
  
 イベントによっては、必ずしも UI の状態が変更されたことを意味しません。 たとえば、ユーザーが Tab キーを押して、テキスト入力フィールドに移動し、フィールドを更新するボタンをクリックした場合、ユーザーが実際にテキストを変更していない場合でも `TextChangedEvent` が発生します。 イベントを処理する際、クライアント アプリケーションがアクションを起こす前に、実際に変更が行われたかどうかのチェックが必要となる場合があります。  
  
 次のイベントは、UI の状態が変更されていない場合でも発生する可能性があります。  
  
- `AutomationPropertyChangedEvent` (変更されたプロパティによって変わります)  
  
- `ElementSelectedEvent`  
  
- `InvalidatedEvent`  
  
- `TextChangedEvent`  
  
<a name="UI_Automation_Event_Identifiers"></a>   
## <a name="ui-automation-event-identifiers"></a>UI オートメーション イベント識別子  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] イベントは、 <xref:System.Windows.Automation.AutomationEvent> オブジェクトによって識別されます。 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> プロパティには、イベントの種類を一意に識別する値が含まれます。  
  
 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> に指定できる値と、イベント引数に使用する種類を次の表に示します。 クライアントとプロバイダーが使用する識別子は、異なるクラスで同じ名前のフィールドです。  
  
|クライアント識別子|プロバイダー識別子|イベント引数の種類|  
|-----------------------|-------------------------|--------------------------|  
|<xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|  
|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPattern.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePattern.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.StructureChangedEventArgs>|  
|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowClosedEventArgs>|  
  
<a name="UI_Automation_Event_Arguments"></a>   
## <a name="ui-automation-event-arguments"></a>UI オートメーション イベント引数  
 次のクラスは、イベント引数をカプセル化します。  
  
|クラス|説明|  
|-----------|-----------------|  
|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|読み込みが完了した割合など、コンテンツの非同期読み込みに関する情報を含んでいます。|  
|<xref:System.Windows.Automation.AutomationEventArgs>|追加データを必要としない単純なイベントに関する情報を含んでいます。|  
|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|ある要素から別の要素への入力フォーカスの変化に関する情報を含んでいます。 この種類のイベントは、プロバイダーではなく、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] システムによって発生します。|  
|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|要素のプロパティ値またはコントロール パターンの変更に関する情報を含んでいます。|  
|<xref:System.Windows.Automation.StructureChangedEventArgs>|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの変更に関する情報を含んでいます。|  
|<xref:System.Windows.Automation.WindowClosedEventArgs>|ウィンドウ クローズに関する情報を含んでいます。|  
  
 すべてのイベント引数クラスには、 <xref:System.Windows.Automation.AutomationEventArgs.EventId%2A> メンバーが含まれます。 この識別子は <xref:System.Windows.Automation.AutomationEvent>にカプセル化されます。  
  
 イベントを識別するために使用する <xref:System.Windows.Automation.AutomationEvent> オブジェクトは、 <xref:System.Windows.Automation.AutomationElementIdentifiers> およびコントロール パターン識別子クラス ( <xref:System.Windows.Automation.DockPatternIdentifiers>など) のフィールドからプロバイダーによって取得されます。 等価のフィールドが、 <xref:System.Windows.Automation.AutomationElement> およびコントロール パターン クラス ( <xref:System.Windows.Automation.DockPattern>など) のフィールドからクライアント アプリケーションによって取得されます。  
  
 イベント識別子の一覧については、「 [UI Automation Events for Clients](ui-automation-events-for-clients.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [UI Automation Events for Clients](ui-automation-events-for-clients.md)
- [サーバー側 UI オートメーション プロバイダーの実装](server-side-ui-automation-provider-implementation.md)
- [UI オートメーション イベントのサブスクライブ](subscribe-to-ui-automation-events.md)
