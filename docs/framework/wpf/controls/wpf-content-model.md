---
title: コンテンツ モデル
ms.date: 03/30/2017
helpviewer_keywords:
- UIElement class [WPF], displaying content
- content model [WPF], controls
- controls [WPF], displaying text
- content display [WPF], controls
- controls [WPF], formatting text
- displaying content [WPF]
- arbitrary content classes [WPF], content model
- ContentControl class [WPF], displaying content
ms.assetid: 214da5ef-547a-4cf8-9b07-4aa8a0e52cdd
ms.openlocfilehash: a84ab2e66b4e373591fc9365b1c17d0bb0c66713
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738273"
---
# <a name="wpf-content-model"></a>WPF のコンテンツ モデル
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、多くのコントロールやコントロールのような型を提供する表示プラットフォームで、その主な目的は、異なる種類のコンテンツを表示することです。 使用するコントロールまたは派生元のコントロールを判断するには、特定のコントロールが最適に表示できるオブジェクトの種類を理解する必要があります。  
  
 このトピックは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールとコントロールのような型に関するコンテンツ モデルのまとめです。 コンテンツ モデルは、どのようなコンテンツをコントロールで使用できるかについて説明します。 このトピックは、各コンテンツ モデルのコンテンツのプロパティもリストします。 コンテンツのプロパティは、オブジェクトのコンテンツの格納に使用されるプロパティです。  

<a name="classes_that_contain_arbitrary_content"></a>   
## <a name="classes-that-contain-arbitrary-content"></a>任意のコンテンツを含むクラス  
 一部のコントロールには、文字列、<xref:System.DateTime> オブジェクト、追加項目のコンテナーである <xref:System.Windows.UIElement> など、任意の型のオブジェクトを含めることができます。 たとえば、<xref:System.Windows.Controls.Button> にはイメージとテキストを含めることができます。または、<xref:System.Windows.Controls.CheckBox> に <xref:System.DateTime.Now%2A?displayProperty=nameWithType>の値を含めることができます。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、任意のコンテンツを含めることができる 4 つのクラスがあります。 次の表に、<xref:System.Windows.Controls.Control>から継承するクラスの一覧を示します。  
  
|任意のコンテンツを含むクラス|コンテンツ|  
|-------------------------------------------|-------------|  
|<xref:System.Windows.Controls.ContentControl>|1 つの任意のオブジェクト。|  
|<xref:System.Windows.Controls.HeaderedContentControl>|ヘッダーと 1 つの項目。両方とも任意のオブジェクトです。|  
|<xref:System.Windows.Controls.ItemsControl>|任意のオブジェクトのコレクション。|  
|<xref:System.Windows.Controls.HeaderedItemsControl>|ヘッダーと項目のコレクション。すべて任意のオブジェクトです。|  
  
 これらのクラスから継承するコントロールは、同じ種類のコンテンツを格納でき、同じ方法でコンテンツを処理することができます。 次の図は、イメージとテキストを含む各コンテンツモデルの1つのコントロールを示しています。  
  
 ![各コンテンツモデルの4つの異なるコントロールを示すスクリーンショット。](./media/wpf-content-model/control-content-model-image-text.png)  
  
### <a name="controls-that-contain-a-single-arbitrary-object"></a>任意の 1 つのオブジェクトを格納しているコントロール  
 <xref:System.Windows.Controls.ContentControl> クラスには、任意のコンテンツの1つの部分が含まれています。 そのコンテンツプロパティは <xref:System.Windows.Controls.ContentControl.Content%2A>です。 次のコントロールは <xref:System.Windows.Controls.ContentControl> から継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Controls.Button>  
  
- <xref:System.Windows.Controls.Primitives.ButtonBase>  
  
- <xref:System.Windows.Controls.CheckBox>  
  
- <xref:System.Windows.Controls.ComboBoxItem>  
  
- <xref:System.Windows.Controls.ContentControl>  
  
- <xref:System.Windows.Controls.Frame>  
  
