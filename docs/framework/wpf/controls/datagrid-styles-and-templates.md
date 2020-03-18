---
title: DataGrid のスタイルとテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- states [WPF], DataGrid
- ControlTemplate [WPF], DataGrid
- DataGrid [WPF], styles and templates
- templates [WPF], DataGrid
- styles [WPF], DataGrid
- parts [WPF], DataGrid
ms.assetid: 9cb31d63-f148-4d25-b079-816e73f988c7
ms.openlocfilehash: 066e8c9ce1112399be8128d0821498f0d56a3dc3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283804"
---
# <a name="datagrid-styles-and-templates"></a>DataGrid のスタイルとテンプレート
このトピックでは、<xref:System.Windows.Controls.DataGrid> コントロールのスタイルとテンプレートについて説明します。 <xref:System.Windows.Controls.ControlTemplate>の既定値を変更して外観を制御します。 詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。  
  
## <a name="datagrid-parts"></a>DataGrid のパーツ  
 次の表に、<xref:System.Windows.Controls.DataGrid> コントロールの名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_ColumnHeadersPresenter|<xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter>|列ヘッダーを格納している行。|  
  
 <xref:System.Windows.Controls.DataGrid>の <xref:System.Windows.Controls.ControlTemplate> を作成する場合、テンプレートに <xref:System.Windows.Controls.ScrollViewer>内の <xref:System.Windows.Controls.ItemsPresenter> が含まれている可能性があります。 (<xref:System.Windows.Controls.ItemsPresenter> には、<xref:System.Windows.Controls.DataGrid>内の各項目が表示されます。 <xref:System.Windows.Controls.ScrollViewer> では、コントロール内でのスクロールが有効になります)。  <xref:System.Windows.Controls.ItemsPresenter> が <xref:System.Windows.Controls.ScrollViewer>の直接の子でない場合は、<xref:System.Windows.Controls.ItemsPresenter> の名前を `ItemsPresenter`に指定する必要があります。  
  
 <xref:System.Windows.Controls.DataGrid> の既定のテンプレートには、<xref:System.Windows.Controls.ScrollViewer> コントロールが含まれています。 <xref:System.Windows.Controls.ScrollViewer>によって定義されるパートの詳細については、「 [ScrollViewer のスタイルとテンプレート](scrollviewer-styles-and-templates.md)」を参照してください。  
  
## <a name="datagrid-states"></a>DataGrid の状態  
 次の表は、<xref:System.Windows.Controls.DataGrid> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|無効|CommonStates|コントロールが無効になっています。|  
|InvalidFocused|ValidationStates|コントロールが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|コントロールが無効で、フォーカスがありません。|  
|Valid|ValidationStates|コントロールは有効です。|  
  
## <a name="datagridcell-parts"></a>DataGridCell パーツ  
 <xref:System.Windows.Controls.DataGridCell> 要素には名前付きの部分がありません。  
  
## <a name="datagridcell-states"></a>DataGridCell の状態  
 次の表は、<xref:System.Windows.Controls.DataGridCell> 要素の表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|MouseOver|CommonStates|マウスポインターがセルの上に置かれています。|  
|フォーカスされている|FocusStates|セルにフォーカスがあります。|  
|フォーカスされていない|FocusStates|セルにフォーカスがありません|  
|現在|CurrentStates|セルが現在のセルです。|  
|Regular|CurrentStates|セルが現在のセルではありません。|  
|表示|InteractionStates|セルは表示モードです。|  
|編集中|InteractionStates|セルは編集モードです。|  
|Selected|SelectionStates|セルが選択されています。|  
|未選択|SelectionStates|セルが選択されていません。|  
|InvalidFocused|ValidationStates|セルが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|セルが無効であり、フォーカスがありません。|  
|Valid|ValidationStates|セルが有効です。|  
  
## <a name="datagridrow-parts"></a>DataGridRow パーツ  
 <xref:System.Windows.Controls.DataGridRow> 要素には名前付きの部分がありません。  
  
## <a name="datagridrow-states"></a>DataGridRow の状態  
 次の表は、<xref:System.Windows.Controls.DataGridRow> 要素の表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|MouseOver|CommonStates|マウスポインターが行の上に配置されます。|  
|MouseOver_Editing|CommonStates|マウスポインターが行の上に置かれ、行が編集モードになっています。|  
|MouseOver_Selected|CommonStates|マウスポインターが行の上に配置され、行が選択されます。|  
|MouseOver_Unfocused_Editing|CommonStates|マウスポインターが行の上に配置されており、行が編集モードであり、フォーカスがありません。|  
|MouseOver_Unfocused_Selected|CommonStates|マウスポインターが行の上に配置され、行が選択されており、フォーカスがありません。|  
|Normal_AlternatingRow|CommonStates|行は交互の行です。|  
|Normal_Editing|CommonStates|行は編集モードです。|  
|Normal_Selected|CommonStates|行が選択されています。|  
|Unfocused_Editing|CommonStates|行が編集モードであり、フォーカスがありません。|  
|Unfocused_Selected|CommonStates|行が選択されていますが、フォーカスがありません。|  
|InvalidFocused|ValidationStates|コントロールが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|コントロールが無効で、フォーカスがありません。|  
|Valid|ValidationStates|コントロールは有効です。|  
  
