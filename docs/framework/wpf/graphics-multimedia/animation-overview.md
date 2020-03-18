---
title: アニメーションの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Storyboards [WPF], animations
- animations [WPF], overview
ms.assetid: bd9ce563-725d-4385-87c9-d7ee38cf79ea
ms.openlocfilehash: 00f01b63cdf9397fe25f28fff08767dfc3a83e69
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453118"
---
# <a name="animation-overview"></a>アニメーションの概要

<a name="introduction"></a>
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、魅力的なユーザーインターフェイスや魅力的なドキュメントを作成するための強力なグラフィックスおよびレイアウト機能が用意されています。 アニメーションを使うと、魅力的なユーザー インターフェイスをさらに豪華で使いやすいものにすることができます。 背景色をアニメーション化するか、アニメーション化された <xref:System.Windows.Media.Transform>を適用するだけで、大幅な画面切り替え効果を作成したり、便利な視覚的な手掛かりを提供したりすることができます。

この概要では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションとタイミングシステムの概要について説明します。 ストーリーボードを使用した [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクトのアニメーションに焦点を当てています。

<a name="introducinganimations"></a>

## <a name="introducing-animations"></a>アニメーションの概要

アニメーションは、それぞれが直前のイメージと少しずつ異なる一連のイメージをすばやく繰り返すことで生み出される錯覚です。 脳は、イメージのグループを 1 つの変化するシーンとして認識します。 映画では、この錯覚は 1 秒あたりに多数の写真またはフレームを記録するカメラを使って生み出されます。 フレームを映写機で再生すると、見る人には動く画像として映ります。

コンピューター上のアニメーションも同様です。 たとえば、四角形の描画をビューからフェードアウトするプログラムは次のように動作する可能性があります。

- プログラムがタイマーを作成します。

- 設定された間隔でプログラムがタイマーをチェックして経過時間を確認します。

- プログラムは、タイマーをチェックするたびに、経過時間に基づいて四角形の現在の不透明度値を計算します。

- プログラムは、新しい値で四角形を更新し、再描画します。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]前に、Microsoft Windows の開発者は、独自のタイミングシステムを作成して管理するか、特別なカスタムライブラリを使用する必要がありました。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、マネージコードと [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] を通じて公開され、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] framework に緊密に統合された効率的なタイミングシステムが含まれています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションを使うと、コントロールやその他のグラフィカル オブジェクトを簡単にアニメーション化できます。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、タイミング システムの管理と画面の効率的な再描画をすべてバックグラウンドで処理します。 効果を実現するしくみではなく、作り出す効果に重点を置くことのできるタイミング クラスが提供されます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、クラスが継承できるアニメーションの基底クラスを公開しているため、独自のアニメーションを簡単に作成して、カスタマイズされたアニメーションを生成することもできます。 これらのカスタム アニメーションは、標準のアニメーション クラスのパフォーマンス上のメリットの多くを得ます。

<a name="thewpftimingsystem"></a>

## <a name="wpf-property-animation-system"></a>WPF プロパティ アニメーション システム

タイミングシステムに関するいくつかの重要な概念を理解している場合は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションを使用する方が簡単です。 最も重要な点は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、個々のプロパティにアニメーションを適用してオブジェクトをアニメーション化することです。 たとえば、フレームワーク要素を拡大するには、その <xref:System.Windows.FrameworkElement.Width%2A> をアニメーション化し、プロパティを <xref:System.Windows.FrameworkElement.Height%2A> します。 オブジェクトをビューからフェードさせるには、その <xref:System.Windows.UIElement.Opacity%2A> プロパティをアニメーション化します。

プロパティにアニメーション機能を持たせるには、次の 3 つの要件を満たす必要があります。

- 依存関係プロパティである必要があります。

- <xref:System.Windows.DependencyObject> から継承し、<xref:System.Windows.Media.Animation.IAnimatable> インターフェイスを実装するクラスに属している必要があります。

