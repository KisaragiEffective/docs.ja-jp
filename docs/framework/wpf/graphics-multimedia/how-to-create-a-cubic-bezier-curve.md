---
title: '方法 : 3 次ベジエ曲線を作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- curves [WPF], cubic Bezier
- Bezier curves [WPF], cubic
- graphics [WPF], cubic Bezier curves
- cubic Bezier curves [WPF]
ms.assetid: 450a3a77-7c57-48b0-a008-0f6051add980
ms.openlocfilehash: c12bd84fcebb3acebb80bef5f4479ad535fd6691
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452111"
---
# <a name="how-to-create-a-cubic-bezier-curve"></a>方法 : 3 次ベジエ曲線を作成する
この例では、3次ベジエ曲線を作成する方法を示します。 3次ベジエ曲線を作成するには、<xref:System.Windows.Media.PathGeometry>、<xref:System.Windows.Media.PathFigure>、および <xref:System.Windows.Media.BezierSegment> クラスを使用します。  結果として得られるジオメトリを表示するには、<xref:System.Windows.Shapes.Path> 要素を使用するか、<xref:System.Windows.Media.GeometryDrawing> または <xref:System.Windows.Media.DrawingContext>と共に使用します。 次の例では、3次ベジエ曲線が (10, 100) から (300, 100) に描画されます。 曲線には、(100, 0) と (200, 200) の制御点があります。  
  
## <a name="example"></a>例  
 [xaml]  
  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、省略したマークアップ構文を使用してパスを記述できます。  
  
 [!code-xaml[GeometrySample#53](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/geometryattributesyntaxexample.xaml#53)]  
  
 [xaml]  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、オブジェクトタグを使用して3次ベジエ曲線を描画することもできます。 次の例は、前の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の例と同じです。  
  
 [!code-xaml[GeometrySample#33](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/pathgeometryexample.xaml#33)]  
  
 この例は、より大きなサンプルの一部です。完全なサンプルについては、「[ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)」を参照してください。  
  
## <a name="see-also"></a>参照

- [楕円の円弧を作成する](how-to-create-an-elliptical-arc.md)
- [PathGeometry で LineSegment を作成する](how-to-create-a-linesegment-in-a-pathgeometry.md)
- [3 次ベジエ曲線を作成する](how-to-create-a-cubic-bezier-curve.md)
- [2 次ベジエ曲線を作成する](how-to-create-a-quadratic-bezier-curve.md)
