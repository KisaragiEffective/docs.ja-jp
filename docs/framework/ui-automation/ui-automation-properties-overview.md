---
title: UI オートメーション プロパティの概要
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, properties
- properties, UI Automation
ms.assetid: a6c31d7b-b33e-49b3-b5c1-31a345f9b7c8
ms.openlocfilehash: 2f96f3c7261882af58cd10038d729c4e723d6fa0
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447957"
---
# <a name="ui-automation-properties-overview"></a>UI オートメーション プロパティの概要
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 UI オートメーション プロバイダーは、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 要素のプロパティを公開します。 これらのプロパティにより、UI オートメーション クライアント アプリケーションは、静的データと動的データを含め、 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)]の構成部分 (特にコントロール) に関する情報を探索できます。  
  
 このセクションでは、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] プロパティの概要を説明します。 詳細については、以下のトピックで説明しています。  
  
- [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)  
  
- [サーバー側 UI オートメーション プロバイダーの実装](server-side-ui-automation-provider-implementation.md)  
  
<a name="Property_Identifiers"></a>   
## <a name="property-identifiers"></a>プロパティ識別子  
 すべてのプロパティは、番号と名前によって識別されます。 プロパティの名前を使用するのは、デバッグおよび診断を行う場合のみです。 プロバイダーは、数値 Id を使用して、受信プロパティ要求を識別します。 ただし、クライアント アプリケーションは、番号と名前をカプセル化した <xref:System.Windows.Automation.AutomationProperty>だけを使用して、取得したいプロパティを識別します。  
  
 特定のプロパティを表す<xref:System.Windows.Automation.AutomationProperty> オブジェクトは、さまざまなクラスでフィールドとして使用できます。 セキュリティ上の理由から、UI オートメーション プロバイダーは、Uiautomationtypes.dll に含まれている別のクラスのセットから、これらのオブジェクトを取得します。  
  
 次の表は、<xref:System.Windows.Automation.AutomationProperty>Id を含むクラスによってプロパティを分類しています。  
  
|プロパティの種類|クライアントが ID を取得する場所|プロバイダーが ID を取得する場所|  
|-------------------------|--------------------------|----------------------------|  
|すべての要素に共通のプロパティ (下記の表を参照)|<xref:System.Windows.Automation.AutomationElement>|<xref:System.Windows.Automation.AutomationElementIdentifiers>|  
|ドッキング ウィンドウの位置|<xref:System.Windows.Automation.DockPattern>|<xref:System.Windows.Automation.DockPatternIdentifiers>|  
|展開または折りたたみが可能な要素の状態|<xref:System.Windows.Automation.ExpandCollapsePattern>|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers>|  
|グリッド内の項目のプロパティ|<xref:System.Windows.Automation.GridItemPattern>|<xref:System.Windows.Automation.GridItemPatternIdentifiers>|  
|グリッドのプロパティ|<xref:System.Windows.Automation.GridPattern>|<xref:System.Windows.Automation.GridPatternIdentifiers>|  
|複数のビューを持つ要素の現在のサポートされているビュー|<xref:System.Windows.Automation.MultipleViewPattern>|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers>|  
|一定の値の範囲を移動する要素 (スライダーなど) のプロパティ|<xref:System.Windows.Automation.RangeValuePattern>|<xref:System.Windows.Automation.RangeValuePatternIdentifiers>|  
|スクロール ウィンドウのプロパティ|<xref:System.Windows.Automation.ScrollPattern>|<xref:System.Windows.Automation.ScrollPatternIdentifiers>|  
|選択できる項目 (リスト内の項目など) の状態およびコンテナー|<xref:System.Windows.Automation.SelectionItemPattern>|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers>|  
|選択項目を含むコントロールのプロパティ|<xref:System.Windows.Automation.SelectionPattern>|<xref:System.Windows.Automation.SelectionPatternIdentifiers>|  
|テーブル内の項目の列と行のヘッダー|<xref:System.Windows.Automation.TableItemPattern>|<xref:System.Windows.Automation.TableItemPatternIdentifiers>|  
|テーブルの列と行のヘッダーおよびテーブルの向き|<xref:System.Windows.Automation.TablePattern>|<xref:System.Windows.Automation.TablePatternIdentifiers>|  
|切り替えコントロールの状態|<xref:System.Windows.Automation.TogglePattern>|<xref:System.Windows.Automation.TogglePatternIdentifiers>|  
|移動、回転、またはサイズ変更できる要素の機能|<xref:System.Windows.Automation.TransformPattern>|<xref:System.Windows.Automation.TransformPatternIdentifiers>|  
|値を持つ要素の値および読み取り/書き込み機能|<xref:System.Windows.Automation.ValuePattern>|<xref:System.Windows.Automation.ValuePatternIdentifiers>|  
|ウィンドウの機能および状態|<xref:System.Windows.Automation.WindowPattern>|<xref:System.Windows.Automation.WindowPatternIdentifiers>|  
  
