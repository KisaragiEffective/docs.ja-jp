---
title: グラフィックスレンダリングの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- graphics [WPF], rendering
- rendering graphics [WPF]
ms.assetid: 6dec9657-4d8c-4e46-8c54-40fb80008265
ms.openlocfilehash: 103d086857bce8ae0960452bb92a69b68dc49dfa
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744807"
---
# <a name="wpf-graphics-rendering-overview"></a>WPF グラフィックス レンダリングの概要
ここでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のビジュアル層の概要について説明します。 この記事では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] モデルでのレンダリングのサポートのための <xref:System.Windows.Media.Visual> クラスの役割に焦点を当てています。  

<a name="role_of_visual_object"></a>   
## <a name="role-of-the-visual-object"></a>ビジュアル オブジェクトの役割  
 <xref:System.Windows.Media.Visual> クラスは、すべての <xref:System.Windows.FrameworkElement> オブジェクトが派生する基本抽象化です。 また、このクラスは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で新しいコントロールを作成するためのエントリ ポイントとしても機能し、多くの点で Win32 アプリケーション モデルのウィンドウ ハンドル (HWND) と考えることができます。  
  
 <xref:System.Windows.Media.Visual> オブジェクトは、レンダリングサポートを提供する主要な役割を持つ主要な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクトです。 <xref:System.Windows.Controls.Button> や <xref:System.Windows.Controls.TextBox>などのユーザーインターフェイスコントロールは <xref:System.Windows.Media.Visual> クラスから派生し、レンダリングデータを保持するために使用されます。 <xref:System.Windows.Media.Visual> オブジェクトは、次の機能をサポートします。  
  
- 出力表示: ビジュアルの、シリアル化された永続的な描画内容をレンダリングします。  
  
- 変換: ビジュアルで変換を実行します。  
  
- クリッピング: ビジュアルのクリッピング領域をサポートします。  
  
- ヒット テスト: 座標またはジオメトリがビジュアルの境界内に含まれているかどうかを判断します。  
  
- 境界ボックスの計算: ビジュアルの四角形領域を決定します。  
  
 ただし、<xref:System.Windows.Media.Visual> オブジェクトには、次のような非レンダリング機能のサポートは含まれていません。  
  
- イベント処理  
  
- [レイアウト]  
  
- スタイル  
  
- データ バインディング  
  
- グローバリゼーション  
  
 <xref:System.Windows.Media.Visual> は、子クラスを派生させる必要があるパブリック抽象クラスとして公開されます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で公開されるビジュアル オブジェクトの階層構造を次の図に示します。  
  
 ![Visual オブジェクトから派生したクラスのダイアグラム](./media/wpf-graphics-rendering-overview/classes-derived-visual-object.png)    
  
### <a name="drawingvisual-class"></a>DrawingVisual クラス  
 <xref:System.Windows.Media.DrawingVisual> は、図形、画像、またはテキストを表示するために使用される軽量の描画クラスです。 このクラスが軽量と見なされる理由は、レイアウトやイベントの処理を行わないため、実行時のパフォーマンスが向上するからです。 このため、背景やクリップ アートの描画に適しています。 <xref:System.Windows.Media.DrawingVisual> を使用すると、カスタムビジュアルオブジェクトを作成できます。 詳しくは、「[DrawingVisual オブジェクトの使用](using-drawingvisual-objects.md)」を参照してください。  
  
### <a name="viewport3dvisual-class"></a>Viewport3DVisual クラス  
 <xref:System.Windows.Media.Media3D.Viewport3DVisual> は、2D <xref:System.Windows.Media.Visual> と <xref:System.Windows.Media.Media3D.Visual3D> オブジェクトの間のブリッジを提供します。 <xref:System.Windows.Media.Media3D.Visual3D> クラスは、すべての3D ビジュアル要素の基本クラスです。 <xref:System.Windows.Media.Media3D.Viewport3DVisual> では、<xref:System.Windows.Media.Media3D.Viewport3DVisual.Camera%2A> 値と <xref:System.Windows.Media.Media3D.Viewport3DVisual.Viewport%2A> 値を定義する必要があります。 カメラを使用するとシーンを表示できます。 ビューポートは、投影が 2D サーフェイス上にマップされる場所を設定します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の 3D について詳しくは、「[3-D グラフィックスの概要](3-d-graphics-overview.md)」をご覧ください。  
  
