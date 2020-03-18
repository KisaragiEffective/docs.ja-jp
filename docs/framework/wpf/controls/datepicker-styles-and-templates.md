---
title: DatePicker のスタイルとテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- ControlTemplate [WPF], DatePicker
- DatePicker [WPF], styles and templates
- templates [WPF], DatePicker
- parts [WPF], DatePicker
- styles [WPF], DatePicker
- states [WPF], DatePicker
ms.assetid: c430a657-692f-44bd-a549-2341f92d6115
ms.openlocfilehash: 002d1c3271827239dcd3a319621f66fb5bc68d4e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283772"
---
# <a name="datepicker-styles-and-templates"></a>DatePicker のスタイルとテンプレート
このトピックでは、<xref:System.Windows.Controls.DatePicker> コントロールのスタイルとテンプレートについて説明します。 <xref:System.Windows.Controls.ControlTemplate>の既定値を変更して外観を制御します。 詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。  
  
## <a name="datepicker-parts"></a>DatePicker パーツ  
 次の表に、<xref:System.Windows.Controls.DatePicker> コントロールの名前付きの部分を示します。  
  
|要素|型|説明|  
|-|-|-|  
|PART_Root|<xref:System.Windows.Controls.Grid>|コントロールのルート。|  
|PART_Button|<xref:System.Windows.Controls.Button>|<xref:System.Windows.Controls.Calendar>を開いて閉じるボタン。|  
|PART_TextBox|<xref:System.Windows.Controls.Primitives.DatePickerTextBox>|日付を入力できるテキストボックス。|  
|PART_Popup|<xref:System.Windows.Controls.Primitives.Popup>|<xref:System.Windows.Controls.DatePicker> コントロールのポップアップ。|  
  
## <a name="datepicker-states"></a>DatePicker の状態  
 次の表は、<xref:System.Windows.Controls.DatePicker> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|Disabled|CommonStates|<xref:System.Windows.Controls.DatePicker> が無効になっています。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="datepickertextbox-parts"></a>DatePickerTextBox パーツ  
 次の表に、<xref:System.Windows.Controls.Primitives.DatePickerTextBox> コントロールの名前付きの部分を示します。  
  
|要素|型|説明|  
|-|-|-|  
|PART_Watermark|<xref:System.Windows.Controls.ContentControl>|<xref:System.Windows.Controls.DatePicker>の最初のテキストを含む要素。|  
|PART_ContentElement|<xref:System.Windows.FrameworkElement>|<xref:System.Windows.FrameworkElement>を含むことができるビジュアル要素。 <xref:System.Windows.Controls.TextBox> のテキストがこの要素に表示されます。|  
  
## <a name="datepickertextbox-states"></a>DatePickerTextBox の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.DatePickerTextBox> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|Disabled|CommonStates|<xref:System.Windows.Controls.Primitives.DatePickerTextBox> が無効になっています。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.Primitives.DatePickerTextBox>上に配置されています。|  
|ReadOnly|CommonStates|ユーザーは、<xref:System.Windows.Controls.Primitives.DatePickerTextBox>内のテキストを変更することはできません。|  
|フォーカスされている|FocusStates|コントロールにフォーカスがあります。|  
|フォーカスされていない|FocusStates|コントロールにフォーカスがありません。|  
|目標|WatermarkStates|コントロールには、最初のテキストが表示されます。  <xref:System.Windows.Controls.Primitives.DatePickerTextBox> は、ユーザーがテキストを入力しなかった場合、または日付を選択した場合の状態になります。|  
|Unwatermarked|WatermarkStates|ユーザーが <xref:System.Windows.Controls.Primitives.DatePickerTextBox> にテキストを入力したか、<xref:System.Windows.Controls.DatePicker>で日付を選択しました。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティが `true`、コントロールにフォーカスがあります。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティが `true`、コントロールにフォーカスがありません。|  
  
## <a name="datepicker-controltemplate-example"></a>DatePicker ControlTemplate の例  
 次の例は、<xref:System.Windows.Controls.DatePicker> コントロールの <xref:System.Windows.Controls.ControlTemplate> を定義する方法を示しています。  
  
 [!code-xaml[ControlTemplateExamples#DatePicker](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/datepicker.xaml#datepicker)]  
  
 前の例では、次のリソースの 1 つ以上を使用します。  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 完全なサンプルについては、[Styling with ControlTemplates Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating)を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [コントロールのスタイルとテンプレート](control-styles-and-templates.md)
- [コントロールのカスタマイズ](control-customization.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)
