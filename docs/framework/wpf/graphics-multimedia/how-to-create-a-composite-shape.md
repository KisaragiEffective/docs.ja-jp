---
title: '方法 : 複合図形を作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- shapes [WPF], composite
- composite shapes [WPF]
- graphics [WPF], composite shapes
ms.assetid: 8e5c7ef4-d7ed-4c43-afc9-ca01325c300b
ms.openlocfilehash: c56053f2b07d6055deac5097a68fd7b80ad704ba
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452098"
---
# <a name="how-to-create-a-composite-shape"></a>方法 : 複合図形を作成する
この例では、<xref:System.Windows.Media.Geometry> オブジェクトを使用して複合図形を作成し、<xref:System.Windows.Shapes.Path> 要素を使用してその図形を表示する方法を示します。 次の例では、複合図形を作成するために、<xref:System.Windows.Media.LineGeometry>、<xref:System.Windows.Media.EllipseGeometry>、および <xref:System.Windows.Media.RectangleGeometry> を <xref:System.Windows.Media.GeometryGroup> と共に使用します。 次に、<xref:System.Windows.Shapes.Path> 要素を使用してジオメトリが描画されます。  
  
## <a name="example"></a>例  
 [!code-xaml[GeometrySample#19](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/combininggeometriesexample.xaml#19)]  
  
 [!code-csharp[GeometriesMiscSnippets_procedural_snip#CompositeShapeCodeExampleInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometriesMiscSnippets_procedural_snip/CSharp/CompositeShapeExample.cs#compositeshapecodeexampleinline1)]
 [!code-vb[GeometriesMiscSnippets_procedural_snip#CompositeShapeCodeExampleInline1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GeometriesMiscSnippets_procedural_snip/visualbasic/compositeshapeexample.vb#compositeshapecodeexampleinline1)]  
  
 前の例で作成した図を次に示します。  
  
 ![System.windows.media.geometrygroup> を使用して作成された複合ジオメトリ](./media/wcpsdk-graphicsmm-compositegeometryexample1.jpg "wcpsdk_graphicsmm_compositegeometryexample1")  
複合ジオメトリ  
  
 多角形や曲線セグメントを持つ図形など、より複雑な図形は、<xref:System.Windows.Media.PathGeometry>を使用して作成できます。 <xref:System.Windows.Media.PathGeometry>を使用して図形を作成する方法を示す例については、「 [PathGeometry を使用](how-to-create-a-shape-by-using-a-pathgeometry.md)して図形を作成する」を参照してください。  この例では、<xref:System.Windows.Shapes.Path> 要素を使用して図形を画面に表示しますが、<xref:System.Windows.Media.Geometry> オブジェクトを使用して <xref:System.Windows.Media.GeometryDrawing> または <xref:System.Windows.Media.DrawingContext>の内容を記述することもできます。 また、クリッピングやヒットテストに使用することもできます。  
  
 この例は、より大きなサンプルの一部です。完全なサンプルについては、「[ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)」を参照してください。
