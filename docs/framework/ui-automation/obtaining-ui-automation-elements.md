---
title: UI オートメーション要素の取得
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, obtaining elements
- elements, UI Automation, obtaining
ms.assetid: c2caaf45-e59c-42a1-bc9b-77a6de520171
ms.openlocfilehash: 0ae4694e2efb6f6c51b279adf2851baf38785c8b
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446892"
---
# <a name="obtaining-ui-automation-elements"></a>UI オートメーション要素の取得
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、 <xref:System.Windows.Automation.AutomationElement> 要素の [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] オブジェクトを取得するさまざまな方法について説明します。  
  
> [!CAUTION]
> クライアント アプリケーションが独自のユーザー インターフェイスで要素の検索を試行する可能性がある場合は、すべての [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の呼び出しを個別のスレッドで実行する必要があります。 詳細については、「 [UI Automation Threading Issues](ui-automation-threading-issues.md)」を参照してください。  
  
<a name="The_Root_Element"></a>   
## <a name="root-element"></a>ルート要素  
 <xref:System.Windows.Automation.AutomationElement> オブジェクトの検索には必ず、開始場所が必要です。 この開始場所には、デスクトップ、アプリケーション ウィンドウ、コントロールなど任意の要素を指定できます。  
  
 デスクトップのルート要素 (すべての要素が派生する) が静的 <xref:System.Windows.Automation.AutomationElement.RootElement%2A?displayProperty=nameWithType> プロパティから取得されます。  
  
> [!CAUTION]
> 通常、 <xref:System.Windows.Automation.AutomationElement.RootElement%2A>の直接の子のみの取得を試行する必要があります。 子孫の検索は、数百または数千もの要素を反復処理する場合があり、スタック オーバーフローを引き起こす可能性があります。 下位レベルの特定の要素を取得しようとする場合、アプリケーション ウィンドウから、または下位レベルのコンテナーから検索を開始する必要があります。  
  
<a name="Using_Conditions"></a>   
## <a name="conditions"></a>状態  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素を取得するために使用できるほとんどの手法では、 <xref:System.Windows.Automation.Condition>を指定する必要があります。これは、取得する要素を定義する一連の条件になります。  
  
 最も簡単な条件は <xref:System.Windows.Automation.Condition.TrueCondition>で、検索範囲内のすべての要素を指定する定義済みオブジェクトが返されます。 要素が検出されなくなるため、<xref:System.Windows.Automation.Condition.FalseCondition>( <xref:System.Windows.Automation.Condition.TrueCondition>の逆) はあまり有用ではありません。  
  
 その他の 3 つの定義済み条件 ( <xref:System.Windows.Automation.Automation.ContentViewCondition>、 <xref:System.Windows.Automation.Automation.ControlViewCondition>、および <xref:System.Windows.Automation.Automation.RawViewCondition>) は、単独で使用することも、その他の条件と組み合わせて使用することもできます。 <xref:System.Windows.Automation.Automation.RawViewCondition>を単独で使用した場合、 <xref:System.Windows.Automation.Condition.TrueCondition>または <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> プロパティで要素をフィルター処理しないため <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> と同等になります。  
  
 その他の条件は 1 つ以上の <xref:System.Windows.Automation.PropertyCondition> オブジェクトで構築され、それぞれはプロパティ値を指定します。 たとえば、 <xref:System.Windows.Automation.PropertyCondition> は要素が有効であることを指定したり、特定のコントロール パターンをサポートすることを指定したりします。  
  
 <xref:System.Windows.Automation.AndCondition>、 <xref:System.Windows.Automation.OrCondition>、および <xref:System.Windows.Automation.NotCondition>の型のオブジェクトを構築することによって、ブール ロジックを使用して条件を組み合わせることができます。  
  
<a name="Search_Scope"></a>   
## <a name="search-scope"></a>検索範囲  
 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> または <xref:System.Windows.Automation.AutomationElement.FindAll%2A> 使用した検索には、範囲と開始場所が必要です。  
  
 範囲は、検索対象の開始場所の周囲の領域を定義します。 これには、要素自体、その兄弟、その親、その先祖、その直接の子、およびその子孫を含めることができます。  
  
 <xref:System.Windows.Automation.TreeScope> 列挙の値のビットごとの組み合わせによって、検索の範囲が定義されます。  
  
<a name="Finding_a_Known_Element"></a>   
## <a name="finding-a-known-element"></a>既知の要素の検索  
 既知の要素 (その <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A>、 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AutomationId%2A>、または他のプロパティまたはプロパティの組み合わせによって識別される) を検索する方法として、 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> メソッドを使用する方法が最も簡単です。 検索する要素がアプリケーション ウィンドウである場合は、検索の開始点を <xref:System.Windows.Automation.AutomationElement.RootElement%2A>にすることができます。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素を検索するこの方法は、自動化されたシナリオ テストの最も有用な方法になります。  
  
<a name="Finding_Elements_in_a_Subtree"></a>   
## <a name="finding-elements-in-a-subtree"></a>サブツリー内の要素の検索  
 既知の要素に関連する特定の条件を満たすすべての要素を検索するには、 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>を使用します。 たとえば、このメソッドを使用して、リストまたはメニューからリスト項目またはメニュー項目を取得したり、ダイアログ ボックスのすべてのコントロールを識別したりできます。  
  
<a name="Walking_a_Subtree"></a>   
## <a name="walking-a-subtree"></a>サブツリーのウォーク  
 クライアントで使用されるアプリケーションについて予備知識がない場合、 <xref:System.Windows.Automation.TreeWalker> クラスを使用して、対象となるすべての要素のサブツリーを構築することができます。 アプリケーションは、フォーカス変更イベントへの応答でこれを実行することができます。つまり、アプリケーションやコントロールが入力フォーカスを受け取るときに、UI オートメーション クライアントはフォーカスされている要素の子およびすべての子孫を調べます。  
  
 <xref:System.Windows.Automation.TreeWalker> で使用できる別の方法は、要素の先祖を特定する方法です。 たとえば、ツリーをウォークすることによって、コントロールの親ウィンドウを特定できます。  
  
 クラスのオブジェクトを作成する ( <xref:System.Windows.Automation.TreeWalker> を渡すことによって対象の要素を定義する) か、 <xref:System.Windows.Automation.Condition>のフィールドとして定義されている次の定義済みオブジェクトのいずれかを使用することによって、 <xref:System.Windows.Automation.TreeWalker>を使用できます。  
  
|||  
|-|-|  
|<xref:System.Windows.Automation.TreeWalker.ContentViewWalker>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> プロパティが `true`である要素のみを検索します。|  
|<xref:System.Windows.Automation.TreeWalker.ControlViewWalker>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> プロパティが `true`である要素のみを検索します。|  
|<xref:System.Windows.Automation.TreeWalker.RawViewWalker>|すべての要素を検索します。|  
  
 <xref:System.Windows.Automation.TreeWalker>を取得した後、それを使用することは簡単です。 `Get` メソッドを呼び出し、サブツリーの要素間を移動するだけです。  
  
 <xref:System.Windows.Automation.TreeWalker.Normalize%2A> メソッドを使用すると、ビューの一部ではない要素からサブツリー内の他の要素に移動できます。 たとえば、 <xref:System.Windows.Automation.TreeWalker.ContentViewWalker>を使用してサブツリーのビューを作成したとします。 スクロール バーが入力フォーカスを受信したことを示す通知を、アプリケーションが受け取ります。 スクロール バーはコンテンツ要素ではないため、サブツリーのビューには存在しません。 しかし、スクロール バーを表す <xref:System.Windows.Automation.AutomationElement> を <xref:System.Windows.Automation.TreeWalker.Normalize%2A> に渡し、コンテンツ ビュー内にある最も近い先祖を取得することができます。  
  
<a name="Other_Ways_to_Retrieve_an_Element"></a>   
## <a name="other-ways-to-retrieve-an-element"></a>要素を取得する別の方法  
 検索とナビゲーションに加えて、次の方法で <xref:System.Windows.Automation.AutomationElement> を取得できます。  
  
### <a name="from-an-event"></a>イベントから  
 アプリケーションが [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントを受信するときに、イベント ハンドラーに渡されるソース オブジェクトは <xref:System.Windows.Automation.AutomationElement>です。 たとえば、フォーカス変更イベントをサブスクライブしている場合、 <xref:System.Windows.Automation.AutomationFocusChangedEventHandler> に渡されるソースはフォーカスを受け取った要素になります。  
  
 詳細については、「 [Subscribe to UI Automation Events](subscribe-to-ui-automation-events.md)」を参照してください。  
  
### <a name="from-a-point"></a>ポイントから  
 画面座標 (たとえば、カーソル位置) がある場合、静的 <xref:System.Windows.Automation.AutomationElement> メソッドを使用して <xref:System.Windows.Automation.AutomationElement.FromPoint%2A> を取得できます。  
  
### <a name="from-a-window-handle"></a>ウィンドウ ハンドルから  
 HWND から <xref:System.Windows.Automation.AutomationElement> を取得するには、静的 <xref:System.Windows.Automation.AutomationElement.FromHandle%2A> メソッドを使用します。  
  
### <a name="from-the-focused-control"></a>フォーカスされたコントロールから  
 静的 <xref:System.Windows.Automation.AutomationElement> プロパティから、フォーカスされたコントロールを表す <xref:System.Windows.Automation.AutomationElement.FocusedElement%2A> を取得できます。  
  
## <a name="see-also"></a>参照

- [プロパティ条件に基づく UI オートメーション要素の検索](find-a-ui-automation-element-based-on-a-property-condition.md)
- [TreeWalker を使用した UI オートメーション要素間の移動](navigate-among-ui-automation-elements-with-treewalker.md)
- [UI Automation ツリーの概要](ui-automation-tree-overview.md)
