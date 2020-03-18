---
title: タイミング動作の概要
ms.date: 03/30/2017
helpviewer_keywords:
- timing behaviors [WPF]
- behaviors [WPF], timing
ms.assetid: 5b714d46-bd46-48b8-b467-b4be89ba3091
ms.openlocfilehash: a85f980a0cefaa282e9e92d533a2306a9009e3e7
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559951"
---
# <a name="timing-behaviors-overview"></a>タイミング動作の概要
このトピックでは、アニメーションとその他の <xref:System.Windows.Media.Animation.Timeline> オブジェクトのタイミングの動作について説明します。  
  
<a name="prerequisites"></a>   
## <a name="prerequisites"></a>Prerequisites  
 このトピックを理解するには、基本的なアニメーション機能に精通している必要があります。 詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください。  
  
<a name="timelinetypes"></a>   
## <a name="timeline-types"></a>タイムラインの型  
 時間のセグメントを表す <xref:System.Windows.Media.Animation.Timeline>。 用意されているプロパティを使用して、そのセグメントの長さ、開始時間、繰り返し回数、時間の進行の速度などを指定できます。  
  
 Timeline クラスを継承するクラスには、アニメーションやメディアの再生などの追加機能が用意されています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、次の <xref:System.Windows.Media.Animation.Timeline> の種類が用意されています。  
  
|タイムラインの型|説明|  
|-------------------|-----------------|  
|<xref:System.Windows.Media.Animation.AnimationTimeline>|アニメーションプロパティの出力値を生成する <xref:System.Windows.Media.Animation.Timeline> オブジェクトの抽象基本クラス。|  
|<xref:System.Windows.Media.MediaTimeline>|メディア ファイルから出力を生成します。|  
|<xref:System.Windows.Media.Animation.ParallelTimeline>|子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトをグループ化および制御する <xref:System.Windows.Media.Animation.TimelineGroup> の型。|  
|<xref:System.Windows.Media.Animation.Storyboard>|含まれているタイムラインオブジェクトのターゲット情報を提供する <xref:System.Windows.Media.Animation.ParallelTimeline> の型。|  
|<xref:System.Windows.Media.Animation.Timeline>|タイミング動作を定義する抽象基本クラス。|  
|<xref:System.Windows.Media.Animation.TimelineGroup>|他の <xref:System.Windows.Media.Animation.Timeline> オブジェクトを含むことができる <xref:System.Windows.Media.Animation.Timeline> オブジェクトの抽象クラスです。|  
  
<a name="propertiesthatcontroltimelinelength"></a>   
## <a name="properties-that-control-the-length-of-a-timeline"></a>タイムラインの長さを制御するプロパティ  
 <xref:System.Windows.Media.Animation.Timeline> は時間のセグメントを表し、タイムラインの長さはさまざまな方法で記述できます。 タイムラインの長さを示すいくつかの用語の定義を次の表に示します。  
  
|用語|説明|[プロパティ]||||  
|----------|-----------------|----------------|-|-|-|  
|単純継続時間|タイムラインが順方向の反復を 1 回完了するのに要する時間の長さ。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>||||  
|1 回の繰り返し|タイムラインが1回再生されるまでにかかる時間の長さ。 <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティが true の場合は、1回前に再生します。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>、<xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A>||||  
|アクティブ期間|タイムラインが <xref:System.Windows.Media.Animation.RepeatBehavior> プロパティによって指定されたすべての繰り返しを完了するためにかかる時間の長さ。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>では、 <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A>では、 <xref:System.Windows.Media.Animation.RepeatBehavior>||||  
  