- 互換性のあるアニメーションの種類が使用可能である必要があります。 ([!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で提供されていない場合は、独自のものを作成できます。 「[カスタムアニメーションの概要](custom-animations-overview.md)」を参照してください)。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、<xref:System.Windows.Media.Animation.IAnimatable> プロパティを持つ多数のオブジェクトが含まれています。 <xref:System.Windows.Controls.Button> や <xref:System.Windows.Controls.TabControl>などのコントロール、および <xref:System.Windows.Controls.Panel> および <xref:System.Windows.Shapes.Shape> オブジェクトは <xref:System.Windows.DependencyObject>から継承されます。 これらのプロパティの大部分は依存関係プロパティです。

アニメーションは、スタイルやコントロール テンプレートなど、ほぼ任意の場所で使用できます。 アニメーションはビジュアルである必要はありません。このセクションで説明している条件を満たしていれば、ユーザー インターフェイスの一部ではないオブジェクトをアニメーション化できます。

<a name="storyboardwalkthrough"></a>

## <a name="example-make-an-element-fade-in-and-out-of-view"></a>例: ビューで要素のフェードインとフェードアウトを行う

この例では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションを使用して、依存関係プロパティの値をアニメーション化する方法を示します。 <xref:System.Windows.Media.Animation.DoubleAnimation>を使用します。これは <xref:System.Double> 値を生成するアニメーションの一種であり、<xref:System.Windows.Shapes.Rectangle>の <xref:System.Windows.UIElement.Opacity%2A> プロパティをアニメーション化します。 その結果、<xref:System.Windows.Shapes.Rectangle> フェードインして表示されなくなります。

この例の最初の部分では、<xref:System.Windows.Shapes.Rectangle> 要素を作成します。 次の手順では、アニメーションを作成し、四角形の <xref:System.Windows.UIElement.Opacity%2A> プロパティに適用する方法を示します。

次の例は、XAML で <xref:System.Windows.Controls.StackPanel> に <xref:System.Windows.Shapes.Rectangle> 要素を作成する方法を示しています。

[!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_1](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_1)]

コードで <xref:System.Windows.Controls.StackPanel> に <xref:System.Windows.Shapes.Rectangle> 要素を作成する方法を次に示します。

[!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_1](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Class1.cs#rectangleopacityfadeexamplecode_1)]
[!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/Class1.vb#rectangleopacityfadeexamplecode_1)]

<a name="opacity_animation_step1"></a>

### <a name="part-1-create-a-doubleanimation"></a>パート 1: DoubleAnimation を作成する

要素のフェードインとビューアウトを行う方法の1つは、その <xref:System.Windows.UIElement.Opacity%2A> プロパティをアニメーション化することです。 <xref:System.Windows.UIElement.Opacity%2A> プロパティは <xref:System.Double>型であるため、double 値を生成するアニメーションが必要になります。 <xref:System.Windows.Media.Animation.DoubleAnimation> は、このようなアニメーションの1つです。 <xref:System.Windows.Media.Animation.DoubleAnimation> は、2つの double 値の間の遷移を作成します。 開始値を指定するには、その <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを設定します。 終了値を指定するには、その <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを設定します。

1. 不透明度の値 `1.0` を指定すると、オブジェクトは完全に不透明になり、不透明度の値 `0.0` によって完全に非表示になります。 アニメーションを `1.0` からに遷移させるには `0.0` <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを `1.0` に設定し、その <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを `0.0`に設定します。 XAML で <xref:System.Windows.Media.Animation.DoubleAnimation> を作成する方法を次に示します。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_2](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_2)]

    コードで <xref:System.Windows.Media.Animation.DoubleAnimation> を作成する方法を次に示します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_2](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Class1.cs#rectangleopacityfadeexamplecode_2)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/Class1.vb#rectangleopacityfadeexamplecode_2)]

2. 次に、<xref:System.Windows.Media.Animation.Timeline.Duration%2A>を指定する必要があります。 アニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は、開始値から対象の値への移動にかかる時間を指定します。 XAML で <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を5秒に設定する方法を次に示します。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_3](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_3)]

    次に、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> をコードで5秒に設定する方法を示します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_3](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Class1.cs#rectangleopacityfadeexamplecode_3)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/Class1.vb#rectangleopacityfadeexamplecode_3)]

3. 前のコードでは、`1.0` から `0.0`に遷移するアニメーションを示しています。これにより、target 要素は完全に不透明なものから完全に非表示になります。 消滅した後に要素をビューにフェードバックするには、アニメーションの <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティを `true`に設定します。 アニメーションを無期限に繰り返すには、その <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A>に設定します。 XAML で <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> と <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> のプロパティを設定する方法を次に示します。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_4](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_4)]

    <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> と <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> のプロパティをコードで設定する方法を次に示します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_4](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Class1.cs#rectangleopacityfadeexamplecode_4)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/Class1.vb#rectangleopacityfadeexamplecode_4)]

<a name="opacity_animation_step2"></a>

### <a name="part-2-create-a-storyboard"></a>パート 2: ストーリーボードを作成する

オブジェクトにアニメーションを適用するには、<xref:System.Windows.Media.Animation.Storyboard> を作成し、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> および <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> 添付プロパティを使用して、アニメーション化するオブジェクトとプロパティを指定します。

1. <xref:System.Windows.Media.Animation.Storyboard> を作成し、アニメーションをその子として追加します。 XAML で <xref:System.Windows.Media.Animation.Storyboard> を作成する方法を次に示します。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_5](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_5)]

    コードで <xref:System.Windows.Media.Animation.Storyboard> を作成するには、クラスレベルで <xref:System.Windows.Media.Animation.Storyboard> 変数を宣言します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_100](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_100)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_100](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_100)]

    次に、<xref:System.Windows.Media.Animation.Storyboard> を初期化し、その子としてアニメーションを追加します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_101](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_101)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_101)]

