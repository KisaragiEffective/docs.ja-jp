---
title: コントロールの作成の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], authoring overview
- authoring overview for controls [WPF]
ms.assetid: 3d864748-cff0-4e63-9b23-d8e5a635b28f
ms.openlocfilehash: 1ac8964f915206205d5c9e6ab782fcaa59bf2a99
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975725"
---
# <a name="control-authoring-overview"></a>コントロールの作成の概要

[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] コントロール モデルの機能拡張により、新しいコントロールを作成する必要性が大幅に削減されます。 ただし、場合によっては、カスタム コントロールを作成する必要があります。 このトピックでは、カスタム コントロールを作成する必要性を最小限に抑える機能と、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] のさまざまなコントロール作成モデルについて説明します。 また、新しいコントロールを作成する方法も示します。

<a name="when_to_write_a_new_control"></a>

## <a name="alternatives-to-writing-a-new-control"></a>新しいコントロールの作成に代わる方法

従来は、既存のコントロールをカスタマイズする場合、背景色、境界線の幅、フォントのサイズなど、コントロールの標準プロパティを変更するなどの範囲に制限されていました。 これらの定義済みのパラメーター以外に、コントロールの外観や動作にまでカスタマイズを拡張しようとすると、通常、既存のコントロールを継承し、コントロールを描画するメソッドをオーバーライドして、新しいコントロールを作成する必要がありました。  その方法は今でも選択できますが、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の場合、リッチ コンテンツ モデル、スタイル、テンプレート、トリガーを使用して、既存のコントロールをカスタマイズできます。 新しいコントロールを作成しなくても、これらの機能を使用して、カスタマイズされた一貫性のあるエクスペリエンスを得られる方法としては、次のような例が挙げられます。

- **リッチ コンテンツ。** 標準の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの多くがリッチ コンテンツをサポートしています。 たとえば、<xref:System.Windows.Controls.Button> の content プロパティは <xref:System.Object>型であるため、理論的には <xref:System.Windows.Controls.Button>に何でも表示できます。  ボタンに画像とテキストを表示させるには、画像と <xref:System.Windows.Controls.TextBlock> を <xref:System.Windows.Controls.StackPanel> に追加し、<xref:System.Windows.Controls.StackPanel> を <xref:System.Windows.Controls.ContentControl.Content%2A> プロパティに割り当てます。 コントロールには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の視覚的要素と任意のデータを表示できるため、複雑な視覚化をサポートするために、新しいコントロールを作成したり、既存のコントロールを変更したりする必要性が少なくなります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]における <xref:System.Windows.Controls.Button> およびその他のコンテンツモデルのコンテンツモデルの詳細については、「 [WPF コンテンツモデル](wpf-content-model.md)」を参照してください。

- **スタイル** <xref:System.Windows.Style> は、コントロールのプロパティを表す値のコレクションです。 スタイルを使用すると、新しいコントロールを作成しなくても、必要なコントロールの外観と動作を備えた再利用可能な表現を作成できます。 たとえば、すべての <xref:System.Windows.Controls.TextBlock> コントロールに対して、フォントサイズが14の赤い Arial フォントを使用するとします。 そこで、リソースとしてスタイルを作成し、それに応じて、適切なプロパティを設定します。 アプリケーションに追加したすべての <xref:System.Windows.Controls.TextBlock> の外観は同じになります。

- **データ テンプレート。** <xref:System.Windows.DataTemplate> を使用すると、コントロールにデータを表示する方法をカスタマイズできます。 たとえば、<xref:System.Windows.DataTemplate> を使用すると、<xref:System.Windows.Controls.ListBox>でのデータの表示方法を指定できます。  この例については、「[データ テンプレートの概要](../data/data-templating-overview.md)」を参照してください。  データの外観をカスタマイズすることに加えて、<xref:System.Windows.DataTemplate> には UI 要素を含めることができます。これにより、カスタム Ui の柔軟性が大幅に向上します。  たとえば、<xref:System.Windows.DataTemplate>を使用すると、各項目にチェックボックスが含まれている <xref:System.Windows.Controls.ComboBox> を作成できます。

