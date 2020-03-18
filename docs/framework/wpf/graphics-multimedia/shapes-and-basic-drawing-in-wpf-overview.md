---
title: 図形と基本描画の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- shapes [WPF], about shapes
- basic drawing [WPF]
- vector drawing [WPF], overview
- vectors [WPF], drawing
- Shape objects [WPF]
ms.assetid: 66d7a6d6-e3b6-47bc-8dfe-8a1b26f7d901
ms.openlocfilehash: dfdbf67d35cbb13e80d0c5184f165b0cc660e2ef
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731626"
---
# <a name="shapes-and-basic-drawing-in-wpf-overview"></a>WPF での図形と基本描画の概要
このトピックでは、<xref:System.Windows.Shapes.Shape> オブジェクトを使用して描画する方法の概要について説明します。 <xref:System.Windows.Shapes.Shape> は、画面に図形を描画できるようにする <xref:System.Windows.UIElement> の一種です。 これらは UI 要素であるため、<xref:System.Windows.Shapes.Shape> のオブジェクトは <xref:System.Windows.Controls.Panel> の要素やほとんどのコントロール内で使用できます。  
  
 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、グラフィックス サービスやレンダリング サービスへのアクセスのレイヤーがいくつか用意されています。 最上位のレイヤーでは、<xref:System.Windows.Shapes.Shape> オブジェクトは使いやすく、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] イベントシステムへのレイアウトや参加など、多くの便利な機能を提供します。  

<a name="shapes"></a>   
## <a name="shape-objects"></a>図形オブジェクト  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、すぐに使用できる <xref:System.Windows.Shapes.Shape> オブジェクトがいくつか用意されています。  すべての図形オブジェクトは、<xref:System.Windows.Shapes.Shape> クラスから継承されます。 使用できる図形オブジェクトには、<xref:System.Windows.Shapes.Ellipse>、<xref:System.Windows.Shapes.Line>、<xref:System.Windows.Shapes.Path>、<xref:System.Windows.Shapes.Polygon>、<xref:System.Windows.Shapes.Polyline>、および <xref:System.Windows.Shapes.Rectangle>があります。 <xref:System.Windows.Shapes.Shape> オブジェクトは、次の共通プロパティを共有します。  
  
- <xref:System.Windows.Shapes.Shape.Stroke%2A>: 図形のアウトラインの描画方法を記述します。  
  
- <xref:System.Windows.Shapes.Shape.StrokeThickness%2A>: 図形の輪郭の太さを表します。  
  
- <xref:System.Windows.Shapes.Shape.Fill%2A>: 図形の内部を塗りつぶす方法を記述します。  
  
