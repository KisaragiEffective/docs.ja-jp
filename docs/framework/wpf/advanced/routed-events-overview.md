---
title: ルーティング イベントの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- attached events [WPF]
- grouped button set [WPF]
- routed events [WPF]
- events [WPF], routed
- tunneling [WPF]
- events [WPF], attached
- routing strategies for events [WPF]
- button set [WPF], grouped
- bubbling [WPF]
ms.assetid: 1a2189ae-13b4-45b0-b12c-8de2e49c29d2
ms.openlocfilehash: f47eccac4e960bd6869da0da139803cd4e433393
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794301"
---
# <a name="routed-events-overview"></a>ルーティング イベントの概要

このトピックでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] でのルーティング イベントの概念について説明します。 ここでは、ルーティング イベントの用語を定義し、要素ツリーを通じたルーティング イベントのルーティング方法、ルーティング イベントの処理方法、カスタム ルーティング イベントの作成方法について説明します。

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

このトピックでは、共通言語ランタイム (CLR) とオブジェクト指向プログラミングについての基本的な知識があること、および [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素間のリレーションシップをツリーとして概念化する方法の概念を理解していることを前提としています。 このトピックの例に従うには、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] について理解し、ごく基本的な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションまたはページを作成できる必要があります。 詳細については、「[チュートリアル: 初めての wpf デスクトップアプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)と[XAML の概要 (wpf)](../../../desktop-wpf/fundamentals/xaml.md)」を参照してください。

<a name="routing"></a>

## <a name="what-is-a-routed-event"></a>ルーティング イベントとは

ルーティング イベントについては、機能または実装の観点から考えることができます。 どちらを便利と感じるかは個人によって異なるため、ここでは両方の見方を提示します。

観点を機能に置いた場合、ルーティング イベントは、イベントを生成したオブジェクト上だけでなく、要素ツリー内の複数のリスナー上でハンドラーを呼び出すことができる種類のイベントです。

実装定義: ルーティングイベントは、<xref:System.Windows.RoutedEvent> クラスのインスタンスによってサポートされ、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] イベントシステムによって処理される CLR イベントです。