- **コントロール テンプレート。** [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の多くのコントロールでは、コントロールの構造と外観を定義するために <xref:System.Windows.Controls.ControlTemplate> を使用します。これにより、コントロールの外観とコントロールの機能が分離されます。 <xref:System.Windows.Controls.ControlTemplate>を再定義することで、コントロールの外観を大幅に変更できます。  たとえば、信号機のような外観のコントロールが必要だとします。 このコントロールのユーザー インターフェイスと機能は単純です。  コントロールは 3 つの円で構成され、一度に点灯するのはそのうちの 1 つだけです。 一部のリフレクションでは、一度に選択できるのは1つの機能だけであることがわかっている場合がありますが、<xref:System.Windows.Controls.RadioButton> の既定の外観は、ライト信号のようなものでは <xref:System.Windows.Controls.RadioButton> ないように見えます。  <xref:System.Windows.Controls.RadioButton> はコントロールテンプレートを使用してその外観を定義するため、コントロールの要件に合わせて <xref:System.Windows.Controls.ControlTemplate> を再定義し、オプションボタンを使用して信号を行うことが簡単です。

  > [!NOTE]
  > <xref:System.Windows.Controls.RadioButton> は <xref:System.Windows.DataTemplate>を使用できますが、この例では <xref:System.Windows.DataTemplate> では不十分です。  <xref:System.Windows.DataTemplate> は、コントロールのコンテンツの外観を定義します。 <xref:System.Windows.Controls.RadioButton>の場合、コンテンツは、<xref:System.Windows.Controls.RadioButton> が選択されているかどうかを示す円の右側に表示されます。  信号機の例では、オプション ボタンに必要なのは "点灯" する円だけです。 信号の外観要件は <xref:System.Windows.Controls.RadioButton>の既定の外観とは異なるため、<xref:System.Windows.Controls.ControlTemplate>を再定義する必要があります。  一般に、コントロールのコンテンツ (またはデータ) を定義するために <xref:System.Windows.DataTemplate> を使用し、<xref:System.Windows.Controls.ControlTemplate> を使用してコントロールを構造化する方法を定義します。

- **トリガー。** <xref:System.Windows.Trigger> を使用すると、新しいコントロールを作成せずに、コントロールの外観と動作を動的に変更できます。 たとえば、アプリケーションに複数の <xref:System.Windows.Controls.ListBox> コントロールがあり、各 <xref:System.Windows.Controls.ListBox> の項目を選択したときに太字と赤にする必要があるとします。 最初の性質は、<xref:System.Windows.Controls.ListBox> から継承するクラスを作成し、選択した項目の外観を変更するために <xref:System.Windows.Controls.Primitives.Selector.OnSelectionChanged%2A> メソッドをオーバーライドすることですが、より適切な方法は、選択した項目の外観を変更する <xref:System.Windows.Controls.ListBoxItem> のスタイルにトリガーを追加する方法です。 トリガーを使用すると、プロパティ値を変更したり、プロパティ値に基づいた処理を実行したりできます。 <xref:System.Windows.EventTrigger> を使用すると、イベントが発生したときにアクションを実行できます。

スタイル、テンプレート、トリガーの詳細については、「[スタイルとテンプレート](styling-and-templating.md)」を参照してください。

一般に、既存のコントロールと同じ機能を持ち、外観が異なるコントロールが必要な場合は、このセクションで説明した方法のいずれかを使用して、既存のコントロールの外観を変更できないかどうかをまず検討することをお勧めします。

<a name="models_for_control_authoring"></a>

## <a name="models-for-control-authoring"></a>コントロール作成モデル

リッチ コンテンツ モデル、スタイル、テンプレート、トリガーを使用すると、新しいコントロールを作成する必要性が最小限に抑えられます。 ただし、新しいコントロールを作成する必要がある場合は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の各種のコントロール作成モデルを理解することが重要です。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、コントロールを作成するための一般的なモデルが 3 つあり、各モデルはそれぞれ異なる機能と柔軟性レベルを備えています。 3つのモデルの基本クラスは、<xref:System.Windows.Controls.UserControl>、<xref:System.Windows.Controls.Control>、および <xref:System.Windows.FrameworkElement>です。

### <a name="deriving-from-usercontrol"></a>UserControl からの派生

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でコントロールを作成する最も簡単な方法は、<xref:System.Windows.Controls.UserControl>から派生することです。 <xref:System.Windows.Controls.UserControl>から継承するコントロールを作成する場合は、既存のコンポーネントを <xref:System.Windows.Controls.UserControl>に追加し、コンポーネントに名前を付けると共に、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]でイベントハンドラーを参照します。 次に、指定の要素を参照し、コードでイベント ハンドラーを定義します。 この開発モデルは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でのアプリケーション開発に使用されるモデルとよく似ています。

