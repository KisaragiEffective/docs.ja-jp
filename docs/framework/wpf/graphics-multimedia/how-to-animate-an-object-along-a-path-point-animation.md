---
title: '方法 : パスに沿ってオブジェクトをアニメーション化する (ポイント アニメーション)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], objects along paths (point animation)
- point animation [WPF]
ms.assetid: 1fa3f817-35bc-41a1-b366-f5a20b70da0c
ms.openlocfilehash: eff0c24a9369ffaa0cfca1cc46af4eff39f58a38
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452897"
---
# <a name="how-to-animate-an-object-along-a-path-point-animation"></a>方法 : パスに沿ってオブジェクトをアニメーション化する (ポイント アニメーション)
この例では、<xref:System.Windows.Media.Animation.PointAnimationUsingPath> オブジェクトを使用して、曲線パスに沿って <xref:System.Windows.Point> をアニメーション化する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Media.PathGeometry>によって定義されたパスに沿って <xref:System.Windows.Media.EllipseGeometry> を移動します。 楕円のジオメトリの <xref:System.Windows.Media.EllipseGeometry.Center%2A> プロパティは、<xref:System.Windows.Point> 値を受け取り、その位置を指定します。楕円のジオメトリを移動するには、その <xref:System.Windows.Media.EllipseGeometry.Center%2A> プロパティをアニメーション化します。 この例では、<xref:System.Windows.Media.Animation.PointAnimationUsingPath> を使用して、<xref:System.Windows.Media.EllipseGeometry> オブジェクトの <xref:System.Windows.Media.EllipseGeometry.Center%2A> プロパティをアニメーション化します。  
  
 [!code-xaml[PathAnimationGallery_snippet#PointAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/pointanimationusingpathexample.xaml#pointanimationusingpathwholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#PointAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/PointAnimationUsingPathExample.cs#pointanimationusingpathwholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#PointAnimationUsingPathWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/PointAnimationUsingPathExample.vb#pointanimationusingpathwholepage)]  
  
 完全なサンプルについては、「[パスアニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)」を参照してください。  
  
 前のサンプルのコードバージョンでは、1つのアニメーションしか適用されていない場合でも、<xref:System.Windows.Media.Animation.Storyboard> を使用して <xref:System.Windows.Media.EllipseGeometry>をアニメーション化していました。 複数のアニメーションを適用するには、多くの場合、<xref:System.Windows.Media.Animation.Storyboard> を使用するのが最も簡単な方法です。これらのアニメーションは同じ <xref:System.Windows.Media.Animation.Storyboard>で制御できるためです。 ただし、コードを使用するときに1つのアニメーションをプロパティに適用する方が簡単な方法は、<xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A> メソッドを使用することです。 例については、「[ストーリーボードを使用せずにプロパティをアニメーション化する](how-to-animate-a-property-without-using-a-storyboard.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [パス アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)
- [アニメーションの概要](animation-overview.md)
- [パス アニメーションに関する「方法」トピック](path-animation-how-to-topics.md)
