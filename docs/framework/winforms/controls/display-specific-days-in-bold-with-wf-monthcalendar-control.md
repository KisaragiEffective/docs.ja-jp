---
title: MonthCalendar コントロールを使用して特定の日を太字で表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- calendars [Windows Forms], displaying dates in bold
- examples [Windows Forms], calendar controls
- GetDayBold event
- MonthCalendar control [Windows Forms], dates displayed in bold
ms.assetid: 8b20db5b-8118-4825-90e8-2c45c186ac7d
ms.openlocfilehash: 377eb76f562fff20aae2136ddb7130516d2d76f7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745883"
---
# <a name="how-to-display-specific-days-in-bold-with-the-windows-forms-monthcalendar-control"></a>方法 : Windows フォームの MonthCalendar コントロールを使用して特定の日付を太字で表示する
Windows フォーム <xref:System.Windows.Forms.MonthCalendar> コントロールでは、単数形または繰り返しのいずれかの形式で、日付を太字で表示できます。 これは、休日や週末など、特別な日付に注意を引くために使用できます。  
  
 この機能は、3つのプロパティによって制御できます。 <xref:System.Windows.Forms.MonthCalendar.BoldedDates%2A> プロパティには、1つの日付が含まれます。 <xref:System.Windows.Forms.MonthCalendar.AnnuallyBoldedDates%2A> プロパティには、毎年太字で表示される日付が含まれています。 <xref:System.Windows.Forms.MonthCalendar.MonthlyBoldedDates%2A> プロパティには、毎月太字で表示される日付が含まれています。 これらの各プロパティには、<xref:System.DateTime> オブジェクトの配列が含まれています。 これらの一覧のいずれかから日付を追加または削除するには、<xref:System.DateTime> オブジェクトを追加または削除する必要があります。  
  
### <a name="to-make-a-date-appear-in-bold-type"></a>日付を太字で表示するには  
  
1. <xref:System.DateTime> オブジェクトを作成します。  
  
    ```vb  
    Dim myVacation1 As Date = New DateTime(2001, 6, 10)  
    Dim myVacation2 As Date = New DateTime(2001, 6, 17)  
    ```  
  
    ```csharp  
    DateTime myVacation1 = new DateTime(2001, 6, 10);  
    DateTime myVacation2 = new DateTime(2001, 6, 17);  
    ```  
  
    ```cpp  
    DateTime myVacation1 = DateTime(2001, 6, 10);  
    DateTime myVacation2 = DateTime(2001, 6, 17);  
    ```  
  
2. <xref:System.Windows.Forms.MonthCalendar> コントロールの <xref:System.Windows.Forms.MonthCalendar.AddBoldedDate%2A>、<xref:System.Windows.Forms.MonthCalendar.AddAnnuallyBoldedDate%2A>、または <xref:System.Windows.Forms.MonthCalendar.AddMonthlyBoldedDate%2A> メソッドを呼び出して、1つの日付を太字にします。  
  
    ```vb  
    MonthCalendar1.AddBoldedDate(myVacation1)  
    MonthCalendar1.AddBoldedDate(myVacation2)  
    ```  
  
    ```csharp  
    monthCalendar1.AddBoldedDate(myVacation1);  
    monthCalendar1.AddBoldedDate(myVacation2);  
    ```  
  
    ```cpp  
    monthCalendar1->AddBoldedDate(myVacation1);  
    monthCalendar1->AddBoldedDate(myVacation2);  
    ```  
  
     または  
  
     一連の日付を一度に太字にするには、<xref:System.DateTime> オブジェクトの配列を作成し、プロパティの1つに割り当てます。  
  
    ```vb  
    Dim VacationDates As DateTime() = {myVacation1, myVacation2}  
    MonthCalendar1.BoldedDates = VacationDates  
    ```  
  
    ```csharp  
    DateTime[] VacationDates = {myVacation1, myVacation2};  
    monthCalendar1.BoldedDates = VacationDates;  
    ```  
  
    ```cpp  
    Array<DateTime>^ VacationDates = {myVacation1, myVacation2};  
    monthCalendar1->BoldedDates = VacationDates;  
    ```  
  
### <a name="to-make-a-date-appear-in-the-regular-font"></a>標準フォントで日付を表示するには  
  
1. <xref:System.Windows.Forms.MonthCalendar.RemoveBoldedDate%2A>、<xref:System.Windows.Forms.MonthCalendar.RemoveAnnuallyBoldedDate%2A>、または <xref:System.Windows.Forms.MonthCalendar.RemoveMonthlyBoldedDate%2A> メソッドを呼び出して、1つの太字の日付を通常のフォントで表示します。  
  
    ```vb  
    MonthCalendar1.RemoveBoldedDate(myVacation1)  
    MonthCalendar1.RemoveBoldedDate(myVacation2)  
    ```  
  
    ```csharp  
    monthCalendar1.RemoveBoldedDate(myVacation1);  
    monthCalendar1.RemoveBoldedDate(myVacation2);  
    ```  
  
    ```cpp  
    monthCalendar1->RemoveBoldedDate(myVacation1);  
    monthCalendar1->RemoveBoldedDate(myVacation2);  
    ```  
  
     または  
  
     <xref:System.Windows.Forms.MonthCalendar.RemoveAllBoldedDates%2A>、<xref:System.Windows.Forms.MonthCalendar.RemoveAllAnnuallyBoldedDates%2A>、または <xref:System.Windows.Forms.MonthCalendar.RemoveAllMonthlyBoldedDates%2A> メソッドを呼び出して、3つのリストのいずれかから太字で表示されているすべての日付を削除します。  
  
    ```vb  
    MonthCalendar1.RemoveAllBoldedDates()  
    ```  
  
    ```csharp  
    monthCalendar1.RemoveAllBoldedDates();  
    ```  
  
    ```cpp  
    monthCalendar1->RemoveAllBoldedDates();  
    ```  
  
2. <xref:System.Windows.Forms.MonthCalendar.UpdateBoldedDates%2A> メソッドを呼び出して、フォントの外観を更新します。  
  
    ```vb  
    MonthCalendar1.UpdateBoldedDates()  
    ```  
  
    ```csharp  
    monthCalendar1.UpdateBoldedDates();  
    ```  
  
    ```cpp  
    monthCalendar1->UpdateBoldedDates();  
    ```  
  
## <a name="see-also"></a>参照

- [MonthCalendar コントロール](monthcalendar-control-windows-forms.md)
- [方法: Windows フォームの MonthCalendar コントロールで日付の範囲を選択する](how-to-select-a-range-of-dates-in-the-windows-forms-monthcalendar-control.md)
- [方法: Windows フォームの MonthCalendar コントロールの外観を変更する](how-to-change-monthcalendar-control-appearance.md)
- [方法: Windows フォームの MonthCalendar コントロールにおいて複数の月を表示する](display-more-than-one-month-wf-monthcalendar-control.md)
