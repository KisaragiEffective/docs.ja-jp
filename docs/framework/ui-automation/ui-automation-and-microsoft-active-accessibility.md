---
title: UI オートメーションと Microsoft Active Accessibility
ms.date: 03/30/2017
helpviewer_keywords:
- Active Accessibility
- UI Automation, Active Accessibility compared to
- UI Automation, Microsoft Active Accessibility
- Active Accessibility, UI Automation compared to
ms.assetid: 87bee662-0a3e-4232-a421-20e7a5968321
ms.openlocfilehash: f9fc7e2e1a6d5ee26f04b239723c6b7d4283dbce
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75632324"
---
# <a name="ui-automation-and-microsoft-active-accessibility"></a>UI オートメーションと Microsoft Active Accessibility
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「Windows Automation API の [」を参照してください。UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)。  
  
 Microsoft Active Accessibility は、アプリケーションをアクセス可能にするための以前のソリューションでした。 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] は、Microsoft Windows の新しいユーザー補助モデルであり、支援技術製品および自動テストツールのニーズに対応することを目的としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、Active Accessibility よりも多くの機能強化を提供します。  
  
 このトピックでは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の主な機能と、これらの機能が Active Accessibility とどのように異なるかについて説明します。  
  
<a name="Programming_Languages_compare"></a>   
## <a name="programming-languages"></a>プログラミング言語  
< Active Accessibility は、デュアルインターフェイスをサポートするコンポーネントオブジェクトモデル (COM) に基づいているため、C/C++、Microsoft Visual Basic 6.0、スクリプト言語でプログラミングできます。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] (標準コントロールのクライアント側プロバイダーライブラリを含む) はマネージコードで記述されており、UI オートメーションクライアントアプリケーションはまたはC# Visual Basic .net を使用して最も簡単にプログラミングできます。 インターフェイスを実装する UI オートメーション プロバイダーは、マネージド コードまたは C/C++ で記述できます。  
  
<a name="Support_in_Windows_Presentation_Foundation_"></a>   
## <a name="support-in-windows-presentation-foundation"></a>Windows Presentation Foundation におけるサポート  
 Windows Presentation Foundation (WPF) は、ユーザーインターフェイスを作成するための新しいモデルです。 WPF 要素には Active Accessibility のネイティブサポートが含まれていません。ただし、Active Accessibility クライアントのブリッジングサポートを含む [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]をサポートしています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 専用に記述されたクライアントのみが、テキストの豊富なサポートなど、WPF のユーザー補助機能を最大限に活用できます。  
  
<a name="Servers_and_Clients_compare"></a>   
## <a name="servers-and-clients"></a>サーバーとクライアント  
 Active Accessibility では、サーバーとクライアントは、主にサーバーの `IAccessible`の実装を介して直接通信します。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]では、サーバー (プロバイダー) とクライアントの間にコア サービスが存在します。 このコア サービスは、プロバイダーによって実装されたインターフェイスを呼び出して、追加のサービス (要素の一意のランタイム識別子の生成など) を提供します。 クライアント アプリケーションはライブラリ関数を使用して [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] サービスを呼び出します。  
  
 UI オートメーションプロバイダーは Active Accessibility クライアントに情報を提供でき、Active Accessibility サーバーは UI オートメーションクライアントアプリケーションに情報を提供できます。 ただし、Active Accessibility は [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]ほど多くの情報を公開しないため、2つのモデルは完全には互換性がありません。  
  
<a name="UI_Elements_compare"></a>   
## <a name="ui-elements"></a>UI 要素  
 Active Accessibility は、`IAccessible` インターフェイスまたは子識別子として [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 要素を提示します。 2 つの `IAccessible` ポインターを比較してそれらが同じ要素を参照しているかどうかを判断するのは困難です。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]では、すべての要素が <xref:System.Windows.Automation.AutomationElement> オブジェクトとして表現されます。 比較は等値演算子または <xref:System.Windows.Automation.AutomationElement.Equals%2A> メソッドを使用して行われますが、どちらの方法でも、要素の一意のランタイム識別子が比較されます。  
  
