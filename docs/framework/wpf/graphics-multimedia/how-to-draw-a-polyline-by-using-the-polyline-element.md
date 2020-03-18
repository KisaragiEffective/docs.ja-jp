---
title: '方法 : ポリライン要素を使用してポリラインを描画する'
ms.date: 03/30/2017
helpviewer_keywords:
- connected lines [WPF]
- polylines [WPF], drawing
- graphics [WPF], polylines
- lines [WPF], connected (see polylines)
- drawing [WPF], polylines
ms.assetid: 65db8935-d047-4295-87c4-b427ff3ad293
ms.openlocfilehash: 6698141bcaa3b0fd5f5b9cf66bcab1be1f4ea2f0
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452962"
---
# <a name="how-to-draw-a-polyline-by-using-the-polyline-element"></a>方法 : ポリライン要素を使用してポリラインを描画する
この例では、<xref:System.Windows.Shapes.Polyline> 要素を使用して、接続された一連の線であるポリラインを描画する方法を示します。  
  
 ポリラインを描画するには、<xref:System.Windows.Shapes.Polyline> 要素を作成し、その <xref:System.Windows.Shapes.Polyline.Points%2A> プロパティを使用して図形の頂点を指定します。 最後に、<xref:System.Windows.Shapes.Shape.Stroke%2A> と <xref:System.Windows.Shapes.Shape.StrokeThickness%2A> のプロパティを使用して、ストロークの輪郭を記述します。これは、ストロークのない線が非表示になっているためです。  
  
> [!NOTE]
> <xref:System.Windows.Shapes.Polyline> 要素は閉じた図形ではないため、図形のアウトラインを意図的に閉じた場合でも、<xref:System.Windows.Shapes.Shape.Fill%2A> プロパティは効果がありません。 <xref:System.Windows.Shapes.Shape.Fill%2A>を持つ閉じた図形を作成するには、<xref:System.Windows.Shapes.Polygon> 要素を使用します。  
  
 次の例では、<xref:System.Windows.Controls.Canvas>内に2つの <xref:System.Windows.Shapes.Polyline> 要素を描画します。  
  
## <a name="example"></a>例  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、ポイントの有効な構文は、コンマで区切られた x 座標と y 座標のペアのスペース区切りのリストです。  
  
 [!code-xaml[drawingwithshapeelements#PolylineExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/polylineexample.xaml#polylineexample1)]  
  
 この例では、<xref:System.Windows.Controls.Canvas> を使用して、ポリラインを格納しますが、テキスト以外のコンテンツをサポートする <xref:System.Windows.Controls.Panel> または <xref:System.Windows.Controls.Control> では、ポリライン要素 (およびその他のすべての図形要素) を使用できます。  
  
 この例は、大規模なサンプルに含まれています。完全なサンプルについては、「 [Shape Elements sample](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Shapes.Polyline>
- <xref:System.Windows.Shapes.Polygon>
- <xref:System.Windows.Shapes.Shape>
- [図形要素のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)
- [WPF での図形と基本描画の概要](shapes-and-basic-drawing-in-wpf-overview.md)
