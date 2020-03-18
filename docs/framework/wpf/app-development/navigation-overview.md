---
title: ナビゲーションの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- loose XAML files [WPF]
- windows [WPF]
- Start page [WPF]
- HTML files [WPF]
- structured navigation [WPF]
- fragment navigation [WPF]
- URIs (Uniform Resource Identifiers)
- custom objects [WPF]
- Uniform Resource Identifiers (URIs)
- pages [WPF]
- frames [WPF]
- navigation hosts [WPF]
- journals [WPF]
- lifetimes [WPF]
- retaining content state [WPF]
- content state [WPF]
- programmatic navigation [WPF]
- hyperlinks [WPF]
ms.assetid: 86ad2143-606a-4e34-bf7e-51a2594248b8
ms.openlocfilehash: 8afda2a314bd04e91c6686fb254a1bd9e773913d
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636290"
---
# <a name="navigation-overview"></a>ナビゲーションの概要

Windows Presentation Foundation (WPF) では、スタンドアロンアプリケーションと XAML ブラウザーアプリケーション (Xbap) の2種類のアプリケーションで使用できるブラウザースタイルのナビゲーションがサポートされています。 コンテンツをナビゲーション用にパッケージ化するために、WPF には <xref:System.Windows.Controls.Page> クラスが用意されています。 <xref:System.Windows.Documents.Hyperlink>を使用するか、プログラムによって <xref:System.Windows.Navigation.NavigationService>を使用して、<xref:System.Windows.Controls.Page> 間を宣言によって相互に移動できます。 WPF は、ジャーナルを使用して、移動されたページとそのページに戻るページを記憶します。