2. <xref:System.Windows.Media.Animation.Storyboard> は、アニメーションを適用する場所を認識している必要があります。 アニメーション化するオブジェクトを指定するには、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A?displayProperty=nameWithType> 添付プロパティを使用します。 次の例は、XAML で `MyRectangle` する <xref:System.Windows.Media.Animation.DoubleAnimation> のターゲット名を設定する方法を示しています。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_6](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_6)]

    コードで `MyRectangle` する <xref:System.Windows.Media.Animation.DoubleAnimation> のターゲット名を設定する方法を次に示します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_102](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_102)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_102](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_102)]

3. <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> 添付プロパティを使用して、アニメーション化するプロパティを指定します。 次の例は、XAML で <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.UIElement.Opacity%2A> プロパティをターゲットとするようにアニメーションを構成する方法を示しています。

    [!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml_7](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/Window1.xaml#rectangleopacityfadeexamplexaml_7)]

    次に、コードで <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.UIElement.Opacity%2A> プロパティを対象とするようにアニメーションを構成する方法を示します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_103](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_103)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_103](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_103)]

<xref:System.Windows.Media.Animation.Storyboard.TargetProperty> 構文の詳細とその他の例については、「[ストーリーボードの概要](storyboards-overview.md)」を参照してください。

<a name="opacity_animation_step3"></a>

### <a name="part-3-xaml-associate-the-storyboard-with-a-trigger"></a>パート 3 (XAML): ストーリーボードをトリガーに関連付ける

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で <xref:System.Windows.Media.Animation.Storyboard> を適用および開始する最も簡単な方法は、イベントトリガーを使用することです。 このセクションでは、XAML で <xref:System.Windows.Media.Animation.Storyboard> をトリガーに関連付ける方法について説明します。