正しく構築されている場合、<xref:System.Windows.Controls.UserControl> はリッチコンテンツ、スタイル、およびトリガーの利点を活用できます。 ただし、コントロールが <xref:System.Windows.Controls.UserControl>から継承されている場合、コントロールを使用するユーザーは、<xref:System.Windows.DataTemplate> または <xref:System.Windows.Controls.ControlTemplate> を使用してその外観をカスタマイズすることはできません。  テンプレートをサポートするカスタムコントロールを作成するには、<xref:System.Windows.Controls.Control> クラスまたはその派生クラス (<xref:System.Windows.Controls.UserControl>以外) から派生する必要があります。

#### <a name="benefits-of-deriving-from-usercontrol"></a>UserControl からの派生の利点

次のすべてに該当する場合は、<xref:System.Windows.Controls.UserControl> から派生することを検討してください。

- アプリケーションの構築と同じ方法でコントロールをビルドする必要がある場合。

- コントロールが既存のコンポーネントのみで構成されている場合。

- 複雑なカスタマイズをサポートする必要がない場合。

### <a name="deriving-from-control"></a>Control からの派生

<xref:System.Windows.Controls.Control> クラスからの派生は、既存の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールの大部分で使用されるモデルです。 <xref:System.Windows.Controls.Control> クラスから継承するコントロールを作成する場合は、テンプレートを使用してそのコントロールの外観を定義します。 これにより、操作ロジックと視覚的表現とが分離されます。 また、イベントではなくコマンドとバインドを使用して UI とロジックを分離し、可能な限り <xref:System.Windows.Controls.ControlTemplate> 内の要素を参照しないようにすることもできます。  コントロールの UI とロジックが適切に分離されている場合は、コントロールのユーザーがコントロールの <xref:System.Windows.Controls.ControlTemplate> を再定義して、外観をカスタマイズできます。 カスタム <xref:System.Windows.Controls.Control> の構築は、<xref:System.Windows.Controls.UserControl>を構築するほど簡単ではありませんが、カスタム <xref:System.Windows.Controls.Control> は最も柔軟性があります。

#### <a name="benefits-of-deriving-from-control"></a>Control からの派生の利点

次のいずれかに該当する場合は、<xref:System.Windows.Controls.UserControl> クラスを使用するのではなく、<xref:System.Windows.Controls.Control> から派生することを検討してください。

- コントロールの外観をカスタマイズできるようにするには、<xref:System.Windows.Controls.ControlTemplate>を使用します。

- コントロールがさまざまなテーマをサポートする必要がある場合。

### <a name="deriving-from-frameworkelement"></a>FrameworkElement からの派生

<xref:System.Windows.Controls.UserControl> または <xref:System.Windows.Controls.Control> から派生するコントロールは、既存の要素の作成に依存します。 多くのシナリオでは、これは許容できるソリューションです。 <xref:System.Windows.FrameworkElement> から継承されたオブジェクトは <xref:System.Windows.Controls.ControlTemplate>内に存在する可能性があるためです。 しかし、場合によっては、単純な要素コンポジションでは、コントロールの外観に必要な機能を実現できないことがあります。 これらのシナリオでは、<xref:System.Windows.FrameworkElement> 上のコンポーネントに基づいて適切な選択を行います。

<xref:System.Windows.FrameworkElement>ベースのコンポーネントを構築するには、直接レンダリングとカスタム要素構成という2つの標準的な方法があります。 直接レンダリングでは、<xref:System.Windows.FrameworkElement> の <xref:System.Windows.UIElement.OnRender%2A> メソッドをオーバーライドし、コンポーネントのビジュアルを明示的に定義する <xref:System.Windows.Media.DrawingContext> 操作を提供します。 これは、<xref:System.Windows.Controls.Image> と <xref:System.Windows.Controls.Border>によって使用されるメソッドです。 カスタム要素の構成では、<xref:System.Windows.Media.Visual> 型のオブジェクトを使用して、コンポーネントの外観を構成します。 例については、「[DrawingVisual オブジェクトの使用](../graphics-multimedia/using-drawingvisual-objects.md)」を参照してください。 <xref:System.Windows.Controls.Primitives.Track> は、カスタム要素構成を使用する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 内のコントロールの例です。 同じコントロールでダイレクト レンダリングとカスタム要素コンポジションを混在させることもできます。

#### <a name="benefits-of-deriving-from-frameworkelement"></a>FrameworkElement からの派生の利点

