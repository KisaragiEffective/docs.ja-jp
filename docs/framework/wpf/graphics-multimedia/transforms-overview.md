---
title: 変換の概要
ms.date: 03/30/2017
helpviewer_keywords:
- transformations [WPF], about transformations
- classes [WPF], 2-D transform
- transform classes [WPF], 2-D
- 2-D transform classes
- FrameworkElement objects [WPF], rotating
- FrameworkElement objects [WPF], skewing
- FrameworkElement objects [WPF], translating
- Transforms [WPF], about Transforms
- FrameworkElement objects [WPF], scaling
ms.assetid: 8f153d5e-ed61-4aa5-a7cd-286f0c427a13
ms.openlocfilehash: ead1d114f773cba88e8b3e20f395019fbde3ee15
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458586"
---
# <a name="transforms-overview"></a>変換の概要
このトピックでは、2次元 <xref:System.Windows.Media.Transform> クラスを使用して、<xref:System.Windows.FrameworkElement> オブジェクトを回転、拡大縮小、移動 (平行移動) し、傾斜させる方法について説明します。  

<a name="whatIsATransformSection"></a>   
## <a name="what-is-a-transform"></a>変換とは  
 <xref:System.Windows.Media.Transform> は、ある座標空間から別の座標空間へのポインターをマップまたは変換する方法を定義します。 このマッピングは、<xref:System.Double> 値の3つの列を含む3つの行のコレクションである変換 <xref:System.Windows.Media.Matrix>によって記述されます。  
  
> [!NOTE]
> Windows Presentation Foundation (WPF) では、行メジャーのマトリックスを使用します。 ベクターは、列ベクターではなく行ベクターとして表されます。  
  
 次の表は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 行列の構造を示したものです。  
  
### <a name="a-2-d-transformation-matrix"></a>2-D 変換行列  
  
||||  
|-|-|-|  
|<xref:System.Windows.Media.Matrix.M11%2A><br /><br /> 既定値: 1.0|<xref:System.Windows.Media.Matrix.M12%2A><br /><br /> 既定値: 0.0|0.0|  
|<xref:System.Windows.Media.Matrix.M21%2A><br /><br /> 既定値: 0.0|<xref:System.Windows.Media.Matrix.M22%2A><br /><br /> 既定値: 1.0|0.0|  
|<xref:System.Windows.Media.Matrix.OffsetX%2A><br /><br /> 既定値: 0.0|<xref:System.Windows.Media.Matrix.OffsetY%2A><br /><br /> 既定値: 0.0|1|  
  
 行列の値を操作することで、オブジェクトを回転、拡大縮小、傾斜、移動 (平行移動) させることができます。 たとえば、3番目の行の最初の列 (<xref:System.Windows.Media.Matrix.OffsetX%2A> 値) の値を100に変更した場合、オブジェクト100単位を x 軸に沿って移動するために使用できます。 2 番目の行の 2 列目の値を 3 に変更すると、オブジェクトの高さを現在の 3 倍に拡張できます。 両方の値を変更した場合は、オブジェクトが x 軸に沿って 100 単位移動し、高さが 3 倍に拡張されます。 Windows Presentation Foundation (WPF) ではアフィン変換のみがサポートされているため、右側の列の値は常に0、0、1になります。  
  
 Windows Presentation Foundation (WPF) を使用すると、直接マトリックス値を操作できますが、基になるマトリックス構造がどのように構成されているかわからなくてもオブジェクトを変換できるいくつかの <xref:System.Windows.Media.Transform> クラスも提供されます。 たとえば、<xref:System.Windows.Media.ScaleTransform> クラスを使用すると、変換行列を操作するのではなく、<xref:System.Windows.Media.ScaleTransform.ScaleX%2A> プロパティと <xref:System.Windows.Media.ScaleTransform.ScaleY%2A> プロパティを設定することによって、オブジェクトのサイズを変更できます。 同様に、<xref:System.Windows.Media.RotateTransform> クラスを使用すると、<xref:System.Windows.Media.RotateTransform.Angle%2A> プロパティを設定するだけでオブジェクトを回転させることができます。  
  
<a name="transformClassesSection"></a>   
## <a name="transform-classes"></a>変換クラス  
 Windows Presentation Foundation (WPF) には、一般的な変換操作に対して次の2次元 <xref:System.Windows.Media.Transform> クラスが用意されています。  
  
