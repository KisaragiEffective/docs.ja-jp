---
title: '方法 : 2 次ベジエ曲線を作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- Bezier curves [WPF], creating
- quadratic Bezier curves [WPF], creating
- graphics [WPF], quadratic Bezier curves
ms.assetid: cd8fca4a-504e-4fd8-92ea-2969065a6e02
ms.openlocfilehash: caaf967b7cb5447d86dd031beeb16195413b0393
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452072"
---
# <a name="how-to-create-a-quadratic-bezier-curve"></a>方法 : 2 次ベジエ曲線を作成する
次の例では、2次ベジエ曲線を作成する方法を示します。  2次ベジエ曲線を作成するには、<xref:System.Windows.Media.PathGeometry>、<xref:System.Windows.Media.PathFigure>、および <xref:System.Windows.Media.QuadraticBezierSegment> クラスを使用します。  
  
## <a name="example"></a>例  
 次の例では、2次ベジエ曲線を (10100) から (300100) に描画しています。 曲線の制御点は (200200) です。  
  
 [xaml]  
  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、属性構文を使用してパスを記述できます。  
  
 [!code-xaml[GeometrySample#54](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/geometryattributesyntaxexample.xaml#54)]  
  
 [xaml]  
  
 (この属性構文では、実際には <xref:System.Windows.Media.PathGeometry>の軽量バージョンである <xref:System.Windows.Media.StreamGeometry>が作成されることに注意してください。 詳細については、「[パス マークアップ構文](path-markup-syntax.md)」のページを参照してください。)  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、オブジェクト要素構文を使用して2次ベジエ曲線を描画することもできます。 次の例は、前の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の例と同じです。  
  
 [!code-xaml[GeometrySample#34](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample/CS/pathgeometryexample.xaml#34)]  
  
 この例は、より大きなサンプルの一部です。完全なサンプルについては、「[ジオメトリのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry)」を参照してください。  
  
## <a name="see-also"></a>参照

- [楕円の円弧を作成する](how-to-create-an-elliptical-arc.md)
- [3 次ベジエ曲線を作成する](how-to-create-a-cubic-bezier-curve.md)
