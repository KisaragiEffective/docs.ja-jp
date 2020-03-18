---
title: UI オートメーションによる MenuItem コントロール型のサポート
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Menu Item
- Menu Item control type
- UI Automation, Menu Item control type
ms.assetid: 54bce311-3d23-40b9-ba90-1bdbdaf8fbba
ms.openlocfilehash: ab050b3d515be325694d6e92ef1891d44cb309c7
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789440"
---
# <a name="ui-automation-support-for-the-menuitem-control-type"></a>UI オートメーションによる MenuItem コントロール型のサポート

> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。

このトピックでは、MenuItem コントロール型の [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] サポートについて説明します。 ここではコントロールの [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] ツリー構造について説明し、MenuItem コントロール型に必要なプロパティとコントロール パターンを示します。

メニュー コントロールを使用すると、コマンドおよびイベント ハンドラーに関連付けられている要素を階層に編成できます。 一般的な Microsoft Windows アプリケーションでは、メニューバーに複数のメニュー項目 ( **[ファイル]** 、 **[編集]** 、 **[ウィンドウ]** など) が含まれ、各メニュー項目にメニューが表示されます。 メニューには、メニュー項目 ( **[新規]** 、 **[開く]** 、 **[閉じる]** など) のコレクションが含まれていて、クリックされると展開して追加のメニュー項目を表示したり、特定の操作を実行したりできます。 メニュー項目は、メニュー、メニュー バー、またはツール バーに配置できます。

以下のセクションでは、MenuItem コントロール型で必要な [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー構造、プロパティ、コントロール パターン、イベントを定義します。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の要件は、[!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)]、Win32、Windows フォームにかかわらず、すべてのリストコントロールに適用されます。

<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必須の UI オートメーション ツリー構造

次の表では、メニュー項目コントロールに関連する [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューとコンテンツ ビューを示し、各ビューに含めることができる内容について説明します。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]ツリーの詳細については、[UI Automation Tree Overview](ui-automation-tree-overview.md)をご覧ください。

|コントロール ビュー|コンテンツ ビュー|
|------------------|------------------|
|MenuItem "Help"<br /><br /> <ul><li>Menu (Help メニュー項目のサブ メニュー)<br /><br /> <ul><li>MenuItem "Help Topics"</li><li>MenuItem "About Notepad"</li></ul></li></ul>|MenuItem "Help"<br /><br /> -MenuItem "ヘルプトピック"<br />-MenuItem "メモ帳"|