- <xref:System.Windows.Controls.GridViewColumnHeader>  
  
- <xref:System.Windows.Controls.GroupItem>  
  
- <xref:System.Windows.Controls.Label>  
  
- <xref:System.Windows.Controls.ListBoxItem>  
  
- <xref:System.Windows.Controls.ListViewItem>  
  
- <xref:System.Windows.Navigation.NavigationWindow>  
  
- <xref:System.Windows.Controls.RadioButton>  
  
- <xref:System.Windows.Controls.Primitives.RepeatButton>  
  
- <xref:System.Windows.Controls.ScrollViewer>  
  
- <xref:System.Windows.Controls.Primitives.StatusBarItem>  
  
- <xref:System.Windows.Controls.Primitives.ToggleButton>  
  
- <xref:System.Windows.Controls.ToolTip>  
  
- <xref:System.Windows.Controls.UserControl>  
  
- <xref:System.Windows.Window>  
  
 次の図は、<xref:System.Windows.Controls.ContentControl.Content%2A> が文字列、<xref:System.DateTime> オブジェクト、<xref:System.Windows.Shapes.Rectangle>、および <xref:System.Windows.Shapes.Ellipse> と <xref:System.Windows.Controls.TextBlock>を含む <xref:System.Windows.Controls.Panel> に設定されている4つのボタンを示しています。  
  
 ![コンテンツの種類が異なる4つのボタンを表示するスクリーンショット。](./media/wpf-content-model/control-content-model-buttons.png)  
  
 <xref:System.Windows.Controls.ContentControl.Content%2A> プロパティの設定方法の例については、「<xref:System.Windows.Controls.ContentControl>」を参照してください。  
  
### <a name="controls-that-contain-a-header-and-a-single-arbitrary-object"></a>ヘッダーと任意の 1 つのオブジェクトを格納しているコントロール  
 <xref:System.Windows.Controls.HeaderedContentControl> クラスは <xref:System.Windows.Controls.ContentControl> から継承し、ヘッダー付きでコンテンツを表示します。 コンテンツプロパティ <xref:System.Windows.Controls.ContentControl.Content%2A>を <xref:System.Windows.Controls.ContentControl> から継承し、<xref:System.Object>型の <xref:System.Windows.Controls.HeaderedContentControl.Header%2A> プロパティを定義します。そのため、どちらも任意のオブジェクトにすることができます。  
  
 次のコントロールは <xref:System.Windows.Controls.HeaderedContentControl> から継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Controls.Expander>  
  
- <xref:System.Windows.Controls.GroupBox>  
  
- <xref:System.Windows.Controls.TabItem>  
  
 次の図は、2つの <xref:System.Windows.Controls.TabItem> オブジェクトを示しています。 最初の <xref:System.Windows.Controls.TabItem> には、<xref:System.Windows.Controls.HeaderedContentControl.Header%2A> と <xref:System.Windows.Controls.ContentControl.Content%2A>として <xref:System.Windows.UIElement> オブジェクトがあります。 <xref:System.Windows.Controls.HeaderedContentControl.Header%2A> は、<xref:System.Windows.Shapes.Ellipse> と <xref:System.Windows.Controls.TextBlock>を含む <xref:System.Windows.Controls.StackPanel> に設定されます。 <xref:System.Windows.Controls.ContentControl.Content%2A> は、<xref:System.Windows.Controls.TextBlock> と <xref:System.Windows.Controls.Label>を含む <xref:System.Windows.Controls.StackPanel> に設定されます。 2番目の <xref:System.Windows.Controls.TabItem> には、<xref:System.Windows.Controls.HeaderedContentControl.Header%2A> 内の文字列と <xref:System.Windows.Controls.ContentControl.Content%2A>内の <xref:System.Windows.Controls.TextBlock> があります。  
  
 ![ヘッダープロパティの異なる型を使用する TabControl。](./media/wpf-content-model/control-content-model-tab.png)  
  
 <xref:System.Windows.Controls.TabItem> オブジェクトを作成する方法の例については、「<xref:System.Windows.Controls.HeaderedContentControl>」を参照してください。  
  
