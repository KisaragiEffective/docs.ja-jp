---
title: UI オートメーションを使用したコントロールの呼び出し
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- invoking controls
- UI Automation, invoking controls
- controls, invoking
ms.assetid: 5ee2de3f-256c-43ec-b64c-62ace91f9983
ms.openlocfilehash: e1b489e8daaaf9f5b8c0cb46374fa54bf165d49c
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447003"
---
# <a name="invoke-a-control-using-ui-automation"></a>UI オートメーションを使用したコントロールの呼び出し
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、次のタスクを実行する方法について示します。  
  
- ターゲット アプリケーションの [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューを調べて、特定のプロパティ条件に一致するコントロールを検索する。  
  
- 各コントロールに対して <xref:System.Windows.Automation.AutomationElement> を作成する。  
  
- <xref:System.Windows.Automation.InvokePattern> コントロール パターンをサポートする [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素から <xref:System.Windows.Automation.InvokePattern> オブジェクトを取得する。  
  
- <xref:System.Windows.Automation.InvokePattern.Invoke%2A> を使用して、クライアントのイベント ハンドラーからコントロールを呼び出す。  
  
## <a name="example"></a>例  
 この例では、 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> クラスの <xref:System.Windows.Automation.AutomationElement> メソッドを使用して <xref:System.Windows.Automation.InvokePattern> オブジェクトを生成し、 <xref:System.Windows.Automation.InvokePattern.Invoke%2A> メソッドを使用してコントロールを呼び出します。  
  
 [!code-csharp[InvokePatternApp#1100](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1100)]
 [!code-vb[InvokePatternApp#1100](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1100)]  
[!code-csharp[InvokePatternApp#1102](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1102)]
[!code-vb[InvokePatternApp#1102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1102)]  
  
## <a name="see-also"></a>参照

- [InvokePattern、ExpandCollapsePattern、および TogglePattern サンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
