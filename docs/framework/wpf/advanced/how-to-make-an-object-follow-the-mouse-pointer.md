---
title: '方法 : オブジェクトをマウス ポインターに追従させる'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- following the mouse pointer (cursor)
- mouse pointer (cursor), making objects follow
- cursor (mouse pointer), making objects follow
ms.assetid: 50b20415-14bc-405c-baf3-2fb254fffde3
ms.openlocfilehash: 4ef3028b6c71b94a593d168ad6570c4aec12b86b
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005068"
---
# <a name="how-to-make-an-object-follow-the-mouse-pointer"></a>方法 : オブジェクトをマウス ポインターに追従させる
この例では、マウスポインターが画面上を移動したときにオブジェクトの寸法を変更する方法を示します。  
  
 この例には、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] を作成する [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルと、イベントハンドラーを作成する分離コードファイルが含まれています。  
  
## <a name="example"></a>例  
 次の XAML は、<xref:System.Windows.Controls.StackPanel>内の <xref:System.Windows.Shapes.Ellipse> で構成される [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を作成し、<xref:System.Windows.UIElement.MouseMove> イベントのイベントハンドラーをアタッチします。  
  
 [!code-xaml[mouseMoveWithPointer#MouseMoveWithPointerXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/mouseMoveWithPointer/CSharp/Window1.xaml#mousemovewithpointerxaml)]  
  
 次のコードでは、<xref:System.Windows.UIElement.MouseMove> イベントハンドラーを作成します。  マウスポインターを動かすと、<xref:System.Windows.Shapes.Ellipse> の高さと幅が増減します。  
  
 [!code-csharp[mouseMoveWithPointer#MouseMovePointerGetPosition](~/samples/snippets/csharp/VS_Snippets_Wpf/mouseMoveWithPointer/CSharp/Window1.xaml.cs#mousemovepointergetposition)]
 [!code-vb[mouseMoveWithPointer#MouseMovePointerGetPosition](~/samples/snippets/visualbasic/VS_Snippets_Wpf/mouseMoveWithPointer/VisualBasic/Window1.xaml.vb#mousemovepointergetposition)]  
  
## <a name="see-also"></a>参照

- [入力の概要](input-overview.md)
