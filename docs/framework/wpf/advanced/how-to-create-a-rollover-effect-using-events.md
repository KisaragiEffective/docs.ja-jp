---
title: '方法 : イベントを使用してロールオーバー効果を作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- colors of elements [WPF], changing
- rollover effect [WPF]
- element colors [WPF], changing
ms.assetid: 3b20d028-6f1c-4b25-95d2-fa68cefbdb4c
ms.openlocfilehash: ccdc8aeb26b538814b2f9020e1e3a5b311fa5a99
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460162"
---
# <a name="how-to-create-a-rollover-effect-using-events"></a>方法 : イベントを使用してロールオーバー効果を作成する
この例では、マウスポインターが要素によって占有されている領域を離れるときに、要素の色を変更する方法を示します。  
  
 この例は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルと分離コードファイルで構成されています。  
  
> [!NOTE]
> この例では、イベントを使用する方法を示していますが、これと同じ効果を得るには、スタイルで <xref:System.Windows.Trigger> を使用することをお勧めします。 詳しくは、「 [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」をご覧ください。  
  
## <a name="example"></a>例  
 次の XAML は、<xref:System.Windows.Controls.TextBlock>の周囲 <xref:System.Windows.Controls.Border> で構成されるユーザーインターフェイスを作成し、<xref:System.Windows.Input.Mouse.MouseEnter> と <xref:System.Windows.UIElement.MouseLeave> のイベントハンドラーを <xref:System.Windows.Controls.Border>にアタッチします。  
  
 [!code-xaml[mouseenterMouseleave#MouseEnterLeaveSampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/mouseenterMouseleave/CSharp/Window1.xaml#mouseenterleavesamplexaml)]  
  
 次のコードでは、<xref:System.Windows.UIElement.MouseEnter> と <xref:System.Windows.UIElement.MouseLeave> のイベントハンドラーを作成します。  マウスポインターが <xref:System.Windows.Controls.Border>に入ると、<xref:System.Windows.Controls.Border> の背景が赤に変更されます。  マウスポインターが <xref:System.Windows.Controls.Border>から離れると、<xref:System.Windows.Controls.Border> の背景が白に戻されます。  
  
 [!code-csharp[mouseenterMouseleave#MouseEnterLeaveSampleEventHandlers](~/samples/snippets/csharp/VS_Snippets_Wpf/mouseenterMouseleave/CSharp/Window1.xaml.cs#mouseenterleavesampleeventhandlers)]
 [!code-vb[mouseenterMouseleave#MouseEnterLeaveSampleEventHandlers](~/samples/snippets/visualbasic/VS_Snippets_Wpf/mouseenterMouseleave/VisualBasic/Window1.xaml.vb#mouseenterleavesampleeventhandlers)]