### <a name="controls-that-contain-a-collection-of-arbitrary-objects"></a>任意のオブジェクトのコレクションを格納しているコントロール  
 <xref:System.Windows.Controls.ItemsControl> クラスは <xref:System.Windows.Controls.Control> から継承され、複数の項目 (文字列、オブジェクト、その他の要素など) を含むことができます。 そのコンテンツのプロパティは、<xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> と <xref:System.Windows.Controls.ItemsControl.Items%2A>です。 <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> は、通常、<xref:System.Windows.Controls.ItemsControl> にデータコレクションを設定するために使用されます。 コレクションを使用して <xref:System.Windows.Controls.ItemsControl>を設定しない場合は、<xref:System.Windows.Controls.ItemsControl.Items%2A> プロパティを使用して項目を追加できます。  
  
 次のコントロールは <xref:System.Windows.Controls.ItemsControl> から継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Controls.Menu>  
  
- <xref:System.Windows.Controls.Primitives.MenuBase>  
  
- <xref:System.Windows.Controls.ContextMenu>  
  
- <xref:System.Windows.Controls.ComboBox>  
  
- <xref:System.Windows.Controls.ItemsControl>  
  
- <xref:System.Windows.Controls.ListBox>  
  
- <xref:System.Windows.Controls.ListView>  
  
- <xref:System.Windows.Controls.TabControl>  
  
- <xref:System.Windows.Controls.TreeView>  
  
- <xref:System.Windows.Controls.Primitives.Selector>  
  
- <xref:System.Windows.Controls.Primitives.StatusBar>  
  
 次の図は、これらの種類の項目を含む <xref:System.Windows.Controls.ListBox> を示しています。  
  
- 文字列。  
  
- <xref:System.DateTime> オブジェクト。  
  
- <xref:System.Windows.UIElement> です。  
  
- <xref:System.Windows.Shapes.Ellipse> と <xref:System.Windows.Controls.TextBlock>を格納している <xref:System.Windows.Controls.Panel>。  
  
 ![4種類のコンテンツを含む ListBox を示すスクリーンショット。](./media/wpf-content-model/control-content-model-listbox.png)  
  
### <a name="controls-that-contain-a-header-and-a-collection-of-arbitrary-objects"></a>ヘッダーと任意のオブジェクトのコレクションを含むコントロール  
 <xref:System.Windows.Controls.HeaderedItemsControl> クラスは <xref:System.Windows.Controls.ItemsControl> から継承され、複数の項目 (文字列、オブジェクト、その他の要素、ヘッダーなど) を含めることができます。 <xref:System.Windows.Controls.ItemsControl> コンテンツのプロパティ、<xref:System.Windows.Controls.ItemsControl.ItemsSource%2A>、および <xref:System.Windows.Controls.ItemsControl.Items%2A>を継承し、任意のオブジェクトにすることができる <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> プロパティを定義します。  
  
 次のコントロールは <xref:System.Windows.Controls.HeaderedItemsControl> から継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Controls.MenuItem>  
  
- <xref:System.Windows.Controls.ToolBar>  
  
- <xref:System.Windows.Controls.TreeViewItem>  
  
<a name="classes_that_contain_a_collection_of_uielement_objects"></a>   
## <a name="classes-that-contain-a-collection-of-uielement-objects"></a>UIElement オブジェクトのコレクションを含むクラス  
 <xref:System.Windows.Controls.Panel> クラスは、子 <xref:System.Windows.UIElement> オブジェクトを配置して整列します。 そのコンテンツプロパティは <xref:System.Windows.Controls.Panel.Children%2A>です。  
  
 次のクラスは、<xref:System.Windows.Controls.Panel> クラスを継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Controls.Canvas>  
  
- <xref:System.Windows.Controls.DockPanel>  
  
