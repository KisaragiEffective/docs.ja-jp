---
title: MonthCalendar コントロールで日付の範囲を選択する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- dates [Windows Forms], selecting range in calendar controls
- examples [Windows Forms], calendar controls
- calendars [Windows Forms], selecting date range
- MonthCalendar control [Windows Forms], selecting date range
ms.assetid: 95d9ab95-b0f8-4c19-9f63-b5cd4593a5d0
ms.openlocfilehash: bda96af21a8f86a54d5c0fe0204546b980076d26
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732900"
---
# <a name="how-to-select-a-range-of-dates-in-the-windows-forms-monthcalendar-control"></a>方法 : Windows フォームの MonthCalendar コントロールで日付の範囲を選択する
Windows フォーム <xref:System.Windows.Forms.MonthCalendar> 制御の重要な機能は、ユーザーが日付の範囲を選択できることです。 この機能は、ユーザーが単一の日付/時刻値を選択できるようにする、<xref:System.Windows.Forms.DateTimePicker> コントロールの日付選択機能を改善したものです。 <xref:System.Windows.Forms.MonthCalendar> コントロールのプロパティを使用して、日付の範囲を設定したり、ユーザーが設定した範囲を取得したりできます。 次のコード例は、選択範囲を設定する方法を示しています。  
  
### <a name="to-select-a-range-of-dates"></a>日付の範囲を選択するには  
  
1. 範囲内の最初と最後の日付を表す <xref:System.DateTime> オブジェクトを作成します。  
  
    ```vb  
    Dim projectStart As Date = New DateTime(2001, 2, 13)  
    Dim projectEnd As Date = New DateTime(2001, 2, 28)  
    ```  
  
    ```csharp  
    DateTime projectStart = new DateTime(2001, 2, 13);  
    DateTime projectEnd = new DateTime(2001, 2, 28);  
    ```  
  
    ```cpp  
    DateTime projectStart = DateTime(2001, 2, 13);  
    DateTime projectEnd = DateTime(2001, 2, 28);  
    ```  
  
2. <xref:System.Windows.Forms.MonthCalendar.SelectionRange%2A> プロパティを設定します。  
  
    ```vb  
    MonthCalendar1.SelectionRange = New SelectionRange(projectStart, projectEnd)  
    ```  
  
    ```csharp  
    monthCalendar1.SelectionRange = new SelectionRange(projectStart, projectEnd);  
    ```  
  
    ```cpp  
    monthCalendar1->SelectionRange = gcnew  
       SelectionRange(projectStart, projectEnd);  
    ```  
  
     または  
  
     <xref:System.Windows.Forms.MonthCalendar.SelectionStart%2A> と <xref:System.Windows.Forms.MonthCalendar.SelectionEnd%2A> プロパティを設定します。  
  
    ```vb  
    MonthCalendar1.SelectionStart = projectStart  
    MonthCalendar1.SelectionEnd = projectEnd  
    ```  
  
    ```csharp  
    monthCalendar1.SelectionStart = projectStart;  
    monthCalendar1.SelectionEnd = projectEnd;  
    ```  
  
    ```cpp  
    monthCalendar1->SelectionStart = projectStart;  
    monthCalendar1->SelectionEnd = projectEnd;  
    ```  
  
## <a name="see-also"></a>参照

- [MonthCalendar コントロール](monthcalendar-control-windows-forms.md)
- [方法: Windows フォームの MonthCalendar コントロールの外観を変更する](how-to-change-monthcalendar-control-appearance.md)
- [方法: Windows フォームの MonthCalendar コントロールを使用して特定の日付を太字で表示する](display-specific-days-in-bold-with-wf-monthcalendar-control.md)
- [方法: Windows フォームの MonthCalendar コントロールにおいて複数の月を表示する](display-more-than-one-month-wf-monthcalendar-control.md)
