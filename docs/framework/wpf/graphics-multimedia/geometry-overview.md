---
title: ジオメトリの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- geometry classes [WPF]
- graphics [WPF], geometry classes
ms.assetid: 9fba8934-98b7-4af6-82f6-f4ef887f963a
ms.openlocfilehash: f45c13e1af9bc2d1f75da11b13a2c03936b136c1
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73455008"
---
# <a name="geometry-overview"></a>ジオメトリの概要
この概要では、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] <xref:System.Windows.Media.Geometry> クラスを使用して図形を記述する方法について説明します。 このトピックでは、<xref:System.Windows.Media.Geometry> オブジェクトと <xref:System.Windows.Shapes.Shape> 要素の違いについても説明します。  

<a name="wcpsdk_graphics_geometry_introduction"></a>   
## <a name="what-is-a-geometry"></a>ジオメトリとは  
 <xref:System.Windows.Media.Geometry> クラスとそれから派生するクラス (<xref:System.Windows.Media.EllipseGeometry>、<xref:System.Windows.Media.PathGeometry>、<xref:System.Windows.Media.CombinedGeometry>など) を使用すると、2-d 図形のジオメトリを記述できます。 これらの幾何学的な記述には、画面を塗りつぶす図形を定義したり、ヒット テストやクリップ領域を定義するなど、多くの用途があります。 ジオメトリを使用して、アニメーション パスを定義することもできます。  
  
 <xref:System.Windows.Media.Geometry> オブジェクトは、2つ以上の geometry オブジェクトから作成された四角形、円、複合など、単純な場合があります。  <xref:System.Windows.Media.PathGeometry> クラスと <xref:System.Windows.Media.StreamGeometry> クラスを使用して、より複雑なジオメトリを作成できます。これにより、円弧と曲線を記述できます。  
  
 <xref:System.Windows.Media.Geometry> は <xref:System.Windows.Freezable>の一種であるため、<xref:System.Windows.Media.Geometry> オブジェクトはいくつかの特殊な機能を提供します。これらは[リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)として宣言したり、複数のオブジェクト間で共有したり、パフォーマンスを向上させたり、複製したり、スレッドセーフにしたりするために読み取り専用にすることができます。 <xref:System.Windows.Freezable> オブジェクトによって提供されるさまざまな機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。  
  
<a name="wcpsdk_graphics_geometry_geometryandshapes"></a>   
## <a name="geometries-vs-shapes"></a>ジオメトリと図形  
 <xref:System.Windows.Media.Geometry> クラスと <xref:System.Windows.Shapes.Shape> クラスは、両方とも2-d 図形 (たとえば、<xref:System.Windows.Media.EllipseGeometry> と <xref:System.Windows.Shapes.Ellipse> を比較する) を記述するのと同じように見えますが、重要な違いがあります。  
  
 1つの場合、<xref:System.Windows.Media.Geometry> クラスは <xref:System.Windows.Freezable> クラスを継承し、<xref:System.Windows.Shapes.Shape> クラスは <xref:System.Windows.FrameworkElement>から継承されます。 これらは要素であるため、<xref:System.Windows.Shapes.Shape> オブジェクトは、それ自体をレンダリングしてレイアウトシステムに参加させることができますが、<xref:System.Windows.Media.Geometry> オブジェクトは使用できません。  
  
 <xref:System.Windows.Shapes.Shape> オブジェクトは <xref:System.Windows.Media.Geometry> オブジェクトよりも簡単に使用できますが、<xref:System.Windows.Media.Geometry> オブジェクトの方が汎用性があります。 <xref:System.Windows.Shapes.Shape> オブジェクトは2-d グラフィックスのレンダリングに使用されますが、<xref:System.Windows.Media.Geometry> オブジェクトを使用して、2-d グラフィックスの幾何学領域を定義したり、クリッピング用の領域を定義したり、ヒットテストのための領域を定義したりできます。  
  