- <xref:System.Windows.Controls.Grid>  
  
- <xref:System.Windows.Controls.Primitives.TabPanel>  
  
- <xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel>  
  
- <xref:System.Windows.Controls.Primitives.ToolBarPanel>  
  
- <xref:System.Windows.Controls.Primitives.UniformGrid>  
  
- <xref:System.Windows.Controls.StackPanel>  
  
- <xref:System.Windows.Controls.VirtualizingPanel>  
  
- <xref:System.Windows.Controls.VirtualizingStackPanel>  
  
- <xref:System.Windows.Controls.WrapPanel>  
  
 詳細については、「[パネルの概要](panels-overview.md)」を参照してください。  
  
<a name="classes_that_affects_the_appearance_of_a_uielement"></a>   
## <a name="classes-that-affect-the-appearance-of-a-uielement"></a>UIElement の外観に影響を与えるクラス  
 <xref:System.Windows.Controls.Decorator> クラスは、1つの子 <xref:System.Windows.UIElement>に視覚効果を適用します。 そのコンテンツプロパティは <xref:System.Windows.Controls.Decorator.Child%2A>です。 次のクラスは <xref:System.Windows.Controls.Decorator> から継承し、そのコンテンツモデルを使用します。  
  
- <xref:System.Windows.Documents.AdornerDecorator>  
  
- <xref:System.Windows.Controls.Border>  
  
- <xref:System.Windows.Controls.Primitives.BulletDecorator>  
  
- <xref:Microsoft.Windows.Themes.ButtonChrome>  
  
- <xref:Microsoft.Windows.Themes.ClassicBorderDecorator>  
  
- <xref:System.Windows.Controls.InkPresenter>  
  
- <xref:Microsoft.Windows.Themes.ListBoxChrome>  
  
- <xref:Microsoft.Windows.Themes.SystemDropShadowChrome>  
  
- <xref:System.Windows.Controls.Viewbox>  
  
 次の図は、その周囲の <xref:System.Windows.Controls.Border> を (で修飾) <xref:System.Windows.Controls.TextBox> を示しています。  
  
 ![境界線が黒の TextBox](./media/layout-border-around-textbox.png "Layout_Border_around_TextBox")  
境界がある TextBlock  
  
<a name="classes_that_provides_visual_feedback_about_a_uielement"></a>   
## <a name="classes-that-provide-visual-feedback-about-a-uielement"></a>UIElement についての視覚的なフィードバックを提供するクラス  
 <xref:System.Windows.Documents.Adorner> クラスは、ユーザーに視覚的な手掛かりを提供します。 たとえば、<xref:System.Windows.Documents.Adorner> を使用して、要素に機能ハンドルを追加したり、コントロールに関する状態情報を提供したりします。 <xref:System.Windows.Documents.Adorner> クラスは、独自の装飾を作成するためのフレームワークを提供します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は実装された装飾は提供しません。 詳しくは、[Adorners Overview](adorners-overview.md)をご覧ください。  
  
<a name="classes_that_enable_users_to_enter_text"></a>   
## <a name="classes-that-enable-users-to-enter-text"></a>ユーザーがテキストを入力できるようにするクラス  
 WPF は、ユーザーがテキストを入力できるようにする 3 つの主なコントロールを提供します。 各コントロールは、異なる方法で、テキストを表示します。 次の表は、これら 3 つのテキスト関連のコントロール、テキストを表示するときのそれぞれの機能、およびコントロールのテキストを格納するそれぞれのプロパティの一覧です。  
  
|コントロール|テキストの表示形態|コンテンツのプロパティ|  
|-------------|--------------------------|----------------------|  
|<xref:System.Windows.Controls.TextBox>|プレーンテキスト ファイル|<xref:System.Windows.Controls.TextBox.Text%2A>|  
|<xref:System.Windows.Controls.RichTextBox>|書式付きテキスト|<xref:System.Windows.Controls.RichTextBox.Document%2A>|  
|<xref:System.Windows.Controls.PasswordBox>|非表示のテキスト (文字はマスクされます)|<xref:System.Windows.Controls.PasswordBox.Password%2A>|  
  
