---
title: サーバー側 UI オートメーション プロバイダーの実装
ms.date: 03/30/2017
helpviewer_keywords:
- server-side UI Automation provider implementation
- UI Automation, server-side provider implementation
- provider implementation, UI Automation
ms.assetid: 6acc6d08-bd67-4e2e-915c-9c1d34eb86fe
ms.openlocfilehash: 8a52d84f7152b9cb431ad0aa97c88b143463be2d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789623"
---
# <a name="server-side-ui-automation-provider-implementation"></a>サーバー側 UI オートメーション プロバイダーの実装

> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。

このセクションでは、カスタム コントロールのサーバー側 UI オートメーション プロバイダーを実装する方法について説明します。

Windows Presentation Foundation (WPF) 要素と非 WPF 要素 (Windows フォーム向けに設計された要素など) の実装は、根本的に異なります。 WPF 要素は、<xref:System.Windows.Automation.Peers.AutomationPeer>から派生したクラスを使用した [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のサポートを提供します。 非 WPF 要素は、プロバイダーインターフェイスの実装を通じてサポートを提供します。

<a name="Security_Considerations"></a>

## <a name="security-considerations"></a>セキュリティの考慮事項

プロバイダーは、部分的に信頼された環境で動作できるように記述する必要があります。 UIAutomationClient.dll は部分的な信頼で動作するように構成されていないため、プロバイダーのコードではこのアセンブリを参照しないでください。 参照している場合、完全に信頼された環境ではコードを実行できますが、部分的に信頼された環境では失敗します。

具体的には、 <xref:System.Windows.Automation.AutomationElement>などの UIAutomationClient.dll のクラスのフィールドを使用しないでください。 代わりに、 <xref:System.Windows.Automation.AutomationElementIdentifiers>などの UIAutomationTypes.dll のクラスの同等のフィールドを使用します。

<a name="Provider_Implementation_by_WPF_Elements"></a>

## <a name="provider-implementation-by-windows-presentation-foundation-elements"></a>Windows Presentation Foundation 要素によるプロバイダーの実装

このトピックの詳細については、「 [WPF カスタム コントロールの UI オートメーション](../wpf/controls/ui-automation-of-a-wpf-custom-control.md)」を参照してください。

<a name="Provider_Implementation_by_non_WPF_Elements"></a>

## <a name="provider-implementation-by-non-wpf-elements"></a>非 WPF 要素によるプロバイダーの実装

WPF フレームワークの一部ではないが、マネージコードで記述されたカスタムコントロール (多くの場合、これらは Windows フォームコントロール)、インターフェイスを実装することによって [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のサポートを提供します。 すべての要素は、次のセクションの最初のテーブルに示されているインターフェイスを 1 つ以上実装する必要があります。 さらに、要素が 1 つ以上のコントロール パターンをサポートする場合、コントロール パターンごとに適切なインターフェイスを実装する必要があります。

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダー プロジェクトは、次のアセンブリを参照する必要があります。

- UIAutomationProviders.dll

- UIAutomationTypes.dll

- WindowsBase.dll

<a name="Provider_Interfaces"></a>

### <a name="provider-interfaces"></a>プロバイダーのインターフェイス

すべての [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーは、次のインターフェイスのいずれかを実装する必要があります。

|Interface|説明|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderSimple>|コントロール パターンやプロパティのサポートを含む、ウィンドウでホストされる単純なコントロールの機能を提供します。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragment>|<xref:System.Windows.Automation.Provider.IRawElementProviderSimple>から継承します。 フラグメント内のナビゲーション、フォーカスの設定、要素の外接する四角形の復帰などを含む、複雑なコントロールの要素の機能を追加します。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>|<xref:System.Windows.Automation.Provider.IRawElementProviderFragment>から継承します。 指定した座標での子要素の検索やコントロール全体のフォーカス状態の設定などを含む、複雑なコントロールのルート要素の機能を追加します。|

次のインターフェイスは追加機能を提供しますが、実装する必要はありません。

|Interface|説明|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|プロバイダーがイベントの要求を追跡できるようにします。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride>|フラグメントの [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー内のウィンドウ ベースの要素の位置を変更できるようにします。|

<xref:System.Windows.Automation.Provider> 名前空間内の他のすべてのインターフェイスは、コントロール パターンをサポートします。

<a name="Requirements_for_Non_WPF_Providers"></a>

### <a name="requirements-for-non-wpf-providers"></a>非 WPF プロバイダーの要件

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]と通信するために、コントロールは次の主な領域の機能を実装する必要があります。

|機能|実装|
|-------------------|--------------------|
|プロバイダーを [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]に公開する|コントロール ウィンドウに送信された WM_GETOBJECT メッセージの応答として、 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple> (または派生インターフェイス) を実装するオブジェクトを返します。 フラグメントの場合、これはフラグメント ルートのプロバイダーである必要があります。|
|プロパティ値を指定する|値を指定またはオーバーライドする <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPropertyValue%2A> を実装します。|
|クライアントがコントロールと対話できるようにする|<xref:System.Windows.Automation.Provider.IInvokeProvider>などのコントロール パターンをサポートするインターフェイスを実装します。 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPatternProvider%2A>の実装でこれらのパターンのプロバイダーを返します。|
|イベントを発生させる|<xref:System.Windows.Automation.Provider.AutomationInteropProvider> のメソッドの 1 つを呼び出して、クライアントがリッスンできるイベントを発生させます。|
|フラグメント内のナビゲーションとフォーカス設定を有効にする|フラグメント内の各要素に <xref:System.Windows.Automation.Provider.IRawElementProviderFragment> を実装します (フラグメントの一部でない要素に対しては必要ありません)。|
|フラグメント内のフォーカス設定と子要素の配置を有効にする|<xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>を実装します。 (フラグメント ルートでない要素に対しては必要ありません)。|

<a name="Property_Values_in_Non_WPF_Providers"></a>

### <a name="property-values-in-non-wpf-providers"></a>非 WPF プロバイダー内のプロパティ値

カスタム コントロールの[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーは、オートメーション システムおよびクライアント アプリケーションで使用できる特定のプロパティをサポートする必要があります。 ウィンドウでホストされている (HWND) 要素の場合、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は一部のプロパティを既定のウィンドウ プロバイダーから取得できますが、他のプロパティはカスタム プロバイダーから取得する必要があります。

HWND ベースのコントロールのプロバイダーは通常、次のプロパティを指定する必要はありません (フィールド値で識別されます)。

- <xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ProcessIdProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClassNameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.HasKeyboardFocusProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsPasswordProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty>

> [!NOTE]
> 単純な要素またはウィンドウでホストされているフラグメント ルートの <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty> は、ウィンドウから取得されます。ただし、ルートの下にあるフラグメント要素 (リスト ボックス内のリスト項目など) は独自の識別子を提供する必要があります。 詳細については、「 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.GetRuntimeId%2A>」を参照してください。
>
> Windows フォームコントロールでホストされているプロバイダーに対して <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty> が返される必要があります。 この場合、ウィンドウの既定のプロバイダーは適切な値を取得できないことがあります。
>
> <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> は通常、ホスト プロバイダーによって提供されます。 たとえば、カスタム コントロールが <xref:System.Windows.Forms.Control>から派生している場合、名前はこのコントロールの `Text` プロパティから派生します。

コード例については、「 [UI オートメーション プロバイダーからのプロパティの返却](return-properties-from-a-ui-automation-provider.md)」を参照してください。

<a name="Events_in_Non_WPF_Providers"></a>

### <a name="events-in-non-wpf-providers"></a>非 WPF プロバイダー内のイベント

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーは、イベントを発生させて、UI の状態の変更をクライアント アプリケーションに通知する必要があります。 イベントを発生させるには、次のメソッドを使用します。

|メソッド|説明|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationEvent%2A>|コントロール パターンによってトリガーされるイベントを含む、さまざまなイベントを発生させます。|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationPropertyChangedEvent%2A>|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティが変更された場合にイベントを発生させます。|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseStructureChangedEvent%2A>|要素の追加や削除などによって [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの構造が変更された場合にイベントを発生させます。|

イベントの目的は、 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)]システム自体によってトリガーされたアクティビティかどうかにかかわらず、何かが [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 内で行われたことをクライアントに通知することです。 たとえば、 <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent> によって識別されるイベントは、ユーザーの直接入力の場合でもクライアント アプリケーションの <xref:System.Windows.Automation.InvokePattern.Invoke%2A>呼び出しの場合でも、コントロールが呼び出されるたびに発生させる必要があります。

パフォーマンスを最適化するため、プロバイダーは選択的にイベントを発生させたり、イベントを受け取るクライアント アプリケーションが登録されていないときにはイベントを発生させないようにすることができます。 最適化には、次のメソッドを使用します。

|メソッド|説明|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.ClientsAreListening%2A>|この静的プロパティは、クライアント アプリケーションが [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントにサブスクライブしているかどうかを指定します。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|プロバイダーがこのインターフェイスをフラグメント ルートに実装すると、クライアントが フラグメント上のイベン用のイベント ハンドラーを登録および登録解除したときに通知されるようにすることができます。|

<a name="Non_WPF_Provider_Navigation"></a>

### <a name="non-wpf-provider-navigation"></a>非 WPF プロバイダーのナビゲーション

ウィンドウでホストされる (HWND) カスタム ボタンなどの単純なコントロールのプロバイダーは、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー内のナビゲーションをサポートする必要はありません。 要素間のナビゲーションは、ホスト ウィンドウの既定のプロバイダーによって処理されます。これは、 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>の実装で指定されます。 ただし、複雑なカスタム コントロール用のプロバイダーを実装する場合は、フラグメントのルート ノードと子孫ノード、および兄弟ノード間のナビゲーションをサポートする必要があります。

> [!NOTE]
> ルート以外のフラグメントの要素は、 `null` から <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>参照を返す必要があります。これらが直接ウィンドウでホストされておらず、これらの間のナビゲーションをサポートできる既定のプロバイダーがないためです。

フラグメントの構造は、 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A>の実装によって決まります。 このメソッドは、各フラグメントから可能なそれぞれの方向に対して、その方向の要素に対するプロバイダー オブジェクトを返します。 その方向に要素がない場合、メソッドは `null` 参照を返します。

フラグメント ルートは、子要素へのナビゲーションのみをサポートします。 たとえば、リスト ボックスは、方向が <xref:System.Windows.Automation.Provider.NavigateDirection.FirstChild>の場合はリスト内の最初の項目を、方向が <xref:System.Windows.Automation.Provider.NavigateDirection.LastChild>の場合は最後の項目を返します。 フラグメント ルートは、親または兄弟へのナビゲーションをサポートしません。これはホスト ウィンドウ プロバイダーによって処理されます。

ルート以外のフラグメントの要素は、親へのナビゲーションと、(存在する場合は) 兄弟や子へのナビゲーションをサポートする必要があります。

<a name="Non_WPF_Provider_Reparenting"></a>

### <a name="non-wpf-provider-reparenting"></a>非 WPF プロバイダーの親の変更

ポップアップ ウィンドウは、実際には最上位のウィンドウであり、既定ではデスクトップの子として [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーに表示されます。 ただし、多くの場合、ポップアップ ウィンドウは論理的には他のコントロールの子になります。 たとえば、コンボ ボックスのドロップダウン リストは、論理的にはコンボ ボックスの子です。 同様に、メニューのポップアップ ウィンドウは、論理的にはメニューの子になります。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] はポップアップ ウィンドウの親の変更をサポートしているため、ポップアップ ウィンドウを関連するコントロールの子として表示できます。

ポップアップ ウィンドウの親を変更するには:

1. ポップアップ ウィンドウ用のプロバイダーを作成します。 これには、ポップアップ ウィンドウのクラスを事前に知っておく必要があります。

2. そのポップアップ自体がコントロールであるかのように、ポップアップのすべてのプロパティとパターンを通常どおりに実装します。

3. <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> プロパティを実装し、 <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A>から取得した値を返せるようにします。パラメーターは、ポップアップ ウィンドウのウィンドウ ハンドルです。

4. ポップアップ ウィンドウとその親の <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> を実装し、論理上の親から論理上の子へのナビゲーションおよび子の兄弟間のナビゲーションが適切に処理されるようにします。

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は、ポップアップ ウィンドウを検出すると、既定のナビゲーションがオーバーライドされていると認識し、デスクトップの子として検出されたポップアップ ウィンドウをスキップします。 代わりに、ノードはフラグメントによってのみ、到達可能になります。

コントロールが任意のクラスのウィンドウをホストできる場合には、親の変更は適切ではありません。 たとえば、Rebar はそのバンド内で任意の型の HWND をホストできます。 このような場合を処理するため、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は次のセクションで説明する、別の形の HWND 再配置をサポートしています。

<a name="Non_WPF_Provider_Repositioning"></a>

### <a name="non-wpf-provider-repositioning"></a>非 WPF プロバイダーの再配置

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] フラグメントは、それぞれウィンドウ (HWND) に含まれる 2 つ以上の要素を含んでいる場合があります。 各 HWND には独自の既定のプロバイダーがあり、その HWND を含む HWND をその親と見なしているため、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーは既定で、HWND を親ウィンドウの子としてフラグメント内に表示します。 ほとんどの場合、これは適切な動作ですが、場合によっては [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]の論理構造が一致しないために混乱を招く可能性があります。

良い例が Rebar コントロールです。 Rebar はバンドを格納し、各バンドはツール バー、編集ボックス、コンボ ボックスなど HWND ベースのコントロールを格納します。 Rebar HWND の既定のウィンドウ プロバイダーは、バンド コントロール HWND を子と見なし、Rebar プロバイダーはバンドを子と見なします。 HWND プロバイダーと Rebar プロバイダーは連動し、互いの子を結合するため、バンドと HWND ベースのコントロールはどちらも Rebar の子として表示されます。 ただし、論理的には、バンドのみが Rebar の子として表示され、各バンド プロバイダーは、格納されるコントロールの既定の HWND プロバイダーと結合される必要があります。

これを実現するため、Rebar のフラグメント ルート プロバイダーは、バンドを表す子のセットを公開します。 各バンドには、プロパティとパターンを公開するプロバイダーが 1 つあります。 その <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>の実装で、バンド プロバイダーはコントロール HWND の既定のウィンドウ プロバイダーを返します。これを取得するには、コントロールのウィンドウ ハンドルを渡して <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A>を呼び出します。 最後に、Rebar のフラグメント ルート プロバイダーは <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride> インターフェイスを実装し、その <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride.GetOverrideProviderForHwnd%2A> の実装で、指定した HWND に含まれるコントロールの適切なバンド プロバイダーを返します。

## <a name="see-also"></a>関連項目

- [UI オートメーション プロバイダーの概要](ui-automation-providers-overview.md)
- [サーバー側 UI オートメーション プロバイダーの公開](expose-a-server-side-ui-automation-provider.md)
- [UI オートメーション プロバイダーからのプロパティの返却](return-properties-from-a-ui-automation-provider.md)
- [UI オートメーション プロバイダーからのイベントの発生](raise-events-from-a-ui-automation-provider.md)
- [UI オートメーション フラグメント プロバイダーでのナビゲーションの有効化](enable-navigation-in-a-ui-automation-fragment-provider.md)
- [UI オートメーション プロバイダーでのコントロール パターンのサポート](support-control-patterns-in-a-ui-automation-provider.md)
