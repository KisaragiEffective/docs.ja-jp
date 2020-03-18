---
title: '方法 : "From"、"To"、および "By" を使用してアニメーションを制御する'
ms.date: 03/30/2017
helpviewer_keywords:
- animation [WPF], From/to/by
- basic animation [WPF]
- animation [WPF], basic animation
- From/to/by animation
ms.assetid: 59afba57-6fc1-44c8-987e-8a5f4142adad
ms.openlocfilehash: b06df97dc57c58a01f30be2d70bfeebf104521ad
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77451786"
---
# <a name="how-to-control-an-animation-using-from-to-and-by"></a>方法 : "From"、"To"、および "By" を使用してアニメーションを制御する
"From/To/By" または "基本アニメーション" は、2つのターゲット値の間の遷移を作成します (さまざまな種類のアニメーションの概要については、「[アニメーションの概要](animation-overview.md)」を参照してください)。 基本的なアニメーションのターゲット値を設定するには、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>、および <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> の各プロパティを使用します。  次の表は、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> の各プロパティを組み合わせて使用する方法と、アニメーションのターゲット値を決定するために個別に使用する方法をまとめたものです。  
  
|指定するプロパティ|行われる動作|  
|--------------------------|------------------------|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>|アニメーションは、前のアニメーションがどのように構成されているかによって、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティで指定された値から、アニメーション化するプロパティの基本値、または前のアニメーションの出力値に向かって進行します。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> および <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>|アニメーションは、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティによって指定された値から、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティによって指定された値に進みます。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> および <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>|アニメーションは、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティによって指定された値から、<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティと <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティの合計によって指定された値に進みます。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>|アニメーションは、アニメーション化されたプロパティの基本値または前のアニメーションの出力値から、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティによって指定された値に進行します。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>|アニメーションは、アニメーション化されているプロパティの基本値または前のアニメーションの出力値から、その値と <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティで指定された値の合計まで進行します。|  
  
> [!NOTE]
> 同じアニメーションで、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> プロパティと <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> プロパティの両方を設定しないでください。  
  
 他の補間方式を使用したり、3 つ以上のターゲット値の間でアニメーション化したりするには、キー フレーム アニメーションを使用します。 詳細については[、「キーフレームアニメーションの概要](key-frame-animations-overview.md)」を参照してください。  
  
 1つのプロパティに複数のアニメーションを適用する方法の詳細については、「[キーフレームアニメーションの概要](key-frame-animations-overview.md)」を参照してください。  
  
 次の例は、アニメーションの <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>、および <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> プロパティの設定によるさまざまな効果を示しています。  
  
## <a name="example"></a>例  
 [!code-xaml[BasicAnimations_snippet#AnimationTargetValuesWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/BasicAnimations_snippet/CS/AnimationTargetValuesExample.xaml#animationtargetvalueswholepage)]  
  
## <a name="see-also"></a>参照

- [アニメーションの概要](animation-overview.md)
- [キー フレーム アニメーションの概要](key-frame-animations-overview.md)
- [アニメーションのターゲット値 (From、To、および By) のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/TargetValues)