<a name="classes_that_display_text"></a>   
## <a name="classes-that-display-your-text"></a>テキストを表示するクラス  
 いくつかのクラスを使用して、プレーン テキストまたは書式設定されたテキストを表示できます。 <xref:System.Windows.Controls.TextBlock> を使用すると、少量のテキストを表示できます。 大量のテキストを表示する場合は、<xref:System.Windows.Controls.FlowDocumentReader>、<xref:System.Windows.Controls.FlowDocumentPageViewer>、または <xref:System.Windows.Controls.FlowDocumentScrollViewer> コントロールを使用します。  
  
 <xref:System.Windows.Controls.TextBlock> には、<xref:System.Windows.Controls.TextBlock.Text%2A> と <xref:System.Windows.Controls.TextBlock.Inlines%2A>の2つのコンテンツプロパティがあります。 一貫した書式設定を使用するテキストを表示する場合は、多くの場合、<xref:System.Windows.Controls.TextBlock.Text%2A> プロパティを選択することをお勧めします。 テキスト全体で異なる書式設定を使用する場合は、<xref:System.Windows.Controls.TextBlock.Inlines%2A> プロパティを使用します。 <xref:System.Windows.Controls.TextBlock.Inlines%2A> プロパティは、テキストの書式設定方法を指定する <xref:System.Windows.Documents.Inline> オブジェクトのコレクションです。  
  
 次の表は、<xref:System.Windows.Controls.FlowDocumentReader>、<xref:System.Windows.Controls.FlowDocumentPageViewer>、および <xref:System.Windows.Controls.FlowDocumentScrollViewer> クラスのコンテンツプロパティを示しています。  
  
|コントロール|コンテンツのプロパティ|コンテンツのプロパティの型|  
|-------------|----------------------|---------------------------|  
|<xref:System.Windows.Controls.FlowDocumentPageViewer>|ドキュメント|<xref:System.Windows.Documents.IDocumentPaginatorSource>|  
|<xref:System.Windows.Controls.FlowDocumentReader>|ドキュメント|<xref:System.Windows.Documents.FlowDocument>|  
|<xref:System.Windows.Controls.FlowDocumentScrollViewer>|ドキュメント|<xref:System.Windows.Documents.FlowDocument>|  
  
 <xref:System.Windows.Documents.FlowDocument> は <xref:System.Windows.Documents.IDocumentPaginatorSource> インターフェイスを実装します。したがって、3つのクラスはすべて、<xref:System.Windows.Documents.FlowDocument> をコンテンツとして受け取ることができます。  
  
<a name="classes_that_format_text"></a>   
## <a name="classes-that-format-your-text"></a>テキストを書式設定するクラス  
 <xref:System.Windows.Documents.TextElement> とその関連クラスを使用すると、テキストの書式を設定できます。 <xref:System.Windows.Documents.TextElement> オブジェクトは、<xref:System.Windows.Controls.TextBlock> オブジェクトと <xref:System.Windows.Documents.FlowDocument> オブジェクトのテキストを格納および書式設定します。 <xref:System.Windows.Documents.TextElement> オブジェクトの2つの主な種類は、<xref:System.Windows.Documents.Block> 要素と <xref:System.Windows.Documents.Inline> 要素です。 <xref:System.Windows.Documents.Block> 要素は、段落やリストなどのテキストのブロックを表します。 <xref:System.Windows.Documents.Inline> 要素は、ブロック内のテキストの一部を表します。 多くの <xref:System.Windows.Documents.Inline> クラスは、適用先のテキストの書式を指定します。 各 <xref:System.Windows.Documents.TextElement> には、独自のコンテンツモデルがあります。 詳細については、「[TextElement Content Model Overview](../advanced/textelement-content-model-overview.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [詳細](../advanced/index.md)
