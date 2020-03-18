---
title: ツリー
ms.date: 03/30/2017
helpviewer_keywords:
- logical tree [WPF]
- element tree [WPF]
- visual tree [WPF]
ms.assetid: e83f25e5-d66b-4fc7-92d2-50130c9a6649
ms.openlocfilehash: d4b17c34fb33f73ca1c173bebc8f94ddac5b1942
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746567"
---
# <a name="trees-in-wpf"></a>WPF のツリー
多くのテクノロジでは、要素とコンポーネントはツリー構造で構成されており、開発者はツリー内のオブジェクトノードを直接操作して、アプリケーションのレンダリングや動作に影響を与えます。 また [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、複数のツリー構造のメタファを使用して、プログラム要素間の関係を定義します。 ほとんどの場合、WPF 開発者は、オブジェクトツリーの比喩について概念的に考えると同時に、コードでアプリケーションを作成したり、アプリケーションの一部を XAML で定義したりできますが、特定の API を呼び出すか、特定のマークアップを使用して汎用的なものではありません。などのオブジェクトツリー操作 API は、XML DOM でを使用する場合があります。 WPF は、ツリーの比喩ビュー、<xref:System.Windows.LogicalTreeHelper> および <xref:System.Windows.Media.VisualTreeHelper>を提供する2つのヘルパークラスを公開します。 これらの同じツリーが特定の主要な WPF 機能の動作を理解するのに役立ちます。そのため、ビジュアルツリーと論理ツリーは WPF ドキュメントでも使用されます。 このトピックでは、ビジュアルツリーと論理ツリーが表す内容を定義し、そのようなツリーが全体的なオブジェクトツリーの概念とどのように関連しているかについて説明し、<xref:System.Windows.LogicalTreeHelper> と <xref:System.Windows.Media.VisualTreeHelper>を紹介します。  

<a name="element_tree"></a>   
## <a name="trees-in-wpf"></a>WPF のツリー  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 内の最も完全なツリー構造は、オブジェクトツリーです。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でアプリケーションページを定義し、その後 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を読み込むと、マークアップ内の要素の入れ子の関係に基づいてツリー構造が作成されます。 アプリケーションまたはアプリケーションの一部をコードで定義した場合、特定のオブジェクトのコンテンツモデルを実装するプロパティのプロパティ値の割り当て方法に基づいて、ツリー構造が作成されます。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]には、2つの方法で、完全なオブジェクトツリーを概念化し、そのパブリック API (論理ツリーおよびビジュアルツリー) にレポートできます。 論理ツリーとビジュアルツリーの違いは必ずしも重要であるとは限りませんが、特定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] サブシステムで問題が発生し、マークアップまたはコードで行った選択に影響することがあります。  
  
 論理ツリーまたはビジュアルツリーを直接操作しているわけではありませんが、ツリーの相互作用の概念を理解することは、WPF をテクノロジとして理解するうえで役に立ちます。 WPF は、ある種のツリーの比喩と考えることも、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]でのプロパティの継承とイベントルーティングのしくみを理解する上で重要です。  
  
> [!NOTE]
> オブジェクトツリーは実際の API よりも概念が多いため、概念を考えるもう1つの方法はオブジェクトグラフです。 実際には、ツリーの比喩が分割される、実行時のオブジェクト間のリレーションシップがあります。 それにもかかわらず、特に XAML で定義された UI の場合、ツリーの比喩は、この一般的な概念を参照するときにほとんどの WPF ドキュメントがオブジェクトツリーという用語を使用することに関係しています。  
  
