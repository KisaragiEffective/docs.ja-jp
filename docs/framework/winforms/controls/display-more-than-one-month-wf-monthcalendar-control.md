---
title: MonthCalendar コントロールに複数の月を表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- calendars [Windows Forms], formatting display
- examples [Windows Forms], calendar controls
- calendars [Windows Forms], multiple months
- MonthCalendar control [Windows Forms], formatting display
ms.assetid: d197caa2-38a5-4cb4-acc3-562130c2ace3
ms.openlocfilehash: 5d3925bc19ddcd67742f0ab8b5b2e45530820f38
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745899"
---
# <a name="how-to-display-more-than-one-month-in-the-windows-forms-monthcalendar-control"></a>方法 : Windows フォームの MonthCalendar コントロールにおいて複数の月を表示する
Windows フォーム <xref:System.Windows.Forms.MonthCalendar> コントロールには、一度に最大12か月を表示できます。 既定では、コントロールに表示されるのは1か月のみですが、表示される月の数と、コントロール内での表示方法を指定できます。 カレンダーの寸法を変更すると、コントロールのサイズが変更されます。そのため、フォームには新しい寸法用に十分な余裕があることを確認してください。  
  
### <a name="to-display-multiple-months"></a>複数の月を表示するには  
  
- <xref:System.Windows.Forms.MonthCalendar.CalendarDimensions%2A> プロパティを、水平方向および垂直方向に表示する月数に設定します。  
  
    ```vb  
    MonthCalendar1.CalendarDimensions = New System.Drawing.Size (3,2)  
    ```  
  
    ```csharp  
    monthCalendar1.CalendarDimensions = new System.Drawing.Size (3,2);  
    ```  
  
    ```cpp  
    monthCalendar1->CalendarDimensions = System::Drawing::Size (3,2);  
    ```  
  
## <a name="see-also"></a>参照

- [MonthCalendar コントロール](monthcalendar-control-windows-forms.md)
- [方法: Windows フォームの MonthCalendar コントロールで日付の範囲を選択する](how-to-select-a-range-of-dates-in-the-windows-forms-monthcalendar-control.md)
- [方法: Windows フォームの MonthCalendar コントロールの外観を変更する](how-to-change-monthcalendar-control-appearance.md)
