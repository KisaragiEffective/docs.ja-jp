---
title: ToolBar の概要
ms.date: 03/30/2017
helpviewer_keywords:
- controls [WPF], ToolBar
- ToolBar control [WPF]
ms.assetid: a8edb32c-118d-4f31-b6e6-8899082b504b
ms.openlocfilehash: 460d4420d5faf849a8d05a94e0170aea384f69b4
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452696"
---
# <a name="toolbar-overview"></a>ToolBar の概要
<xref:System.Windows.Controls.ToolBar> コントロールは、通常は関数に関連するコマンドまたはコントロールのグループのコンテナーです。 <xref:System.Windows.Controls.ToolBar> には、通常、コマンドを呼び出すボタンが含まれています。  

<a name="ToolBarControl"></a>   
## <a name="toolbar-control"></a>ToolBar コントロール  
 <xref:System.Windows.Controls.ToolBar> コントロールは、ボタンやその他のコントロールを1つの行または列に配置するのと同じように、バーのような名前を持ちます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Controls.ToolBar> コントロールにはオーバーフロー機構が用意されており、サイズ制約のある <xref:System.Windows.Controls.ToolBar> 内で自然に収まらない項目は、特殊なオーバーフロー領域に配置されます。 また、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Controls.ToolBar> コントロールは、通常、関連する <xref:System.Windows.Controls.ToolBarTray> コントロールと共に使用されます。このコントロールは、ユーザーが開始したツールバーのサイズと配置をサポートするだけでなく、特別なレイアウト動作を提供します。  
  
<a name="Creating_ToolBars"></a>   
## <a name="specifying-the-position-of-toolbars-in-a-toolbartray"></a>ToolBarTray でツールバーの位置を指定する  
 <xref:System.Windows.Controls.ToolBar.Band%2A> と <xref:System.Windows.Controls.ToolBar.BandIndex%2A> のプロパティを使用して、<xref:System.Windows.Controls.ToolBar> を <xref:System.Windows.Controls.ToolBarTray>に配置します。 <xref:System.Windows.Controls.ToolBar.Band%2A> は、<xref:System.Windows.Controls.ToolBar> が親 <xref:System.Windows.Controls.ToolBarTray>内に配置される位置を示します。 <xref:System.Windows.Controls.ToolBar.BandIndex%2A> は、<xref:System.Windows.Controls.ToolBar> がそのバンド内に配置される順序を示します。 次の例は、このプロパティを使用して、<xref:System.Windows.Controls.ToolBarTray>内に <xref:System.Windows.Controls.ToolBar> コントロールを配置する方法を示しています。  
  
 [!code-xaml[ToolBarExample#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ToolBarExample/CS/Pane1.xaml#2)]  
  
<a name="ToolBars_with_Overflow_Items"></a>   
## <a name="toolbars-with-overflow-items"></a>オーバーフロー項目を含むツールバー  
 多くの場合 <xref:System.Windows.Controls.ToolBar> コントロールには、ツールバーのサイズに収まりきらない項目が含まれています。 この場合、<xref:System.Windows.Controls.ToolBar> にオーバーフローボタンが表示されます。 オーバーフロー項目を表示するには、ユーザーがオーバーフローボタンをクリックすると、<xref:System.Windows.Controls.ToolBar>の下のポップアップウィンドウに項目が表示されます。 次の図は、オーバーフロー項目を含む <xref:System.Windows.Controls.ToolBar> を示しています。  
  
 ![オーバーフロー項目を含むツールバーを示すスクリーンショット。](./media/toolbar-overview/toolbar-overflow-items.png)  
  
 <xref:System.Windows.Controls.ToolBar.OverflowMode%2A?displayProperty=nameWithType> 添付プロパティを <xref:System.Windows.Controls.OverflowMode.Always?displayProperty=nameWithType>、<xref:System.Windows.Controls.OverflowMode.Never?displayProperty=nameWithType>、または <xref:System.Windows.Controls.OverflowMode.AsNeeded?displayProperty=nameWithType>に設定することにより、ツールバーのアイテムをオーバーフローパネルに配置するタイミングを指定できます。 次の例では、ツールバーの最後の 4 つのボタンを常にオーバーフロー パネルに配置します。  
  
 [!code-xaml[ToolBarExample#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ToolBarExample/CS/Pane1.xaml#3)]  
  
 <xref:System.Windows.Controls.ToolBar> は、<xref:System.Windows.Controls.Primitives.ToolBarPanel> と、その <xref:System.Windows.Controls.ControlTemplate>内の <xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel> を使用します。  <xref:System.Windows.Controls.Primitives.ToolBarPanel> は、ツールバー上の項目のレイアウトを担当します。  <xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel> は、<xref:System.Windows.Controls.ToolBar>に合わない項目のレイアウトを担当します。 <xref:System.Windows.Controls.ToolBar>の <xref:System.Windows.Controls.ControlTemplate> の例については、「」を参照してください。  
  
 [ツール バーのスタイルとテンプレート](toolbar-styles-and-templates.md)  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Controls.Primitives.ToolBarPanel>
- <xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel>
- [ToolBar のコントロールのスタイルを設定する](how-to-style-controls-on-a-toolbar.md)
- [WPF Controls Gallery Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Getting%20Started/ControlsAndLayout)
