---
title: UI オートメーションを使用して混合テキスト属性の詳細を取得する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d0e4c005-abd1-42bb-92a4-5faf87097311
ms.openlocfilehash: 9f2cba1f602cedf3a13bd909b4dc2f1a7b4ab972
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443121"
---
# <a name="obtain-mixed-text-attribute-details-using-ui-automation"></a>UI オートメーションを使用して混合テキスト属性の詳細を取得する
> [!NOTE]
> このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージド <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]の最新情報については、「 [Windows Automation API: UI オートメーション](/windows/win32/winauto/entry-uiauto-win32)」を参照してください。  
  
 このトピックでは、 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] を使用して、複数の属性値にまたがるテキスト範囲からテキスト属性の詳細を取得する方法を示します。 テキスト範囲は、ドキュメント内でのキャレット (^) の現在位置 (つまり低次元の選択範囲)、連続したテキストを選択したもの、個別に選択したテキストの集合、またはドキュメントに含まれるテキスト全体のいずれかに対応します。  
  
## <a name="example"></a>例  
 次のコード例は、 <xref:System.Windows.Automation.TextPattern.FontNameAttribute> が <xref:System.Windows.Automation.Text.TextPatternRange.GetAttributeValue%2A> オブジェクトを返すテキスト範囲から <xref:System.Windows.Automation.TextPattern.MixedAttributeValue> を取得する方法を示しています。  
  
[!code-csharp[FindText#RetrieveMixedAttributes](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#retrievemixedattributes)]
[!code-vb[FindText#RetrieveMixedAttributes](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#retrievemixedattributes)]  
  
 <xref:System.Windows.Automation.TextPattern> コントロール パターンは、 <xref:System.Windows.Automation.Text.TextPatternRange> クラスと共に、基本的なテキスト属性、プロパティ、およびメソッドをサポートします。 <xref:System.Windows.Automation.TextPattern> または <xref:System.Windows.Automation.Text.TextPatternRange>によってサポートされないコントロール固有の機能の場合、UI オートメーション クライアントが対応するネイティブ オブジェクト モデルにアクセスできるように、 <xref:System.Windows.Automation.AutomationElement> クラスがメソッドを提供します。  
  
## <a name="see-also"></a>参照

- [UI オートメーション TextPattern の概要](ui-automation-textpattern-overview.md)
- [UI オートメーションを使用した、テキスト ボックスへのコンテンツの追加](add-content-to-a-text-box-using-ui-automation.md)
- [UI オートメーションを使用した、テキストの検索と強調表示](find-and-highlight-text-using-ui-automation.md)
- [UI Automation コントロール パターンの概要](ui-automation-control-patterns-overview.md)
- [UI Automation Control Patterns for Clients](ui-automation-control-patterns-for-clients.md)
- [UI オートメーションを使用してテキスト属性を取得する](obtain-text-attributes-using-ui-automation.md)
