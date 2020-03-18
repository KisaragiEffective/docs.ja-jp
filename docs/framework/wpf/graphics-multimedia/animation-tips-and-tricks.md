---
title: アニメーションのヒントとテクニック
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- troubleshooting [WPF], animation
- animations [WPF], FillBehavior property
- troubleshooting animation [WPF]
- animating objects [WPF], troubleshooting
- animation tips and tricks [WPF]
- tips and tricks [WPF], animation
- performance troubleshooting [WPF], animation
- animations [WPF], use of system resources
ms.assetid: e467796b-d5d4-45a6-a108-8c5d7ff69a0f
ms.openlocfilehash: ef59631663e6cf1c98adfed77a2dbdb6ca124fa1
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636030"
---
# <a name="animation-tips-and-tricks"></a>アニメーションのヒントとテクニック
WPF でアニメーションを使用する場合、アニメーションのパフォーマンスを向上させ、フラストレーションを軽減するためのヒントとテクニックがいくつかあります。  
  
<a name="generalissuessection"></a>   
## <a name="general-issues"></a>一般的な問題  
  
### <a name="animating-the-position-of-a-scroll-bar-or-slider-freezes-it"></a>スクロール バーまたはスライダーの位置をアニメーション化するとフリーズする  
 <xref:System.Windows.Media.Animation.FillBehavior> <xref:System.Windows.Media.Animation.FillBehavior.HoldEnd> (既定値) のアニメーションを使用してスクロールバーまたはスライダーの位置をアニメーション化すると、ユーザーはスクロールバーまたはスライダーを移動できなくなります。 原因は、アニメーションが終了しても、ターゲット プロパティの基底値をまだオーバーライドしているためです。 アニメーションによってプロパティの現在の値が上書きされないようにするには、それを削除するか、<xref:System.Windows.Media.Animation.FillBehavior.Stop>の <xref:System.Windows.Media.Animation.FillBehavior> を指定します。 詳細と例については、「[ストーリーボードを使用してアニメーション化した後にプロパティを設定する](how-to-set-a-property-after-animating-it-with-a-storyboard.md)」を参照してください。  
  
### <a name="animating-the-output-of-an-animation-has-no-effect"></a>アニメーションの出力のアニメーション化の効果がない  
 別のアニメーションの出力であるオブジェクトをアニメーション化することはできません。 たとえば、<xref:System.Windows.Media.Animation.ObjectAnimationUsingKeyFrames> を使用して <xref:System.Windows.Media.RadialGradientBrush> から <xref:System.Windows.Media.SolidColorBrush>に <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.Shapes.Shape.Fill%2A> をアニメーション化する場合、<xref:System.Windows.Media.RadialGradientBrush> または <xref:System.Windows.Media.SolidColorBrush>のプロパティをアニメーション化することはできません。  
  
### <a name="cant-change-the-value-of-a-property-after-animating-it"></a>プロパティをアニメーション化した後にその値を変更できない  
 場合によっては、アニメーションが完了した後でも、アニメーション化の後にプロパティの値を変更できないように見えることがあります。 原因は、アニメーションが終了しても、プロパティの基底値をまだオーバーライドしているためです。 アニメーションによってプロパティの現在の値が上書きされないようにするには、それを削除するか、<xref:System.Windows.Media.Animation.FillBehavior.Stop>の <xref:System.Windows.Media.Animation.FillBehavior> を指定します。 詳細と例については、「[ストーリーボードを使用してアニメーション化した後にプロパティを設定する](how-to-set-a-property-after-animating-it-with-a-storyboard.md)」を参照してください。  
  
### <a name="changing-a-timeline-has-no-effect"></a>タイムラインを変更しても効果がない  
 ほとんどの <xref:System.Windows.Media.Animation.Timeline> プロパティは system.windows.media.animation.animatable> ですが、データバインドすることができますが、アクティブな <xref:System.Windows.Media.Animation.Timeline> のプロパティ値を変更しても効果はありません。 これは、<xref:System.Windows.Media.Animation.Timeline> が開始されたときに、タイミングシステムが <xref:System.Windows.Media.Animation.Timeline> のコピーを作成し、それを使用して <xref:System.Windows.Media.Animation.Clock> オブジェクトを作成するためです。 元を変更しても、システムのコピーには影響しません。  
  
 <xref:System.Windows.Media.Animation.Timeline> が変更を反映するには、そのクロックを再生成し、以前に作成したクロックを置き換えるために使用する必要があります。 クロックは、自動的には再生成されません。 タイムラインの変更を適用するいくつかの方法を次に示します。  
  
