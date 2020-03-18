---
title: '方法 : 要素の Width プロパティを設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- width properties [WPF]
- Panel control [WPF], width properties of elements
ms.assetid: 6ee04a9d-63f0-4f5b-a406-0a8cd4c35729
ms.openlocfilehash: 682929625612114d042e4619d8388617988b3c48
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124274"
---
# <a name="how-to-set-the-width-properties-of-an-element"></a>方法 : 要素の Width プロパティを設定する
## <a name="example"></a>例  
 この例では、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]の4つの幅に関連するプロパティのレンダリング動作の違いを視覚的に示します。  
  
 <xref:System.Windows.FrameworkElement> クラスは、要素の幅特性を記述する4つのプロパティを公開します。 これらの4つのプロパティは競合する可能性があります。その場合、優先される値は次のように決定されます。 <xref:System.Windows.FrameworkElement.MinWidth%2A> 値は、<xref:System.Windows.FrameworkElement.MaxWidth%2A> 値よりも優先されるため、<xref:System.Windows.FrameworkElement.Width%2A> 値よりも優先されます。 4番目のプロパティである <xref:System.Windows.FrameworkElement.ActualWidth%2A>は読み取り専用であり、レイアウトプロセスとの対話によって決定された実際の幅を報告します。  
  
 次の [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] の例では、<xref:System.Windows.Controls.Canvas>の子として <xref:System.Windows.Shapes.Rectangle> 要素 (`rect1`) を描画します。 <xref:System.Windows.FrameworkElement.MinWidth%2A>、<xref:System.Windows.FrameworkElement.MaxWidth%2A>、および <xref:System.Windows.FrameworkElement.Width%2A>のプロパティ値を表す一連の <xref:System.Windows.Controls.ListBox> 要素を使用して、<xref:System.Windows.Shapes.Rectangle> の幅のプロパティを変更できます。 この方法では、各プロパティの優先順位が視覚的に表示されます。  
  
 [!code-xaml[WidthMinWidthMaxWidth#1](~/samples/snippets/csharp/VS_Snippets_Wpf/WidthMinWidthMaxWidth/CSharp/Window1.xaml#1)]  
[!code-xaml[WidthMinWidthMaxWidth#2](~/samples/snippets/csharp/VS_Snippets_Wpf/WidthMinWidthMaxWidth/CSharp/Window1.xaml#2)]  
  
 次のコードビハインドの例では、<xref:System.Windows.Controls.Primitives.Selector.SelectionChanged> イベントによって発生するイベントを処理します。 各カスタムメソッドは、<xref:System.Windows.Controls.ListBox>からの入力を受け取り、その値を <xref:System.Double>として解析し、指定された幅に関連するプロパティに値を適用します。 幅の値も文字列に変換され、さまざまな <xref:System.Windows.Controls.TextBlock> 要素に書き込まれます (これらの要素の定義は、選択された XAML には表示されません)。  
  
 [!code-csharp[WidthMinWidthMaxWidth#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WidthMinWidthMaxWidth/CSharp/Window1.xaml.cs#3)]
 [!code-vb[WidthMinWidthMaxWidth#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WidthMinWidthMaxWidth/VisualBasic/Window1.xaml.vb#3)]  
  
 完全なサンプルについては、「 [Width Properties Comparison sample](https://github.com/Microsoft/WPF-Samples/tree/master/Elements/WidthProperties)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Controls.ListBox>
- <xref:System.Windows.FrameworkElement>
- <xref:System.Windows.FrameworkElement.ActualWidth%2A>
- <xref:System.Windows.FrameworkElement.MaxWidth%2A>
- <xref:System.Windows.FrameworkElement.MinWidth%2A>
- <xref:System.Windows.FrameworkElement.Width%2A>
- [パネル概要](panels-overview.md)
- [要素の Height プロパティを設定する](how-to-set-the-height-properties-of-an-element.md)
- [Width プロパティの比較のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Elements/WidthProperties)
