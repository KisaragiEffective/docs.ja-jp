---
title: ストーリーボードの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Storyboard syntax [WPF]
- syntax [WPF], Storyboard
- timelines [WPF]
ms.assetid: 1a698c3c-30f1-4b30-ae56-57e8a39811bd
ms.openlocfilehash: 5c058dbaf2ae209a198a8299790d4002447ac443
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559694"
---
# <a name="storyboards-overview"></a>ストーリーボードの概要

このトピックでは、<xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用してアニメーションを整理および適用する方法について説明します。 <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを対話形式で操作する方法と、間接的なプロパティのターゲット構文について説明します。

## <a name="prerequisites"></a>Prerequisites

このトピックを理解するには、さまざまな種類のアニメーションとその基本的な機能に精通している必要があります。 アニメーションの概要については、「[アニメーションの概要](animation-overview.md)」を参照してください。 また、添付プロパティの使用方法についても理解しておく必要があります。 添付プロパティの詳細については、「[添付プロパティの概要](../advanced/attached-properties-overview.md)」を参照してください。

<a name="whatisatimeline"></a>

## <a name="what-is-a-storyboard"></a>ストーリーボードとは何か

タイムラインの型で役に立つのは、アニメーションだけではありません。 他にも一連のタイムラインの編成を容易にし、タイムラインをプロパティに適用するための Timeline クラスが提供されています。 コンテナータイムラインは <xref:System.Windows.Media.Animation.TimelineGroup> クラスから派生し、<xref:System.Windows.Media.Animation.ParallelTimeline> と <xref:System.Windows.Media.Animation.Storyboard>を含みます。

<xref:System.Windows.Media.Animation.Storyboard> は、格納されているタイムラインのターゲット情報を提供するコンテナータイムラインの一種です。 ストーリーボードには、他のコンテナーのタイムラインやアニメーションなど、任意の種類の <xref:System.Windows.Media.Animation.Timeline>を含めることができます。 <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用すると、さまざまなオブジェクトやプロパティに影響を与えるタイムラインを1つのタイムラインツリーにまとめることができるため、複雑なタイミングの動作を簡単に整理したり制御したりすることができます。 たとえば、次の 3 つのことを実行するボタンを必要としているとします。

- ユーザーに選択されると、拡大して色が変わる。

- クリックされると、いったん縮小してから元のサイズに戻る。

- 無効にされると、縮小し、50% の不透明度になる。

この場合、同じオブジェクトに適用するアニメーションのセットを複数用意し、ボタンの状態に応じてさまざまな場合に再生します。 <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用すると、アニメーションを整理し、グループ内で1つ以上のオブジェクトに適用することができます。

<a name="wherecanyouuseastoryboard"></a>

## <a name="where-can-you-use-a-storyboard"></a>ストーリーボードはどこで使用できるか

<xref:System.Windows.Media.Animation.Storyboard> は、system.windows.media.animation.animatable> クラスの依存関係プロパティをアニメーション化するために使用できます (クラス system.windows.media.animation.animatable> を作成する方法の詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください)。 ただし、storyboarding はフレームワークレベルの機能であるため、オブジェクトは <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>の <xref:System.Windows.NameScope> に属している必要があります。

たとえば、<xref:System.Windows.Media.Animation.Storyboard> を使用して、次の操作を行うことができます。

- ボタン (<xref:System.Windows.FrameworkElement>の種類) の背景を描画する <xref:System.Windows.Media.SolidColorBrush> (非フレームワーク要素) をアニメーション化します。

- <xref:System.Windows.Controls.Image> (<xref:System.Windows.FrameworkElement>) を使用して表示される <xref:System.Windows.Media.GeometryDrawing> (非フレームワーク要素) の塗りつぶしを描画する <xref:System.Windows.Media.SolidColorBrush> (非フレームワーク要素) をアニメーション化します。

- コードでは、<xref:System.Windows.FrameworkElement>を含むクラスによって宣言された <xref:System.Windows.Media.SolidColorBrush> をアニメーション化します (<xref:System.Windows.Media.SolidColorBrush> にその名前が <xref:System.Windows.FrameworkElement>で登録されている場合)。