次のいずれかに該当する場合は、<xref:System.Windows.FrameworkElement> から派生することを検討してください。

- コントロールの外観について、単純な要素コンポジションが提供する以上の厳密な制御を必要とする場合。

- 独自のレンダリング ロジックを定義して、コントロールの外観を定義する必要がある場合。

- 既存の要素を新しい方法で作成し、<xref:System.Windows.Controls.UserControl> と <xref:System.Windows.Controls.Control>で可能なものを超えています。

<a name="control_authoring_basics"></a>

## <a name="control-authoring-basics"></a>コントロール作成の基本

既に説明したように、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の最も強力な機能の 1 つは、コントロールの基本的なプロパティ設定だけでは不可能な外観や動作の変更を実現し、しかもカスタム コントロールを作成する必要がないということです。 スタイル設定、データ バインディング、トリガーの各機能は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティ システムおよび [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベント システムによって実現されています。 以降のセクションでは、カスタム コントロールのユーザーが、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に付属のコントロールと同じように、これらの機能を使用できるようにするために、カスタム コントロールの作成に使用するモデルに関係なく、従う必要があるプラクティスについて説明します。

### <a name="use-dependency-properties"></a>依存関係プロパティの使用

プロパティが依存関係プロパティである場合、以下の操作が可能です。

- スタイルのプロパティを設定する。

- プロパティをデータ ソースにバインドする。

- プロパティの値として、動的リソースを使用する。

- プロパティ名をアニメーション化する。

コントロールのプロパティがこれらの機能のいずれかをサポートす必要がある場合、それを依存関係プロパティとして実装する必要があります。 次の例では、以下の処理を実行して、`Value` という名前の依存関係プロパティを定義します。

- `ValueProperty` という名前の <xref:System.Windows.DependencyProperty> 識別子を `readonly` フィールド `static` `public` として定義します。

- <xref:System.Windows.DependencyProperty.Register%2A?displayProperty=nameWithType>を呼び出して、プロパティ名をプロパティシステムに登録し、次の値を指定します。

  - プロパティの名前。

  - プロパティの型。

  - プロパティを所有する型。

  - プロパティのメタデータ。 メタデータには、プロパティの既定値、<xref:System.Windows.CoerceValueCallback> および <xref:System.Windows.PropertyChangedCallback>が含まれています。

- `Value`という名前の CLR ラッパープロパティを定義します。これは、プロパティの `get` と `set` アクセサーを実装することによって、依存関係プロパティの登録に使用される名前と同じです。 `get` アクセサーと `set` アクセサーは、それぞれ <xref:System.Windows.DependencyObject.GetValue%2A> および <xref:System.Windows.DependencyObject.SetValue%2A> を呼び出すだけであることに注意してください。 クライアントと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] はアクセサーをバイパスし、<xref:System.Windows.DependencyObject.GetValue%2A> と <xref:System.Windows.DependencyObject.SetValue%2A> を直接呼び出すことができるため、依存関係プロパティのアクセサーに追加のロジックが含まれないようにすることをお勧めします。 たとえば、プロパティがデータ ソースにバインドされている場合、プロパティの `set` アクセサーは呼び出されません。  Get アクセサーと set アクセサーにロジックを追加するのではなく、<xref:System.Windows.ValidateValueCallback>、<xref:System.Windows.CoerceValueCallback>、および <xref:System.Windows.PropertyChangedCallback> デリゲートを使用して応答するか、変更時に値を確認します。  これらのコールバックの詳細については、「[依存関係プロパティのコールバックと検証](../advanced/dependency-property-callbacks-and-validation.md)」を参照してください。

- `CoerceValue`という名前の <xref:System.Windows.CoerceValueCallback> のメソッドを定義します。 `CoerceValue` によって、`Value` は `MinValue` 以上で `MaxValue` 以下になります。

- `OnValueChanged`という名前の <xref:System.Windows.PropertyChangedCallback>のメソッドを定義します。 `OnValueChanged` は <xref:System.Windows.RoutedPropertyChangedEventArgs%601> オブジェクトを作成し、`ValueChanged` ルーティングイベントの発生を準備します。 ルーティング イベントについては、次のセクションで説明します。

