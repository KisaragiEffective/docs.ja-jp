---
title: ComboBox のスタイルとテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- ComboBox [WPF], styles and templates
- states [WPF], ComboBox
- ControlTemplate [WPF], ComboBox
- styles [WPF], ComboBox
- templates [WPF], ComboBox
- parts [WPF], ComboBox
ms.assetid: b0662fa1-16d7-4320-b26b-c1804e565a44
ms.openlocfilehash: 887698bdaebf7bc5ddac8997167589d9fbd3dd4d
ms.sourcegitcommit: 42ed59871db1f29a32b3d8e7abeb20e6eceeda7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74960378"
---
# <a name="combobox-styles-and-templates"></a>ComboBox のスタイルとテンプレート
このトピックでは、<xref:System.Windows.Controls.ComboBox> コントロールのスタイルとテンプレートについて説明します。 <xref:System.Windows.Controls.ControlTemplate>の既定値を変更して外観を制御します。 詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。  
  
## <a name="combobox-parts"></a>ComboBox パーツ  
 次の表に、<xref:System.Windows.Controls.ComboBox> コントロールの名前付きの部分を示します。  
  
|のコンポーネント|の型|説明|  
|-|-|-|  
|PART_EditableTextBox|<xref:System.Windows.Controls.TextBox>|<xref:System.Windows.Controls.ComboBox>のテキストを格納します。|  
|PART_Popup|<xref:System.Windows.Controls.Primitives.Popup>|コンボボックス内の項目を含むドロップダウン。|  
  
 <xref:System.Windows.Controls.ComboBox>の <xref:System.Windows.Controls.ControlTemplate> を作成する場合、テンプレートに <xref:System.Windows.Controls.ScrollViewer>内の <xref:System.Windows.Controls.ItemsPresenter> が含まれている可能性があります。 (<xref:System.Windows.Controls.ItemsPresenter> には、<xref:System.Windows.Controls.ComboBox>内の各項目が表示されます。 <xref:System.Windows.Controls.ScrollViewer> では、コントロール内でのスクロールが有効になります)。  <xref:System.Windows.Controls.ItemsPresenter> が <xref:System.Windows.Controls.ScrollViewer>の直接の子でない場合は、<xref:System.Windows.Controls.ItemsPresenter> の名前を `ItemsPresenter`に指定する必要があります。  
  
## <a name="combobox-states"></a>コンボボックスの状態  
 次の表に、<xref:System.Windows.Controls.ComboBox> コントロールの状態を示します。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|Disabled|CommonStates|コントロールが無効になっています。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.ComboBox> コントロールの上にあります。|  
|フォーカスされている|FocusStates|コントロールにフォーカスがあります。|  
|フォーカスされていない|FocusStates|コントロールにフォーカスがありません。|  
|FocusedDropDown|FocusStates|<xref:System.Windows.Controls.ComboBox> のドロップダウンにフォーカスがあります。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
|Editable|EditStates|<xref:System.Windows.Controls.ComboBox.IsEditable%2A> プロパティが `true` です。|  
|不可能|EditStates|<xref:System.Windows.Controls.ComboBox.IsEditable%2A> プロパティが `false` です。|  
  
## <a name="comboboxitem-parts"></a>ComboBoxItem パーツ  
 <xref:System.Windows.Controls.ComboBoxItem> コントロールには、名前付きの部分がありません。  
  
## <a name="comboboxitem-states"></a>ComboBoxItem の状態  
 次の表に、<xref:System.Windows.Controls.ComboBoxItem> コントロールの状態を示します。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|Disabled|CommonStates|コントロールが無効になっています。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.ComboBoxItem> コントロールの上にあります。|  
|フォーカスされている|FocusStates|コントロールにフォーカスがあります。|  
|フォーカスされていない|FocusStates|コントロールにフォーカスがありません。|  
|選択された|SelectionStates|項目は現在選択されています。|  
|未選択|SelectionStates|この項目は選択されていません。|  
|SelectedUnfocused|SelectionStates|この項目は選択されていますが、フォーカスがありません。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="combobox-controltemplate-example"></a>ComboBox ControlTemplate の例  
 次の例は、<xref:System.Windows.Controls.ComboBox> コントロールとそれに関連付けられている型の <xref:System.Windows.Controls.ControlTemplate> を定義する方法を示しています。  
  
 [!code-xaml[ControlTemplateExamples#ComboBox](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/combobox.xaml#combobox)]  
  
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