ただし、<xref:System.Windows.Media.Animation.Storyboard> を使用して、名前が <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>に登録されていないか、または <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement>のプロパティの設定に使用されていない <xref:System.Windows.Media.SolidColorBrush> をアニメーション化することはできませんでした。

<a name="applyanimationswithastoryboard"></a>

## <a name="how-to-apply-animations-with-a-storyboard"></a>ストーリーボードを使用してアニメーションを適用する方法

アニメーションを整理して適用するために <xref:System.Windows.Media.Animation.Storyboard> を使用するには、<xref:System.Windows.Media.Animation.Storyboard>の子タイムラインとしてアニメーションを追加します。 <xref:System.Windows.Media.Animation.Storyboard> クラスは、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A?displayProperty=nameWithType> および <xref:System.Windows.Media.Animation.Storyboard.TargetProperty?displayProperty=nameWithType> 添付プロパティを提供します。 これらのプロパティをアニメーションで設定して、アニメーションのターゲット オブジェクトとターゲット プロパティを指定します。

アニメーションをターゲットに適用するには、トリガーアクションまたはメソッドを使用して <xref:System.Windows.Media.Animation.Storyboard> を開始します。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、<xref:System.Windows.EventTrigger>、<xref:System.Windows.Trigger>、または <xref:System.Windows.DataTrigger>で <xref:System.Windows.Media.Animation.BeginStoryboard> オブジェクトを使用します。 コードでは、<xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用することもできます。

次の表は、各 <xref:System.Windows.Media.Animation.Storyboard> の開始方法がサポートされている場所を示しています。インスタンスごと、スタイル、コントロールテンプレート、およびデータテンプレートです。 "インスタンス単位" とは、スタイル、コントロール テンプレート、またはデータ テンプレート内のインスタンスではなく、オブジェクトのインスタンスに直接アニメーションまたはストーリーボードを適用する方法のことです。

|ストーリーボードが開始される場所|インスタンス単位|スタイル|コントロール テンプレート|データ テンプレート|使用例|
|--------------------------------|-------------------|-----------|----------------------|-------------------|-------------|
|<xref:System.Windows.Media.Animation.BeginStoryboard> と <xref:System.Windows.EventTrigger>|○|○|○|○|[ストーリーボードを使ってプロパティをアニメーション化する](how-to-animate-a-property-by-using-a-storyboard.md)|
|<xref:System.Windows.Media.Animation.BeginStoryboard> とプロパティ <xref:System.Windows.Trigger>|いいえ|○|○|○|[プロパティ値が変化したときにアニメーションをトリガーする](how-to-trigger-an-animation-when-a-property-value-changes.md)|
|<xref:System.Windows.Media.Animation.BeginStoryboard> と <xref:System.Windows.DataTrigger>|いいえ|○|○|○|[方法: データが変化したときにアニメーションをトリガーする](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa970679(v=vs.90))|
|<xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッド|○|いいえ|いいえ|いいえ|[ストーリーボードを使ってプロパティをアニメーション化する](how-to-animate-a-property-by-using-a-storyboard.md)|

次の例では、<xref:System.Windows.Media.Animation.Storyboard> を使用して、<xref:System.Windows.Shapes.Rectangle> 要素の <xref:System.Windows.FrameworkElement.Width%2A> と、その <xref:System.Windows.Media.SolidColorBrush> を描画するために使用される <xref:System.Windows.Shapes.Rectangle>の <xref:System.Windows.Media.SolidColorBrush.Color%2A> をアニメーション化します。

