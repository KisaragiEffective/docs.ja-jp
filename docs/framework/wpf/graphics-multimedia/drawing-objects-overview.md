---
title: Drawing オブジェクトの概要
ms.date: 03/30/2017
helpviewer_keywords:
- ImageDrawing objects [WPF]
- GlyphRunDrawing objects [WPF]
- GeometryDrawing objects [WPF]
- drawings [WPF], about drawings
- Drawing objects [WPF]
- DrawingGroup objects [WPF]
ms.assetid: 9b5ce5c0-e204-4320-a7a8-0b2210d62f88
ms.openlocfilehash: d4527c331308ff6cd517705212e09428216d2378
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459736"
---
# <a name="drawing-objects-overview"></a>Drawing オブジェクトの概要
このトピックでは <xref:System.Windows.Media.Drawing> オブジェクトを紹介し、それらを使用して図形、ビットマップ、テキスト、およびメディアを効率的に描画する方法について説明します。 クリップアートを作成したり、<xref:System.Windows.Media.DrawingBrush>で塗りつぶしたり、<xref:System.Windows.Media.Visual> オブジェクトを使用したりするときに <xref:System.Windows.Media.Drawing> オブジェクトを使用します。  

<a name="whatisadrawingsection"></a>   
## <a name="what-is-a-drawing-object"></a>Drawing オブジェクトとは  
 <xref:System.Windows.Media.Drawing> オブジェクトは、形状、ビットマップ、ビデオ、テキスト行など、表示される内容を記述します。 さまざまな種類の描画で、さまざまな種類のコンテンツを記述します。 次の一覧に、さまざまな種類の描画オブジェクトを示します。  
  
- <xref:System.Windows.Media.GeometryDrawing> –図形を描画します。  
  
- <xref:System.Windows.Media.ImageDrawing> –イメージを描画します。  
  
- <xref:System.Windows.Media.GlyphRunDrawing> –テキストを描画します。  
  
- <xref:System.Windows.Media.VideoDrawing> –オーディオファイルまたはビデオファイルを再生します。  
  
- <xref:System.Windows.Media.DrawingGroup> –他の描画を描画します。 他の描画を 1 つの複合描画に結合するには、描画グループを使用します。  
  
 <xref:System.Windows.Media.Drawing> オブジェクトはさまざまです。<xref:System.Windows.Media.Drawing> オブジェクトを使用するには、さまざまな方法があります。  
  
- <xref:System.Windows.Media.DrawingImage> と <xref:System.Windows.Controls.Image> コントロールを使用して、イメージとして表示できます。  
  
- これを <xref:System.Windows.Media.DrawingBrush> と共に使用して、<xref:System.Windows.Controls.Page>の <xref:System.Windows.Controls.Page.Background%2A> などのオブジェクトを塗りつぶすことができます。  
  
- これを使用して、<xref:System.Windows.Media.DrawingVisual>の外観を記述できます。  
  
- これを使用して、<xref:System.Windows.Media.Visual>の内容を列挙できます。  
  
 WPF には、図形、ビットマップ、テキスト、メディアを描画できる他の型のオブジェクトが用意されています。 たとえば、<xref:System.Windows.Shapes.Shape> オブジェクトを使用して図形を描画することもできます。また、<xref:System.Windows.Controls.MediaElement> コントロールは、アプリケーションにビデオを追加する別の方法を提供します。 <xref:System.Windows.Media.Drawing> オブジェクトを使用する場合はどうすればよいでしょうか。 フレームワークレベルの機能を犠牲にして、パフォーマンスを向上させることができる場合、または <xref:System.Windows.Freezable> 機能が必要な場合。 <xref:System.Windows.Media.Drawing> オブジェクトは[レイアウト](../advanced/layout.md)、入力、およびフォーカスをサポートしていないため、<xref:System.Windows.Media.Visual> オブジェクトを使用した背景、クリップアート、低レベルの描画に最適なパフォーマンス上の利点が得られます。  
  
 オブジェクトは <xref:System.Windows.Freezable> 型であるため、<xref:System.Windows.Media.Drawing> オブジェクトには、[リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)として宣言したり、複数のオブジェクト間で共有したり、読み取り専用にしてパフォーマンスを向上させたり、複製したり、作成したりできる、いくつかの特殊な機能があります。スレッドセーフ。 <xref:System.Windows.Freezable> オブジェクトによって提供されるさまざまな機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。  
  
