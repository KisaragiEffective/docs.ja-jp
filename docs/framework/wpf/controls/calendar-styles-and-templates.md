---
title: カレンダーのスタイルとテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- styles [WPF], Calendar
- templates [WPF], Calendar
- states [WPF], Calendar
- parts [WPF], Calendar
- Calendar [WPF], styles and templates
- ControlTemplate [WPF], Calendar
ms.assetid: f4fcf046-7a8f-41b8-b5a8-534b64e0345c
ms.openlocfilehash: 64cb62a3459a3eeea6aa5e91b433a58a88ab08ea
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283555"
---
# <a name="calendar-styles-and-templates"></a>カレンダーのスタイルとテンプレート
このトピックでは、<xref:System.Windows.Controls.Calendar> コントロールのスタイルとテンプレートについて説明します。 <xref:System.Windows.Controls.ControlTemplate>の既定値を変更して外観を制御します。 詳細については、「[コントロールのテンプレートを作成する](../../../desktop-wpf/themes/how-to-create-apply-template.md)」を参照してください。  
  
## <a name="calendar-parts"></a>予定表のパーツ  
 次の表に、<xref:System.Windows.Controls.Calendar> コントロールの名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_CalendarItem|<xref:System.Windows.Controls.Primitives.CalendarItem>|<xref:System.Windows.Controls.Calendar>に現在表示されている月または年。|  
|PART_Root|<xref:System.Windows.Controls.Panel>|<xref:System.Windows.Controls.Primitives.CalendarItem>を含むパネル。|  
  
## <a name="calendar-states"></a>予定表の状態  
 次の表は、<xref:System.Windows.Controls.Calendar> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|----------------------|---------------------------|-----------------|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="calendaritem-parts"></a>CalendarItem の部分  
 次の表に、<xref:System.Windows.Controls.Primitives.CalendarItem> コントロールの名前付きの部分を示します。  
  
|要素|種類|説明|  
|-|-|-|  
|PART_Root|<xref:System.Windows.FrameworkElement>|コントロールのルート。|  
|PART_PreviousButton|<xref:System.Windows.Controls.Button>|クリックされたときに予定表の前のページを表示するボタン。|  
|PART_NextButton|<xref:System.Windows.Controls.Button>|クリックされたときに予定表の次のページを表示するボタン。|  
|PART_HeaderButton|<xref:System.Windows.Controls.Button>|月モード、年モード、および10月モードの切り替えを可能にするボタン。|  
|PART_MonthView|<xref:System.Windows.Controls.Grid>|月モードでコンテンツをホストします。|  
|PART_YearView|<xref:System.Windows.Controls.Grid>|年または10年単位のモードでコンテンツをホストします。|  
|PART_DisabledVisual|<xref:System.Windows.FrameworkElement>|無効化された状態のオーバーレイ。|  
|Dayタイトルテンプレート|<xref:System.Windows.DataTemplate>|ビジュアル構造を説明する <xref:System.Windows.DataTemplate>。|  
  
## <a name="calendaritem-states"></a>CalendarItem の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.CalendarItem> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|通常の状態|CommonStates|既定の状態です。|  
|無効状態|CommonStates|<xref:System.Windows.UIElement.IsEnabled%2A> プロパティが `false`場合のカレンダーの状態。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="calendardaybutton-parts"></a>CalendarDayButton パーツ  
 <xref:System.Windows.Controls.Primitives.CalendarDayButton> コントロールには、名前付きの部分がありません。  
  
## <a name="calendardaybutton-states"></a>CalendarDayButton の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.CalendarDayButton> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|無効|CommonStates|<xref:System.Windows.Controls.Primitives.CalendarDayButton> が無効になっています。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.Primitives.CalendarDayButton>上に配置されています。|  
|押されている|CommonStates|<xref:System.Windows.Controls.Primitives.CalendarDayButton> が押されています。|  
|Selected|SelectionStates|ボタンが選択されています。|  
|未選択|SelectionStates|このボタンは選択されていません。|  
|CalendarButtonFocused|CalendarButtonFocusStates|ボタンにフォーカスがあります。|  
|CalendarButtonUnfocused|CalendarButtonFocusStates|ボタンにフォーカスがありません。|  
|フォーカスされている|FocusStates|ボタンにフォーカスがあります。|  
|フォーカスされていない|FocusStates|ボタンにフォーカスがありません。|  
|Active|ActiveStates|このボタンはアクティブです。|  
|Inactive|ActiveStates|ボタンが非アクティブです。|  
|RegularDay|DayStates|このボタンは <xref:System.DateTime.Today%2A?displayProperty=nameWithType>を表していません。|  
|今日|DayStates|このボタンは <xref:System.DateTime.Today%2A?displayProperty=nameWithType>を表します。|  
|NormalDay|BlackoutDayStates|このボタンは、選択できる日を表します。|  
|ブラックアウト日|BlackoutDayStates|このボタンは、選択できない日を表します。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="calendarbutton-parts"></a>CalendarButton パーツ  
 <xref:System.Windows.Controls.Primitives.CalendarButton> コントロールには、名前付きの部分がありません。  
  
## <a name="calendarbutton-states"></a>CalendarButton の状態  
 次の表は、<xref:System.Windows.Controls.Primitives.CalendarButton> コントロールの表示状態を示しています。  
  
|VisualState 名|VisualStateGroup 名|説明|  
|-|-|-|  
|標準|CommonStates|既定の状態です。|  
|無効|CommonStates|<xref:System.Windows.Controls.Primitives.CalendarButton> が無効になっています。|  
|MouseOver|CommonStates|マウスポインターが <xref:System.Windows.Controls.Primitives.CalendarButton>上に配置されています。|  
|押されている|CommonStates|<xref:System.Windows.Controls.Primitives.CalendarButton> が押されています。|  
|Selected|SelectionStates|ボタンが選択されています。|  
|未選択|SelectionStates|このボタンは選択されていません。|  
|CalendarButtonFocused|CalendarButtonFocusStates|ボタンにフォーカスがあります。|  
|CalendarButtonUnfocused|CalendarButtonFocusStates|ボタンにフォーカスがありません。|  
|フォーカスされている|FocusStates|ボタンにフォーカスがあります。|  
|フォーカスされていない|FocusStates|ボタンにフォーカスがありません。|  
|Active|ActiveStates|このボタンはアクティブです。|  
|Inactive|ActiveStates|ボタンが非アクティブです。|  
|Valid|ValidationStates|コントロールは <xref:System.Windows.Controls.Validation> クラスを使用し、<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは `false`ます。|  
|InvalidFocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがある `true` です。|  
|InvalidUnfocused|ValidationStates|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 添付プロパティは、コントロールにフォーカスがない `true` です。|  
  
## <a name="calendar-controltemplate-example"></a>Calendar ControlTemplate の例  
 次の例は、<xref:System.Windows.Controls.Calendar> コントロールとそれに関連付けられている型の <xref:System.Windows.Controls.ControlTemplate> を定義する方法を示しています。  
  
 [!code-xaml[ControlTemplateExamples#Calendar](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/calendar.xaml#calendar)]  
  
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