## <a name="datagridrowheader-parts"></a>DataGridRowHeader パーツ  
 次の表に、<xref:System.Windows.Controls.Primitives.DataGridRowHeader> 要素の名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_TopHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|先頭から行ヘッダーのサイズを変更するために使用される要素。|  
|PART_BottomHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|行ヘッダーのサイズを下端から変更するために使用される要素。|  
  
## <a name="datagridrowheader-states"></a>DataGridRowHeader の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.DataGridRowHeader> 要素の表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|MouseOver|CommonStates|マウスポインターが行の上に配置されます。|  
|MouseOver_CurrentRow|CommonStates|マウスポインターが行の上に配置され、行が現在の行になります。|  
|MouseOver_CurrentRow_Selected|CommonStates|マウスポインターが行の上に配置され、行が現在選択されています。|  
|MouseOver_EditingRow|CommonStates|マウスポインターが行の上に置かれ、行が編集モードになっています。|  
|MouseOver_Selected|CommonStates|マウスポインターが行の上に配置され、行が選択されます。|  
|MouseOver_Unfocused_CurrentRow_Selected|CommonStates|マウスポインターが行の上に配置されていて、行が最新で選択されており、フォーカスがありません。|  
|MouseOver_Unfocused_EditingRow|CommonStates|マウスポインターが行の上に配置されており、行が編集モードであり、フォーカスがありません。|  
|MouseOver_Unfocused_Selected|CommonStates|マウスポインターが行の上に配置され、行が選択されており、フォーカスがありません。|  
|Normal_CurrentRow|CommonStates|行は現在の行です。|  
|Normal_CurrentRow_Selected|CommonStates|行は現在の行であり、選択されています。|  
|Normal_EditingRow|CommonStates|行は編集モードです。|  
|Normal_Selected|CommonStates|行が選択されています。|  
|Unfocused_CurrentRow_Selected|CommonStates|行が現在の行であり、が選択されており、フォーカスがありません。|  
|Unfocused_EditingRow|CommonStates|行が編集モードであり、フォーカスがありません。|  
|Unfocused_Selected|CommonStates|行が選択されていますが、フォーカスがありません。|  
|InvalidFocused|ValidationStates|コントロールが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|コントロールが無効で、フォーカスがありません。|  
|Valid|ValidationStates|コントロールは有効です。|  
  
## <a name="datagridcolumnheaderspresenter-parts"></a>DataGridColumnHeadersPresenter パーツ  
 次の表に、<xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter> 要素の名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_FillerColumnHeader|<xref:System.Windows.Controls.Primitives.DataGridColumnHeader>|列ヘッダーのプレースホルダーです。|  
  
## <a name="datagridcolumnheaderspresenter-states"></a>DataGridColumnHeadersPresenter の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter> 要素の表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|InvalidFocused|ValidationStates|セルが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|セルが無効であり、フォーカスがありません。|  
|Valid|ValidationStates|セルが有効です。|  
  
## <a name="datagridcolumnheader-parts"></a>DataGridColumnHeader パーツ  
 次の表に、<xref:System.Windows.Controls.Primitives.DataGridColumnHeader> 要素の名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_LeftHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|列ヘッダーのサイズを左から変更するために使用される要素。|  
|PART_RightHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|列ヘッダーのサイズを右側から変更するために使用される要素。|  
  
## <a name="datagridcolumnheader-states"></a>DataGridColumnHeader の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.DataGridColumnHeader> 要素の表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|MouseOver|CommonStates|マウス ポインターがコントロール上に配置されます。|  
|押されている|CommonStates|コントロールが押されています。|  
|SortAscending|SortStates|列は昇順に並べ替えられます。|  
|SortDescending|SortStates|列は降順で並べ替えられます。|  
|［並び替えなし］|SortStates|列が並べ替えられていません。|  
|InvalidFocused|ValidationStates|コントロールが無効で、フォーカスがあります。|  
|InvalidUnfocused|ValidationStates|コントロールが無効で、フォーカスがありません。|  
|Valid|ValidationStates|コントロールは有効です。|  
  
## <a name="datagrid-controltemplate-example"></a>DataGrid ControlTemplate の例  
 次の例は、<xref:System.Windows.Controls.DataGrid> コントロールとそれに関連付けられている型の <xref:System.Windows.Controls.ControlTemplate> を定義する方法を示しています。  
  
 [!code-xaml[ControlTemplateExamples#DataGrid](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/datagrid.xaml#datagrid)]  
  
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