<a name="duration"></a>   
### <a name="the-duration-property"></a>Duration プロパティ  
 既に述べたように、タイムラインは時間のセグメントを表します。 そのセグメントの長さは、タイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A>によって決まります。 タイムラインは、期間の最後に到達すると、再生を停止します。 タイムラインに子タイムラインがある場合は、子も再生を停止します。 アニメーションの場合、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> は、アニメーションが開始値から終了値に遷移するまでの時間を指定します。 タイムラインの期間は、1回の反復の継続時間と、繰り返しを含むアニメーションの合計時間を区別するために、*単純な期間*と呼ばれることもあります。 期間は、有限の時間値または特殊な値 <xref:System.Windows.Duration.Automatic%2A> または <xref:System.Windows.Duration.Forever%2A>を使用して指定できます。 アニメーションの期間は <xref:System.Windows.Duration.TimeSpan%2A> 値に解決されるため、値を切り替えることができます。  
  
 次の例は、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> が5秒の <xref:System.Windows.Media.Animation.DoubleAnimation> を示しています。  
  
 [!code-xaml[animation_ovws_snippet#AnimationWith5SecondDurationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#animationwith5seconddurationinline)]  
  
 コンテナータイムライン (<xref:System.Windows.Media.Animation.Storyboard> や <xref:System.Windows.Media.Animation.ParallelTimeline>など) の既定の期間は <xref:System.Windows.Duration.Automatic%2A>です。これは、最後の子の再生が停止したときに自動的に終了することを意味します。 次の例では、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> が5秒に解決され、すべての子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトが完了するまでにかかる時間の長さを <xref:System.Windows.Media.Animation.Storyboard> 示しています。  
  
 [!code-xaml[animation_ovws_snippet#ContainerTimelineExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#containertimelineexampleinline)]  
  
 コンテナータイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を <xref:System.Windows.Duration.TimeSpan%2A> 値に設定することにより、その子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトが再生するよりも長くまたは短くすることができます。 <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を、コンテナーのタイムラインの子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの長さよりも小さい値に設定すると、コンテナーのタイムラインが行われたときに子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの再生が停止します。 次の例では、前の例の <xref:System.Windows.Media.Animation.Storyboard> の <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を3秒に設定しています。 その結果、ターゲットの四角形の幅が60にアニメーション化されている場合、最初の <xref:System.Windows.Media.Animation.DoubleAnimation> は3秒後に進行を停止します。  
  
 [!code-xaml[animation_ovws_snippet#ContainerTimelineWithShorterDurationExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#containertimelinewithshorterdurationexampleinline)]  
  
<a name="repeatinganimations"></a>   
### <a name="the-repeatbehavior-property"></a>RepeatBehavior プロパティ  
 <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティは、単純な期間を繰り返す回数を制御します。 <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用すると、タイムラインの再生回数 (イテレーション <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A>) または再生する時間の合計 (繰り返し <xref:System.Windows.Media.Animation.RepeatBehavior.Duration%2A>) を指定できます。 いずれの場合も、アニメーションは、要求されたカウントまたは期間を満たすのに必要な回数だけ実行を繰り返します。 既定では、タイムラインには `1.0`のイテレーション数があります。これは、1回だけ再生され、繰り返しは行われないことを意味します。  
  
 次の例では、<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用して、反復回数を指定することによって、<xref:System.Windows.Media.Animation.DoubleAnimation> を単純な継続時間の2倍にします。  
  
 [!code-xaml[animation_ovws_snippet#TBRepeatBehavior2xExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbrepeatbehavior2xexampleinline)]  
  
 次の例では、<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用して、<xref:System.Windows.Media.Animation.DoubleAnimation> を単純な期間の半分に再生します。  
  
 [!code-xaml[animation_ovws_snippet#TBRepeatBehavior05xExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbrepeatbehavior05xexampleinline)]  
  
 <xref:System.Windows.Media.Animation.Timeline> の [<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A>] プロパティを <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A>に設定すると、対話的に、またはタイミングシステムによって停止されるまで、<xref:System.Windows.Media.Animation.Timeline> が繰り返されます。 次の例では、<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用して、<xref:System.Windows.Media.Animation.DoubleAnimation> が無期限に再生されるようにします。  
  
 [!code-xaml[animation_ovws_snippet#TBRepeatBehaviorForeverExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbrepeatbehaviorforeverexampleinline)]  
  
 その他の例については、「[アニメーションを繰り返す](how-to-repeat-an-animation.md)」を参照してください。  
  
<a name="autoreverseproperty"></a>   
### <a name="the-autoreverse-property"></a>AutoReverse プロパティ  
 <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティは、<xref:System.Windows.Media.Animation.Timeline> を各前方反復の最後に再生するかどうかを指定します。 次の例では、を `true`に <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティに設定します。その結果、0から100にアニメーション化され、その後、100から0にアニメーション化されます。 合計 10 秒間再生されます。  
  
 [!code-xaml[animation_ovws_snippet#TBAutoReverseExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbautoreverseexampleinline)]  
  
 <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A> 値を使用して <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> を指定し、その <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティを指定すると、1回の繰り返しが1つの前方反復で構成され、その後に1回の反復処理が続きます。  次の例では、前の例の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> を2の <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A> に設定します。 その結果、<xref:System.Windows.Media.Animation.DoubleAnimation> は20秒間再生されます。この場合、5秒間、後方5秒間、5秒間転送し、5秒間後方に進みます。  
  
 [!code-xaml[animation_ovws_snippet#TBAutoReverseRepeatExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbautoreverserepeatexampleinline)]  
  
 コンテナーのタイムラインに子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトがある場合は、コンテナーのタイムラインが行われたときに元に戻します。 その他の例については、[タイムラインを自動的に反転するかどうかを指定](how-to-specify-whether-a-timeline-automatically-reverses.md)する  
  
<a name="timelinebegin"></a>   
## <a name="the-begintime-property"></a>BeginTime プロパティ  
 <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> プロパティを使用すると、タイムラインをいつ開始するかを指定できます。  タイムラインの開始時間は、親タイムラインを基準とした相対値になります。 開始時間を 0 秒に設定すると、タイムラインはその親が開始されると直ちに開始されます。その他の値に設定すると、親タイムラインの再生開始時点と子タイムラインの再生時点の間のオフセットが作成されます。 たとえば、開始時間を 2 秒に設定すると、タイムラインは、その親が 2 秒の時点に到達すると再生を開始します。 既定では、すべてのタイムラインの開始時間は 0 秒です。 タイムラインの開始時刻を `null`に設定して、タイムラインが開始されないようにすることもできます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]では、 [X:Null マークアップ拡張機能](../../../desktop-wpf/xaml-services/xnull-markup-extension.md)を使用して null を指定します。  
  
 <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> 設定によってタイムラインが繰り返されるたびに開始時刻が適用されないことに注意してください。 <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> が10秒、<xref:System.Windows.Media.Animation.RepeatBehavior> が <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A>のアニメーションを作成する場合は、アニメーションが最初に再生される前に10秒の遅延が発生しますが、連続した繰り返しは実行されません。 ただし、アニメーションの親タイムラインの再開または繰り返しが行われた場合は、10 秒の遅延が発生します。  
  
 <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> プロパティは、タイムラインのずらすに便利です。 次の例では、2つの子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトを持つ <xref:System.Windows.Media.Animation.Storyboard> を作成します。 最初のアニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は5秒で、2番目のアニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は3秒です。 この例では、2番目の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> を5秒に設定して、最初の <xref:System.Windows.Media.Animation.DoubleAnimation> が終了した後に再生を開始します。  
  
 [!code-xaml[animation_ovws_snippet#TBBeginTimeExampleInline](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbbegintimeexampleinline)]  
  
<a name="fillbehaviorproperty"></a>   
## <a name="the-fillbehavior-property"></a>FillBehavior プロパティ  
 <xref:System.Windows.Media.Animation.Timeline> が合計アクティブ期間の最後に達すると、<xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは、その最後の値を停止するか保持するかを指定します。 <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> <xref:System.Windows.Media.Animation.FillBehavior.HoldEnd> のアニメーションでは、出力値が "保持" されます。アニメーション化するプロパティは、アニメーションの最後の値を保持します。 <xref:System.Windows.Media.Animation.FillBehavior.Stop> の値を指定すると、終了後にアニメーションが対象のプロパティに影響を与えなくなります。  
  
 次の例では、2つの子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトを持つ <xref:System.Windows.Media.Animation.Storyboard> を作成します。 どちらの <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトも、0 ~ 100 の <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> をアニメーション化します。 <xref:System.Windows.Shapes.Rectangle> 要素には、アニメーション化されていない <xref:System.Windows.FrameworkElement.Width%2A> 値 500 [デバイス非依存ピクセル] があります。  
  
- 最初の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは <xref:System.Windows.Media.Animation.FillBehavior.HoldEnd>(既定値) に設定されます。 その結果、<xref:System.Windows.Media.Animation.DoubleAnimation> が終了した後、四角形の幅は100のままになります。  
  
- 2番目の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは <xref:System.Windows.Media.Animation.FillBehavior.Stop>に設定されます。 その結果、<xref:System.Windows.Media.Animation.DoubleAnimation> の終了後、2番目の <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は500に戻ります。  
  
 [!code-xaml[animation_ovws_snippet#TBFillBehaviorExample](~/samples/snippets/csharp/VS_Snippets_Wpf/animation_ovws_snippet/CS/TimingBehaviorsExample1.xaml#tbfillbehaviorexample)]  
  
<a name="speedproperties"></a>   
## <a name="properties-that-control-the-speed-of-a-timeline"></a>タイムラインの速度を制御するプロパティ  
 <xref:System.Windows.Media.Animation.Timeline> クラスには、速度を指定するための3つのプロパティがあります。  
  
- <xref:System.Windows.Media.Animation.Timeline.SpeedRatio%2A> –その親を基準として、<xref:System.Windows.Media.Animation.Timeline>の時間が経過する速度を指定します。 1より大きい値を指定すると、<xref:System.Windows.Media.Animation.Timeline> とその子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの速度が上がります。0から1の間の値。 値1は、<xref:System.Windows.Media.Animation.Timeline> が親と同じレートで進行することを示します。 コンテナータイムラインの <xref:System.Windows.Media.Animation.Timeline.SpeedRatio%2A> 設定は、そのすべての子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトにも影響します。  
  
- <xref:System.Windows.Media.Animation.Timeline.AccelerationRatio%2A> –時間の短縮に費やされたタイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> の割合を指定します。 例については、「[方法: アニメーションを加速または減速させる](how-to-accelerate-or-decelerate-an-animation.md)」を参照してください。 
  
- <xref:System.Windows.Media.Animation.Timeline.DecelerationRatio%2A>-減速に費やされたタイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> の割合を指定します。 例については、「[方法: アニメーションを加速または減速させる](how-to-accelerate-or-decelerate-an-animation.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [アニメーションの概要](animation-overview.md)
- [アニメーションとタイミング システムの概要](animation-and-timing-system-overview.md)
- [タイミング イベントの概要](timing-events-overview.md)
- [方法のトピック](animation-and-timing-how-to-topics.md)
- [アニメーションのタイミング動作のサンプル](https://go.microsoft.com/fwlink/?LinkID=159970)
