---
title: From To By アニメーションの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], From/to/by
- From/to/by animation
ms.assetid: 516fce0a-e7f8-49b8-b018-53b3d409a8a3
ms.openlocfilehash: 661c035f55ba1fb550726d75921cd01a72b2eecc
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449001"
---
# <a name="fromtoby-animations-overview"></a>From/To/By アニメーションの概要
このトピックでは、From/To/By アニメーションを使って依存関係プロパティをアニメーション化する方法を説明します。 From/To/By アニメーションでは、2 つの値の間の遷移が作成されます。  
  
<a name="prereq"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックを理解するには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーション機能について理解しておく必要があります。 アニメーション機能の概要については、「[アニメーションの概要](animation-overview.md)」を参照してください。  
  
<a name="whatisanimation"></a>   
## <a name="what-is-a-fromtoby-animation"></a>From/To/By アニメーションとは  
 From/To/By アニメーションは、開始値と終了値の間の遷移を作成する <xref:System.Windows.Media.Animation.AnimationTimeline> の一種です。 遷移が完了するまでにかかる時間は、そのアニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> によって決まります。  
  
 マークアップとコードの <xref:System.Windows.Media.Animation.Storyboard> を使用するか、コードで <xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを使用することにより、プロパティに From/To/By アニメーションを適用できます。 また、From/To/By アニメーションを使用して <xref:System.Windows.Media.Animation.AnimationClock> を作成し、1つまたは複数のプロパティに適用することもできます。 アニメーションを適用するためのさまざまなメソッドの詳細については、「[プロパティ アニメーションの手法の概要](property-animation-techniques-overview.md)」を参照してください。  
  
 From/To/By アニメーションは、2 つのターゲット値以外の値を持つことはできません。 3 つ以上のターゲット値を持つアニメーションを必要とする場合は、キー フレーム アニメーションを使います。 キーフレームアニメーションについては、「[キーフレームアニメーションの概要](key-frame-animations-overview.md)」を参照してください。  
  
<a name="animation_types"></a>   
## <a name="fromtoby-animation-types"></a>From/To/By アニメーションの種類  
 アニメーションはプロパティ値を生成するため、プロパティの型ごとに異なるアニメーションの種類があります。 要素の <xref:System.Windows.FrameworkElement.Width%2A> プロパティなど、<xref:System.Double>を受け取るプロパティをアニメーション化するには <xref:System.Double> 値を生成するアニメーションを使用します。 <xref:System.Windows.Point>を受け取るプロパティをアニメーション化するには、<xref:System.Windows.Point> 値を生成するアニメーションを使用します。  
  
 From/To/By アニメーションクラスは <xref:System.Windows.Media.Animation> 名前空間に属し、次の名前付け規則を使用します。  
  
 *\<の種類 >* `Animation`  
  
 ここで、 *\<Type>* は、クラスがアニメーション化する値の型です。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、次の From/To/By アニメーション クラスが用意されています。  
  
|プロパティの種類|対応する From/To/By アニメーションのクラス|  
|-------------------|------------------------------------------------|  
|<xref:System.Byte>|<xref:System.Windows.Media.Animation.ByteAnimation>|  
|<xref:System.Windows.Media.Color>|<xref:System.Windows.Media.Animation.ColorAnimation>|  
|<xref:System.Decimal>|<xref:System.Windows.Media.Animation.DecimalAnimation>|  
|<xref:System.Double>|<xref:System.Windows.Media.Animation.DoubleAnimation>|  
|<xref:System.Int16>|<xref:System.Windows.Media.Animation.Int16Animation>|  
|<xref:System.Int32>|<xref:System.Windows.Media.Animation.Int32Animation>|  
|<xref:System.Int64>|<xref:System.Windows.Media.Animation.Int64Animation>|  
|<xref:System.Windows.Point>|<xref:System.Windows.Media.Animation.PointAnimation>|  
|<xref:System.Windows.Media.Media3D.Quaternion>|<xref:System.Windows.Media.Animation.QuaternionAnimation>|  
|<xref:System.Windows.Rect>|<xref:System.Windows.Media.Animation.RectAnimation>|  
|<xref:System.Windows.Media.Media3D.Rotation3D>|<xref:System.Windows.Media.Animation.Rotation3DAnimation>|  
|<xref:System.Single>|<xref:System.Windows.Media.Animation.SingleAnimation>|  
|<xref:System.Windows.Size>|<xref:System.Windows.Media.Animation.SizeAnimation>|  
|<xref:System.Windows.Thickness>|<xref:System.Windows.Media.Animation.ThicknessAnimation>|  
|<xref:System.Windows.Media.Media3D.Vector3D>|<xref:System.Windows.Media.Animation.Vector3DAnimation>|  
|<xref:System.Windows.Vector>|<xref:System.Windows.Media.Animation.VectorAnimation>|  
  
