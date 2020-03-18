---
title: '方法 : ストーリーボードを使ってプロパティをアニメーション化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], Storyboards
- Storyboards [WPF], animation
ms.assetid: f4a314e9-1da2-4367-85fc-1232487efa7a
ms.openlocfilehash: 6cfce9a5b070a23ed9ac03473888bf572b61393b
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559639"
---
# <a name="how-to-animate-a-property-by-using-a-storyboard"></a>方法 : ストーリーボードを使ってプロパティをアニメーション化する
この例では、<xref:System.Windows.Media.Animation.Storyboard> を使用してプロパティをアニメーション化する方法を示します。 <xref:System.Windows.Media.Animation.Storyboard>を使用してプロパティをアニメーション化するには、アニメーション化するプロパティごとにアニメーションを作成し、アニメーションを格納するための <xref:System.Windows.Media.Animation.Storyboard> も作成します。  
  
 プロパティの種類によって、使用するアニメーションの種類が決まります。 たとえば、<xref:System.Double> 値を受け取るプロパティをアニメーション化するには、<xref:System.Windows.Media.Animation.DoubleAnimation>を使用します。 <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> および <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> の添付プロパティでは、アニメーションを適用するオブジェクトとプロパティを指定します。  
  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]でストーリーボードを開始するには、<xref:System.Windows.Media.Animation.BeginStoryboard> アクションと <xref:System.Windows.EventTrigger>を使用します。 <xref:System.Windows.EventTrigger.RoutedEvent%2A> プロパティによって指定されたイベントが発生すると、<xref:System.Windows.EventTrigger> は <xref:System.Windows.Media.Animation.BeginStoryboard> アクションを開始します。 <xref:System.Windows.Media.Animation.BeginStoryboard> アクションは、<xref:System.Windows.Media.Animation.Storyboard>を開始します。  
  
 次の例では <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用して、2つの <xref:System.Windows.Controls.Button> コントロールをアニメーション化します。 最初のボタンのサイズを変更するには、その <xref:System.Windows.FrameworkElement.Width%2A> をアニメーション化します。 2番目のボタンの色を変更するには、<xref:System.Windows.Media.SolidColorBrush> の [<xref:System.Windows.Media.SolidColorBrush.Color%2A>] プロパティを使用して、アニメーション化されているボタンの <xref:System.Windows.Controls.Control.Background%2A> を設定します。  
  
## <a name="example"></a>使用例  
 [!code-xaml[AnimatePropertyStoryboards#1](~/samples/snippets/xaml/VS_Snippets_Wpf/AnimatePropertyStoryboards/XAML/StoryboardExample.xaml#1)]  
  
> [!NOTE]
> アニメーションは、<xref:System.Windows.Controls.Control> や <xref:System.Windows.Controls.Panel>などの <xref:System.Windows.FrameworkElement> オブジェクトと、<xref:System.Windows.Media.Brush> や <xref:System.Windows.Media.Transform>などの <xref:System.Windows.Freezable> オブジェクトの両方を対象にすることができますが、<xref:System.Windows.FrameworkElement.Name%2A> プロパティを持つのはフレームワーク要素だけです。 名前をフリーズ可能オブジェクトに割り当てて、アニメーションの対象にできるようにするには、前の例で示したように [x:Name ディレクティブ](../../../desktop-wpf/xaml-services/xname-directive.md)を使用します。  
  
 コードを使用する場合は、<xref:System.Windows.FrameworkElement> の <xref:System.Windows.NameScope> を作成し、その <xref:System.Windows.FrameworkElement>でアニメーション化するためにオブジェクトの名前を登録する必要があります。 コードでアニメーションを開始するには、<xref:System.Windows.EventTrigger>で <xref:System.Windows.Media.Animation.BeginStoryboard> アクションを使用します。 必要に応じて、イベントハンドラーと <xref:System.Windows.Media.Animation.Storyboard>の <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用できます。 <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> メソッドを使用する方法の例を次に示します。  
  
 [!code-csharp[AnimatePropertyStoryboards#11](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimatePropertyStoryboards/CSharp/StoryboardExample.cs#11)]
 [!code-vb[AnimatePropertyStoryboards#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/AnimatePropertyStoryboards/VisualBasic/StoryboardExample.vb#11)]  
  
 アニメーションとストーリー ボードの詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください。  
  
 コードを使用する場合は、プロパティをアニメーション化するために <xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用するだけではありません。 使用例を含む詳細については、「[ストーリーボードを使用せずにプロパティをアニメーション化する](how-to-animate-a-property-without-using-a-storyboard.md)」と「[AnimationClock を使用してプロパティをアニメーション化する](how-to-animate-a-property-by-using-an-animationclock.md)」を参照してください。