<a name="Tree_Views_and_Navigation_compare"></a>   
## <a name="tree-views-and-navigation"></a>ツリー ビューとナビゲーション  
 画面上の [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 要素は、ツリー構造で表すことができます。デスクトップがルートで、その直接の子としてアプリケーション ウィンドウがあり、アプリケーション内の要素がその子孫となります。  
  
 Active Accessibility では、エンドユーザーに関係のないオートメーション要素の多くがツリーで公開されます。 クライアント アプリケーションは、すべての要素の中から意味のあるものを特定する必要があります。  
  
 UI オートメーション クライアント アプリケーションは、フィルタリングしたビューを使用して [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] を確認します。 このビューには、必要な要素 (ユーザーに情報を提供したり、対話を可能にしたりする要素) のみが表示されます。 コントロール要素だけが含まれるビューや、コンテンツ要素だけが含まれるビューがあらかじめ定義されています。さらに、アプリケーションでカスタム ビューを定義することもできます。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、ユーザーに対して [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] を簡単に表せるようにして、ユーザーとアプリケーションの対話を支援します。  
  
 Active Accessibility の要素間のナビゲーションは、空間 (画面の左側にある要素に移動するなど)、論理 (たとえば、次のメニュー項目に移動する、ダイアログボックス内のタブオーダーで次の項目に移動するなど)、または階層 (たとえば、コンテナー内の最初の子を移動したり、子からその親に移動したりすることができます。 子要素が `IAccessible`を実装しているオブジェクトであるとは限らないため、階層的ナビゲーションは複雑です。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]では、すべての [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 要素は、同じ基本機能をサポートする <xref:System.Windows.Automation.AutomationElement> オブジェクトです (プロバイダーの観点からは、これらは <xref:System.Windows.Automation.Provider.IRawElementProviderSimple>から継承されたインターフェイスを実装するオブジェクトです)。ナビゲーションは主に階層的 (親から子、兄弟から次の兄弟) です (兄弟間のナビゲーションは、タブ オーダーに従う場合があるため、論理的要素も持っています)。<xref:System.Windows.Automation.TreeWalker> クラスを使用すると、ツリーのフィルター処理されたビューを使用して、任意の開始点から移動できます。 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> と <xref:System.Windows.Automation.AutomationElement.FindAll%2A>を使用して、特定の子または子孫に移動することもできます。たとえば、指定したコントロール パターンがサポートされるダイアログ ボックス内のすべての要素を非常に簡単に取得できます。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] でのナビゲーションは、Active Accessibility よりも一貫性があります。 ドロップダウンリストやポップアップウィンドウなどの一部の要素が Active Accessibility ツリーに2回表示され、それらの要素からのナビゲーションが予期しない結果になる場合があります。 実際には、rebar コントロールに Active Accessibility を正しく実装することはできません。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] では親の再指定や位置の変更が可能なため、ウィンドウの所有関係による階層にかかわらず、要素をツリー内の任意の場所に配置できます。  
  