<a name="anim_values"></a>   
## <a name="target-values"></a>ターゲット値  
 From/To/By アニメーションでは、2 つのターゲット値の間の遷移が作成されます。 通常は、開始値 (<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを使用して設定) と終了値 (<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを使用して設定) を指定します。 ただし、開始値、終了値、またはオフセット値のみを指定することもできます。 これらの場合、アニメーションは不足しているターゲット値をアニメーション化するプロパティから取得します。 次の一覧では、アニメーションのターゲット値を指定する方法について説明します。  
  
- **開始値**  
  
     アニメーションの開始値を明示的に指定する場合は、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを使用します。 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティは、単独で使用することも、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> または <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティと共に使用することもできます。 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティのみを指定すると、アニメーションはその値からアニメーション化されたプロパティの基本値に遷移します。  
  
- **終了値**  
  
     アニメーションの終了値を指定するには、その <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを使用します。 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを単独で使用する場合、アニメーションは、アニメーション化されているプロパティ、または同じプロパティに適用されている別のアニメーションの出力から開始値を取得します。 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティと共に <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを使用して、アニメーションの開始値と終了値を明示的に指定できます。  
  
- **オフセット値**  
  
     <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティを使用すると、アニメーションの開始値または終了値の明示的な値ではなくオフセットを指定できます。 アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティは、アニメーションがその期間にわたって値を変更する量を指定します。 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティは、単独で、または <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティで使用できます。 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティのみを指定すると、アニメーションは、プロパティの基本値または別のアニメーションの出力にオフセット値を追加します。  
  
<a name="examples"></a>   
## <a name="using-fromtoby-values"></a>From/To/By 値の使用  
 次のセクションでは、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>、および <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> の各プロパティを一緒に使用する方法、または個別に使用する方法について説明します。  
  
 このセクションの例では、<xref:System.Windows.Media.Animation.DoubleAnimation>を使用します。これは、From/To/By アニメーションの一種であり、10デバイス非依存ピクセルの高さが10で、デバイスに依存しないピクセルが100である <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> プロパティをアニメーション化します。  
  
 各例では <xref:System.Windows.Media.Animation.DoubleAnimation>を使用しますが、from/To/By アニメーションの From、To、および By の各プロパティは同じように動作します。 これらの各例では <xref:System.Windows.Media.Animation.Storyboard>を使用しますが、From/To/By アニメーションを他の方法で使用することもできます。 詳細については、「[プロパティアニメーションの手法の概要](property-animation-techniques-overview.md)」を参照してください。  
  