- デバイス非依存ピクセル単位で計測される座標と頂点を指定するデータ プロパティ。  
  
 これらは <xref:System.Windows.UIElement>から派生するため、図形オブジェクトは、パネルやほとんどのコントロール内で使用できます。 <xref:System.Windows.Controls.Canvas> パネルは、子オブジェクトの絶対配置をサポートするため、複雑な描画を作成する場合に特に適しています。  
  
 <xref:System.Windows.Shapes.Line> クラスを使用すると、2つの点の間に線を描画できます。 直線の座標およびストローク プロパティを指定するいくつかの方法を次の例に示します。  
  
 [!code-xaml[drawingwithshapeelements#LineExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/lineexample.xaml#lineexample1)]  
  
 [!code-cpp[shapesprocedural#ShapesProceduralLine](~/samples/snippets/cpp/VS_Snippets_Wpf/ShapesProcedural/CPP/ShapesProcedural.cpp#shapesproceduralline)]
 [!code-csharp[shapesprocedural#ShapesProceduralLine](~/samples/snippets/csharp/VS_Snippets_Wpf/ShapesProcedural/Csharp/ShapesProcedural.cs#shapesproceduralline)]
 [!code-vb[shapesprocedural#ShapesProceduralLine](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ShapesProcedural/VisualBasic/ShapesProceduralVB.vb#shapesproceduralline)]  
  
 次の図は、表示される <xref:System.Windows.Shapes.Line>を示しています。  
  
 ![線の図](./media/shape-ovw-line2.gif "shape_ovw_line2")  
  
 <xref:System.Windows.Shapes.Line> クラスは <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティを提供しますが、<xref:System.Windows.Shapes.Line> には領域がないため、このプロパティを設定しても効果はありません。  
  
 もう1つの一般的な図形は、<xref:System.Windows.Shapes.Ellipse>です。  図形の <xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> プロパティを定義して、<xref:System.Windows.Shapes.Ellipse> を作成します。 円を描画するには、<xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> 値が等しい <xref:System.Windows.Shapes.Ellipse> を指定します。  
  
 [!code-xaml[ShapeOverviews#ShapesOVW1](~/samples/snippets/csharp/VS_Snippets_Wpf/ShapeOverviews/CS/Window1.xaml#shapesovw1)]  
  
 [!code-csharp[brushesmiscsnippets_procedural_snip#SetBackgroundColorOfShapeCodeExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushesMiscSnippets_procedural_snip/CSharp/SetBackgroundColorOfShapeExample.cs#setbackgroundcolorofshapecodeexamplewholepage)]
 [!code-vb[brushesmiscsnippets_procedural_snip#SetBackgroundColorOfShapeCodeExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BrushesMiscSnippets_procedural_snip/visualbasic/setbackgroundcolorofshapeexample.vb#setbackgroundcolorofshapecodeexamplewholepage)]  
  
 次の図は、表示される <xref:System.Windows.Shapes.Ellipse>の例を示しています。  
  
 ![楕円の図](./media/shape-ovw-ellipse2.png "shape_ovw_ellipse2")  
  
<a name="paths"></a>   
## <a name="using-paths-and-geometries"></a>パスとジオメトリの使用  
 <xref:System.Windows.Shapes.Path> クラスを使用すると、曲線や複雑な形状を描画できます。 これらの曲線と図形は <xref:System.Windows.Media.Geometry> オブジェクトを使用して記述されます。 <xref:System.Windows.Shapes.Path>を使用するには、<xref:System.Windows.Media.Geometry> を作成し、それを使用して <xref:System.Windows.Shapes.Path> オブジェクトの <xref:System.Windows.Shapes.Path.Data%2A> プロパティを設定します。  
  
 さまざまな <xref:System.Windows.Media.Geometry> オブジェクトから選択できます。 <xref:System.Windows.Media.LineGeometry>、<xref:System.Windows.Media.RectangleGeometry>、および <xref:System.Windows.Media.EllipseGeometry> クラスでは、比較的単純なシェイプが記述されています。 より複雑な図形を作成したり、曲線を作成したりするには、<xref:System.Windows.Media.PathGeometry>を使用します。  
  
<a name="pathgeometry"></a>   
### <a name="pathgeometry-and-pathsegments"></a>PathGeometry と PathSegment  
 <xref:System.Windows.Media.PathGeometry> オブジェクトは、1つまたは複数の <xref:System.Windows.Media.PathFigure> オブジェクトで構成されます。各 <xref:System.Windows.Media.PathFigure> は、別の "図" または図形を表します。 各 <xref:System.Windows.Media.PathFigure> は、それぞれが図形または形状の接続された部分を表す1つ以上の <xref:System.Windows.Media.PathSegment> オブジェクトで構成されています。 セグメントの種類には、<xref:System.Windows.Media.LineSegment>、<xref:System.Windows.Media.BezierSegment>、および <xref:System.Windows.Media.ArcSegment>があります。  
  
 次の例では、2次ベジエ曲線を描画するために <xref:System.Windows.Shapes.Path> が使用されています。  
  
 [!code-xaml[geometrysample#34](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/pathgeometryexample.xaml#34)]  
  
 レンダリングされた図形を次の図に示します。  
  
 ![パスの図](./media/shape-ovw-path2.gif "shape_ovw_path2")  
  
 <xref:System.Windows.Media.PathGeometry> およびその他の <xref:System.Windows.Media.Geometry> クラスの詳細については、「 [Geometry の概要](geometry-overview.md)」を参照してください。  
  
<a name="pathdatastring"></a>   
### <a name="xaml-abbreviated-syntax"></a>省略された XAML 構文  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、<xref:System.Windows.Shapes.Path>を記述するために、特殊な省略構文を使用することもできます。 次の例では、省略された構文を使用して複雑な図形を描画します。  
  
```xaml  
      <Path Stroke="DarkGoldenRod" StrokeThickness="3"   
Data="M 100,200 C 100,25 400,350 400,175 H 280" />  
```  
  
 次の図は、表示される <xref:System.Windows.Shapes.Path>を示しています。  
  
 ![パスの図](./media/shape-ovw-path.PNG "shape_ovw_path")  
  
 <xref:System.Windows.Shapes.Path.Data%2A> 属性文字列は、M で示される "moveto" コマンドで始まります。このコマンドは、<xref:System.Windows.Controls.Canvas>の座標系のパスの始点を確立します。 <xref:System.Windows.Shapes.Path> のデータパラメーターでは、大文字と小文字が区別されます。 大文字の M は、新しい現在のポイントの絶対位置を示します。 小文字の m は、相対座標を示します。 最初のセグメントは、2 つの制御点 (100,25) と (400,350) を使用して描画される、始点が (100,200) で終点が (400,175) の 3 次ベジエ曲線です。 このセグメントは、<xref:System.Windows.Shapes.Path.Data%2A> 属性文字列の C コマンドによって示されます。 ここでも、大文字の C は絶対パスを示し、小文字の c は相対パスを示します。  
  
 2 番目のセグメントは、絶対水平 "lineto" コマンド H で始まります。このコマンドは、前のサブパスの終点 (400,175) から新しい終点 (280,175) まで描画される直線を指定します。 水平方向の "lineto" コマンドであるため、指定された値は*x*座標です。  
  
 完全なパスの構文については、「<xref:System.Windows.Shapes.Path.Data%2A> 参照」と「 [PathGeometry を使用して図形を作成](how-to-create-a-shape-by-using-a-pathgeometry.md)する」を参照してください。  
  
<a name="fillpaint"></a>   
## <a name="painting-shapes"></a>図形の塗りつぶし  
 <xref:System.Windows.Media.Brush> オブジェクトは、図形の <xref:System.Windows.Shapes.Shape.Stroke%2A> と <xref:System.Windows.Shapes.Shape.Fill%2A>を描画するために使用されます。 次の例では、<xref:System.Windows.Shapes.Ellipse> のストロークと塗りつぶしが指定されています。 ブラシ プロパティの有効な入力は、キーワードまたは 16 進数のカラー値のいずれかです。 使用できる色のキーワードの詳細については、<xref:System.Windows.Media> 名前空間の <xref:System.Windows.Media.Colors> クラスのプロパティに関する説明を参照してください。  
  
```xaml
<Canvas Background="LightGray">   
   <Ellipse  
      Canvas.Top="50"  
      Canvas.Left="50"  
      Fill="#FFFFFF00"  
      Height="75"  
      Width="75"  
      StrokeThickness="5"  
      Stroke="#FF0000FF"/>  
</Canvas>  
```  
  
 次の図は、表示される <xref:System.Windows.Shapes.Ellipse>を示しています。  
  
 ![楕円](./media/shape-ovw-ellipsefill.PNG "shape_ovw_ellipsefill")  
  
 または、プロパティ要素構文を使用して、<xref:System.Windows.Media.SolidColorBrush> オブジェクトを明示的に作成し、純色で図形を塗りつぶすこともできます。  
  
```xaml
<!-- This polygon shape uses pre-defined color values for its Stroke and  
     Fill properties.   
     The SolidColorBrush's Opacity property affects the fill color in   
     this case by making it slightly transparent (opacity of 0.4) so   
     that it blends with any underlying color. -->  
  
<Polygon  
    Points="300,200 400,125 400,275 300,200"  
    Stroke="Purple"   
    StrokeThickness="2">  
    <Polygon.Fill>  
       <SolidColorBrush Color="Blue" Opacity="0.4"/>  
    </Polygon.Fill>  
</Polygon>  
```  
  
 レンダリングされた図形を次の図に示します。  
  
 ![System.windows.media.solidcolorbrush> の図](./media/shape-ovw-solidcolorbrush.PNG "shape_ovw_solidcolorbrush")  
  
 図形のストロークまたは塗りつぶしをグラデーション、イメージ、パターンなどで塗りつぶすこともできます。 詳細については、「[純色とグラデーションによる塗りつぶしの概要](painting-with-solid-colors-and-gradients-overview.md)」を参照してください。  
  
<a name="stretchableshapessection"></a>   
## <a name="stretchable-shapes"></a>伸縮可能な図形  
 <xref:System.Windows.Shapes.Line>、<xref:System.Windows.Shapes.Path>、<xref:System.Windows.Shapes.Polygon>、<xref:System.Windows.Shapes.Polyline>、および <xref:System.Windows.Shapes.Rectangle> の各クラスには、すべて <xref:System.Windows.Shapes.Shape.Stretch%2A> プロパティがあります。 このプロパティは、<xref:System.Windows.Shapes.Shape> オブジェクトのコンテンツ (描画される図形) が <xref:System.Windows.Shapes.Shape> オブジェクトのレイアウト空間を埋めるように拡大される方法を決定します。 <xref:System.Windows.Shapes.Shape> オブジェクトのレイアウト空間は、<xref:System.Windows.Shapes.Shape> がレイアウトシステムによって割り当てられる領域の量です。これは、明示的な <xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> の設定、またはその <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> と <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> の設定によって行われるためです。 Windows Presentation Foundation のレイアウトの詳細については、「[レイアウト](../advanced/layout.md)の概要」を参照してください。  
  
 プロパティには、次のいずれかの値を指定します。  
  
- <xref:System.Windows.Media.Stretch.None>: <xref:System.Windows.Shapes.Shape> オブジェクトのコンテンツは拡張されていません。  
  
- <xref:System.Windows.Media.Stretch.Fill>: <xref:System.Windows.Shapes.Shape> オブジェクトの内容が、レイアウトスペースを埋めるように拡大されます。  縦横比は維持されません。  
  
- <xref:System.Windows.Media.Stretch.Uniform>: 元の縦横比を維持したまま、<xref:System.Windows.Shapes.Shape> オブジェクトの内容は、レイアウト領域を塗りつぶすために可能な限り拡大されます。  
  
- <xref:System.Windows.Media.Stretch.UniformToFill>: 元の縦横比を維持したまま、<xref:System.Windows.Shapes.Shape> オブジェクトの内容が拡大して、レイアウト領域を完全に塗りつぶすことができます。  
  
 <xref:System.Windows.Shapes.Shape> オブジェクトのコンテンツが拡張されると、<xref:System.Windows.Shapes.Shape> オブジェクトのアウトラインが伸縮の後に描画されることに注意してください。  
  
 次の例では、(0, 0) から (0, 1) までの非常に小さな三角形を (1, 1) に描画するために、<xref:System.Windows.Shapes.Polygon> が使用されています。 <xref:System.Windows.Shapes.Polygon> オブジェクトの <xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> が100に設定され、その stretch プロパティが Fill に設定されています。 その結果、<xref:System.Windows.Shapes.Polygon> オブジェクトの内容 (三角形) が拡大され、より大きなスペースがいっぱいになります。  
  
```xaml
<Polygon  
  Points="0,0 0,1 1,1"  
  Fill="Blue"  
  Width="100"  
  Height="100"  
  Stretch="Fill"  
  Stroke="Black"  
  StrokeThickness="2" />  
```

```csharp
PointCollection myPointCollection = new PointCollection();  
myPointCollection.Add(new Point(0,0));  
myPointCollection.Add(new Point(0,1));  
myPointCollection.Add(new Point(1,1));  
  
Polygon myPolygon = new Polygon();  
myPolygon.Points = myPointCollection;  
myPolygon.Fill = Brushes.Blue;  
myPolygon.Width = 100;  
myPolygon.Height = 100;  
myPolygon.Stretch = Stretch.Fill;  
myPolygon.Stroke = Brushes.Black;  
myPolygon.StrokeThickness = 2;  
```

<a name="transforms"></a>   
## <a name="transforming-shapes"></a>図形の変換  
 <xref:System.Windows.Media.Transform> クラスは、2次元平面内の図形を変換するための手段を提供します。  さまざまな種類の変換には、回転 (<xref:System.Windows.Media.RotateTransform>)、スケール (<xref:System.Windows.Media.ScaleTransform>)、傾斜 (<xref:System.Windows.Media.SkewTransform>)、および平行移動 (<xref:System.Windows.Media.TranslateTransform>) があります。  
  
 図形に適用される一般的な変換は回転です。  図形を回転させるには、<xref:System.Windows.Media.RotateTransform> を作成し、その <xref:System.Windows.Media.RotateTransform.Angle%2A>を指定します。 45の <xref:System.Windows.Media.RotateTransform.Angle%2A> により、要素は時計回りに45度回転します。90の角度では、要素が時計回りに90度回転します。などなど。 要素を回転させるポイントを制御する場合は、<xref:System.Windows.Media.RotateTransform.CenterX%2A> プロパティと <xref:System.Windows.Media.RotateTransform.CenterY%2A> プロパティを設定します。 これらのプロパティ値は、変換する要素の座標空間で表します。 <xref:System.Windows.Media.RotateTransform.CenterX%2A> と <xref:System.Windows.Media.RotateTransform.CenterY%2A> の既定値は0です。 最後に、要素に <xref:System.Windows.Media.RotateTransform> を適用します。 変換がレイアウトに影響しないようにするには、図形の <xref:System.Windows.UIElement.RenderTransform%2A> プロパティを設定します。  
  
 次の例では、図形の左上隅 (0, 0) について図形を45度回転させるために、<xref:System.Windows.Media.RotateTransform> が使用されています。  
  
 [!code-xaml[transformssample#14](~/samples/snippets/csharp/VS_Snippets_Wpf/transformsSample/CS/RotateTransformExample.xaml#14)]  
  
 次の例では、別の図形を 45 度回転していますが、この場合は (25,50) が回転の中心です。  
  
 [!code-xaml[transformssample#15](~/samples/snippets/csharp/VS_Snippets_Wpf/transformsSample/CS/RotateTransformExample.xaml#15)]  
  
 上記 2 つの変換を適用すると、結果は次の図のようになります。  
  
 ![中心点が異なる 45 度の回転](./media/wcpsdk-graphicsmm-rotatetransform45degrees.gif "wcpsdk_graphicsmm_rotatetransform45degrees")  
  
 前の例では、単一の変換を各図形オブジェクトに適用しました。 図形 (またはその他の UI 要素) に複数の変換を適用するには、<xref:System.Windows.Media.TransformGroup>を使用します。  
  
## <a name="see-also"></a>参照

- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [純色およびグラデーションによる塗りつぶしの概要](painting-with-solid-colors-and-gradients-overview.md)
- [ジオメトリの概要](geometry-overview.md)
- [チュートリアル: 初めての WPF デスクトップ アプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)
- [アニメーションの概要](animation-overview.md)
