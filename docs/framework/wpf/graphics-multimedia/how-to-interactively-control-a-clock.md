---
title: '方法: クロックを対話的に制御する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controlling clocks interactively [WPF]
- clocks [WPF], controlling interactively
ms.assetid: d0b520e0-2f18-4cef-977f-2909e709548a
ms.openlocfilehash: 2d18f395974750a6b85458f636a27f6101e7978f
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69951346"
---
# <a name="how-to-interactively-control-a-clock"></a>方法: クロックを対話的に制御する
オブジェクト<xref:System.Windows.Media.Animation.Clock> の<xref:System.Windows.Media.Animation.ClockController>プロパティを使用すると、対話形式で開始、一時停止、再開、シーク、時計をその塗りつぶし期間に進め、時計を止めることができます。 タイミングツリーのルートクロックだけを対話式で制御できます。  
  
> [!NOTE]
> クロックを直接操作する必要のないアニメーションを対話的に制御する方法は他にもあります。ストーリーボードを使用することもできます。 ストーリーボードは、マークアップとコードの両方でサポートされています。 例については、「ストーリーボードまたは[アニメーションの概要](animation-overview.md)を使用して[プロパティをアニメーション化](how-to-animate-a-property-by-using-a-storyboard.md)する」を参照してください。  
  
 次の例では、いくつかのボタンを使用して、アニメーションクロックを対話的に制御しています。  
  
## <a name="example"></a>例  
 [!code-csharp[timingbehaviors_procedural_snip#GraphicsMMClockControllerExample](~/samples/snippets/csharp/VS_Snippets_Wpf/timingbehaviors_procedural_snip/CSharp/ClockControllerExample.cs#graphicsmmclockcontrollerexample)]
 [!code-vb[timingbehaviors_procedural_snip#GraphicsMMClockControllerExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/timingbehaviors_procedural_snip/visualbasic/clockcontrollerexample.vb#graphicsmmclockcontrollerexample)]  
  
## <a name="see-also"></a>関連項目

- [ストーリーボードを使ってプロパティをアニメーション化する](how-to-animate-a-property-by-using-a-storyboard.md)
- [アニメーションの概要](animation-overview.md)
