---
title: Controls
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], about WPF controls
ms.assetid: 3f255a8a-35a8-4712-9065-472ff7d75599
ms.openlocfilehash: 42596c8adf1b8b27f250fa7a3cf6cc173a63e2eb
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459449"
---
# <a name="controls"></a>Controls
<a name="introduction"></a>
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Controls.Label>、<xref:System.Windows.Controls.TextBox>、<xref:System.Windows.Controls.Menu>、<xref:System.Windows.Controls.ListBox>など、ほぼすべての Windows アプリケーションで使用される一般的な UI コンポーネントの多くが付属しています。 これまで、これらのオブジェクトはコントロールと呼ばれてきました。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] SDK は、アプリケーション内の表示されたオブジェクトを表すクラスを厳密には "コントロール" として使用しますが、クラスは、<xref:System.Windows.Controls.Control> クラスを継承して可視性を持つ必要がないことに注意することが重要です。 <xref:System.Windows.Controls.Control> クラスを継承するクラスには、<xref:System.Windows.Controls.ControlTemplate>が含まれています。これにより、コントロールのコンシューマーは、新しいサブクラスを作成しなくても、コントロールの外観を根本的に変更できます。  このトピックでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で一般的に使用されるコントロール (<xref:System.Windows.Controls.Control> クラスから継承されるコントロールとそれ以外のコントロールの両方) について説明します。  

