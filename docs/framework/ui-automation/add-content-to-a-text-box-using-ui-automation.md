---
title: UI オートメーションを使用した、テキスト ボックスへのコンテンツの追加
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- adding content to text boxes
- text boxes, adding content
- UI Automation, adding content to text boxes
ms.assetid: 8bdd1a73-1ecb-4a05-a891-a7827ebb767f
ms.openlocfilehash: 71385690c01d261b4ff1b495674bab377232e81d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447247"
---
# <a name="add-content-to-a-text-box-using-ui-automation"></a>UI オートメーションを使用した、テキスト ボックスへのコンテンツの追加
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を使用して1行のテキストボックスにテキストを挿入する方法を示すコード例について説明します。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] が適用されない複数行およびリッチテキストコントロールには、代替の方法が用意されています。 比較のために、この例では、Win32 メソッドを使用して同じ結果を実現する方法も示しています。  
  
## <a name="example"></a>例  
 次の例では、対象アプリケーションのテキストコントロールのシーケンスをステップ実行します。 各テキストコントロールは、<xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> メソッドを使用して <xref:System.Windows.Automation.ValuePattern> オブジェクトを取得できるかどうかをテストします。 テキストコントロールで <xref:System.Windows.Automation.ValuePattern>がサポートされている場合は、<xref:System.Windows.Automation.ValuePattern.SetValue%2A> メソッドを使用して、ユーザー定義の文字列をテキストコントロールに挿入します。 それ以外の場合は、<xref:System.Windows.Forms.SendKeys.SendWait%2A?displayProperty=nameWithType> メソッドが使用されます。  
  
 [!code-csharp[InsertText#InsertText](../../../samples/snippets/csharp/VS_Snippets_Wpf/InsertText/CSharp/Window1.xaml.cs#inserttext)]
 [!code-vb[InsertText#InsertText](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InsertText/VisualBasic/Window1.xaml.vb#inserttext)]  
  
## <a name="see-also"></a>参照

- [TextPattern の挿入テキストのサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771478(v=vs.90))
