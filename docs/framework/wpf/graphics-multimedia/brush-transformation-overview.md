---
title: ブラシの変換の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- brushes [WPF], transformation properties
- properties [WPF], transformation
- transformation properties of brushes [WPF]
ms.assetid: 8b9bfc09-12fd-4cd5-b445-99949f27bc39
ms.openlocfilehash: 1d3a56014e0975f3616b7f90021b4290ced5daab
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453131"
---
# <a name="brush-transformation-overview"></a>ブラシの変換の概要
Brush クラスには、<xref:System.Windows.Media.Brush.Transform%2A> と <xref:System.Windows.Media.Brush.RelativeTransform%2A>の2つの変換プロパティが用意されています。 これらのプロパティを使うと、ブラシの内容を回転、拡大縮小、傾斜、移動できます。 このトピックでは、これら 2 つのプロパティの違いについて説明し、それらの使用例を示します。  
  
<a name="prerequisites"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックを理解するには、変換するブラシの機能を理解している必要があります。 <xref:System.Windows.Media.LinearGradientBrush> と <xref:System.Windows.Media.RadialGradientBrush>については、「[純色とグラデーションによる塗りつぶしの概要](painting-with-solid-colors-and-gradients-overview.md)」を参照してください。 <xref:System.Windows.Media.ImageBrush>、<xref:System.Windows.Media.DrawingBrush>、<xref:System.Windows.Media.VisualBrush>については、「[イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)」を参照してください。 また、「[変換の概要](transforms-overview.md)」で説明されている 2D 変換についても理解しておく必要があります。  
  
<a name="transformversusrelativetransform"></a>   
## <a name="differences-between-the-transform-and-relativetransform-properties"></a>Transform プロパティと RelativeTransform プロパティの違い  
 ブラシの <xref:System.Windows.Media.Brush.Transform%2A> プロパティに変換を適用する場合、その中心に関するブラシの内容を変換する場合は、塗りつぶされた領域のサイズを把握しておく必要があります。 描画領域の幅が 200 デバイス独立ピクセル、高さが 150 ピクセルであるものとします。  <xref:System.Windows.Media.RotateTransform> を使用して、その中心についてブラシの出力45度を回転させた場合は、<xref:System.Windows.Media.RotateTransform> <xref:System.Windows.Media.RotateTransform.CenterX%2A> 100 と75の <xref:System.Windows.Media.RotateTransform.CenterY%2A> を指定します。  
  
 ブラシの <xref:System.Windows.Media.Brush.RelativeTransform%2A> プロパティに変換を適用すると、その変換がブラシに適用されてから、塗りつぶされた領域に変換されます。 次の一覧では、ブラシの内容が処理および変換される順序を説明します。  
  
1. ブラシの内容を処理します。 <xref:System.Windows.Media.GradientBrush>の場合、これはグラデーション領域を決定することを意味します。 <xref:System.Windows.Media.TileBrush>の場合、<xref:System.Windows.Media.TileBrush.Viewbox%2A> は <xref:System.Windows.Media.TileBrush.Viewport%2A>にマップされます。 これがブラシの出力になります。  
  
2. ブラシの出力を 1 x 1 の変換四角形に投影します。  
  
3. ブラシの <xref:System.Windows.Media.Brush.RelativeTransform%2A>を適用します (ある場合)。  
  
4. 変換された出力を描画領域に投影します。  
  
5. ブラシの <xref:System.Windows.Media.Transform>を適用します (ある場合)。  
  
 ブラシの出力が 1 x 1 の四角形にマップされている間、<xref:System.Windows.Media.Brush.RelativeTransform%2A> が適用されるため、変換の中心とオフセットの値は相対値として表示されます。 たとえば、<xref:System.Windows.Media.RotateTransform> を使用して、その中心についてブラシの出力45度を回転させた場合、<xref:System.Windows.Media.RotateTransform> <xref:System.Windows.Media.RotateTransform.CenterX%2A> 0.5 と0.5 の <xref:System.Windows.Media.RotateTransform.CenterY%2A> になります。  
  
 次の図は、<xref:System.Windows.Media.Brush.RelativeTransform%2A> と <xref:System.Windows.Media.Brush.Transform%2A> のプロパティを使用して、45度回転した複数のブラシの出力を示しています。  
  
 ![RelativeTransform プロパティと Transform プロパティ](./media/graphicsmm-brushrelativetransform-transform-small.png "graphicsmm_brushrelativetransform_transform_small")  
  
