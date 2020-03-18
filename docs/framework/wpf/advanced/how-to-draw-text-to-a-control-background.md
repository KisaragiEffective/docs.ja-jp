---
title: '方法 : コントロールの背景にテキストを描画する'
ms.date: 03/30/2017
helpviewer_keywords:
- controls [WPF], drawing text to backgrounds
- text [WPF], drawing to control backgrounds
- drawing [WPF], text to control backgrounds
- backgrounds [WPF], drawing text to
- typography [WPF], drawing text to control backgrounds
ms.assetid: 686d8fba-f61c-4974-a871-c635d67a7f69
ms.openlocfilehash: 14300f763b67c6ac67ee408de2009c328d499c13
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424362"
---
# <a name="how-to-draw-text-to-a-controls-background"></a>方法 : コントロールの背景にテキストを描画する
テキスト文字列を <xref:System.Windows.Media.FormattedText> オブジェクトに変換し、そのオブジェクトをコントロールの <xref:System.Windows.Media.DrawingContext>に描画することによって、コントロールの背景にテキストを直接描画できます。 また、この方法を使用して、<xref:System.Windows.Controls.Canvas> や <xref:System.Windows.Controls.StackPanel>などの <xref:System.Windows.Controls.Panel>から派生したオブジェクトの背景を描画することもできます。  
  
 ![テキストを背景として表示するコントロールのスクリーンショット。](./media/how-to-draw-text-to-a-control-background/draw-text-background.png "DrawText2Background01")  
カスタム テキスト背景のコントロールの例  
  
## <a name="example"></a>例  
 コントロールの背景に描画するには、新しい <xref:System.Windows.Media.DrawingBrush> オブジェクトを作成し、変換されたテキストをオブジェクトの <xref:System.Windows.Media.DrawingContext>に描画します。 次に、コントロールの background プロパティに新しい <xref:System.Windows.Media.DrawingBrush> を割り当てます。  
  
 次のコード例は、<xref:System.Windows.Media.FormattedText> オブジェクトを作成し、<xref:System.Windows.Controls.Label> と <xref:System.Windows.Controls.Button> オブジェクトの背景に描画する方法を示しています。  
  
 [!code-csharp[DrawTextToControlBackground#DrawTextToControlBackground1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawTextToControlBackground/CSHARP/Window1.xaml.cs#drawtexttocontrolbackground1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.FormattedText>
- [書式設定されたテキストの描画](drawing-formatted-text.md)
