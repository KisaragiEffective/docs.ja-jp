---
title: '方法 : 線を描画する'
ms.date: 03/30/2017
helpviewer_keywords:
- drawing [WPF], lines
- graphics [WPF], lines
- lines [WPF], drawing
ms.assetid: 0513ee01-6b27-4bb3-85f3-3a3e6710d80e
ms.openlocfilehash: a803c1be01086ca8911ef4cc33bd67697239e2c0
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452949"
---
# <a name="how-to-draw-a-line"></a>方法 : 線を描画する
この例では、<xref:System.Windows.Shapes.Line> 要素を使用して線を描画する方法を示します。  
  
 線を描画するには、<xref:System.Windows.Shapes.Line> 要素を作成します。 その <xref:System.Windows.Shapes.Line.X1%2A> と <xref:System.Windows.Shapes.Line.Y1%2A> プロパティを使用して、開始点を設定します。とは、その <xref:System.Windows.Shapes.Line.X2%2A> と <xref:System.Windows.Shapes.Line.Y2%2A> プロパティを使用してエンドポイントを設定します。 最後に、ストロークのない線が非表示になっているため、<xref:System.Windows.Shapes.Shape.Stroke%2A> と <xref:System.Windows.Shapes.Shape.StrokeThickness%2A> を設定します。  
  
 線に内部がないため、行の <xref:System.Windows.Shapes.Shape.Fill%2A> 要素を設定しても効果はありません。  
  
 次の例では、<xref:System.Windows.Controls.Canvas> 要素内に3つの直線を描画します。  
  
## <a name="example"></a>例  
 [!code-xaml[drawingwithshapeelements#LineExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/lineexample.xaml#lineexample1)]  
  
 この例は、大規模なサンプルに含まれています。完全なサンプルについては、「 [Shape Elements sample](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Shapes.Line>
- [図形要素のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)
