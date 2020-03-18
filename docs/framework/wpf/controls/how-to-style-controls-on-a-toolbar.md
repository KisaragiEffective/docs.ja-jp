---
title: '方法 : ToolBar のコントロールのスタイルを指定する'
ms.date: 03/30/2017
helpviewer_keywords:
- styling controls on toolbar [WPF]
- toolbars [WPF]
- customizing controls on toolbar [WPF]
ms.assetid: ba6ae056-d6a9-4c24-90f8-467ab0bc0b1a
ms.openlocfilehash: 78b9fc505c3c9045a0ca16ddaa1361c90bcc896a
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459409"
---
# <a name="how-to-style-controls-on-a-toolbar"></a>方法 : ToolBar のコントロールのスタイルを指定する
<xref:System.Windows.Controls.ToolBar> は、<xref:System.Windows.Controls.ToolBar>内のコントロールのスタイルを指定する <xref:System.Windows.ResourceKey> オブジェクトを定義します。  <xref:System.Windows.Controls.ToolBar>内のコントロールのスタイルを設定するには、スタイルの `x:key` 属性を <xref:System.Windows.Controls.ToolBar>で定義されている <xref:System.Windows.ResourceKey> に設定します。  
  
 <xref:System.Windows.Controls.ToolBar> は、次の <xref:System.Windows.ResourceKey> オブジェクトを定義します。  
  
- <xref:System.Windows.Controls.ToolBar.ButtonStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.CheckBoxStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.ComboBoxStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.MenuStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.RadioButtonStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.SeparatorStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.TextBoxStyleKey%2A>  
  
- <xref:System.Windows.Controls.ToolBar.ToggleButtonStyleKey%2A>  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Controls.ToolBar>内のコントロールのスタイルを定義します。  
  
 [!code-xaml[ToolBar_snip#ToolBarAllStyles](~/samples/snippets/csharp/VS_Snippets_Wpf/ToolBar_snip/CS/pane1.xaml#toolbarallstyles)]  
[!code-xaml[ToolBar_snip#ToolBar](~/samples/snippets/csharp/VS_Snippets_Wpf/ToolBar_snip/CS/pane1.xaml#toolbar)]  
  
## <a name="see-also"></a>関連項目

- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