### <a name="the-path-shape"></a>パス図形  
 <xref:System.Windows.Shapes.Path> クラスである1つの <xref:System.Windows.Shapes.Shape>は、実際には <xref:System.Windows.Media.Geometry> を使用してその内容を記述します。 <xref:System.Windows.Shapes.Path> の [<xref:System.Windows.Shapes.Path.Data%2A>] プロパティを <xref:System.Windows.Media.Geometry> に設定し、その <xref:System.Windows.Shapes.Shape.Fill%2A> および <xref:System.Windows.Shapes.Shape.Stroke%2A> のプロパティを設定することにより、<xref:System.Windows.Media.Geometry>を表示できます。  
  
<a name="commonproperties"></a>   
## <a name="common-properties-that-take-a-geometry"></a>ジオメトリを使用する一般的なプロパティ  
 これまでのセクションでは、図形の描画、アニメーション、クリッピングなどのさまざまな目的で、ジオメトリ オブジェクトを他のオブジェクトと共に使用できることを説明しました。 次の表に、<xref:System.Windows.Media.Geometry> オブジェクトを受け取るプロパティを持つクラスをいくつか示します。  
  
|[種類]|property|  
|----------|--------------|  
|<xref:System.Windows.Media.Animation.DoubleAnimationUsingPath>|<xref:System.Windows.Media.Animation.DoubleAnimationUsingPath.PathGeometry%2A>|  
|<xref:System.Windows.Media.DrawingGroup>|<xref:System.Windows.Media.DrawingGroup.ClipGeometry%2A>|  
|<xref:System.Windows.Media.GeometryDrawing>|<xref:System.Windows.Media.GeometryDrawing.Geometry%2A>|  
|<xref:System.Windows.Shapes.Path>|<xref:System.Windows.Shapes.Path.Data%2A>|  
|<xref:System.Windows.UIElement>|<xref:System.Windows.UIElement.Clip%2A>|  
  
<a name="wcpsdk_graphics_geometry_geometrytypes"></a>   
## <a name="simple-geometry-types"></a>単純なジオメトリの種類  
 すべてのジオメトリの基底クラスは <xref:System.Windows.Media.Geometry>抽象クラスです。  <xref:System.Windows.Media.Geometry> クラスから派生したクラスは、単純なジオメトリ、パスジオメトリ、および複合ジオメトリという3つのカテゴリに大まかに分類できます。  
  
 単純な geometry クラスには、<xref:System.Windows.Media.LineGeometry>、<xref:System.Windows.Media.RectangleGeometry>、および <xref:System.Windows.Media.EllipseGeometry> が含まれ、線、四角形、円などの基本的なジオメトリック形状を作成するために使用されます。  
  
- <xref:System.Windows.Media.LineGeometry> は、直線と終点の始点を指定することによって定義されます。  
  
- <xref:System.Windows.Media.RectangleGeometry> は、その相対位置と高さと幅を指定する <xref:System.Windows.Rect> 構造体で定義されます。 <xref:System.Windows.Media.RectangleGeometry.RadiusX%2A> と <xref:System.Windows.Media.RectangleGeometry.RadiusY%2A> のプロパティを設定することにより、角丸四角形を作成できます。  
  
- <xref:System.Windows.Media.EllipseGeometry> は、中心点、x 半径、および y 半径によって定義されます。  レンダリングとクリッピング用の単純ジオメトリの作成方法を次の例に示します。  
  
 これらの同じ図形と複雑な図形は、<xref:System.Windows.Media.PathGeometry> を使用して作成することも、geometry オブジェクトを組み合わせることによって作成することもできますが、これらのクラスは、これらの基本的なジオメトリック形状を生成するためのより簡単な方法を提供します。  
  
 次の例では、<xref:System.Windows.Media.LineGeometry>を作成して表示する方法を示します。  前述のように、<xref:System.Windows.Media.Geometry> オブジェクトはそれ自体を描画できないため、この例では <xref:System.Windows.Shapes.Path> 図形を使用して線を表示します。  線には領域がないため、<xref:System.Windows.Shapes.Path> の <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティを設定しても効果はありません。代わりに、<xref:System.Windows.Shapes.Shape.Stroke%2A> と <xref:System.Windows.Shapes.Shape.StrokeThickness%2A> のプロパティのみが指定されています。 この例からの出力を次の図に示します。  
  
 ![LineGeometry](./media/graphicsmm-line.gif "graphicsmm_line")  
