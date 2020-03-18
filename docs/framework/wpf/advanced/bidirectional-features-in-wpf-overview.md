---
title: 双方向機能の概要
ms.date: 03/30/2017
helpviewer_keywords:
- Span elements [WPF]
- bidirectional features [WPF]
ms.assetid: fd850e25-7dba-408c-b521-8873e51dc968
ms.openlocfilehash: 17167b1afa5037998d9ea8ed679a66c89babe242
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741635"
---
# <a name="bidirectional-features-in-wpf-overview"></a>WPF の双方向機能の概要

他の開発プラットフォームとは異なり、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、双方向のコンテンツの迅速な開発をサポートする多数の機能があります。たとえば、左から右へ、および同じドキュメント内の右から左へのデータを混在させることができます。 同時に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、アラビア語やヘブライ語圏のユーザーなどの双方向機能を必要とするユーザーに優れたエクスペリエンスを提供します。

以降のセクションでは、多数の双方向機能と双方向コンテンツを最適に表示した例について説明します。 ほとんどのサンプルでは XAML を使用しますが、または Microsoft C# Visual Basic コードに簡単に概念を適用できます。

<a name="FlowDirection"></a>

## <a name="flowdirection"></a>FlowDirection

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでコンテンツフローの方向を定義する基本プロパティは <xref:System.Windows.FrameworkElement.FlowDirection%2A>。 このプロパティは、<xref:System.Windows.FlowDirection.LeftToRight> または <xref:System.Windows.FlowDirection.RightToLeft>の2つの列挙値のいずれかに設定できます。 プロパティは、<xref:System.Windows.FrameworkElement>から継承されるすべての [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素で使用できます。

次の例では、<xref:System.Windows.Controls.TextBox> 要素のフロー方向を設定します。

**左から右へ記述するフロー方向**

[!code-xaml[LTRRTL#LTR](~/samples/snippets/csharp/VS_Snippets_Wpf/LTRRTL/CS/Pane1.xaml#ltr)]

**右から左へ記述するフロー方向**

[!code-xaml[LTRRTL#RTL](~/samples/snippets/csharp/VS_Snippets_Wpf/LTRRTL/CS/Pane1.xaml#rtl)]

次の図は、前のコードがどのように表示されるのかを示します。

![さまざまなフロー方向を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/left-right-right-left.png)

[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] ツリー内の要素は、そのコンテナーから <xref:System.Windows.FrameworkElement.FlowDirection%2A> を継承します。 次の例では、<xref:System.Windows.Controls.TextBlock> は <xref:System.Windows.Window>に存在する <xref:System.Windows.Controls.Grid>内にあります。 <xref:System.Windows.Window> の <xref:System.Windows.FrameworkElement.FlowDirection%2A> を設定することは、<xref:System.Windows.Controls.Grid> と <xref:System.Windows.Controls.TextBlock> にも設定することを意味します。

<xref:System.Windows.FrameworkElement.FlowDirection%2A>の設定例を次に示します。

[!code-xaml[FlowDirection#FlowDirection](~/samples/snippets/csharp/VS_Snippets_Wpf/FlowDirection/CS/Window1.xaml#flowdirection)]

最上位レベルの <xref:System.Windows.Window> には <xref:System.Windows.FlowDirection.RightToLeft><xref:System.Windows.FlowDirection>があるため、その中に含まれるすべての要素も同じ <xref:System.Windows.FrameworkElement.FlowDirection%2A>を継承します。 要素が指定された <xref:System.Windows.FrameworkElement.FlowDirection%2A> をオーバーライドするには、前の例の2番目の <xref:System.Windows.Controls.TextBlock> のように、<xref:System.Windows.FlowDirection.LeftToRight>に変更する明示的な方向の変更を追加する必要があります。 <xref:System.Windows.FrameworkElement.FlowDirection%2A> が定義されていない場合は、既定の <xref:System.Windows.FlowDirection.LeftToRight> が適用されます。

次の図は、前の例の出力を示しています。

![明示的なフロー方向の変更を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/explicit-direction-change.png)

<a name="FlowDocument"></a>

## <a name="flowdocument"></a>FlowDocument

HTML、Win32、Java などの多くの開発プラットフォームでは、双方向のコンテンツ開発に対して特別なサポートを提供しています。 HTML などのマークアップ言語では、必要な方向にテキストを表示するために必要なマークアップがコンテンツライターに与えられます。たとえば、HTML 4.0 タグでは、"rtl" または "ltr" を値として受け取ります。 このタグは <xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティに似ていますが、<xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティは、テキストコンテンツをレイアウトするより高度な方法で動作し、テキスト以外のコンテンツに使用できます。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、<xref:System.Windows.Documents.FlowDocument> は、テキスト、テーブル、画像、およびその他の要素の組み合わせをホストできる、汎用性のある [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素です。 以下のセクションのサンプルでは、この要素を使用します。

<xref:System.Windows.Documents.FlowDocument> へのテキストの追加は、1つの方法で行うことができます。 これを行う簡単な方法は、テキストなどのコンテンツをグループ化するために使用されるブロックレベルの要素である <xref:System.Windows.Documents.Paragraph> を使用することです。 インラインレベルの要素にテキストを追加するには、サンプルで <xref:System.Windows.Documents.Span> と <xref:System.Windows.Documents.Run>を使用します。 <xref:System.Windows.Documents.Span> は、他のインライン要素のグループ化に使用されるインラインレベルのフローコンテンツ要素であり、<xref:System.Windows.Documents.Run> は、書式設定されていないテキストの実行を格納するためのインラインレベルのフローコンテンツ要素です。 <xref:System.Windows.Documents.Span> には、複数の <xref:System.Windows.Documents.Run> 要素を含めることができます。

最初のドキュメントの例には、多数のネットワーク共有名を持つドキュメントが含まれています。たとえば、`\\server1\folder\file.ext`です。 このネットワーク リンクがアラビア語または英語のいずれのドキュメントにあっても、常に同じように表示する必要があります。 次の図は、Span 要素を使用して、アラビア <xref:System.Windows.FlowDirection.RightToLeft> ドキュメントにリンクを示しています。

![Span 要素の使用方法を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/flow-direction-span-element.png "FlowDocument")

テキストが <xref:System.Windows.FlowDirection.RightToLeft>ので、"\\" などのすべての特殊文字は、テキストを右から左の順序で区切ります。 その結果、リンクが正しい順序で表示されなくなるため、問題を解決するには、別の <xref:System.Windows.Documents.Run> フロー <xref:System.Windows.FlowDirection.LeftToRight>を保持するためにテキストを埋め込む必要があります。 この問題を解決するには、言語ごとに個別の <xref:System.Windows.Documents.Run> を使用するのではなく、使用頻度の低い英語のテキストをより大きなアラビア語の <xref:System.Windows.Documents.Span>に埋め込む方法が適しています。

次の図は、Span 要素に埋め込まれた Run 要素を使用してこれを示しています。

![Span 要素に埋め込まれている Run 要素を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/embedded-span-element.png)

次の例では、ドキュメント内の <xref:System.Windows.Documents.Run> 要素と <xref:System.Windows.Documents.Span> 要素の使用方法を示します。

[!code-xaml[RunSpan#RunSpan](~/samples/snippets/csharp/VS_Snippets_Wpf/RunSpan/CS/Window1.xaml#runspan)]

<a name="SpanElements"></a>

## <a name="span-elements"></a>Span 要素

<xref:System.Windows.Documents.Span> 要素は、フロー方向が異なるテキスト間の境界区切り記号として機能します。  同じフロー方向の <xref:System.Windows.Documents.Span> 要素でも、異なる双方向スコープを持つと見なされます。これは、<xref:System.Windows.Documents.Span> 要素がコンテナーの <xref:System.Windows.FlowDirection>内で順序付けられていることを意味します。 <xref:System.Windows.Documents.Span> 要素内のコンテンツのみが <xref:System.Windows.FlowDirection> の <xref:System.Windows.Documents.Span>に従います。

次の図は、複数の <xref:System.Windows.Controls.TextBlock> 要素のフロー方向を示しています。

![フロー方向が異なるテキストブロックを示すグラフィック。](./media/bidirectional-features-in-wpf-overview/flow-direction-text-blocks.png)

次の例は、<xref:System.Windows.Documents.Span> と <xref:System.Windows.Documents.Run> の要素を使用して、前の図に示した結果を生成する方法を示しています。

[!code-xaml[Span#Span](~/samples/snippets/csharp/VS_Snippets_Wpf/Span/CS/Window1.xaml#span)]

このサンプルの <xref:System.Windows.Controls.TextBlock> 要素では、<xref:System.Windows.Documents.Span> の要素は親の <xref:System.Windows.FlowDirection> に従ってレイアウトされますが、各 <xref:System.Windows.Documents.Span> 要素内のテキストは、独自の <xref:System.Windows.FlowDirection>に従ってフローします。 これは、ラテン語やアラビア語だけでなく、他のどの言語にも当てはまります。

### <a name="adding-xmllang"></a>xml:lang の追加

次の図は、`"200.0+21.4=221.4"`などの数値と算術式を使用する別の例を示しています。 <xref:System.Windows.FlowDirection> のみが設定されていることに注意してください。

![System.windows.flowdirection> のみを使用して数値を表示するグラフィック。](./media/bidirectional-features-in-wpf-overview/numbers-flow-right-left.png)

このアプリケーションのユーザーには出力が表示されますが、<xref:System.Windows.FlowDirection> が正しい場合でも、数字はアラビア数字として整形されます。

XAML 要素には、各要素の言語を定義する XML 属性 (`xml:lang`) を含めることができます。 XAML では、ツリー内の親要素に適用される値 `xml:lang` 子要素によって使用される XML 言語の原則もサポートされています。 前の例では、<xref:System.Windows.Documents.Run> 要素または最上位レベルの要素に対して言語が定義されていなかったため、既定の `xml:lang` が使用されていました。これは、XAML に `en-US` ます。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の内部数値整形アルゴリズムでは、対応する言語 (この場合は英語) の数値が選択されます。 アラビア数字が正しく表示されるようにするには `xml:lang` を設定する必要があります。

次の図は `xml:lang` が追加された例を示しています。

![右から左に記述されるアラビア数字を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/arabic-numbers-flow-right-left.png)

次の例では、アプリケーションに `xml:lang` を追加します。

[!code-xaml[LangAttribute#LangAttribute](~/samples/snippets/csharp/VS_Snippets_Wpf/LangAttribute/CS/Window1.xaml#langattribute)]

多くの言語では、対象となる地域に応じて `xml:lang` 値が異なることに注意してください。たとえば、`"ar-SA"` と `"ar-EG"` はアラビア語の2つのバリエーションを表します。 前の例では、`xml:lang` と <xref:System.Windows.FlowDirection> の両方の値を定義する必要があることを示しています。

<a name="FlowDirectionNontext"></a>

## <a name="flowdirection-with-non-text-elements"></a>非テキスト要素のある FlowDirection

<xref:System.Windows.FlowDirection> は、テキスト要素内のテキストフローだけでなく、ほぼすべての [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素のフロー方向も定義します。 次の図は、水平方向の <xref:System.Windows.Media.LinearGradientBrush> を使用して、左から右へのグラデーションで背景を描画する <xref:System.Windows.Controls.ToolBar> を示しています。

![左から右へのグラデーションが表示されたツールバーを示すグラフィック。](./media/bidirectional-features-in-wpf-overview/toolbar-left-right-gradient.png)

<xref:System.Windows.FlowDirection> を <xref:System.Windows.FlowDirection.RightToLeft>に設定した後は、<xref:System.Windows.Controls.ToolBar> ボタンだけが右から左に配置されますが、<xref:System.Windows.Media.LinearGradientBrush> では、右から左にフローするようにオフセットを再配置します。

次の図は、<xref:System.Windows.Media.LinearGradientBrush>の再編成を示しています。

![右から左にグラデーションが表示されたツールバーを示すグラフィック。](./media/bidirectional-features-in-wpf-overview/toolbar-right-left-gradient.png)

次の例では、<xref:System.Windows.Controls.ToolBar><xref:System.Windows.FlowDirection.RightToLeft>を描画します。 (左から右に描画するには、<xref:System.Windows.Controls.ToolBar>の <xref:System.Windows.FlowDirection> 属性を削除します。

[!code-xaml[Gradient#Gradient](~/samples/snippets/csharp/VS_Snippets_Wpf/Gradient/CS/Window1.xaml#gradient)]

<a name="FlowDirectionExceptions"></a>

### <a name="flowdirection-exceptions"></a>FlowDirection の例外

<xref:System.Windows.FlowDirection> が想定どおりに動作しないケースがいくつかあります。 ここでは、そのような例外を 2 つ取り上げて説明します。

**Image**

イメージを表示するコントロールを表す <xref:System.Windows.Controls.Image>。 XAML では、表示する <xref:System.Windows.Controls.Image> の URI (uniform resource identifier) を定義する <xref:System.Windows.Controls.Image.Source%2A> プロパティと共に使用できます。

他の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素とは異なり、<xref:System.Windows.Controls.Image> はコンテナーから <xref:System.Windows.FlowDirection> を継承しません。 ただし、<xref:System.Windows.FlowDirection> が明示的に <xref:System.Windows.FlowDirection.RightToLeft>に設定されている場合、<xref:System.Windows.Controls.Image> は水平方向に反転表示されます。 これは、双方向コンテンツの開発者にとって便利な機能として実装されています。場合によっては、イメージを左右反転して表示することで必要な効果が得られるためです。

次の図は、<xref:System.Windows.Controls.Image>を反転したものを示しています。

![反転された画像を示す図。](./media/bidirectional-features-in-wpf-overview/flipped-image-example.png)

次の例は、<xref:System.Windows.Controls.Image> がそれを含む <xref:System.Windows.Controls.StackPanel> から <xref:System.Windows.FlowDirection> を継承できないことを示しています。

> [!NOTE]
> C:\ に ms_logo という名前のファイルが必要**です。** この例を実行するドライブです。

[!code-xaml[Image#Image](~/samples/snippets/csharp/VS_Snippets_Wpf/Image/CS/Window1.xaml#image)]

> [!NOTE]
> ダウンロードファイルには、 **ms_logo .jpg**ファイルが含まれています。 コードは、この .jpg ファイルがプロジェクト内ではなく、C:\ ドライブ上にあることを前提としています。 プロジェクト内のファイルを検索するためには、プロジェクト ファイルから C:\ドライブに .jpg をコピーするかコードを変更する必要があります。 これを行うには、`Source="file://c:/ms_logo.jpg"` を `Source="ms_logo.jpg"`に変更します。

**Paths**

<xref:System.Windows.Controls.Image>に加えて、もう1つの興味深い要素が <xref:System.Windows.Shapes.Path>ます。 パスは、接続された一連の線と曲線を描画できるオブジェクトです。 <xref:System.Windows.FlowDirection>に関する <xref:System.Windows.Controls.Image> と同様の方法で動作します。たとえば、<xref:System.Windows.FlowDirection.RightToLeft><xref:System.Windows.FlowDirection> は <xref:System.Windows.FlowDirection.LeftToRight> 1 つの水平方向のミラーです。 ただし、<xref:System.Windows.Controls.Image>とは異なり、<xref:System.Windows.Shapes.Path> はコンテナーからその <xref:System.Windows.FlowDirection> を継承し、1つを明示的に指定する必要はありません。

次の例では、3 本の線を使用して単純な矢印を描画します。 最初の矢印は、始点と終点が右側のルートから測定されるように、<xref:System.Windows.Controls.StackPanel> から <xref:System.Windows.FlowDirection.RightToLeft> フロー方向を継承します。 また、明示的な <xref:System.Windows.FlowDirection.RightToLeft><xref:System.Windows.FlowDirection> を持つ2番目の矢印も右側で開始します。 ただし、3 番目の矢印の起点は左側です。 描画の詳細については、「<xref:System.Windows.Media.LineGeometry>」と「<xref:System.Windows.Media.GeometryGroup>」を参照してください。

[!code-xaml[Paths#Paths](~/samples/snippets/csharp/VS_Snippets_Wpf/Paths/CS/Window1.xaml#paths)]

次の図は、`Path` 要素を使用して描画された矢印を持つ前の例の出力を示しています。

![Path 要素を使用して描画された矢印を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/arrows-drawn-path-element.png)

<xref:System.Windows.Controls.Image> と <xref:System.Windows.Shapes.Path> は、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] が <xref:System.Windows.FlowDirection>を使用する方法の2つの例です。 コンテナー内の特定の方向に [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素をレイアウトする場合、<xref:System.Windows.FlowDirection> は、サーフェイス、<xref:System.Windows.Media.LinearGradientBrush>、<xref:System.Windows.Media.RadialGradientBrush>でインクをレンダリング <xref:System.Windows.Controls.InkPresenter> などの要素で使用できます。 左から右への動作を模倣するコンテンツに対して右から左の動作が必要な場合、またはその逆の場合は、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] によってこの機能が提供されます。

<a name="NumberSubstitution"></a>

## <a name="number-substitution"></a>数字の置換

従来、Windows では数字の置換がサポートされています。これにより、さまざまなカルチャを同じ数字で表現できるようになり、数字の内部ストレージを異なるロケール間で統合できるようになりました。たとえば、数値はよく知られている16進値0x40、0x41。ただし、選択した言語に従って表示されます。

これにより、アプリケーションで数値を処理できるようになりましたが、1つの言語から別の言語に変換する必要はありません。たとえば、ユーザーはローカライズされたアラビアウィンドウで Microsoft Excel スプレッドシートを開き、アラビア語の数字を表示できますが、Windows の欧州バージョンと、同じ数値の欧州表現を参照してください。 これは、コンマ区切りやパーセント記号などの他の記号でも必要です。これらの記号は、通常同じドキュメント内で数字を伴っているからです。

[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は引き続きこの機能を備え、置換を使用するタイミングと方法をユーザーがさらに制御できるようにこの機能のサポートが強化されています。 この機能はすべての言語向けに設計されていますが、アプリケーションが実行される可能性のあるカルチャが多岐にわたるために特定の言語に合わせて数字の形状を設定することがアプリケーション開発者にとって通常は問題となる双方向コンテンツに対して特に有用です。

[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] での数値置換の動作を制御するコアプロパティは <xref:System.Windows.Media.NumberSubstitution.Substitution%2A> 依存関係プロパティです。 <xref:System.Windows.Media.NumberSubstitution> クラスは、テキスト内の数字の表示方法を指定します。 その動作を定義する 3 つのパブリック プロパティがあります。 各プロパティの概要を次に示します。

**CultureSource:**

このプロパティは、数字のカルチャを決定する方法を指定します。 これは、3つの <xref:System.Windows.Media.NumberCultureSource> 列挙値のいずれかを取ります。

- Override: Number culture は <xref:System.Windows.Media.NumberSubstitution.CultureOverride%2A> プロパティの値です。

- Text: 数字カルチャは、テキスト ランのカルチャです。 マークアップでは、これは `xml:lang`、またはそのエイリアス `Language` プロパティ (<xref:System.Windows.FrameworkElement.Language%2A> または <xref:System.Windows.FrameworkContentElement.Language%2A>) になります。 また、<xref:System.Windows.FrameworkContentElement>から派生するクラスの既定値でもあります。 このようなクラスには、<xref:System.Windows.Documents.Paragraph?displayProperty=nameWithType>、<xref:System.Windows.Documents.Table?displayProperty=nameWithType>、<xref:System.Windows.Documents.TableCell?displayProperty=nameWithType> などがあります。

- User: 数字カルチャは、現在のスレッドのカルチャです。 このプロパティは、<xref:System.Windows.Controls.Page>、<xref:System.Windows.Window>、<xref:System.Windows.Controls.TextBlock>などの <xref:System.Windows.FrameworkElement> のすべてのサブクラスの既定値です。

**CultureOverride**:

<xref:System.Windows.Media.NumberSubstitution.CultureOverride%2A> プロパティは、<xref:System.Windows.Media.NumberSubstitution.CultureSource%2A> プロパティが <xref:System.Windows.Media.NumberCultureSource.Override> に設定されている場合にのみ使用され、それ以外の場合は無視されます。 このプロパティは数字カルチャを指定します。 `null`の値 (既定値) は en-us として解釈されます。

**Substitution**:

このプロパティは、実行する数字の置換の種類を指定します。 次の <xref:System.Windows.Media.NumberSubstitutionMethod> 列挙値のいずれかを取得します。

- <xref:System.Windows.Media.NumberSubstitutionMethod.AsCulture>: 置換メソッドは、数値カルチャの <xref:System.Globalization.NumberFormatInfo.DigitSubstitution%2A?displayProperty=nameWithType> プロパティに基づいて決定されます。 これは既定値です。

- <xref:System.Windows.Media.NumberSubstitutionMethod.Context>: 数字カルチャがアラビア語またはペルシャ語のカルチャである場合は、数字がコンテキストに依存することを指定します。

- <xref:System.Windows.Media.NumberSubstitutionMethod.European>: 数値は常にヨーロッパの数字として表示されます。

- <xref:System.Windows.Media.NumberSubstitutionMethod.NativeNational>: 数字は、カルチャの <xref:System.Globalization.CultureInfo.NumberFormat%2A>によって指定された、数字カルチャの national 数字を使用して表示されます。

- <xref:System.Windows.Media.NumberSubstitutionMethod.Traditional>: 数値カルチャでは、従来の数字を使用して数値がレンダリングされます。 ほとんどのカルチャでは、これは <xref:System.Windows.Media.NumberSubstitutionMethod.NativeNational>と同じです。 ただし、<xref:System.Windows.Media.NumberSubstitutionMethod.NativeNational> は、一部のアラビアカルチャでラテン数字になりますが、この値はすべてのアラビアカルチャでアラビア数字になります。

これらの値は双方向コンテンツ開発者にとってどういった意味があるのでしょうか。 ほとんどの場合、開発者は <xref:System.Windows.FlowDirection> と、各テキスト [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素の言語 (たとえば `Language="ar-SA"`) を定義するだけで済みます。 <xref:System.Windows.Media.NumberSubstitution> ロジックは、正しい [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]に従って数値を表示します。 アラビア語版の Windows で実行されている [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションでアラビア数字と英語番号を使用する例を次に示します。

[!code-xaml[Numbers#Numbers](~/samples/snippets/csharp/VS_Snippets_Wpf/Numbers/CS/Window1.xaml#numbers)]

アラビア語と英語の数字が表示されたアラビア語版の Windows で実行している場合は、前の例の出力を次の図に示します。

![アラビア数字と英語番号を示すグラフィック。](./media/bidirectional-features-in-wpf-overview/arabic-english-numbers.png)

この場合、<xref:System.Windows.FlowDirection> は、<xref:System.Windows.FlowDirection> を <xref:System.Windows.FlowDirection.LeftToRight> に設定すると、ヨーロッパの数字が生成されるため、重要でした。 以下のセクションでは、ドキュメント全体で数字の表示を統一する方法について説明します。 この例では、アラビア語版 Windows で実行されていなければ、すべての数字がヨーロッパ数字として表示されます。

**置換ルールの定義**

実際のアプリケーションでは、プログラムでの言語設定が必要となる場合があります。 たとえば、`xml:lang` 属性を、システムの [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]によって使用されているものと同じになるように設定する場合や、アプリケーションの状態によっては言語を変更する場合があります。

アプリケーションの状態に基づいて変更を行う場合は、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]によって提供される他の機能を使用してください。

最初に、アプリケーションコンポーネントの `NumberSubstitution.CultureSource="Text"`を設定します。 この設定を使用すると、<xref:System.Windows.Controls.TextBlock>のように、既定値として "User" を持つテキスト要素の設定が [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] から取得されないようになります。

次に例を示します。

```xaml
<TextBlock
   Name="text1" NumberSubstitution.CultureSource="Text">
   1234+5679=6913
</TextBlock>
```

対応C#するコードで、たとえば、`Language` プロパティを `"ar-SA"`に設定します。

```csharp
text1.Language = System.Windows.Markup.XmlLanguage.GetLanguage("ar-SA");
```

`Language` プロパティを現在のユーザーの UI 言語に設定する必要がある場合は、次のコードを使用します。

```csharp
text1.Language = System.Windows.Markup.XmlLanguage.GetLanguage(System.Globalization.CultureInfo.CurrentUICulture.IetfLanguageTag);
```

<xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> は、実行時に現在のスレッドによって使用されている現在のカルチャを表します。

最終的な XAML の例は、次の例のようになります。

[!code-xaml[Numbers2#Numbers2](~/samples/snippets/csharp/VS_Snippets_Wpf/Numbers2/CS/Window1.xaml#numbers2)]

最後C#の例は次のようになります。

[!code-csharp[NumbersCSharp#NumbersCSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/NumbersCSharp/CSharp/Window1.xaml.cs#numberscsharp)]

次の図は、いずれかのプログラミング言語でのウィンドウの外観を示しています。アラビア数字が表示されています。

![アラビア数字を表示するグラフィック。](./media/bidirectional-features-in-wpf-overview/displays-arabic-numbers.png)

**Substitution プロパティの使用**

[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] での数値置換の動作は、テキスト要素の言語とその <xref:System.Windows.FlowDirection>によって異なります。 <xref:System.Windows.FlowDirection> が左から右にある場合は、ヨーロッパの数字が表示されます。 ただし、その前にアラビア語のテキストが付いている場合、または言語が "ar" に設定されていて、<xref:System.Windows.FlowDirection> が <xref:System.Windows.FlowDirection.RightToLeft>場合は、アラビア数字が代わりに表示されます。

ただし、たとえば、すべてのユーザーに対してヨーロッパ数字を表示するなどの、統一されたアプリケーションを作成する必要がある場合もあります。 または、特定の <xref:System.Windows.Style>を持つセル <xref:System.Windows.Documents.Table> にはアラビア数字が使用されます。 これを行う簡単な方法の1つは、<xref:System.Windows.Media.NumberSubstitution.Substitution%2A> プロパティを使用することです。

次の例では、最初の <xref:System.Windows.Controls.TextBlock> に <xref:System.Windows.Media.NumberSubstitution.Substitution%2A> プロパティが設定されていないため、アルゴリズムではアラビア数字が想定どおりに表示されます。 ただし、2番目の <xref:System.Windows.Controls.TextBlock>では、置換はヨーロッパに設定され、アラビア数字の既定の置換が上書きされ、ヨーロッパの数字が表示されます。

[!code-xaml[Numbers3#Numbers3](~/samples/snippets/csharp/VS_Snippets_Wpf/Numbers3/CS/Window1.xaml#numbers3)]