### <a name="containervisual-class"></a>ContainerVisual クラス  
 <xref:System.Windows.Media.ContainerVisual> クラスは、<xref:System.Windows.Media.Visual> オブジェクトのコレクションのコンテナーとして使用されます。 <xref:System.Windows.Media.DrawingVisual> クラスは、<xref:System.Windows.Media.ContainerVisual> クラスから派生し、ビジュアルオブジェクトのコレクションを格納できるようにします。  
  
### <a name="drawing-content-in-visual-objects"></a>ビジュアル オブジェクトの描画コンテンツ  
 <xref:System.Windows.Media.Visual> オブジェクトは、そのレンダリングデータを**ベクターグラフィックス命令リスト**として格納します。 命令リスト内の各項目は、グラフィックス データと関連リソースの低レベル セットを、シリアル化された形式で表します。 描画コンテンツを格納できるレンダリング データには、次の 4 つの種類があります。  
  
|描画コンテンツの種類|[説明]|  
|--------------------------|-----------------|  
|ベクター グラフィックス|ベクターグラフィックスデータ、および関連付けられているすべての <xref:System.Windows.Media.Brush> と <xref:System.Windows.Media.Pen> 情報を表します。|  
|Image|<xref:System.Windows.Rect>によって定義される領域内のイメージを表します。|  
|グリフ|指定されたフォントリソースからの一連のグリフである <xref:System.Windows.Media.GlyphRun>を表示する描画を表します。 テキストはこれによって表示されます。|  
|ビデオ|ビデオをレンダリングする描画を表します。|  
  
 <xref:System.Windows.Media.DrawingContext> を使用すると、<xref:System.Windows.Media.Visual> にビジュアルコンテンツを設定できます。 <xref:System.Windows.Media.DrawingContext> オブジェクトの描画コマンドを使用する場合、実際には、グラフィックスシステムによって使用されるレンダリングデータのセットを格納します。リアルタイムで画面に描画していません。  
  
 <xref:System.Windows.Controls.Button>などの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールを作成すると、コントロールによって描画用のレンダリングデータが暗黙的に生成されます。 たとえば、<xref:System.Windows.Controls.Button> の [<xref:System.Windows.Controls.ContentControl.Content%2A>] プロパティを設定すると、コントロールにグリフのレンダリング表現が格納されます。  
  
 <xref:System.Windows.Media.Visual> は、<xref:System.Windows.Media.DrawingGroup>内に含まれる1つ以上の <xref:System.Windows.Media.Drawing> オブジェクトとしてそのコンテンツを記述します。 <xref:System.Windows.Media.DrawingGroup> は、不透明マスク、変換、ビットマップ効果、およびその内容に適用されるその他の操作についても説明します。 <xref:System.Windows.Media.DrawingGroup> 操作は、コンテンツがレンダリングされるときに、<xref:System.Windows.Media.DrawingGroup.OpacityMask%2A>、<xref:System.Windows.Media.DrawingGroup.Opacity%2A>、<xref:System.Windows.Media.DrawingGroup.BitmapEffect%2A>、<xref:System.Windows.Media.DrawingGroup.ClipGeometry%2A>、<xref:System.Windows.Media.DrawingGroup.GuidelineSet%2A>、<xref:System.Windows.Media.DrawingGroup.Transform%2A>の順に適用されます。  
  
 次の図は、レンダリングシーケンス中に <xref:System.Windows.Media.DrawingGroup> 操作が適用される順序を示しています。  
  
 ![DrawingGroup の操作の順序](./media/graphcismm-drawinggroup-order.png "graphcismm_drawinggroup_order")  
DrawingGroup の操作の順序  
  
 詳しくは、「[Drawing オブジェクトの概要](drawing-objects-overview.md)」をご覧ください。  
  
