---
title: '方法 : グラデーション ストップの位置または色をアニメーション化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- position [WPF], animating
- animation [WPF], position of GradientStop objects
- GradientStop objects [WPF], animating color of
- colors [WPF], animating
- animation [WPF], color of GradientStop objects
- GradientStop objects [WPF], animating position of
ms.assetid: 6f5b8b47-6c32-4b8e-98ee-fdf6515ec843
ms.openlocfilehash: aeae33f5f3c8016808988f58d61969e9b6f05039
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452845"
---
# <a name="how-to-animate-the-position-or-color-of-a-gradient-stop"></a>方法 : グラデーション ストップの位置または色をアニメーション化する
この例では、<xref:System.Windows.Media.GradientStop> オブジェクトの <xref:System.Windows.Media.GradientStop.Color%2A> と <xref:System.Windows.Media.GradientStop.Offset%2A> をアニメーション化する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Media.LinearGradientBrush>内の3つのグラデーションの分岐点をアニメーション化します。 この例では、3つのアニメーションを使用して、それぞれ異なるグラデーションの分岐点をアニメーション化します。  
  
- 最初のアニメーション (<xref:System.Windows.Media.Animation.DoubleAnimation>) では、最初のグラデーションの分岐点の <xref:System.Windows.Media.GradientStop.Offset%2A> を0.0 から1.0 に、次に0.0 に戻します。 その結果、グラデーションの最初の色が四角形の左側から右側に移動し、次に左側に戻ります。  
  
- 2番目のアニメーション (<xref:System.Windows.Media.Animation.ColorAnimation>) では、2番目のグラデーション分岐点の <xref:System.Windows.Media.GradientStop.Color%2A> を <xref:System.Windows.Media.Colors.Purple%2A> から <xref:System.Windows.Media.Colors.Yellow%2A>、<xref:System.Windows.Media.Colors.Purple%2A>に戻します。 その結果、グラデーションの中間色は紫色から黄色に変わり、紫色に戻ります。  
  
- 3番目のアニメーション (もう1つの <xref:System.Windows.Media.Animation.ColorAnimation>) は、3番目のグラデーション分岐点の不透明度を-1 で <xref:System.Windows.Media.GradientStop.Color%2A> し、次に戻るようにアニメーション化します。 その結果、グラデーションの3番目の色が消え、もう一度不透明になります。  
  
 [!code-csharp[BrushesIntroduction_snip#GraphicsMMGradientAnimationExamplesWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushesIntroduction_snip/CSharp/GradientStopAnimationExample.cs#graphicsmmgradientanimationexampleswholepage)]
 [!code-vb[BrushesIntroduction_snip#GraphicsMMGradientAnimationExamplesWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BrushesIntroduction_snip/visualbasic/gradientstopanimationexample.vb#graphicsmmgradientanimationexampleswholepage)]
 [!code-xaml[BrushesIntroduction_snip#GraphicsMMGradientAnimationExamplesWholePage](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushesIntroduction_snip/XAML/GradientStopAnimationExample.xaml#graphicsmmgradientanimationexampleswholepage)]  
  
 この例では <xref:System.Windows.Media.LinearGradientBrush>を使用しますが、プロセスは <xref:System.Windows.Media.RadialGradientBrush>内の <xref:System.Windows.Media.GradientStop> オブジェクトをアニメーション化する場合と同じです。  
  
 その他の例については、「[ブラシのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.GradientStop>
- [アニメーションの概要](animation-overview.md)
- [ストーリーボードの概要](storyboards-overview.md)
