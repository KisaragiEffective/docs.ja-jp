---
title: '方法 : 線形グラデーションを使用して領域を塗りつぶす'
ms.date: 03/30/2017
helpviewer_keywords:
- linear gradients [WPF], painting with
- brushes [WPF], painting with linear gradients
- painting [WPF], with linear gradients
ms.assetid: 00e0cd04-48c0-4ec5-850e-d321beb37a34
ms.openlocfilehash: 76c491632911c48db34d932ba3895278591378a5
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452767"
---
# <a name="how-to-paint-an-area-with-a-linear-gradient"></a>方法 : 線形グラデーションを使用して領域を塗りつぶす
この例では、<xref:System.Windows.Media.LinearGradientBrush> クラスを使用して、線状グラデーションで領域を塗りつぶす方法を示します。 次の例では、<xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.Shapes.Shape.Fill%2A> が、黄色から赤、青から緑に変化する対角線の線形グラデーションで塗りつぶされています。  
  
## <a name="example"></a>例  
 [!code-xaml[GradientBrushExamples_snip#DiagonalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#diagonalgradient1xaml)]  
  
 [!code-csharp[GradientBrushExamples_snip#DiagonalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#diagonalgradient1csharp)]  
  
 次の図は、前の例で作成したグラデーションを示しています。  
  
 ![対角線方向の線形グラデーション](./media/graphicsmm-diagonallgb.jpg "graphicsmm_DiagonalLGB")  
  
 水平方向の線状グラデーションを作成するには、<xref:System.Windows.Media.LinearGradientBrush> の <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> と <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> を (0, 0.5) と (1, 0.5) に変更します。 次の例では、<xref:System.Windows.Shapes.Rectangle> は水平方向の線形グラデーションで塗りつぶされています。  
  
 [!code-xaml[GradientBrushExamples_snip#HorizontalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#horizontalgradient1xaml)]  
  
 [!code-csharp[GradientBrushExamples_snip#HorizontalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#horizontalgradient1csharp)]  
  
 次の図は、前の例で作成したグラデーションを示しています。  
  
 ![水平方向の線形グラデーション](./media/graphicsmm-horizontallgb.jpg "graphicsmm_HorizontalLGB")  
  
 垂直方向の線状グラデーションを作成するには、<xref:System.Windows.Media.LinearGradientBrush> の <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> と <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> を (0.5, 0) と (0.5, 1) に変更します。 次の例では、<xref:System.Windows.Shapes.Rectangle> は垂直方向の線形グラデーションで塗りつぶされています。  
  
 [!code-xaml[GradientBrushExamples_snip#VerticalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#verticalgradient1xaml)]  
  
 [!code-csharp[GradientBrushExamples_snip#VerticalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#verticalgradient1csharp)]  
  
 次の図は、前の例で作成したグラデーションを示しています。  
  
 ![垂直方向の線状グラデーション](./media/graphicsmm-verticallgb.jpg "graphicsmm_VerticalLGB")  
  
> [!NOTE]
> このトピックの例では、開始点と終了点を設定するための既定の座標系を使用します。 既定の座標系は境界ボックスに対して相対的です。0は境界ボックスの0% を示し、1は境界ボックスの100% を示します。 この座標系を変更するには、[<xref:System.Windows.Media.GradientBrush.MappingMode%2A>] プロパティを <xref:System.Windows.Media.BrushMappingMode.Absolute?displayProperty=nameWithType>の値に設定します。 絶対座標系は、境界ボックスに相対しません。 値は、ローカル空間に直接変換されます。  
  
 その他の例については、「[ブラシのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes)」を参照してください。 グラデーションとその他の種類のブラシの詳細については、「[純色およびグラデーションによる塗りつぶしの概要](painting-with-solid-colors-and-gradients-overview.md)」を参照してください。