<xref:System.Windows.Controls.Page>、<xref:System.Windows.Documents.Hyperlink>、<xref:System.Windows.Navigation.NavigationService>、および journal は、WPF が提供するナビゲーションサポートの中核となります。 この概要では、これらの機能について詳しく説明してから、緩やかな [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイル、HTML ファイル、およびオブジェクトへの移動を含む高度なナビゲーションサポートについて説明します。

> [!NOTE]
> このトピックでは、"browser" という用語は、現在 Microsoft Internet Explorer と Firefox を含む WPF アプリケーションをホストできるブラウザーのみを指します。 特定の WPF 機能が特定のブラウザーでのみサポートされている場合は、ブラウザーのバージョンが参照されます。

## <a name="navigation-in-wpf-applications"></a>WPF アプリケーションでのナビゲーション

このトピックでは、WPF の主要なナビゲーション機能の概要について説明します。 これらの機能は、スタンドアロンアプリケーションと Xbap の両方で使用できます。ただし、このトピックでは、XBAP のコンテキスト内でこれらの機能を示しています。

> [!NOTE]
> このトピックでは、Xbap をビルドおよび配置する方法については説明しません。 Xbap の詳細については、「 [WPF XAML ブラウザーアプリケーションの概要](wpf-xaml-browser-applications-overview.md)」を参照してください。

このセクションでは、ナビゲーションの次の側面について説明し、例を示します。

- [ページの実装](#CreatingAXAMLPage)

- [スタート ページの構成](#Configuring_a_Start_Page)

- [ホスト ウィンドウのタイトル、幅、および高さの構成](#ConfiguringAXAMLPage)

- [ハイパーリンクのナビゲーション](#NavigatingBetweenXAMLPages)

- [フラグメント ナビゲーション](#FragmentNavigation)

- [ナビゲーション サービス](#NavigationService)

- [ナビゲーション サービスによるプログラム ナビゲーション](#Programmatic_Navigation_with_the_Navigation_Service)

- [ナビゲーションの有効期間](#Navigation_Lifetime)

- [履歴によるナビゲーションの記憶](#NavigationHistory)

- [ページの有効期間と履歴](#PageLifetime)

- [ナビゲーション履歴によるコンテンツの状態の保持](#RetainingContentStateWithNavigationHistory)

- [クッキー](#Cookies)

- [構造化ナビゲーション](#Structured_Navigation)

<a name="CreatingAXAMLPage"></a>

### <a name="implementing-a-page"></a>ページの実装

WPF では、.NET Framework オブジェクト、カスタムオブジェクト、列挙値、ユーザーコントロール、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル、HTML ファイルなど、いくつかのコンテンツの種類に移動できます。 ただし、コンテンツをパッケージ化する最も一般的で便利な方法は、<xref:System.Windows.Controls.Page>を使用することです。 さらに、<xref:System.Windows.Controls.Page> はナビゲーション固有の機能を実装して、外観を向上させ、開発を簡素化します。

<xref:System.Windows.Controls.Page>を使用すると、次のようなマークアップを使用して、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] コンテンツのナビゲート可能なページを宣言によって実装できます。

[!code-xaml[NavigationOverviewSnippets#Page1XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page1.xaml#page1xaml)]

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] マークアップに実装されている <xref:System.Windows.Controls.Page> は、ルート要素として `Page`、WPF XML 名前空間宣言が必要です。 `Page` 要素には、移動して表示するコンテンツが含まれています。 コンテンツを追加するには、次のマークアップに示すように、`Page.Content` property 要素を設定します。

[!code-xaml[NavigationOverviewSnippets#Page2XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page2.xaml#page2xaml)]

`Page.Content` には、1 つの子要素のみを含めることができます。前の例では、コンテンツは単一の文字列「Hello, Page!」です。 実際には、通常はレイアウトコントロールを子要素として使用し (「[レイアウト](../advanced/layout.md)」を参照)、コンテンツを格納して構成します。

`Page` 要素の子要素は <xref:System.Windows.Controls.Page> のコンテンツと見なされるため、明示的な `Page.Content` 宣言を使用する必要はありません。 次のマークアップは、前のサンプルの宣言と同等です。

[!code-xaml[NavigationOverviewSnippets#Page3XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page3.xaml#page3xaml)]

この場合、`Page.Content` は `Page` 要素の子要素を使用して自動的に設定されます。 詳細については、「[WPF のコンテンツ モデル](../controls/wpf-content-model.md)」を参照してください。

マークアップのみの <xref:System.Windows.Controls.Page> は、コンテンツを表示する場合に便利です。 ただし、<xref:System.Windows.Controls.Page> は、ユーザーがページと対話できるようにするコントロールを表示することもできます。また、イベントを処理し、アプリケーションロジックを呼び出すことによって、ユーザーの操作に応答できます。 次の例に示すように、マークアップと分離コードの組み合わせを使用して、対話型の <xref:System.Windows.Controls.Page> を実装します。

[!code-xaml[XBAPAppDefSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml#homepagemarkup)]

[!code-csharp[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml.cs#homepagecodebehind)]
[!code-vb[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/HomePage.xaml.vb#homepagecodebehind)]

マークアップ ファイルと分離コード ファイルを連携させるには、次の構成が必要です。

- マークアップでは、`Page` 要素に `x:Class` 属性が含まれている必要があります。 アプリケーションがビルドされると、マークアップファイルに `x:Class` が存在することにより、Microsoft build engine (MSBuild) によって <xref:System.Windows.Controls.Page> から派生した `partial` クラスが作成され、`x:Class` 属性によって指定された名前になります。 そのためには、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] スキーマ (`xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`) の XML 名前空間宣言を追加する必要があります。 生成された `partial` クラスは `InitializeComponent`を実装します。これは、イベントを登録し、マークアップで実装されるプロパティを設定するために呼び出されます。

- 分離コードでは、クラスは、マークアップで `x:Class` 属性によって指定された名前と同じ名前を持つ `partial` クラスである必要があり、<xref:System.Windows.Controls.Page>から派生する必要があります。 これにより、分離コードファイルは、アプリケーションのビルド時にマークアップファイル用に生成される `partial` クラスに関連付けられます (「 [WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)」を参照してください)。

- 分離コードでは、<xref:System.Windows.Controls.Page> クラスは、`InitializeComponent` メソッドを呼び出すコンストラクターを実装する必要があります。 `InitializeComponent` は、マークアップファイルによって生成される `partial` クラスによって実装され、イベントを登録したり、マークアップで定義されているプロパティを設定したりします。

> [!NOTE]
> Visual Studio を使用して新しい <xref:System.Windows.Controls.Page> をプロジェクトに追加すると、<xref:System.Windows.Controls.Page> はマークアップと分離コードの両方を使用して実装されます。ここで説明するように、マークアップファイルと分離コードファイルの関連付けを作成するために必要な構成が含まれます。

<xref:System.Windows.Controls.Page>が完了したら、そこに移動できます。 アプリケーションが移動する最初の <xref:System.Windows.Controls.Page> を指定するには、開始 <xref:System.Windows.Controls.Page>を構成する必要があります。

<a name="Configuring_a_Start_Page"></a>

### <a name="configuring-a-start-page"></a>スタート ページの構成

Xbap では、特定の量のアプリケーションインフラストラクチャがブラウザーでホストされている必要があります。 WPF では、<xref:System.Windows.Application> クラスは、必要なアプリケーションインフラストラクチャを確立するアプリケーション定義の一部です (「[アプリケーション管理の概要](application-management-overview.md)」を参照してください)。

アプリケーション定義は通常、マークアップと分離コードの両方を使用して実装され、マークアップファイルは MSBuild`ApplicationDefinition` 項目として構成されます。 XBAP のアプリケーション定義を次に示します。

[!code-xaml[XBAPAppDefSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

[!code-csharp[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml.cs#xbapapplicationdefinitioncodebehind)]
[!code-vb[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/Application.xaml.vb#xbapapplicationdefinitioncodebehind)]

XBAP は、アプリケーション定義を使用して開始 <xref:System.Windows.Controls.Page>を指定できます。これは、XBAP の起動時に自動的に読み込まれる <xref:System.Windows.Controls.Page> です。 これを行うには、目的の <xref:System.Windows.Controls.Page>の URI (uniform resource identifier) を使用して <xref:System.Windows.Application.StartupUri%2A> プロパティを設定します。

> [!NOTE]
> ほとんどの場合、<xref:System.Windows.Controls.Page> は、アプリケーションにコンパイルされるか、アプリケーションと共に配置されます。 このような場合、<xref:System.Windows.Controls.Page> を識別する URI はパック URI です。これは、*パック*スキームに準拠した uri です。 パック Uri については、「 [WPF のパック uri](pack-uris-in-wpf.md)」で詳しく説明します。 以下で説明する http スキームを使用してコンテンツにナビゲートすることもできます。

<xref:System.Windows.Application.StartupUri%2A> は、次の例に示すように、マークアップで宣言によって設定できます。

[!code-xaml[NavigationOverviewSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

この例では、`StartupUri` 属性は、ホームページを識別する相対パック URI で設定されます。 XBAP が起動すると、ホームページに自動的に移動して表示されます。 これは、次の図に示すように、Web サーバーから起動された XBAP を示しています。

![XBAP ページ](./media/navigation-overview/xbap-launched-from-a-web-server.png "これは、Web サーバーから起動された XBAP を示しています。")

> [!NOTE]
> Xbap の開発と配置の詳細については、「 [WPF XAML ブラウザーアプリケーションの概要](wpf-xaml-browser-applications-overview.md)」および「 [Wpf アプリケーションの配置](deploying-a-wpf-application-wpf.md)」を参照してください。

<a name="ConfiguringAXAMLPage"></a>

### <a name="configuring-the-host-windows-title-width-and-height"></a>ホスト ウィンドウのタイトル、幅、および高さの構成

前の図から、ブラウザーとタブパネルの両方のタイトルが XBAP の URI であることがわかりました。 このタイトルは、長いだけでなく、見た目が良いわけでも、有益な情報になっているわけでもありません。 このため、<xref:System.Windows.Controls.Page> には、<xref:System.Windows.Controls.Page.WindowTitle%2A> プロパティを設定してタイトルを変更する方法が用意されています。 さらに、<xref:System.Windows.Controls.Page.WindowWidth%2A> と <xref:System.Windows.Controls.Page.WindowHeight%2A>をそれぞれ設定して、ブラウザーウィンドウの幅と高さを構成することもできます。

<xref:System.Windows.Controls.Page.WindowTitle%2A>、<xref:System.Windows.Controls.Page.WindowWidth%2A>、および <xref:System.Windows.Controls.Page.WindowHeight%2A> は、次の例に示すように、マークアップで宣言によって設定できます。

[!code-xaml[NavigationOverviewSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/HomePage.xaml#homepagemarkup)]

結果を次の例に示します。

![ウィンドウのタイトル、高さ、幅](./media/navigation-overview/window-title-width-height.png "これにより、構成できるウィンドウのタイトル、高さ、および幅が表示されます。")

<a name="NavigatingBetweenXAMLPages"></a>

### <a name="hyperlink-navigation"></a>ハイパーリンクのナビゲーション

一般的な XBAP は複数のページで構成します。 あるページから別のページに移動する最も簡単な方法は、<xref:System.Windows.Documents.Hyperlink>を使用することです。 次のマークアップに示されている `Hyperlink` 要素を使用して、<xref:System.Windows.Controls.Page> に <xref:System.Windows.Documents.Hyperlink> を宣言によって追加できます。

[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml1)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml2)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml3)]

`Hyperlink` 要素には、次のものが必要です。

- `NavigateUri` 属性によって指定された移動先の <xref:System.Windows.Controls.Page> のパック URI。

- ユーザーがクリックしてナビゲーションを開始できるコンテンツ (`Hyperlink` 要素に含めることができるコンテンツについては、「<xref:System.Windows.Documents.Hyperlink>」を参照してください)。

次の図は、<xref:System.Windows.Documents.Hyperlink>を持つ <xref:System.Windows.Controls.Page> を持つ XBAP を示しています。

![ハイパーリンクがあるページ](./media/navigation-overview/xbap-with-a-page-with-a-hyperlink.png "これは、ページとハイパーリンクを持つ XBAP を示しています。")

ご想像のとおり、<xref:System.Windows.Documents.Hyperlink> をクリックすると、XBAP は `NavigateUri` 属性によって識別される <xref:System.Windows.Controls.Page> に移動します。 また、XBAP は、Internet Explorer の [最近使ったページ] の一覧に、前の <xref:System.Windows.Controls.Page> のエントリを追加します。 これを次の図に示します。

![[戻る] ボタンと [進む] ボタン](./media/navigation-overview/back-and-forward-navigation.png "[戻る] ボタンと [進む] ボタンを使用して移動します。")

また、ある <xref:System.Windows.Controls.Page> から別のへのナビゲーションをサポートすると、<xref:System.Windows.Documents.Hyperlink> フラグメントナビゲーションもサポートされます。

<a name="FragmentNavigation"></a>

### <a name="fragment-navigation"></a>フラグメント ナビゲーション

*フラグメントナビゲーション*は、現在の <xref:System.Windows.Controls.Page> または別の <xref:System.Windows.Controls.Page>のコンテンツフラグメントへのナビゲーションです。 WPF では、コンテンツフラグメントとは、名前付き要素に含まれるコンテンツのことです。 名前付き要素は、`Name` 属性が設定されている要素です。 次のマークアップは、コンテンツフラグメントを含む名前付きの `TextBlock` 要素を示しています。

[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup2)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup3)]

<xref:System.Windows.Documents.Hyperlink> がコンテンツフラグメントに移動するには、`NavigateUri` 属性に次のものが含まれている必要があります。

- 移動先のコンテンツフラグメントを持つ <xref:System.Windows.Controls.Page> の URI。

- 「#」文字。

- コンテンツフラグメントを含む <xref:System.Windows.Controls.Page> 上の要素の名前。

フラグメント URI の形式は次のとおりです。

*PageURI* `#` *ElementName*

コンテンツフラグメントに移動するように構成されている `Hyperlink` の例を次に示します。

[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml1)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml2)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml3)]

> [!NOTE]
> このセクションでは、WPF での既定のフラグメントナビゲーション実装について説明します。 WPF では、<xref:System.Windows.Navigation.NavigationService.FragmentNavigation?displayProperty=nameWithType> イベントの処理を必要とする独自のフラグメントナビゲーションスキームを実装することもできます。

> [!IMPORTANT]
> ページを HTTP で参照できる場合に限り、ルース [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページ (`Page` がルート要素として設定されている [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル) 内のフラグメントに移動できます。
>
> ただし、緩やかな [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページは、独自のフラグメントに移動できます。

<a name="NavigationService"></a>

### <a name="navigation-service"></a>ナビゲーション サービス

<xref:System.Windows.Documents.Hyperlink> では、ユーザーは特定の <xref:System.Windows.Controls.Page>へのナビゲーションを開始できますが、ページの検索とダウンロードの作業は <xref:System.Windows.Navigation.NavigationService> クラスによって実行されます。 基本的に、<xref:System.Windows.Navigation.NavigationService> は、<xref:System.Windows.Documents.Hyperlink>などのクライアントコードに代わってナビゲーション要求を処理する機能を提供します。 さらに、<xref:System.Windows.Navigation.NavigationService> は、ナビゲーション要求を追跡して影響を与える上位レベルのサポートを実装します。

<xref:System.Windows.Documents.Hyperlink> がクリックされると、WPF は <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> を呼び出して、指定されたパック URI にある <xref:System.Windows.Controls.Page> を見つけてダウンロードします。 ダウンロードした <xref:System.Windows.Controls.Page> は、ルートオブジェクトがダウンロードされた <xref:System.Windows.Controls.Page>のインスタンスであるオブジェクトのツリーに変換されます。 ルート <xref:System.Windows.Controls.Page> オブジェクトへの参照は、<xref:System.Windows.Navigation.NavigationService.Content%2A?displayProperty=nameWithType> プロパティに格納されます。 移動先のコンテンツのパック URI は <xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> プロパティに格納され、<xref:System.Windows.Navigation.NavigationService.CurrentSource%2A?displayProperty=nameWithType> は移動先の最後のページのパック URI を格納します。

> [!NOTE]
> WPF アプリケーションに現在アクティブな <xref:System.Windows.Navigation.NavigationService>が複数存在する可能性があります。 詳細については、このトピックで後述する「[ナビゲーションホスト](#Navigation_Hosts)」を参照してください。

<a name="Programmatic_Navigation_with_the_Navigation_Service"></a>

### <a name="programmatic-navigation-with-the-navigation-service"></a>ナビゲーション サービスによるプログラム ナビゲーション

<xref:System.Windows.Documents.Hyperlink>を使用してマークアップで宣言的にナビゲーションが実装されている場合、<xref:System.Windows.Navigation.NavigationService> について知る必要はありません。 <xref:System.Windows.Documents.Hyperlink> では <xref:System.Windows.Navigation.NavigationService> が使用されるためです。 つまり、<xref:System.Windows.Documents.Hyperlink> の直接または間接の親がナビゲーションホスト (「[ナビゲーション](#Navigation_Hosts)ホスト」を参照) である限り、<xref:System.Windows.Documents.Hyperlink> はナビゲーションホストのナビゲーションサービスを検索して使用し、ナビゲーション要求を処理することができます。

ただし、次のように <xref:System.Windows.Navigation.NavigationService> を直接使用する必要がある場合もあります。

- パラメーター化されていないコンストラクターを使用して <xref:System.Windows.Controls.Page> をインスタンス化する必要がある場合。

- <xref:System.Windows.Controls.Page> に移動する前に、プロパティを設定する必要がある場合。

- 移動する必要がある <xref:System.Windows.Controls.Page> を実行時にのみ決定できる場合。

このような状況では、<xref:System.Windows.Navigation.NavigationService> オブジェクトの <xref:System.Windows.Navigation.NavigationService.Navigate%2A> メソッドを呼び出すことによって、プログラムによってナビゲーションを開始するコードを記述する必要があります。 これには、<xref:System.Windows.Navigation.NavigationService>への参照を取得する必要があります。

#### <a name="getting-a-reference-to-the-navigationservice"></a>NavigationService への参照の取得

[ナビゲーションホスト](#Navigation_Hosts)セクションで説明されている理由により、WPF アプリケーションは複数の <xref:System.Windows.Navigation.NavigationService>を持つことができます。 つまり、コードには <xref:System.Windows.Navigation.NavigationService>を検索する方法が必要です。これは通常、現在の <xref:System.Windows.Controls.Page>に移動した <xref:System.Windows.Navigation.NavigationService> です。 `static`<xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、<xref:System.Windows.Navigation.NavigationService> への参照を取得できます。 特定の <xref:System.Windows.Controls.Page>に移動した <xref:System.Windows.Navigation.NavigationService> を取得するには、<xref:System.Windows.Controls.Page> への参照を <xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A> メソッドの引数として渡します。 次のコードは、現在の <xref:System.Windows.Controls.Page>の <xref:System.Windows.Navigation.NavigationService> を取得する方法を示しています。

[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPage.xaml.vb#getnscodebehind2)]

<xref:System.Windows.Controls.Page>の <xref:System.Windows.Navigation.NavigationService> を見つけるためのショートカットとして、<xref:System.Windows.Controls.Page> は <xref:System.Windows.Controls.Page.NavigationService%2A> プロパティを実装します。 これを次の例に示します。

[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPageShortCut.xaml.vb#getnsshortcutcodebehind2)]

> [!NOTE]
> <xref:System.Windows.Controls.Page> は、<xref:System.Windows.FrameworkElement.Loaded> イベント <xref:System.Windows.Controls.Page> 発生した場合にのみ、その <xref:System.Windows.Navigation.NavigationService> への参照を取得できます。

#### <a name="programmatic-navigation-to-a-page-object"></a>ページ オブジェクトへのプログラム ナビゲーション

次の例では、<xref:System.Windows.Navigation.NavigationService> を使用して、プログラムで <xref:System.Windows.Controls.Page>に移動する方法を示します。 移動先の <xref:System.Windows.Controls.Page> は、単一のパラメーターなしのコンストラクターを使用してのみインスタンス化できるため、プログラムによるナビゲーションが必要です。 パラメーターなしのコンストラクターを使用した <xref:System.Windows.Controls.Page> は、次のマークアップとコードに示されています。

[!code-xaml[NavigationOverviewSnippets#PageWithNonDefaultConstructorXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml#pagewithnondefaultconstructorxaml)]

[!code-csharp[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml.cs#pagewithnondefaultconstructorcodebehind)]
[!code-vb[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithNonDefaultConstructor.xaml.vb#pagewithnondefaultconstructorcodebehind)]

パラメーターなしのコンストラクターを使用して <xref:System.Windows.Controls.Page> に移動する <xref:System.Windows.Controls.Page> は、次のマークアップとコードに示されています。

[!code-xaml[NavigationOverviewSnippets#NSNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml#nsnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml.cs#nsnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSNavigationPage.xaml.vb#nsnavigationpagecodebehind)]

この <xref:System.Windows.Controls.Page> の <xref:System.Windows.Documents.Hyperlink> をクリックすると、<xref:System.Windows.Controls.Page> をインスタンス化して、パラメーター化されていないコンストラクターを使用し、<xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、ナビゲーションが開始されます。 <xref:System.Windows.Navigation.NavigationService.Navigate%2A> は、パック URI ではなく、<xref:System.Windows.Navigation.NavigationService> が移動するオブジェクトへの参照を受け入れます。

#### <a name="programmatic-navigation-with-a-pack-uri"></a>パック URI によるプログラム ナビゲーション

プログラムによってパック URI を構築する必要がある場合 (実行時にパック URI を特定できる場合など) は、<xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> メソッドを使用できます。 これを次の例に示します。

[!code-xaml[NavigationOverviewSnippets#NSUriNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml#nsurinavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml.cs#nsurinavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSUriNavigationPage.xaml.vb#nsurinavigationpagecodebehind)]

#### <a name="refreshing-the-current-page"></a>現在のページの更新

<xref:System.Windows.Controls.Page> は、<xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> プロパティに格納されているパック URI と同じパック URI がある場合はダウンロードされません。 WPF が現在のページを再びダウンロードするようにするには、次の例に示すように、<xref:System.Windows.Navigation.NavigationService.Refresh%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。

[!code-xaml[NavigationOverviewSnippets#NSRefreshNavigationPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml#nsrefreshnavigationpagexaml1)]

[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind1)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind2)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind2)]

<a name="Navigation_Lifetime"></a>

### <a name="navigation-lifetime"></a>ナビゲーションの有効期間

これまでに説明したように、ナビゲーションを開始するには多くの方法があります。 ナビゲーションが開始されている間にナビゲーションが進行しているときに、<xref:System.Windows.Navigation.NavigationService>によって実装された次のイベントを使用して、ナビゲーションを追跡し、影響を与えることができます。

- <xref:System.Windows.Navigation.NavigationService.Navigating>. 新しいナビゲーションが要求されたときに発生します。 ナビゲーションのキャンセルに使用できます。

- <xref:System.Windows.Navigation.NavigationService.NavigationProgress>. ナビゲーション進行状況の情報提供を目的として、ダウンロード中に定期的に発生します。

- <xref:System.Windows.Navigation.NavigationService.Navigated>. ページの位置が特定され、ダウンロードされたときに発生します。

- <xref:System.Windows.Navigation.NavigationService.NavigationStopped>. <xref:System.Windows.Navigation.NavigationService.StopLoading%2A>を呼び出すことによって) ナビゲーションが停止したとき、または現在のナビゲーションの進行中に新しいナビゲーションが要求されたときに発生します。

- <xref:System.Windows.Navigation.NavigationService.NavigationFailed>. 要求されたコンテンツにナビゲートするときにエラーが発生したときに発生します。

- <xref:System.Windows.Navigation.NavigationService.LoadCompleted>. ナビゲート先のコンテンツが読み込まれ、解析されて、レンダリングが開始されたときに発生します。

- <xref:System.Windows.Navigation.NavigationService.FragmentNavigation>. コンテンツ フラグメントへのナビゲーションが開始されたときに、次のタイミングで発生します。

  - 目的のフラグメントが現在のコンテンツの場合は、すぐに発生します。

  - 目的のフラグメントが別のコンテンツにある場合は、ソース コンテンツが読み込まれた後で発生します。

ナビゲーション イベントは、次の図に示されている順序で発生します。

![ページナビゲーションのフローチャート](./media/navigation-overview/order-of-navigation-events.png "ページナビゲーションイベントのフローチャート")

一般に、<xref:System.Windows.Controls.Page> では、これらのイベントについて心配することはありません。 多くの場合、アプリケーションで問題が発生する可能性があります。そのため、これらのイベントは <xref:System.Windows.Application> クラスによっても発生します。

- <xref:System.Windows.Application.Navigating?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationProgress?displayProperty=nameWithType>

- <xref:System.Windows.Application.Navigated?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationFailed?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationStopped?displayProperty=nameWithType>

- <xref:System.Windows.Application.LoadCompleted?displayProperty=nameWithType>

- <xref:System.Windows.Application.FragmentNavigation?displayProperty=nameWithType>

<xref:System.Windows.Navigation.NavigationService> によってイベントが発生するたびに、<xref:System.Windows.Application> クラスによって対応するイベントが発生します。 <xref:System.Windows.Controls.Frame> と <xref:System.Windows.Navigation.NavigationWindow> は、それぞれのスコープ内のナビゲーションを検出するために同じイベントを提供します。

場合によっては、<xref:System.Windows.Controls.Page> がこれらのイベントに関心を持つことがあります。 たとえば、<xref:System.Windows.Controls.Page> は、ナビゲーションをキャンセルするかどうかを決定するために <xref:System.Windows.Navigation.NavigationService.Navigating?displayProperty=nameWithType> イベントを処理することがあります。 これを次の例に示します。

[!code-xaml[NavigationOverviewSnippets#CancelNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml#cancelnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml.cs#cancelnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/CancelNavigationPage.xaml.vb#cancelnavigationpagecodebehind)]

前の例のように、<xref:System.Windows.Controls.Page>からナビゲーションイベントを使用してハンドラーを登録する場合は、イベントハンドラーの登録を解除する必要もあります。 そうでない場合は、WPF ナビゲーションが履歴を使用した <xref:System.Windows.Controls.Page> ナビゲーションを記憶する方法に関して副作用が生じる可能性があります。

<a name="NavigationHistory"></a>

### <a name="remembering-navigation-with-the-journal"></a>履歴によるナビゲーションの記憶

WPF は2つのスタックを使用して、移動したページを記憶します。これは、バックスタックと順方向スタックです。 現在の <xref:System.Windows.Controls.Page> から新しい <xref:System.Windows.Controls.Page> に移動するか、既存の <xref:System.Windows.Controls.Page>に進むと、現在の <xref:System.Windows.Controls.Page> が*バックスタック*に追加されます。 現在の <xref:System.Windows.Controls.Page> から前の <xref:System.Windows.Controls.Page>に移動すると、現在の <xref:System.Windows.Controls.Page> が*前方スタック*に追加されます。 戻るスタック、進むスタック、およびそれらを管理する機能を、まとめて履歴と呼びます。 バックスタックと転送スタックの各項目は <xref:System.Windows.Navigation.JournalEntry> クラスのインスタンスであり、*履歴エントリ*と呼ばれます。

#### <a name="navigating-the-journal-from-internet-explorer"></a>Internet Explorer からの履歴のナビゲート

概念的には、journal は Internet Explorer の **[戻る]** ボタンと **[進む]** ボタンと同じ方法で動作します。 これを次の図に示します。

![[戻る] ボタンと [進む] ボタン](./media/navigation-overview/back-and-forward-navigation.png "[戻る] ボタンと [進む] ボタンを使用して移動します。")

Internet Explorer でホストされている Xbap の場合、WPF は journal を Internet Explorer のナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] に統合します。 これにより、ユーザーは Internet Explorer の **[戻る]** 、 **[進む]** 、 **[最近使ったページ]** の各ボタンを使用して、XBAP 内のページを移動できます。

> [!IMPORTANT]
> Internet Explorer では、ユーザーが XBAP との間を移動したときに、保持されていないページの履歴項目だけが履歴に保持されます。 ページを維持したままにする方法については、このトピックで後述する「[ページの有効期間と履歴](#PageLifetime)」を参照してください。

既定では、Internet Explorer の **[最近使ったページ]** の一覧に表示される各 <xref:System.Windows.Controls.Page> のテキストは、<xref:System.Windows.Controls.Page>の URI です。 多くの場合、これは、ユーザーにとって特に意味がありません。 幸い、次のオプションのいずれかを使用して、テキストを変更できます。

1. アタッチされた `JournalEntry.Name` 属性値。

2. `Page.Title` 属性値。

3. 現在の <xref:System.Windows.Controls.Page>の `Page.WindowTitle` 属性値と URI。

4. 現在の <xref:System.Windows.Controls.Page>の URI。 (既定)

これらのオプションの順序は、テキスト検索の優先順位と一致します。 たとえば、`JournalEntry.Name` が設定されている場合、他の値は無視されます。

次の例では、`Page.Title` 属性を使用して、journal エントリに対して表示されるテキストを変更します。

[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup2)]

[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind1)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind2)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind2)]

#### <a name="navigating-the-journal-using-wpf"></a>WPF を使用する履歴のナビゲート

ユーザーは、Internet Explorer の **戻る**、**進む**、および 最近 の各**ページ**を使用して履歴内を移動できますが、WPF によって提供される宣言型とプログラム的なメカニズムの両方を使用して、履歴内を移動することもできます。 これを行う理由の1つは、ページにカスタムナビゲーション Ui を提供することです。

<xref:System.Windows.Input.NavigationCommands>によって公開されているナビゲーションコマンドを使用して、journal ナビゲーションサポートを宣言によって追加できます。 次の例は、`BrowseBack` ナビゲーションコマンドの使用方法を示しています。

[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml1)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml2)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml3)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML4](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml4)]

<xref:System.Windows.Navigation.NavigationService> クラスの次のいずれかのメンバーを使用して、プログラムを使用して履歴をナビゲートできます。

- <xref:System.Windows.Navigation.NavigationService.GoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.GoForward%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoForward%2A>

このトピックの「[ナビゲーション履歴によるコンテンツの状態の保持](#RetainingContentStateWithNavigationHistory)」で説明されているように、履歴はプログラムによって操作することもできます。

<a name="PageLifetime"></a>

### <a name="page-lifetime-and-the-journal"></a>ページの有効期間と履歴

グラフィックス、アニメーション、メディアなどのリッチコンテンツを含むいくつかのページを含む XBAP を考えてみましょう。 このようなページのメモリ使用量は、特にビデオやオーディオ メディアが使用されている場合、非常に大きくなることがあります。 ジャーナルは移動先のページを "記憶" しているので、このような XBAP は、大量のメモリを迅速に消費する可能性があります。

このため、履歴の既定の動作では、<xref:System.Windows.Controls.Page> オブジェクトへの参照ではなく、各履歴エントリに <xref:System.Windows.Controls.Page> メタデータが格納されます。 履歴項目に移動すると、その <xref:System.Windows.Controls.Page> メタデータを使用して、指定された <xref:System.Windows.Controls.Page>の新しいインスタンスが作成されます。 その結果、移動される各 <xref:System.Windows.Controls.Page> には、次の図に示す有効期間があります。

![ページの有効期間](./media/navigation-overview/navigated-page-lifetime.png "これは、ページが移動されたときの有効期間を示します。")

既定のジャーナル動作を使用すると、メモリ使用量を節約できますが、ページごとのレンダリングのパフォーマンスが低下する可能性があります。特に大量のコンテンツがある場合は、<xref:System.Windows.Controls.Page> を再インスタンス化すると時間がかかることがあります。 <xref:System.Windows.Controls.Page> インスタンスをジャーナルに保持する必要がある場合は、2つの方法を使用して作成できます。 まず、<xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、プログラムを使用して <xref:System.Windows.Controls.Page> オブジェクトに移動できます。

次に、<xref:System.Windows.Controls.Page.KeepAlive%2A> プロパティを `true` (既定値は `false`) に設定することによって、WPF が <xref:System.Windows.Controls.Page> のインスタンスをジャーナルに保持するように指定できます。 次の例に示すように、マークアップで宣言によって <xref:System.Windows.Controls.Page.KeepAlive%2A> を設定できます。

[!code-xaml[NavigationOverviewSnippets#KeepAlivePageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/KeepAlivePage.xaml#keepalivepagexaml)]

保持されている <xref:System.Windows.Controls.Page> の有効期間は、それとは異なるものとはわずかに異なります。 保持されている <xref:System.Windows.Controls.Page> が最初に移動されたときには、保持されていない <xref:System.Windows.Controls.Page> と同じようにインスタンス化されます。 ただし、<xref:System.Windows.Controls.Page> のインスタンスはジャーナルに保持されるため、ジャーナルに残っている限り、再インスタンス化されることはありません。 その結果、<xref:System.Windows.Controls.Page> がナビゲートされるたびに呼び出される必要がある初期化ロジックが <xref:System.Windows.Controls.Page> にある場合は、コンストラクターから <xref:System.Windows.FrameworkElement.Loaded> イベントのハンドラーに移動する必要があります。 次の図に示すように、<xref:System.Windows.FrameworkElement.Loaded> イベントと <xref:System.Windows.FrameworkElement.Unloaded> イベントは、それぞれ <xref:System.Windows.Controls.Page> に移動するたびに発生します。

![読み込みイベントとアンロードイベントが発生したとき](./media/navigation-overview/loaded-and-unloaded-events.png "読み込まれたイベントとアンロードされたイベントは、ページに移動したり、ページを移動したりすると発生します。")

<xref:System.Windows.Controls.Page> が維持されていない場合は、次のいずれの操作も実行しないでください。

- ページへの参照、またはその一部への参照を格納する。

- ページによって実装されていないイベントのイベント ハンドラーを登録する。

これらのいずれかを実行すると、履歴から削除された後でも、<xref:System.Windows.Controls.Page> を強制的にメモリに保持する参照が作成されます。

一般に、<xref:System.Windows.Controls.Page> を使用しないようにする場合は、既定の <xref:System.Windows.Controls.Page> 動作を使用することをお勧めします。 ただし、これには、次のセクションで説明されている状態も関係します。

<a name="RetainingContentStateWithNavigationHistory"></a>

### <a name="retaining-content-state-with-navigation-history"></a>ナビゲーション履歴によるコンテンツの状態の保持

<xref:System.Windows.Controls.Page> が維持されていない状態で、ユーザーからデータを収集するコントロールがある場合、ユーザーが <xref:System.Windows.Controls.Page>に移動して戻ると、データはどうなりますか。 ユーザー エクスペリエンスの観点から見ると、ユーザーは以前に入力したデータが表示されることを期待します。 残念ながら、各ナビゲーションを使用して <xref:System.Windows.Controls.Page> の新しいインスタンスが作成されるため、データを収集したコントロールは再インスタンス化され、データは失われます。

幸いにも、この履歴では、制御データを含む <xref:System.Windows.Controls.Page> ナビゲーション全体でデータを記憶することがサポートされています。 具体的には、各 <xref:System.Windows.Controls.Page> の履歴エントリは、関連付けられた <xref:System.Windows.Controls.Page> 状態の一時コンテナーとして機能します。 次の手順では、<xref:System.Windows.Controls.Page> を移動するときに、このサポートを使用する方法を説明します。

1. 現在の <xref:System.Windows.Controls.Page> のエントリが履歴に追加されます。

2. <xref:System.Windows.Controls.Page> の状態は、そのページの履歴エントリと共に保存され、バックスタックに追加されます。

3. 新しい <xref:System.Windows.Controls.Page> に移動します。

ページ <xref:System.Windows.Controls.Page> がに戻ると、ジャーナルを使用して次の手順が実行されます。

1. <xref:System.Windows.Controls.Page> (バックスタックの一番上のジャーナルエントリ) がインスタンス化されます。

2. <xref:System.Windows.Controls.Page> は、<xref:System.Windows.Controls.Page>の履歴エントリと共に保存された状態で更新されます。

3. <xref:System.Windows.Controls.Page> がに戻ります。

WPF は、次のコントロールが <xref:System.Windows.Controls.Page>で使用されている場合に、このサポートを自動的に使用します。

- <xref:System.Windows.Controls.CheckBox>

- <xref:System.Windows.Controls.ComboBox>

- <xref:System.Windows.Controls.Expander>

- <xref:System.Windows.Controls.Frame>

- <xref:System.Windows.Controls.ListBox>

- <xref:System.Windows.Controls.ListBoxItem>

- <xref:System.Windows.Controls.MenuItem>

- <xref:System.Windows.Controls.ProgressBar>

- <xref:System.Windows.Controls.RadioButton>

- <xref:System.Windows.Controls.Slider>

- <xref:System.Windows.Controls.TabControl>

- <xref:System.Windows.Controls.TabItem>

- <xref:System.Windows.Controls.TextBox>

<xref:System.Windows.Controls.Page> がこれらのコントロールを使用している場合、次の図に示すように、**お気に入りの色**<xref:System.Windows.Controls.ListBox> に示されているように、これらのコントロールに入力されたデータは <xref:System.Windows.Controls.Page> ナビゲーション全体で記憶されます。

![状態を記憶するコントロールを含むページ](./media/navigation-overview/data-remembered-across-page-navigations.png "入力したデータはページナビゲーション間で記憶されます。")

<xref:System.Windows.Controls.Page> に上記のリスト以外のコントロールが含まれている場合、または状態がカスタムオブジェクトに格納されている場合は、<xref:System.Windows.Controls.Page> ナビゲーション全体で履歴を記憶するようにコードを記述する必要があります。

<xref:System.Windows.Controls.Page> ナビゲーション全体で小さな状態を記憶する必要がある場合は、<xref:System.Windows.FrameworkPropertyMetadata.Journal%2A?displayProperty=nameWithType> メタデータフラグで構成されている依存関係プロパティ (<xref:System.Windows.DependencyProperty>を参照) を使用できます。

ナビゲーション間で <xref:System.Windows.Controls.Page> が記憶する必要のある状態が複数のデータで構成されている場合は、1つのクラスで状態をカプセル化して <xref:System.Windows.Navigation.IProvideCustomContentState> インターフェイスを実装することで、コードの負担が少なくなることがあります。

<xref:System.Windows.Controls.Page> 自体から移動せずに、1つの <xref:System.Windows.Controls.Page>のさまざまな状態を移動する必要がある場合は、<xref:System.Windows.Navigation.IProvideCustomContentState> と <xref:System.Windows.Navigation.NavigationService.AddBackEntry%2A?displayProperty=nameWithType>を使用できます。

<a name="Cookies"></a>

### <a name="cookies"></a>Cookies

WPF アプリケーションがデータを格納できるもう1つの方法は、<xref:System.Windows.Application.SetCookie%2A> および <xref:System.Windows.Application.GetCookie%2A> メソッドを使用して作成、更新、および削除された cookie を使用することです。 WPF で作成できる cookie は、他の種類の Web アプリケーションで使用される cookie と同じです。cookie は、アプリケーションセッション中またはアプリケーションセッション間でクライアントコンピューター上のアプリケーションによって保存される任意のデータです。 クッキー データは、通常、次の形式の名前と値のペアです。

*Name* `=` *Value*

クッキーが設定される場所の <xref:System.Uri> と共に <xref:System.Windows.Application.SetCookie%2A>にデータが渡されると、cookie はメモリ内に作成され、現在のアプリケーションセッションの間のみ使用できます。 この種類の cookie は、*セッション cookie*と呼ばれます。

複数のアプリケーション セッションにまたがってクッキーを格納するには、次の形式を使用して、有効期限をクッキーに追加する必要があります。

*NAME* `=` *VALUE* `; expires=DAY, DD-MMM-YYYY HH:MM:SS GMT`

有効期限が切れたクッキーは、cookie の有効期限が切れるまで、現在の Windows インストールのインターネット一時ファイルフォルダーに格納されます。 このような cookie は、アプリケーションセッション間で保持されるため、*永続的なクッキー*と呼ばれます。

<xref:System.Windows.Application.GetCookie%2A> メソッドを呼び出して、セッションと永続的な cookie の両方を取得し、<xref:System.Windows.Application.SetCookie%2A> メソッドを使用して cookie が設定されている場所の <xref:System.Uri> を渡します。

WPF でクッキーがサポートされるいくつかの方法を次に示します。

- WPF スタンドアロンアプリケーションと Xbap は、両方ともクッキーを作成および管理できます。

- XBAP によって作成された cookie は、ブラウザーからアクセスできます。

- 同じドメインの Xbap は、cookie を作成および共有できます。

- Xbap と同じドメインの HTML ページでは、cookie を作成して共有できます。

- Xbap およびルース [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページが Web 要求を行うときに、cookie がディスパッチされます。

- IFRAME でホストされているトップレベルの Xbap と Xbap は、どちらもクッキーにアクセスできます。

- WPF での Cookie のサポートは、サポートされているすべてのブラウザーで同じです。

- Internet Explorer では、cookie に関連する P3P ポリシーは、特にファーストパーティおよびサードパーティの Xbap に関して、WPF によって受け入れられます。

<a name="Structured_Navigation"></a>

### <a name="structured-navigation"></a>構造化ナビゲーション

ある <xref:System.Windows.Controls.Page> から別のにデータを渡す必要がある場合は、<xref:System.Windows.Controls.Page>のパラメーターなしのコンストラクターにデータを引数として渡すことができます。 この手法を使用する場合は、<xref:System.Windows.Controls.Page> を有効な状態に保つ必要があることに注意してください。それ以外の場合、次に <xref:System.Windows.Controls.Page>に移動したときに、WPF はパラメーターなしのコンストラクターを使用して <xref:System.Windows.Controls.Page> を再インスタンス化します。

または、<xref:System.Windows.Controls.Page> に渡す必要があるデータで設定されたプロパティを実装できます。 ただし、<xref:System.Windows.Controls.Page> がデータを移動する <xref:System.Windows.Controls.Page> にデータを渡す必要がある場合は、注意が必要です。 問題は、ナビゲーションによって、移動後に <xref:System.Windows.Controls.Page> が返されることを保証するメカニズムがネイティブでサポートされないことです。 基本的に、ナビゲーションは、"呼び出す/戻る" というセマンティクスをサポートしていません。 この問題を解決するために、WPF には <xref:System.Windows.Navigation.PageFunction%601> クラスが用意されています。このクラスを使用すると、予測可能で構造化された方法で <xref:System.Windows.Controls.Page> がに返されるようにすることができます。 詳細については、「[構造化ナビゲーションの概要](structured-navigation-overview.md)」を参照してください。

<a name="The_NavigationWindow_Class"></a>

## <a name="the-navigationwindow-class"></a>NavigationWindow クラス

ここまでで、ナビゲート可能なコンテンツを含むアプリケーションをビルドするために使用する可能性が最も高いナビゲーション サービスの全容を説明しました。 Xbap に限定されませんが、これらのサービスについては Xbap のコンテキストで説明しました。 最新のオペレーティングシステムと Windows アプリケーションでは、最新のユーザーのブラウザーエクスペリエンスを利用して、ブラウザー形式のナビゲーションをスタンドアロンアプリケーションに組み込むことができます。 一般的な例は、次のとおりです。

- **Word の類義語辞典**: 選択可能な単語をナビゲートします。

- **ファイル エクスプ ローラー**: ファイルとフォルダーをナビゲートします。

- **ウィザード**: 複雑なタスクを複数のページに分割し、ページ間をナビゲートできます。 例として、windows の機能の追加と削除を処理する Windows コンポーネントウィザードがあります。

スタンドアロンアプリケーションにブラウザースタイルのナビゲーションを組み込むには、<xref:System.Windows.Navigation.NavigationWindow> クラスを使用できます。 <xref:System.Windows.Navigation.NavigationWindow> は <xref:System.Windows.Window> から派生し、Xbap が提供するナビゲーションと同じサポートを使用して拡張します。 <xref:System.Windows.Navigation.NavigationWindow> は、スタンドアロンアプリケーションのメインウィンドウとして使用することも、ダイアログボックスなどのセカンダリウィンドウとして使用することもできます。

WPF の最上位レベルのクラス (<xref:System.Windows.Window>、<xref:System.Windows.Controls.Page>など) と同様に <xref:System.Windows.Navigation.NavigationWindow>を実装するには、マークアップと分離コードの組み合わせを使用します。 これを次の例に示します。

[!code-xaml[IntroToNavNavigationWindowSnippets#NavigationWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml#navigationwindowmarkup)]

[!code-csharp[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml.cs#navigationwindowcodebehind)]
[!code-vb[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/MainWindow.xaml.vb#navigationwindowcodebehind)]

このコードは、<xref:System.Windows.Navigation.NavigationWindow> を開いたときに <xref:System.Windows.Controls.Page> (ホームページ) に自動的に移動する <xref:System.Windows.Navigation.NavigationWindow> を作成します。 <xref:System.Windows.Navigation.NavigationWindow> がメインアプリケーションウィンドウである場合は、`StartupUri` 属性を使用して起動できます。 これを次のマークアップに示します。

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchNavWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/App.xaml#applaunchnavwindow)]

次の図は、スタンドアロンアプリケーションのメインウィンドウとして <xref:System.Windows.Navigation.NavigationWindow> を示しています。

![メインウィンドウ](./media/navigation-overview/navigation-window-as-main-window.png "メインウィンドウとしてのナビゲーションウィンドウ")

この図からは、前の例の <xref:System.Windows.Navigation.NavigationWindow> の実装コードで設定されていなくても、<xref:System.Windows.Navigation.NavigationWindow> にタイトルがあることがわかります。 代わりに、次のコードに示すように、<xref:System.Windows.Controls.Page.WindowTitle%2A> プロパティを使用してタイトルを設定します。

[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup1)]
[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup2)]

<xref:System.Windows.Controls.Page.WindowWidth%2A> と <xref:System.Windows.Controls.Page.WindowHeight%2A> のプロパティを設定すると、<xref:System.Windows.Navigation.NavigationWindow>も影響を及ぼします。

通常、動作または外観をカスタマイズする必要がある場合は、独自の <xref:System.Windows.Navigation.NavigationWindow> を実装します。 どちらも行わない場合は、ショートカットを使用できます。 スタンドアロンアプリケーションの <xref:System.Windows.Application.StartupUri%2A> として <xref:System.Windows.Controls.Page> のパック URI を指定すると、<xref:System.Windows.Application> によって、<xref:System.Windows.Controls.Page>をホストするための <xref:System.Windows.Navigation.NavigationWindow> が自動的に作成されます。 次のマークアップは、この方法を示しています。

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchPage](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/AnotherApp.xaml#applaunchpage)]

ダイアログボックスなどのセカンダリアプリケーションウィンドウを <xref:System.Windows.Navigation.NavigationWindow>にする場合は、次の例のコードを使用して開くことができます。

[!code-csharp[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/DialogOwnerWindow.xaml.cs#createnwdialogbox)]
[!code-vb[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/DialogOwnerWindow.xaml.vb#createnwdialogbox)]

次の図に、結果を示します。

![ダイアログボックス](./media/navigation-overview/navigation-window-as-dialog-box.png "ダイアログボックスとしてのナビゲーションウィンドウ")

ご覧のように、<xref:System.Windows.Navigation.NavigationWindow> には、ユーザーが journal を操作できるようにする Internet Explorer スタイルの **[戻る]** ボタンと **[進む]** ボタンが表示されます。 これらのボタンは、次の図に示されているように、同じユーザー エクスペリエンスを提供します。

![NavigationWindow の [戻る] ボタンと [進む] ボタン](./media/navigation-overview/back-and-forward-buttons-in-navigation-window.png "ナビゲーションウィンドウの [戻る] ボタンと [進む] ボタン")

ページに独自のジャーナルナビゲーションサポートと UI が用意されている場合は、[<xref:System.Windows.Navigation.NavigationWindow.ShowsNavigationUI%2A>] プロパティの値を `false`に設定することによって、<xref:System.Windows.Navigation.NavigationWindow> によって表示される **[戻る]** ボタンと **[進む]** ボタンを非表示にすることができます。

または、WPF のカスタマイズサポートを使用して、<xref:System.Windows.Navigation.NavigationWindow> 自体の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を置き換えることもできます。

<a name="Frame_in_Standalone_Applications"></a>

## <a name="the-frame-class"></a>Frame クラス

ブラウザーと <xref:System.Windows.Navigation.NavigationWindow> はどちらも、誘導可能なコンテンツをホストするウィンドウです。 場合によっては、アプリケーションには、ウィンドウ全体でホストする必要のないコンテンツがあることもあります。 このようなコンテンツは、代わりに、他のコンテンツ内でホストされます。 <xref:System.Windows.Controls.Frame> クラスを使用して、他のコンテンツに誘導可能なコンテンツを挿入できます。 <xref:System.Windows.Controls.Frame> は <xref:System.Windows.Navigation.NavigationWindow> と Xbap と同じサポートを提供します。

次の例は、`Frame` 要素を使用して <xref:System.Windows.Controls.Page> に宣言によって <xref:System.Windows.Controls.Frame> を追加する方法を示しています。

[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml3)]

このマークアップは、`Frame` 要素の `Source` 属性に、<xref:System.Windows.Controls.Frame> が最初に移動する <xref:System.Windows.Controls.Page> のパック URI を設定します。 次の図は、複数のページ間を移動する <xref:System.Windows.Controls.Frame> を持つ <xref:System.Windows.Controls.Page> を持つ XBAP を示しています。

![複数のページ間を移動するフレーム](./media/navigation-overview/frame-navigation-between-multiple-pages.png "これは、複数のページ間のフレームナビゲーションを示しています。")

<xref:System.Windows.Controls.Page>のコンテンツ内で <xref:System.Windows.Controls.Frame> を使用する必要があるだけではありません。 また、<xref:System.Windows.Window>のコンテンツ内の <xref:System.Windows.Controls.Frame> をホストすることもよくあります。

既定では、<xref:System.Windows.Controls.Frame> は、別の履歴がない場合にのみ独自の履歴を使用します。 <xref:System.Windows.Controls.Frame> が <xref:System.Windows.Navigation.NavigationWindow> または XBAP の内部でホストされているコンテンツの一部である場合、<xref:System.Windows.Controls.Frame> は <xref:System.Windows.Navigation.NavigationWindow> または XBAP に属するジャーナルを使用します。 ただし、場合によっては、<xref:System.Windows.Controls.Frame> が独自のジャーナルを担当する必要があります。 その理由の1つは、<xref:System.Windows.Controls.Frame>でホストされているページ内での journal ナビゲーションを許可することです。 これを次の図に示します。

![フレームとページのダイアグラム](./media/navigation-overview/journal-navigation-within-pages-hosted-by-a-frame.png "これは、フレームによってホストされるページ内の journal ナビゲーションを示しています。")

この場合、<xref:System.Windows.Controls.Frame> の <xref:System.Windows.Controls.Frame.JournalOwnership%2A> プロパティを <xref:System.Windows.Navigation.JournalOwnership.OwnsJournal>に設定することによって、独自のジャーナルを使用するように <xref:System.Windows.Controls.Frame> を構成できます。 これを次のマークアップに示します。

[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml3)]

次の図は、独自の履歴を使用する <xref:System.Windows.Controls.Frame> 内で移動した場合の影響を示しています。

![独自の履歴を使用するフレーム](./media/navigation-overview/frame-uses-its-own-journal.png "これは、独自の履歴を使用するフレーム内で移動した場合の効果を示しています。")

履歴エントリは、Internet Explorer ではなく <xref:System.Windows.Controls.Frame>のナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] によって表示されることに注意してください。

> [!NOTE]
> <xref:System.Windows.Controls.Frame> が <xref:System.Windows.Window>でホストされているコンテンツの一部である場合、<xref:System.Windows.Controls.Frame> は独自の履歴を使用します。その結果、独自のナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]が表示されます。

ユーザーエクスペリエンスで、ナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を表示せずに独自の履歴を提供するために <xref:System.Windows.Controls.Frame> が必要な場合は、<xref:System.Windows.Controls.Frame.NavigationUIVisibility%2A> を <xref:System.Windows.Visibility.Hidden>に設定することにより、ナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を非表示にすることができます。 これを次のマークアップに示します。

[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml3)]

<a name="Navigation_Hosts"></a>

## <a name="navigation-hosts"></a>ナビゲーション ホスト

<xref:System.Windows.Controls.Frame> と <xref:System.Windows.Navigation.NavigationWindow> は、ナビゲーションホストと呼ばれるクラスです。 *ナビゲーションホスト*は、コンテンツに移動して表示できるクラスです。 これを実現するために、各ナビゲーションホストは独自の <xref:System.Windows.Navigation.NavigationService> とジャーナルを使用します。 ナビゲーション ホストの基本的な構造を次の図に示します。

![ナビゲーターの図](./media/navigation-overview/navigation-host-construction.png "ナビゲーションホストの基本的な構築")

基本的に、これにより、<xref:System.Windows.Navigation.NavigationWindow> と <xref:System.Windows.Controls.Frame> は、ブラウザーでホストされるときに XBAP と同じナビゲーションサポートを提供できます。

<xref:System.Windows.Navigation.NavigationService> とジャーナルを使用するだけでなく、ナビゲーションホストは、を実装 <xref:System.Windows.Navigation.NavigationService> 同じメンバーを実装します。 これを次の図に示します。

![フレーム内および NavigationWindow 内のジャーナル](./media/navigation-overview/navigation-window-and-frame.png "ナビゲーションウィンドウとフレーム")

これにより、これらのメンバーに対して直接、ナビゲーション サポートをプログラミングできます。 <xref:System.Windows.Window>でホストされている <xref:System.Windows.Controls.Frame> にカスタムナビゲーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を提供する必要がある場合は、この点を考慮してください。 さらに、どちらの種類も、`BackStack` (<xref:System.Windows.Navigation.NavigationWindow.BackStack%2A?displayProperty=nameWithType>、<xref:System.Windows.Controls.Frame.BackStack%2A?displayProperty=nameWithType>) と `ForwardStack` (<xref:System.Windows.Navigation.NavigationWindow.ForwardStack%2A?displayProperty=nameWithType>、<xref:System.Windows.Controls.Frame.ForwardStack%2A?displayProperty=nameWithType>) を含む追加のナビゲーション関連のメンバーを実装します。これにより、バックスタックと転送スタックの履歴エントリをそれぞれ列挙できます。

前に述べたように、アプリケーション内に複数の履歴が存在することがあります。 次の図は、この例を示しています。

![1つのアプリケーション内の複数のジャーナル](./media/navigation-overview/multiple-journals-in-one-application.png "アプリケーション内の複数のジャーナルの例を次に示します。")

<a name="Navigating_to_Content_Other_than_Pages"></a>

## <a name="navigating-to-content-other-than-xaml-pages"></a>XAML ページ以外のコンテンツへのナビゲート

このトピックでは、<xref:System.Windows.Controls.Page> とパック Xbap を使用して WPF のさまざまなナビゲーション機能を示しています。 ただし、アプリケーションにコンパイルされた <xref:System.Windows.Controls.Page> は、移動できる唯一の種類のコンテンツではなく、パック Xbap がコンテンツを識別する唯一の方法ではありません。

このセクションで示すように、ルース [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル、HTML ファイル、およびオブジェクトに移動することもできます。

<a name="Navigating_to_Loose_XAML_Files"></a>

### <a name="navigating-to-loose-xaml-files"></a>Loose XAML ファイルへのナビゲート

疎 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルは、次の特性を持つファイルです。

- [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] のみが含まれています (つまり、コードがありません)。

- 適切な名前空間宣言がある。

- .xaml ファイル名拡張子を持つ。

たとえば、次のコンテンツは、ルース [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルとして保存されているとします。

[!code-xaml[NavigationOverviewSnippets#LooseXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Person.xaml#loosexaml)]

ファイルをダブルクリックすると、ブラウザーが開き、コンテンツにナビゲートして、コンテンツを表示します。 これを次の図に示します。

![ユーザー .XAML ファイルの内容の表示](./media/navigation-overview/contents-of-person-xaml-file.png "ユーザーの .XAML ファイルの内容を表示します。")

次のものから、疎な [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルを表示できます。

- ローカル コンピューター、イントラネット、またはインターネット上の Web サイト。

- UNC (汎用名前付け規則) ファイル共有。

- ローカル ディスク。

圧縮されていない [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルは、ブラウザーのお気に入りに追加することも、ブラウザーのホームページに追加することもできます。

> [!NOTE]
> 疎 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページのパブリッシュと起動の詳細については、「 [WPF アプリケーションの配置](deploying-a-wpf-application-wpf.md)」を参照してください。

緩い [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] に対する1つの制限は、部分信頼で実行しても安全なコンテンツのみをホストできることです。 たとえば、`Window` は、ルース [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルのルート要素にすることはできません。 詳細については、「 [WPF 部分信頼セキュリティ](../wpf-partial-trust-security.md)」を参照してください。

<a name="Navigating_to_HTML_Files_Using_Frame"></a>

### <a name="navigating-to-html-files-by-using-frame"></a>フレームを使用した HTML ファイルへのナビゲート

ご想像のとおり、HTML に移動することもできます。 Http スキームを使用する URI を指定するだけで済みます。 たとえば、次の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] は、HTML ページに移動する <xref:System.Windows.Controls.Frame> を示しています。

[!code-xaml[NavigationOverviewSnippets#FrameHtmlNavMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHTMLNavPage.xaml#framehtmlnavmarkup)]

HTML に移動するには、特殊なアクセス許可が必要です。 たとえば、インターネットゾーン部分信頼セキュリティサンドボックスで実行されている XBAP から移動することはできません。 詳細については、「 [WPF 部分信頼セキュリティ](../wpf-partial-trust-security.md)」を参照してください。

<a name="Navigating_to_HTML_Files_Using_WebBrowser"></a>

### <a name="navigating-to-html-files-by-using-the-webbrowser-control"></a>WebBrowser コントロールを使用した HTML ファイルへのナビゲート

<xref:System.Windows.Controls.WebBrowser> コントロールは、HTML ドキュメントのホスト、ナビゲーション、スクリプト/マネージコードの相互運用性をサポートします。 <xref:System.Windows.Controls.WebBrowser> コントロールの詳細については、「<xref:System.Windows.Controls.WebBrowser>」を参照してください。

<xref:System.Windows.Controls.Frame>と同様に、<xref:System.Windows.Controls.WebBrowser> を使用して HTML に移動するには、特別なアクセス許可が必要です。 たとえば、部分信頼アプリケーションでは、元のサイトにある HTML にのみ移動できます。 詳細については、「 [WPF 部分信頼セキュリティ](../wpf-partial-trust-security.md)」を参照してください。

<a name="Navigating_to_Objects"></a>

### <a name="navigating-to-custom-objects"></a>カスタム オブジェクトへのナビゲート

カスタムオブジェクトとして格納されているデータがある場合、そのデータを表示する方法の1つとして、それらのオブジェクトにバインドされたコンテンツを含む <xref:System.Windows.Controls.Page> を作成します (「[データバインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください)。 オブジェクトを表示するためだけにページ全体を作成するオーバーヘッドが必要ない場合には、代わりに、オブジェクトに直接ナビゲートすることもできます。

次のコードに実装されている `Person` クラスを考えてみましょう。

[!code-csharp[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/Person.cs#personclasscode)]
[!code-vb[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/Person.vb#personclasscode)]

このファイルに移動するには、次のコードに示すように、<xref:System.Windows.Navigation.NavigationWindow.Navigate%2A?displayProperty=nameWithType> メソッドを呼び出します。

[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject1)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject2)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject3)]

[!code-csharp[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml.cs#pagethatnavstoobjectcodebehind)]
[!code-vb[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/HomePage.xaml.vb#pagethatnavstoobjectcodebehind)]

次の図に、結果を示します。

![クラスに移動するページ](./media/navigation-overview/page-navigates-to-an-object.png "これは、オブジェクトに移動するページの例です。")

この図には、役立つものが何も表示されていません。 実際に表示される値は、 **Person**オブジェクトの `ToString` メソッドの戻り値です。既定では、これは WPF がオブジェクトを表すために使用できる唯一の値です。 `ToString` メソッドをオーバーライドして、より意味のある情報を返すこともできますが、それでも文字列値になります。 WPF のプレゼンテーション機能を利用する方法の1つは、データテンプレートを使用することです。 WPF が特定の種類のオブジェクトに関連付けることができるデータテンプレートを実装できます。 次のコードは、`Person` オブジェクトのデータテンプレートを示しています。

[!code-xaml[NavigateToObjectSnippets#DataTemplateMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/App.xaml#datatemplatemarkup)]

ここでは、データテンプレートは、`DataType` 属性の `x:Type` マークアップ拡張機能を使用して、`Person` 型に関連付けられています。 次に、データテンプレートは `TextBlock` 要素 (<xref:System.Windows.Controls.TextBlock>を参照) を `Person` クラスのプロパティにバインドします。 次の図は、`Person` オブジェクトの更新された外観を示しています。

![データテンプレートを持つクラスへの移動](./media/navigation-overview/navigating-to-a-class.png "データテンプレートを持つクラスへの移動。")

この方法の利点は、データ テンプレートを再利用して、アプリケーションの任意の場所で オブジェクトを一貫して表示できることによって得られる一貫性です。

データテンプレートの詳細については、「データテンプレートの[概要](../data/data-templating-overview.md)」を参照してください。

<a name="Security"></a>

## <a name="security"></a>セキュリティ

WPF ナビゲーションサポートによって、Xbap がインターネット経由で移動できるようになり、アプリケーションでサードパーティのコンテンツをホストできるようになります。 アプリケーションとユーザーの両方を有害な動作から保護するために、WPF には、[セキュリティ](../security-wpf.md)と[Wpf 部分信頼セキュリティ](../wpf-partial-trust-security.md)について説明されているさまざまなセキュリティ機能が用意されています。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Application.SetCookie%2A>
- <xref:System.Windows.Application.GetCookie%2A>
- [アプリケーション管理の概要](application-management-overview.md)
- [WPF におけるパッケージの URI](pack-uris-in-wpf.md)
- [構造化ナビゲーションの概要](structured-navigation-overview.md)
- [ナビゲーション トポロジの概要](navigation-topologies-overview.md)
- [方法のトピック](navigation-how-to-topics.md)
- [WPF アプリケーションの配置](deploying-a-wpf-application-wpf.md)