一般的な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションには、多数の要素が含まれます。 コードで作成したか [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の宣言によって作成したかにかかわらず、これらの要素は互いに要素ツリー リレーションシップにあります。 イベントのルーティングは、イベント定義に従って 2 つの方向のいずれかをたどることができますが、一般にはルーティングは発生元要素から要素ツリーに沿って "浮上" し、最終的には要素ツリーのルート (通常はページまたはウィンドウ) に到達します。 このバブル (浮上) の概念は、DHTML オブジェクト モデルで使用される概念と似ています。

たとえば、次のような単純な要素ツリーがあるとします。

[!code-xaml[EventOvwSupport#GroupButton](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml#groupbutton)]

この要素ツリーでは、次のようなものが生成されます。

![[はい]、[いいえ]、および [キャンセル] ボタン](./media/routedevent-ovw-1.gif "RoutedEvent_ovw_1")

この簡略化された要素ツリーでは、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのソースが <xref:System.Windows.Controls.Button> 要素の1つであり、どちらかの <xref:System.Windows.Controls.Button> がクリックされたは、イベントを処理する機会を持つ最初の要素です。 ただし、<xref:System.Windows.Controls.Button> にアタッチされているハンドラーがイベントに対して動作しない場合、イベントは、<xref:System.Windows.Controls.StackPanel>である要素ツリーの <xref:System.Windows.Controls.Button> 親にバブルされます。 場合によっては、イベントが <xref:System.Windows.Controls.Border>にバブルした後、要素ツリーのページルートに収まりません (表示されません)。

言い換えると、この <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのイベントルートは次のようになります。

Button-->StackPanel-->Border-->...

### <a name="top-level-scenarios-for-routed-events"></a>ルーティング イベントのトップレベルのシナリオ

次に、ルーティングイベントの概念を実現するシナリオの概要と、一般的な CLR イベントがこれらのシナリオに適していなかった理由について簡単に説明します。

**コントロールの複合とカプセル化:** [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のさまざまなコントロールには、多機能のコンテンツ モデルが採用されています。 たとえば、<xref:System.Windows.Controls.Button>内にイメージを配置すると、ボタンのビジュアルツリーを効果的に拡張できます。 ただし、追加されたイメージは、ユーザーがイメージの技術的な部分にあるピクセルをクリックした場合でも、ボタンがそのコンテンツの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> に応答するようにするヒットテスト動作を中断しないようにする必要があります。

**単一ハンドラーの添付ファイルポイント:** Windows フォームでは、複数の要素から発生する可能性のあるイベントを処理するために、同じハンドラーを複数回アタッチする必要があります。 ルーティング イベントを使用すると、前の例に示したとおり、ハンドラーを一度だけアタッチし、必要に応じてハンドラーのロジックを使用してイベントの発生元を特定することができます。 たとえば、前に示した [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] では次のようなハンドラーを使用します。

[!code-csharp[EventOvwSupport#GroupButtonCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml.cs#groupbuttoncodebehind)]
[!code-vb[EventOvwSupport#GroupButtonCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/EventOvwSupport/visualbasic/default.xaml.vb#groupbuttoncodebehind)]

**クラス処理:** ルーティング イベントでは、クラスに定義した静的ハンドラーを使用できます。 このクラス ハンドラーでは、アタッチされたどのインスタンス ハンドラーよりも先にイベントを処理できます。

**リフレクションを使用しないイベント参照:** 特定のコードやマークアップのテクニックでは、特定のイベントを識別する機能が必要とされます。 ルーティングイベントによって、<xref:System.Windows.RoutedEvent> フィールドが識別子として作成されます。このフィールドは、静的または実行時のリフレクションを必要としない、堅牢なイベント識別手法を提供します。

### <a name="how-routed-events-are-implemented"></a>ルーティング イベントの実装方法

ルーティングイベントは、<xref:System.Windows.RoutedEvent> クラスのインスタンスによってサポートされ、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のイベントシステムに登録される CLR イベントです。 登録から取得した <xref:System.Windows.RoutedEvent> インスタンスは、通常、ルーティングイベントを登録して "所有" するクラスの `public` `static` `readonly` フィールドメンバーとして保持されます。 同じ名前を持つ CLR イベント ("ラッパー" イベントと呼ばれることもあります) への接続は、CLR イベントの `add` および `remove` の実装をオーバーライドすることによって実現されます。 通常、`add` と `remove` は、暗黙の既定のままにされ、そのイベントのハンドラーの追加や削除に言語固有の適切なイベント構文が使用されます。 ルーティングイベントのバッキングと接続のメカニズムは、依存関係プロパティが、<xref:System.Windows.DependencyProperty> クラスによってサポートされ、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティシステムに登録されている CLR プロパティであるという概念に似ています。

次の例は、カスタム `Tap` ルーティングイベントの宣言を示しています。これには、<xref:System.Windows.RoutedEvent> 識別子フィールドの登録と公開、`Tap` CLR イベントの `add` および `remove` 実装が含まれます。

[!code-csharp[RoutedEventCustom#AddRemoveHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/RoutedEventCustom/CSharp/SDKSampleLibrary/class1.cs#addremovehandler)]
[!code-vb[RoutedEventCustom#AddRemoveHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/RoutedEventCustom/VB/SDKSampleLibrary/Class1.vb#addremovehandler)]

### <a name="routed-event-handlers-and-xaml"></a>ルーティング イベント ハンドラーと XAML

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] を使用してイベントのハンドラーを追加するには、イベント リスナーである要素の属性としてイベント名を宣言します。 属性の値は、実装したハンドラー メソッドの名前です。このメソッドは、分離コード ファイルの部分クラス内に存在する必要があります。

[!code-xaml[EventOvwSupport#SimplestSyntax](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml#simplestsyntax)]

標準 CLR イベントハンドラーを追加するための [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 構文は、ルーティングイベントハンドラーを追加する場合と同じです。これは、その下にルーティングイベントの実装がある CLR イベントラッパーにハンドラーを追加するためです。 イベント ハンドラーを[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で追加する方法の詳細については、「[XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」を参照してください。

<a name="routing_strategies"></a>

## <a name="routing-strategies"></a>ルーティング方法

ルーティング イベントは、3 つのルーティング方法のいずれかを使用します。

- **バブル:** イベント ソースのイベント ハンドラーが呼び出されます。 ルーティング イベントは、次に、要素ツリー ルートに到達するまで、連続する親要素にルーティングします。 ほとんどのルーティング イベントでは、このバブル ルーティング方法を使用します。 バブル ルーティング イベントは、一般に個別のコントロールまたはその他の UI 要素からの入力や状態変化を報告するために使用されます。

- **直接:** ソース要素自体のみに、応答としてハンドラーを呼び出す機会が与えられます。 これは、Windows フォームがイベントに使用する "ルーティング" に似ています。 ただし、標準の CLR イベントとは異なり、ダイレクトルーティングイベントはクラスの処理をサポートしています (クラスの処理については、今後のセクションで説明します)。 <xref:System.Windows.EventSetter> と <xref:System.Windows.EventTrigger>で使用できます。

- **トンネル:**  要素ツリー ルートのイベント ハンドラーが最初に呼び出されます。 ルーティング イベントは、次に、経路沿いにルーティング イベント ソース (ルーティング イベントを発生させた要素) のノード要素まで、連続する子要素間の経路をたどります。 多くの場合にトンネル ルーティング イベントは、コントロールの複合部分として使用または処理されます。たとえば、複合部分で発生したイベントは、完全なコントロールに固有のイベントによって意図的に抑止されるか置き換えられます。 多くの場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] から提供される入力イベントはトンネルとバブルのペアとして実装されます。 トンネル イベントは、このペアに使用される名前付け規則から、プレビュー イベントと呼ばれることもあります。

<a name="why_use"></a>

## <a name="why-use-routed-events"></a>ルーティング イベントを使用する理由

アプリケーションを開発するときに、処理するイベントがルーティング イベントとして実装されているかどうかを常に確認する必要はありません。 ルーティング イベントの動作は独特ですが、イベントを発生元の要素で処理する限り、動作はあまり表面には見えません。

ルーティング イベントが効果を発揮するのは、共通ハンドラーを共通ルートに定義する、独自のコントロールを複合化する、カスタム コントロール クラスを定義するなどの、特定のシナリオの場合です。

ルーティング イベント リスナーとルーティング イベント ソースは、階層内で共通イベントを共有する必要はありません。 任意の <xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> は、任意のルーティングイベントのイベントリスナーにすることができます。 そのため、アプリケーション内のさまざまな要素がイベント情報を交換できる概念 "インターフェイス" として、動作している API セット全体で利用可能なルーティングイベントの完全なセットを使用できます。 ルーティング イベントのこの "インターフェイス" 概念は、特に入力イベントに当てはまります。

また、ルーティング イベントは、要素ツリー間の通信にも使用できます。これは、イベントのイベント データが経路上の各要素に永続的に保持されるためです。 ある要素でイベント データの一部を変更した場合、この変更は経路上の次の要素で使用できます。

ルーティングの側面以外にも、特定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントが標準の CLR イベントではなくルーティングイベントとして実装される可能性があるという2つの理由があります。 独自のイベントを実装している場合は、次の原則も考慮します。

- <xref:System.Windows.EventSetter> や <xref:System.Windows.EventTrigger> などの特定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] スタイル設定およびテンプレート機能では、参照されるイベントがルーティングイベントである必要があります。 これは、前に説明したイベント識別子シナリオです。

- ルーティング イベントは、クラス処理機構をサポートしています。この機構により、クラスに静的メソッドを指定して、登録されたインスタンス ハンドラーがルーティング イベントにアクセスする前にこの静的メソッドでイベントを処理することができます。 インスタンスでのイベント処理によって誤って抑止されずにイベント駆動のクラス動作を適用できるため、これはコントロールをデザインするときに便利です。

前に示した一連の考慮事項については、このトピックのセクションで個別に説明します。

<a name="event_handing"></a>

## <a name="adding-and-implementing-an-event-handler-for-a-routed-event"></a>ルーティング イベントのイベント ハンドラーの追加と実装

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でイベント ハンドラーを追加するには、次の例に示すように、単にイベント名を要素に属性として追加し、属性の値として、適切なデリゲートを実装するイベント ハンドラーの名前を設定します。

[!code-xaml[EventOvwSupport#SimplestSyntax](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml#simplestsyntax)]

`b1SetColor` は、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントを処理するコードを含む実装されたハンドラーの名前です。 `b1SetColor` は、<xref:System.Windows.RoutedEventHandler> デリゲートと同じシグネチャを持つ必要があります。これは、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのイベントハンドラーデリゲートです。 すべてのルーティング イベント ハンドラー デリゲートの 1 番目のパラメーターでは、イベント ハンドラーの追加先の要素を指定し、2 番目のパラメーターでは、イベントのデータを指定します。

[!code-csharp[EventOvwSupport#SimpleHandlerA](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml.cs#simplehandlera)]
[!code-vb[EventOvwSupport#SimpleHandlerA](~/samples/snippets/visualbasic/VS_Snippets_Wpf/EventOvwSupport/visualbasic/default.xaml.vb#simplehandlera)]

<xref:System.Windows.RoutedEventHandler> は、基本的なルーティングイベントハンドラーデリゲートです。 特定のコントロールやシナリオに特化したルーティング イベントの場合、ルーティング イベント ハンドラーに使用するデリゲートも、より特化したものとなることがあるため、特別なイベント データを転送できます。 たとえば、一般的な入力シナリオでは、<xref:System.Windows.UIElement.DragEnter> ルーティングイベントを処理する場合があります。 ハンドラーは <xref:System.Windows.DragEventHandler> デリゲートを実装する必要があります。 最も具体的なデリゲートを使用して、ハンドラー内の <xref:System.Windows.DragEventArgs> を処理し、ドラッグ操作のクリップボードペイロードを含む <xref:System.Windows.DragEventArgs.Data%2A> プロパティを読み取ることができます。

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] を使用してイベント ハンドラーを要素に追加する方法の詳細な例については、「[ルーティング イベントを処理する](how-to-handle-a-routed-event.md)」を参照してください。

コードで作成されたアプリケーションでルーティング イベントのハンドラーを追加するのは簡単です。 ルーティングイベントハンドラーは、常にヘルパーメソッド <xref:System.Windows.UIElement.AddHandler%2A> (既存のバッキングが `add`を呼び出す場合と同じメソッド) を使用して追加できます。ただし、既存の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ルーティングイベントは、一般に、ヘルパーメソッドよりも直感的な構文である言語固有のイベント構文でルーティングイベントのハンドラーを追加できるようにする、`add` および `remove` ロジックのバッキング実装を備えています。 ヘルパー メソッドの使用例を次に示します。

[!code-csharp[EventOvwSupport#AddHandlerCode](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml.cs#addhandlercode)]
[!code-vb[EventOvwSupport#AddHandlerCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/EventOvwSupport/visualbasic/default.xaml.vb#addhandlercode)]

次の例は、 C#演算子の構文を示しています (Visual Basic 逆参照の処理によって、演算子の構文が少し異なります)。

[!code-csharp[EventOvwSupport#AddHandlerPlusEquals](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml.cs#addhandlerplusequals)]
[!code-vb[EventOvwSupport#AddHandlerPlusEquals](~/samples/snippets/visualbasic/VS_Snippets_Wpf/EventOvwSupport/visualbasic/default.xaml.vb#addhandlerplusequals)]

コードでイベント ハンドラーを追加する方法の例については、「[コードを使用してイベント ハンドラーを追加する](how-to-add-an-event-handler-using-code.md)」を参照してください。

Visual Basic を使用している場合は、`Handles` キーワードを使用してハンドラー宣言の一部としてハンドラーを追加することもできます。 詳細については、「[Visual Basic と WPF のイベント処理](visual-basic-and-wpf-event-handling.md)」を参照してください。

<a name="concept_handled"></a>

### <a name="the-concept-of-handled"></a>処理済みの概念

すべてのルーティングイベントは、共通のイベントデータの基本クラスである <xref:System.Windows.RoutedEventArgs>を共有します。 <xref:System.Windows.RoutedEventArgs> は、ブール値を受け取る <xref:System.Windows.RoutedEventArgs.Handled%2A> プロパティを定義します。 <xref:System.Windows.RoutedEventArgs.Handled%2A> プロパティの目的は、<xref:System.Windows.RoutedEventArgs.Handled%2A> の値を `true`に設定することによって、ルーティングイベントを*処理*済みとしてマークするために、ルート上の任意のイベントハンドラーを有効にすることです。 経路上の 1 つの要素のハンドラーで処理された後、共有イベント データが経路上の各リスナーに再び報告されます。

<xref:System.Windows.RoutedEventArgs.Handled%2A> の値は、ルーティングイベントがルートに沿って移動するときに、そのイベントの報告方法や処理方法に影響します。 ルーティングイベントのイベントデータに <xref:System.Windows.RoutedEventArgs.Handled%2A> が `true` 場合、他の要素でそのルーティングイベントをリッスンするハンドラーは、通常、その特定のイベントインスタンスに対して呼び出されなくなります。 これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でアタッチされたハンドラーの場合も、`+=` や `Handles` など、言語固有のイベント ハンドラー アタッチ構文によって追加されたハンドラーの場合も同様です。 最も一般的なハンドラーのシナリオでは、<xref:System.Windows.RoutedEventArgs.Handled%2A> を `true` に設定してイベントを処理済みとしてマークすると、トンネリングルートまたはバブルルートに対してルーティングが "停止" されます。また、クラスハンドラーによってルートのポイントで処理されるすべてのイベントに対しても "停止" されます。

ただし、イベントデータに <xref:System.Windows.RoutedEventArgs.Handled%2A> が `true` ルーティングイベントに応答して、リスナーがハンドラーを実行できるようにする "handledEventsToo" メカニズムがあります。 つまり、イベント データを処理済みとしてマークしてもイベントのルーティングは完全には停止されません。 HandledEventsToo 機構は、コードまたは <xref:System.Windows.EventSetter>でのみ使用できます。

- コードでは、一般的な CLR イベントに対して機能する言語固有のイベント構文を使用するのではなく、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] メソッド <xref:System.Windows.UIElement.AddHandler%28System.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29> を呼び出してハンドラーを追加します。 `handledEventsToo` の値として `true` を指定します。

- <xref:System.Windows.EventSetter>で、<xref:System.Windows.EventSetter.HandledEventsToo%2A> 属性を `true`に設定します。

<xref:System.Windows.RoutedEventArgs.Handled%2A> 状態がルーティングイベントに生成する動作に加えて、<xref:System.Windows.RoutedEventArgs.Handled%2A> の概念は、アプリケーションを設計し、イベントハンドラーコードを記述する方法に影響します。 <xref:System.Windows.RoutedEventArgs.Handled%2A> は、ルーティングイベントによって公開される単純なプロトコルとして概念化できます。 このプロトコルの使用方法はお勧めしますが、<xref:System.Windows.RoutedEventArgs.Handled%2A> の値を使用する方法の概念設計は次のとおりです。

- ルーティング イベントが処理済みとしてマークされている場合、このイベントを経路上の他の要素でもう一度処理する必要はありません。

- ルーティングイベントが処理済みとしてマークされていない場合は、ルートの前にある他のリスナーがハンドラーを登録しないことを選択したか、または登録されたハンドラーがイベントデータを操作せずに <xref:System.Windows.RoutedEventArgs.Handled%2A> を `true`に設定することを選択しました。 (または、現在のリスナーがルートの最初のポイントである可能性もあります)。現在のリスナーのハンドラーには、次の3つのアクションが可能になりました。

  - アクションをまったく行いません。イベントは未処理のまま、次のリスナーにルーティングされます。

  - イベントに応答してコードを実行しますが、実行したアクションはイベントを処理済みとしてマークするのに十分とは確定されません。 イベントは次のリスナーにルーティングされます。

  - イベントに応答してコードを実行します。 実行したアクションはイベントを処理済みとしてマークするのに十分と考えられるため、ハンドラーに渡されたイベント データでイベントを処理済みとしてマークします。 イベントは次のリスナーにルーティングされますが、イベントデータには <xref:System.Windows.RoutedEventArgs.Handled%2A>=`true` があるため、`handledEventsToo` リスナーだけがさらにハンドラーを呼び出すことができます。

この概念設計は、前に説明したルーティング動作によって強化されています。ルートの前のハンドラーが既に `true`に <xref:System.Windows.RoutedEventArgs.Handled%2A> 設定されている場合でも呼び出されるルーティングイベントのハンドラーをアタッチすることは、より困難です (コードまたはスタイルでも可能です)。

<xref:System.Windows.RoutedEventArgs.Handled%2A>、ルーティングイベントのクラス処理、およびルーティングイベントを <xref:System.Windows.RoutedEventArgs.Handled%2A>としてマークする適切なタイミングに関する推奨事項の詳細については、「ルーティングイベントを[処理済みとして](marking-routed-events-as-handled-and-class-handling.md)マークする」および「クラス処理」を参照してください。

アプリケーションでは、バブル ルーティング イベントを発生元のオブジェクトで処理するのが一般的であり、イベントのルーティング特性についてはまったく考慮されません。 ただし、その場合でも、イベント データでルーティング イベントを処理済みとしてマークすることをお勧めします。そうすれば、要素ツリーの後方にある要素でそれと同じルーティング イベントにハンドラーがアタッチされている場合に起こりうる、予期しない副作用を回避できます。

<a name="class_handlers"></a>

## <a name="class-handlers"></a>クラス ハンドラー

<xref:System.Windows.DependencyObject>から派生するクラスを定義する場合は、クラスの宣言または継承されたイベントメンバーであるルーティングイベントのクラスハンドラーを定義してアタッチすることもできます。 ルーティング イベントが経路上の要素インスタンスに到達すると、クラスのインスタンスにアタッチされたどのインスタンス リスナー ハンドラーよりも先に、クラス ハンドラーが呼び出されます。

一部の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールには、特定のルーティング イベントに対する固有のクラス処理が存在します。 このため、ルーティング イベントが発生していないように見えても、実際にはクラス処理されている場合があります。また、特定の手法を使用した場合、インスタンス ハンドラーでも処理される可能性があります。 また、多くの基底クラスとコントロールからは、クラス処理の動作をオーバーライドするために使用できる仮想メソッドが提供されています。 望ましくないクラス処理の回避方法とカスタム クラスを使った独自のクラス処理定義方法の詳細については、「[ルーティング イベントの処理済みとしてのマーキング、およびクラス処理](marking-routed-events-as-handled-and-class-handling.md)」を参照してください。

<a name="attached_events"></a>

## <a name="attached-events-in-wpf"></a>WPF の添付イベント

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 言語は、*添付イベント*と呼ばれる特殊な種類のイベントも定義します。 添付イベントを使用して、任意の要素に特定のイベントのハンドラーを追加できます。 イベントを処理する要素が添付イベントを定義または継承する必要はありません。また、イベントを発生させる可能性のあるオブジェクトでも、インスタンスを処理する添付先でも、そのイベントを定義したりクラス メンバーとして "所有" したりする必要はありません。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 入力システムでは、添付イベントを多用します。 ただし、ほとんどすべての添付イベントは基本要素間で転送されます。 入力イベントは、基本要素クラスのメンバーである、非添付のルーティング イベントに等しいものとして表されます。 たとえば、基になるアタッチされたイベント <xref:System.Windows.Input.Mouse.MouseDown?displayProperty=nameWithType> は、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] またはコードで添付イベント構文を処理するのではなく、その <xref:System.Windows.UIElement> で <xref:System.Windows.UIElement.MouseDown> を使用することにより、特定の <xref:System.Windows.UIElement> でより簡単に処理できます。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の添付イベントの詳細については、「[添付イベントの概要](attached-events-overview.md)」を参照してください。

<a name="Qualifying_Event_Names_in_XAML_for_Anticipated_Routing"></a>

## <a name="qualified-event-names-in-xaml"></a>XAML の修飾イベント名

*typename*.*eventname* 添付イベント構文に似ているが厳密に言えば添付イベントの使用方法ではない、もう 1 つの構文使用方法では、子要素で発生したルーティング イベントのハンドラーをアタッチします。 対応するルーティング イベントが共通の親要素にメンバーとして含まれない場合でも、共通の親要素にハンドラーをアタッチしてイベントのルーティングを利用します。 次の例をもう一度検討します。

[!code-xaml[EventOvwSupport#GroupButton](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/default.xaml#groupbutton)]

ここでは、ハンドラーが追加される親要素リスナーが <xref:System.Windows.Controls.StackPanel>です。 ただし、宣言されたルーティングイベントのハンドラーを追加しています。これは <xref:System.Windows.Controls.Button> クラスによって発生します (実際には<xref:System.Windows.Controls.Primitives.ButtonBase> ますが、継承によって <xref:System.Windows.Controls.Button> できます)。 イベントは "所有" <xref:System.Windows.Controls.Button> ますが、ルーティングイベントシステムでは、任意のルーティングイベントのハンドラーが、共通言語ランタイム (CLR) イベントのリスナーをアタッチできる任意の <xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> インスタンスリスナーにアタッチすることが許可されます。 これらの修飾イベント属性名の既定の xmlns 名前空間は、通常、既定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] xmlns 名前空間ですが、カスタム ルーティング イベント用のプレフィックスを持つ名前空間を指定することもできます。 xmlns の詳細については、「[XAML 名前空間および WPF XAML の名前空間の割り当て](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md)」を参照してください。

<a name="how_event_processing_works"></a>

## <a name="wpf-input-events"></a>WPF の入力イベント

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プラットフォームにおけるルーティング イベントの主な使用例として、入力イベントがあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、トンネル ルーティング イベントの名前に "Preview" をプレフィックスとして付加するのが慣例です。 入力イベントは、多くの場合、バブル イベントとトンネル イベントがペアになって構成されます。 たとえば、<xref:System.Windows.ContentElement.KeyDown> イベントと <xref:System.Windows.ContentElement.PreviewKeyDown> イベントのシグネチャは同じですが、前者はバブル入力イベント、後者はトンネリング入力イベントです。 入力イベントによっては、バブル形式だけ、または直接ルーティング形式だけを持つ場合もあります。 このドキュメント内のルーティング イベントに関するトピックでは、他のルーティング方法を使用する類似ルーティング イベントがある場合は相互参照を示します。またマネージド リファレンス ページでは、各ルーティング イベントのルーティング方法について説明します。

ペアになった [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 入力イベントを実装する場合、マウス ボタンを押すなどの入力による 1 つのユーザー操作で、ペアになった両方のルーティング イベントが順次発生するようにします。 まず、トンネル イベントが発生し、その経路をたどります。 次に、バブル イベントが発生し、その経路をたどります。 この2つのイベントは、同じイベントデータインスタンスを同じように共有します。これは、バブルイベントを発生させる実装クラスの <xref:System.Windows.UIElement.RaiseEvent%2A> メソッド呼び出しが、トンネルイベントからイベントデータをリッスンし、新しい発生イベントでそのイベントを再利用するためです。 トンネル イベントのハンドラーを持つリスナーは、ルーティング イベントを処理済みとしてマークする機会を最初に与えられます (最初がクラス ハンドラーで、その次がインスタンス ハンドラーです)。 トンネル経路上の要素がルーティング イベントを処理済みとしてマークした場合、処理済みイベントのデータがバブル イベントに送信され、同等のバブル入力イベントにアタッチされた標準のハンドラーは呼び出されません。 外部からは、処理済みのバブル イベントが発生しなかったように見えます。 この処理動作が便利なのは、コントロールの複合化の場合です。この場合、すべてのヒット テスト ベースの入力イベントまたはフォーカス ベースの入力イベントが、コントロールの複合部分ではなく最終的なコントロールから報告される必要があるためです。 最終的なコントロール要素は、複合のルート近くにあるため、トンネル イベントを最初にクラス処理し、コントロール クラスをサポートするコードの一部としてそのルーティング イベントをコントロール固有のイベントで "置き換える" 機会があります。

入力イベント処理のしくみを説明するため、次の入力イベント例について考えます。 次のツリーの図は、`leaf element #2` が `PreviewMouseDown` と `MouseDown` イベントの両方のソースであることを示しています。

![イベント ルーティング ダイアグラム](./media/routed-events-overview/input-event-routing.png)

次の順序でイベントが処理されます。

1. ルート要素の `PreviewMouseDown` (トンネル)。

2. 中間要素 #1 の `PreviewMouseDown` (トンネル)。

3. ソース要素 #2 の `PreviewMouseDown` (トンネル)。

4. ソース要素 #2 の `MouseDown` (バブル)。

5. 中間要素 #1 の `MouseDown` (バブル)。

6. ルート要素の `MouseDown` (バブル)。

ルーティング イベント ハンドラー デリゲートは、2 つのオブジェクトへの参照を提供します。その 2 つは、イベントを発生したオブジェクトと、ハンドラーが呼び出されたオブジェクトです。 ハンドラーが呼び出されたオブジェクトは、`sender` パラメーターによって報告されるオブジェクトです。 イベントが最初に発生したオブジェクトは、イベントデータの <xref:System.Windows.RoutedEventArgs.Source%2A> プロパティによって報告されます。 ルーティングイベントは、同じオブジェクトでも発生して処理できます。この場合、`sender` と <xref:System.Windows.RoutedEventArgs.Source%2A> は同じです (これは、イベント処理の例の一覧にある手順3と4の場合)。

トンネルとバブルにより、親要素は、<xref:System.Windows.RoutedEventArgs.Source%2A> がその子要素の1つである入力イベントを受け取ります。 ソース要素の内容を把握することが重要な場合は、<xref:System.Windows.RoutedEventArgs.Source%2A> プロパティにアクセスして、ソース要素を識別できます。

通常、入力イベントが <xref:System.Windows.RoutedEventArgs.Handled%2A>としてマークされると、それ以降のハンドラーは呼び出されません。 一般に、入力イベントが意味することをアプリケーション固有の方法で論理処理するハンドラーが呼び出されたら、直ちに入力イベントを処理済みとしてマークする必要があります。

<xref:System.Windows.RoutedEventArgs.Handled%2A> 状態に関するこの一般的なステートメントの例外として、イベントデータの <xref:System.Windows.RoutedEventArgs.Handled%2A> の状態を意図的に無視するように登録されている入力イベントハンドラーは、どちらのルートでも呼び出されます。 詳細については、「[プレビュー イベント](preview-events.md)」または「[ルーティング イベントの処理済みとしてのマーキング、およびクラス処理](marking-routed-events-as-handled-and-class-handling.md)」を参照してください。

トンネル イベントとバブル イベント間の共有イベント データ モデルの概念も、トンネル イベントとバブル イベントの順次発生の概念も、すべてのルーティング イベントに当てはまるとは限りません。 そうした動作は、ペアになった入力イベントを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 入力デバイスがどのように発生させ接続するよう選択するかによって、個別に実装されます。 独自の入力イベントの実装は高度なシナリオですが、独自の入力イベントでそうしたモデルを採用することもできます。

一部のクラスでは、特定の入力イベントをクラス処理します。通常、その目的は、ユーザーによる特定の入力イベントの意味をそのコントロール内で再定義して、新しいイベントを発生させることです。 詳細については、「[ルーティング イベントの処理済みとしてのマーキング、およびクラス処理](marking-routed-events-as-handled-and-class-handling.md)」を参照してください。

入力と一般的なアプリケーション シナリオにおける入力とイベントの対話方法の詳細については、「[入力の概要](input-overview.md)」を参照してください。

<a name="events_styles"></a>

## <a name="eventsetters-and-eventtriggers"></a>EventSetter と EventTrigger

スタイルでは、<xref:System.Windows.EventSetter>を使用して、事前に宣言された [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] イベント処理構文をマークアップに含めることができます。 そのスタイルを適用すると、参照されているハンドラーが、スタイルが適用されるインスタンスに追加されます。 <xref:System.Windows.EventSetter> は、ルーティングイベントに対してのみ宣言できます。 次に例を示します。 ここで参照されている `b1SetColor` メソッドは、分離コード ファイル内にあります。

[!code-xaml[EventOvwSupport#XAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/EventOvwSupport/CSharp/page2.xaml#xaml2)]

ここで得られる利点は、スタイルには、アプリケーションの任意のボタンに適用される可能性があるその他の多くの情報が含まれている可能性があります。また、<xref:System.Windows.EventSetter> をそのスタイルの一部にすることで、マークアップレベルでもコードの再利用が促進されます。 また、<xref:System.Windows.EventSetter> は、一般的なアプリケーションやページマークアップからさらに一歩離れたハンドラーのメソッド名を抽象化します。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のルーティングイベント機能とアニメーション機能を組み合わせたもう1つの特殊な構文は、<xref:System.Windows.EventTrigger>です。 <xref:System.Windows.EventSetter>と同様に、<xref:System.Windows.EventTrigger>にはルーティングイベントのみを使用できます。 通常、<xref:System.Windows.EventTrigger> はスタイルの一部として宣言されますが、<xref:System.Windows.EventTrigger> は <xref:System.Windows.FrameworkElement.Triggers%2A> コレクションの一部として、または <xref:System.Windows.Controls.ControlTemplate>内で、ページレベルの要素で宣言することもできます。 <xref:System.Windows.EventTrigger> を使用すると、ルーティングイベントが、そのイベントの <xref:System.Windows.EventTrigger> を宣言するルート内の要素に到達するたびに実行される <xref:System.Windows.Media.Animation.Storyboard> を指定できます。 イベントを処理するだけでなく、既存のストーリーボードを起動させる <xref:System.Windows.EventTrigger> の利点は、<xref:System.Windows.EventTrigger> がストーリーボードとその実行時の動作をより適切に制御できることです。 詳細については、「[開始後のストーリーボードをイベント トリガーを使用して制御する](../graphics-multimedia/how-to-use-event-triggers-to-control-a-storyboard-after-it-starts.md)」を参照してください。

<a name="more_about"></a>

## <a name="more-about-routed-events"></a>ルーティング イベントの詳細

このトピックの主な目的は、ルーティング イベントの基本概念を説明し、さまざまな基本要素や基本コントロール内の既存のルーティング イベントに応答する方法とタイミングについて解説することです。 しかし、独自のルーティング イベントを、特殊なイベント データ クラスやデリゲートなど、必要な支援機能すべてと共に、カスタム クラスに作成することもできます。 ルーティングイベントの所有者は任意のクラスにすることができますが、ルーティングイベントはによって発生し、<xref:System.Windows.UIElement> または <xref:System.Windows.ContentElement> 派生クラスによって処理される必要があります。 カスタム イベントの詳細については、「[カスタム ルーティング イベントを作成する](how-to-create-a-custom-routed-event.md)」を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.EventManager>
- <xref:System.Windows.RoutedEvent>
- <xref:System.Windows.RoutedEventArgs>
- [ルーティング イベントの処理済みとしてのマーキング、およびクラス処理](marking-routed-events-as-handled-and-class-handling.md)
- [入力の概要](input-overview.md)
- [コマンド実行の概要](commanding-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [WPF のツリー](trees-in-wpf.md)
- [弱いイベント パターン](weak-event-patterns.md)