<a name="drawinggeometriessection"></a>   
## <a name="draw-a-shape"></a>図形の描画  
 図形を描画するには、<xref:System.Windows.Media.GeometryDrawing>を使用します。 ジオメトリ描画の <xref:System.Windows.Media.GeometryDrawing.Geometry%2A> プロパティは描画する図形を記述し、その <xref:System.Windows.Media.GeometryDrawing.Brush%2A> プロパティは図形の内部を描画する方法を記述し、その <xref:System.Windows.Media.GeometryDrawing.Pen%2A> プロパティはその輪郭を描画する方法を記述します。  
  
 次の例では、<xref:System.Windows.Media.GeometryDrawing> を使用して図形を描画します。 図形は、<xref:System.Windows.Media.GeometryGroup> と2つの <xref:System.Windows.Media.EllipseGeometry> オブジェクトによって記述されます。 図形の内部は <xref:System.Windows.Media.LinearGradientBrush> で描画され、その輪郭は <xref:System.Windows.Media.Brushes.Black%2A> <xref:System.Windows.Media.Pen>で描画されます。  
  
 この例では、次の <xref:System.Windows.Media.GeometryDrawing>を作成します。  
  
 ![2 つの楕円の GeometryDrawing](./media/graphicsmm-geodraw.jpg "graphicsmm_geodraw")  