(10,20) から (100,130) まで描画された LineGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMLineGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmlinegeometryexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMLineGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmlinegeometryexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMLineGeometryExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmlinegeometryexample)]  
  
 次の例では、<xref:System.Windows.Media.EllipseGeometry>を作成して表示する方法を示します。  この例では、<xref:System.Windows.Media.EllipseGeometry> の <xref:System.Windows.Media.EllipseGeometry.Center%2A> をポイント `50,50` に設定し、x 半径と y 半径の両方を `50`に設定します。これにより、直径100の円が作成されます。  楕円の内部は、パス要素の Fill プロパティ (この場合 <xref:System.Windows.Media.Brushes.Gold%2A>) に値を割り当てることによって描画されます。 この例からの出力を次の図に示します。  
  
 ![EllipseGeometry](./media/graphicsmm-ellipse.gif "graphicsmm_ellipse")  
(50,50) に描画された EllipseGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMEllipseGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmellipsegeometryexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMEllipseGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmellipsegeometryexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMEllipseGeometryExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmellipsegeometryexample)]  
  
 次の例では、<xref:System.Windows.Media.RectangleGeometry>を作成して表示する方法を示します。  四角形の位置と大きさは、<xref:System.Windows.Rect> 構造体によって定義されます。 位置は `50,50`、高さと幅は両方とも `25` で、正方形が作成されます。 この例からの出力を次の図に示します。  
  
 ![RectangleGeometry](./media/graphicsmm-rectangle.gif "graphicsmm_rectangle")  
50,50 に描画された RectangleGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMRectangleGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmrectanglegeometryexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMRectangleGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmrectanglegeometryexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMRectangleGeometryExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmrectanglegeometryexample)]  
  
 次の例は、イメージのクリップ領域として <xref:System.Windows.Media.EllipseGeometry> を使用する方法を示しています。  <xref:System.Windows.Controls.Image> オブジェクトは、200の <xref:System.Windows.FrameworkElement.Width%2A> と150の <xref:System.Windows.FrameworkElement.Height%2A> を使用して定義されます。  <xref:System.Windows.Media.EllipseGeometry.RadiusX%2A> 値が100、<xref:System.Windows.Media.EllipseGeometry.RadiusY%2A> 値が75、<xref:System.Windows.Media.EllipseGeometry.Center%2A> 値が100、75の <xref:System.Windows.Media.EllipseGeometry> が、イメージの <xref:System.Windows.UIElement.Clip%2A> プロパティに設定されています。  イメージの楕円の領域内の部分だけが表示されます。 この例からの出力を次の図に示します。  
  
 ![クリッピングを使用する (または使用しない) イメージ](./media/graphicsmm-clipexample.png "graphicsmm_clipexample")  
イメージ コントロールのクリップに使用される EllipseGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMImageClipGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmimageclipgeometryexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMImageClipGeometryExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmimageclipgeometryexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMImageClipGeometryExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmimageclipgeometryexample)]  
  
<a name="wcpsdk_graphics_geometry_pathgeometry"></a>   
## <a name="path-geometries"></a>パス ジオメトリ  
 <xref:System.Windows.Media.PathGeometry> クラスとその軽量の <xref:System.Windows.Media.StreamGeometry> クラスは、円弧、曲線、および線で構成される複数の複雑な数値を記述する手段を提供します。  
  
 <xref:System.Windows.Media.PathGeometry> の中核となるのは、<xref:System.Windows.Media.PathFigure> オブジェクトのコレクションです。各図には <xref:System.Windows.Media.PathGeometry>内の個別の図形が記述されているため、という名前が付けられています。 各 <xref:System.Windows.Media.PathFigure> は、それぞれが図形のセグメントを表す1つ以上の <xref:System.Windows.Media.PathSegment> オブジェクトで構成されています。  
  
 多くの種類のセグメントがあります。  
  
