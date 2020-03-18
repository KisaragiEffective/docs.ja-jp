---
title: '方法 : ControlTemplate によって生成された要素を検索する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ControlTemplates [WPF], finding elements
- finding ControlTemplate elements [WPF]
ms.assetid: d7b25447-ceff-4bb4-9be5-fd7c40ef00af
ms.openlocfilehash: 232ee7d2859059591c9beff753f45781598a8127
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460703"
---
# <a name="how-to-find-controltemplate-generated-elements"></a>方法 : ControlTemplate によって生成された要素を検索する
この例では、<xref:System.Windows.Controls.ControlTemplate>によって生成された要素を検索する方法を示します。  
  
## <a name="example"></a>例  
 次の例は、<xref:System.Windows.Controls.Button> クラスの単純な <xref:System.Windows.Controls.ControlTemplate> を作成するスタイルを示しています。  
  
 [!code-xaml[FindGeneratedItems#CT](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml#ct)]  
  
 テンプレートが適用された後にテンプレート内の要素を検索するには、<xref:System.Windows.Controls.Control.Template%2A>の <xref:System.Windows.FrameworkTemplate.FindName%2A> メソッドを呼び出すことができます。 次の例では、コントロールテンプレート内の <xref:System.Windows.Controls.Grid> の実際の幅の値を示すメッセージボックスを作成します。  
  
 [!code-csharp[FindGeneratedItems#CTFindElement](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml.cs#ctfindelement)]
 [!code-vb[FindGeneratedItems#CTFindElement](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FindGeneratedItems/VisualBasic/Window1.xaml.vb#ctfindelement)]  
  
## <a name="see-also"></a>関連項目

- [DataTemplate によって生成された要素を検索する](../data/how-to-find-datatemplate-generated-elements.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [WPF XAML 名前スコープ](../advanced/wpf-xaml-namescopes.md)
- [WPF のツリー](../advanced/trees-in-wpf.md)
