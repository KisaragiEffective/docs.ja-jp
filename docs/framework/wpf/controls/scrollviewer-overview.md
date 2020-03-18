---
title: ScrollViewer の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [WPF], ScrollViewer
- ScrollViewer control [WPF], about ScrollViewer control
ms.assetid: 94a13b94-cfdf-4b12-a1aa-90cb50c6e9b9
ms.openlocfilehash: 993f3c861cbead88df3503eb01f18e5d1f69e2d8
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458435"
---
# <a name="scrollviewer-overview"></a>ScrollViewer の概要
ユーザー インターフェイス内のコンテンツは、多くの場合、コンピューター画面の表示領域よりも大きいです。 <xref:System.Windows.Controls.ScrollViewer> コントロールは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションのコンテンツのスクロールを有効にする便利な方法を提供します。 このトピックでは、<xref:System.Windows.Controls.ScrollViewer> 要素について説明し、いくつかの使用例を示します。  
  
<a name="what_is_a_scrollviewer_element"></a>   
## <a name="the-scrollviewer-control"></a>ScrollViewer コントロール  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでのスクロールを有効にする定義済みの要素には、<xref:System.Windows.Controls.Primitives.ScrollBar> と <xref:System.Windows.Controls.ScrollViewer>の2つがあります。 <xref:System.Windows.Controls.ScrollViewer> コントロールは、スクロール可能な領域に他の可視要素を表示するために、水平および垂直の <xref:System.Windows.Controls.Primitives.ScrollBar> 要素とコンテンツコンテナー (<xref:System.Windows.Controls.Panel> 要素など) をカプセル化します。 コンテンツのスクロールに <xref:System.Windows.Controls.Primitives.ScrollBar> 要素を使用するには、カスタムオブジェクトを作成する必要があります。 ただし、<xref:System.Windows.Controls.ScrollViewer> 要素は、<xref:System.Windows.Controls.Primitives.ScrollBar> の機能をカプセル化した複合コントロールであるため、単独で使用できます。  
  
 <xref:System.Windows.Controls.ScrollViewer> コントロールは、マウスとキーボードの両方のコマンドに応答し、事前に定義されたインクリメントでコンテンツをスクロールするために使用する多くのメソッドを定義します。 <xref:System.Windows.Controls.ScrollViewer.ScrollChanged> イベントを使用して、<xref:System.Windows.Controls.ScrollViewer> 状態の変更を検出できます。  
  
 <xref:System.Windows.Controls.ScrollViewer> には子を1つだけ設定できます。通常は、要素の <xref:System.Windows.Controls.Panel.Children%2A> コレクションをホストできる <xref:System.Windows.Controls.Panel> 要素です。 <xref:System.Windows.Controls.ContentPresenter.Content%2A> プロパティは、<xref:System.Windows.Controls.ScrollViewer>の唯一の子を定義します。  
  
<a name="scrollviewer_physical_vs_logical"></a>   
## <a name="physical-vs-logical-scrolling"></a>物理スクロールと論理スクロール  
 物理スクロールは、定義済みの物理インクリメント(通常ピクセル単位で宣言されている値) でコンテンツをスクロールするために使用します。 論理スクロールは、論理ツリーの次の項目にスクロールするために使用します。 ほとんどの <xref:System.Windows.Controls.Panel> 要素の既定のスクロール動作は、物理的なスクロールです。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]は両方の種類のスクロールをサポートしています。  
  
