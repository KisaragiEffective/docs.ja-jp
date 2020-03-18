---
title: '方法 : フォーカス イベントを使用して要素の色を変更する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- focus events [WPF], changing element color for
- colors of elements [WPF], changing
- elements [WPF], changing color of
ms.assetid: 7e246802-3625-47a7-ae9d-c8a2a40fd040
ms.openlocfilehash: 5c59dc5f2f8f26fac69933f9ef641a3a51306619
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004838"
---
# <a name="how-to-change-the-color-of-an-element-using-focus-events"></a>方法 : フォーカス イベントを使用して要素の色を変更する
この例では、<xref:System.Windows.UIElement.GotFocus> イベントと <xref:System.Windows.UIElement.LostFocus> イベントを使用して、要素が取得され、フォーカスを失ったときに、その色を変更する方法を示します。  
  
 この例は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルと分離コードファイルで構成されています。  
  
## <a name="example"></a>例  
 次の XAML は、2つの <xref:System.Windows.Controls.Button> オブジェクトで構成されるユーザーインターフェイスを作成し、<xref:System.Windows.UIElement.GotFocus> イベントと <xref:System.Windows.UIElement.LostFocus> イベントのイベントハンドラーを <xref:System.Windows.Controls.Button> オブジェクトにアタッチします。  
  
 [!code-xaml[gotfocusLostfocusEffectUsingEvent#GotLostFocusSampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/gotfocusLostfocusEffectUsingEvent/CSharp/Window1.xaml#gotlostfocussamplexaml)]  
  
 次のコードでは、<xref:System.Windows.UIElement.GotFocus> と <xref:System.Windows.UIElement.LostFocus> のイベントハンドラーを作成します。  <xref:System.Windows.Controls.Button> がキーボードフォーカスを取得すると、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> が赤に変更されます。  <xref:System.Windows.Controls.Button> がキーボードフォーカスを失うと、<xref:System.Windows.Controls.Button> の <xref:System.Windows.Controls.Control.Background%2A> が白に戻されます。  
  
 [!code-csharp[gotfocusLostfocusEffectUsingEvent#GotLostFocusSampleEventHandlers](~/samples/snippets/csharp/VS_Snippets_Wpf/gotfocusLostfocusEffectUsingEvent/CSharp/Window1.xaml.cs#gotlostfocussampleeventhandlers)]
 [!code-vb[gotfocusLostfocusEffectUsingEvent#GotLostFocusSampleEventHandlers](~/samples/snippets/visualbasic/VS_Snippets_Wpf/gotfocusLostfocusEffectUsingEvent/VisualBasic/Window1.xaml.vb#gotlostfocussampleeventhandlers)]  
  
## <a name="see-also"></a>参照

- [入力の概要](input-overview.md)
