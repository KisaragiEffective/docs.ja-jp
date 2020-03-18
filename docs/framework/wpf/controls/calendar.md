---
title: 予定表
ms.date: 03/30/2017
helpviewer_keywords:
- controls [WPF], Calendar
- Calendar control [WPF]
ms.assetid: ee844e4a-eefe-48e2-bd0d-1d82cc5e960b
ms.openlocfilehash: e99a716e7ca8f7b2c9ed11543f37e0b8cb7422a6
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460810"
---
# <a name="calendar"></a>予定表
カレンダーを使用すると、ユーザーは視覚的な予定表の表示を使用して日付を選択できます。  
  
 <xref:System.Windows.Controls.Calendar> コントロールは、単独で使用することも、<xref:System.Windows.Controls.DatePicker> コントロールのドロップダウンの部分として使用することもできます。 詳細については、「<xref:System.Windows.Controls.DatePicker>」を参照してください。  
  
 次の図は、2つの <xref:System.Windows.Controls.Calendar> コントロールを示しています。1つは選択とブラックアウトの日付で、もう1つはありません。  
  
 ![カレンダーコントロール](./media/ndp-calendarcontrols.png "NDP_CalendarControls")  
予定表コントロール  
  
 次の表では、通常、<xref:System.Windows.Controls.Calendar>に関連付けられているタスクについて説明します。  
  
|タスク|実装|  
|----------|--------------------|  
|選択できない日付を指定してください。|<xref:System.Windows.Controls.Calendar.BlackoutDates%2A> プロパティを使用します。|  
|<xref:System.Windows.Controls.Calendar> に、月、年、または10年のいずれかを表示します。|<xref:System.Windows.Controls.Calendar.DisplayMode%2A> プロパティを Month、Year、または10年に設定します。|  
|ユーザーが日付、日付の範囲、または複数の日付範囲を選択できるかどうかを指定します。|<xref:System.Windows.Controls.Calendar.SelectionMode%2A>を使用します。|  
|<xref:System.Windows.Controls.Calendar> に表示する日付の範囲を指定します。|<xref:System.Windows.Controls.Calendar.DisplayDateStart%2A> プロパティおよび <xref:System.Windows.Controls.Calendar.DisplayDateEnd%2A> プロパティを使用します。|  
|現在の日付を強調表示するかどうかを指定します。|<xref:System.Windows.Controls.Calendar.IsTodayHighlighted%2A> プロパティを使用します。 既定では、<xref:System.Windows.Controls.Calendar.IsTodayHighlighted%2A> は `true`です。|  
|<xref:System.Windows.Controls.Calendar>のサイズを変更します。|<xref:System.Windows.Controls.Viewbox> を使用するか、<xref:System.Windows.FrameworkElement.LayoutTransform%2A> プロパティを <xref:System.Windows.Media.ScaleTransform>に設定します。 <xref:System.Windows.Controls.Calendar>の <xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> のプロパティを設定した場合、実際のカレンダーのサイズは変更されないことに注意してください。|  
  
 <xref:System.Windows.Controls.Calendar> コントロールは、マウスまたはキーボードのいずれかを使用して基本的なナビゲーションを提供します。 次の表は、キーボードナビゲーションの概要を示しています。  
  
|キーの組み合わせ|<xref:System.Windows.Controls.Calendar.DisplayMode%2A>|操作|  
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|------------|  
|ボタン|<xref:System.Windows.Controls.CalendarMode.Month>|<xref:System.Windows.Controls.Calendar.SelectionMode%2A> プロパティが <xref:System.Windows.Controls.CalendarSelectionMode.None>に設定されていない場合、<xref:System.Windows.Controls.Calendar.SelectedDate%2A> プロパティを変更します。|  
|ボタン|<xref:System.Windows.Controls.CalendarMode.Year>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A> プロパティの月を変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されないことに注意してください。|  
|ボタン|<xref:System.Windows.Controls.CalendarMode.Decade>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A>の年を変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されないことに注意してください。|  
|SHIFT + 方向キー|<xref:System.Windows.Controls.CalendarMode.Month>|<xref:System.Windows.Controls.Calendar.SelectionMode%2A> が <xref:System.Windows.Controls.CalendarSelectionMode.SingleDate> または <xref:System.Windows.Controls.CalendarSelectionMode.None>に設定されていない場合、は選択された日付の範囲を拡張します。|  
|ホーム|<xref:System.Windows.Controls.CalendarMode.Month>|<xref:System.Windows.Controls.Calendar.SelectedDate%2A> を現在の月の最初の日付に変更します。|  
|ホーム|<xref:System.Windows.Controls.CalendarMode.Year>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A> の月をその年の最初の月に変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されません。|  
|ホーム|<xref:System.Windows.Controls.CalendarMode.Decade>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A> の年を10年の最初の年に変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されません。|  
|End|<xref:System.Windows.Controls.CalendarMode.Month>|<xref:System.Windows.Controls.Calendar.SelectedDate%2A> を現在の月の最後の日付に変更します。|  
|End|<xref:System.Windows.Controls.CalendarMode.Year>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A> の月をその年の最後の月に変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されません。|  
|End|<xref:System.Windows.Controls.CalendarMode.Decade>|<xref:System.Windows.Controls.Calendar.DisplayDate%2A> の年を10年の最後の年に変更します。 <xref:System.Windows.Controls.Calendar.SelectedDate%2A> は変更されません。|  
|Ctrl + ↑|どれでも可|次に大きい <xref:System.Windows.Controls.Calendar.DisplayMode%2A>に切り替えます。 <xref:System.Windows.Controls.Calendar.DisplayMode%2A> が既に <xref:System.Windows.Controls.CalendarMode.Decade>ている場合は、何も行われません。|  
|Ctrl +↓|どれでも可|次に小さい <xref:System.Windows.Controls.Calendar.DisplayMode%2A>に切り替えます。 <xref:System.Windows.Controls.Calendar.DisplayMode%2A> が既に <xref:System.Windows.Controls.CalendarMode.Month>ている場合は、何も行われません。|  
|SPACE または ENTER|<xref:System.Windows.Controls.CalendarMode.Year> または <xref:System.Windows.Controls.CalendarMode.Decade>|フォーカスされている項目で表される <xref:System.Windows.Controls.CalendarMode.Month> または <xref:System.Windows.Controls.CalendarMode.Year> に <xref:System.Windows.Controls.Calendar.DisplayMode%2A> を切り替えます。|  
  
## <a name="see-also"></a>関連項目

- [コントロール](index.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
