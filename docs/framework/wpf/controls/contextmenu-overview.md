---
title: ContextMenu の概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], ContextMenu
- ContextMenu controls [WPF], about ContextMenu controls
ms.assetid: 16909c42-799a-4561-91e0-7d69dcfeea91
ms.openlocfilehash: b973d47711632f4c0fe56f042545598272c79d2d
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124365"
---
# <a name="contextmenu-overview"></a>ContextMenu の概要
<xref:System.Windows.Controls.ContextMenu> クラスは、コンテキスト固有の <xref:System.Windows.Controls.Menu>を使用して機能を公開する要素を表します。 通常、ユーザーはマウスボタンを右クリックして、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] 内の <xref:System.Windows.Controls.ContextMenu> を公開します。 このトピックでは、<xref:System.Windows.Controls.ContextMenu> 要素について説明し、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] およびコードでの使用方法の例を示します。  

<a name="contextmenu_control"></a>   
## <a name="contextmenu-control"></a>ContextMenu コントロール  
 <xref:System.Windows.Controls.ContextMenu> が特定のコントロールにアタッチされています。 <xref:System.Windows.Controls.ContextMenu> 要素を使用すると、特定のコントロールに関連付けられているコマンドやオプション (<xref:System.Windows.Controls.Button>など) を指定する項目の一覧をユーザーに表示できます。 ユーザーがコントロールを右クリックすると、メニューが表示されます。 通常、<xref:System.Windows.Controls.MenuItem> をクリックするとサブメニューが表示され、アプリケーションはコマンドを実行します。  
  
<a name="creating_contextmenus"></a>   
## <a name="creating-contextmenus"></a>ContextMenu の作成  
 次の例は、サブメニューを使用して <xref:System.Windows.Controls.ContextMenu> を作成する方法を示しています。 <xref:System.Windows.Controls.ContextMenu> コントロールは、ボタンコントロールにアタッチされます。  
  
 [!code-xaml[ContextMenu#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ContextMenu/CSharp/Pane1.xaml#1)]  
  
 [!code-csharp[ContextMenu#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ContextMenu/CSharp/Pane1.xaml.cs#2)]
 [!code-vb[ContextMenu#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ContextMenu/VisualBasic/Pane1.xaml.vb#2)]  
  
<a name="applying_styles_to_contextmenu"></a>   
## <a name="applying-styles-to-a-contextmenu"></a>ContextMenu へのスタイルの適用  
 コントロール <xref:System.Windows.Style>を使用すると、カスタムコントロールを記述しなくても、<xref:System.Windows.Controls.ContextMenu> の外観と動作を大幅に変更することができます。 視覚プロパティの設定に加えて、コントロールの一部にスタイルを適用することもできます。 たとえば、プロパティを使用してコントロールの一部の動作を変更することも、<xref:System.Windows.Controls.ContextMenu>のレイアウトにパーツを追加したり、レイアウトを変更したりすることもできます。 次の例では、<xref:System.Windows.Controls.ContextMenu> コントロールにスタイルを追加する方法をいくつか示します。  
  
 最初の例では、`SimpleSysResources` という名前のスタイルを定義し、スタイルで現在のシステム設定を使用する方法を示します。 この例では、<xref:System.Windows.SystemColors.MenuHighlightBrushKey%2A> を <xref:System.Windows.Controls.Control.Background%2A> の色として割り当て、<xref:System.Windows.Controls.ContextMenu>の <xref:System.Windows.Controls.Control.Foreground%2A> の色として <xref:System.Windows.SystemColors.MenuTextBrushKey%2A> します。  
  
```xaml  
<Style x:Key="SimpleSysResources" TargetType="{x:Type MenuItem}">  
  <Setter Property = "Background" Value=   
    "{DynamicResource {x:Static SystemColors.MenuHighlightBrushKey}}"/>  
  <Setter Property = "Foreground" Value=   
    "{DynamicResource {x:Static SystemColors.MenuTextBrushKey}}"/>  
</Style>  
```  
  
 次の例では、<xref:System.Windows.Trigger> 要素を使用して、<xref:System.Windows.Controls.ContextMenu>で発生したイベントに応答して <xref:System.Windows.Controls.Menu> の外観を変更します。 ユーザーがメニューの上にマウスを移動すると、<xref:System.Windows.Controls.ContextMenu> 項目の外観が変わります。  
  
```xaml  
<Style x:Key="Triggers" TargetType="{x:Type MenuItem}">  
  <Style.Triggers>  
    <Trigger Property="MenuItem.IsMouseOver" Value="true">  
      <Setter Property = "FontSize" Value="16"/>  
      <Setter Property = "FontStyle" Value="Italic"/>  
      <Setter Property = "Foreground" Value="Red"/>  
    </Trigger>  
  </Style.Triggers>  
</Style>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Controls.ContextMenu>
- <xref:System.Windows.Style>
- <xref:System.Windows.Controls.Menu>
- <xref:System.Windows.Controls.MenuItem>
- [ContextMenu](contextmenu.md)
- [ContextMenu のスタイルとテンプレート](contextmenu-styles-and-templates.md)
- [WPF Controls Gallery Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Getting%20Started/ControlsAndLayout)
