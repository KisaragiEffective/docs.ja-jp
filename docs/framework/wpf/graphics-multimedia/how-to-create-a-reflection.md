---
title: '方法 : 反射を作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- creating reflections [WPF]
- brushes [WPF], creating reflections
- reflections [WPF], creating
ms.assetid: 4f017e16-ab80-43c7-98df-03b6bddbb203
ms.openlocfilehash: 8a5ed345c0aa25312bd74799264e1f66ab4554e0
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452059"
---
# <a name="how-to-create-a-reflection"></a>方法 : 反射を作成する
この例では、<xref:System.Windows.Media.VisualBrush> を使用してリフレクションを作成する方法を示します。 <xref:System.Windows.Media.VisualBrush> は既存のビジュアルを表示できるため、この機能を使用して、反射や拡大などの興味深い視覚効果を生成することができます。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Media.VisualBrush> を使用して、複数の要素を含む <xref:System.Windows.Controls.Border> のリフレクションを作成します。 次の図は、この例で生成される出力を示しています。  
  
 ![反映された Visual オブジェクト](./media/graphicsmm-visualbrush-reflection-small.jpg "graphicsmm_visualbrush_reflection_small")  
反映された Visual オブジェクト  
  
 [!code-csharp[visualbrush_markup_snip#GraphicsMMVisualBrushReflectionExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/visualbrush_markup_snip/CSharp/ReflectionExample.cs#graphicsmmvisualbrushreflectionexamplewholepage)]
 [!code-vb[visualbrush_markup_snip#GraphicsMMVisualBrushReflectionExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/visualbrush_markup_snip/visualbasic/reflectionexample.vb#graphicsmmvisualbrushreflectionexamplewholepage)]
 [!code-xaml[visualbrush_markup_snip#GraphicsMMVisualBrushReflectionExampleWholePage](~/samples/snippets/xaml/VS_Snippets_Wpf/visualbrush_markup_snip/XAML/ReflectionExample.xaml#graphicsmmvisualbrushreflectionexamplewholepage)]  
  
 画面の一部を拡大する方法と、反射を作成する方法を示す例を含む完全なサンプルについては、「 [Visualbrush sample](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/VisualBrush)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.VisualBrush>
- [イメージ、描画、およびビジュアルによる塗りつぶし](painting-with-images-drawings-and-visuals.md)
