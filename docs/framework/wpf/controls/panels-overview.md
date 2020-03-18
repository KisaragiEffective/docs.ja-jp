---
title: パネルの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Panel control [WPF], about Panel control
- controls [WPF], Panel
ms.assetid: f73644af-9941-4611-8754-6d4cef03fc44
ms.openlocfilehash: d0962793854a6066112eb987fbdb3f703617787f
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124430"
---
# <a name="panels-overview"></a>パネルの概要
<xref:System.Windows.Controls.Panel> 要素は、要素のサイズと大きさ、位置、および子コンテンツの配置を制御するコンポーネントです。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、定義済みの <xref:System.Windows.Controls.Panel> 要素が多数用意されています。また、カスタム <xref:System.Windows.Controls.Panel> 要素を作成することもできます。  
  
 このトピックの内容は次のとおりです。  
  
- [パネル クラス](#Panels_view_from_10000_feet)  
  
- [パネルの要素の一般的なメンバー](#Panels_declared_members)  
  
- [派生パネル要素](#Panels_derived_elements)  
  
- [ユーザー インターフェイス パネル](#Panels_main_UI_elements)  
  
- [入れ子になったパネル要素](#Panels_nested_panel_elements)  
  
- [カスタム パネル要素](#Panels_custom_panel_elements)  
  
- [ローカライズ/グローバリゼーション サポート](#Panels_global_localization)  
  
<a name="Panels_view_from_10000_feet"></a>   
## <a name="the-panel-class"></a>パネル クラス  
 <xref:System.Windows.Controls.Panel> は、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]でレイアウトをサポートするすべての要素の基本クラスです。 派生 <xref:System.Windows.Controls.Panel> 要素は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] およびコード内の要素を配置および整列するために使用されます。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、多くの複雑なレイアウトを可能にする派生パネル実装の総合的なスイートが含まれます。 これらの派生クラスは、標準 [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] シナリオのほとんどを可能にするプロパティとメソッドを公開します。 ニーズを満たす子の配置動作を見つけられない開発者は、<xref:System.Windows.FrameworkElement.ArrangeOverride%2A> および <xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドをオーバーライドすることにより、新しいレイアウトを作成できます。 カスタム レイアウト動作の詳細については、「[カスタム パネル要素](#Panels_custom_panel_elements)」を参照してください。  
  
<a name="Panels_declared_members"></a>   
## <a name="panel-common-members"></a>パネルの一般的なメンバー  
 すべての <xref:System.Windows.Controls.Panel> 要素は、<xref:System.Windows.FrameworkElement.Height%2A>、<xref:System.Windows.FrameworkElement.Width%2A>、<xref:System.Windows.FrameworkElement.HorizontalAlignment%2A>、<xref:System.Windows.FrameworkElement.VerticalAlignment%2A>、<xref:System.Windows.FrameworkElement.Margin%2A>、および <xref:System.Windows.FrameworkElement.LayoutTransform%2A>を含む、<xref:System.Windows.FrameworkElement>で定義された基本サイズ設定および配置プロパティをサポートします。 <xref:System.Windows.FrameworkElement>によって定義される配置プロパティの詳細については、「[配置、余白、パディングの概要](../advanced/alignment-margins-and-padding-overview.md)」を参照してください。  
  
 <xref:System.Windows.Controls.Panel> は、レイアウトを理解して使用するうえで重要な追加プロパティを公開します。 <xref:System.Windows.Controls.Panel.Background%2A> プロパティは、派生パネル要素の境界と <xref:System.Windows.Media.Brush>との間の領域を塗りつぶすために使用されます。 <xref:System.Windows.Controls.Panel.Children%2A> は、<xref:System.Windows.Controls.Panel> が構成されている要素の子コレクションを表します。 <xref:System.Windows.Controls.Panel.InternalChildren%2A> は、<xref:System.Windows.Controls.Panel.Children%2A> コレクションの内容と、データバインディングによって生成されるメンバーを表します。 どちらも、親 <xref:System.Windows.Controls.Panel>内でホストされる子要素の <xref:System.Windows.Controls.UIElementCollection> で構成されます。  
  
 また、Panel は、派生 <xref:System.Windows.Controls.Panel>での階層化された順序の実現に使用できる <xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> 添付プロパティも公開します。 <xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> 値が大きいパネルの <xref:System.Windows.Controls.Panel.Children%2A> コレクションのメンバーは、<xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> 値が小さい方の前に表示されます。 これは、<xref:System.Windows.Controls.Canvas> や <xref:System.Windows.Controls.Grid> などのパネルで、子が同じ座標空間を共有できるようにする場合に特に便利です。  
  
 また <xref:System.Windows.Controls.Panel> は、<xref:System.Windows.Controls.Panel>の既定のプレゼンテーション動作をオーバーライドするために使用できる <xref:System.Windows.Controls.Panel.OnRender%2A> メソッドも定義します。  
  
#### <a name="attached-properties"></a>アタッチされるプロパティ  
 派生パネル要素は、添付プロパティを広く活用しています。 添付プロパティは、従来の共通言語ランタイム (CLR) プロパティ "ラッパー" を持たない特殊な形式の依存関係プロパティです。 添付プロパティは、下記の例に見られるとおり、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 内に特殊な構文を持ちます。  
  
 添付プロパティの目的の 1 つは、親要素によって実際に定義されたプロパティの一意の値を子要素が格納できるようにすることです。 この機能の応用方法の 1 つは、子要素から親要素に [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] でどのように表示したいかを通知させることであり、これはアプリケーション レイアウトに非常に役立ちます。 詳細については、「[添付プロパティの概要](../advanced/attached-properties-overview.md)」を参照してください。  
  
<a name="Panels_derived_elements"></a>   
## <a name="derived-panel-elements"></a>派生パネル要素  
 多くのオブジェクトは <xref:System.Windows.Controls.Panel>から派生しますが、すべてのオブジェクトがルートレイアウトプロバイダーとして使用するためのものではありません。 アプリケーション <xref:System.Windows.Controls.VirtualizingStackPanel>の作成専用に設計された、6つの定義済みのパネルクラス (<xref:System.Windows.Controls.Canvas>、<xref:System.Windows.Controls.DockPanel>、<xref:System.Windows.Controls.Grid>、<xref:System.Windows.Controls.StackPanel>、<xref:System.Windows.Controls.WrapPanel>、および [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]) があります。  
  
 次の表に各パネル要素でカプセル化されているそれぞれの特殊機能を示します。  
  
|要素名|UI パネル?|[説明]|  
|------------------|---------------|-----------------|  
|<xref:System.Windows.Controls.Canvas>|はい|<xref:System.Windows.Controls.Canvas> 領域を基準にして相対的に子要素を座標によって明示的に配置できる領域を定義します。|  
|<xref:System.Windows.Controls.DockPanel>|はい|子要素を互いに水平方向または垂直方向に整列する領域を定義します。|  
|<xref:System.Windows.Controls.Grid>|はい|列と行で構成されている柔軟なグリッド領域を定義します。 <xref:System.Windows.Controls.Grid> の子要素は、<xref:System.Windows.FrameworkElement.Margin%2A> プロパティを使用して正確に配置できます。|  
|<xref:System.Windows.Controls.StackPanel>|はい|子要素を水平方向または垂直方向の単一行に整列します。|  
|<xref:System.Windows.Controls.Primitives.TabPanel>|いいえ|<xref:System.Windows.Controls.TabControl>のタブボタンのレイアウトを処理します。|  
|<xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel>|いいえ|<xref:System.Windows.Controls.ToolBar> コントロール内にコンテンツを配置します。|  
|<xref:System.Windows.Controls.Primitives.UniformGrid>|いいえ|<xref:System.Windows.Controls.Primitives.UniformGrid> は、すべてのセルサイズが等しいグリッド内の子を配置するために使用されます。|  
|<xref:System.Windows.Controls.VirtualizingPanel>|いいえ|子コレクションを "仮想化" できるパネルに基本クラスを提供します。|  
|<xref:System.Windows.Controls.VirtualizingStackPanel>|はい|水平方向または垂直方向の単一行でコンテンツを整列し、仮想化します。|  
|<xref:System.Windows.Controls.WrapPanel>|はい|<xref:System.Windows.Controls.WrapPanel> は、子要素を左から右へ順番に配置し、内容を含んでいるボックスの端の次の行に分割します。 後続の順序は、<xref:System.Windows.Controls.WrapPanel.Orientation%2A> プロパティの値に応じて、上から下または右から左に順番に行われます。|  
  
<a name="Panels_main_UI_elements"></a>   
## <a name="user-interface-panels"></a>ユーザー インターフェイス パネル  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で使用できるパネルクラスは6つあり、<xref:System.Windows.Controls.Canvas>、<xref:System.Windows.Controls.DockPanel>、<xref:System.Windows.Controls.Grid>、<xref:System.Windows.Controls.StackPanel>、<xref:System.Windows.Controls.VirtualizingStackPanel>、<xref:System.Windows.Controls.WrapPanel>の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] シナリオをサポートするように最適化されています。 これらのパネル要素は使いやすく、用途が広く、ほとんどのアプリケーションに対する拡張性があります。  
  
 各派生 <xref:System.Windows.Controls.Panel> 要素は、サイズ変更の制約を別々に扱います。 <xref:System.Windows.Controls.Panel> が水平方向または垂直方向のどちらかで制約を処理する方法を理解すると、レイアウトの予測可能性を高めることができます。  
  
|**パネル名**|**x 方向**|**y 方向**|  
|--------------------|----------------------|----------------------|  
|<xref:System.Windows.Controls.Canvas>|コンテンツに対する制約あり。|コンテンツに対する制約あり。|  
|<xref:System.Windows.Controls.DockPanel>|制約あり。|制約あり。|  
|<xref:System.Windows.Controls.StackPanel> (縦向き)|制約あり。|コンテンツに対する制約あり。|  
|<xref:System.Windows.Controls.StackPanel> (水平方向)|コンテンツに対する制約あり。|制約あり。|  
|<xref:System.Windows.Controls.Grid>|制約あり。|制約付き。ただし <xref:System.Windows.GridUnitType.Auto> 行と列に適用される場合を除きます。|  
|<xref:System.Windows.Controls.WrapPanel>|コンテンツに対する制約あり。|コンテンツに対する制約あり。|  
  
 これらの各要素の詳細な説明と使用例を次に示します。  
  
<a name="Panels_overview_Canvas_subsection"></a>   
### <a name="canvas"></a>キャンバス  
 <xref:System.Windows.Controls.Canvas> 要素を使用すると、 *x*座標と*y*座標の絶対値に応じてコンテンツを配置できます。 要素は一意の場所に描画できます。または、要素が同じ座標を占める場合、マークアップに表示される順序が要素の描画順序を決定します。  
  
 <xref:System.Windows.Controls.Canvas> は、<xref:System.Windows.Controls.Panel>のレイアウトを最も柔軟にサポートします。 Height プロパティと Width プロパティは、キャンバスの領域を定義するために使用され、内の要素には親 <xref:System.Windows.Controls.Canvas>の領域を基準とした絶対座標が割り当てられます。 4つのアタッチされたプロパティ、<xref:System.Windows.Controls.Canvas.Left%2A?displayProperty=nameWithType>、<xref:System.Windows.Controls.Canvas.Top%2A?displayProperty=nameWithType>、<xref:System.Windows.Controls.Canvas.Right%2A?displayProperty=nameWithType> および <xref:System.Windows.Controls.Canvas.Bottom%2A?displayProperty=nameWithType>では、<xref:System.Windows.Controls.Canvas>内のオブジェクトの配置を細かく制御できます。これにより、開発者は、画面上で要素を正確に配置して配置することができます。  
  
#### <a name="cliptobounds-within-a-canvas"></a>キャンバス内の ClipToBounds  
 <xref:System.Windows.Controls.Canvas> は、独自に定義された <xref:System.Windows.FrameworkElement.Height%2A> と <xref:System.Windows.FrameworkElement.Width%2A>の外側にある座標でも、画面上の任意の位置に子要素を配置できます。 さらに、<xref:System.Windows.Controls.Canvas> は、子のサイズの影響を受けません。 その結果、子要素は、親 <xref:System.Windows.Controls.Canvas>の外接する四角形の外側にある他の要素にオーバードローする可能性があります。 <xref:System.Windows.Controls.Canvas> の既定の動作では、親 <xref:System.Windows.Controls.Canvas>の境界の外側に子を描画できます。 この動作が望ましくない場合は、<xref:System.Windows.UIElement.ClipToBounds%2A> プロパティを `true`に設定できます。 これにより、<xref:System.Windows.Controls.Canvas> によって独自のサイズにクリップされます。 <xref:System.Windows.Controls.Canvas> は、子を境界の外側に描画できる唯一のレイアウト要素です。  
  
 この動作は、[Width のプロパティの比較のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Elements/WidthProperties)に図示されています。  
  
#### <a name="defining-and-using-a-canvas"></a>キャンバスの定義と使用  
 <xref:System.Windows.Controls.Canvas> は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] またはコードを使用するだけでインスタンス化できます。 次の例では、<xref:System.Windows.Controls.Canvas> を使用して、コンテンツを絶対に配置する方法を示します。 このコードでは、100 ピクセルの四角形を 3 つ生成します。 最初の四角形は赤色で、左上 (*x, y*) の位置が (0, 0) に指定されます。 2 番目の四角形は緑色で、左上の位置は (100, 100) となり、最初の四角形の右下になります。 3 番目の四角形は青色で、左上の位置は (50, 50) となるため、最初の四角形の右下の象限および 2 番目の四角形の左上の象限を囲む状態になります。 3 番目の四角形が最後にレイアウトされるため、他の 2 つの四角形の上に重なっているように見えます。つまり、重複している部分は 3 番目の四角形の色とみなされます。  
  
 [!code-csharp[CanvasOvwSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CanvasOvwSample/CSharp/Canvas_Ovw_Sample.cs#1)]
 [!code-vb[CanvasOvwSample#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CanvasOvwSample/VisualBasic/canvas_vb.vb#1)]
 [!code-xaml[CanvasOvwSample#1](~/samples/snippets/xaml/VS_Snippets_Wpf/CanvasOvwSample/XAML/default.xaml#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![一般的な Canvas 要素。](./media/panel-intro-canvas.PNG "panel_intro_canvas")  
  
<a name="Panels_overview_DockPanel_subsection"></a>   
### <a name="dockpanel"></a>DockPanel  
 <xref:System.Windows.Controls.DockPanel> 要素は、子コンテンツ要素で設定された <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> 添付プロパティを使用して、コンテナーの端に沿ってコンテンツを配置します。 <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> が <xref:System.Windows.Controls.Dock.Top> または <xref:System.Windows.Controls.Dock.Bottom>に設定されている場合、子要素は互いに上または下に配置されます。 <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> が <xref:System.Windows.Controls.Dock.Left> または <xref:System.Windows.Controls.Dock.Right>に設定されている場合は、子要素を互いの左右に配置します。 <xref:System.Windows.Controls.DockPanel.LastChildFill%2A> プロパティは、<xref:System.Windows.Controls.DockPanel>の子として追加された最後の要素の位置を決定します。  
  
 <xref:System.Windows.Controls.DockPanel> を使用すると、一連のボタンなど、関連するコントロールのグループを配置できます。 または、これを使用して、Microsoft Outlook と同様の "ペイン" [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を作成することもできます。  
  
#### <a name="sizing-to-content"></a>コンテンツに合わせたサイズの変更  
 <xref:System.Windows.FrameworkElement.Height%2A> プロパティと <xref:System.Windows.FrameworkElement.Width%2A> プロパティが指定されていない場合は、コンテンツのサイズを <xref:System.Windows.Controls.DockPanel> ます。 サイズは、子要素のサイズに合うように、増減させることができます。 ただし、これらのプロパティが指定されていて、次に指定された子要素の領域がなくなった場合、<xref:System.Windows.Controls.DockPanel> はその子要素または後続の子要素を表示せず、後続の子要素を測定しません。  
  
#### <a name="lastchildfill"></a>LastChildFill  
 既定では、<xref:System.Windows.Controls.DockPanel> 要素の最後の子は、残りの未割り当て領域を "塗りつぶし" します。 この動作が望ましくない場合は、<xref:System.Windows.Controls.DockPanel.LastChildFill%2A> プロパティを `false`に設定します。  
  
#### <a name="defining-and-using-a-dockpanel"></a>DockPanel の定義と使用  
 次の例では、<xref:System.Windows.Controls.DockPanel>を使用して領域をパーティション分割する方法を示します。 親 <xref:System.Windows.Controls.DockPanel>の子として5つの <xref:System.Windows.Controls.Border> 要素が追加されます。 各は、領域をパーティション分割するために、<xref:System.Windows.Controls.DockPanel> の異なる配置プロパティを使用します。 最後の要素が、残っている未割り当て領域の "塗り潰し" を実行します。  
  
 [!code-cpp[DockPanelOvwSample#1](~/samples/snippets/cpp/VS_Snippets_Wpf/DockPanelOvwSample/CPP/DockPanel_Ovw_Sample.cpp#1)]
 [!code-csharp[DockPanelOvwSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/DockPanelOvwSample/CSharp/DockPanel_Ovw_Sample.cs#1)]
 [!code-vb[DockPanelOvwSample#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DockPanelOvwSample/VisualBasic/dockpanel_vb.vb#1)]
 [!code-xaml[DockPanelOvwSample#1](~/samples/snippets/xaml/VS_Snippets_Wpf/DockPanelOvwSample/XAML/default.xaml#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![一般的な System.windows.controls.dockpanel> シナリオ。](./media/panel-intro-dockpanel.PNG "panel_intro_dockpanel")  
  
<a name="Panels_overview_Grid_subsection"></a>   
### <a name="grid"></a>グリッド  
 <xref:System.Windows.Controls.Grid> 要素は、絶対位置と表形式のデータコントロールの機能をマージします。 <xref:System.Windows.Controls.Grid> を使用すると、要素を簡単に配置したりスタイルを適用したりできます。 <xref:System.Windows.Controls.Grid> を使用すると、柔軟な行と列のグループ化を定義できるだけでなく、複数の <xref:System.Windows.Controls.Grid> 要素間でサイズ設定情報を共有するためのメカニズムも提供されます。  
  
#### <a name="how-is-grid-different-from-table"></a>グリッドとテーブルを区別する方法  
 <xref:System.Windows.Documents.Table> と <xref:System.Windows.Controls.Grid> はいくつかの一般的な機能を共有しますが、それぞれが異なるシナリオに最適です。 <xref:System.Windows.Documents.Table> は、フローコンテンツ内で使用するように設計されています (フローコンテンツの詳細については、「[フロードキュメントの概要](../advanced/flow-document-overview.md)」を参照してください)。 グリッドは、フォーム内で最適に使用される (基本的に任意の場所以外のフロー コンテンツ)。 <xref:System.Windows.Documents.FlowDocument>内では、<xref:System.Windows.Documents.Table> は改ページ位置、列の折り返し、コンテンツの選択などのフローコンテンツの動作をサポートしていますが、<xref:System.Windows.Controls.Grid> ではサポートされていません。 一方、<xref:System.Windows.Controls.Grid> は <xref:System.Windows.Documents.FlowDocument> の外部で最もよく使用されます。たとえば、行インデックスと列インデックスに基づいて要素が追加される <xref:System.Windows.Controls.Grid> などですが、<xref:System.Windows.Documents.Table> はできません。 <xref:System.Windows.Controls.Grid> 要素を使用すると、1つの "セル" 内に複数の要素を持つことができるように、子コンテンツをレイヤー化できます。 <xref:System.Windows.Documents.Table> は、レイヤーをサポートしていません。 <xref:System.Windows.Controls.Grid> の子要素は、"セル" の境界の領域を基準として絶対に配置できます。 <xref:System.Windows.Documents.Table> では、この機能はサポートされていません。 最後に、<xref:System.Windows.Controls.Grid> は <xref:System.Windows.Documents.Table>よりも軽量です。  
  
#### <a name="sizing-behavior-of-columns-and-rows"></a>列と行のサイズ変更動作  
 <xref:System.Windows.Controls.Grid> 内で定義されている列と行では、残りの領域を均等に分散するために <xref:System.Windows.GridUnitType.Star> のサイズ変更を利用できます。 行または列の高さまたは幅として <xref:System.Windows.GridUnitType.Star> が選択されている場合、その列または行は、残りの使用可能な領域の重み付け比率を受け取ります。 これは <xref:System.Windows.GridUnitType.Auto>とは対照的です。これは、列または行内のコンテンツのサイズに基づいて、スペースを均等に分散します。 `*` を使用する場合、この値は `2*` または [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] と表現されます。 最初のケースでは、行または列は使用可能なスペース領域を 1 回受け取り、2 番目のケースでは 2 回受け取ることになります。 この手法を組み合わせて、<xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> と <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> `Stretch` の値を使用して領域を均等に分散することにより、レイアウト領域を画面領域の割合でパーティション分割することができます。 <xref:System.Windows.Controls.Grid> は、この方法で領域を分散できる唯一のレイアウトパネルです。  
  
#### <a name="defining-and-using-a-grid"></a>グリッドの定義と使用  
 次の例では、Windows の [スタート] メニューで使用できる [実行] ダイアログボックスに表示されるのと同様の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] をビルドする方法を示します。  
  
 [!code-csharp[GridRunDialog#1](~/samples/snippets/csharp/VS_Snippets_Wpf/GridRunDialog/CSharp/window1.xaml.cs#1)]
 [!code-vb[GridRunDialog#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GridRunDialog/VisualBasic/grid_vb.vb#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![一般的な Grid 要素。](./media/avalon-run-dialog.PNG "avalon_run_dialog")  
  
<a name="Panels_overview_StackPanel_subsection"></a>   
### <a name="stackpanel"></a>StackPanel  
 <xref:System.Windows.Controls.StackPanel> を使用すると、割り当てられた方向に要素を "積み重ねる" ことができます。 既定のスタック方向は、縦です。 <xref:System.Windows.Controls.StackPanel.Orientation%2A> プロパティを使用して、コンテンツフローを制御できます。  
  
#### <a name="stackpanel-vs-dockpanel"></a>StackPanel と System.windows.controls.dockpanel>  
 <xref:System.Windows.Controls.DockPanel> は子要素を "積み重ねる" こともできますが <xref:System.Windows.Controls.DockPanel> と <xref:System.Windows.Controls.StackPanel> では、一部の使用シナリオでは同様の結果が生成されません。 たとえば、子要素の順序は、<xref:System.Windows.Controls.StackPanel>ではなく <xref:System.Windows.Controls.DockPanel> のサイズに影響することがあります。 これは、<xref:System.Windows.Controls.StackPanel> が <xref:System.Double.PositiveInfinity>のスタックの方向に測定されるのに対し、<xref:System.Windows.Controls.DockPanel> は使用可能なサイズのみを測定するためです。  
  
 この主な相違点を次の例に示します。  
  
 [!code-cpp[StackPanelOvw4#1](~/samples/snippets/cpp/VS_Snippets_Wpf/StackPanelOvw4/CPP/StackPanel_Ovw_Sample4.cpp#1)]
 [!code-csharp[StackPanelOvw4#1](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanelOvw4/CSharp/StackPanel_Ovw_Sample4.cs#1)]
 [!code-vb[StackPanelOvw4#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StackPanelOvw4/VisualBasic/StackPanelSamp.vb#1)]
 [!code-xaml[StackPanelOvw4#1](~/samples/snippets/xaml/VS_Snippets_Wpf/StackPanelOvw4/XAML/default.xaml#1)]  
  
 このイメージにはレンダリング動作の違いが見られます。  
  
 ![スクリーンショット: StackPanel System.windows.controls.dockpanel> スクリーンショット](./media/layout-smiley-stackpanel.PNG "layout_smiley_stackpanel")  
  
#### <a name="defining-and-using-a-stackpanel"></a>StackPanel の定義と使用  
 次の例では、<xref:System.Windows.Controls.StackPanel> を使用して、垂直方向に配置された一連のボタンを作成する方法を示します。 水平方向の配置の場合は、<xref:System.Windows.Controls.StackPanel.Orientation%2A> プロパティを <xref:System.Windows.Controls.Orientation.Horizontal>に設定します。  
  
 [!code-csharp[StackPanel_ovw2#1](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanel_ovw2/CSharp/StackPanel_Ovw_Sample2.cs#1)]
 [!code-vb[StackPanel_ovw2#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StackPanel_ovw2/VisualBasic/StackPanelOvw.vb#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![一般的な StackPanel 要素。](./media/panel-intro-stackpanel.PNG "panel_intro_stackpanel")  
  
<a name="Panels_overview_VirtualizingStackPanel_subsection"></a>   
#### <a name="virtualizingstackpanel"></a>VirtualizingStackPanel  
 また [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、データバインドされた子コンテンツを自動的に "仮想化" する <xref:System.Windows.Controls.StackPanel> 要素のバリエーションも提供します。 ここで、"仮想化" は、どの項目を画面に表示するかに基づいて、要素のサブセットを多数のデータ項目から生成する手法を指します。 特定の時間に画面に UI 要素が少ししか表示されていない場合に多数の UI 要素を生成すると、メモリおよびプロセッサの両方に負荷がかかります。 <xref:System.Windows.Controls.VirtualizingStackPanel> (<xref:System.Windows.Controls.VirtualizingPanel>によって提供される機能を使用) によって、表示可能な項目が計算され、<xref:System.Windows.Controls.ItemsControl> (<xref:System.Windows.Controls.ListBox> や <xref:System.Windows.Controls.ListView>など) の <xref:System.Windows.Controls.ItemContainerGenerator> と連動して、表示される項目の要素のみが作成されます。  
  
 <xref:System.Windows.Controls.VirtualizingStackPanel> 要素は、<xref:System.Windows.Controls.ListBox>などのコントロールの項目ホストとして自動的に設定されます。 データバインドされたコレクションをホストする場合、コンテンツが <xref:System.Windows.Controls.ScrollViewer>の境界内にある限り、コンテンツは自動的に仮想化されます。 これにより、多くの子項目をホストする際のパフォーマンスが大幅に向上します。  
  
 次のマークアップは、<xref:System.Windows.Controls.VirtualizingStackPanel> を項目ホストとして使用する方法を示しています。 仮想化を実行するには、<xref:System.Windows.Controls.VirtualizingStackPanel.IsVirtualizingProperty?displayProperty=nameWithType> 添付プロパティを `true` (既定値) に設定する必要があります。  
  
 [!code-xaml[VirtualizingStackPanel_Intro#1](~/samples/snippets/csharp/VS_Snippets_Wpf/VirtualizingStackPanel_Intro/CS/default.xaml#1)]  
  
<a name="Panels_overview_WrapPanel"></a>   
### <a name="wrappanel"></a>WrapPanel  
 <xref:System.Windows.Controls.WrapPanel> は、子要素を左から右へ順番に配置し、親コンテナーの端に達したときにコンテンツを次の行に分割するために使用されます。 コンテンツは水平方向または垂直方向に設定できます。 <xref:System.Windows.Controls.WrapPanel> は、単純なフロー [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] シナリオに役立ちます。 また、子要素すべてに均等なサイズを適用する際にも使用できます。  
  
 次の例では、コンテナーの端に達したときにラップする <xref:System.Windows.Controls.Button> コントロールを表示する <xref:System.Windows.Controls.WrapPanel> を作成する方法を示します。  
  
 [!code-cpp[WrapPanel_Intro#1](~/samples/snippets/cpp/VS_Snippets_Wpf/WrapPanel_Intro/CPP/WrapPanel_Code.cpp#1)]
 [!code-csharp[WrapPanel_Intro#1](~/samples/snippets/csharp/VS_Snippets_Wpf/WrapPanel_Intro/CSharp/WrapPanel_Code.cs#1)]
 [!code-vb[WrapPanel_Intro#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WrapPanel_Intro/VisualBasic/WrapPanel_vb.vb#1)]
 [!code-xaml[WrapPanel_Intro#1](~/samples/snippets/xaml/VS_Snippets_Wpf/WrapPanel_Intro/XAML/default.xaml#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![一般的な WrapPanel 要素。](./media/wrappanel-element.PNG "WrapPanel_Element")  
  
<a name="Panels_nested_panel_elements"></a>   
## <a name="nested-panel-elements"></a>入れ子になったパネル要素  
 複雑なレイアウトを生成するために、<xref:System.Windows.Controls.Panel> の要素を相互に入れ子にすることができます。 これは、1つの <xref:System.Windows.Controls.Panel> が [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]の一部に適しているが、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]の異なる部分のニーズを満たしていない場合に非常に便利です。  
  
 アプリケーションがサポートできる入れ子の量に決められた制限はありませんが、一般的に、目的のレイアウトに必要なパネルのみを使用するようアプリケーションを制限することをお勧めします。 多くの場合、レイアウトコンテナーとして柔軟性があるため、入れ子になったパネルの代わりに <xref:System.Windows.Controls.Grid> 要素を使用できます。 これにより、ツリーから不要な要素が排除され、アプリケーションのパフォーマンスが向上します。  
  
 次の例では、特定のレイアウトを実現するために、入れ子になった <xref:System.Windows.Controls.Panel> 要素を利用する [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を作成する方法を示します。 この場合は、<xref:System.Windows.Controls.DockPanel> 要素を使用して [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 構造体、入れ子になった <xref:System.Windows.Controls.StackPanel> 要素、<xref:System.Windows.Controls.Grid>、および <xref:System.Windows.Controls.Canvas> を使用して親 <xref:System.Windows.Controls.DockPanel>内で子要素を正確に配置します。  
  
 [!code-csharp[Nested_Panels#1](~/samples/snippets/csharp/VS_Snippets_Wpf/Nested_Panels/CSharp/nestedpanels.cs#1)]
 [!code-vb[Nested_Panels#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Nested_Panels/VisualBasic/nestedpanels.vb#1)]  
  
 コンパイルされたアプリケーションは、これと同様の新しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を生成します。  
  
 ![入れ子になったパネルを利用する UI。](./media/nested-panels.PNG "nested_panels")  
  
<a name="Panels_custom_panel_elements"></a>   
## <a name="custom-panel-elements"></a>カスタム パネル要素  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は柔軟なレイアウトコントロールの配列を提供しますが、<xref:System.Windows.FrameworkElement.ArrangeOverride%2A> メソッドと <xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドをオーバーライドすることによって、カスタムレイアウト動作を実現することもできます。 このオーバーライド メソッド内で新しい配置動作を定義することにより、カスタムのサイズ変更と配置が実行されます。  
  
 同様に、派生クラス (<xref:System.Windows.Controls.Canvas> や <xref:System.Windows.Controls.Grid>など) に基づくカスタムレイアウト動作は、その <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> および <xref:System.Windows.FrameworkElement.MeasureOverride%2A> メソッドをオーバーライドすることによって定義できます。  
  
 次のマークアップは、カスタム <xref:System.Windows.Controls.Panel> 要素を作成する方法を示しています。 この新しい <xref:System.Windows.Controls.Panel>は `PlotPanel`として定義されており、ハードコーディングされた*x*座標と*y*座標を使用した子要素の配置をサポートしています。 この例では、<xref:System.Windows.Shapes.Rectangle> 要素 (示されていません) はプロットポイント 50 (*x*) および 50 (*y*) に位置しています。  
  
 [!code-cpp[PlotPanel#1](~/samples/snippets/cpp/VS_Snippets_Wpf/PlotPanel/CPP/PlotPanel.cpp#1)]
 [!code-csharp[PlotPanel#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PlotPanel/CSharp/PlotPanel.cs#1)]
 [!code-vb[PlotPanel#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PlotPanel/VisualBasic/PlotPanel.vb#1)]  
  
 より複雑なカスタム パネルの実装を確認するには、[カスタム コンテンツ折り返しパネルの作成のサンプル](https://go.microsoft.com/fwlink/?LinkID=159979)を参照してください。  
  
<a name="Panels_global_localization"></a>   
## <a name="localizationglobalization-support"></a>ローカライズ/グローバリゼーション サポート  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、ローカライズ可能な [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の作成を支援する多数の機能をサポートしています。  
  
 すべてのパネル要素は、<xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティをネイティブでサポートしています。これを使用すると、ユーザーのロケールまたは言語設定に基づいてコンテンツを動的に再フローできます。 詳細については、<xref:System.Windows.FrameworkElement.FlowDirection%2A> を参照してください。  
  
 <xref:System.Windows.Window.SizeToContent%2A> プロパティは、アプリケーション開発者がローカライズされた [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]のニーズを予測できるようにするためのメカニズムを提供します。 親 <xref:System.Windows.Window> は、このプロパティの <xref:System.Windows.SizeToContent.WidthAndHeight> 値を使用して、常に動的にサイズを調整してコンテンツに合わせることができ、人為的な高さや幅の制限によって制約されることはありません。  
  
 <xref:System.Windows.Controls.DockPanel>、<xref:System.Windows.Controls.Grid>、および <xref:System.Windows.Controls.StackPanel> は、ローカライズ可能な [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]に適しています。 ただし、コンテンツは完全に配置されるため、ローカライズが困難になるため、<xref:System.Windows.Controls.Canvas> は適していません。  
  
 ローカライズ可能なユーザーインターフェイス (Ui) を使用して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションを作成する方法の詳細については、「[自動レイアウトの使用の概要](../advanced/use-automatic-layout-overview.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [チュートリアル: 初めての WPF デスクトップ アプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)
- [WPF レイアウト ギャラリー サンプル](https://go.microsoft.com/fwlink/?LinkID=160054)
- [レイアウト](../advanced/layout.md)
- [WPF Controls Gallery Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Getting%20Started/ControlsAndLayout)
- [配置、余白、パディングの概要](../advanced/alignment-margins-and-padding-overview.md)
- [カスタムコンテンツラッピングパネルサンプルの作成](https://go.microsoft.com/fwlink/?LinkID=159979)
- [添付プロパティの概要](../advanced/attached-properties-overview.md)
- [自動レイアウトの使用の概要](../advanced/use-automatic-layout-overview.md)
- [レイアウトとデザイン](../advanced/optimizing-performance-layout-and-design.md)