GeometryDrawing  
  
 [!code-csharp[DrawingMiscSnippets_snip#GeometryDrawingExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/GeometryDrawingExample.cs#geometrydrawingexampleinline)]
 [!code-xaml[DrawingMiscSnippets_snip#GeometryDrawingExampleInline](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/GeometryDrawingExample.xaml#geometrydrawingexampleinline)]  
  
 完全な例については、「[GeometryDrawing を作成する](how-to-create-a-geometrydrawing.md)」をご覧ください。  
  
 <xref:System.Windows.Media.PathGeometry> などの他の <xref:System.Windows.Media.Geometry> クラスを使用すると、曲線と円弧を作成することで、より複雑な図形を作成できます。 <xref:System.Windows.Media.Geometry> オブジェクトの詳細については、「 [Geometry の概要](geometry-overview.md)」を参照してください。  
  
 <xref:System.Windows.Media.Drawing> オブジェクトを使用しない図形を描画するその他の方法の詳細については、「 [WPF での図形と基本描画の概要](shapes-and-basic-drawing-in-wpf-overview.md)」を参照してください。  
  
<a name="drawingimagessection"></a>   
## <a name="draw-an-image"></a>イメージの描画  
 イメージを描画するには、<xref:System.Windows.Media.ImageDrawing>を使用します。 <xref:System.Windows.Media.ImageDrawing> オブジェクトの <xref:System.Windows.Media.ImageDrawing.ImageSource%2A> プロパティは描画するイメージを記述し、その <xref:System.Windows.Media.ImageDrawing.Rect%2A> プロパティはイメージが描画される領域を定義します。  
  
 次の例では、(75,75) に存在する 100 x 100 ピクセルの四角形にイメージを描画します。 次の図は、例で作成した <xref:System.Windows.Media.ImageDrawing> を示しています。 <xref:System.Windows.Media.ImageDrawing>の境界を表示するために、灰色の枠線が追加されました。  
  
 ![&#40;75,75&#41; に描画される 100 × 100 の ImageDrawing](./media/graphicsmm-simple-imagedrawing-offset.png "graphicsmm_simple_imagedrawing_offset")  
100×100 の ImageDrawing  
  
 [!code-csharp[DrawingMiscSnippets_snip#ImageDrawing100by100Inline](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/ImageDrawingExample.cs#imagedrawing100by100inline)]
 [!code-xaml[DrawingMiscSnippets_snip#ImageDrawing100by100Inline](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/ImageDrawingExample.xaml#imagedrawing100by100inline)]  
  
 イメージについて詳しくは、「[イメージングの概要](imaging-overview.md)」をご覧ください。  
  
<a name="playmedia"></a>   
## <a name="play-media-code-only"></a>メディアの再生 (コードのみ)  
  
> [!NOTE]
> [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]で <xref:System.Windows.Media.VideoDrawing> を宣言することはできますが、コードを使用してのみ、そのメディアの読み込みと再生を行うことができます。 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]でビデオを再生するには、代わりに <xref:System.Windows.Controls.MediaElement> を使用します。  
  
 オーディオファイルまたはビデオファイルを再生するには、<xref:System.Windows.Media.VideoDrawing> と <xref:System.Windows.Media.MediaPlayer>を使用します。 メディアを読み込んで再生するには、2 つの方法があります。 1つ目は、<xref:System.Windows.Media.MediaPlayer> と <xref:System.Windows.Media.VideoDrawing> を単独で使用することです。2番目の方法は、<xref:System.Windows.Media.MediaPlayer> と <xref:System.Windows.Media.VideoDrawing>で使用する独自の <xref:System.Windows.Media.MediaTimeline> を作成することです。  
  
> [!NOTE]
> アプリケーションでメディアを配布するときは、イメージのようにプロジェクト リソースとしてメディア ファイルを使うことはできません。 プロジェクト ファイルで、メディアの種類を `Content` に設定し、`CopyToOutputDirectory` を `PreserveNewest` または `Always` に設定する必要があります。  
  
 独自の <xref:System.Windows.Media.MediaTimeline>を作成せずにメディアを再生するには、次の手順を実行します。  
  
1. <xref:System.Windows.Media.MediaPlayer> オブジェクトを作成します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline1)]  
  
2. <xref:System.Windows.Media.MediaPlayer.Open%2A> メソッドを使用してメディアファイルを読み込みます。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline2](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline2)]  
  
3. <xref:System.Windows.Media.VideoDrawing> を作成します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline3](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline3)]  
  
4. <xref:System.Windows.Media.VideoDrawing>の [<xref:System.Windows.Media.VideoDrawing.Rect%2A>] プロパティを設定して、メディアを描画するサイズと場所を指定します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline4](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline4)]  
  
5. <xref:System.Windows.Media.VideoDrawing> の <xref:System.Windows.Media.VideoDrawing.Player%2A> プロパティに、作成した <xref:System.Windows.Media.MediaPlayer> を設定します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline5](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline5)]  
  