<a name="Roles_and_Control_Types"></a>   
## <a name="roles-and-control-types"></a>役割とコントロール型  
 Active Accessibility は、`accRole` プロパティ (`IAccessible::get_actRole`) を使用して、[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]内の要素のロールの説明 (ROLE_SYSTEM_SLIDER や ROLE_SYSTEM_MENUITEM など) を取得します。 要素の役割は、要素の機能を表す主要な鍵になります。 コントロールとの対話は、 `IAccessible::accSelect` や `IAccessible::accDoDefaultAction`などの固定のメソッドを使用して実現されます。 クライアント アプリケーションと [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] の間の対話は、 `IAccessible`を通して実行できる範囲に限定されます。  
  
 これに対して、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] では、要素のコントロール型 ( <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> プロパティで記述) と、その要素に期待される機能とが大きく分離されています。 機能は、特殊なインターフェイスの実装を通じてプロバイダーによってサポートされる、コントロール パターンによって決定されます。 複数のコントロール パターンを組み合わせると、特定の [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 要素がサポートする機能をすべて記述することができます。 プロバイダーによっては、ある 1 つの特定のコントロール パターンをサポートしなければならないものがあります。たとえば、チェック ボックスのプロバイダーは Toggle コントロール パターンをサポートする必要があります。 また、コントロール パターンのセットを 1 つ以上サポートしなければならないものもあります。たとえば、ボタンは、Toggle と Invoke のいずれかをサポートする必要があります。 さらに、コントロール パターンをまったくサポートしないものもあります。たとえば、移動もサイズ変更もドッキングもできないペインには、コントロール パターンはありません。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、 <xref:System.Windows.Automation.ControlType.Custom> プロパティによって識別され、 <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> プロパティによって記述可能なカスタム コントロールをサポートします。  
  
 次の表は、Active Accessibility ロールと [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コントロール型とのマッピングを示しています。  
  
|Active Accessibility ロール|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のコントロール型|  
|----------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
|ROLE_SYSTEM_PUSHBUTTON|ボタン|  
|ROLE_SYSTEM_CLIENT|予定表|  
|ROLE_SYSTEM_CHECKBUTTON|チェック ボックス|  
|ROLE_SYSTEM_COMBOBOX|コンボ ボックス|  
|ROLE_SYSTEM_CLIENT|カスタム|  
|ROLE_SYSTEM_LIST|データ グリッド|  
|ROLE_SYSTEM_LISTITEM|データ項目|  
|ROLE_SYSTEM_DOCUMENT|ドキュメント|  
|ROLE_SYSTEM_TEXT|編集|  
|ROLE_SYSTEM_GROUPING|グループ化|  
|ROLE_SYSTEM_LIST|Header|  
|ROLE_SYSTEM_COLUMNHEADER|ヘッダー項目|  
|ROLE_SYSTEM_LINK|ハイパーリンク|  
|ROLE_SYSTEM_GRAPHIC|イメージ|  
|ROLE_SYSTEM_LIST|リスト|  
|ROLE_SYSTEM_LISTITEM|リスト項目|  
|ROLE_SYSTEM_MENUPOPUP|メニュー|  
|ROLE_SYSTEM_MENUBAR|メニュー バー|  
|ROLE_SYSTEM_MENUITEM|メニュー項目|  
|ROLE_SYSTEM_PANE|ペイン|  
|ROLE_SYSTEM_PROGRESSBAR|進行状況バー|  
|ROLE_SYSTEM_RADIOBUTTON|オプション ボタン|  
|ROLE_SYSTEM_SCROLLBAR|スクロール バー|  
|ROLE_SYSTEM_SEPARATOR|区切り記号|  
|ROLE_SYSTEM_SLIDER|スライダー|  
|ROLE_SYSTEM_SPINBUTTON|Spinner|  
|ROLE_SYSTEM_SPLITBUTTON|分割ボタン|  
|ROLE_SYSTEM_STATUSBAR|ステータス バー|  
|ROLE_SYSTEM_PAGETABLIST|タブ|  
|ROLE_SYSTEM_PAGETAB|タブ項目|  
|ROLE_SYSTEM_TABLE|テーブル|  
|ROLE_SYSTEM_STATICTEXT|テキスト|  
|ROLE_SYSTEM_INDICATOR|つまみ|  
|ROLE_SYSTEM_TITLEBAR|タイトル バー|  
|ROLE_SYSTEM_TOOLBAR|ツール バー|  
|ROLE_SYSTEM_TOOLTIP|ヒント|  
|ROLE_SYSTEM_OUTLINE|ツリー|  
|ROLE_SYSTEM_OUTLINEITEM|ツリー項目|  
|ROLE_SYSTEM_WINDOW|[Window]|  
  
 さまざまなコントロール型の詳細については、「 [UI オートメーション コントロール型](ui-automation-control-types.md)」を参照してください。  
  
<a name="States_and_Properties"></a>   
## <a name="states-and-properties"></a>状態とプロパティ  
 Active Accessibility では、要素は共通のプロパティセットをサポートし、一部のプロパティ (`accState`など) は、要素のロールに応じて、非常に異なる項目を記述する必要があります。 サーバーは、プロパティを返す `IAccessible` のメソッドをすべて (要素に無関係なものも含む) 実装する必要があります。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] では、さらに多くのプロパティが定義されていますが、その一部は Active Accessibility の状態に対応しています。 すべての要素に共通するものもあれば、コントロール型とコントロール パターンに固有のものもあります。 プロパティは一意の識別子によって識別され、ほとんどのプロパティは単一のメソッド ( <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> または <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>) を使用して取得できます。 多くのプロパティは、 <xref:System.Windows.Automation.AutomationElement.Current%2A> プロパティ アクセサーおよび <xref:System.Windows.Automation.AutomationElement.Cached%2A> プロパティ アクセサーからも容易に取得できます。  
  
 UI オートメーション プロバイダーは、無関係なプロパティを実装する必要はなく、サポートしていないプロパティに対しては単に `null` 値を返すことができます。 また、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のコア サービスは、一部のプロパティを既定のウィンドウ プロバイダーから取得できます。これらのプロパティは、プロバイダーによって明示的に実装されたプロパティと 1 つにまとめられます。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] では、多くのプロパティをサポートすることに加えて、単一のプロセス間呼び出しで複数のプロパティを取得できるので、パフォーマンスが向上します。  
  
 次の表に、2 つのモデルのプロパティ間の対応を示します。  
  