1. <xref:System.Windows.Media.Animation.BeginStoryboard> オブジェクトを作成し、ストーリーボードを関連付けます。 <xref:System.Windows.Media.Animation.BeginStoryboard> は、<xref:System.Windows.Media.Animation.Storyboard>を適用して開始する <xref:System.Windows.TriggerAction> の一種です。

    [!code-xaml[animation_ovws_snippet#RectangleOpacityFadeExampleInline_3](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/RectangleOpacityFadeExample.xaml#rectangleopacityfadeexampleinline_3)]

2. <xref:System.Windows.EventTrigger> を作成し、その <xref:System.Windows.EventTrigger.Actions%2A> コレクションに <xref:System.Windows.Media.Animation.BeginStoryboard> を追加します。 <xref:System.Windows.EventTrigger> の <xref:System.Windows.EventTrigger.RoutedEvent%2A> プロパティを、<xref:System.Windows.Media.Animation.Storyboard>を開始するルーティングイベントに設定します。 (ルーティングイベントの詳細については、「[ルーティングイベントの概要](../advanced/routed-events-overview.md)」を参照してください)。

    [!code-xaml[animation_ovws_snippet#RectangleOpacityFadeExampleInline_2](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/RectangleOpacityFadeExample.xaml#rectangleopacityfadeexampleinline_2)]

3. 四角形の <xref:System.Windows.FrameworkElement.Triggers%2A> コレクションに <xref:System.Windows.EventTrigger> を追加します。

    [!code-xaml[animation_ovws_snippet#RectangleOpacityFadeExampleInline_1](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/RectangleOpacityFadeExample.xaml#rectangleopacityfadeexampleinline_1)]

<a name="opacity_animation_step3code"></a>

### <a name="part-3-code-associate-the-storyboard-with-an-event-handler"></a>パート 3 (コード): ストーリーボードをイベント ハンドラーに関連付ける

コードで <xref:System.Windows.Media.Animation.Storyboard> を適用して開始する最も簡単な方法は、イベントハンドラーを使用することです。 このセクションでは、<xref:System.Windows.Media.Animation.Storyboard> をコード内のイベントハンドラーに関連付ける方法について説明します。

1. 四角形の <xref:System.Windows.FrameworkElement.Loaded> イベントに登録します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_104](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_104)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_104](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_104)]

2. イベント ハンドラーを宣言します。 イベントハンドラーで、<xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用してストーリーボードを適用します。

    [!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode_105](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode_105)]
    [!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode_105](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode_105)]

### <a name="complete-example"></a>完全なサンプル コード

次に、XAML でフェードインおよびフェードアウトする四角形を作成する方法を示します。

[!code-xaml[animation_ovws2#RectangleOpacityFadeExampleXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml#rectangleopacityfadeexamplexaml)]

ビューでフェードインおよびフェードアウトする四角形をコードで作成する方法を次に示します。

[!code-csharp[animation_ovws2#RectangleOpacityFadeExampleCode](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws2/CSharp/MainWindow.xaml.cs#rectangleopacityfadeexamplecode)]
[!code-vb[animation_ovws2#RectangleOpacityFadeExampleCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws2/VisualBasic/MainWindow.xaml.vb#rectangleopacityfadeexamplecode)]

<a name="animationtypes"></a>

## <a name="animation-types"></a>アニメーションの種類

アニメーションはプロパティの値を生成するため、異なるプロパティの型ごとに異なるアニメーションの種類が存在します。 要素の <xref:System.Windows.FrameworkElement.Width%2A> プロパティなど、<xref:System.Double>を受け取るプロパティをアニメーション化するには <xref:System.Double> 値を生成するアニメーションを使用します。 <xref:System.Windows.Point>を受け取るプロパティをアニメーション化するには、<xref:System.Windows.Point> 値を生成するアニメーションを使用します。 さまざまなプロパティの型の数が多いため、<xref:System.Windows.Media.Animation> 名前空間にはいくつかのアニメーションクラスがあります。 幸いにも、それらは厳密な名前付け規則に従っているため、簡単に区別できます。

- \<*Type*>Animation

  これらは "From/To/By" または "基本" アニメーションと呼ばれ、開始値と宛先値の間をアニメーション化するか、開始値にオフセット値を加算することでアニメーション化します。

  - 開始値を指定するには、アニメーションの From プロパティを設定します。

  - 終了値を指定するには、アニメーションの To プロパティを設定します。

  - オフセット値を指定するには、アニメーションの By プロパティを設定します。

  これらのアニメーションは最も簡単に使用できるため、この概要の例ではこれらを使っています。 From/To/By アニメーションの詳細については、「From/To/By アニメーションの概要」を参照してください。

- \<*Type*>AnimationUsingKeyFrames

  キー フレーム アニメーションは、任意の数のターゲット値を指定でき、補間方法も制御できるため、From/To/By アニメーションよりも強力です。 一部の型は、キー フレーム アニメーションでのみアニメーション化できます。 キーフレームアニメーションの詳細については、 [「キーフレームアニメーションの概要](key-frame-animations-overview.md)」を参照してください。

- \<*Type*>AnimationUsingPath

  パス アニメーションでは、アニメーション値を生成するためにジオメトリック パスを使用できます。

- \<*Type*>AnimationBase

  抽象クラスを実装すると、\<*Type*> 値がアニメーション化されます。 このクラスは、\<*Type*>Animation クラスと \<*Type*>AnimationUsingKeyFrames クラスの基底クラスとして機能します。 これらのクラスは、ユーザー独自のカスタム アニメーションを作成する場合にのみ直接扱う必要があります。 それ以外の場合は、\<*Type*>Animation または KeyFrame\<*Type*>Animation を使います。

ほとんどの場合、<xref:System.Windows.Media.Animation.DoubleAnimation> や <xref:System.Windows.Media.Animation.ColorAnimation>などのアニメーションクラス > \<*型*を使用します。

次の表は、いくつかの一般的なアニメーションの種類と、そこで使われるいくつかのプロパティを示しています。

|プロパティの種類|対応する基本 (From/To/By) アニメーション|対応するキー フレーム アニメーション|対応するパス アニメーション|使用例|
|-------------------|----------------------------------------------------|---------------------------------------|----------------------------------|-------------------|
|<xref:System.Windows.Media.Color>|<xref:System.Windows.Media.Animation.ColorAnimation>|<xref:System.Windows.Media.Animation.ColorAnimationUsingKeyFrames>|なし|<xref:System.Windows.Media.SolidColorBrush> または <xref:System.Windows.Media.GradientStop>の <xref:System.Windows.Media.SolidColorBrush.Color%2A> をアニメーション化します。|
|<xref:System.Double>|<xref:System.Windows.Media.Animation.DoubleAnimation>|<xref:System.Windows.Media.Animation.DoubleAnimationUsingKeyFrames>|<xref:System.Windows.Media.Animation.DoubleAnimationUsingPath>|<xref:System.Windows.Controls.DockPanel> または <xref:System.Windows.Controls.Button>の <xref:System.Windows.FrameworkElement.Height%2A> の <xref:System.Windows.FrameworkElement.Width%2A> をアニメーション化します。|
|<xref:System.Windows.Point>|<xref:System.Windows.Media.Animation.PointAnimation>|<xref:System.Windows.Media.Animation.PointAnimationUsingKeyFrames>|<xref:System.Windows.Media.Animation.PointAnimationUsingPath>|<xref:System.Windows.Media.EllipseGeometry>の <xref:System.Windows.Media.EllipseGeometry.Center%2A> 位置をアニメーション化します。|
|<xref:System.String>|なし|<xref:System.Windows.Media.Animation.StringAnimationUsingKeyFrames>|なし|<xref:System.Windows.Controls.TextBlock> または <xref:System.Windows.Controls.Button>の <xref:System.Windows.Controls.ContentControl.Content%2A> の <xref:System.Windows.Controls.TextBlock.Text%2A> をアニメーション化します。|

<a name="animationsaretimelines"></a>

### <a name="animations-are-timelines"></a>アニメーションはタイムラインである

すべてのアニメーションの種類は、<xref:System.Windows.Media.Animation.Timeline> クラスから継承されます。したがって、すべてのアニメーションは特殊な種類のタイムラインです。 <xref:System.Windows.Media.Animation.Timeline> は、時間のセグメントを定義します。 タイムラインの*タイミングの動作*を指定するには、タイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A>、繰り返し回数、およびそれにかかる時間の短縮についても指定します。

アニメーションは <xref:System.Windows.Media.Animation.Timeline>であるため、時間のセグメントも表します。 また、アニメーションは、指定された時間 (または <xref:System.Windows.Media.Animation.Timeline.Duration%2A>) の進行状況に応じて出力値を計算します。 アニメーションは、進行時、つまり "再生" 時に、関連付けられているプロパティを更新します。

頻繁に使用される3つのタイミングプロパティは、<xref:System.Windows.Media.Animation.Timeline.Duration%2A>、<xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A>、および <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A>です。

#### <a name="the-duration-property"></a>Duration プロパティ

既に述べたように、タイムラインは時間のセグメントを表します。 セグメントの長さは、タイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> によって決定されます。これは通常、<xref:System.Windows.Duration.TimeSpan%2A> 値を使用して指定されます。 タイムラインがその期間の最後に達すると、イテレーションが完了します。

アニメーションは、その <xref:System.Windows.Media.Animation.Timeline.Duration%2A> プロパティを使用して、現在の値を決定します。 アニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> 値を指定しなかった場合は、既定の1秒が使用されます。

次の構文は、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> プロパティの [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 属性構文の簡略化されたバージョンを示しています。

*時間*`:`*分*`:`*秒*

次の表は、いくつかの <xref:System.Windows.Duration> 設定とその結果値を示しています。

|設定|結果の値|
|-------------|---------------------|
|0:0:5.5|5.5 秒。|
|0:30:5.5|30 分 5.5 秒。|
|1:30:5.5|1 時間 30 分 5.5 秒。|

コードで <xref:System.Windows.Duration> を指定する方法の1つとして、<xref:System.TimeSpan.FromSeconds%2A> メソッドを使用して <xref:System.TimeSpan>を作成し、その <xref:System.TimeSpan>を使用して新しい <xref:System.Windows.Duration> 構造体を宣言する方法があります。

<xref:System.Windows.Duration> 値と完全な [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 構文の詳細については、<xref:System.Windows.Duration> 構造体を参照してください。

#### <a name="autoreverse"></a>AutoReverse

<xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティは、タイムラインが <xref:System.Windows.Media.Animation.Timeline.Duration%2A>の末尾に達した後に戻るかどうかを指定します。 このアニメーションのプロパティを `true`に設定した場合、アニメーションは <xref:System.Windows.Media.Animation.Timeline.Duration%2A>の末尾に達した後で反転し、終了値から開始値まで再生されます。 既定では、このプロパティは `false`です。

#### <a name="repeatbehavior"></a>RepeatBehavior

<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティは、タイムラインの再生回数を指定します。 既定では、タイムラインには `1.0`のイテレーション数が含まれています。これは、1回だけ再生され、繰り返されないことを意味します。

これらのプロパティとその他のプロパティの詳細については、「[タイミング動作の概要](timing-behaviors-overview.md)」を参照してください。

<a name="applyanimationstoproperty"></a>

## <a name="applying-an-animation-to-a-property"></a>プロパティにアニメーションを適用する

これまでのセクションでは、さまざまな種類のアニメーションとそれらのタイミング プロパティについて説明しました。 このセクションでは、アニメーション化するプロパティにアニメーションを適用する方法を示します。 <xref:System.Windows.Media.Animation.Storyboard> オブジェクトは、アニメーションをプロパティに適用するための1つの方法を提供します。 <xref:System.Windows.Media.Animation.Storyboard> は、含まれているアニメーションのターゲット情報を提供する*コンテナータイムライン*です。

### <a name="targeting-objects-and-properties"></a>ターゲットとするオブジェクトとプロパティ

<xref:System.Windows.Media.Animation.Storyboard> クラスは、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> および <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> 添付プロパティを提供します。 アニメーションでこれらのプロパティを設定して、アニメーション化する内容をアニメーションに指示します。 ただし、アニメーションがオブジェクトをターゲット指定するには、通常その前にオブジェクの名前を指定する必要があります。

<xref:System.Windows.FrameworkElement> への名前の割り当ては、<xref:System.Windows.Freezable> オブジェクトへの名前の割り当てとは異なります。 ほとんどのコントロールとパネルはフレームワーク要素ですが、ブラシ、変換、ジオメトリなどのほとんどの純粋なグラフィカル オブジェクトは Freezable オブジェクトです。 型が <xref:System.Windows.FrameworkElement> であるか <xref:System.Windows.Freezable>であるかわからない場合は、リファレンスドキュメントの「**継承階層**」セクションを参照してください。

- アニメーションターゲット <xref:System.Windows.FrameworkElement> を作成するには、その <xref:System.Windows.FrameworkElement.Name%2A> プロパティを設定して名前を指定します。 コードでは、<xref:System.Windows.FrameworkElement.RegisterName%2A> メソッドを使用して、要素名を所属するページに登録する必要もあります。

- <xref:System.Windows.Freezable> オブジェクトを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]のアニメーションターゲットにするには、 [X:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)を使用して名前を割り当てます。 コードでは、<xref:System.Windows.FrameworkElement.RegisterName%2A> メソッドを使用して、オブジェクトが属しているページにオブジェクトを登録するだけです。

以下のセクションでは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] とコードで要素に名前を付ける例を示します。 名前付けとターゲット設定の詳細については、「[ストーリーボードの概要](storyboards-overview.md)」を参照してください。

### <a name="applying-and-starting-storyboards"></a>ストーリーボードの適用と開始

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]でストーリーボードを開始するには、ストーリーボードを <xref:System.Windows.EventTrigger>に関連付けます。 <xref:System.Windows.EventTrigger> は、指定されたイベントが発生したときに実行するアクションを記述するオブジェクトです。 これらのアクションの1つに、ストーリーボードを開始するために使用する <xref:System.Windows.Media.Animation.BeginStoryboard> アクションを指定できます。 イベント トリガーは、アプリケーションが特定のイベントに応答する方法を指定できるようにするため、概念はイベント ハンドラーに似ています。 イベントハンドラーとは異なり、イベントトリガーは [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で完全に記述できます。他のコードは必要ありません。

コードで <xref:System.Windows.Media.Animation.Storyboard> を開始するには、<xref:System.Windows.EventTrigger> を使用するか、<xref:System.Windows.Media.Animation.Storyboard> クラスの <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用します。

<a name="controllingstoryboards"></a>

## <a name="interactively-control-a-storyboard"></a>ストーリーボードを対話的に制御する

前の例では、イベントが発生したときに <xref:System.Windows.Media.Animation.Storyboard> を開始する方法を示しました。 また、開始後に <xref:System.Windows.Media.Animation.Storyboard> を対話形式で制御することもできます。これにより、一時停止、再開、停止、塗りつぶし期間への先行、シーク、<xref:System.Windows.Media.Animation.Storyboard>の削除を行うことができます。 <xref:System.Windows.Media.Animation.Storyboard>を対話的に制御する方法の詳細と例については、「[ストーリーボードの概要](storyboards-overview.md)」を参照してください。

<a name="fillbehaviorsection"></a>

## <a name="what-happens-after-an-animation-ends"></a>アニメーションの終了後の動作

<xref:System.Windows.Media.Animation.FillBehavior> プロパティは、タイムラインが終了したときの動作を指定します。 既定では、タイムラインは終了時に <xref:System.Windows.Media.Animation.ClockState.Filling> 開始されます。 <xref:System.Windows.Media.Animation.ClockState.Filling> のアニメーションは、最終的な出力値を保持します。

<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティが <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A>に設定されているため、前の例の <xref:System.Windows.Media.Animation.DoubleAnimation> は終了しません。 次の例では、類似のアニメーションを使って四角形をアニメーション化します。 前の例とは異なり、このアニメーションの <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> と <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> のプロパティは既定値のままです。 したがって、アニメーションは 5 秒間で 1 から 0 まで進行し、停止します。

[!code-xaml[animation_ovws_snippet#FillBehaviorExampleRectangleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/FillBehaviorExample.xaml#fillbehaviorexamplerectangleinline)]

[!code-csharp[animation_ovws_procedural_snip#FillBehaviorExampleRectangleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_procedural_snip/CSharp/FillBehaviorExample.cs#fillbehaviorexamplerectangleinline)]
[!code-vb[animation_ovws_procedural_snip#FillBehaviorExampleRectangleInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animation_ovws_procedural_snip/visualbasic/fillbehaviorexample.vb#fillbehaviorexamplerectangleinline)]

<xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> が <xref:System.Windows.Media.Animation.FillBehavior.HoldEnd>の既定値から変更されていないため、アニメーションは終了時に最終的な値0を保持します。 したがって、アニメーションが終了した後も四角形の <xref:System.Windows.UIElement.Opacity%2A> は0のままです。 四角形の <xref:System.Windows.UIElement.Opacity%2A> を別の値に設定した場合、アニメーションはまだ <xref:System.Windows.UIElement.Opacity%2A> プロパティに影響しているので、コードは効果がないように見えます。

コードでアニメーション化されたプロパティを再び制御する方法の1つとして、<xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを使用し、<xref:System.Windows.Media.Animation.AnimationTimeline> パラメーターに null を指定する方法があります。 詳細と例については、「[ストーリーボードを使用してアニメーション化した後にプロパティを設定する](how-to-set-a-property-after-animating-it-with-a-storyboard.md)」を参照してください。

<xref:System.Windows.Media.Animation.ClockState.Active> または <xref:System.Windows.Media.Animation.ClockState.Filling> アニメーションを持つプロパティ値を設定しても効果はありませんが、プロパティ値は変更されることに注意してください。 詳細については、「[アニメーションとタイミングシステムの概要](animation-and-timing-system-overview.md)」を参照してください。

<a name="databindingAndAnimatingAnimationsSection"></a>

## <a name="data-binding-and-animating-animations"></a>アニメーションのデータ バインディングとアニメーション化

ほとんどのアニメーションプロパティは、データバインドまたはアニメーション化できます。たとえば、<xref:System.Windows.Media.Animation.DoubleAnimation>の <xref:System.Windows.Media.Animation.Timeline.Duration%2A> プロパティをアニメーション化できます。 ただし、タイミング システムの動作が原因で、データ バインドまたはアニメーション化されたアニメーションは他のデータ バインドまたはアニメーション化されたオブジェクトと同様には動作しません。 その動作を理解するには、アニメーションをプロパティに適用する意味を理解することが役立ちます。

四角形の <xref:System.Windows.UIElement.Opacity%2A> をアニメーション化する方法については、前のセクションの例を参照してください。 前の例の四角形が読み込まれると、そのイベントトリガーによって <xref:System.Windows.Media.Animation.Storyboard>が適用されます。 タイミングシステムは、<xref:System.Windows.Media.Animation.Storyboard> とそのアニメーションのコピーを作成します。 これらのコピーは固定され (読み取り専用になり)、<xref:System.Windows.Media.Animation.Clock> オブジェクトがそれらから作成されます。 これらのクロックは、ターゲット プロパティをアニメーション化する実際の作業を実行します。

タイミングシステムによって <xref:System.Windows.Media.Animation.DoubleAnimation> のクロックが作成され、<xref:System.Windows.Media.Animation.DoubleAnimation>の <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> と <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> によって指定されたオブジェクトおよびプロパティに適用されます。 この場合、タイミングシステムは、"MyRectangle" という名前のオブジェクトの <xref:System.Windows.UIElement.Opacity%2A> プロパティに時計を適用します。

<xref:System.Windows.Media.Animation.Storyboard>のクロックも作成されますが、時計はプロパティには適用されません。 その目的は、<xref:System.Windows.Media.Animation.DoubleAnimation>に対して作成される時計である子クロックを制御することです。

アニメーションがデータ バインディングまたはアニメーションの変更を反映するためには、そのクロックを再生成する必要があります。 クロックは、自動的には再生成されません。 アニメーションに変更を反映させるには、<xref:System.Windows.Media.Animation.BeginStoryboard> または <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用して、そのストーリーボードを再適用します。 これらのメソッドのいずれかを使うと、アニメーションが再起動されます。 コードでは、<xref:System.Windows.Media.Animation.Storyboard.Seek%2A> メソッドを使用して、ストーリーボードを前の位置に戻すことができます。

データバインドされたアニメーションの例については、「[キースプラインアニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/KeySplineAnimations)」を参照してください。 アニメーションとタイミングシステムの動作の詳細については、「[アニメーションとタイミングシステムの概要](animation-and-timing-system-overview.md)」を参照してください。

<a name="otherWaysToAnimateSection"></a>

## <a name="other-ways-to-animate"></a>その他のアニメーション化方法

この概要の例では、ストーリーボードを使ってアニメーション化する方法を示します。 コードを使う場合は、その他のいくつかの方法でアニメーション化できます。 詳細については、「[プロパティアニメーションの手法の概要](property-animation-techniques-overview.md)」を参照してください。

<a name="animation_samples"></a>

## <a name="animation-samples"></a>アニメーションのサンプル

以下のサンプルは、アプリケーションへのアニメーションの追加を開始するのに役立つ場合があります。

- [アニメーションのターゲット値 (From、To、および By) のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/TargetValues)

  さまざまな From/To/By 設定を示します。

- [アニメーションのタイミング動作のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/AnimationTiming)

  アニメーションのタイミング動作を制御するさまざまな方法を示します。 このサンプルは、アニメーションの宛先値をデータ バインドする方法も示しています。

<a name="related_topics"></a>

## <a name="related-topics"></a>関連トピック

|タイトル|説明|
|-----------|-----------------|
|[アニメーションとタイミング システムの概要](animation-and-timing-system-overview.md)|タイミングシステムが <xref:System.Windows.Media.Animation.Timeline> クラスと <xref:System.Windows.Media.Animation.Clock> クラスを使用して、アニメーションを作成できるようにする方法について説明します。|
|[アニメーションのヒントとテクニック](animation-tips-and-tricks.md)|パフォーマンスなど、アニメーションでの問題を解決するための役に立つヒントの一覧を示します。|
|[カスタム アニメーションの概要](custom-animations-overview.md)|アニメーション システムをキー フレーム、アニメーション クラス、またはフレームごとのコールバックで拡張する方法について説明します。|
|[From/To/By アニメーションの概要](from-to-by-animations-overview.md)|2 つの値の間を遷移するアニメーションを作成する方法について説明します。|
|[キー フレーム アニメーションの概要](key-frame-animations-overview.md)|補間メソッドを制御する機能など、複数のターゲット値を持つアニメーションを作成する方法について説明します。|
|[イージング関数](easing-functions.md)|数式をアニメーションに適用してバウンスなどの現実的な動作を実現する方法について説明します。|
|[パス アニメーションの概要](path-animations-overview.md)|複雑なパスに沿ってオブジェクトを移動または回転する方法について説明します。|
|[プロパティ アニメーションの手法の概要](property-animation-techniques-overview.md)|ストーリーボード、ローカル アニメーション、クロック、フレームごとのアニメーションを使ったプロパティのアニメーションについて説明します。|
|[ストーリーボードの概要](storyboards-overview.md)|複数のタイムラインを持つストーリーボードを使って複雑なアニメーションを作成する方法について説明します。|
|[タイミング動作の概要](timing-behaviors-overview.md)|アニメーションで使用される <xref:System.Windows.Media.Animation.Timeline> の型とプロパティについて説明します。|
|[タイミング イベントの概要](timing-events-overview.md)|開始、一時停止、再開、スキップ、停止など、タイムラインのポイントでコードを実行するために <xref:System.Windows.Media.Animation.Timeline> および <xref:System.Windows.Media.Animation.Clock> オブジェクトで使用できるイベントについて説明します。|
|[データ バインドに関する「方法」トピック](animation-and-timing-how-to-topics.md)|アプリケーションでアニメーションとタイムラインを使うためのコード例を示します。|
|[クロックに関する「方法」トピック](clocks-how-to-topics.md)|アプリケーションで <xref:System.Windows.Media.Animation.Clock> オブジェクトを使用するためのコード例が含まれています。|
|[キー フレームに関する「方法」トピック](key-frame-animation-how-to-topics.md)|アプリケーションでキー フレーム アニメーションを使うためのコード例を示します。|
|[パス アニメーションに関する「方法」トピック](path-animation-how-to-topics.md)|アプリケーションでパス アニメーションを使うためのコード例を示します。|

<a name="reference"></a>

## <a name="reference"></a>参照

- <xref:System.Windows.Media.Animation.Timeline>

- <xref:System.Windows.Media.Animation.Storyboard>

- <xref:System.Windows.Media.Animation.BeginStoryboard>

- <xref:System.Windows.Media.Animation.Clock>