|セグメントの種類|説明|例|  
|------------------|-----------------|-------------|  
|<xref:System.Windows.Media.ArcSegment>|2 つの点を結ぶ楕円の円弧を作成します。|[楕円の円弧を作成する](how-to-create-an-elliptical-arc.md)。|  
|<xref:System.Windows.Media.BezierSegment>|2 つの点を結ぶ 3 次ベジエ曲線を作成します。|[3 次ベジエ曲線を作成する](how-to-create-a-cubic-bezier-curve.md)。|  
|<xref:System.Windows.Media.LineSegment>|2 つの点を結ぶ直性を作成します。|[PathGeometry で LineSegment を作成する](how-to-create-a-linesegment-in-a-pathgeometry.md)|  
|<xref:System.Windows.Media.PolyBezierSegment>|一続きの 3 次ベジエ曲線を作成します。|<xref:System.Windows.Media.PolyBezierSegment> の種類のページを参照してください。|  
|<xref:System.Windows.Media.PolyLineSegment>|一続きの直線を作成します。|<xref:System.Windows.Media.PolyLineSegment> の種類のページを参照してください。|  
|<xref:System.Windows.Media.PolyQuadraticBezierSegment>|一続きの 2 次ベジエ曲線を作成します。|<xref:System.Windows.Media.PolyQuadraticBezierSegment> のページを参照してください。|  
|<xref:System.Windows.Media.QuadraticBezierSegment>|2 次ベジエ曲線を作成します。|[2 次ベジエ曲線を作成する](how-to-create-a-quadratic-bezier-curve.md)。|  
  
 <xref:System.Windows.Media.PathFigure> 内のセグメントは1つの幾何図形に結合され、各セグメントの終点が次のセグメントの始点になります。 <xref:System.Windows.Media.PathFigure> の <xref:System.Windows.Media.PathFigure.StartPoint%2A> プロパティは、最初のセグメントが描画される地点を指定します。 後続の各セグメントは、前のセグメントの終点から始まります。 たとえば、`10,50` から `10,150` までの垂直線を定義するには、<xref:System.Windows.Media.PathFigure.StartPoint%2A> プロパティを `10,50` に設定し、<xref:System.Windows.Media.LineSegment> プロパティ設定 <xref:System.Windows.Media.LineSegment.Point%2A> を使用して `10,150`を作成します。  
  
 次の例では、<xref:System.Windows.Media.LineSegment> を持つ単一の <xref:System.Windows.Media.PathFigure> で構成される単純な <xref:System.Windows.Media.PathGeometry> を作成し、<xref:System.Windows.Shapes.Path> 要素を使用して表示します。 <xref:System.Windows.Media.PathFigure> オブジェクトの <xref:System.Windows.Media.PathFigure.StartPoint%2A> は `10,20` に設定され、<xref:System.Windows.Media.LineSegment> は `100,130`のエンドポイントで定義されます。 次の図は、この例で作成した <xref:System.Windows.Media.PathGeometry> を示しています。  
  
 ![LineGeometry](./media/graphicsmm-line.gif "graphicsmm_line")  
