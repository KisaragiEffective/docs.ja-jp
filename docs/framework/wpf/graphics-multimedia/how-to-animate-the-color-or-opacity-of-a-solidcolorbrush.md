---
title: '方法 : SolidColorBrush の色または不透明度をアニメーション化する'
ms.date: 03/30/2017
helpviewer_keywords:
- SolidColorBrush [WPF], animating color of
- colors [WPF], animating
- opacity [WPF], animating
- animation [WPF], color of SolidColorBrush
- animation [WPF], opacity of SolidColorBrush
- SolidColorBrush [WPF], animating opacity of
ms.assetid: d9154354-843f-4713-bad1-35bb0ba6eaeb
ms.openlocfilehash: 08b85935e0cb1ababd1fb63b9d02518ea3fcfa17
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452884"
---
# <a name="how-to-animate-the-color-or-opacity-of-a-solidcolorbrush"></a>方法 : SolidColorBrush の色または不透明度をアニメーション化する
この例では、<xref:System.Windows.Media.SolidColorBrush>の <xref:System.Windows.Media.SolidColorBrush.Color%2A> と <xref:System.Windows.Media.Brush.Opacity%2A> をアニメーション化する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、3つのアニメーションを使用して、<xref:System.Windows.Media.SolidColorBrush>の <xref:System.Windows.Media.SolidColorBrush.Color%2A> と <xref:System.Windows.Media.Brush.Opacity%2A> をアニメーション化します。  
  
- 最初のアニメーション (<xref:System.Windows.Media.Animation.ColorAnimation>) は、マウスが四角形に入ったときにブラシの色を <xref:System.Windows.Media.Colors.Gray%2A> に変更します。  
  
- 次のアニメーション (もう1つの <xref:System.Windows.Media.Animation.ColorAnimation>) では、マウスポインターを四角形から離したときに、ブラシの色が <xref:System.Windows.Media.Colors.Orange%2A> に変更されます。  
  
- <xref:System.Windows.Media.Animation.DoubleAnimation>の最後のアニメーションでは、マウスの左ボタンが押されたときにブラシの不透明度が0.0 に変更されます。  
  
 [!code-csharp[brushanimations_snip#SolidColorBrushAnimationExample](~/samples/snippets/csharp/VS_Snippets_Wpf/brushanimations_snip/CSharp/SolidColorBrushExample.cs#solidcolorbrushanimationexample)]  
  
 さまざまな種類のブラシをアニメーション化する方法を示す完全なサンプルについては、「[ブラシのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes)」を参照してください。 アニメーションの詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください。  
  
 他のアニメーションの例との一貫性を保つために、この例のコードバージョンでは、<xref:System.Windows.Media.Animation.Storyboard> オブジェクトを使用してアニメーションを適用します。 ただし、コードで1つのアニメーションを適用する場合は、<xref:System.Windows.Media.Animation.Storyboard>を使用する代わりに、<xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを使用する方が簡単です。 例については、「[ストーリーボードを使用せずにプロパティをアニメーション化する](how-to-animate-a-property-without-using-a-storyboard.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [アニメーションの概要](animation-overview.md)
- [ストーリーボードの概要](storyboards-overview.md)
- [ブラシのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes)