6. <xref:System.Windows.Media.MediaPlayer> の <xref:System.Windows.Media.MediaPlayer.Play%2A> メソッドを使用して、メディアの再生を開始します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline6](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline6)]  
  
 次の例では、<xref:System.Windows.Media.VideoDrawing> と <xref:System.Windows.Media.MediaPlayer> を使用してビデオファイルを1回再生します。  
  
 [!code-csharp[DrawingMiscSnippets_snip#VideoDrawingExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#videodrawingexampleinline)]  
  
 メディアに対する追加のタイミング制御を取得するには、<xref:System.Windows.Media.MediaPlayer> オブジェクトと <xref:System.Windows.Media.VideoDrawing> オブジェクトで <xref:System.Windows.Media.MediaTimeline> を使用します。 <xref:System.Windows.Media.MediaTimeline> を使用すると、ビデオを繰り返し表示するかどうかを指定できます。 <xref:System.Windows.Media.VideoDrawing>で <xref:System.Windows.Media.MediaTimeline> を使用するには、次の手順を実行します。  
  
1. <xref:System.Windows.Media.MediaTimeline> を宣言し、そのタイミングの動作を設定します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#RepeatingVideoDrawingExampleInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#repeatingvideodrawingexampleinline1)]  
  
2. <xref:System.Windows.Media.MediaTimeline>から <xref:System.Windows.Media.MediaClock> を作成します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#RepeatingVideoDrawingExampleInline2](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#repeatingvideodrawingexampleinline2)]  
  
3. <xref:System.Windows.Media.MediaPlayer> を作成し、<xref:System.Windows.Media.MediaClock> を使用してその <xref:System.Windows.Media.MediaPlayer.Clock%2A> プロパティを設定します。  
  
     [!code-csharp[DrawingMiscSnippets_snip#RepeatingVideoDrawingExampleInline3](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#repeatingvideodrawingexampleinline3)]  
  
4. <xref:System.Windows.Media.VideoDrawing> を作成し、<xref:System.Windows.Media.MediaPlayer> を <xref:System.Windows.Media.VideoDrawing>の <xref:System.Windows.Media.VideoDrawing.Player%2A> プロパティに割り当てます。  
  
     [!code-csharp[DrawingMiscSnippets_snip#RepeatingVideoDrawingExampleInline4](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#repeatingvideodrawingexampleinline4)]  
  
 次の例では、<xref:System.Windows.Media.MediaTimeline> と <xref:System.Windows.Media.MediaPlayer> と <xref:System.Windows.Media.VideoDrawing> を使用して、ビデオを繰り返し再生します。  
  
 [!code-csharp[DrawingMiscSnippets_snip#RepeatingVideoDrawingExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/VideoDrawingExample.cs#repeatingvideodrawingexampleinline)]  
  
 <xref:System.Windows.Media.MediaTimeline>を使用する場合は、<xref:System.Windows.Media.MediaClock> の <xref:System.Windows.Media.Animation.Clock.Controller%2A> プロパティから返された対話型の <xref:System.Windows.Media.Animation.ClockController> を使用して、<xref:System.Windows.Media.MediaPlayer>の対話型メソッドではなく、メディアの再生を制御することに注意してください。  
  
<a name="drawtext"></a>   
## <a name="draw-text"></a>テキストの描画  
 テキストを描画するには、<xref:System.Windows.Media.GlyphRunDrawing> と <xref:System.Windows.Media.GlyphRun>を使用します。 次の例では、<xref:System.Windows.Media.GlyphRunDrawing> を使用して、"Hello World" というテキストを描画します。  
  
 [!code-csharp[DrawingMiscSnippets_snip#GlyphRunDrawingExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/GlyphRunDrawingExample.cs#glyphrundrawingexampleinline)]
 [!code-xaml[DrawingMiscSnippets_snip#GlyphRunDrawingExampleInline](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/GlyphRunExample.xaml#glyphrundrawingexampleinline)]  
  
 <xref:System.Windows.Media.GlyphRun> は、固定形式のドキュメントプレゼンテーションおよび印刷シナリオで使用するための低レベルのオブジェクトです。 画面にテキストを描画するためのより簡単な方法は、<xref:System.Windows.Controls.Label> または <xref:System.Windows.Controls.TextBlock>を使用することです。 <xref:System.Windows.Media.GlyphRun>の詳細については、「 [GlyphRun オブジェクトと Glyphs 要素の](../advanced/introduction-to-the-glyphrun-object-and-glyphs-element.md)概要」を参照してください。  
  
<a name="compositedrawingssection"></a>   
## <a name="composite-drawings"></a>複合描画  
 <xref:System.Windows.Media.DrawingGroup> を使用すると、複数の描画を1つの複合描画に結合できます。 <xref:System.Windows.Media.DrawingGroup>を使用すると、図形、画像、テキストを1つの <xref:System.Windows.Media.Drawing> オブジェクトに結合できます。  
  
 次の例では、<xref:System.Windows.Media.DrawingGroup> を使用して、2つの <xref:System.Windows.Media.GeometryDrawing> オブジェクトと <xref:System.Windows.Media.ImageDrawing> オブジェクトを結合します。 この例を実行すると、次の出力が生成されます。  
  
 ![複数の描画を含む DrawingGroup](./media/graphicsmm-simple.jpg "graphicsmm_simple")  
複合描画  
  
 [!code-csharp[DrawingMiscSnippets_snip#GraphicsMMSimpleDrawingGroupExample](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/DrawingGroupExample.cs#graphicsmmsimpledrawinggroupexample)]
 [!code-xaml[DrawingMiscSnippets_snip#GraphicsMMSimpleDrawingGroupExample](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/DrawingGroupExample.xaml#graphicsmmsimpledrawinggroupexample)]  
  
 <xref:System.Windows.Media.DrawingGroup> を使用すると、不透明マスク、変換、ビットマップ効果などの操作をそのコンテンツに適用することもできます。 <xref:System.Windows.Media.DrawingGroup> 操作は、<xref:System.Windows.Media.DrawingGroup.OpacityMask%2A>、<xref:System.Windows.Media.DrawingGroup.Opacity%2A>、<xref:System.Windows.Media.DrawingGroup.BitmapEffect%2A>、<xref:System.Windows.Media.DrawingGroup.ClipGeometry%2A>、<xref:System.Windows.Media.DrawingGroup.GuidelineSet%2A>、および <xref:System.Windows.Media.DrawingGroup.Transform%2A>の順序で適用されます。  
  
 次の図は <xref:System.Windows.Media.DrawingGroup> 操作が適用される順序を示しています。  
  
 ![DrawingGroup 操作の順序](./media/graphcismm-drawinggroup-order.png "graphcismm_drawinggroup_order")  
DrawingGroup 操作の順序  
  
 次の表では、<xref:System.Windows.Media.DrawingGroup> オブジェクトのコンテンツを操作するために使用できるプロパティについて説明します。  
  
|property|説明|図|  
|--------------|-----------------|------------------|  
|<xref:System.Windows.Media.DrawingGroup.OpacityMask%2A>|<xref:System.Windows.Media.DrawingGroup> コンテンツの選択部分の不透明度を変更します。 例については、「[How to: Control the Opacity of a Drawing](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms748242(v=vs.90))」(方法: 描画の不透明度を制御する) をご覧ください。|![不透明度マスクを持つ図面グループ](./media/graphicsmm-opmask.png "graphicsmm_opmask")|  
|<xref:System.Windows.Media.DrawingGroup.Opacity%2A>|<xref:System.Windows.Media.DrawingGroup> の内容の不透明度を一様に変更します。 <xref:System.Windows.Media.Drawing> を透明または部分的に透明にするには、このプロパティを使用します。 例については、「[How to: Apply an Opacity Mask to a Drawing](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms753195(v=vs.90))」(方法: 不透明マスクを描画に適用する) をご覧ください。|![不透明度の設定が異なる図面グループ](./media/graphicsmm-opacity.png "graphicsmm_opacity")|  
|<xref:System.Windows.Media.DrawingGroup.BitmapEffect%2A>|<xref:System.Windows.Media.DrawingGroup> の内容に <xref:System.Windows.Media.Effects.BitmapEffect> を適用します。 例については、「[How to: Apply a BitmapEffect to a Drawing](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752341(v=vs.90))」(方法: BitmapEffect を描画に適用する) をご覧ください。|![BlurBitmapEffect を持つ図面グループ](./media/graphicsmm-bitmap.png "graphicsmm_bitmap")|  
|<xref:System.Windows.Media.DrawingGroup.ClipGeometry%2A>|<xref:System.Windows.Media.Geometry>を使用して記述した領域に <xref:System.Windows.Media.DrawingGroup> の内容をクリップします。 例については、「[How to: Clip a Drawing](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms743068(v=vs.90))」(方法: 描画をクリッピングする) をご覧ください。|![クリップ領域が定義されている図面グループ](./media/graphicsmm-clipgeom.png "graphicsmm_clipgeom")|  
|<xref:System.Windows.Media.DrawingGroup.GuidelineSet%2A>|デバイスに依存しないピクセルを、指定したガイドラインに沿ったデバイス ピクセルにスナップします。 このプロパティは、低解像度ディスプレイで、非常に詳細なグラフィックスがはっきりとレンダリングされるようにするのに便利です。 例については、「[方法 : 描画に GuidelineSet を適用する](how-to-apply-a-guidelineset-to-a-drawing.md)」をご覧ください。|![ガイドの Lineset の有無に関係なく、図面グループ](./media/graphicsmm-drawinggroup-guidelineset.png "graphicsmm_drawinggroup_guidelineset")|  
|<xref:System.Windows.Media.DrawingGroup.Transform%2A>|<xref:System.Windows.Media.DrawingGroup> の内容を変換します。 例については、「[How to: Apply a Transform to a Drawing](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms742304(v=vs.90))」(方法: 変換を描画に適用する) をご覧ください。|![回転した図面グループ](./media/graphicsmm-rotate.png "graphicsmm_rotate")|  
  
<a name="usingimagedrawing"></a>   
## <a name="display-a-drawing-as-an-image"></a>描画をイメージとして表示する  
 <xref:System.Windows.Controls.Image> コントロールを使用して <xref:System.Windows.Media.Drawing> を表示するには、<xref:System.Windows.Media.DrawingImage> を <xref:System.Windows.Controls.Image> コントロールの <xref:System.Windows.Controls.Image.Source%2A> として使用し、<xref:System.Windows.Media.DrawingImage> オブジェクトの <xref:System.Windows.Media.DrawingImage.Drawing%2A?displayProperty=nameWithType> プロパティを表示する描画に設定します。  
  
 次の例では、<xref:System.Windows.Media.DrawingImage> と <xref:System.Windows.Controls.Image> コントロールを使用して <xref:System.Windows.Media.GeometryDrawing>を表示します。 この例を実行すると、次の出力が生成されます。  
  
 ![2 つの楕円の GeometryDrawing](./media/graphicsmm-geodraw.jpg "graphicsmm_geodraw")  
DrawingImage  
  
 [!code-csharp[DrawingMiscSnippets_snip#DrawingImageExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/DrawingImageExample.cs#drawingimageexamplewholepage)]
 [!code-xaml[DrawingMiscSnippets_snip#DrawingImageExampleWholePage](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/DrawingImageExample.xaml#drawingimageexamplewholepage)]  
  
<a name="renderingwithdrawingbrushsection"></a>   
## <a name="paint-an-object-with-a-drawing"></a>描画によるオブジェクトの塗りつぶし  
 <xref:System.Windows.Media.DrawingBrush> は、描画オブジェクトを使用して領域を塗りつぶすブラシの一種です。 描画でほとんどすべてのグラフィカル オブジェクトを塗りつぶすために使うことができます。 <xref:System.Windows.Media.DrawingBrush> の <xref:System.Windows.Media.Drawing> プロパティには、その <xref:System.Windows.Media.DrawingBrush.Drawing%2A>が記述されています。 <xref:System.Windows.Media.DrawingBrush>で <xref:System.Windows.Media.Drawing> を表示するには、ブラシの <xref:System.Windows.Media.Drawing> プロパティを使用してブラシに追加し、ブラシを使用してコントロールやパネルなどのグラフィカルオブジェクトを描画します。  
  
 次の例では、<xref:System.Windows.Media.DrawingBrush> を使用して、<xref:System.Windows.Media.GeometryDrawing>から作成されたパターンで <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.Shapes.Shape.Fill%2A> を描画します。 この例を実行すると、次の出力が生成されます。  
  
 ![並べて表示される DrawingBrush](./media/graphicsmm-drawingbrush-geometrydrawing.png "graphicsmm_drawingbrush_geometrydrawing")  
DrawingBrush で使われる GeometryDrawing  
  
 [!code-csharp[DrawingMiscSnippets_snip#DrawingBrushExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/DrawingBrushExample.cs#drawingbrushexamplewholepage)]
 [!code-xaml[DrawingMiscSnippets_snip#DrawingBrushExampleWholePage](~/samples/snippets/xaml/VS_Snippets_Wpf/DrawingMiscSnippets_snip/XAML/DrawingBrushExample.xaml#drawingbrushexamplewholepage)]  
  
 <xref:System.Windows.Media.DrawingBrush> クラスには、コンテンツを拡大してタイル表示するためのさまざまなオプションが用意されています。 <xref:System.Windows.Media.DrawingBrush>の詳細については、「[イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)の概要」を参照してください。  
  
<a name="renderingwithvisualsection"></a>   
## <a name="render-a-drawing-with-a-visual"></a>ビジュアルで描画をレンダリングする  
 <xref:System.Windows.Media.DrawingVisual> は、描画をレンダリングするように設計されたビジュアルオブジェクトの一種です。 高度にカスタマイズされたグラフィカル環境を構築したい開発者は、ビジュアル レイヤーを直接操作できますが、この概要ではそれについては説明しません。 詳しくは、「[DrawingVisual オブジェクトの使用](using-drawingvisual-objects.md)」をご覧ください。  
  
<a name="drawingcontextobjects"></a>   
## <a name="drawingcontext-objects"></a>DrawingContext オブジェクト  
 <xref:System.Windows.Media.DrawingContext> クラスを使用すると、<xref:System.Windows.Media.Visual> または <xref:System.Windows.Media.Drawing> にビジュアルコンテンツを設定できます。 このような下位レベルのグラフィックスオブジェクトの多くは、グラフィカルコンテンツを非常に効率的に記述しているため、<xref:System.Windows.Media.DrawingContext> を使用します。  
  
 <xref:System.Windows.Media.DrawingContext> の描画メソッドは、<xref:System.Drawing.Graphics?displayProperty=nameWithType> 型の描画メソッドと同様に表示されますが、実際にはまったく異なります。 <xref:System.Windows.Media.DrawingContext> は、保持モードのグラフィックスシステムで使用されます。一方、<xref:System.Drawing.Graphics?displayProperty=nameWithType> の種類は、イミディエイトモードのグラフィックスシステムで使用されます。 <xref:System.Windows.Media.DrawingContext> オブジェクトの描画コマンドを使用する場合、実際には、レンダリング命令のセットを格納します (ただし、正確なストレージメカニズムは、後でグラフィックスシステムによって使用される、<xref:System.Windows.Media.DrawingContext>を提供するオブジェクトの型に依存します)。リアルタイムで画面に描画していません。 Windows Presentation Foundation (WPF) グラフィックスシステムの動作の詳細については、「 [Wpf グラフィックスレンダリングの概要](wpf-graphics-rendering-overview.md)」を参照してください。  
  
 <xref:System.Windows.Media.DrawingContext>を直接インスタンス化することはできません。ただし、<xref:System.Windows.Media.DrawingGroup.Open%2A?displayProperty=nameWithType> や <xref:System.Windows.Media.DrawingVisual.RenderOpen%2A?displayProperty=nameWithType>などの特定のメソッドから描画コンテキストを取得することはできます。  
  
<a name="enumeratevisualcontents"></a>   
## <a name="enumerate-the-contents-of-a-visual"></a>ビジュアルの内容の列挙  
 <xref:System.Windows.Media.Drawing> オブジェクトは、他の用途に加えて、<xref:System.Windows.Media.Visual>の内容を列挙するためのオブジェクトモデルも提供します。  
  
 次の例では、<xref:System.Windows.Media.VisualTreeHelper.GetDrawing%2A> メソッドを使用して <xref:System.Windows.Media.Visual> の <xref:System.Windows.Media.DrawingGroup> 値を取得し、それを列挙します。  
  
 [!code-csharp[DrawingMiscSnippets_snip#GraphicsMMRetrieveDrawings](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/EnumerateDrawingsExample.xaml.cs#graphicsmmretrievedrawings)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Drawing>
- <xref:System.Windows.Media.DrawingGroup>
- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)
- [ジオメトリの概要](geometry-overview.md)
- [WPF での図形と基本描画の概要](shapes-and-basic-drawing-in-wpf-overview.md)
- [WPF グラフィックス レンダリングの概要](wpf-graphics-rendering-overview.md)
- [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)
- [方法トピック](drawings-how-to-topics.md)