#### <a name="drawing-content-at-the-visual-layer"></a>ビジュアル層での描画コンテンツ  
 <xref:System.Windows.Media.DrawingContext>を直接インスタンス化することはできません。ただし、<xref:System.Windows.Media.DrawingGroup.Open%2A?displayProperty=nameWithType> や <xref:System.Windows.Media.DrawingVisual.RenderOpen%2A?displayProperty=nameWithType>などの特定のメソッドから描画コンテキストを取得することはできます。 次の例では、<xref:System.Windows.Media.DrawingVisual> から <xref:System.Windows.Media.DrawingContext> を取得し、それを使用して四角形を描画します。  
  
 [!code-csharp[drawingvisualsample#101](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingVisualSample/CSharp/Window1.xaml.cs#101)]
 [!code-vb[drawingvisualsample#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DrawingVisualSample/visualbasic/window1.xaml.vb#101)]  
  
#### <a name="enumerating-drawing-content-at-the-visual-layer"></a>ビジュアル層で描画コンテンツを列挙する  
 <xref:System.Windows.Media.Drawing> オブジェクトは、他の用途に加えて、<xref:System.Windows.Media.Visual>の内容を列挙するためのオブジェクトモデルも提供します。  
  
> [!NOTE]
> ビジュアルの内容を列挙する場合、ベクターグラフィックス命令リストとしてレンダリングデータの基になる表現ではなく、<xref:System.Windows.Media.Drawing> オブジェクトを取得します。  
  
 次の例では、<xref:System.Windows.Media.VisualTreeHelper.GetDrawing%2A> メソッドを使用して <xref:System.Windows.Media.Visual> の <xref:System.Windows.Media.DrawingGroup> 値を取得し、それを列挙します。  
  
 [!code-csharp[DrawingMiscSnippets_snip#GraphicsMMRetrieveDrawings](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingMiscSnippets_snip/CSharp/EnumerateDrawingsExample.xaml.cs#graphicsmmretrievedrawings)]  
  
<a name="how_visual_objects_are_used_to_build_controls"></a>   
## <a name="how-visual-objects-are-used-to-build-controls"></a>ビジュアル オブジェクトを使用してコントロールをビルドする方法  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のオブジェクトの多くは、他のビジュアル オブジェクトで構成されています。そのため、それらのオブジェクトには、子孫オブジェクトのさまざまな階層を格納することができます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のユーザー インターフェイス要素 (コントロールなど) の多くは、さまざまな種類のレンダリング要素を表す、複数のビジュアル オブジェクトで構成されています。 たとえば、<xref:System.Windows.Controls.Button> コントロールには、<xref:Microsoft.Windows.Themes.ClassicBorderDecorator>、<xref:System.Windows.Controls.ContentPresenter>、<xref:System.Windows.Controls.TextBlock>など、他の多くのオブジェクトを含めることができます。  
  
 次のコードは、マークアップで定義されている <xref:System.Windows.Controls.Button> コントロールを示しています。  
  
 [!code-xaml[VisualsOverview#VisualsOverviewSnippet1](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml#visualsoverviewsnippet1)]  
  
 既定の <xref:System.Windows.Controls.Button> コントロールを構成するビジュアルオブジェクトを列挙すると、次に示すビジュアルオブジェクトの階層が表示されます。  
  
 ![ビジュアル ツリー階層のダイアグラム](./media/wpf-graphics-rendering-overview/visual-object-diagram.gif) 
  
 <xref:System.Windows.Controls.Button> コントロールには、<xref:Microsoft.Windows.Themes.ClassicBorderDecorator> 要素が含まれています。この要素には <xref:System.Windows.Controls.ContentPresenter> 要素が含まれています。 <xref:Microsoft.Windows.Themes.ClassicBorderDecorator> 要素は、<xref:System.Windows.Controls.Button>の境界線と背景を描画します。 <xref:System.Windows.Controls.ContentPresenter> 要素は、<xref:System.Windows.Controls.Button>の内容を表示します。 この場合、テキストを表示しているため、<xref:System.Windows.Controls.ContentPresenter> 要素には <xref:System.Windows.Controls.TextBlock> 要素が含まれます。 <xref:System.Windows.Controls.Button> コントロールが <xref:System.Windows.Controls.ContentPresenter> を使用するということは、コンテンツを他の要素 (<xref:System.Windows.Controls.Image> や、<xref:System.Windows.Media.EllipseGeometry>などのジオメトリ) で表すことができることを意味します。  
  
### <a name="control-templates"></a>コントロール テンプレート  
 コントロールの階層にコントロールを展開する際の鍵となるのは、<xref:System.Windows.Controls.ControlTemplate>です。 コントロール テンプレートは、コントロールの既定のビジュアル階層を指定します。 コントロールを明示的に参照すると、コントロールのビジュアル階層が暗黙的に参照されます。 コントロール テンプレートの既定値をオーバーライドして、コントロールの外観をカスタマイズすることもできます。 たとえば、<xref:System.Windows.Controls.Button> コントロールの背景色の値を変更して、純色の値の代わりに線状グラデーションの色の値を使用することができます。 詳しくは、「[ボタンのスタイルとテンプレート](../controls/button-styles-and-templates.md)」をご覧ください。  
  
 <xref:System.Windows.Controls.Button> コントロールなどのユーザーインターフェイス要素には、コントロールのレンダリング定義全体を記述するいくつかのベクターグラフィックス命令リストが含まれています。 次のコードは、マークアップで定義されている <xref:System.Windows.Controls.Button> コントロールを示しています。  
  
 [!code-xaml[VisualsOverview#VisualsOverviewSnippet2](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml#visualsoverviewsnippet2)]  
  
 <xref:System.Windows.Controls.Button> コントロールを構成するビジュアルオブジェクトとベクターグラフィックス命令リストを列挙すると、次の図に示すようなオブジェクトの階層が表示されます。  
  
 ![ビジュアル ツリーおよび描画データのダイアグラム](./media/wpf-graphics-rendering-overview/visual-tree-rendering-data.png)  
  
 <xref:System.Windows.Controls.Button> コントロールには、<xref:Microsoft.Windows.Themes.ClassicBorderDecorator> 要素が含まれています。この要素には <xref:System.Windows.Controls.ContentPresenter> 要素が含まれています。 <xref:Microsoft.Windows.Themes.ClassicBorderDecorator> 要素は、ボタンの境界線と背景を構成するすべての不連続グラフィック要素を描画する役割を担います。 <xref:System.Windows.Controls.ContentPresenter> 要素は、<xref:System.Windows.Controls.Button>の内容を表示します。 この場合、イメージを表示しているため、<xref:System.Windows.Controls.ContentPresenter> 要素には <xref:System.Windows.Controls.Image> 要素が含まれます。  
  
 ビジュアル オブジェクトとベクター グラフィックス命令リストの階層については、次の点に注意する必要があります。  
  
- 階層内の順序は、描画情報のレンダリング順序を表します。 子要素は、ルート ビジュアル要素を起点として、左から右、上から下に走査されます。 要素に子ビジュアル要素がある場合、子ビジュアル要素は要素の兄弟よりも先に走査されます。  
  
- <xref:System.Windows.Controls.ContentPresenter>など、階層内の非リーフノード要素は、子要素を格納するために使用されます。これには、命令リストは含まれません。  
  
- ビジュアル要素にベクター グラフィックス命令リストとビジュアル子の両方が含まれている場合は、ビジュアル子オブジェクトが描画される前に、親ビジュアル要素の命令リストがレンダリングされます。  
  
- ベクター グラフィックス命令リスト内の項目は、左から右の順にレンダリングされます。  
  
<a name="visual_tree"></a>   
## <a name="visual-tree"></a>ビジュアル ツリー  
 ビジュアル ツリーには、アプリケーションのユーザー インターフェイスで使用されるすべてのビジュアル要素が含まれます。 ビジュアル要素には永続化された描画情報が含まれているので、ビジュアル ツリーは、ディスプレイ デバイスへの出力を構成するのに必要なレンダリング情報をすべて含んだ、シーン グラフであると考えることができます。 このツリーは、コードかマークアップかを問わず、アプリケーションで直接作成されたすべてのビジュアル要素を累積したものです。 ビジュアル ツリーには、コントロールやデータ オブジェクトなど、要素のテンプレート拡張によって作成されたビジュアル要素もすべて含まれます。  
  
 次のコードは、マークアップで定義された <xref:System.Windows.Controls.StackPanel> 要素を示しています。  
  
 [!code-xaml[VisualsOverview#VisualsOverviewSnippet3](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml#visualsoverviewsnippet3)]  
  
 マークアップの例で <xref:System.Windows.Controls.StackPanel> 要素を構成するビジュアルオブジェクトを列挙すると、次の図に示すようなビジュアルオブジェクトの階層が表示されます。  
  
 ![ビジュアル ツリー階層のダイアグラム](./media/wpf-graphics-rendering-overview/visual-tree-hierarchy.gif)  
  
### <a name="rendering-order"></a>レンダリング順序  
 ビジュアル ツリーは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のビジュアル オブジェクトと描画オブジェクトの表示順序を決定します。 走査の順序は、ビジュアル ツリーの最上位ノードであるルート ビジュアルから始まります。 次に、ルート ビジュアルの子が左から右に走査されます。 ビジュアルの子が存在する場合、子はビジュアルの兄弟よりも先に走査されます。 つまり、子ビジュアルの内容は、そのビジュアル自体の内容よりも前面に表示されます。  
  
 ![ビジュアルツリーの描画順序のダイアグラム](./media/wpf-graphics-rendering-overview/visual-tree-rendering-order.gif) 
  
### <a name="root-visual"></a>ルート ビジュアル  
 **ルート ビジュアル**は、ビジュアル ツリー階層内の最上位の要素です。 ほとんどのアプリケーションでは、ルートビジュアルの基底クラスは <xref:System.Windows.Window> または <xref:System.Windows.Navigation.NavigationWindow>です。 ただし、ビジュアル オブジェクトを Win32 アプリケーションでホストする場合は、Win32 ウィンドウにホストする最上位のビジュアルがルート ビジュアルになります。 詳しくは、「[チュートリアル: Win32 アプリケーションでのビジュアル オブジェクトのホスト](tutorial-hosting-visual-objects-in-a-win32-application.md)」をご覧ください。  
  
### <a name="relationship-to-the-logical-tree"></a>論理ツリーとの関係  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の論理ツリーは、実行時のアプリケーションの要素を表します。 このツリーを直接操作することはありませんが、アプリケーションのこのビューは、プロパティの継承やイベントのルーティングを理解する上で役立ちます。 ビジュアルツリーとは異なり、論理ツリーは、<xref:System.Windows.Documents.ListItem>などの非ビジュアルデータオブジェクトを表すことができます。 多くの場合、論理ツリーはアプリケーションのマークアップ定義にほぼ一致します。 次のコードは、マークアップで定義された <xref:System.Windows.Controls.DockPanel> 要素を示しています。  
  
 [!code-xaml[VisualsOverview#VisualsOverviewSnippet5](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml#visualsoverviewsnippet5)]  
  
 マークアップの例で <xref:System.Windows.Controls.DockPanel> 要素を構成する論理オブジェクトを列挙すると、次の図に示すような論理オブジェクトの階層が表示されます。  
  
 ![ツリー ダイアグラム](./media/tree1-wcp.gif "Tree1_wcp")  
論理ツリーのダイアグラム  
  
 ビジュアル ツリーと論理ツリーはどちらも、現在のアプリケーション要素セットと同期し、要素の追加、削除、または変更をすべて反映します。 ただし、これらのツリーが表すアプリケーションのビューはそれぞれ異なるものです。 ビジュアルツリーとは異なり、論理ツリーはコントロールの <xref:System.Windows.Controls.ContentPresenter> 要素を展開しません。 つまり、同じオブジェクト セットについて見ても、論理ツリーとビジュアルツリーの間に 1 対 1 の対応関係はありません。 実際、パラメーターと同じ要素を使用して、 **Logicaltreehelper**オブジェクトの <xref:System.Windows.LogicalTreeHelper.GetChildren%2A> メソッドと**VisualTreeHelper**オブジェクトの <xref:System.Windows.Media.VisualTreeHelper.GetChild%2A> メソッドを呼び出すと、結果は異なります。  
  
 論理ツリーについて詳しくは、「[WPF のツリー](../advanced/trees-in-wpf.md)」をご覧ください。  
  
### <a name="viewing-the-visual-tree-with-xamlpad"></a>XamlPad を使用したビジュアル ツリーの表示  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ツール XamlPad は、現在定義されている XAML コンテンツに対応するビジュアルツリーを表示し、探索するためのオプションを提供します。 ビジュアル ツリーを表示するには、メニュー バーの **[Show Visual Tree]** (ビジュアル ツリーを表示) ボタンをクリックします。 次に示すのは、XamlPad の**ビジュアルツリーエクスプローラー**パネルで、ビジュアルツリーノードに XAML コンテンツを展開する方法を示しています。  
  
 ![XamlPad のビジュアル ツリー エクスプローラー パネル](./media/wpf-graphics-rendering-overview/visual-tree-explorer.png)  

 <xref:System.Windows.Controls.Label>、<xref:System.Windows.Controls.TextBox>、および <xref:System.Windows.Controls.Button> の各コントロールで、XamlPad の**ビジュアルツリーエクスプローラー**パネルにそれぞれ個別のビジュアルオブジェクト階層が表示されていることに注意してください。 これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールに、そのコントロールのビジュアルツリーを含む <xref:System.Windows.Controls.ControlTemplate> があるためです。 コントロールを明示的に参照すると、コントロールのビジュアル階層が暗黙的に参照されます。  
  
### <a name="profiling-visual-performance"></a>ビジュアル パフォーマンスのプロファイリング  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] にはパフォーマンス プロファイリング ツールのセットがあります。アプリケーションの実行時動作を分析したり、適用できるパフォーマンス最適化の種類を決定したりできます。 Visual Profiler ツールでは、パフォーマンス データをアプリケーションのビジュアル ツリーに直接マップすることにより、それらのデータを多彩なグラフィカル ビューで表示できます。 このスクリーンショットでは、Visual Profiler の **[CPU 使用率]** セクションに、オブジェクトの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] サービスの使用率が詳しく表示されています (レンダリングやレイアウトなど)。  
  
 ![Visual Profiler の表示出力](./media/wpfperf-visualprofiler-04.png "WPFPerf_VisualProfiler_04")  
Visual Profiler 表示出力  
  
<a name="visual_rendering_behavior"></a>   
## <a name="visual-rendering-behavior"></a>ビジュアル レンダリング動作  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、ビジュアル オブジェクトのレンダリング動作に影響を及ぼす機能が導入されています。保持モード グラフィックス、ベクター グラフィックス、およびデバイス非依存グラフィックスです。  
  
### <a name="retained-mode-graphics"></a>保持モード グラフィックス  
 ビジュアル オブジェクトの役割を理解するためには、**イミディエイト モード**のグラフィックス システムと**保持モード**のグラフィックス システムの違いについて理解することが重要です。 GDI または GDI+ に基づく標準的な Win32 アプリケーションでは、イミディエイト モードのグラフィックス システムが使用されます。 これは、ウィンドウのサイズ変更やオブジェクトの外観変更などといったアクションによって無効化されたクライアント領域の部分を、アプリケーションが再描画するということを意味します。  
  
 ![Win32 レンダリング シーケンスのダイアグラム](./media/wpf-graphics-rendering-overview/win32-rendering-squence.png)  
  
 これに対し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、保持モード システムが使用されます。 これは、外観を持つアプリケーション オブジェクト側で、シリアル化された一連の描画データが定義されるということを意味します。 描画データが定義された後は、アプリケーション オブジェクトをレンダリングするためのすべての再描画要求に、システム側が対応します。 実行時であっても、アプリケーション オブジェクトを変更したり作成した場合の描画要求への対応は、システムに任せることができます。 保持モードのグラフィックス システムでは、描画情報が常にシリアル化された状態でアプリケーションに保持されますが、レンダリングはシステムによって実行されます。これが、保持モードのグラフィックス システムの長所です。 次の図は、アプリケーションでの描画要求が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] によって処理される様子を示したものです。  
  
 ![WPF レンダリング シーケンスのダイアグラム](./media/wpf-graphics-rendering-overview/wpf-rendering-sequence.png)  

#### <a name="intelligent-redrawing"></a>インテリジェントな再描画  
 保持モード グラフィックスを使用することの最も大きな利点の 1 つは、アプリケーション内のどの項目を再描画するのかを、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が効率的に最適化できることです。 さまざまな不透明度が適用された複雑なシーンがある場合でも、通常、再描画を最適化するために特殊な目的のコードを記述する必要はありません。 これに対し、Win32 プログラミングでは、更新領域内の再描画量を最小化してアプリケーションを最適化するために、多大な労力が費やされることもあります。 Win32 アプリケーションで再描画を最適化するための複雑な作業の例については、「[Redrawing in the Update Region](/windows/desktop/gdi/redrawing-in-the-update-region)」(更新領域での再描画) をご覧ください。  
  
### <a name="vector-graphics"></a>ベクター グラフィックス  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、がレンダリング データ形式として**ベクタ グラフィックス**が使用されます。 スケーラブル ベクター グラフィックス (SVG)、Windows メタファイル (.wmf)、TrueType フォントなどのベクター グラフィックスは、レンダリング データを格納し、それを命令リストとして伝達します。命令リストには、グラフィックス プリミティブを使用してイメージを再作成するための方法が記述されます。 たとえば、TrueType フォントは、ピクセル配列ではなく、直線、曲線、およびコマンドのセットで表されるアウトライン フォントです。 ベクター グラフィックスの主な利点の 1 つは、任意のサイズや解像度に拡大縮小できることです。  
  
 ビットマップ グラフィックスは、ベクター グラフィックスとは異なり、特定の解像度で事前にレンダリングされた、ピクセル単位のイメージ表現としてレンダリング データを格納します。 ビットマップ グラフィックス形式とベクター グラフィックス形式の主な違いの 1 つは、元のソース イメージへの忠実性です。 たとえば、ソース イメージのサイズを変更した場合、ビットマップ グラフィックス システムではイメージが伸縮されますが、ベクタ グラフィックス システムでは、イメージが拡大縮小されるため、イメージの忠実性が維持されます。  
  
 次の図は、ソース イメージが 300% に拡大された場合の例です。 ソース イメージをビットマップ グラフィックス イメージとして拡大したイメージには、ゆがみが生じています。ベクター グラフィックス イメージとして拡大縮小した場合、こうしたゆがみは生じません。  
  
 ![ラスター グラフィックスとベクター グラフィックスの違い](./media/wpf-graphics-rendering-overview/raster-vector-differences.png)  
  
 次のマークアップは、定義されている2つの <xref:System.Windows.Shapes.Path> 要素を示しています。 2番目の要素は、<xref:System.Windows.Media.ScaleTransform> を使用して、最初の要素の描画命令のサイズを300% で変更します。 <xref:System.Windows.Shapes.Path> 要素の描画命令は変更されていないことに注意してください。  
  
 [!code-xaml[VectorGraphicsSnippets#VectorGraphicsSnippet1](~/samples/snippets/csharp/VS_Snippets_Wpf/VectorGraphicsSnippets/CS/PageOne.xaml#vectorgraphicssnippet1)]  
  
### <a name="about-resolution-and-device-independent-graphics"></a>解像度とデバイス非依存グラフィックスについて  
 画面上のテキストやグラフィックスのサイズを決定するシステム要素は、2 つあります。解像度と DPI です。 解像度は、画面に表示されるピクセルの数を表します。 解像度が高くなると、ピクセルが小さくなり、グラフィックスとテキストがより滑らかに表示されます。 モニターの解像度が 1024 x 768 に設定されている場合、解像度を 1600 x 1200 に変更すると、表示されるグラフィックスがより小さくなります。  
  
 もう 1 つのシステム設定である DPI は、画面上の 1 インチのサイズをピクセル単位で表したものです。 ほとんどの Windows システムでは DPI が96であるため、画面インチは96ピクセルです。 DPI の設定を上げると、画面上の 1 インチが長くなり、設定を下げると、画面上の 1 インチが短くなります。 そのため、ほとんどのシステムでは、画面上の 1 インチと実際の 1 インチが同じサイズにはなりません。 DPI を大きくすると、画面上の 1 インチのサイズが大きくなるため、DPI 対応のグラフィックスとテキストは大きくなります。 DPI を大きくすると、特に高解像度の場合にはテキストが読みやすくなります。  
  
 ただし、すべてのアプリケーションが DPI に対応しているわけではありません。一部のアプリケーションでは、主要な測定単位としてハードウェア ピクセルが使用されているため、システム DPI を変更しても影響がありません。 また、フォント サイズに DPI 対応の単位を使用している場合でも、それ以外の項目にはすべてピクセルを使用するアプリケーションが数多く存在します。 これらのアプリケーションで DPI を過度に小さくしたり、大きくしたりした場合、アプリケーションのテキストはシステムの DPI 設定に従って拡大縮小されますが、アプリケーションの UI は拡大縮小されないため、レイアウトの問題が発生する可能性があります。 この問題は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用して開発されたアプリケーションでは発生しません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、ハードウェア ピクセルの代わりに、デバイス非依存のピクセルを主要測定単位として用いた、自動拡大縮小機能がサポートされています。つまり、アプリケーション開発者が手を加えなくても、グラフィックスとテキストが適切に拡大縮小されます。 次の図は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のテキストとグラフィックスが、さまざまな DPI でどのように表示されるかを示したものです。  
  
 ![異なる DPI 設定のグラフィックとテキスト](./media/graphicsmm-dpi-setting-examples.png "graphicsmm_dpi_setting_examples")  
異なる DPI 設定のグラフィックスとテキスト  
  
<a name="visualtreehelper_class"></a>   
## <a name="visualtreehelper-class"></a>VisualTreeHelper クラス  
 <xref:System.Windows.Media.VisualTreeHelper> クラスは、visual オブジェクトレベルでプログラミングするための低レベルの機能を提供する静的ヘルパークラスです。これは、高パフォーマンスのカスタムコントロールの開発など、非常に具体的なシナリオで役立ちます。 ほとんどの場合、<xref:System.Windows.Controls.Canvas> や <xref:System.Windows.Controls.TextBlock>などの上位レベルの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] framework オブジェクトを使用すると、柔軟性と使いやすさが向上します。  
  
### <a name="hit-testing"></a>ヒット テスト  
 <xref:System.Windows.Media.VisualTreeHelper> クラスは、既定のヒットテストのサポートがニーズに合わない場合に、ビジュアルオブジェクトに対してヒットテストを行うためのメソッドを提供します。 <xref:System.Windows.Media.VisualTreeHelper> クラスの <xref:System.Windows.Media.VisualTreeHelper.HitTest%2A> メソッドを使用すると、geometry または point 座標の値が、コントロールやグラフィック要素など、特定のオブジェクトの境界内にあるかどうかを判断できます。 たとえば、ヒット テストを使用して、オブジェクトの四角形領域内でのマウス クリックが円のジオメトリ内にあるかどうかを確認することもできます。また、ヒット テストの既定の実装をオーバーライドして、独自のカスタム ヒット テスト計算を実行することもできます。  
  
 ヒット テストについて詳しくは、「[ビジュアル層でのヒット テスト](hit-testing-in-the-visual-layer.md)」をご覧ください。  
  
### <a name="enumerating-the-visual-tree"></a>ビジュアル ツリーを列挙する  
 <xref:System.Windows.Media.VisualTreeHelper> クラスは、ビジュアルツリーのメンバーを列挙するための機能を提供します。 親を取得するには、<xref:System.Windows.Media.VisualTreeHelper.GetParent%2A> メソッドを呼び出します。 ビジュアルオブジェクトの子 (直接の子孫) を取得するには、<xref:System.Windows.Media.VisualTreeHelper.GetChild%2A> メソッドを呼び出します。 このメソッドは、指定したインデックス位置にある親の <xref:System.Windows.Media.Visual> 子を返します。  
  
 次の例に示すのは、ビジュアル オブジェクトのすべての子孫を列挙する方法です。これは、ビジュアル オブジェクト階層のすべての描画情報をシリアル化する必要がある場合に使用できる手法です。  
  
 [!code-csharp[VisualsOverview#101](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml.cs#101)]
 [!code-vb[VisualsOverview#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/VisualsOverview/visualbasic/window1.xaml.vb#101)]  
  
 ほとんどの場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションの要素を表すのには、論理ツリーの方が役立ちます。 論理ツリーを直接変更することはありませんが、アプリケーションのこのビューは、プロパティの継承やイベントのルーティングを理解する上で役立ちます。 ビジュアルツリーとは異なり、論理ツリーは、<xref:System.Windows.Documents.ListItem>などの非ビジュアルデータオブジェクトを表すことができます。 論理ツリーについて詳しくは、「[WPF のツリー](../advanced/trees-in-wpf.md)」をご覧ください。  
  
 <xref:System.Windows.Media.VisualTreeHelper> クラスには、ビジュアルオブジェクトの外接する四角形を返すメソッドが用意されています。 <xref:System.Windows.Media.VisualTreeHelper.GetContentBounds%2A>を呼び出すことによって、ビジュアルオブジェクトの外接する四角形を返すことができます。 <xref:System.Windows.Media.VisualTreeHelper.GetDescendantBounds%2A>を呼び出すことによって、ビジュアルオブジェクト自体を含む、ビジュアルオブジェクトのすべての子孫の外接する四角形を返すことができます。 次のコードは、ビジュアル オブジェクトとそのすべての子孫の四角形領域を計算する方法を示したものです。  
  
 [!code-csharp[VisualsOverview#102](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsOverview/CSharp/Window1.xaml.cs#102)]
 [!code-vb[VisualsOverview#102](~/samples/snippets/visualbasic/VS_Snippets_Wpf/VisualsOverview/visualbasic/window1.xaml.vb#102)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.Visual>
- <xref:System.Windows.Media.VisualTreeHelper>
- <xref:System.Windows.Media.DrawingVisual>
- [2D グラフィックスとイメージング](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [ビジュアル層でのヒット テスト](hit-testing-in-the-visual-layer.md)
- [DrawingVisual オブジェクトの使用](using-drawingvisual-objects.md)
- [チュートリアル : Win32 アプリケーションでのビジュアル オブジェクトのホスト](tutorial-hosting-visual-objects-in-a-win32-application.md)
- [WPF アプリケーションのパフォーマンスの最適化](../advanced/optimizing-wpf-application-performance.md)
