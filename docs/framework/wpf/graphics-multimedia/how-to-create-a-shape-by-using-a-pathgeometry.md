---
title: '方法 : PathGeometry を使用して図形を作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- shapes [WPF], creating with PathGeometry class
- graphics [WPF], shapes
ms.assetid: 49a4a8b7-e738-45be-8dac-b54a6d8f5b21
ms.openlocfilehash: bdf3c937bfff1780a72e8743bc86a3c3dad2558d
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452046"
---
# <a name="how-to-create-a-shape-by-using-a-pathgeometry"></a>方法 : PathGeometry を使用して図形を作成する
この例では、<xref:System.Windows.Media.PathGeometry> クラスを使用して図形を作成する方法を示します。 <xref:System.Windows.Media.PathGeometry> オブジェクトは、1つまたは複数の <xref:System.Windows.Media.PathFigure> オブジェクトで構成されます。各 <xref:System.Windows.Media.PathFigure> は、別の "図" または図形を表します。 各 <xref:System.Windows.Media.PathFigure> は、それぞれが図形または図形の接続された部分を表す1つ以上の <xref:System.Windows.Media.PathSegment> オブジェクトで構成されています。 セグメントの種類には、<xref:System.Windows.Media.LineSegment>、<xref:System.Windows.Media.ArcSegment>、および <xref:System.Windows.Media.BezierSegment>があります。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Media.PathGeometry> を使用して三角形を作成します。 <xref:System.Windows.Media.PathGeometry> は、<xref:System.Windows.Shapes.Path> 要素を使用して表示されます。  
  
 [!code-xaml[GeometrySample#49](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/pathgeometryexample.xaml#49)]  
  
 前の例で作成した図を次に示します。  
  
 ![PathGeometry](./media/wcpsdk-graphicsmm-pathgeometry-triangle.gif "wcpsdk_graphicsmm_pathgeometry_triangle")  
PathGeometry で作成された三角形  
  
 前の例では、比較的単純なシェイプである三角形を作成する方法を示しました。 <xref:System.Windows.Media.PathGeometry> を使用して、円弧や曲線などのより複雑な図形を作成することもできます。 例については、「[楕円弧を作成](how-to-create-an-elliptical-arc.md)する」、「 [3 次ベジエ曲線を作成](how-to-create-a-cubic-bezier-curve.md)する」、および「 [2 次ベジエ曲線を作成](how-to-create-a-quadratic-bezier-curve.md)する」を参照してください。  
  
 この例は、より大きなサンプルの一部です。完全なサンプルについては、「[ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Shapes.Path>
- <xref:System.Windows.Media.GeometryDrawing>
- [ジオメトリの概要](geometry-overview.md)
- [ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)
