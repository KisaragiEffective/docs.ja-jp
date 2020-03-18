---
title: '方法 : 要素のスピンを設定する'
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [WPF], spinning elements
- spinning elements [WPF]
ms.assetid: 1f011976-8b07-4c31-9faf-019e0ddaa24c
ms.openlocfilehash: 2e72389a11e48629c2763fcbd9f7b1945ffff5dd
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452793"
---
# <a name="how-to-make-an-element-spin-in-place"></a>方法 : 要素のスピンを設定する
この例では、<xref:System.Windows.Media.RotateTransform> と <xref:System.Windows.Media.Animation.DoubleAnimation>を使用して要素をスピンさせる方法を示します。  
  
 次の例では、要素の <xref:System.Windows.UIElement.RenderTransform%2A> プロパティに <xref:System.Windows.Media.RotateTransform> を適用します。 この例では、<xref:System.Windows.Media.Animation.DoubleAnimation> を使用して <xref:System.Windows.Media.RotateTransform>の <xref:System.Windows.Media.RotateTransform.Angle%2A> をアニメーション化します。 要素のスピンを可能にするために、この例では要素の <xref:System.Windows.UIElement.RenderTransformOrigin%2A> プロパティをポイント (0.5, 0.5) に設定しています。  
  
## <a name="example"></a>例  
 [!code-xaml[transformanimations_snip#11](~/samples/snippets/xaml/VS_Snippets_Wpf/transformanimations_snip/XAML/RotateAboutCenterExample.xaml#11)]  
  
 より多くの変換例を含む完全なサンプルについては、「 [2-D 変換のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/2DTransforms)」を参照してください。  
  
## <a name="see-also"></a>参照

- [アニメーションの概要](animation-overview.md)
- [変換の概要](transforms-overview.md)
