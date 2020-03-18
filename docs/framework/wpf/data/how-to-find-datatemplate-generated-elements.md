---
title: '方法 : DataTemplate によって生成された要素を検索する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- finding DataTemplate elements [WPF]
- DataTemplate [WPF]
ms.assetid: bfcd564e-5e9e-451e-8641-a9b5c3cfac90
ms.openlocfilehash: 2cb3d73574cd207c0e06abe15f6a953a67cd5c78
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73733431"
---
# <a name="how-to-find-datatemplate-generated-elements"></a>方法 : DataTemplate によって生成された要素を検索する
この例では、<xref:System.Windows.DataTemplate>によって生成された要素を検索する方法を示します。  
  
## <a name="example"></a>例  
 この例では、いくつかの XML データにバインドされている <xref:System.Windows.Controls.ListBox> があります。  
  
 [!code-xaml[FindGeneratedItems#LB](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml#lb)]  
  
 <xref:System.Windows.Controls.ListBox> では、次の <xref:System.Windows.DataTemplate>を使用します。  
  
 [!code-xaml[FindGeneratedItems#DT](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml#dt)]  
  
 特定の <xref:System.Windows.Controls.ListBoxItem>の <xref:System.Windows.DataTemplate> によって生成された <xref:System.Windows.Controls.TextBlock> 要素を取得する場合は、<xref:System.Windows.Controls.ListBoxItem>を取得し、その <xref:System.Windows.Controls.ContentPresenter> 内の <xref:System.Windows.Controls.ListBoxItem>を見つけて、に設定されている <xref:System.Windows.FrameworkTemplate.FindName%2A> で <xref:System.Windows.DataTemplate> を呼び出す必要があります。<xref:System.Windows.Controls.ContentPresenter>ます。 次の例では、これらの手順を実行する方法を示します。 デモンストレーションを目的として、この例では、<xref:System.Windows.DataTemplate>生成されたテキストブロックのテキストの内容を示すメッセージボックスを作成します。  
  
 [!code-csharp[FindGeneratedItems#DTFindElement](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml.cs#dtfindelement)]
 [!code-vb[FindGeneratedItems#DTFindElement](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FindGeneratedItems/VisualBasic/Window1.xaml.vb#dtfindelement)]  
  
 次に示すのは、<xref:System.Windows.Media.VisualTreeHelper> メソッドを使用する `FindVisualChild`の実装です。  
  
 [!code-csharp[FindGeneratedItems#FVC](~/samples/snippets/csharp/VS_Snippets_Wpf/FindGeneratedItems/CSharp/Window1.xaml.cs#fvc)]
 [!code-vb[FindGeneratedItems#FVC](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FindGeneratedItems/VisualBasic/Window1.xaml.vb#fvc)]  
  
## <a name="see-also"></a>関連項目

- [方法: ControlTemplate によって生成された要素を検索する](../controls/how-to-find-controltemplate-generated-elements.md)
- [データ バインディングの概要](data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [WPF XAML 名前スコープ](../advanced/wpf-xaml-namescopes.md)
- [WPF のツリー](../advanced/trees-in-wpf.md)