|インスタンス|説明|例|図|  
|-----------|-----------------|-------------|------------------|  
|<xref:System.Windows.Media.RotateTransform>|指定した <xref:System.Windows.Media.RotateTransform.Angle%2A>で要素を回転します。|[オブジェクトを回転させる](how-to-rotate-an-object.md)|![図の回転](./media/graphicsmm-thumbnails-rotate.png "graphicsmm_thumbnails_rotate")|  
|<xref:System.Windows.Media.ScaleTransform>|指定された <xref:System.Windows.Media.ScaleTransform.ScaleX%2A> と <xref:System.Windows.Media.ScaleTransform.ScaleY%2A> 量で要素をスケールします。|[要素を拡大縮小する](how-to-scale-an-element.md)|![拡大/縮小の図](./media/graphicsmm-thumbnails-scale.png "graphicsmm_thumbnails_scale")|  
|<xref:System.Windows.Media.SkewTransform>|要素を、指定した <xref:System.Windows.Media.SkewTransform.AngleX%2A> と <xref:System.Windows.Media.SkewTransform.AngleY%2A> 量で傾斜させます。|[要素を傾斜させる](how-to-skew-an-element.md)|![傾斜の図](./media/graphicsmm-thumbnails-skew.png "graphicsmm_thumbnails_skew")|  
|<xref:System.Windows.Media.TranslateTransform>|指定された <xref:System.Windows.Media.TranslateTransform.X%2A> と <xref:System.Windows.Media.TranslateTransform.Y%2A> 量だけ要素を移動 (平行移動) します。|[要素を平行移動する](how-to-translate-an-element.md)|![平行移動の図](./media/graphicsmm-thumbnails-translate.png "graphicsmm_thumbnails_translate")|  
  
 より複雑な変換を作成するために、Windows Presentation Foundation (WPF) には次の2つのクラスが用意されています。  
  
|インスタンス|説明|例|  
|-----------|-----------------|-------------|  
|<xref:System.Windows.Media.TransformGroup>|複数の <xref:System.Windows.Media.TransformGroup> オブジェクトを1つの <xref:System.Windows.Media.Transform> にグループ化して、変換プロパティに適用することができます。|[オブジェクトに複数の変換を適用する](how-to-apply-multiple-transforms-to-an-object.md)|  
|<xref:System.Windows.Media.MatrixTransform>|他の <xref:System.Windows.Media.Transform> クラスによって提供されないカスタム変換を作成します。 <xref:System.Windows.Media.MatrixTransform>を使用する場合は、マトリックスを直接操作します。|[MatrixTransform を使用してカスタム変換を作成する](how-to-use-a-matrixtransform-to-create-custom-transforms.md)|  
  
 Windows Presentation Foundation (WPF) には、3-d 変換も用意されています。 詳細については、<xref:System.Windows.Media.Media3D.Transform3D> クラスを参照してください。  
  
<a name="transformationproperties"></a>   
## <a name="common-transformation-properties"></a>一般的な変換プロパティ  
 オブジェクトを変換する方法の1つとして、適切な <xref:System.Windows.Media.Transform> 型を宣言し、オブジェクトの変換プロパティに適用する方法があります。 オブジェクトの型ごとに、異なる型の変換プロパティがあります。 次の表に、一般的に使用される Windows Presentation Foundation (WPF) 型とその変換プロパティの一覧を示します。  
  
|[種類]|変換プロパティ|  
|----------|-------------------------------|  
|<xref:System.Windows.Media.Brush>|<xref:System.Windows.Media.Brush.Transform%2A>、 <xref:System.Windows.Media.Brush.RelativeTransform%2A>|  
|<xref:System.Windows.Media.ContainerVisual>|<xref:System.Windows.Media.ContainerVisual.Transform%2A>|  
|<xref:System.Windows.Media.DrawingGroup>|<xref:System.Windows.Media.DrawingGroup.Transform%2A>|  
|<xref:System.Windows.FrameworkElement>|<xref:System.Windows.UIElement.RenderTransform%2A>、 <xref:System.Windows.FrameworkElement.LayoutTransform%2A>|  
|<xref:System.Windows.Media.Geometry>|<xref:System.Windows.Media.Geometry.Transform%2A>|  
|<xref:System.Windows.Media.TextEffect>|<xref:System.Windows.Media.TextEffect.Transform%2A>|  
|<xref:System.Windows.UIElement>|<xref:System.Windows.UIElement.RenderTransform%2A>|  
  