<a name="Properties_by_Category"></a>   
## <a name="properties-by-category"></a>カテゴリ別プロパティ  
 次の表は、<xref:System.Windows.Automation.AutomationElement> と <xref:System.Windows.Automation.AutomationElementIdentifiers>で Id が見つかったプロパティを分類したものです。 これらのプロパティは、すべてのコントロールに共通です。 一部の例外を除き、ほとんどのプロパティがプロバイダー アプリケーションの有効期間にわたって静的です。動的プロパティのほとんどは、コントロール パターンに関連付けられています。  
  
 **「プロパティ アクセス」** 列には、 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> と <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>だけでなく、各プロパティのすべてのアクセサーを示しています。 クライアント アプリケーションでプロパティを取得する方法の詳細については、「 [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)」を参照してください。  
  
> [!NOTE]
> 各プロパティの詳細については、 **「プロパティ アクセス」** 列のリンク先を参照してください。  
  
### <a name="display-characteristics"></a>[特性の表示]  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.BoundingRectangle%2A>|  
|<xref:System.Windows.Automation.AutomationElement.CultureProperty>|n/a|  
|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HelpText%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsOffscreen%2A>|  
|<xref:System.Windows.Automation.AutomationElement.OrientationProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Orientation%2A>|  
  
### <a name="element-type"></a>Type 要素  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsContentElementProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsControlElementProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ItemTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ItemType%2A>|  
|<xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.LocalizedControlType%2A>|  
  
### <a name="identification"></a>Identification  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AutomationId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ClassNameProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ClassName%2A>|  
|<xref:System.Windows.Automation.AutomationElement.FrameworkIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.FrameworkId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.LabeledByProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.LabeledBy%2A>|  
|<xref:System.Windows.Automation.AutomationElement.NameProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ProcessIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ProcessId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.RuntimeIdProperty>|<xref:System.Windows.Automation.AutomationElement.GetRuntimeId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.NativeWindowHandleProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.NativeWindowHandle%2A>|  
  
### <a name="interaction"></a>［対話］  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AcceleratorKey%2A>|  
|<xref:System.Windows.Automation.AutomationElement.AccessKeyProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AccessKey%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ClickablePointProperty>|<xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A>|  
|<xref:System.Windows.Automation.AutomationElement.HasKeyboardFocusProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HasKeyboardFocus%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsEnabled%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsKeyboardFocusableProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsKeyboardFocusable%2A>|  
  
### <a name="support-for-patterns"></a>パターンのサポート  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.IsDockPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsExpandCollapsePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsGridItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsGridPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsInvokePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsMultipleViewPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsRangeValuePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsScrollItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsScrollPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsSelectionItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsSelectionPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTableItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTablePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTogglePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTransformPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsValuePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsWindowPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
  
### <a name="miscellaneous"></a>その他  
  
|プロパティの識別子|「プロパティ アクセス」|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.IsRequiredForFormProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsRequiredForForm%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsPasswordProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsPassword%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ItemStatusProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ItemStatus%2A>|  
  
<a name="Localization"></a>   
## <a name="localization"></a>ローカリゼーション  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーは、以下のプロパティをオペレーティング システムの言語で提示する必要があります。  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.AcceleratorKeyProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>  
  
<a name="Properties_and_Events"></a>   
## <a name="properties-and-events"></a>プロパティおよびイベント  
 プロパティ変更イベントの概念は、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のプロパティと密接に関連しています。 動的プロパティの場合は、プロパティの値が変更されたときにキャッシュの情報を更新したり新しい情報に何らかの形で対応したりできるように、クライアント アプリケーションは、プロパティの値が変更されたことを認識できなければなりません。  
  
 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] で何らかの変更が発生すると、プロバイダーはイベントを生成します。 たとえば、チェック ボックスのオン/オフが切り替えられると、プロバイダーの Toggle パターンの実装によって、プロパティ変更イベントが生成されます。 プロバイダーは、クライアントがイベントをリッスンしているのか、特定のイベントをリッスンしているのかに応じて、イベントを選択的に生成できます。  
  
 すべてのプロパティ変更がイベントを生成するわけではありません。これは完全に、該当する要素の UI オートメーション プロバイダーの実装に依存します。 たとえば、リスト ボックスの標準プロキシ プロバイダーは、 <xref:System.Windows.Automation.SelectionPattern.SelectionProperty> が変更されてもイベントを生成しません。 この場合、アプリケーションが <xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent>をリッスンする必要があります。  
  
 クライアントは、イベントをサブスクライブすることでイベントをリッスンします。 イベントをサブスクライブすることは、イベントを処理できるデリゲート メソッドを作成し、それらのメソッドで処理する特定のイベントと共に、それらのメソッドを [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] に渡すことを意味します。 特にプロパティ変更イベントについては、クライアントが <xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>を実装する必要があります。  
  
## <a name="see-also"></a>参照

- [Caching in UI Automation Clients](caching-in-ui-automation-clients.md)
- [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)
- [サーバー側 UI オートメーション プロバイダーの実装](server-side-ui-automation-provider-implementation.md)
- [プロパティ条件に基づく UI オートメーション要素の検索](find-a-ui-automation-element-based-on-a-property-condition.md)
- [UI オートメーション プロバイダーからのプロパティの返却](return-properties-from-a-ui-automation-provider.md)
- [UI オートメーション プロバイダーからのイベントの発生](raise-events-from-a-ui-automation-provider.md)
