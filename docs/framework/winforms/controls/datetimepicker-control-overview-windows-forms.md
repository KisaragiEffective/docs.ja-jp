---
title: DateTimePicker コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- DateTimePicker
helpviewer_keywords:
- DateTimePicker control [Windows Forms], about
- date and time picker controls
ms.assetid: 501af106-e9fc-4efc-b9b3-c9d8dcaf8c5c
ms.openlocfilehash: 63271dd91116c1f68d426f3ed5aa710644ded928
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746931"
---
# <a name="datetimepicker-control-overview-windows-forms"></a>DateTimePicker コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.DateTimePicker> コントロールを使用すると、ユーザーは日付または時刻の一覧から1つの項目を選択できます。 日付を表すために使用した場合、2つの部分に表示されます。テキストで日付が表示されたドロップダウンリストと、一覧の横にある下向き矢印をクリックしたときに表示されるグリッドです。 グリッドは <xref:System.Windows.Forms.MonthCalendar> コントロールのようになります。これは、複数の日付を選択するために使用できます。 <xref:System.Windows.Forms.MonthCalendar> コントロールの詳細については、「 [MonthCalendar control の概要](monthcalendar-control-overview-windows-forms.md)」を参照してください。  
  
## <a name="key-properties"></a>キー プロパティ  
 <xref:System.Windows.Forms.DateTimePicker> を日付ではなく、選択した時間の制御として表示したい場合は、<xref:System.Windows.Forms.DateTimePicker.ShowUpDown%2A> プロパティを `true` に設定し、<xref:System.Windows.Forms.DateTimePicker.Format%2A> プロパティを <xref:System.Windows.Forms.DateTimePickerFormat.Time>に設定します。 詳細について[は、「方法: DateTimePicker コントロールを使用して時刻を表示する](how-to-display-time-with-the-datetimepicker-control.md)」を参照してください。  
  
 <xref:System.Windows.Forms.DateTimePicker.ShowCheckBox%2A> プロパティが `true`に設定されている場合、コントロールで選択されている日付の横にチェックボックスが表示されます。 チェックボックスをオンにすると、選択した日付/時刻値を更新できます。 このチェックボックスが空の場合、値は使用できません。  
  
 コントロールの <xref:System.Windows.Forms.DateTimePicker.MaxDate%2A> プロパティと <xref:System.Windows.Forms.DateTimePicker.MinDate%2A> プロパティによって、日付と時刻の範囲が決まります。 <xref:System.Windows.Forms.DateTimePicker.Value%2A> プロパティには、コントロールが設定されている現在の日付と時刻が格納されます。 詳細については、「[方法: Windows フォーム DateTimePicker コントロールを使用して日付を設定して返す](how-to-set-and-return-dates-with-the-windows-forms-datetimepicker-control.md)」を参照してください。 値は、<xref:System.Windows.Forms.DateTimePicker.Format%2A> プロパティ (<xref:System.Windows.Forms.DateTimePickerFormat.Long>、<xref:System.Windows.Forms.DateTimePickerFormat.Short>、<xref:System.Windows.Forms.DateTimePickerFormat.Time>、または <xref:System.Windows.Forms.DateTimePickerFormat.Custom>) によって設定される4つの形式で表示できます。 カスタム形式を選択した場合は、<xref:System.Windows.Forms.DateTimePicker.CustomFormat%2A> プロパティを適切な文字列に設定する必要があります。 詳細については、「[方法: Windows フォーム DateTimePicker コントロールを使用してカスタム書式で日付を表示する](display-a-date-in-a-custom-format-with-wf-datetimepicker-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [方法: Windows フォームの DateTimePicker コントロールを使用してカスタム形式で日付を表示する](display-a-date-in-a-custom-format-with-wf-datetimepicker-control.md)
- [方法: Windows フォームの DateTimePicker コントロールを使用して日付を設定および取得する](how-to-set-and-return-dates-with-the-windows-forms-datetimepicker-control.md)