<a name="relativetransformandtilebrush"></a>   
## <a name="using-relativetransform-with-a-tilebrush"></a>TileBrush での RelativeTransform の使用  
 タイルブラシは他のブラシよりも複雑であるため、<xref:System.Windows.Media.Brush.RelativeTransform%2A> を1つに適用すると、予期しない結果が生じる可能性があります。 たとえば、次のようなイメージについて考えます。  
  
 ![ソースイメージ](./media/graphicsmm-reltransform-1-original-image.jpg "graphicsmm_reltransform_1_original_image")  
  
 次の例では、<xref:System.Windows.Media.ImageBrush> を使用して、前の画像で四角形の領域を塗りつぶします。 このメソッドは、<xref:System.Windows.Media.ImageBrush> オブジェクトの <xref:System.Windows.Media.Brush.RelativeTransform%2A> プロパティに <xref:System.Windows.Media.RotateTransform> を適用し、その <xref:System.Windows.Media.TileBrush.Stretch%2A> プロパティを <xref:System.Windows.Media.Stretch.UniformToFill>に設定します。これにより、四角形を完全に塗りつぶすために画像の縦横比を維持する必要があります。  
  
 [!code-xaml[BrushOverviewExamples_snip#GraphicsMMRelativeTransformExample2Inline](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushOverviewExamples_snip/XAML/RelativeTransformIllustration.xaml#graphicsmmrelativetransformexample2inline)]  
  
 この例の結果は、次のようになります。  
  
 ![変換された出力](./media/graphicsmm-reltransform-6-output.png "graphicsmm_reltransform_6_output")  
  
 ブラシの <xref:System.Windows.Media.TileBrush.Stretch%2A> が <xref:System.Windows.Media.Stretch.UniformToFill>に設定されていても、画像がゆがんでいることに注意してください。 これは、ブラシの <xref:System.Windows.Media.TileBrush.Viewbox%2A> が <xref:System.Windows.Media.TileBrush.Viewport%2A>にマップされた後に、相対変換が適用されるためです。 次の一覧では、処理の各ステップについて説明します。  
  
1. ブラシの <xref:System.Windows.Media.TileBrush.Stretch%2A> 設定を使用して、ブラシのコンテンツ (<xref:System.Windows.Media.TileBrush.Viewbox%2A>) を基本タイル (<xref:System.Windows.Media.TileBrush.Viewport%2A>) に投影します。  
  
     ![ビューポートに合うように Viewbox を拡大する](./media/graphicsmm-reltransform-2-viewbox-to-viewport.png "graphicsmm_reltransform_2_viewbox_to_viewport")  
  
2. 基本タイルを 1 x 1 の変換四角形に投影します。  
  
     ![ビューポートを変換四角形にマップする](./media/graphicsmm-reltransform-3-output-to-transform.png "graphicsmm_reltransform_3_output_to_transform")  
  
3. <xref:System.Windows.Media.RotateTransform>を適用します。  
  
     ![相対変換を適用する](./media/graphicsmm-reltransform-4-transform-rotate.png "graphicsmm_reltransform_4_transform_rotate")  
  
4. 変換された基本タイルを描画領域に投影します。  
  
     ![変換されたブラシを出力領域に投影する](./media/graphicsmm-reltransform-5-transform-to-output.png "graphicsmm_reltransform_5_transform_to_output")  
  
<a name="rotateexample"></a>   
## <a name="example-rotate-an-imagebrush-45-degrees"></a>例: ImageBrush を 45 度回転する  
 次の例では、<xref:System.Windows.Media.ImageBrush>の <xref:System.Windows.Media.Brush.RelativeTransform%2A> プロパティに <xref:System.Windows.Media.RotateTransform> を適用します。 <xref:System.Windows.Media.RotateTransform> オブジェクトの <xref:System.Windows.Media.RotateTransform.CenterX%2A> プロパティと <xref:System.Windows.Media.RotateTransform.CenterY%2A> プロパティは両方とも、コンテンツの中心点の相対座標である0.5 に設定されます。 その結果、ブラシの内容は中心の周りに回転されます。  
  
 [!code-csharp[BrushesIntroduction_snip#ImageBrushRelativeTransformExample](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushesIntroduction_snip/CSharp/BrushTransformExample.cs#imagebrushrelativetransformexample)]
 [!code-vb[BrushesIntroduction_snip#ImageBrushRelativeTransformExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BrushesIntroduction_snip/visualbasic/brushtransformexample.vb#imagebrushrelativetransformexample)]
 [!code-xaml[BrushesIntroduction_snip#ImageBrushRelativeTransformExample](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushesIntroduction_snip/XAML/BrushTransformExample.xaml#imagebrushrelativetransformexample)]  
  
 次の例では、<xref:System.Windows.Media.ImageBrush>に <xref:System.Windows.Media.RotateTransform> も適用しますが、<xref:System.Windows.Media.Brush.RelativeTransform%2A> プロパティの代わりに <xref:System.Windows.Media.Brush.Transform%2A> プロパティを使用します。 中心のブラシを回転させるには、<xref:System.Windows.Media.RotateTransform> オブジェクトの <xref:System.Windows.Media.RotateTransform.CenterX%2A> と <xref:System.Windows.Media.RotateTransform.CenterY%2A> を絶対座標に設定する必要があります。 ブラシによって描画される四角形は 175 x 90 ピクセルなので、その中心点は (87.5, 45) です。  
  
 [!code-csharp[BrushesIntroduction_snip#ImageBrushTransformExample](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushesIntroduction_snip/CSharp/BrushTransformExample.cs#imagebrushtransformexample)]
 [!code-vb[BrushesIntroduction_snip#ImageBrushTransformExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BrushesIntroduction_snip/visualbasic/brushtransformexample.vb#imagebrushtransformexample)]
 [!code-xaml[BrushesIntroduction_snip#ImageBrushTransformExample](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushesIntroduction_snip/XAML/BrushTransformExample.xaml#imagebrushtransformexample)]  
  
 次の図は、変換を含まないブラシを示しています。変換が <xref:System.Windows.Media.Brush.RelativeTransform%2A> プロパティに適用され、変換が <xref:System.Windows.Media.Brush.Transform%2A> プロパティに適用されています。  
  
 ![Brush RelativeTransform と Transform の設定](./media/wcpsdk-graphicsmm-transformandrelativetransform.png "wcpsdk_graphicsmm_transformandrelativetransform")  
  
 この例は、さらに大きなサンプルの一部です。 完全なサンプルについては、「[ブラシのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes)」をご覧ください。 ブラシについて詳しくは、「[WPF のブラシの概要](wpf-brushes-overview.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.Brush.Transform%2A>
- <xref:System.Windows.Media.Brush.RelativeTransform%2A>
- <xref:System.Windows.Media.Transform>
- <xref:System.Windows.Media.Brush>
- [純色およびグラデーションによる塗りつぶしの概要](painting-with-solid-colors-and-gradients-overview.md)
- [イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)
- [変換の概要](transforms-overview.md)
