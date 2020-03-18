---
title: '方法 : パスに沿ってオブジェクトをアニメーション化する (行列アニメーション)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], objects along paths (matrix animation)
- matrix animation [WPF]
ms.assetid: 7000e697-1414-468c-b915-cf66062fc49e
ms.openlocfilehash: 7dfc233fe60e1cea293edc44a2bba79dc6962f7c
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452910"
---
# <a name="how-to-animate-an-object-along-a-path-matrix-animation"></a>方法 : パスに沿ってオブジェクトをアニメーション化する (行列アニメーション)
この例では、<xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> クラスを使用して、<xref:System.Windows.Media.PathGeometry>によって定義されたパスに沿ってオブジェクトをアニメーション化する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、以下の処理を実行し、パスに沿ってオブジェクトをアニメーション化します。  
  
- オブジェクトを移動するために、オブジェクトに <xref:System.Windows.Media.MatrixTransform> を適用します。  
  
- <xref:System.Windows.Media.PathGeometry>を使用してパスを定義します。  
  
- <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> を作成し、それを使用して <xref:System.Windows.Media.MatrixTransform>の <xref:System.Windows.Media.Matrix> プロパティをアニメーション化します。 <xref:System.Windows.Media.Animation.MatrixAnimationUsingPath> は <xref:System.Windows.Media.PathGeometry> を受け取り、それを使用して <xref:System.Windows.Media.Matrix> 値を生成します。  
  
 [!code-xaml[PathAnimationGallery_snippet#MatrixAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/matrixanimationusingpathexample.xaml#matrixanimationusingpathwholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/MatrixAnimationUsingPathExample.cs#matrixanimationusingpathwholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/MatrixAnimationUsingPathExample.vb#matrixanimationusingpathwholepage)]  
  
 完全なサンプルについては、「[パスアニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)」を参照してください。 ジオメトリックパスの詳細については、「 [Geometry の概要](geometry-overview.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [アニメーションの概要](animation-overview.md)
- [パス アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)
- [パス アニメーションに関する「方法」トピック](path-animation-how-to-topics.md)
