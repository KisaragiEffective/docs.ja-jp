---
title: UI オートメーションを使用した、チェック ボックスのトグル状態の取得
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, getting toggle states of check boxes
- check boxes, getting toggle states of
- getting, toggle states of check boxes
ms.assetid: 84fc31a3-175f-4e93-90a0-dd29d89b77ce
ms.openlocfilehash: d4a3fa01dcfee2eafbb6dbd46cd67bed9e874a73
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74435588"
---
# <a name="get-the-toggle-state-of-a-check-box-using-ui-automation"></a>UI オートメーションを使用した、チェック ボックスのトグル状態の取得
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を使用して、コントロールのトグル状態を取得する方法について説明します。  
  
## <a name="example"></a>例  
 この例では、<xref:System.Windows.Automation.AutomationElement> クラスの <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> メソッドを使用して、コントロールから <xref:System.Windows.Automation.TogglePattern> オブジェクトを取得し、その <xref:System.Windows.Automation.ToggleState> プロパティを返します。  
  
 [!code-csharp[NavigatingWithTreeWalker#1200](../../../samples/snippets/csharp/VS_Snippets_Wpf/NavigatingWithTreeWalker/CSharp/ClientClass.cs#1200)]
 [!code-vb[NavigatingWithTreeWalker#1200](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/NavigatingWithTreeWalker/visualbasic/clientclass.vb#1200)]
