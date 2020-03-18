---
title: UI オートメーション要素のプロパティの取得
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties, retrieving
- UI Automation, retrieving properties of elements
ms.assetid: 09576b1a-291f-435c-980e-dee32d899ae1
ms.openlocfilehash: 1b82302ff89d2e8407871cbfba6c10e8322ac4e7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74435498"
---
# <a name="get-ui-automation-element-properties"></a>UI オートメーション要素のプロパティの取得
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 要素のプロパティを取得する方法について説明します。  
  
### <a name="get-a-current-property-value"></a>現在のプロパティ値を取得します。  
  
1. 取得するプロパティを持つ <xref:System.Windows.Automation.AutomationElement> を取得します。  
  
2. <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A>を呼び出すか、<xref:System.Windows.Automation.AutomationElement.Current%2A> のプロパティ構造体を取得し、そのメンバーの1つから値を取得します。  
  
### <a name="get-a-cached-property-value"></a>キャッシュされたプロパティ値を取得する  
  
1. 取得するプロパティを持つ <xref:System.Windows.Automation.AutomationElement> を取得します。 プロパティは、<xref:System.Windows.Automation.CacheRequest>で指定されている必要があります。  
  
2. <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>を呼び出すか、<xref:System.Windows.Automation.AutomationElement.Cached%2A> のプロパティ構造体を取得し、そのメンバーの1つから値を取得します。  
  
## <a name="example"></a>例  
 次の例は、<xref:System.Windows.Automation.AutomationElement>の現在のプロパティを取得するさまざまな方法を示しています。  
  
 [!code-csharp[UIAClient_snip#170](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#170)]
 [!code-vb[UIAClient_snip#170](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#170)]  
  
## <a name="see-also"></a>参照

- [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)
- [UI オートメーションにおけるキャッシュの使用](use-caching-in-ui-automation.md)
- [Caching in UI Automation Clients](caching-in-ui-automation-clients.md)