- タイムラインがまたは <xref:System.Windows.Media.Animation.Storyboard>に属している場合は、<xref:System.Windows.Media.Animation.BeginStoryboard> または <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用してストーリーボードを再適用することで、変更を反映させることができます。 これには、アニメーションが再起動されるという副作用があります。 コードでは、<xref:System.Windows.Media.Animation.Storyboard.Seek%2A> メソッドを使用して、ストーリーボードを前の位置に進めることができます。  
  
- <xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを使用してプロパティにアニメーションを直接適用した場合は、もう一度 <xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを呼び出し、変更されたアニメーションを渡します。  
  
- クロック レベルで直接操作している場合は、新しいクロック セットを作成して適用し、それらを使って、生成されたクロックの以前のセットを置換します。  
  
 タイムラインとクロックの詳細については、「[アニメーションとタイミングシステムの概要](animation-and-timing-system-overview.md)」を参照してください。  
  
### <a name="fillbehaviorstop-doesnt-work-as-expected"></a>FillBehavior.Stop が期待どおりに動作しない  
 <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティを <xref:System.Windows.Media.Animation.FillBehavior.Stop> に設定すると、<xref:System.Windows.Media.Animation.HandoffBehavior.SnapshotAndReplace>の <xref:System.Windows.Media.Animation.BeginStoryboard.HandoffBehavior%2A> 設定があるために1つのアニメーションが別のアニメーションに "ハンドオフ" された場合など、何の効果もありません。  
  
 次の例では、<xref:System.Windows.Controls.Canvas>、<xref:System.Windows.Shapes.Rectangle> と <xref:System.Windows.Media.TranslateTransform>を作成します。 <xref:System.Windows.Media.TranslateTransform> は、<xref:System.Windows.Controls.Canvas>の周り <xref:System.Windows.Shapes.Rectangle> を移動するようにアニメーション化されます。  
  
 [!code-xaml[AnimationTipsAndTricksSample_snip#FillBehaviorTipAnimatedObject](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml#fillbehaviortipanimatedobject)]  
  
 このセクションの例では、前のオブジェクトを使用して、<xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティが期待どおりに動作しないいくつかのケースを示します。  
  
#### <a name="fillbehaviorstop-and-handoffbehavior-with-multiple-animations"></a>複数のアニメーションでの FillBehavior="Stop" と HandoffBehavior  
 アニメーションが2番目のアニメーションで置き換えられたときに、その <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティを無視するように見えることがあります。 次の例を実行します。この例では、2つの <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを作成し、それらを使用して前の例で示したのと同じ <xref:System.Windows.Media.TranslateTransform> をアニメーション化しています。  
  
 最初の <xref:System.Windows.Media.Animation.Storyboard>、`B1`は、<xref:System.Windows.Media.TranslateTransform> の <xref:System.Windows.Media.TranslateTransform.X%2A> プロパティを0から350にアニメーション化します。これにより、四角形が350ピクセル右に移動します。 アニメーションが期間の終わりに達し、再生が停止すると、<xref:System.Windows.Media.TranslateTransform.X%2A> プロパティは元の値0に戻ります。 その結果、四角形は右に 350 ピクセル移動し、元の位置に移動します。  
  
 [!code-xaml[AnimationTipsAndTricksSample_snip#FillBehaviorTipStoryboardB1Button](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml#fillbehaviortipstoryboardb1button)]  
  
 2番目の `B2`<xref:System.Windows.Media.Animation.Storyboard>は、同じ <xref:System.Windows.Media.TranslateTransform>の <xref:System.Windows.Media.TranslateTransform.X%2A> プロパティもアニメーション化します。 この <xref:System.Windows.Media.Animation.Storyboard> では、アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティのみが設定されているため、アニメーションは、アニメーション化するプロパティの現在の値を開始値として使用します。  
  
 [!code-xaml[AnimationTipsAndTricksSample_snip#FillBehaviorTipStoryboardB2Button](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml#fillbehaviortipstoryboardb2button)]  
  
 最初の <xref:System.Windows.Media.Animation.Storyboard> の再生中に2番目のボタンをクリックすると、次のような動作が発生する可能性があります。  
  
1. アニメーションには <xref:System.Windows.Media.Animation.FillBehavior.Stop>の <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> があるため、最初のストーリーボードが終了し、四角形が元の位置に戻されます。  
  
2. 2 つ目のストーリーボードが反映され、現在位置である 0 から 500 にアニメーション化します。  
  
 **しかし、これは行われません。** 代わりに、四角形は戻らずに、右に移動し続けます。 その理由は、2 番目のアニメーションは最初のアニメーションの現在の値を開始値として使い、その値から 500 までアニメーション化するためです。 <xref:System.Windows.Media.Animation.HandoffBehavior.SnapshotAndReplace><xref:System.Windows.Media.Animation.HandoffBehavior> が使用されているために2番目のアニメーションが最初のアニメーションを置き換えるときは、最初のアニメーションの <xref:System.Windows.Media.Animation.FillBehavior> は関係ありません。  
  
#### <a name="fillbehavior-and-the-completed-event"></a>FillBehavior と完了イベント  
 次の例では、<xref:System.Windows.Media.Animation.FillBehavior.Stop><xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> が効果がないと思われるもう1つのシナリオを示します。 ここでも、ストーリーボードを使用して、0 ~ 350 の <xref:System.Windows.Media.TranslateTransform> の <xref:System.Windows.Media.TranslateTransform.X%2A> プロパティをアニメーション化します。 ただし今回は、この例では <xref:System.Windows.Media.Animation.Timeline.Completed> イベントに登録します。  
  
 [!code-xaml[AnimationTipsAndTricksSample_snip#FillBehaviorTipStoryboardCButton](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml#fillbehaviortipstoryboardcbutton)]  
  
 <xref:System.Windows.Media.Animation.Timeline.Completed> イベントハンドラーは、同じプロパティを現在の値から500にアニメーション化する別の <xref:System.Windows.Media.Animation.Storyboard> を開始します。  
  
 [!code-csharp[AnimationTipsAndTricksSample_snip#FillBehaviorTipStoryboardC1CompletedHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml.cs#fillbehaviortipstoryboardc1completedhandler)]
 [!code-vb[AnimationTipsAndTricksSample_snip#FillBehaviorTipStoryboardC1CompletedHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/VisualBasic/FillBehaviorTip.xaml.vb#fillbehaviortipstoryboardc1completedhandler)]  
  
 次に示すのは、2番目の <xref:System.Windows.Media.Animation.Storyboard> をリソースとして定義するマークアップです。  
  
 [!code-xaml[AnimationTipsAndTricksSample_snip#FillBehaviorTipResources](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimationTipsAndTricksSample_snip/CSharp/FillBehaviorTip.xaml#fillbehaviortipresources)]  
  
 <xref:System.Windows.Media.Animation.Storyboard>を実行すると、<xref:System.Windows.Media.TranslateTransform> の <xref:System.Windows.Media.TranslateTransform.X%2A> プロパティが0から350のアニメーション化され、完了後に0に戻されます (<xref:System.Windows.Media.Animation.FillBehavior.Stop>の <xref:System.Windows.Media.Animation.FillBehavior> 設定があるため)。その後、0から500のアニメーション化が行われます。 代わりに、<xref:System.Windows.Media.TranslateTransform> は 0 ~ 350 の間で500にアニメーション化されます。  
  
 これは、WPF によってイベントが発生する順序と、プロパティ値がキャッシュされ、プロパティが無効にされない限り再計算されないためです。 <xref:System.Windows.Media.Animation.Timeline.Completed> イベントは、ルートタイムライン (最初の <xref:System.Windows.Media.Animation.Storyboard>) によってトリガーされたため、最初に処理されます。 現時点では、<xref:System.Windows.Media.TranslateTransform.X%2A> プロパティはまだ無効になっていないため、アニメーション化された値を返します。 2番目の <xref:System.Windows.Media.Animation.Storyboard> は、キャッシュされた値を開始値として使用し、アニメーション化を開始します。  
  
<a name="performancesection"></a>   
## <a name="performance"></a>パフォーマンス  
  
### <a name="animations-continue-to-run-after-navigating-away-from-a-page"></a>アニメーションがページからの移動後も実行され続ける  
 実行中のアニメーションを含む <xref:System.Windows.Controls.Page> から移動すると、そのアニメーションは、<xref:System.Windows.Controls.Page> がガベージコレクトされるまで再生を続行します。 使っているナビゲーション システムによっては、離れた後のページが無期限にメモリに残り、その間、そのアニメーションでリソースが消費されることがあります。 これは、ページに常時実行 ("アンビエント") アニメーションが含まれる場合に最も顕著です。  
  
 このため、ページから移動するときに、<xref:System.Windows.FrameworkElement.Unloaded> イベントを使用してアニメーションを削除することをお勧めします。  
  
 アニメーションを削除する別の方法もあります。 次の手法は、<xref:System.Windows.Media.Animation.Storyboard>に属するアニメーションを削除するために使用できます。  
  
- イベントトリガーを使用して開始した <xref:System.Windows.Media.Animation.Storyboard> を削除する方法については、「[方法: ストーリーボードを削除](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms749412(v=vs.90))する」を参照してください。  
  
- コードを使用して <xref:System.Windows.Media.Animation.Storyboard>を削除するには、<xref:System.Windows.Media.Animation.Storyboard.Remove%2A> メソッドを参照してください。  
  
 次の手法は、アニメーションの開始方法に関係なく使用できます。  
  
- 特定のプロパティからアニメーションを削除するには、<xref:System.Windows.Media.Animation.Animatable.BeginAnimation%28System.Windows.DependencyProperty%2CSystem.Windows.Media.Animation.AnimationTimeline%29> メソッドを使用します。 最初のパラメーターとしてアニメーション化するプロパティを指定し、2番目のパラメーターとして `null` します。 これにより、すべてのアニメーション クロックがプロパティから削除されます。  
  
 プロパティをアニメーション化するさまざまな方法の詳細については、「[プロパティアニメーションの手法の概要](property-animation-techniques-overview.md)」を参照してください。  
  
### <a name="using-the-compose-handoffbehavior-consumes-system-resources"></a>HandoffBehavior を使うとシステム リソースが消費される  
 <xref:System.Windows.Media.Animation.HandoffBehavior.Compose><xref:System.Windows.Media.Animation.HandoffBehavior>を使用してプロパティに <xref:System.Windows.Media.Animation.Storyboard>、<xref:System.Windows.Media.Animation.AnimationTimeline>、または <xref:System.Windows.Media.Animation.AnimationClock> を適用すると、そのプロパティに以前関連付けられていたすべての <xref:System.Windows.Media.Animation.Clock> オブジェクトが引き続きシステムリソースを消費します。タイミングシステムでは、これらのクロックは自動的に削除されません。  
  
 <xref:System.Windows.Media.Animation.HandoffBehavior.Compose>を使用して多数のクロックを適用したときのパフォーマンスの問題を回避するには、アニメーション化されたプロパティの完了後に、作成中のクロックを削除する必要があります。 クロックを削除する方法はいくつかあります。  
  
- プロパティからすべてのクロックを削除するには、アニメーション化されたオブジェクトの <xref:System.Windows.Media.Animation.Animatable.ApplyAnimationClock%28System.Windows.DependencyProperty%2CSystem.Windows.Media.Animation.AnimationClock%29> または <xref:System.Windows.Media.Animation.Animatable.BeginAnimation%28System.Windows.DependencyProperty%2CSystem.Windows.Media.Animation.AnimationTimeline%29> メソッドを使用します。 最初のパラメーターとしてアニメーション化するプロパティを指定し、2番目のパラメーターとして `null` します。 これにより、すべてのアニメーション クロックがプロパティから削除されます。  
  
- 特定の <xref:System.Windows.Media.Animation.AnimationClock> をクロックの一覧から削除するには、<xref:System.Windows.Media.Animation.AnimationClock> の <xref:System.Windows.Media.Animation.Clock.Controller%2A> プロパティを使用して <xref:System.Windows.Media.Animation.ClockController>を取得し、次に <xref:System.Windows.Media.Animation.ClockController.Remove%2A> の <xref:System.Windows.Media.Animation.ClockController>メソッドを呼び出します。 これは通常、クロックの <xref:System.Windows.Media.Animation.Clock.Completed> イベントハンドラーで実行されます。 <xref:System.Windows.Media.Animation.ClockController>によって制御できるのはルートクロックのみであることに注意してください。子クロックの <xref:System.Windows.Media.Animation.Clock.Controller%2A> プロパティは `null`を返します。 また、クロックの有効期間が無期限の場合は、<xref:System.Windows.Media.Animation.Clock.Completed> イベントが呼び出されないことにも注意してください。  その場合、ユーザーは <xref:System.Windows.Media.Animation.ClockController.Remove%2A>をいつ呼び出すかを決定する必要があります。  
  
 これは主に、有効期間が長いオブジェクトでのアニメーションの問題です。  オブジェクトがガベージ コレクションされる場合は、そのクロックも切断されて、ガベージ コレクションされます。  
  
 クロックオブジェクトの詳細については、「[アニメーションとタイミングシステムの概要](animation-and-timing-system-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [アニメーションの概要](animation-overview.md)
