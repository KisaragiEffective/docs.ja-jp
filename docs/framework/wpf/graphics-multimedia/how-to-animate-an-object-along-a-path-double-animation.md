---
title: '方法 : パスに沿ってオブジェクトをアニメーション化する (ダブル アニメーション)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], objects along paths (double animation)
- double animation [WPF]
ms.assetid: 5a3c4a99-f303-42ad-a52a-e4794bb1798e
ms.openlocfilehash: 084caac26fd68b6914ec3858652ec44557a0dbd7
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452858"
---
# <a name="how-to-animate-an-object-along-a-path-double-animation"></a>方法 : パスに沿ってオブジェクトをアニメーション化する (ダブル アニメーション)
この例では、<xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> クラスを使用して、<xref:System.Windows.Media.PathGeometry>によって定義されたパスに沿ってオブジェクトを移動する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、2つの <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> オブジェクトを使用して、幾何学的なパスに沿って四角形を移動します。  
  
- 最初の <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> は、四角形に適用されている <xref:System.Windows.Media.TranslateTransform> の <xref:System.Windows.Media.TranslateTransform.X%2A> をアニメーション化します。 これにより、四角形がパスに沿って水平に移動します。  
  
- 2番目の <xref:System.Windows.Media.Animation.DoubleAnimationUsingPath> は、四角形に適用されている <xref:System.Windows.Media.TranslateTransform> の <xref:System.Windows.Media.TranslateTransform.Y%2A> をアニメーション化します。 これにより、四角形がパスに沿って垂直に移動します。  
  
 [!code-xaml[PathAnimationGallery_snippet#DoubleAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/doubleanimationusingpathexample.xaml#doubleanimationusingpathwholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#DoubleAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/DoubleAnimationUsingPathExample.cs#doubleanimationusingpathwholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#DoubleAnimationUsingPathWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/DoubleAnimationUsingPathExample.vb#doubleanimationusingpathwholepage)]  
  
 完全なサンプルについては、「[パスアニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)」を参照してください。  
  
 ジオメトリックパスを使用してオブジェクトを移動するもう1つの方法は、<xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> オブジェクトを使用することです。 例については、「[パスに沿ってオブジェクトをアニメーション化する (行列アニメーション)](how-to-animate-an-object-along-a-path-matrix-animation.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [アニメーションの概要](animation-overview.md)
- [パス アニメーションに関する「方法」トピック](path-animation-how-to-topics.md)