<a name="logical_tree"></a>   
## <a name="the-logical-tree"></a>論理ツリー  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、これらの要素を返すオブジェクトのプロパティを設定することによって、UI 要素にコンテンツを追加します。 たとえば、<xref:System.Windows.Controls.ListBox> コントロールに項目を追加するには、<xref:System.Windows.Controls.ItemsControl.Items%2A> プロパティを操作します。 これにより、<xref:System.Windows.Controls.ItemsControl.Items%2A> プロパティ値である <xref:System.Windows.Controls.ItemCollection> に項目を配置します。 同様に、オブジェクトを <xref:System.Windows.Controls.DockPanel>に追加するには、その <xref:System.Windows.Controls.Panel.Children%2A> プロパティ値を操作します。 ここでは、<xref:System.Windows.Controls.UIElementCollection>にオブジェクトを追加します。 コード例については、「[方法: 要素を動的に追加する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms752374(v=vs.100))」を参照してください。  
  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、<xref:System.Windows.Controls.DockPanel>内の <xref:System.Windows.Controls.ListBox> またはコントロールなどの UI 要素にリスト項目を配置するときに、次の例のように、明示的または暗黙的に <xref:System.Windows.Controls.ItemsControl.Items%2A> および <xref:System.Windows.Controls.Panel.Children%2A> のプロパティも使用します。  
  
 [!code-xaml[TreeOvwsSupport#AllCode](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeOvwsSupport/CS/page1.xaml#allcode)]  
  
 この XAML をドキュメントオブジェクトモデルの下で XML として処理した場合、(有効な) 暗黙的にコメントアウトされたタグを含めると、結果の XML DOM ツリーには `<ListBox.Items>` とその他の暗黙的な項目の要素が含まれます。 ただし、XAML では、マークアップを読み取ってオブジェクトに書き込むときに、このような処理は行われません。結果として得られるオブジェクトグラフには、`ListBox.Items`は含まれません。 ただし、<xref:System.Windows.Controls.ItemCollection>を含む `Items` という名前の <xref:System.Windows.Controls.ListBox> プロパティがあり、<xref:System.Windows.Controls.ListBox> XAML が処理されると <xref:System.Windows.Controls.ItemCollection> は初期化されますが、空になります。 次に、<xref:System.Windows.Controls.ListBox> のコンテンツとして存在する各子オブジェクト要素が、`ItemCollection.Add`のパーサー呼び出しによって <xref:System.Windows.Controls.ItemCollection> に追加されます。 このように、XAML をオブジェクトツリーに処理する例では、作成されたオブジェクトツリーが基本的に論理ツリーであるという一見のように見えます。  
  
 ただし、論理ツリーは、XAML の暗黙的な構文項目が考慮されていない場合でも、実行時にアプリケーション UI に存在するオブジェクトグラフ全体ではありません。主な理由は、ビジュアルとテンプレートです。 たとえば、<xref:System.Windows.Controls.Button>について考えてみます。 論理ツリーは、<xref:System.Windows.Controls.Button> オブジェクトとその文字列 `Content`を報告します。 ただし、実行時のオブジェクトツリーには、このボタンが追加されています。 特に、特定の <xref:System.Windows.Controls.Button> コントロールテンプレートが適用されたため、ボタンは画面に表示されるだけです。 適用されたテンプレート <xref:System.Windows.Controls.Border> (表示されている UI からの入力イベントの処理、論理ツリーの読み取りなど) で論理ツリーを参照している場合でも、論理ツリー内ではそのビジュアルは報告されません。これは、実行時に論理ツリーを見ている場合でも同様です。 テンプレートビジュアルを検索するには、代わりにビジュアルツリーを調べる必要があります。  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 構文を、作成されたオブジェクトグラフにマップする方法、および XAML で暗黙的な構文を使用する方法の詳細については、「 [Xaml 構文の詳細](xaml-syntax-in-detail.md)」または「 [xaml の概要 (WPF)](xaml-overview-wpf.md)」を参照してください。  
  
<a name="tree_property_inheritance_event_routing"></a>   
### <a name="the-purpose-of-the-logical-tree"></a>論理ツリーの目的  
 論理ツリーは、コンテンツモデルが可能な子オブジェクトを簡単に反復処理できるようにし、コンテンツモデルを拡張できるようにするためのものです。 また、論理ツリーは、論理ツリー内のすべてのオブジェクトが読み込まれるときなど、特定の通知のためのフレームワークを提供します。 基本的に、論理ツリーは、フレームワークレベルでの実行時のオブジェクトグラフの近似値です。ビジュアルは除外されますが、独自の実行時アプリケーションの構成に対する多くのクエリ操作には適しています。  
  
 さらに、静的リソース参照と動的リソース参照の両方が解決されます。そのためには、最初に要求しているオブジェクトの <xref:System.Windows.FrameworkElement.Resources%2A> コレクションの論理ツリーを上方向に検索し、その後、論理ツリーを処理してから、そのキーを含む可能性のある <xref:System.Windows.ResourceDictionary>を含む別の `Resources` 値 (または <xref:System.Windows.FrameworkContentElement>) を <xref:System.Windows.FrameworkElement> 確認します。 論理ツリーは、論理ツリーとビジュアルツリーの両方が存在する場合に、リソース検索に使用されます。 リソースディクショナリと参照の詳細については、「 [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。  
  
<a name="composition"></a>   
### <a name="composition-of-the-logical-tree"></a>論理ツリーの構成  
 論理ツリーは WPF フレームワークレベルで定義されます。これは、論理ツリー操作に最も関連する WPF 基本要素が <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>であることを意味します。 ただし、実際に <xref:System.Windows.LogicalTreeHelper> API を使用しているかどうかを確認できるように、論理ツリーには <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>ないノードが含まれる場合があります。 たとえば、論理ツリーは、<xref:System.Windows.Controls.TextBlock>の <xref:System.Windows.Controls.TextBlock.Text%2A> 値 (文字列) を報告します。  
  
<a name="override_logical_tree"></a>   
### <a name="overriding-the-logical-tree"></a>論理ツリーの上書き  
 高度なコントロールの作成者は、一般的なオブジェクトまたはコンテンツモデルが論理ツリー内のオブジェクトを追加または削除する方法を定義するいくつかの Api をオーバーライドすることにより、論理ツリーをオーバーライドできます。 論理ツリーを上書きする方法の例については、「[論理ツリーの上書き](how-to-override-the-logical-tree.md)」を参照してください。  
  
<a name="pvi"></a>   
### <a name="property-value-inheritance"></a>プロパティ値の継承  
 プロパティ値の継承は、ハイブリッドツリーを介して動作します。 プロパティの継承を有効にする <xref:System.Windows.FrameworkPropertyMetadata.Inherits%2A> プロパティを含む実際のメタデータは、WPF フレームワークレベルの <xref:System.Windows.FrameworkPropertyMetadata> クラスです。 したがって、元の値を保持する親とその値を継承する子オブジェクトは両方とも <xref:System.Windows.FrameworkElement> か <xref:System.Windows.FrameworkContentElement>である必要があり、両方とも論理ツリーの一部である必要があります。 ただし、プロパティの継承をサポートする既存の WPF プロパティについては、プロパティ値の継承は、論理ツリーに存在しない介在するオブジェクトを介して perpetuate できます。 これは主に、テンプレート要素で、テンプレート化されたインスタンスで設定されている継承されたプロパティ値を使用する場合、またはより高いレベルのページレベルの構成で、論理ツリー内で上位に設定する場合に関連します。 プロパティ値の継承をこのような境界を越えて一貫して機能させるには、継承プロパティを添付プロパティとして登録する必要があります。また、プロパティを使用してカスタム依存関係プロパティを定義する場合は、このパターンに従う必要があります。継承動作。 プロパティの継承に使用される正確なツリーは、実行時でも、ヘルパークラスのユーティリティメソッドで完全には期待できません。 詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。  
  
<a name="two_trees"></a>   
## <a name="the-visual-tree"></a>ビジュアルツリー  
 論理ツリーの概念に加えて、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]にビジュアルツリーの概念もあります。 ビジュアルツリーには、<xref:System.Windows.Media.Visual> 基底クラスによって表されるビジュアルオブジェクトの構造が記述されています。 コントロールのテンプレートを作成するときは、そのコントロールに適用されるビジュアルツリーを定義または再定義します。 ビジュアルツリーは、パフォーマンスと最適化の理由により、描画を低レベルで制御する開発者にとっても重要です。 従来の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションプログラミングの一部としてビジュアルツリーが公開されると、ルーティングイベントのイベントルートは、論理ツリーではなく、ほとんどの場合、ビジュアルツリーに沿って移動します。 このはらみのルーティングイベントの動作は、コントロールの作成者でない限り、すぐには表示されない場合があります。 ビジュアルツリーを介してイベントをルーティングすると、ビジュアルレベルでコンポジションを実装するコントロールによって、イベントを処理したり、イベントセッターを作成したりできます。  
  
<a name="trees_content"></a>   
## <a name="trees-content-elements-and-content-hosts"></a>ツリー、コンテンツ要素、およびコンテンツホスト  
 コンテンツ要素 (<xref:System.Windows.ContentElement>から派生したクラス) は、ビジュアルツリーの一部ではありません。これらは、<xref:System.Windows.Media.Visual> から継承せず、視覚表現も持ちません。 UI に表示されるようにするには、<xref:System.Windows.ContentElement> を <xref:System.Windows.Media.Visual> と論理ツリーの両方の参加要素であるコンテンツホストでホストする必要があります。 通常、このようなオブジェクトは <xref:System.Windows.FrameworkElement>です。 コンテンツホストがコンテンツの "ブラウザー" のようなものであり、ホストが制御する画面領域内にそのコンテンツを表示する方法を選択するという概念があります。 コンテンツがホストされている場合、ビジュアルツリーに通常関連付けられている特定のツリープロセスでコンテンツを参加させることができます。 一般に、<xref:System.Windows.FrameworkElement> ホストクラスには、ホストされているコンテンツが true ビジュアルツリーの一部ではない場合でも、ホストされている <xref:System.Windows.ContentElement> をコンテンツ論理ツリーのサブノードを介してイベントルートに追加する実装コードが含まれています。 これは、<xref:System.Windows.ContentElement> がそれ自体以外の任意の要素にルーティングするルーティングイベントをソースに送信できるようにするために必要です。  
  
<a name="tree_traversal"></a>   
## <a name="tree-traversal"></a>ツリートラバーサル  
 <xref:System.Windows.LogicalTreeHelper> クラスには、論理ツリートラバーサルの <xref:System.Windows.LogicalTreeHelper.GetChildren%2A>、<xref:System.Windows.LogicalTreeHelper.GetParent%2A>、および <xref:System.Windows.LogicalTreeHelper.FindLogicalNode%2A> の各メソッドが用意されています。 ほとんどの場合、既存のコントロールの論理ツリーを走査する必要はありません。これらのコントロールは、ほとんどの場合、論理上の子要素を、`Add`、インデクサーなどのコレクションアクセスをサポートする専用のコレクションプロパティとして公開するためです。 ツリートラバーサルは、主に、<xref:System.Windows.Controls.Panel> <xref:System.Windows.Controls.ItemsControl> などの目的のコントロールパターンからの派生を選択せず、コレクションプロパティが既に定義されている場合や、独自のコレクションプロパティサポートを提供する場合に使用する、コントロールの作成者によって使用されるシナリオです。  
  
 ビジュアルツリーでは、ビジュアルツリートラバーサル、<xref:System.Windows.Media.VisualTreeHelper>のヘルパークラスもサポートされています。 ビジュアルツリーは、コントロール固有のプロパティによって簡単に公開されないため、プログラミングシナリオに必要な場合は、<xref:System.Windows.Media.VisualTreeHelper> クラスを使用してビジュアルツリーを走査することをお勧めします。 詳しくは、「[WPF グラフィックス レンダリングの概要](../graphics-multimedia/wpf-graphics-rendering-overview.md)」をご覧ください。  
  
> [!NOTE]
> 適用されているテンプレートのビジュアルツリーを調べることが必要になる場合があります。 この手法を使用する場合は注意が必要です。 テンプレートを定義するコントロールのビジュアルツリーを走査している場合でも、コントロールのコンシューマーはインスタンスの <xref:System.Windows.Controls.Control.Template%2A> プロパティを設定することによっていつでもテンプレートを変更できます。また、エンドユーザーは、システムテーマを変更することによって、適用されているテンプレートにも影響を与えることができます。  
  
<a name="routes"></a>   
## <a name="routes-for-routed-events-as-a-tree"></a>"ツリー" としてのルーティングイベントのルート  
 前述のように、特定のルーティングイベントのルートは、ビジュアルおよび論理ツリー表現をハイブリッドにした、ツリーの1つの事前に定義されたパスに沿って移動します。 イベントルートは、ルーティングイベントとバブルルーティングイベントのどちらであるかに応じて、ツリー内の上下方向に移動できます。 イベントルートの概念には、実際にルーティングするイベントを発生させずにイベントルートを "ウォーク" するために使用できるヘルパークラスが直接サポートされていません。 ルート (<xref:System.Windows.EventRoute>) を表すクラスがありますが、そのクラスのメソッドは一般に内部でのみ使用されます。  
  
<a name="resourcesandtrees"></a>   
## <a name="resource-dictionaries-and-trees"></a>リソースディクショナリとツリー  
 ページで定義されているすべての `Resources` のリソースディクショナリの参照は、基本的に論理ツリーになります。 論理ツリーに含まれていないオブジェクトは、キー付きリソースを参照できますが、リソース参照シーケンスは、そのオブジェクトが論理ツリーに接続されているポイントから開始されます。 WPF では、論理ツリーノードだけが <xref:System.Windows.ResourceDictionary>を含む `Resources` プロパティを持つことができるため、<xref:System.Windows.ResourceDictionary>からキー付きリソースを検索するビジュアルツリーを走査する利点はありません。  
  
 ただし、リソースルックアップは、直接の論理ツリーの範囲を超えて拡張することもできます。 アプリケーションマークアップの場合、リソースルックアップは、アプリケーションレベルのリソースディクショナリに続き、次に、静的なプロパティまたはキーとして参照されるテーマサポートとシステム値に進みます。 また、リソース参照が動的である場合は、テーマの論理ツリーの外側のシステム値を参照することもできます。 リソースディクショナリと参照ロジックの詳細については、「 [XAML Resources](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [入力の概要](input-overview.md)
- [WPF グラフィックス レンダリングの概要](../graphics-multimedia/wpf-graphics-rendering-overview.md)
- [ルーティング イベントの概要](routed-events-overview.md)
- [オブジェクト ツリーに存在しないオブジェクト要素の初期化](initialization-for-object-elements-not-in-an-object-tree.md)
- [WPF アーキテクチャ](wpf-architecture.md)