[!code-xaml[storyboards_ovw_snip_XAML#1](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/StoryboardsExample.xaml#1)]

[!code-csharp[storyboards_ovw_snip#100](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/StoryboardsExample.cs#100)]

以下のセクションでは、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> と <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> 添付プロパティについて詳しく説明します。

<a name="targetingelementsandfreezables"></a>

## <a name="targeting-framework-elements-framework-content-elements-and-freezables"></a>フレームワーク要素、フレームワーク コンテンツ要素、およびフリーズ可能オブジェクトの対象化

アニメーションでは、そのターゲットを見つけるために、ターゲットの名前とアニメーション化するプロパティを認識する必要があることについては前のセクションで説明しました。 アニメーション化するプロパティの指定は簡単です。アニメーション化するプロパティの名前を <xref:System.Windows.Media.Animation.Storyboard.TargetProperty?displayProperty=nameWithType> に設定するだけです。  アニメーション化するプロパティを持つオブジェクトの名前を指定するには、アニメーションの [<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A?displayProperty=nameWithType>] プロパティを設定します。

<xref:System.Windows.Setter.TargetName%2A> プロパティを機能させるには、対象のオブジェクトに名前を付ける必要があります。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 内の <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement> への名前の割り当ては、<xref:System.Windows.Freezable> オブジェクトへの名前の割り当てとは異なります。

フレームワーク要素は、<xref:System.Windows.FrameworkElement> クラスを継承するクラスです。 フレームワーク要素の例としては、<xref:System.Windows.Window>、<xref:System.Windows.Controls.DockPanel>、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Shapes.Rectangle>があります。 基本的に、ウィンドウ、パネル、およびコントロールはすべて要素です。 フレームワークコンテンツ要素は、<xref:System.Windows.FrameworkContentElement> クラスを継承するクラスです。 フレームワークコンテンツ要素の例としては、<xref:System.Windows.Documents.FlowDocument> や <xref:System.Windows.Documents.Paragraph>などがあります。 型がフレームワーク要素またはフレームワーク コンテンツ要素であるかどうかが明確でない場合は、Name プロパティが含まれているかどうかを確認します。 含まれている場合は、フレームワーク要素またはフレームワーク コンテンツ要素と考えられます。 確かめるには、その型のページの「継承階層」を参照してください。

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]でフレームワーク要素またはフレームワークコンテンツ要素のターゲット設定を有効にするには、<xref:System.Windows.FrameworkElement.Name%2A> プロパティを設定します。 コードでは、<xref:System.Windows.NameScope.RegisterName%2A> メソッドを使用して、<xref:System.Windows.NameScope>を作成した要素に要素名を登録する必要もあります。

前の例で取得した次の例では、<xref:System.Windows.Shapes.Rectangle>`MyRectangle` 名前を <xref:System.Windows.FrameworkElement>の型に割り当てています。

[!code-xaml[storyboards_ovw_snip_XAML#2](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/StoryboardsExample.xaml#2)]

[!code-csharp[storyboards_ovw_snip#102](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/StoryboardsExample.cs#102)]

名前が割り当てられると、その要素のプロパティはアニメーション化できます。

[!code-xaml[storyboards_ovw_snip_XAML#5](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/StoryboardsExample.xaml#5)]

[!code-csharp[storyboards_ovw_snip#105](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/StoryboardsExample.cs#105)]

<xref:System.Windows.Freezable> 型は、<xref:System.Windows.Freezable> クラスを継承するクラスです。 <xref:System.Windows.Freezable> の例としては、<xref:System.Windows.Media.SolidColorBrush>、<xref:System.Windows.Media.RotateTransform>、<xref:System.Windows.Media.GradientStop>などがあります。

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]のアニメーションによって <xref:System.Windows.Freezable> のターゲット設定を有効にするには、 [X:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)を使用して名前を割り当てます。 コードでは、<xref:System.Windows.NameScope.RegisterName%2A> メソッドを使用して、<xref:System.Windows.NameScope>を作成した要素に名前を登録します。

次の例では、<xref:System.Windows.Freezable> オブジェクトに名前を割り当てます。

[!code-xaml[storyboards_ovw_snip_XAML#3](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/StoryboardsExample.xaml#3)]

[!code-csharp[storyboards_ovw_snip#103](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/StoryboardsExample.cs#103)]

これで、オブジェクトをアニメーションのターゲットにすることができます。

[!code-xaml[storyboards_ovw_snip_XAML#7](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/StoryboardsExample.xaml#7)]

[!code-csharp[storyboards_ovw_snip#107](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/StoryboardsExample.cs#107)]

<xref:System.Windows.Media.Animation.Storyboard> オブジェクトは、名前スコープを使用して <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> プロパティを解決します。 WPF 名前スコープの詳細については、「[WPF XAML 名前スコープ](../advanced/wpf-xaml-namescopes.md)」を参照してください。 <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> プロパティを省略した場合、アニメーションは、定義されている要素、またはスタイルの場合はスタイルが設定された要素を対象とします。

場合によっては、<xref:System.Windows.Freezable> オブジェクトに名前を割り当てることはできません。 たとえば、<xref:System.Windows.Freezable> がリソースとして宣言されている場合、またはスタイルのプロパティ値を設定するために使用されている場合、名前を指定することはできません。 名前が割り当てられていないため、それを直接ターゲットにすることができません。ただし、間接的にターゲットにすることはできます。 以下のセクションでは、間接的な対象化の使用方法について説明します。

<a name="pathsyntaxforchangeable"></a>

## <a name="indirect-targeting"></a>間接的な対象化

<xref:System.Windows.Freezable> がリソースとして宣言されている場合や、スタイルのプロパティ値を設定するために使用される場合など、<xref:System.Windows.Freezable> がアニメーションによって直接対象になることはありません。 このような場合でも、直接ターゲットにすることはできませんが、<xref:System.Windows.Freezable> オブジェクトをアニメーション化することはできます。 <xref:System.Windows.Freezable>の名前を使用して <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> プロパティを設定する代わりに、<xref:System.Windows.Freezable> "が属している要素の名前を指定します。 たとえば、四角形要素の <xref:System.Windows.Shapes.Shape.Fill%2A> を設定するために使用される <xref:System.Windows.Media.SolidColorBrush> は、その四角形に属します。 このブラシをアニメーション化するには、アニメーションの <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> を、フレームワーク要素またはフレームワークコンテンツ要素のプロパティから開始するプロパティのチェーンに設定します。 <xref:System.Windows.Freezable> は、アニメーション化する <xref:System.Windows.Freezable> プロパティを設定して終了するために使用されます。

[!code-xaml[storyboards_ovw_snip_XAML#33](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/IndirectTargetingExample.xaml#33)]

[!code-csharp[storyboards_ovw_snip#134](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#134)]

<xref:System.Windows.Freezable> が固定されている場合は、複製が行われ、その複製がアニメーション化されます。 この場合、元のオブジェクトは実際にはアニメーション化されていないため、元のオブジェクトの <xref:System.Windows.Media.Animation.Animatable.HasAnimatedProperties%2A> プロパティは引き続き `false`を返します。 複製の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。

また、間接的なプロパティの対象化を使用するとき、存在しないオブジェクトを対象化する可能性があることに注意してください。 たとえば、特定のボタンの <xref:System.Windows.Controls.Control.Background%2A> が <xref:System.Windows.Media.SolidColorBrush> で設定され、その色をアニメーション化しようとしたとします。実際には、ボタンの背景を設定するために <xref:System.Windows.Media.LinearGradientBrush> が使用されていました。 このような場合、例外はスローされません。<xref:System.Windows.Media.LinearGradientBrush> が <xref:System.Windows.Media.SolidColorBrush.Color%2A> プロパティの変更に反応しないため、アニメーションは表示されません。

以下のセクションでは、間接的なプロパティの対象化の構文について詳しく説明します。

<a name="xamlsyntaxchangeableproperty"></a>

### <a name="indirectly-targeting-a-property-of-a-freezable-in-xaml"></a>XAML でフリーズ可能オブジェクトのプロパティを間接的に対象化する

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で freezable のプロパティを対象にするには、次の構文を使用します。

| |
|-|
|*Elementpropertyname* `.` *FreezablePropertyName*|

Where

- *Elementpropertyname*は、<xref:System.Windows.Freezable> を設定するために使用される <xref:System.Windows.FrameworkElement> のプロパティです。

- *FreezablePropertyName*は、アニメーション化する <xref:System.Windows.Freezable> のプロパティです。

次のコードは、四角形要素の <xref:System.Windows.Shapes.Shape.Fill%2A> の設定に使用される <xref:System.Windows.Media.SolidColorBrush> の <xref:System.Windows.Media.SolidColorBrush.Color%2A> をアニメーション化する方法を示しています。

[!code-xaml[storyboards_ovw_snip_XAML#32](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/IndirectTargetingExample.xaml#32)]

コレクションまたは配列に含まれるフリーズ可能オブジェクトをターゲットにしなければならない場合があります。

コレクションに含まれるフリーズ可能オブジェクトをターゲットにするには、次のパス構文を使用します。

| |
|-|
|*Elementpropertyname* `.Children[` *Collectionindex* `].` *FreezablePropertyName*|

ここで、 *Collectionindex*は、配列またはコレクション内のオブジェクトのインデックスです。

たとえば、四角形に <xref:System.Windows.UIElement.RenderTransform%2A> プロパティに適用されている <xref:System.Windows.Media.TransformGroup> リソースがあり、そこに含まれている変換の1つをアニメーション化するとします。

[!code-xaml[storyboards_ovw_snip_XAML#34](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/IndirectTargetingExample.xaml#34)]

次のコードは、前の例で示した <xref:System.Windows.Media.RotateTransform> の <xref:System.Windows.Media.RotateTransform.Angle%2A> プロパティをアニメーション化する方法を示しています。

[!code-xaml[storyboards_ovw_snip_XAML#35](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip_XAML/CS/IndirectTargetingExample.xaml#35)]

<a name="targetingpropertyofchangeableincode"></a>

### <a name="indirectly-targeting-a-property-of-a-freezable-in-code"></a>コードでフリーズ可能オブジェクトのプロパティを間接的に対象化する

コードでは、<xref:System.Windows.PropertyPath> オブジェクトを作成します。 <xref:System.Windows.PropertyPath>を作成するときに、<xref:System.Windows.PropertyPath.Path%2A> と <xref:System.Windows.PropertyPath.PathParameters%2A>を指定します。

<xref:System.Windows.PropertyPath.PathParameters%2A>を作成するには、依存関係プロパティの識別子フィールドの一覧を含む <xref:System.Windows.DependencyProperty> 型の配列を作成します。 最初の識別子フィールドは、<xref:System.Windows.FrameworkElement> のプロパティ、または <xref:System.Windows.Freezable> を設定するために使用される <xref:System.Windows.FrameworkContentElement> のプロパティです。 次の識別子フィールドは、対象となる <xref:System.Windows.Freezable> のプロパティを表します。 これは、<xref:System.Windows.Freezable> を <xref:System.Windows.FrameworkElement> オブジェクトに接続するプロパティのチェーンと考えることができます。

四角形要素の <xref:System.Windows.Shapes.Shape.Fill%2A> を設定するために使用される <xref:System.Windows.Media.SolidColorBrush> の <xref:System.Windows.Media.SolidColorBrush.Color%2A> を対象とする依存関係プロパティチェーンの例を次に示します。

[!code-csharp[storyboards_ovw_snip#135](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#135)]

また、<xref:System.Windows.PropertyPath.Path%2A>を指定する必要もあります。 <xref:System.Windows.PropertyPath.Path%2A> は、<xref:System.Windows.PropertyPath.PathParameters%2A>の解釈方法を <xref:System.Windows.PropertyPath.Path%2A> に指示する <xref:System.String> です。 次の構文が使用されます。

| |
|-|
|`(` *Ownerpropertyarrayindex* `).(` *FreezablePropertyArrayIndex* `)`|

Where

- *Ownerpropertyarrayindex*は、<xref:System.Windows.Freezable> が設定に使用する <xref:System.Windows.FrameworkElement> オブジェクトのプロパティの識別子を含む <xref:System.Windows.DependencyProperty> 配列のインデックスです。

- *FreezablePropertyArrayIndex*は、対象のプロパティの識別子を含む <xref:System.Windows.DependencyProperty> 配列のインデックスです。

次の例は、前の例で定義した <xref:System.Windows.PropertyPath.PathParameters%2A> に付随する <xref:System.Windows.PropertyPath.Path%2A> を示しています。

[!code-csharp[storyboards_ovw_snip#PropertyChainAndPath](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#propertychainandpath)]

次の例では、前の例のコードを組み合わせて、四角形要素の <xref:System.Windows.Shapes.Shape.Fill%2A> を設定するために使用される <xref:System.Windows.Media.SolidColorBrush> の <xref:System.Windows.Media.SolidColorBrush.Color%2A> をアニメーション化します。

[!code-csharp[storyboards_ovw_snip#137](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#137)]

コレクションまたは配列に含まれるフリーズ可能オブジェクトをターゲットにしなければならない場合があります。 たとえば、四角形に <xref:System.Windows.UIElement.RenderTransform%2A> プロパティに適用されている <xref:System.Windows.Media.TransformGroup> リソースがあり、そこに含まれている変換の1つをアニメーション化するとします。

[!code-xaml[storyboards_ovw_snip#142](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml#142)]

コレクションに含まれる <xref:System.Windows.Freezable> を対象にするには、次のパス構文を使用します。

| |
|-|
|`(` *Ownerpropertyarrayindex* `).(` *CollectionChildrenPropertyArrayIndex* `)` `[` *collectionindex* `].(` *FreezablePropertyArrayIndex* `)`|

ここで、 *Collectionindex*は、配列またはコレクション内のオブジェクトのインデックスです。

<xref:System.Windows.Media.TransformGroup>の2番目の変換である <xref:System.Windows.Media.RotateTransform>の <xref:System.Windows.Media.RotateTransform.Angle%2A> プロパティを対象にするには、次の <xref:System.Windows.PropertyPath.Path%2A> と <xref:System.Windows.PropertyPath.PathParameters%2A>を使用します。

[!code-csharp[storyboards_ovw_snip#139](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#139)]

次の例は、<xref:System.Windows.Media.TransformGroup>内に含まれる <xref:System.Windows.Media.RotateTransform> の <xref:System.Windows.Media.RotateTransform.Angle%2A> をアニメーション化するための完全なコードを示しています。

[!code-csharp[storyboards_ovw_snip#138](~/samples/snippets/csharp/VS_Snippets_Wpf/storyboards_ovw_snip/CSharp/IndirectTargetingExample.xaml.cs#138)]

### <a name="indirectly-targeting-with-a-freezable-as-the-starting-point"></a>開始点として Freezable を使用して間接的に対象化する

前のセクションでは、<xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement> から開始し、<xref:System.Windows.Freezable> サブプロパティへのプロパティチェーンを作成することによって、<xref:System.Windows.Freezable> を間接的に対象にする方法について説明します。 また、開始点として <xref:System.Windows.Freezable> を使用し、その <xref:System.Windows.Freezable> サブプロパティの1つを間接的にターゲットにすることもできます。 間接的なターゲット設定の開始点として <xref:System.Windows.Freezable> を使用する場合に適用される追加の制限が1つあります。開始 <xref:System.Windows.Freezable> と、その間のすべての <xref:System.Windows.Freezable> と間接的に対象となるサブプロパティを固定することはできません。

<a name="controllable_storyboards"></a>

## <a name="interactively-controlling-a-storyboard-in-xaml"></a>XAML での対話形式でのストーリーボードの制御

[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]でストーリーボードを開始するには、<xref:System.Windows.Media.Animation.BeginStoryboard> のトリガーアクションを使用します。 <xref:System.Windows.Media.Animation.BeginStoryboard> はアニメーションをアニメーション化してアニメーション化し、ストーリーボードを開始します。 (このプロセスの詳細については、「[アニメーションとタイミングシステムの概要](animation-and-timing-system-overview.md)」を参照してください)。<xref:System.Windows.Media.Animation.BeginStoryboard.Name%2A> プロパティを指定することによって名前の <xref:System.Windows.Media.Animation.BeginStoryboard> を指定すると、制御可能なストーリーボードになります。 ストーリーボードが開始すると対話的に制御できます。 制御可能なストーリーボード アクションの一覧を次に示します。これらをイベント トリガーで使用して、ストーリーボードを制御します。

- <xref:System.Windows.Media.Animation.PauseStoryboard>: ストーリーボードを一時停止します。

- <xref:System.Windows.Media.Animation.ResumeStoryboard>: 一時停止したストーリーボードを再開します。

- <xref:System.Windows.Media.Animation.SetStoryboardSpeedRatio>: ストーリーボードの速度を変更します。

- <xref:System.Windows.Media.Animation.SkipStoryboardToFill>: ストーリーボードがある場合は、そのストーリーボードをその塗りつぶし期間の末尾に進めます。

- <xref:System.Windows.Media.Animation.StopStoryboard>: ストーリーボードを停止します。

- <xref:System.Windows.Media.Animation.RemoveStoryboard>: ストーリーボードを削除します。

次の例では、制御可能なストーリーボード アクションを使用して、ストーリーボードを対話的に制御しています。

[!code-xaml[animation_ovws_snip#ControllableStoryboardExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snip/CS/ControllableStoryboardExample.xaml#controllablestoryboardexamplewholepage)]

<a name="controllable_storyboards_procedural"></a>

## <a name="interactively-controlling-a-storyboard-by-using-code"></a>コードを使用した対話形式でのストーリーボードの制御

トリガー アクションを使用してアニメーション化する方法については、前の例で説明しました。 コードでは、<xref:System.Windows.Media.Animation.Storyboard> クラスの対話型メソッドを使用してストーリーボードを制御することもできます。 <xref:System.Windows.Media.Animation.Storyboard> をコードで対話型にするには、ストーリーボードの <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドの適切なオーバーロードを使用し、`true` を指定して、制御可能にする必要があります。 詳細については、「<xref:System.Windows.Media.Animation.Storyboard.Begin%28System.Windows.FrameworkElement%2CSystem.Boolean%29>」ページを参照してください。

次の一覧は、開始後に <xref:System.Windows.Media.Animation.Storyboard> を操作するために使用できるメソッドを示しています。

- <xref:System.Windows.Media.Animation.Storyboard.Pause%2A>

- <xref:System.Windows.Media.Animation.Storyboard.Resume%2A>

- <xref:System.Windows.Media.Animation.Storyboard.Seek%2A>

- <xref:System.Windows.Media.Animation.Storyboard.SkipToFill%2A>

- <xref:System.Windows.Media.Animation.Storyboard.Stop%2A>

- <xref:System.Windows.Media.Animation.Storyboard.Remove%2A>

これらのメソッドを使用する利点は、<xref:System.Windows.Trigger> オブジェクトや <xref:System.Windows.TriggerAction> オブジェクトを作成する必要がないことです。操作する必要があるのは、制御可能な <xref:System.Windows.Media.Animation.Storyboard> への参照だけです。

> [!NOTE]
> <xref:System.Windows.Media.Animation.Clock>に対して実行されるすべての対話型アクションは、次のレンダリングの直後に発生するタイミングエンジンの次の目盛りで <xref:System.Windows.Media.Animation.Storyboard> も発生します。 たとえば、<xref:System.Windows.Media.Animation.Storyboard.Seek%2A> メソッドを使用してアニメーション内の別の点に移動した場合、プロパティ値はすぐには変更されず、タイミングエンジンの次の目盛りで値が変化します。

次の例は、<xref:System.Windows.Media.Animation.Storyboard> クラスの対話型メソッドを使用してアニメーションを適用および制御する方法を示しています。

[!code-csharp[animation_ovws_procedural_snip#ControllableStoryboardExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_procedural_snip/CSharp/ControllableStoryboardExample.cs#controllablestoryboardexamplewholepage)]
[!code-vb[animation_ovws_procedural_snip#ControllableStoryboardExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws_procedural_snip/visualbasic/controllablestoryboardexample.vb#controllablestoryboardexamplewholepage)]

<a name="usingstoryboardsinstyles"></a>

## <a name="animate-in-a-style"></a>スタイル内でアニメーション化を行う

<xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用して、<xref:System.Windows.Style>でアニメーションを定義できます。 <xref:System.Windows.Style> 内の <xref:System.Windows.Media.Animation.Storyboard> を使用したアニメーション化は、次の3つの例外を除き、<xref:System.Windows.Media.Animation.Storyboard> を別の場所で使用することと似ています。

- <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A>を指定することはできません。<xref:System.Windows.Media.Animation.Storyboard> は常に、<xref:System.Windows.Style> が適用される要素を対象とします。 <xref:System.Windows.Freezable> オブジェクトを対象にするには、間接的なターゲット設定を使用する必要があります。 間接的なターゲット設定の詳細については、「[間接ターゲット](#pathsyntaxforchangeable)」セクションを参照してください。

- <xref:System.Windows.EventTrigger> または <xref:System.Windows.Trigger>に <xref:System.Windows.EventTrigger.SourceName%2A> を指定することはできません。

- 動的リソース参照またはデータバインディング式を使用して、<xref:System.Windows.Media.Animation.Storyboard> またはアニメーションのプロパティ値を設定することはできません。 これは、<xref:System.Windows.Style> 内のすべてがスレッドセーフである必要があり、タイミングシステムは <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを <xref:System.Windows.Freezable.Freeze%2A>してスレッドセーフにする必要があるためです。 <xref:System.Windows.Media.Animation.Storyboard> は、その子タイムラインに動的リソース参照またはデータバインディング式が含まれている場合は、固定できません。 フリーズとその他の <xref:System.Windows.Freezable> 機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。

- [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、<xref:System.Windows.Media.Animation.Storyboard> またはアニメーションイベントのイベントハンドラーを宣言することはできません。

スタイルでストーリーボードを定義する方法を示す例については、「[スタイルのアニメーション化](how-to-animate-in-a-style.md)の例」を参照してください。

<a name="defineAStoryboardInAControlTemplateSection"></a>

## <a name="animate-in-a-controltemplate"></a>ControlTemplate 内でアニメーション化を行う

<xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用して、<xref:System.Windows.Controls.ControlTemplate>でアニメーションを定義できます。 <xref:System.Windows.Controls.ControlTemplate> 内の <xref:System.Windows.Media.Animation.Storyboard> を使用したアニメーション化は、次の2つの例外を除き、<xref:System.Windows.Media.Animation.Storyboard> を別の場所で使用することと似ています。

- <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> は、<xref:System.Windows.Controls.ControlTemplate>の子オブジェクトのみを参照できます。 <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> が指定されていない場合、アニメーションは、<xref:System.Windows.Controls.ControlTemplate> が適用される要素を対象とします。

- <xref:System.Windows.EventTrigger> または <xref:System.Windows.Trigger> の <xref:System.Windows.EventTrigger.SourceName%2A> は、<xref:System.Windows.Controls.ControlTemplate>の子オブジェクトのみを参照できます。

- 動的リソース参照またはデータバインディング式を使用して、<xref:System.Windows.Media.Animation.Storyboard> またはアニメーションのプロパティ値を設定することはできません。 これは、<xref:System.Windows.Controls.ControlTemplate> 内のすべてがスレッドセーフである必要があり、タイミングシステムは <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを <xref:System.Windows.Freezable.Freeze%2A>してスレッドセーフにする必要があるためです。 <xref:System.Windows.Media.Animation.Storyboard> は、その子タイムラインに動的リソース参照またはデータバインディング式が含まれている場合は、固定できません。 フリーズとその他の <xref:System.Windows.Freezable> 機能の詳細については、「 [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」を参照してください。

- [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、<xref:System.Windows.Media.Animation.Storyboard> またはアニメーションイベントのイベントハンドラーを宣言することはできません。

<xref:System.Windows.Controls.ControlTemplate>でストーリーボードを定義する方法を示す例については、「 [ControlTemplate のアニメーション化](how-to-animate-in-a-controltemplate.md)の例」を参照してください。

<a name="animateWhenAPropertyValueChanges"></a>

## <a name="animate-when-a-property-value-changes"></a>プロパティ値が変化したときにアニメーション化を行う

スタイル内およびコントロール テンプレート内では、トリガー オブジェクトを使用して、プロパティが変化したときにストーリーボードを開始します。 例については、「[プロパティ値が変更されたときにアニメーションをトリガー](how-to-trigger-an-animation-when-a-property-value-changes.md)する」および「 [ControlTemplate でアニメーション化](how-to-animate-in-a-controltemplate.md)する」を参照してください。

プロパティ <xref:System.Windows.Trigger> オブジェクトによって適用されるアニメーションは、<xref:System.Windows.Media.Animation.Storyboard> メソッドを使用して開始されたアニメーションまたはアニメーション <xref:System.Windows.EventTrigger> よりも複雑な方法で動作します。  他の <xref:System.Windows.Trigger> オブジェクトによって定義されたアニメーションを "ハンドオフ" し、<xref:System.Windows.EventTrigger> とメソッドによってトリガーされるアニメーションで構成します。

## <a name="see-also"></a>関連項目

- [アニメーションの概要](animation-overview.md)
- [プロパティ アニメーションの手法の概要](property-animation-techniques-overview.md)
- [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)