|Active Accessibility プロパティアクセサー|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のプロパティ ID|Remarks|  
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------|  
|`get_accKeyboardShortcut`|<xref:System.Windows.Automation.AutomationElement.AccessKeyProperty> または <xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>|両方とも存在する場合は`AccessKeyProperty` が優先されます。|  
|`get_accName`|<xref:System.Windows.Automation.AutomationElement.NameProperty>||  
|`get_accRole`|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty>|役割とコントロール型のマッピングについては、前の表を参照してください。|  
|`get_accValue`|<xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType>|ValuePattern または RangeValuePattern をサポートするコントロール型でのみ有効です。 MSAA 動作との一貫性を保つために、RangeValue 値は 0 ～ 100 に正規化されます。 Value 項目は文字列を使用します。|  
|`get_accHelp`|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>||  
|`accLocation`|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>||  
|`get_accDescription`|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|`accDescription` に関する明確な仕様が MSAA 内に存在しなかったので、このプロパティに格納される情報はプロバイダーによって異なります。|  
|`get_accHelpTopic`|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]||  
  
 次の表は、Active Accessibility 状態定数に対応する [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティを示しています。  
  
|Active Accessibility の状態|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティ|状態の変更をトリガーする|  
|-----------------------------------------------------------------------|------------------------------------------------------------------------------------|----------------------------|  
|STATE_SYSTEM_CHECKED|チェック ボックスの場合、 <xref:System.Windows.Automation.TogglePattern.ToggleStateProperty><br /><br /> オプション ボタンの場合、 <xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|Y|  
|STATE_SYSTEM_COLLAPSED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Collapsed>|Y|  
|STATE_SYSTEM_EXPANDED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Expanded> または <xref:System.Windows.Automation.ExpandCollapseState.PartiallyExpanded>|Y|  
|STATE_SYSTEM_FOCUSABLE|<xref:System.Windows.Automation.AutomationElement.IsKeyboardFocusableProperty>|N|  
|STATE_SYSTEM_FOCUSED|<xref:System.Windows.Automation.AutomationElement.HasKeyboardFocusProperty>|N|  
|STATE_SYSTEM_HASPOPUP|<xref:System.Windows.Automation.ExpandCollapsePattern> (メニュー項目の場合)|N|  
|STATE_SYSTEM_INVISIBLE|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> = True かつ <xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A> の場合に <xref:System.Windows.Automation.NoClickablePointException>|N|  
|STATE_SYSTEM_LINKED|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> =<br /><br /> <xref:System.Windows.Automation.ControlType.Hyperlink>|N|  
|STATE_SYSTEM_MIXED|<xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> = <xref:System.Windows.Automation.ToggleState.Indeterminate>|N|  
|STATE_SYSTEM_MOVEABLE|<xref:System.Windows.Automation.TransformPattern.CanMoveProperty>|N|  
|STATE_SYSTEM_MUTLISELECTABLE|<xref:System.Windows.Automation.SelectionPattern.CanSelectMultipleProperty>|N|  
|STATE_SYSTEM_OFFSCREEN|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> = True|N|  
|STATE_SYSTEM_PROTECTED|<xref:System.Windows.Automation.AutomationElement.IsPasswordProperty>|N|  
|STATE_SYSTEM_READONLY|<xref:System.Windows.Automation.RangeValuePattern.IsReadOnlyProperty?displayProperty=nameWithType> および <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty?displayProperty=nameWithType>|N|  
|STATE_SYSTEM_SELECTABLE|<xref:System.Windows.Automation.SelectionItemPattern> がサポートされています|N|  
|STATE_SYSTEM_SELECTED|<xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|N|  
|STATE_SYSTEM_SIZEABLE|<xref:System.Windows.Automation.TransformPattern.TransformPatternInformation.CanResize%2A>|N|  
|STATE_SYSTEM_UNAVAILABLE|<xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>|Y|  
  
 次の状態は、ほとんどの Active Accessibility コントロールサーバーで実装されていないか、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]に同等のものがありませんでした。  
  
