---
title: クライアントの UI オートメーション プロパティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties, UI Automation clients
- UI Automation, client properties
ms.assetid: 255905af-0b17-485c-93d4-8a2db2a6524b
ms.openlocfilehash: 3ef1e7c6e21f30c5bdea096003f192c38059ab2e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441365"
---
# <a name="ui-automation-properties-for-clients"></a>クライアントの UI オートメーション プロパティ
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 ここでは、UI オートメーション クライアント アプリケーションに公開される [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティについて説明します。  
  
 <xref:System.Windows.Automation.AutomationElement> オブジェクトのプロパティには、 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 要素 (通常はコントロール) に関する情報が含まれます。 <xref:System.Windows.Automation.AutomationElement> のプロパティは汎用的なもので、コントロール型に固有ではありません。 これらのプロパティの多くは、 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation> 構造体で公開されます。  
  
 また、コントロール パターンもプロパティを持ちます。 コントロール パターンのプロパティは、パターンに固有です。 たとえば、 <xref:System.Windows.Automation.ScrollPattern> に含まれるプロパティを使用すると、ウィンドウを垂直方向または水平方向のどちらにスクロールできるのかや、現在のビュー サイズおよびスクロール位置をクライアント アプリケーションで検出できます。 コントロール パターンは、そのすべてのプロパティを構造体 ( <xref:System.Windows.Automation.ScrollPattern.ScrollPatternInformation>など) を介して公開します。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティは読み取り専用です。 コントロールのプロパティを設定するには、適切なコントロール パターンのメソッドを使用する必要があります。 たとえば、スクロール ウィンドウの位置の値を変更する場合は、 <xref:System.Windows.Automation.ScrollPattern.Scroll%2A> を使用します。  
  
 パフォーマンスを向上させるために、 <xref:System.Windows.Automation.AutomationElement> オブジェクトを取得したときに、コントロールおよびコントロール パターンのプロパティ値をキャッシュできます。 詳細については、「 [UI オートメーションクライアントでのキャッシュ](caching-in-ui-automation-clients.md)」を参照してください。  
  
## <a name="property-ids"></a>プロパティ ID  
 プロパティ識別子 (IDs) は、<xref:System.Windows.Automation.AutomationProperty> オブジェクトにカプセル化される一意の定数値です。 UI オートメーションクライアントアプリケーションは、これらの Id を <xref:System.Windows.Automation.AutomationElement> クラスまたは適切なコントロールパターンクラス (<xref:System.Windows.Automation.ScrollPattern>など) から取得します。 UI オートメーション プロバイダーは、 <xref:System.Windows.Automation.AutomationElementIdentifiers> またはコントロール パターン識別子クラスの 1 つ ( <xref:System.Windows.Automation.ScrollPatternIdentifiers>など) からこれらを取得します。  
  
 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> の数値 <xref:System.Windows.Automation.AutomationProperty> は、 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPropertyValue%2A?displayProperty=nameWithType> メソッドで照会するプロパティを識別するために、プロバイダーによって使用されます。 通常、クライアント アプリケーションで <xref:System.Windows.Automation.AutomationIdentifier.Id%2A>を調べる必要はありません。 <xref:System.Windows.Automation.AutomationIdentifier.ProgrammaticName%2A> は、デバッグと診断の目的のみに使用されます。  
  
## <a name="property-conditions"></a>プロパティ条件  
 プロパティ Id は、<xref:System.Windows.Automation.AutomationElement> オブジェクトを検索するために使用される <xref:System.Windows.Automation.PropertyCondition> オブジェクトの構築に使用されます。 たとえば、特定の名前を持つ <xref:System.Windows.Automation.AutomationElement> を検出したい場合や、すべての有効なコントロールを検出したい場合があります。 各 <xref:System.Windows.Automation.PropertyCondition> では、 <xref:System.Windows.Automation.AutomationProperty> 識別子と、そのプロパティが一致する必要がある値を指定します。  
  
 詳細については、次のリファレンス トピックを参照してください。  
  
- <xref:System.Windows.Automation.AutomationElement.FindFirst%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.FindAll%2A>  
  
- <xref:System.Windows.Automation.TreeWalker.Condition%2A>  
  
## <a name="retrieving-properties"></a>プロパティの取得  
 <xref:System.Windows.Automation.AutomationElement> のいくつかのプロパティと、コントロール パターン クラスのすべてのプロパティは、 `Current` またはコントロール パターン オブジェクトの、 `Cached` プロパティまたは <xref:System.Windows.Automation.AutomationElement> プロパティの入れ子になったプロパティとして公開されます。  
  
 また、 <xref:System.Windows.Automation.AutomationElement> または <xref:System.Windows.Automation.AutomationElement.Cached%2A> の構造体にはないプロパティを含む、任意の <xref:System.Windows.Automation.AutomationElement.Current%2A> またはコントロール パターン プロパティは、次のいずれかのメソッドを使用して取得できます。  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A>  
  
 これらのメソッドを使用すると、パフォーマンスがわずかながら向上すると共に、すべてのプロパティにアクセスできます。  
  
 <xref:System.Windows.Automation.AutomationElement>のプロパティを取得する 2 とおりの方法を次のコード例で示します。  
  
 [!code-csharp[UIAClient_snip#121](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#121)]
 [!code-vb[UIAClient_snip#121](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#121)]  
  
 <xref:System.Windows.Automation.AutomationElement>でサポートされるコントロール パターンのプロパティを取得する場合は、コントロール パターン オブジェクトを取得する必要はありません。 単にパターン プロパティ識別子の 1 つをメソッドに渡します。  
  
 コントロール パターンのプロパティを取得する 2 とおりの方法を次のコード例で示します。  
  
 [!code-csharp[UIAClient_snip#122](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#122)]
 [!code-vb[UIAClient_snip#122](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#122)]  
  
 `Get` メソッドは <xref:System.Object>を返します。 アプリケーションは、この値を使用する前に、返されたオブジェクトを適切な型にキャストする必要があります。  
  
## <a name="default-property-values"></a>既定のプロパティ値  
 UI オートメーション プロバイダーがプロパティを実装していない場合、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] システムは既定値を提供できます。 たとえば、コントロールのプロバイダーが <xref:System.Windows.Automation.AutomationElement.HelpTextProperty>によって識別されたプロパティをサポートしていない場合、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は空の文字列を返します。 同様に、プロバイダーが <xref:System.Windows.Automation.AutomationElement.IsDockPatternAvailableProperty>によって識別されたプロパティをサポートしていない場合、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は `false`を返します。  
  
 この動作は、 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> と <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> のメソッド オーバーロードを使用して変更できます。 2 番目のパラメーターとして `true` を指定した場合、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] は既定値を返さず、代わりに特殊な値 <xref:System.Windows.Automation.AutomationElement.NotSupported>を返します。  
  
 次のコード例は、要素からプロパティを取得することを試みています。プロパティがサポートされていない場合は、代わりにアプリケーション定義の値が使用されます。  
  
 [!code-csharp[UIAClient_snip#123](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#123)]
 [!code-vb[UIAClient_snip#123](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#123)]  
  
 要素によってサポートされているプロパティを調べるためには、 <xref:System.Windows.Automation.AutomationElement.GetSupportedProperties%2A>を使用します。 これにより、 <xref:System.Windows.Automation.AutomationProperty> 識別子の配列が返されます。  
  
## <a name="property-changed-events"></a>プロパティ変更イベント  
 <xref:System.Windows.Automation.AutomationElement> またはコントロール パターンのプロパティ値が変化すると、イベントが発生します。 アプリケーションは、 <xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>を呼び出し、目的のプロパティを指定するために最後のパラメーターとして <xref:System.Windows.Automation.AutomationProperty> 識別子の配列を提供して、このようなイベントをサブスクライブできます。  
  
 <xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>では、イベント引数の <xref:System.Windows.Automation.AutomationPropertyChangedEventArgs.Property%2A> メンバーを調べることによって、変更されたプロパティを識別できます。 また、引数には、変更された [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティの古い値と新しい値も含まれています。 これらの値は <xref:System.Object> 型であり、使用する前に正しい型にキャストする必要があります。  
  
## <a name="additional-automationelement-properties"></a>その他の AutomationElement プロパティ  
 <xref:System.Windows.Automation.AutomationElement.Current%2A> プロパティおよび <xref:System.Windows.Automation.AutomationElement.Cached%2A> プロパティの構造体に加えて、 <xref:System.Windows.Automation.AutomationElement> には、単純なプロパティ アクセサーを介して取得される次のプロパティがあります。  
  
|プロパティ|説明|  
|--------------|-----------------|  
|<xref:System.Windows.Automation.AutomationElement.CachedChildren%2A>|キャッシュ内にある子 <xref:System.Windows.Automation.AutomationElement> オブジェクトのコレクション。|  
|<xref:System.Windows.Automation.AutomationElement.CachedParent%2A>|キャッシュ内にある <xref:System.Windows.Automation.AutomationElement> 親オブジェクト。|  
|<xref:System.Windows.Automation.AutomationElement.FocusedElement%2A>|(静的なプロパティ) 入力フォーカスがある <xref:System.Windows.Automation.AutomationElement> 。|  
|<xref:System.Windows.Automation.AutomationElement.RootElement%2A>|(静的プロパティ) ルート <xref:System.Windows.Automation.AutomationElement>。|  
  
## <a name="see-also"></a>参照

- [Caching in UI Automation Clients](caching-in-ui-automation-clients.md)
- [サーバー側 UI オートメーション プロバイダーの実装](server-side-ui-automation-provider-implementation.md)
- [UI オートメーション イベントのサブスクライブ](subscribe-to-ui-automation-events.md)