<a name="creating_an_instance_of_a_control"></a>   
## <a name="creating-an-instance-of-a-control"></a>コントロールのインスタンスの作成  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] またはコードを使用して、アプリケーションにコントロールを追加できます。  次の例では、ユーザーに姓と名の入力を求める単純なアプリケーションの作成方法を示します。  この例では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]に2つのラベル、2つのテキストボックス、2つのボタンを作成します。 どのコントロールも同じ方法で作成できます。  
  
 [!code-xaml[ControlsOverview#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/Window1.xaml#1)]  
  
 次の例では、同じアプリケーションをコードで作成します。 簡潔にするために、<xref:System.Windows.Controls.Grid>、`grid1`の作成はサンプルから除外されています。 `grid1` には、前の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の例で示したのと同じ列と行の定義があります。  
  
 [!code-csharp[ControlsOverview#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml.cs#2)]
 [!code-vb[ControlsOverview#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ControlsOverview/VisualBasic/AppInCode.xaml.vb#2)]  
  
<a name="changing_the_appearance_of_a_control"></a>   
## <a name="changing-the-appearance-of-a-control"></a>コントロールの外観の変更  
 通常は、アプリケーションの外観に合わせてコントロールの外観を変更します。 コントロールの外観を変更するには、目的に応じて、次のいずれかの手順を実行します。  
  
- コントロールのプロパティ値を変更します。  
  
- コントロールの <xref:System.Windows.Style> を作成します。  
  
- コントロールの新しい <xref:System.Windows.Controls.ControlTemplate> を作成します。  
  
### <a name="changing-a-controls-property-value"></a>コントロールのプロパティ値の変更  
 多くのコントロールには、<xref:System.Windows.Controls.Button>の <xref:System.Windows.Controls.Control.Background%2A> など、コントロールの表示方法を変更できるプロパティがあります。 値のプロパティは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] とコードの両方で設定できます。 次の例では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]の <xref:System.Windows.Controls.Button> に対して、<xref:System.Windows.Controls.Control.Background%2A>、<xref:System.Windows.Controls.Control.FontSize%2A>、および <xref:System.Windows.Controls.Control.FontWeight%2A> の各プロパティを設定します。  
  
 [!code-xaml[ControlsOverview#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml#3)]  
  
 次の例では、同じプロパティをコードで設定しています。  
  
 [!code-csharp[ControlsOverview#4](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml.cs#4)]
 [!code-vb[ControlsOverview#4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ControlsOverview/VisualBasic/AppInCode.xaml.vb#4)]  
  
### <a name="creating-a-style-for-a-control"></a>コントロールのスタイル作成  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用すると、<xref:System.Windows.Style>を作成することによって、アプリケーションの各インスタンスにプロパティを設定するのではなく、コントロールの外観を指定できます。 次の例では、アプリケーションの各 <xref:System.Windows.Controls.Button> に適用される <xref:System.Windows.Style> を作成します。 <xref:System.Windows.Style> 定義は通常、<xref:System.Windows.ResourceDictionary>内の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で定義されます (<xref:System.Windows.FrameworkElement>の <xref:System.Windows.FrameworkElement.Resources%2A> プロパティなど)。  
  
 [!code-xaml[ControlsOverview#5](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml#5)]  
  
 また、スタイルにキーを割り当て、コントロールの `Style` プロパティでそのキーを指定することによって、特定の種類の特定のコントロールにのみスタイルを適用することもできます。  スタイルの詳細については、「スタイル[とテンプレート](styling-and-templating.md)」を参照してください。  
  
### <a name="creating-a-controltemplate"></a>ControlTemplate の作成  
 <xref:System.Windows.Style> を使用すると、一度に複数のコントロールのプロパティを設定できますが、<xref:System.Windows.Style>を作成することにより、<xref:System.Windows.Controls.Control> の外観をカスタマイズすることもできます。 <xref:System.Windows.Controls.Control> クラスを継承するクラスには、<xref:System.Windows.Controls.Control>の構造と外観を定義する <xref:System.Windows.Controls.ControlTemplate>があります。 <xref:System.Windows.Controls.Control> の <xref:System.Windows.Controls.Control.Template%2A> プロパティはパブリックであるため、<xref:System.Windows.Controls.Control> に既定とは異なる <xref:System.Windows.Controls.ControlTemplate> を与えることができます。 多くの場合、<xref:System.Windows.Controls.Control>の外観をカスタマイズするためにコントロールから継承するのではなく、<xref:System.Windows.Controls.Control> の新しい <xref:System.Windows.Controls.ControlTemplate> を指定できます。  
  
 非常に一般的なコントロールである <xref:System.Windows.Controls.Button>を考えてみましょう。  <xref:System.Windows.Controls.Button> の主な動作は、ユーザーがクリックしたときにアプリケーションが何らかのアクションを実行できるようにすることです。  既定では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の <xref:System.Windows.Controls.Button> は、浮き出した四角形として表示されます。  アプリケーションの開発中に、ボタンのクリックイベントを処理することによって、<xref:System.Windows.Controls.Button>の動作を利用することができますが、ボタンのプロパティを変更することで実行できる操作以外にボタンの外観を変更することもできます。  この場合は、新しい <xref:System.Windows.Controls.ControlTemplate>を作成できます。  
  
 次の例では、<xref:System.Windows.Controls.Button>の <xref:System.Windows.Controls.ControlTemplate> を作成します。  <xref:System.Windows.Controls.ControlTemplate> は、角が丸く、グラデーションの背景が <xref:System.Windows.Controls.Button> を作成します。  <xref:System.Windows.Controls.ControlTemplate> には、<xref:System.Windows.Controls.Border.Background%2A> が2つの <xref:System.Windows.Media.GradientStop> オブジェクトを持つ <xref:System.Windows.Media.LinearGradientBrush> である <xref:System.Windows.Controls.Border> が含まれています。  最初の <xref:System.Windows.Media.GradientStop> では、データバインディングを使用して、<xref:System.Windows.Media.GradientStop> の <xref:System.Windows.Media.GradientStop.Color%2A> プロパティをボタンの背景の色にバインドします。  <xref:System.Windows.Controls.Button>の [<xref:System.Windows.Controls.Control.Background%2A>] プロパティを設定すると、その値の色が最初の <xref:System.Windows.Media.GradientStop>として使用されます。 データ バインディングの詳細については、「[データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。 この例では、<xref:System.Windows.Controls.Primitives.ButtonBase.IsPressed%2A> が `true`されたときの <xref:System.Windows.Controls.Button> の外観を変更する <xref:System.Windows.Trigger> も作成します。  
  
 [!code-xaml[ControlsOverview#6](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/Window1.xaml#6)]  
[!code-xaml[ControlsOverview#7](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml#7)]  
  
> [!NOTE]
> 例を正しく動作させるには、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> プロパティを <xref:System.Windows.Media.SolidColorBrush> に設定する必要があります。  
  
<a name="subscribing_to_events"></a>   
## <a name="subscribing-to-events"></a>イベントのサブスクライブ  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] またはコードを使用して、コントロールのイベントをサブスクライブできますが、イベントを処理できるのはコードのみです。  次の例は、<xref:System.Windows.Controls.Button>の `Click` イベントをサブスクライブする方法を示しています。  
  
 [!code-xaml[ControlsOverview#10](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/Window1.xaml#10)]  
  
 [!code-csharp[ControlsOverview#8](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml.cs#8)]
 [!code-vb[ControlsOverview#8](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ControlsOverview/VisualBasic/AppInCode.xaml.vb#8)]  
  
 次の例では、<xref:System.Windows.Controls.Button>の `Click` イベントを処理します。  
  
 [!code-csharp[ControlsOverview#9](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlsOverview/CSharp/AppInCode.xaml.cs#9)]
 [!code-vb[ControlsOverview#9](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ControlsOverview/VisualBasic/AppInCode.xaml.vb#9)]  
  
<a name="rich_content_in_controls"></a>   
## <a name="rich-content-in-controls"></a>コントロールでのリッチ コンテンツ  
 <xref:System.Windows.Controls.Control> クラスを継承するほとんどのクラスには、豊富なコンテンツを格納できる容量があります。 たとえば、<xref:System.Windows.Controls.Label> には、文字列、<xref:System.Windows.Controls.Image>、<xref:System.Windows.Controls.Panel>など、任意のオブジェクトを含めることができます。  次のクラスは、リッチコンテンツをサポートし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]のほとんどのコントロールの基本クラスとして機能します。  
  
- <xref:System.Windows.Controls.ContentControl>--このクラスを継承するクラスの例として、<xref:System.Windows.Controls.Label>、<xref:System.Windows.Controls.Button>、および <xref:System.Windows.Controls.ToolTip>があります。  
  
- <xref:System.Windows.Controls.ItemsControl>--このクラスを継承するクラスの例として、<xref:System.Windows.Controls.ListBox>、<xref:System.Windows.Controls.Menu>、および <xref:System.Windows.Controls.Primitives.StatusBar>があります。  
  
- <xref:System.Windows.Controls.HeaderedContentControl>--このクラスを継承するクラスの例として、<xref:System.Windows.Controls.TabItem>、<xref:System.Windows.Controls.GroupBox>、および <xref:System.Windows.Controls.Expander>があります。  
  
- <xref:System.Windows.Controls.HeaderedItemsControl>--このクラスを継承するクラスの例として、<xref:System.Windows.Controls.MenuItem>、<xref:System.Windows.Controls.TreeViewItem>、および <xref:System.Windows.Controls.ToolBar>があります。  

 これらの基本クラスの詳細については、「 [WPF コンテンツモデル](wpf-content-model.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [スタイルとテンプレート](styling-and-templating.md)
- [カテゴリ別のコントロール](controls-by-category.md)
- [コントロール ライブラリ](control-library.md)
- [データ テンプレートの概要](../data/data-templating-overview.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [入力](../advanced/input-wpf.md)
- [コマンドを有効にする](../advanced/how-to-enable-a-command.md)
- [チュートリアル: カスタム アニメーション ボタンの作成](walkthroughs-create-a-custom-animated-button.md)
- [コントロールのカスタマイズ](control-customization.md)