#### <a name="the-iscrollinfo-interface"></a>IScrollInfo インターフェイス  
 <xref:System.Windows.Controls.Primitives.IScrollInfo> インターフェイスは、<xref:System.Windows.Controls.ScrollViewer> または派生コントロール内のメインのスクロール領域を表します。 インターフェイスは、物理インクリメントではなく論理単位でのスクロールが必要な <xref:System.Windows.Controls.Panel> 要素によって実装できるスクロールプロパティとメソッドを定義します。 <xref:System.Windows.Controls.Primitives.IScrollInfo> のインスタンスを派生 <xref:System.Windows.Controls.Panel> にキャストした後、そのスクロールメソッドを使用すると、ピクセル単位のインクリメントではなく、子コレクション内の次の論理ユニットにスクロールできます。 既定では、<xref:System.Windows.Controls.ScrollViewer> コントロールは物理単位でのスクロールをサポートしています。  
  
 <xref:System.Windows.Controls.StackPanel> と <xref:System.Windows.Controls.VirtualizingStackPanel> はどちらも <xref:System.Windows.Controls.Primitives.IScrollInfo> を実装し、論理スクロールをネイティブでサポートしています。 論理スクロールをネイティブでサポートするレイアウトコントロールでは、ホスト <xref:System.Windows.Controls.Panel> 要素を <xref:System.Windows.Controls.ScrollViewer> でラップし、<xref:System.Windows.Controls.ScrollViewer.CanContentScroll%2A> プロパティを `false`に設定することによって、物理的なスクロールを実現できます。  
  
 次のコード例は、<xref:System.Windows.Controls.Primitives.IScrollInfo> のインスタンスを <xref:System.Windows.Controls.StackPanel> にキャストし、インターフェイスで定義されているコンテンツスクロールメソッド (<xref:System.Windows.Controls.Primitives.IScrollInfo.LineUp%2A> および <xref:System.Windows.Controls.Primitives.IScrollInfo.LineDown%2A>) を使用する方法を示しています。  
  
 [!code-csharp[IScrollInfoMethods#3](~/samples/snippets/csharp/VS_Snippets_Wpf/IScrollInfoMethods/CSharp/Window1.xaml.cs#3)]
 [!code-vb[IScrollInfoMethods#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IScrollInfoMethods/VisualBasic/Window1.xaml.vb#3)]  
  
<a name="scrollviewer_markup_syntax_and_sample"></a>   
## <a name="defining-and-using-a-scrollviewer-element"></a>ScrollViewer 要素の定義と使用  
 次の例では、テキストと四角形を含むウィンドウに <xref:System.Windows.Controls.ScrollViewer> を作成します。 <xref:System.Windows.Controls.Primitives.ScrollBar> 要素は、必要な場合にのみ表示されます。 ウィンドウのサイズを変更すると、<xref:System.Windows.Controls.ScrollViewer.ComputedHorizontalScrollBarVisibility%2A> と <xref:System.Windows.Controls.ScrollViewer.ComputedVerticalScrollBarVisibility%2A> プロパティの値が更新されるため、<xref:System.Windows.Controls.Primitives.ScrollBar> の要素が表示され、非表示になります。  
  
 [!code-cpp[ScrollViewer#1](~/samples/snippets/cpp/VS_Snippets_Wpf/ScrollViewer/CPP/ScrollViewer_wcp.cpp#1)]
 [!code-csharp[ScrollViewer#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ScrollViewer/CSharp/ScrollViewer_wcp.cs#1)]
 [!code-vb[ScrollViewer#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ScrollViewer/VisualBasic/ScrollViewer.vb#1)]
 [!code-xaml[ScrollViewer#1](~/samples/snippets/xaml/VS_Snippets_Wpf/ScrollViewer/XAML/Pane1.xaml#1)]  
  
<a name="scrollviewer_styling_scrollviewer"></a>   
## <a name="styling-a-scrollviewer"></a>ScrollViewer のスタイル設定  
 Windows Presentation Foundation のすべてのコントロールと同様に、コントロールの既定のレンダリング動作を変更するために、<xref:System.Windows.Controls.ScrollViewer> のスタイルを設定できます。 コントロールのスタイル設定の詳細については、「[スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。  
  
<a name="scrollviewer_scroll_vs_paginate"></a>   
## <a name="paginating-documents"></a>ドキュメントの改ページ調整  
 ドキュメントのコンテンツでは、スクロールする代わりに、改ページ調整をサポートするドキュメントのコンテナーを選択します。 <xref:System.Windows.Documents.FlowDocument> は、<xref:System.Windows.Controls.FlowDocumentPageViewer>などの表示コントロール内でホストされるように設計されたドキュメントを対象としており、複数のページ間でのコンテンツの改ページ位置の自動調整をサポートしているため、スクロールの必要がありません。 <xref:System.Windows.Controls.DocumentViewer> には、<xref:System.Windows.Documents.FixedDocument> コンテンツを表示するためのソリューションが用意されています。これは、従来のスクロールを使用して、表示領域の領域の外にコンテンツを表示します。  
  
 ドキュメント形式とプレゼンテーション オプションの詳細については、「[WPF のドキュメント](../advanced/documents-in-wpf.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Controls.ScrollViewer>
- <xref:System.Windows.Controls.Primitives.ScrollBar>
- <xref:System.Windows.Controls.Primitives.IScrollInfo>
- [方法: スクロールビューアーを作成する](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752352(v=vs.90))
- [WPF のドキュメント](../advanced/documents-in-wpf.md)
- [ScrollBar のスタイルとテンプレート](scrollbar-styles-and-templates.md)
- [コントロール](../advanced/optimizing-performance-controls.md)
