---
title: TreeView のスタイルとテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- ControlTemplate [WPF], TreeView
- templates [WPF], TreeView
- parts [WPF], TreeView
- states [WPF], TreeView
- styles [WPF], TreeView
- TreeView [WPF], styles and templates
ms.assetid: a49adb77-0202-4caa-b94a-8bb110d7fa9a
ms.openlocfilehash: 45276d23380fe956fc3d59b90d5baae23ee8a7e2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283634"
---
# <a name="treeview-styles-and-templates"></a>TreeView のスタイルとテンプレート
このトピックでは、<xref:System.Windows.Controls.TreeView> コントロールのスタイルとテンプレートについて説明します。 <xref:System.Windows.Controls.ControlTemplate>の既定値を変更して外観を制御します。 詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。  
  
## <a name="treeview-parts"></a>TreeView パーツ  
 <xref:System.Windows.Controls.TreeView> コントロールには、名前付きの部分がありません。  
  
 <xref:System.Windows.Controls.TreeView>の <xref:System.Windows.Controls.ControlTemplate> を作成する場合、テンプレートに <xref:System.Windows.Controls.ScrollViewer>内の <xref:System.Windows.Controls.ItemsPresenter> が含まれている可能性があります。 (<xref:System.Windows.Controls.ItemsPresenter> には、<xref:System.Windows.Controls.TreeView>内の各項目が表示されます。 <xref:System.Windows.Controls.ScrollViewer> では、コントロール内でのスクロールが有効になります)。  <xref:System.Windows.Controls.ItemsPresenter> が <xref:System.Windows.Controls.ScrollViewer>の直接の子でない場合は、<xref:System.Windows.Controls.ItemsPresenter> の名前を `ItemsPresenter`に指定する必要があります。  
  
## <a name="treeview-states"></a>TreeView の状態  
 次の表は、<xref:System.Windows.Controls.TreeView> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="treeviewitem-parts"></a>TreeViewItem パーツ  
 次の表に、<xref:System.Windows.Controls.TreeViewItem> コントロールの名前付きの部分を示します。  
  
|要素|種類|説明|  
|----------|----------|-----------------|  
|PART_Header|<xref:System.Windows.FrameworkElement>|<xref:System.Windows.Controls.TreeView> コントロールのヘッダーの内容を格納しているビジュアル要素。|  
  
## <a name="treeviewitem-states"></a>TreeViewItem の状態  
 次の表は、<xref:System.Windows.Controls.TreeViewItem> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|----------------------|---------------------------|-----------------|  
|標準|CommonStates|既定の状態です。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.TreeViewItem>上に配置されています。|  
|無効|CommonStates|<xref:System.Windows.Controls.TreeViewItem> が無効になっています。|  
|フォーカスされている|FocusStates|<xref:System.Windows.Controls.TreeViewItem> にフォーカスがあります。|  
|フォーカスされていない|FocusStates|<xref:System.Windows.Controls.TreeViewItem> にフォーカスがありません。|  
|[展開済み]|すべての状態|<xref:System.Windows.Controls.TreeViewItem> コントロールが展開されます。|  
|Collapsed|すべての状態|<xref:System.Windows.Controls.TreeViewItem> コントロールが折りたたまれています。|  
|HasItems|HasItemsStates|<xref:System.Windows.Controls.TreeViewItem> には項目があります。|  
|NoItems|HasItemsStates|<xref:System.Windows.Controls.TreeViewItem> には項目がありません。|  
|Selected|SelectionStates|<xref:System.Windows.Controls.TreeViewItem> が選択されています。|  
|SelectedInactive|SelectionStates|<xref:System.Windows.Controls.TreeViewItem> が選択されていますが、アクティブになっていません。|  
|未選択|SelectionStates|<xref:System.Windows.Controls.TreeViewItem> が選択されていません。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="treeview-controltemplate-example"></a>TreeView ControlTemplate の例  
 次の例は、<xref:System.Windows.Controls.TreeView> コントロールとそれに関連付けられている型の <xref:System.Windows.Controls.ControlTemplate> を定義する方法を示しています。  
  
 [!code-xaml[ControlTemplateExamples#TreeView](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/treeview.xaml#treeview)]  
  
 前の例では、次のリソースの 1 つ以上を使用します。  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 完全なサンプルについては、[Styling with ControlTemplates Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating)を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [コントロールのスタイルとテンプレート](control-styles-and-templates.md)
- [コントロールのカスタマイズ](control-customization.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)
