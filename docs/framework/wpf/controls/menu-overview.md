---
title: メニューの概要
ms.date: 03/30/2017
helpviewer_keywords:
- Menu control [WPF]
- controls [WPF], Menu
ms.assetid: 67df6de5-db96-4c71-b752-af90729a6537
ms.openlocfilehash: 3d5cfba0734e684349f7d73b7242f4b69089b94d
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124651"
---
# <a name="menu-overview"></a>メニューの概要
<xref:System.Windows.Controls.Menu> クラスを使用すると、コマンドおよびイベントハンドラーに関連付けられている要素を階層順に整理できます。 各 <xref:System.Windows.Controls.Menu> 要素には、<xref:System.Windows.Controls.MenuItem> 要素のコレクションが含まれています。  

<a name="menu_control"></a>   
## <a name="menu-control"></a>メニュー コントロール  
 <xref:System.Windows.Controls.Menu> コントロールには、アプリケーションのコマンドまたはオプションを指定する項目の一覧が表示されます。 通常、<xref:System.Windows.Controls.MenuItem> をクリックするとサブメニューが表示され、アプリケーションはコマンドを実行します。  
  
<a name="creating_menus"></a>   
## <a name="creating-menus"></a>メニューの作成  
 次の例では、<xref:System.Windows.Controls.TextBox>内のテキストを操作する <xref:System.Windows.Controls.Menu> を作成します。 <xref:System.Windows.Controls.Menu> には、<xref:System.Windows.Controls.MenuItem.Command%2A>、<xref:System.Windows.Controls.MenuItem.IsCheckable%2A>、および <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> の各プロパティと、<xref:System.Windows.Controls.MenuItem.Checked>、<xref:System.Windows.Controls.MenuItem.Unchecked>、および <xref:System.Windows.Controls.MenuItem.Click> イベントを使用する <xref:System.Windows.Controls.MenuItem> オブジェクトが含まれています。  
  
 [!code-xaml[MenuItemCommandsAndEvents#1](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuItemCommandsAndEvents/CSharp/Window1.xaml#1)]  
  
 [!code-csharp[MenuItemCommandsAndEvents#2](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuItemCommandsAndEvents/CSharp/Window1.xaml.cs#2)]
 [!code-vb[MenuItemCommandsAndEvents#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MenuItemCommandsAndEvents/VisualBasic/Window1.xaml.vb#2)]  
  
<a name="menus_with_shortcutkeys"></a>   
## <a name="menuitems-with-keyboard-shortcuts"></a>キーボード ショートカット付きのメニュー項目  
 キーボードショートカットは、<xref:System.Windows.Controls.Menu> のコマンドを呼び出すためにキーボードで入力できる文字の組み合わせです。 たとえば、**コピー**のショートカットは CTRL+C です。 キーボードショートカットとメニュー項目で使用するプロパティには、<xref:System.Windows.Controls.MenuItem.InputGestureText%2A> または <xref:System.Windows.Controls.MenuItem.Command%2A>の2つがあります。  
  
<a name="menus_inputgesturetext"></a>   
### <a name="inputgesturetext"></a>InputGestureText  
 次の例では、<xref:System.Windows.Controls.MenuItem.InputGestureText%2A> プロパティを使用して、<xref:System.Windows.Controls.MenuItem> コントロールにキーボードショートカットテキストを割り当てる方法を示します。 これは、キーボード ショートカットをメニュー項目に配置するだけです。  コマンドは <xref:System.Windows.Controls.MenuItem>に関連付けられません。 アプリケーションでは、アクションを実行するために、ユーザーの入力を処理する必要があります。  
  
 [!code-xaml[MenuEvent#6](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuEvent/CSharp/Pane1.xaml#6)]  
  
<a name="menus_commands"></a>   
### <a name="command"></a>command  
 次の例は、<xref:System.Windows.Controls.MenuItem.Command%2A> プロパティを使用して、 **Open**コマンドと**Save**コマンドを <xref:System.Windows.Controls.MenuItem> コントロールに関連付ける方法を示しています。 Command プロパティによってコマンドが <xref:System.Windows.Controls.MenuItem>に関連付けられるだけでなく、ショートカットとして使用する入力ジェスチャテキストも提供されます。  
  
 [!code-xaml[MenuEvent#8](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuEvent/CSharp/Pane1.xaml#8)]  
  
 <xref:System.Windows.Controls.MenuItem> クラスには、コマンドが発生する要素を指定する <xref:System.Windows.Controls.MenuItem.CommandTarget%2A> プロパティもあります。 <xref:System.Windows.Controls.MenuItem.CommandTarget%2A> が設定されていない場合、キーボードフォーカスを持つ要素はコマンドを受け取ります。 コマンドの詳細については、[Commanding Overview](../advanced/commanding-overview.md)を参照してください。  
  
<a name="menu_styling"></a>   
## <a name="menu-styling"></a>メニューのスタイル指定  
 コントロールのスタイル設定を使用すると、カスタムコントロールを記述しなくても、<xref:System.Windows.Controls.Menu> コントロールの外観と動作を大幅に変更できます。 ビジュアルプロパティの設定に加えて、コントロールの個々の部分に <xref:System.Windows.Style> を適用したり、プロパティを使用してコントロールの一部の動作を変更したり、パーツを追加したり、コントロールのレイアウトを変更したりすることもできます。 次の例では、<xref:System.Windows.Style> を <xref:System.Windows.Controls.Menu> コントロールに追加するいくつかの方法を示します。  
  
 最初のコード例では、スタイルで現在のシステム設定を使用する方法を示す `Simple` と呼ばれる <xref:System.Windows.Style> を定義します。 このコードは、`MenuHighlightBrush`の色をメニューの背景色として、 `MenuTextBrush`の色をメニューの前景色として割り当てます。 ブラシを割り当てるには、リソース キーを使用することに注意してください。  
  
 [!code-xaml[MenuStylesSnippet#1](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuStylesSnippet/CS/app.xaml#1)]  
  
 次の例では <xref:System.Windows.Trigger> 要素を使用して、<xref:System.Windows.Controls.Menu>で発生するイベントに応答して <xref:System.Windows.Controls.MenuItem> の外観を変更できます。 <xref:System.Windows.Controls.Menu>上にマウスを移動すると、メニュー項目の前景色とフォント特性が変わります。  
  
 [!code-xaml[MenuStylesSnippet#2](~/samples/snippets/csharp/VS_Snippets_Wpf/MenuStylesSnippet/CS/app.xaml#2)]  
  
## <a name="see-also"></a>参照

- [WPF Controls Gallery Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Getting%20Started/ControlsAndLayout)