|Active Accessibility の状態|Remarks|  
|-----------------------------------------------------------------------|-------------|  
|STATE_SYSTEM_BUSY|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_DEFAULT|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_ANIMATED|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_EXTSELECTABLE|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_MARQUEED|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_SELFVOICING|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_TRAVERSED|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_ALERT_HIGH|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_ALERT_MEDIUM|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_ALERT_LOW|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_FLOATING|Active Accessibility サーバーで広く実装されていない|  
|STATE_SYSTEM_HOTTRACKED|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_PRESSED|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティ識別子の完全な一覧については、「 [UI オートメーションプロパティの概要](ui-automation-properties-overview.md)」を参照してください。  
  
<a name="uiautomation_events_compare"></a>   
## <a name="events"></a>イベント  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]のイベントメカニズムは、Active Accessibility とは異なり、Windows イベントルーティング (ウィンドウハンドルと密接に関連付けられています) に依存せず、クライアントアプリケーションでフックを設定する必要はありません。 イベントのサブスクリプションは、特定のイベントに対してだけでなく、ツリーの特定の部分を対象とするように細かく調整できます。 プロバイダーも、リッスンされているイベントを追跡して、イベントの生成を細かく調整できます。  
  
 イベントを生成した要素をクライアントで取得することも簡単です。このような要素は、イベント コールバックに直接渡されるからです。 クライアントがイベントをサブスクライブしているときにキャッシュ要求がアクティブであった場合は、自動的に要素のプロパティがプリフェッチされます。  
  
 次の表は、Active Accessibility WinEvents と [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントの対応を示しています。  
  
|WinEvent|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のイベント識別子|  
|--------------|--------------------------------------------------------------------------------------------|  
|EVENT_OBJECT_ACCELERATORCHANGE|<xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty> プロパティの変更|  
|EVENT_OBJECT_CONTENTSCROLLED|関連付けられたスクロール バーにおける<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> または <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> プロパティの変更|  
|EVENT_OBJECT_CREATE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_DEFACTIONCHANGE|同等の機能がありません|  
|EVENT_OBJECT_DESCRIPTIONCHANGE|まったく同等の項目はありません (おそらく <xref:System.Windows.Automation.AutomationElement.HelpTextProperty> または <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> プロパティの変更)|  
|EVENT_OBJECT_DESTROY|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_FOCUS|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_OBJECT_HELPCHANGE|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty> の変更|  
|EVENT_OBJECT_HIDE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_LOCATIONCHANGE|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> プロパティの変更|  
|EVENT_OBJECT_NAMECHANGE|<xref:System.Windows.Automation.AutomationElement.NameProperty> プロパティの変更|  
|EVENT_OBJECT_PARENTCHANGE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_REORDER|Active Accessibility では一貫して使用されません。 直接対応するイベントが [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]では定義されていません。|  
|EVENT_OBJECT_SELECTION|<xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent>|  
|EVENT_OBJECT_SELECTIONADD|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent>|  
|EVENT_OBJECT_SELECTIONREMOVE|<xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent>|  
|EVENT_OBJECT_SELECTIONWITHIN|同等の機能がありません|  
|EVENT_OBJECT_SHOW|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_STATECHANGE|さまざまなプロパティ変更イベント|  
|EVENT_OBJECT_VALUECHANGE|<xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType> および <xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType> の変更|  
|EVENT_SYSTEM_ALERT|同等の機能がありません|  
|EVENT_SYSTEM_CAPTUREEND|同等の機能がありません|  
|EVENT_SYSTEM_CAPTURESTART|同等の機能がありません|  
|EVENT_SYSTEM_CONTEXTHELPEND|同等の機能がありません|  
|EVENT_SYSTEM_CONTEXTHELPSTART|同等の機能がありません|  
|EVENT_SYSTEM_DIALOGEND|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent>|  
|EVENT_SYSTEM_DIALOGSTART|<xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent>|  
|EVENT_SYSTEM_DRAGDROPEND|同等の機能がありません|  
|EVENT_SYSTEM_DRAGDROPSTART|同等の機能がありません|  
|EVENT_SYSTEM_FOREGROUND|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_SYSTEM_MENUEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MENUSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MINIMIZEEND|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> プロパティの変更|  
|EVENT_SYSTEM_MINIMIZESTART|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> プロパティの変更|  
|EVENT_SYSTEM_MOVESIZEEND|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> プロパティの変更|  
|EVENT_SYSTEM_MOVESIZESTART|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> プロパティの変更|  
|EVENT_SYSTEM_SCROLLINGEND|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> または <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> プロパティの変更|  
|EVENT_SYSTEM_SCROLLINGSTART|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> または <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> プロパティの変更|  
|EVENT_SYSTEM_SOUND|同等の機能がありません|  
|EVENT_SYSTEM_SWITCHEND|同等の項目はありませんが、新しいアプリケーションがフォーカスを受け取ったことは <xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent> イベントによって通知されます。|  
|EVENT_SYSTEM_SWITCHSTART|同等の機能がありません|  
|同等の機能がありません|<xref:System.Windows.Automation.MultipleViewPattern.CurrentViewProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.HorizontallyScrollableProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.VerticallyScrollableProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.HorizontalViewSizeProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.ScrollPattern.VerticalViewSizeProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.TogglePattern.ToggleStateProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> プロパティの変更|  
|同等の機能がありません|<xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent> イベント|  
|同等の機能がありません|<xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent>|  
  
<a name="Security_compare"></a>   
## <a name="security"></a>セキュリティ  
 `IAccessible` をカスタマイズするシナリオでは、基本 `IAccessible` をラップしてからこれに対する呼び出しを行うという要件が生じることがあります。 このことは、セキュリティに影響を及ぼします。部分信頼コンポーネントにコード パスを中継させてはならないからです。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] モデルでは、プロバイダーが他のプロバイダー コードを呼び出す必要がありません。 必要な集約はすべて [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コア サービスが行います。  
  
## <a name="see-also"></a>関連項目

- [UI オートメーションの基礎](index.md)
