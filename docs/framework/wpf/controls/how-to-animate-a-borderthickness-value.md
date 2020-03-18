---
title: '方法 : BorderThickness 値をアニメーション化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- border thickness [WPF], animating changes to
- animation [WPF], changes to border thickness
ms.assetid: fd021978-f74b-4e7b-a7f7-3987dcad9e0f
ms.openlocfilehash: 4533ce6f2a1fe7243267ee8d638e2ad0a4f9cf3a
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124664"
---
# <a name="how-to-animate-a-borderthickness-value"></a>方法 : BorderThickness 値をアニメーション化する
この例では、<xref:System.Windows.Media.Animation.ThicknessAnimation> クラスを使用して、境界線の太さに対する変更をアニメーション化する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Media.Animation.ThicknessAnimation>を使用して、境界線の太さをアニメーション化します。 この例では、<xref:System.Windows.Controls.Border>の <xref:System.Windows.Controls.Border.BorderThickness%2A> プロパティを使用します。  
  
 [!code-csharp[BasicAnimations_snip#ThicknessAnimationWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/BasicAnimations_snip/CSharp/ThicknessAnimationExample.cs#thicknessanimationwholepage)]
 [!code-vb[BasicAnimations_snip#ThicknessAnimationWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BasicAnimations_snip/VisualBasic/ThicknessAnimationExample.vb#thicknessanimationwholepage)]  
  
 完全なサンプルについては、「アニメーションのサンプル[ギャラリー](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/AnimationExamples)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.Animation.ThicknessAnimation>
- <xref:System.Windows.Controls.Border.BorderThickness%2A>
- <xref:System.Windows.Controls.Border>
- [アニメーションの概要](../graphics-multimedia/animation-overview.md)
- [アニメーションとタイミングに関する「方法」トピック](../graphics-multimedia/animation-and-timing-how-to-topics.md)
- [キー フレームを使用して境界線の太さをアニメーション化する](../graphics-multimedia/how-to-animate-the-thickness-of-a-border-by-using-key-frames.md)
