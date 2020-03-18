---
title: '方法 : 楕円の円弧を作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [WPF], elliptical arcs
- elliptical arcs [WPF], creating
- arcs [WPF], elliptical
ms.assetid: 3dcfe502-3485-45de-99fb-d53a1367c484
ms.openlocfilehash: d0fffbb25f3c5aaceb2cd80af4f1093e44111200
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453066"
---
# <a name="how-to-create-an-elliptical-arc"></a>方法 : 楕円の円弧を作成する
この例では、楕円の円弧を描画する方法を示します。楕円の円弧を作成するには、<xref:System.Windows.Media.PathGeometry>、<xref:System.Windows.Media.PathFigure>、および <xref:System.Windows.Media.ArcSegment> クラスを使用します。  
  
## <a name="example"></a>例  
 次の例では、楕円弧が (10100) から (200100) に描画されます。 弧には、100の <xref:System.Windows.Media.ArcSegment.Size%2A> が50デバイスに依存しないピクセル、<xref:System.Windows.Media.ArcSegment.RotationAngle%2A> 45 度、`true`の <xref:System.Windows.Media.ArcSegment.IsLargeArc%2A> 設定、<xref:System.Windows.Media.ArcSegment.SweepDirection%2A> の <xref:System.Windows.Media.SweepDirection.Counterclockwise>があります。  
  
 [xaml]  
  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、属性構文を使用してパスを記述できます。  
  
 [!code-xaml[GeometrySample#56](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/geometryattributesyntaxexample.xaml#56)]  
  
 [xaml]  
  
 (この属性構文では、実際には <xref:System.Windows.Media.PathGeometry>の軽量バージョンである <xref:System.Windows.Media.StreamGeometry>が作成されることに注意してください。 詳細については、「[パス マークアップ構文](path-markup-syntax.md)」のページを参照してください。)  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、オブジェクトタグを明示的に使用して楕円の円弧を描画することもできます。 次に示すのは、前の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップに相当します。  
  
 [!code-xaml[GeometrySample#36](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/pathgeometryexample.xaml#36)]  
  
 この例は、さらに大きなサンプルの一部です。 完全なサンプルについては、「[ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)」を参照してください。  
  
## <a name="see-also"></a>参照

- [2 次ベジエ曲線を作成する](how-to-create-a-quadratic-bezier-curve.md)
- [3 次ベジエ曲線を作成する](how-to-create-a-cubic-bezier-curve.md)