単一の LineSegment を含む PathGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMPathGeometryLineExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmpathgeometrylineexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryLineExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmpathgeometrylineexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryLineExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmpathgeometrylineexample)]  
  
 上記の <xref:System.Windows.Media.LineGeometry> 例と比較すると、この例と比較してみる価値があります。  <xref:System.Windows.Media.PathGeometry> に使用される構文は、単純な <xref:System.Windows.Media.LineGeometry>に使用される構文よりもはるかに詳細であり、この場合は <xref:System.Windows.Media.LineGeometry> クラスを使用する方が理にかなっていますが、<xref:System.Windows.Media.PathGeometry> の詳細な構文では非常に複雑で複雑なジオメトリック領域を使用できます。  
  
 <xref:System.Windows.Media.PathSegment> オブジェクトを組み合わせて使用すると、より複雑なジオメトリを作成できます。  
  
 次の例では、図形を作成するために、<xref:System.Windows.Media.BezierSegment>、<xref:System.Windows.Media.LineSegment>、および <xref:System.Windows.Media.ArcSegment> を使用します。 この例では、最初に3番目のベジエ曲線を作成します。開始点は、前のセグメントの終点、エンドポイント (<xref:System.Windows.Media.BezierSegment.Point3%2A>)、および2つの制御点 (<xref:System.Windows.Media.BezierSegment.Point1%2A> と <xref:System.Windows.Media.BezierSegment.Point2%2A>) を定義することです。  3 次ベジエ曲線の 2 つの制御点は磁石のように動作し、本来は直線になる部分を制御点の方へ引き寄せ、曲線を生成します。 最初の制御点である <xref:System.Windows.Media.BezierSegment.Point1%2A>は、曲線の開始部分に影響します。2つ目の制御点である <xref:System.Windows.Media.BezierSegment.Point2%2A>は、曲線の終了部分に影響します。  
  
 次に、<xref:System.Windows.Media.LineSegment>を追加します。これは、前の <xref:System.Windows.Media.BezierSegment> の終点の間に描画され、その <xref:System.Windows.Media.LineSegment> プロパティによって指定された点に先行します。  
  
 この例では、<xref:System.Windows.Media.ArcSegment>を追加します。これは、前の <xref:System.Windows.Media.LineSegment> の終点から <xref:System.Windows.Media.ArcSegment.Point%2A> プロパティで指定されたポイントまで描画されます。 この例では、円弧の x と y 半径 (<xref:System.Windows.Media.ArcSegment.Size%2A>)、回転角度 (<xref:System.Windows.Media.ArcSegment.RotationAngle%2A>)、結果として得られる弧の角度 (<xref:System.Windows.Media.ArcSegment.IsLargeArc%2A>)、および円弧が描画される方向 (<xref:System.Windows.Media.ArcSegment.SweepDirection%2A>) を示すフラグも指定します。 この例で作成した図形を次の図に示します。  
  
 ![PathGeometry](./media/graphicsmm-path2.gif "graphicsmm_path2")  
PathGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMPathGeometryComplexExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmpathgeometrycomplexexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryComplexExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmpathgeometrycomplexexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryComplexExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmpathgeometrycomplexexample)]  
  
 <xref:System.Windows.Media.PathGeometry>内の複数の <xref:System.Windows.Media.PathFigure> オブジェクトを使用して、より複雑なジオメトリを作成することもできます。  
  
 次の例では、2つの <xref:System.Windows.Media.PathFigure> オブジェクトを持つ <xref:System.Windows.Media.PathGeometry> を作成します。各オブジェクトには複数の <xref:System.Windows.Media.PathSegment> オブジェクトが含まれています。  上の例の <xref:System.Windows.Media.PathFigure> と、<xref:System.Windows.Media.PolyLineSegment> と <xref:System.Windows.Media.QuadraticBezierSegment> を持つ <xref:System.Windows.Media.PathFigure> が使用されます。  <xref:System.Windows.Media.PolyLineSegment> は、点の配列で定義され、<xref:System.Windows.Media.QuadraticBezierSegment> は制御ポイントとエンドポイントで定義されます。 この例で作成した図形を次の図に示します。  
  
 ![PathGeometry](./media/graphicsmm-path.gif "graphicsmm_path")  
