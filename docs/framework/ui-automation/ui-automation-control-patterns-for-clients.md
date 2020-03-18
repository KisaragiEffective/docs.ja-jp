---
title: クライアントの UI オートメーション コントロール パターン
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, control patterns for clients
- control patterns, UI Automation clients
ms.assetid: 571561d8-5f49-43a9-a054-87735194e013
ms.openlocfilehash: 193049aed6da3375b687e465678dca4dc90e6b39
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448814"
---
# <a name="ui-automation-control-patterns-for-clients"></a>クライアントの UI オートメーション コントロール パターン
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 この概要では、UI オートメーション クライアントのコントロール パターンについて説明します。 また、UI オートメーション クライアントがコントロール パターンを使用して、 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)]の情報にアクセスするしくみについても説明します。  
  
 コントロール パターンは、コントロール型や外観に関係なく、コントロールの機能を分類したり公開したりするための手段です。 UI オートメーション クライアントは、 <xref:System.Windows.Automation.AutomationElement> を調べてどのコントロール パターンがサポートされているかを特定し、そのコントロールの動作を確実に実行することができます。  
  
 コントロール パターンの完全なリストについては、「 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)」をご覧ください。  
  
<a name="uiautomation_getting_control_patterns"></a>   
## <a name="getting-control-patterns"></a>コントロール パターンの取得  
 クライアントは、 <xref:System.Windows.Automation.AutomationElement> または <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A?displayProperty=nameWithType> を呼び出して、 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A?displayProperty=nameWithType>からコントロール パターンを取得します。  
  
 クライアントは、 <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> メソッドまたは個別の `IsPatternAvailable` プロパティ ( <xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty>など) を使用して、パターンまたはパターンのグループが <xref:System.Windows.Automation.AutomationElement>でサポートされているかどうかを特定できます。 ただし、サポートされているプロパティを確認してコントロール パターンを取得するよりも、コントロール パターンを取得して `null` 参照に対するテストを試行する方が、プロセス間呼び出しが少なくなるため効率的です。  
  
 <xref:System.Windows.Automation.TextPattern> から <xref:System.Windows.Automation.AutomationElement>コントロール パターンを取得する方法を次の例に示します。  
  
 [!code-csharp[UIATextPattern_snip#1037](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#1037)]  
  
<a name="uiautomation_properties_on_control_patterns"></a>   
## <a name="retrieving-properties-on-control-patterns"></a>コントロール パターンのプロパティの取得  
 クライアントは、 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> または <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> を呼び出し、返されたオブジェクトを適切な型にキャストすることで、コントロール パターンのプロパティ値を取得できます。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のプロパティの詳細については、「[クライアントの UI オートメーションのプロパティ](ui-automation-properties-for-clients.md)」を参照してください。  
  
 `GetPropertyValue` メソッドに加えて、共通言語ランタイム (CLR) アクセサーを使用してプロパティ値を取得し、パターンの [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティにアクセスすることもできます。  
  
<a name="uiautomation_with_variable_patterns"></a>   
## <a name="controls-with-variable-patterns"></a>変数パターンを持つコントロール  
 一部のコントロール型は、状態やコントロールの使用方法に応じた複数のパターンをサポートしています。 変数パターンを持つコントロールの例としては、リストビュー (サムネイル、タイル、アイコン、リスト、詳細)、Microsoft Excel グラフ (円、線、横棒、数式を使用したセル値)、Microsoft Word のドキュメント領域 (標準、Web レイアウト、アウトライン、印刷レイアウト、印刷など) があります。プレビュー)、および Microsoft Windows Media Player スキン。  
  
 カスタム コントロール型を実装するコントロールは、機能を表すために必要なコントロール パターンの任意のセットを持つことができます。  
  
## <a name="see-also"></a>参照

- [UI オートメーション コントロール パターン](ui-automation-control-patterns.md)
- [UI オートメーション テキスト パターン](ui-automation-text-pattern.md)
- [Invoke a Control Using UI Automation](invoke-a-control-using-ui-automation.md)
- [Get the Toggle State of a Check Box Using UI Automation](get-the-toggle-state-of-a-check-box-using-ui-automation.md)
- [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)
- [TextPattern の挿入テキストのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText)
- [TextPattern の検索と選択のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FindText)
- [InvokePattern、ExpandCollapsePattern、および TogglePattern サンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