メニュー項目コントロールのコントロール ビューには、上記の [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー構造があります。 **[ヘルプ]** メニュー項目は、一般的なメニューからサブメニュー階層への構造をわかりやすく示すために含まれています。

コンテンツ ビューでは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーにメニューがありません。これは、メニューでは有用な情報がエンド ユーザーに提供されないためです。

<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必須の UI オートメーション プロパティ

次の表に、メニュー項目コントロールに特に関連する値または定義を持つ [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティを示します。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のプロパティの詳細については、「[クライアントの UI オートメーションのプロパティ](ui-automation-properties-for-clients.md)」を参照してください。

|property|Value|説明|
|--------------|-----------|-----------------|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|「ノート」を参照してください。|このプロパティの値は、アプリケーション内のすべてのコントロールで一意である必要があります。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|「ノート」を参照してください。|コントロール全体を格納する最も外側の四角形。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|「ノート」を参照してください。|四角形領域が存在する場合にサポートされます。 四角形領域内にクリック不可能な点が存在し、特別なヒット テストを実行する場合は、オーバーライドしてクリック可能な点を提供します。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|「ノート」を参照してください。|コントロールがキーボード フォーカスを受け取ることができる場合は、このプロパティをサポートする必要があります。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|「ノート」を参照してください。|メニュー項目コントロールは [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューに含まれていて、名前が自動的にラベル付けされます。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|ラベルなし。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|MenuItem|この値は、すべての UI フレームワークで同じです。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"メニュー項目"|MenuItem コントロール型に対応するローカライズされた文字列。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|メニュー項目コントロールは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューには含まれません。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|メニュー項目コントロールは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューに常に含まれている必要があります。|

<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必須の UI オートメーション コントロール パターン

次の表に、メニュー項目コントロールでサポートする必要がある [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コントロール パターンの一覧を示します。 コントロール パターンの詳細については、「 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)」を参照してください。

|コントロール パターン プロパティ|でのサポート|メモ|
|------------------------------|-------------|-----------|
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|状況に依存|コントロールを展開または折りたたむことができる場合は、 <xref:System.Windows.Automation.Provider.IExpandCollapseProvider>を実装します。|
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|状況に依存|コントロールが単一のアクションまたはコマンドを実行する場合は、 <xref:System.Windows.Automation.Provider.IInvokeProvider>を実装します。|
|<xref:System.Windows.Automation.Provider.IToggleProvider>|状況に依存|コントロールがオンまたはオフを切り替えられるオプションを表す場合は、 <xref:System.Windows.Automation.Provider.IToggleProvider>を実装します。|
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|状況に依存|コントロールを使用してメニュー項目のオプションのリストから選ぶ場合は、 <xref:System.Windows.Automation.Provider.ISelectionItemProvider>を実装します。|

<a name="UI_Automation_Events_for_Menu_Item"></a>

## <a name="ui-automation-events-for-menu-item"></a>メニュー項目の UI オートメーション イベント

次の表に、メニュー項目コントロールに関連付けられている [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] イベントの一覧を示します。

|Event|でのサポート|説明|
|-----------|-------------|-----------------|
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|状況に依存|コントロールが呼び出しコントロール パターンをサポートしている場合に、発生させる必要があります。|
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty> プロパティ変更イベント。|状況に依存|コントロールがトグル コントロール パターンをサポートしている場合に、発生させる必要があります。|
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> プロパティ変更イベント。|状況に依存|コントロールが展開/折りたたみコントロール パターンをサポートしている場合に、発生させる必要があります。|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|状況に依存|ありません。|

<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必須の UI オートメーション イベント

次の表に、すべてのメニュー項目コントロールでサポートする必要がある [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントの一覧を示します。 イベントの詳細については、「 [UI Automation Events Overview](ui-automation-events-overview.md)」を参照してください。

|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベント|サポート/値|メモ|
|---------------------------------------------------------------------------------|--------------------|-----------|
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|状況に依存|[なし]|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|状況に依存|[なし]|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|状況に依存|[なし]|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|状況に依存|[なし]|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> プロパティ変更イベント。|必須|[なし]|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> プロパティ変更イベント。|必須|[なし]|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> プロパティ変更イベント。|必須|[なし]|
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> プロパティ変更イベント。|状況に依存|[なし]|
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty> プロパティ変更イベント。|状況に依存|[なし]|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必須|[なし]|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必須|[なし]|

<a name="Legacy_Issues"></a>

## <a name="legacy-issues"></a>既知の問題

トグルパターンは、Win32 メニュー項目がチェックされ、トグルパターンをサポートするために必要なプログラムによって判断できる場合にのみサポートされます。 Win32 メニュー項目はチェックできるかどうかを公開しないため、メニュー項目がチェックされない場合、Invoke Pattern がサポートされます。 トグル パターンのみをサポートするべきメニュー項目に対して常に呼び出しパターンをサポートするには、例外が作成されます。 このようにすると、(メニュー項目がクリアされていたときに) 呼び出しパターンをサポートしていた要素が、チェックされたときにそのパターンをサポートしなくなっても、クライアントは混乱しません。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Automation.ControlType.MenuItem>
- [UI Automation コントロール パターンの概要](ui-automation-control-patterns-overview.md)
- [UI オートメーション コントロール型の概要](ui-automation-control-types-overview.md)
- [UI オートメーションの概要](ui-automation-overview.md)