### <a name="fromto"></a>送信元/送信先  
 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> と <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> 値を一緒に設定すると、アニメーションは <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティによって指定された値から <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティによって指定された値に進みます。  
  
 次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを50に、その <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティを300に設定しています。 その結果、<xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は50から300にアニメーション化されます。  
  
 [!code-csharp[basicvalues_snip#FromToAnimationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/basicvalues_snip/CSharp/AnimationTargetValuesExample.cs#fromtoanimationinline)]
 [!code-vb[basicvalues_snip#FromToAnimationInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/basicvalues_snip/VisualBasic/AnimationTargetValuesExample.vb#fromtoanimationinline)]  
  
### <a name="to"></a>目的  
 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティのみを設定すると、アニメーションは、アニメーション化されたプロパティの基本値、または同じプロパティに以前に適用されていた作成中のアニメーションの出力から、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティで指定された値に進みます。  
  
 ("アニメーションの作成" とは、前に <xref:System.Windows.Media.Animation.HandoffBehavior.Compose> ハンドオフ動作を使用して現在のアニメーションが適用されていた場合に有効な、同じプロパティに以前に適用された <xref:System.Windows.Media.Animation.ClockState.Active> または <xref:System.Windows.Media.Animation.ClockState.Filling> のアニメーションを指します)。  
  
 次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティのみを300に設定します。 開始値が指定されていないため、<xref:System.Windows.Media.Animation.DoubleAnimation> は <xref:System.Windows.FrameworkElement.Width%2A> プロパティの基本値 (100) を開始値として使用します。 <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は、100からアニメーションのターゲット値300にアニメーション化されます。  
  
 [!code-csharp[basicvalues_snip#ToAnimationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/basicvalues_snip/CSharp/AnimationTargetValuesExample.cs#toanimationinline)]
 [!code-vb[basicvalues_snip#ToAnimationInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/basicvalues_snip/VisualBasic/AnimationTargetValuesExample.vb#toanimationinline)]  
  
### <a name="by"></a>別  
 アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティのみを設定すると、アニメーションは、アニメーション化されているプロパティの基本値、または作成中のアニメーションの出力から、その値と <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティで指定された値の合計まで進行します。  
  
 次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティのみを300に設定します。 この例では開始値が指定されていないため、<xref:System.Windows.Media.Animation.DoubleAnimation> は <xref:System.Windows.FrameworkElement.Width%2A> プロパティ100の基本値を開始値として使用します。 終了値は、アニメーション300の <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 値を開始値 100: 400 に追加することによって決定されます。 その結果、<xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は100から400にアニメーション化されます。  
  
 [!code-csharp[basicvalues_snip#ByAnimationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/basicvalues_snip/CSharp/AnimationTargetValuesExample.cs#byanimationinline)]
 [!code-vb[basicvalues_snip#ByAnimationInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/basicvalues_snip/VisualBasic/AnimationTargetValuesExample.vb#byanimationinline)]  
  
### <a name="fromby"></a>From/By  
 アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> と <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> のプロパティを設定すると、アニメーションは <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティによって指定された値から、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> および <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティの合計で指定された値に進みます。  
  
 次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティを50に、その <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティを300に設定しています。 終了値は、アニメーション300の <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 値を開始値 50: 350 に追加することによって決定されます。 その結果、<xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は50から350にアニメーション化されます。  
  
 [!code-csharp[basicvalues_snip#FromByAnimationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/basicvalues_snip/CSharp/AnimationTargetValuesExample.cs#frombyanimationinline)]
 [!code-vb[basicvalues_snip#FromByAnimationInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/basicvalues_snip/VisualBasic/AnimationTargetValuesExample.vb#frombyanimationinline)]  
  
### <a name="from"></a>ソース  
 アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> の値のみを指定すると、アニメーションは、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティで指定された値から、アニメーション化されるプロパティの基本値、または作成中のアニメーションの出力に進行します。  
  
 次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティのみを50に設定します。 終了値が指定されていないため、<xref:System.Windows.Media.Animation.DoubleAnimation> は <xref:System.Windows.FrameworkElement.Width%2A> プロパティ100の基本値を終了値として使用します。 <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は、50から、<xref:System.Windows.FrameworkElement.Width%2A> プロパティ100の基本値にアニメーション化されます。  
  
 [!code-csharp[basicvalues_snip#FromAnimationInline](~/samples/snippets/csharp/VS_Snippets_Wpf/basicvalues_snip/CSharp/AnimationTargetValuesExample.cs#fromanimationinline)]
 [!code-vb[basicvalues_snip#FromAnimationInline](~/samples/snippets/visualbasic/VS_Snippets_Wpf/basicvalues_snip/VisualBasic/AnimationTargetValuesExample.vb#fromanimationinline)]  
  
### <a name="toby"></a>To/By  
 アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> と <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> の両方のプロパティを設定した場合、<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティは無視されます。  
  
<a name="otheranimationtypes"></a>   
## <a name="other-animation-types"></a>他のアニメーションの種類  
 From/To/By アニメーションは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が提供するアニメーションの唯一の種類ではありません。キーフレームアニメーションとパスアニメーションも用意されています。  
  
- キー フレーム アニメーションは、キー フレームを使って記述されている任意の数の目標値に沿ってアニメーション化します。 詳細については、「[キーフレームアニメーションの概要](key-frame-animations-overview.md)」を参照してください。  
  
- パスアニメーションは、<xref:System.Windows.Media.PathGeometry>から出力値を生成します。 詳細については、「[パスアニメーションの概要](path-animations-overview.md)」を参照してください。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、独自のカスタム アニメーションの種類を作成することもできます。 詳細については、「[カスタムアニメーションの概要](custom-animations-overview.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.Animation.Timeline>
- <xref:System.Windows.Media.Animation.Storyboard>
- [アニメーションの概要](animation-overview.md)
- [ストーリーボードの概要](storyboards-overview.md)
- [キー フレーム アニメーションの概要](key-frame-animations-overview.md)
- [パス アニメーションの概要](path-animations-overview.md)
- [カスタム アニメーションの概要](custom-animations-overview.md)
- [アニメーションのターゲット値 (From、To、および By) のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/TargetValues)