[!code-csharp[UserControlNumericUpDown#DependencyProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml.cs#dependencyproperty)]
[!code-vb[UserControlNumericUpDown#DependencyProperty](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDown/visualbasic/numericupdown.xaml.vb#dependencyproperty)]

詳細については、「[カスタム依存関係プロパティ](../advanced/custom-dependency-properties.md)」を参照してください。

### <a name="use-routed-events"></a>ルーティング イベントの使用

依存関係プロパティが CLR プロパティの概念を拡張し、機能が追加されるのと同様に、ルーティングイベントは標準の CLR イベントの概念を拡張します。 新しい [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール作成する場合、イベントをルーティング イベントとして実装することをお勧めします。ルーティング イベントは以下の機能をサポートしているためです。

- 複数のコントロールの親でイベントを処理できます。 イベントがバブル イベントの場合、要素ツリー内の単一の親はイベントをサブスクライブできます。 これにより、アプリケーション開発者は、複数のコントロールのイベントに 1 つのハンドラーで対応できます。 たとえば、コントロールが <xref:System.Windows.Controls.ListBox> 内の各項目の一部である場合 (<xref:System.Windows.DataTemplate>に含まれているため)、アプリケーション開発者は <xref:System.Windows.Controls.ListBox>に対するコントロールのイベントのイベントハンドラーを定義できます。 いずれかのコントロールでイベントが発生するたびに、そのイベント ハンドラーが呼び出されます。

- ルーティングイベントは、<xref:System.Windows.EventSetter>で使用できます。これにより、アプリケーション開発者は、スタイル内でイベントのハンドラーを指定できます。

- ルーティングイベントは、<xref:System.Windows.EventTrigger>で使用できます。これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を使用してプロパティをアニメーション化する場合に便利です。 詳しくは、「 [アニメーションの概要](../graphics-multimedia/animation-overview.md)」をご覧ください。

次に示す例では、以下の処理を実行して、ルーティング イベントを定義します。

- `ValueChangedEvent` という名前の <xref:System.Windows.RoutedEvent> 識別子を `readonly` フィールド `static` `public` として定義します。

- <xref:System.Windows.EventManager.RegisterRoutedEvent%2A?displayProperty=nameWithType> メソッドを呼び出して、ルーティングイベントを登録します。 この例では、<xref:System.Windows.EventManager.RegisterRoutedEvent%2A>を呼び出すときに、次の情報を指定します。

  - イベントの名前が `ValueChanged` であること。

  - ルーティング方法は <xref:System.Windows.RoutingStrategy.Bubble>です。つまり、ソース (イベントを発生させたオブジェクト) のイベントハンドラーが最初に呼び出され、次に、ソースの親要素のイベントハンドラーが連続して呼び出されます。その後、最も近い親要素。

  - イベントハンドラーの種類は、<xref:System.Decimal> の種類を使用して構築された <xref:System.Windows.RoutedPropertyChangedEventHandler%601>です。

  - イベントを所有する型が `NumericUpDown` であること。

- `ValueChanged` という名前のパブリック イベントを宣言し、イベント アクセサー宣言を含めます。 この例では、`add` アクセサー宣言で <xref:System.Windows.UIElement.AddHandler%2A> を呼び出し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントサービスを使用するために `remove` アクセサー宣言で <xref:System.Windows.UIElement.RemoveHandler%2A> します。

- `ValueChanged`イベントを発生させる、保護された仮想メソッド `OnValueChanged` を作成します。

[!code-csharp[UserControlNumericUpDown#RoutedEvent](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml.cs#routedevent)]
[!code-vb[UserControlNumericUpDown#RoutedEvent](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDown/visualbasic/numericupdown.xaml.vb#routedevent)]

詳細については、「[ルーティング イベントの概要](../advanced/routed-events-overview.md)」および「[カスタム ルーティング イベントを作成する](../advanced/how-to-create-a-custom-routed-event.md)」を参照してください。

### <a name="use-binding"></a>バインディングの使用

コントロールの UI とロジックを分離するには、データ バインディングを使用する方法もあります。 これは、<xref:System.Windows.Controls.ControlTemplate>を使用してコントロールの外観を定義する場合に特に重要です。 データ バインディングを使用すると、コードから UI の特定の部分を参照する必要性がなくなる場合があります。 <xref:System.Windows.Controls.ControlTemplate> 内の要素を参照しないようにすることをお勧めします。コードが <xref:System.Windows.Controls.ControlTemplate> 内の要素を参照し、<xref:System.Windows.Controls.ControlTemplate> が変更された場合、参照される要素を新しい <xref:System.Windows.Controls.ControlTemplate>に含める必要があるためです。

次の例では、`NumericUpDown` コントロールの <xref:System.Windows.Controls.TextBlock> を更新し、それに名前を割り当て、コードでテキストボックスを名前で参照しています。

[!code-xaml[UserControlNumericUpDownSimple#UIRefMarkup](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDownSimple/CSharp/NumericUpDown.xaml#uirefmarkup)]

[!code-csharp[UserControlNumericUpDownSimple#UIRefCode](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDownSimple/CSharp/NumericUpDown.xaml.cs#uirefcode)]
[!code-vb[UserControlNumericUpDownSimple#UIRefCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDownSimple/VisualBasic/NumericUpDown.xaml.vb#uirefcode)]

次の例では、バインディングを使用して同じことを実現しています。

[!code-xaml[UserControlNumericUpDown#Binding](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml#binding)]

データ バインディングの詳細については、「[データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。

### <a name="design-for-designers"></a>デザイナーに対応したデザイン

Visual Studio の WPF デザイナー (たとえば、プロパティウィンドウを使用したプロパティ編集) でカスタム WPF コントロールのサポートを受けるには、次のガイドラインに従います。  WPF デザイナーの開発の詳細については、「 [Visual Studio での XAML のデザイン](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)」を参照してください。

#### <a name="dependency-properties"></a>依存関係プロパティ

「依存関係プロパティの使用」で前述したように、CLR `get` と `set` アクセサーを実装してください。 デザイナーは、ラッパーを使用して依存関係プロパティの存在を検出する場合がありますが、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] およびコントロールのクライアントと同様、プロパティを取得または設定するときにアクセサーを呼び出す必要はありません。

#### <a name="attached-properties"></a>アタッチされるプロパティ

以下のガイドラインに従って、カスタム コントロールに添付プロパティを実装する必要があります。

- `Property` メソッドを使用して作成された*PropertyName* <xref:System.Windows.DependencyProperty.RegisterAttached%2A> フォームの <xref:System.Windows.DependencyProperty> `readonly` `public` `static` を持っている。 <xref:System.Windows.DependencyProperty.RegisterAttached%2A> に渡されるプロパティ名は*PropertyName*と一致している必要があります。

- `Set` *PropertyName* および `Get` *PropertyName* という名前の `public``static` CLR メソッドのペアを実装します。 どちらのメソッドも、最初の引数として <xref:System.Windows.DependencyProperty> から派生したクラスを受け入れる必要があります。 また、`Set` *PropertyName* メソッドでは、プロパティの登録データ型と同じ型の引数も受け取ります。 `Get` *PropertyName*メソッドでは、同じ型の値を返す必要があります。 `Set` *PropertyName*メソッドがない場合、プロパティは読み取り専用としてマークされます。

- `Set` *propertyname*および `Get`*propertyname*は、ターゲットの依存関係オブジェクトの <xref:System.Windows.DependencyObject.GetValue%2A> および <xref:System.Windows.DependencyObject.SetValue%2A> メソッドにそれぞれ直接ルーティングする必要があります。 デザイナーが添付プロパティにアクセスするには、メソッド ラッパー経由で呼び出す場合もあれば、対象の依存関係オブジェクトを直接呼び出す場合もあります。

添付プロパティの詳細については、「[添付プロパティの概要](../advanced/attached-properties-overview.md)」を参照してください。

### <a name="define-and-use-shared-resources"></a>共有リソースの定義と使用

アプリケーションと同じアセンブリにコントロールを含めることも、複数のアプリケーションが使用できる別のアセンブリにコントロールをパッケージ化することもできます。 このトピックで説明した情報の大部分は、使用する方法に関係なく適用されます。  ただし、1 つだけ例外があります。  アプリケーションと同じアセンブリ内にコントロールを配置する場合、App.xaml ファイルにグローバル リソースを自由に追加できます。 ただし、コントロールのみを含むアセンブリには <xref:System.Windows.Application> オブジェクトが関連付けられていないため、app.xaml ファイルは使用できません。

アプリケーションがリソースを検索するときは、次に示す順序で 3 つのレベルを検索します。

1. 要素レベル。

   システムは、リソースを参照する要素から検索を開始し、ルート要素に到達するまで、論理上の親のリソースの検索を継続します。

2. アプリケーション レベル。

   <xref:System.Windows.Application> オブジェクトによって定義されたリソース。

3. テーマ レベル。

   テーマ レベルのディクショナリは、Themes という名前のサブフォルダーに格納されています。  Themes フォルダー内のファイルはテーマに対応しています。  たとえば、Aero.NormalColor.xaml、Luna.NormalColor.xaml、Royale.NormalColor.xaml などのファイルがあります。  generic.xaml という名前のファイルが含まれている場合もあります。  システムがテーマ レベルでリソースを検索するとき、最初にテーマ固有のファイル内を検索し、次に generic.xaml 内を検索します。

アプリケーションとは別のアセンブリ内にコントロールを含めるときは、グローバル リソースを要素レベルまたはテーマ レベルに配置する必要があります。 どちらに配置する場合も、それぞれの利点があります。

#### <a name="defining-resources-at-the-element-level"></a>要素レベルでのリソース定義

カスタムのリソース ディクショナリを作成し、それをコントロールのリソース ディクショナリと結合することによって、共有リソースを要素レベルで定義できます。  このメソッドで定義する場合は、リソース ファイルに任意の名前を付けて、コントロールと同じフォルダーに配置できます。 要素レベルでのリソースでは、単純な文字列をキーとして使用することもできます。 次の例では、Dictionary1 という名前の <xref:System.Windows.Media.LinearGradientBrush> リソースファイルを作成します。

[!code-xaml[SharedResources#1](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/Dictionary1.xaml#1)]

ディクショナリを定義したら、それをコントロールのリソース ディクショナリにマージする必要があります。  これには、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] またはコードを使用します。

次の例では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] を使用してリソース ディクショナリを結合します。

[!code-xaml[SharedResources#2](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/ShapeResizer.xaml#2)]

この方法の欠点は、参照するたびに <xref:System.Windows.ResourceDictionary> オブジェクトが作成されることです。  たとえば、ライブラリに10個のカスタムコントロールがあり、XAML を使用して各コントロールの共有リソースディクショナリをマージする場合は、10個の同一 <xref:System.Windows.ResourceDictionary> オブジェクトを作成します。  これを回避するには、コード内のリソースをマージし、結果の <xref:System.Windows.ResourceDictionary>を返す静的クラスを作成します。

次の例では、共有 <xref:System.Windows.ResourceDictionary>を返すクラスを作成します。

[!code-csharp[SharedResources#3](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/SharedDictionaryManager.cs#3)]

次の例では、`InitializeComponent` を呼び出す前に、共有リソースをコントロールのコンス トラクター内でカスタム コントロールのリソースと結合します。  `SharedDictionaryManager.SharedDictionary` は静的プロパティであるため、<xref:System.Windows.ResourceDictionary> は1回だけ作成されます。 `InitializeComponent` が呼び出される前に、リソース ディクショナリが結合されため、コントロールは [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル内でリソースを使用できます。

[!code-csharp[SharedResources#4](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/ShapeResizer.xaml.cs#4)]

#### <a name="defining-resources-at-the-theme-level"></a>テーマ レベルでのリソース定義

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、さまざまな Windows テーマ用にリソースを作成できます。  コントロールの作成者は、特定のテーマ用のリソースを定義して、使用するテーマに応じてコントロールの外観を変更できます。 たとえば、windows クラシックテーマでの <xref:System.Windows.Controls.Button> の外観 (Windows 2000 の既定のテーマ) は、Windows Luna テーマの <xref:System.Windows.Controls.Button> (windows XP の既定のテーマ) とは異なります。これは、<xref:System.Windows.Controls.Button> がテーマごとに異なる <xref:System.Windows.Controls.ControlTemplate> を使用するためです。

テーマ固有のリソースは、固有のファイル名でリソース ディクショナリに保持されます。 これらのファイルは、コントロールが格納されているフォルダーのサブフォルダーである `Themes` フォルダー内に配置する必要があります。 次の表は、リソース ディクショナリ ファイルと、各ファイルに関連付けられているテーマを示しています。

|リソース ディクショナリ ファイル名|Windows テーマ|
|-----------------------------------|-------------------|
|`Classic.xaml`|Windows XP のクラシックな Windows 9x/2000 の外観|
|`Luna.NormalColor.xaml`|Windows XP の既定の青のテーマ|
|`Luna.Homestead.xaml`|Windows XP のオリーブのテーマ|
|`Luna.Metallic.xaml`|Windows XP のシルバーのテーマ|
|`Royale.NormalColor.xaml`|Windows XP Media Center Edition の既定テーマ|
|`Aero.NormalColor.xaml`|Windows Vista の既定テーマ|

すべてのテーマのリソースを定義する必要はありません。 特定のテーマについてリソースが定義されていない場合、コントロールはリソースの `Classic.xaml` を確認します。 現在のテーマに対応するファイルや `Classic.xaml` でリソースが定義されていない場合、コントロールは汎用のリソースを使用します。汎用のリソースは、`generic.xaml` という名前のリソース ディクショナリ ファイルにあります。  `generic.xaml` ファイルは、テーマ固有のリソース ディクショナリ ファイルと同じフォルダーに配置されています。 `generic.xaml` は、特定の Windows テーマには対応していませんが、テーマ レベルのディクショナリであることに変わりありません。

[C#](https://github.com/dotnet/samples/tree/master/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp)または[Visual Basic](https://github.com/dotnet/samples/tree/master/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic) NumericUpDown カスタムコントロールとテーマおよび UI オートメーションサポートのサンプルには、`NumericUpDown` コントロール用の2つのリソースディクショナリが含まれています。1つは汎用 .xaml で、もう1つは Luna にあります。 

テーマ固有のリソースディクショナリファイルに <xref:System.Windows.Controls.ControlTemplate> を配置する場合は、次の例に示すように、コントロールの静的コンストラクターを作成し、<xref:System.Windows.FrameworkElement.DefaultStyleKey%2A>で <xref:System.Windows.DependencyProperty.OverrideMetadata%28System.Type%2CSystem.Windows.PropertyMetadata%29> メソッドを呼び出す必要があります。

[!code-csharp[CustomControlNumericUpDownOneProject#StaticConstructor](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDownOneProject/CSharp/NumericUpDown.cs#staticconstructor)]
[!code-vb[CustomControlNumericUpDownOneProject#StaticConstructor](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDownOneProject/visualbasic/numericupdown.vb#staticconstructor)]

##### <a name="defining-and-referencing-keys-for-theme-resources"></a>テーマ リソース用のキーの定義と参照

要素レベルでリソースを定義するときに、文字列をキーとして割り当て、その文字列を使用してリソースにアクセスできます。 テーマレベルでリソースを定義する場合は、キーとして <xref:System.Windows.ComponentResourceKey> を使用する必要があります。  次の例では、generic.xaml でリソースを定義します。

[!code-xaml[ThemeResourcesControlLibrary#5](~/samples/snippets/csharp/VS_Snippets_Wpf/ThemeResourcesControlLibrary/CS/Themes/generic.xaml#5)]

次の例では、キーとして <xref:System.Windows.ComponentResourceKey> を指定することによって、リソースを参照しています。

[!code-xaml[ThemeResourcesControlLibrary#6](~/samples/snippets/csharp/VS_Snippets_Wpf/ThemeResourcesControlLibrary/CS/NumericUpDown.xaml#6)]

##### <a name="specifying-the-location-of-theme-resources"></a>テーマ リソースの場所の指定

コントロールのリソースを見つけるには、アセンブリにコントロール固有のリソースが含まれていることを、ホスト アプリケーションが認識する必要があります。 これを行うには、コントロールを含むアセンブリに <xref:System.Windows.ThemeInfoAttribute> を追加します。 <xref:System.Windows.ThemeInfoAttribute> には、汎用リソースの場所を指定する <xref:System.Windows.ThemeInfoAttribute.GenericDictionaryLocation%2A> プロパティと、テーマ固有のリソースの場所を指定する <xref:System.Windows.ThemeInfoAttribute.ThemeDictionaryLocation%2A> プロパティがあります。

次の例では、<xref:System.Windows.ThemeInfoAttribute.GenericDictionaryLocation%2A> プロパティと <xref:System.Windows.ThemeInfoAttribute.ThemeDictionaryLocation%2A> プロパティを <xref:System.Windows.ResourceDictionaryLocation.SourceAssembly>に設定して、汎用およびテーマ固有のリソースがコントロールと同じアセンブリ内にあることを指定しています。

[!code-csharp[CustomControlNumericUpDown#ThemesSection](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp/CustomControlLibrary/Properties/AssemblyInfo.cs#themessection)]
[!code-vb[CustomControlNumericUpDown#ThemesSection](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic/customcontrollibrary/my project/assemblyinfo.vb#themessection)]

## <a name="see-also"></a>関連項目

- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [WPF におけるパッケージの URI](../app-development/pack-uris-in-wpf.md)
- [コントロールのカスタマイズ](control-customization.md)