複数の図形を使用する PathGeometry  
  
 [!code-xaml[GeometryOverviewSamples_snip#GraphicsMMPathGeometryComplexMultiExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_snip/CS/GeometryExamples.xaml#graphicsmmpathgeometrycomplexmultiexample)]  
  
 [!code-csharp[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryComplexMultiExample](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/CSharp/GeometryExamples.cs#graphicsmmpathgeometrycomplexmultiexample)]
 [!code-vb[GeometryOverviewSamples_procedural_snip#GraphicsMMPathGeometryComplexMultiExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometryOverviewSamples_procedural_snip/visualbasic/geometryexamples.vb#graphicsmmpathgeometrycomplexmultiexample)]  
  
### <a name="streamgeometry"></a>StreamGeometry  
 <xref:System.Windows.Media.PathGeometry> クラスと同様に、<xref:System.Windows.Media.StreamGeometry> は曲線、円弧、直線を含む複雑な幾何学図形を定義します。 <xref:System.Windows.Media.PathGeometry>とは異なり、<xref:System.Windows.Media.StreamGeometry> のコンテンツは、データバインディング、アニメーション、または変更をサポートしていません。 複雑なジオメトリを記述する必要があり、データバインディング、アニメーション、または変更をサポートするオーバーヘッドを避けたい場合は、<xref:System.Windows.Media.StreamGeometry> を使用します。 <xref:System.Windows.Media.StreamGeometry> クラスは、その効率性のために、装飾を記述するのに適した選択肢です。  
  
 例については、「[方法 : StreamGeometry を使用して図形を作成する](how-to-create-a-shape-using-a-streamgeometry.md)」をご覧ください。  
  
### <a name="path-markup-syntax"></a>パス マークアップ構文  
 <xref:System.Windows.Media.PathGeometry> 型と <xref:System.Windows.Media.StreamGeometry> 型では、一連の移動および描画コマンドを使用して [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 属性構文をサポートしています。 詳しくは、「[パス マークアップ構文](path-markup-syntax.md)」をご覧ください。  
  
<a name="wcpsdk_graphics_geometry_introduction2"></a>   
## <a name="composite-geometries"></a>複合ジオメトリ  
 複合 geometry オブジェクトを作成するには、<xref:System.Windows.Media.GeometryGroup>、<xref:System.Windows.Media.CombinedGeometry>を使用するか、または静的 <xref:System.Windows.Media.Geometry> メソッド <xref:System.Windows.Media.Geometry.Combine%2A>を呼び出します。  
  
- <xref:System.Windows.Media.CombinedGeometry> オブジェクトと <xref:System.Windows.Media.Geometry.Combine%2A> メソッドは、2つのジオメトリによって定義された領域を結合するブール演算を実行します。 領域のない <xref:System.Windows.Media.Geometry> オブジェクトは破棄されます。 結合できる <xref:System.Windows.Media.Geometry> オブジェクトは2つだけです (ただし、これら2つのジオメトリを複合ジオメトリにすることもできます)。  
  
- <xref:System.Windows.Media.GeometryGroup> クラスは、領域を結合せずに、含まれている <xref:System.Windows.Media.Geometry> オブジェクトの併せ持つを作成します。 <xref:System.Windows.Media.GeometryGroup>には、任意の数の <xref:System.Windows.Media.Geometry> オブジェクトを追加できます。 例については、「[方法 : 複合図形を作成する](how-to-create-a-composite-shape.md)」をご覧ください。  
  
 結合操作を実行しないため、<xref:System.Windows.Media.GeometryGroup> オブジェクトを使用すると、<xref:System.Windows.Media.CombinedGeometry> オブジェクトまたは <xref:System.Windows.Media.Geometry.Combine%2A> メソッドを使用した場合よりもパフォーマンスが向上します。  
  
<a name="combindgeometriessection"></a>   
## <a name="combined-geometries"></a>結合したジオメトリ  
 前のセクションでは、<xref:System.Windows.Media.CombinedGeometry> オブジェクトと <xref:System.Windows.Media.Geometry.Combine%2A> メソッドが、含まれているジオメトリによって定義された領域を結合しています。 <xref:System.Windows.Media.GeometryCombineMode> 列挙体は、ジオメトリの結合方法を指定します。 <xref:System.Windows.Media.CombinedGeometry.GeometryCombineMode%2A> プロパティに指定できる値は、<xref:System.Windows.Media.GeometryCombineMode.Union>、<xref:System.Windows.Media.GeometryCombineMode.Intersect>、<xref:System.Windows.Media.GeometryCombineMode.Exclude>、および <xref:System.Windows.Media.GeometryCombineMode.Xor>です。  
  
 次の例では、和集合の結合モードで <xref:System.Windows.Media.CombinedGeometry> が定義されています。  <xref:System.Windows.Media.CombinedGeometry.Geometry1%2A> と <xref:System.Windows.Media.CombinedGeometry.Geometry2%2A> は両方とも同じ半径の円として定義されますが、中心は50でオフセットされます。  
  
 [!code-xaml[GeometrySample#23](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/combininggeometriesexample.xaml#23)]  
  
 ![和集合結合モードの結果](./media/mil-task-combined-geometry-union.PNG "mil_task_combined_geometry_union")  
  
 次の例では、<xref:System.Windows.Media.CombinedGeometry> が <xref:System.Windows.Media.GeometryCombineMode.Xor>の結合モードで定義されています。  <xref:System.Windows.Media.CombinedGeometry.Geometry1%2A> と <xref:System.Windows.Media.CombinedGeometry.Geometry2%2A> は両方とも同じ半径の円として定義されますが、中心は50でオフセットされます。  
  
 [!code-xaml[GeometrySample#24](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/combininggeometriesexample.xaml#24)]  
  
 ![Xor 結合モードの結果](./media/mil-task-combined-geometry-xor.PNG "mil_task_combined_geometry_xor")  
  
 その他の例については、「[方法 : 複合図形を作成する](how-to-create-a-composite-shape.md)」と「[方法 : 結合したジオメトリを作成する](how-to-create-a-combined-geometry.md)」をご覧ください。  
  
<a name="freezable_features"></a>   
## <a name="freezable-features"></a>Freezable 機能  
 <xref:System.Windows.Media.Geometry> クラスは <xref:System.Windows.Freezable> クラスから継承するため、いくつかの特殊な機能を提供します。 <xref:System.Windows.Media.Geometry> オブジェクトを[XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)として宣言したり、複数のオブジェクト間で共有したり、パフォーマンスを向上させるために読み取り専用にしたり、複製したり、作成したりすることができます。スレッドセーフ。 <xref:System.Windows.Freezable> オブジェクトによって提供されるさまざまな機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。  
  
<a name="othergeometryfeatures"></a>   
## <a name="other-geometry-features"></a>その他のジオメトリ機能  
 <xref:System.Windows.Media.Geometry> クラスには、次のような便利なユーティリティメソッドも用意されています。  
  
- <xref:System.Windows.Media.Geometry.GetArea%2A>-<xref:System.Windows.Media.Geometry>の領域を取得します。  
  
- <xref:System.Windows.Media.Geometry.FillContains%2A>-ジオメトリに別の <xref:System.Windows.Media.Geometry>が含まれているかどうかを判断します。  
  
- <xref:System.Windows.Media.Geometry.StrokeContains%2A>-<xref:System.Windows.Media.Geometry> のストロークに指定した点が含まれているかどうかを判断します。  
  
 メソッドの完全な一覧については、<xref:System.Windows.Media.Geometry> クラスを参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Geometry>
- <xref:System.Windows.Media.PathGeometry>
- <xref:System.Windows.Shapes.Path>
- <xref:System.Windows.Media.GeometryDrawing>
- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [パス マークアップ構文](path-markup-syntax.md)
- [方法トピック](geometries-how-to-topics.md)
- [アニメーションの概要](animation-overview.md)
- [WPF での図形と基本描画の概要](shapes-and-basic-drawing-in-wpf-overview.md)
- [Drawing オブジェクトの概要](drawing-objects-overview.md)
