---
title: MonthCalendar コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- MonthCalendar
helpviewer_keywords:
- calendars [Windows Forms], Windows Forms controls
- calendar controls [Windows Forms], Windows Forms
- MonthCalendar control [Windows Forms], setting the first day of the week
ms.assetid: 788c5325-b721-44ec-95bf-9b680ba0f6a2
ms.openlocfilehash: a9b56164339d03b380a564b21855f6d94ec06af5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76734944"
---
# <a name="monthcalendar-control-overview-windows-forms"></a>MonthCalendar コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.MonthCalendar> コントロールは、ユーザーが日付情報を表示および設定するための直感的なグラフィカルインターフェイスを提供します。 このコントロールには、カレンダーが表示されます。このグリッドには、月の日付を含むグリッドが表示されます。選択した日付範囲が強調表示されます。 月のキャプションの両側にある矢印ボタンをクリックすると、別の月を選択できます。 同様の <xref:System.Windows.Forms.DateTimePicker> コントロールとは異なり、このコントロールでは複数の日付を選択できます。 <xref:System.Windows.Forms.DateTimePicker> コントロールの詳細については、「 [DateTimePicker control](datetimepicker-control-windows-forms.md)」を参照してください。  
  
## <a name="configuring-the-monthcalendar-control"></a>MonthCalendar コントロールの構成  
 <xref:System.Windows.Forms.MonthCalendar> コントロールの外観は、高度な構成が可能です。 既定では、今日の日付は丸で囲まれて表示され、グリッドの下部にも示されます。 この機能を変更するには、<xref:System.Windows.Forms.MonthCalendar.ShowToday%2A> プロパティと <xref:System.Windows.Forms.MonthCalendar.ShowTodayCircle%2A> プロパティを `false`に設定します。 また、<xref:System.Windows.Forms.MonthCalendar.ShowWeekNumbers%2A> プロパティを `true`に設定して、週番号をカレンダーに追加することもできます。 <xref:System.Windows.Forms.MonthCalendar.CalendarDimensions%2A> プロパティを設定することにより、複数の月を水平方向および垂直方向に表示できます。 既定では、日曜日は週の最初の曜日として表示されますが、<xref:System.Windows.Forms.MonthCalendar.FirstDayOfWeek%2A> プロパティを使用して任意の日を指定することもできます。  
  
 また、<xref:System.Windows.Forms.MonthCalendar.BoldedDates%2A>、<xref:System.Windows.Forms.MonthCalendar.AnnuallyBoldedDates%2A>、および <xref:System.Windows.Forms.MonthCalendar.MonthlyBoldedDates%2A> プロパティに <xref:System.DateTime> オブジェクトを追加することによって、特定の日付を1回だけ、または月単位で太字で表示するように設定することもできます。 詳細については、「[方法: Windows フォーム MonthCalendar コントロールを使用して特定の日付を太字で表示する](display-specific-days-in-bold-with-wf-monthcalendar-control.md)」を参照してください。  
  
 <xref:System.Windows.Forms.MonthCalendar> コントロールのキープロパティは、コントロールで選択されている日付の範囲 <xref:System.Windows.Forms.MonthCalendar.SelectionRange%2A>です。 <xref:System.Windows.Forms.MonthCalendar.SelectionRange%2A> 値は、<xref:System.Windows.Forms.MonthCalendar.MaxSelectionCount%2A> プロパティで設定できる最大日数を超えることはできません。 ユーザーが選択できる最も早い日と最後の日付は、<xref:System.Windows.Forms.MonthCalendar.MaxDate%2A> プロパティと <xref:System.Windows.Forms.MonthCalendar.MinDate%2A> プロパティによって決まります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.MonthCalendar>
- [MonthCalendar コントロール](monthcalendar-control-windows-forms.md)