<a name="transformcenter"></a>   
## <a name="transformations-and-coordinate-systems"></a>変換と座標系  
 オブジェクトを変換する場合は、単にオブジェクトを変換するのではなく、そのオブジェクトが存在する座標空間を変換することになります。 既定では、ターゲット オブジェクトの座標系の原点が変換の中心になります: (0, 0)。 唯一の例外は <xref:System.Windows.Media.TranslateTransform>です。<xref:System.Windows.Media.TranslateTransform> には、中央にある場所に関係なく、変換効果が同じであるため、設定する中心プロパティはありません。  
  
 次の例では、<xref:System.Windows.Media.RotateTransform> を使用して、<xref:System.Windows.FrameworkElement>の型 <xref:System.Windows.Shapes.Rectangle> 要素を、既定の中心 (0, 0) について45度ずつ回転させます。 次の図は、回転の結果を示したものです。  
  
 ![&#40;0,0&#41; を軸に 45 度回転した FrameworkElement](./media/graphicsmm-fe-rotated-about-upperleft-corner.png "graphicsmm_FE_rotated_about_upperleft_corner")  
ポイント (0, 0) を軸に 45 度回転した四角形要素  
  
 [!code-xaml[Transforms_snip#TransformsFERotatedAboutTopLeft](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/CoordinateSystemExample.xaml#transformsferotatedabouttopleft)]  
  
 既定では、要素は左上隅 (0, 0) を軸に回転します。 <xref:System.Windows.Media.RotateTransform>、<xref:System.Windows.Media.ScaleTransform>、および <xref:System.Windows.Media.SkewTransform> クラスは、変換が適用されるポイントを指定できるようにする System.windows.media.rotatetransform.centerx プロパティとセンター y プロパティを提供します。  
  
 次の例では、<xref:System.Windows.Media.RotateTransform> を使用して、<xref:System.Windows.Shapes.Rectangle> 要素を45度回転させます。ただし今回は、<xref:System.Windows.Media.RotateTransform> が (25, 25) の中心になるように、<xref:System.Windows.Media.RotateTransform.CenterX%2A> と <xref:System.Windows.Media.RotateTransform.CenterY%2A> のプロパティが設定されています。 次の図は、回転の結果を示したものです。  
  
 ![&#40;25, 25&#41; を軸に 45 度回転した Geometry](./media/graphicsmm-fe-rotated-about-center.png "graphicsmm_FE_rotated_about_center")  
ポイント (25, 25) を軸に 45 度回転した四角形要素  
  
 [!code-xaml[Transforms_snip#TransformsFERotatedAboutCenter](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/CoordinateSystemExample.xaml#transformsferotatedaboutcenter)]  
  
<a name="layoutTransformsAndRenderTransformsSection"></a>   
## <a name="transforming-a-frameworkelement"></a>FrameworkElement の変換  
 変換を <xref:System.Windows.FrameworkElement>に適用するには、<xref:System.Windows.Media.Transform> を作成し、<xref:System.Windows.FrameworkElement> クラスによって提供される2つのプロパティのいずれかにそれを適用します。  
  
- <xref:System.Windows.FrameworkElement.LayoutTransform%2A> –レイアウトパスの前に適用される変換です。 変換が適用されると、レイアウト システムは変換後のサイズと要素の位置を処理します。  
  
- <xref:System.Windows.UIElement.RenderTransform%2A> –要素の外観を変更する変換ですが、レイアウトパスの完了後に適用されます。 <xref:System.Windows.FrameworkElement.LayoutTransform%2A> プロパティの代わりに <xref:System.Windows.UIElement.RenderTransform%2A> プロパティを使用すると、パフォーマンス上の利点を得ることができます。  
  
 どちらのプロパティを使用すればよいのでしょうか。 特に、アニメーション化された <xref:System.Windows.Media.Transform> オブジェクトを使用する場合は、パフォーマンス上の利点があるため、可能な限り <xref:System.Windows.UIElement.RenderTransform%2A> プロパティを使用してください。 拡大縮小、回転、または傾斜を行うときは、<xref:System.Windows.FrameworkElement.LayoutTransform%2A> プロパティを使用します。要素の親は、要素の変換後のサイズに合わせて調整する必要があります。 <xref:System.Windows.FrameworkElement.LayoutTransform%2A> プロパティと共に使用すると、<xref:System.Windows.Media.TranslateTransform> オブジェクトは要素に影響しないように見えます。 これは、レイアウト システムがその処理の一環として、変換後の要素を元の位置に返すためです。  
  
 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] でのレイアウトに関する追加情報については、[レイアウト](../advanced/layout.md)の概要をご覧ください。  
  
<a name="exampleRotateAnElement45degSection"></a>   
## <a name="example-rotate-a-frameworkelement-45-degrees"></a>例: FrameworkElement を 45 度回転させる  
 次の例では、<xref:System.Windows.Media.RotateTransform> を使用して、ボタンを時計回りに45度回転させます。 このボタンは、他の2つのボタンがある <xref:System.Windows.Controls.StackPanel> に含まれています。  
  
 既定では、<xref:System.Windows.Media.RotateTransform> はポイント (0, 0) を中心に回転します。 この例では中心の値を指定していないので、ボタンは左上隅のポイント (0, 0) を軸に回転します。 <xref:System.Windows.Media.RotateTransform> は <xref:System.Windows.UIElement.RenderTransform%2A> プロパティに適用されます。 次の図は、変換の結果を示したものです。  
  
 ![RenderTransform を使用して変換されたボタン](./media/graphicsmm-rendertransformwithdefaultcenter.png "graphicsmm_RenderTransformWithDefaultCenter")  
左上隅を軸とした、時計回り 45 度の回転  
  
 [!code-xaml[Transforms_snip#GraphicsMMRotateButtonExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/ButtonRotateTransformExample.xaml#graphicsmmrotatebuttonexample1)]  
  
 次の例では、<xref:System.Windows.Media.RotateTransform> を使用してボタンを時計回りに45に回転させますが、ボタンの <xref:System.Windows.UIElement.RenderTransformOrigin%2A> を (0.5、0.5) に設定することもできます。 <xref:System.Windows.UIElement.RenderTransformOrigin%2A> プロパティの値は、ボタンのサイズに対して相対的です。 その結果、回転は左上隅ではなく、ボタンの中心に適用されています。 次の図は、変換の結果を示したものです。  
  
 ![中心を軸に変換されたボタン](./media/graphicsmm-rendertransformrelativecenter.png "graphicsmm_RenderTransformRelativeCenter")  
中心を軸とした、時計回り 45 度の回転  
  
 [!code-xaml[Transforms_snip#GraphicsMMRotateButtonExample2](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/ButtonRotateTransformExample.xaml#graphicsmmrotatebuttonexample2)]  
  
 次の例では、<xref:System.Windows.UIElement.RenderTransform%2A> プロパティの代わりに <xref:System.Windows.FrameworkElement.LayoutTransform%2A> プロパティを使用して、ボタンを回転させます。  そのため、変換がボタンのレイアウトに影響し、レイアウト システムによるフル パスがトリガーされています。 その結果、ボタンが回転された後、位置が変更されています。これは、サイズが変更されたためです。 次の図は、変換の結果を示したものです。  
  
 ![LayoutTransform を使用して変換されたボタン](./media/graphicsmm-layouttransform.png "graphicsmm_LayoutTransform")  
LayoutTransform を使用したボタンの回転  
  
 [!code-xaml[Transforms_snip#GraphicsMMRotateButtonExample3](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/ButtonRotateTransformExample.xaml#graphicsmmrotatebuttonexample3)]  
  
<a name="animate_transforms"></a>   
## <a name="animating-transformations"></a>変換のアニメーション化  
 これらは <xref:System.Windows.Media.Animation.Animatable> クラスから継承するため、<xref:System.Windows.Media.Transform> クラスをアニメーション化できます。 <xref:System.Windows.Media.Transform>をアニメーション化するには、アニメーション化するプロパティに互換性のある型のアニメーションを適用します。  
  
 次の例では、<xref:System.Windows.Media.Animation.Storyboard> と <xref:System.Windows.Media.Animation.DoubleAnimation> を <xref:System.Windows.Media.RotateTransform> と共に使用して、クリックされたときに <xref:System.Windows.Controls.Button> のスピンを設定します。  
  
 [!code-xaml[Transforms_snip#GraphicsMMAnimatedRotateButtonExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Transforms_snip/CS/ButtonAnimatedRotateTransformExample.xaml#graphicsmmanimatedrotatebuttonexamplewholepage)]  
  
 完全なサンプルについては、「[2-D 変換のサンプル](https://go.microsoft.com/fwlink/?LinkID=158252)」をご覧ください。 アニメーション化について詳しくは、「[アニメーションの概要](animation-overview.md)」をご覧ください。  
  
<a name="freezable_features"></a>   
## <a name="freezable-features"></a>Freezable 機能  
 <xref:System.Windows.Media.Transform> クラスは <xref:System.Windows.Freezable> クラスから継承するため、いくつかの特殊な機能を提供します。 <xref:System.Windows.Media.Transform> オブジェクトを[リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)として宣言したり、複数のオブジェクト間で共有したり、読み取り専用にして、パフォーマンスを向上させたり、複製したり、スレッドセーフにしたりすることができます。 <xref:System.Windows.Freezable> オブジェクトによって提供されるさまざまな機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Transform>
- <xref:System.Windows.Media.Matrix>
- [方法トピック](transformations-how-to-topics.md)
- [2-D 変換のサンプル](https://go.microsoft.com/fwlink/?LinkID=158252)
