---
title: '方法 : 多角形要素を使用して、閉じた図形を描画する'
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [WPF], Polygon elements
- closed shapes [WPF], drawing with Polygon elements
- Polygon elements [WPF]
- drawing [WPF], closed shapes with Polygon elements
ms.assetid: 4b0ca008-29ce-48dd-8bc3-f3a20ffca6a6
ms.openlocfilehash: 324a5623ee658789b8600a43a89ce26cab7cd6cd
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452975"
---
# <a name="how-to-draw-a-closed-shape-by-using-the-polygon-element"></a>方法 : 多角形要素を使用して、閉じた図形を描画する
この例では、<xref:System.Windows.Shapes.Polygon> 要素を使用して、閉じた図形を描画する方法を示します。 閉じた図形を描画するには、<xref:System.Windows.Shapes.Polygon> 要素を作成し、その <xref:System.Windows.Shapes.Polygon.Points%2A> プロパティを使用して図形の頂点を指定します。 最初と最後の点をつなぐ線が自動的に描画されます。 最後に、<xref:System.Windows.Shapes.Shape.Fill%2A>、<xref:System.Windows.Shapes.Shape.Stroke%2A>、またはその両方を指定します。  
  
## <a name="example"></a>例  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、ポイントの有効な構文は、コンマで区切られた x 座標と y 座標のペアのスペース区切りのリストです。  
  
 [!code-xaml[drawingwithshapeelements#PolygonExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/polygonexample.xaml#polygonexample1)]  
  
 この例では、<xref:System.Windows.Controls.Canvas> を使用して多角形を格納していますが、テキスト以外のコンテンツをサポートする <xref:System.Windows.Controls.Panel> または <xref:System.Windows.Controls.Control> で polygon 要素 (およびその他のすべての図形要素) を使用できます。  
  
 この例は、大規模なサンプルに含まれています。完全なサンプルについては、「 [Shape Elements sample](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)」を参照してください。
