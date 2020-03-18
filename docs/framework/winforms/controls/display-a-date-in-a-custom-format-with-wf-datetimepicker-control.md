---
title: DateTimePicker コントロールを使用してカスタム形式で日付を表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- DateTimePicker control [Windows Forms], display styles
- examples [Windows Forms], DateTimePicker control
- dates [Windows Forms], displaying in DateTimePicker control
ms.assetid: 39767691-2d2b-46b6-a663-b7901e581a6e
ms.openlocfilehash: a27dbe737b81af86c0ac50b791bcd87bafe05b4f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745932"
---
# <a name="how-to-display-a-date-in-a-custom-format-with-the-windows-forms-datetimepicker-control"></a>方法 : Windows フォームの DateTimePicker コントロールを使用してカスタム形式で日付を表示する
Windows フォーム <xref:System.Windows.Forms.DateTimePicker> コントロールを使用すると、コントロールの日付と時刻の表示形式を柔軟に設定できます。 <xref:System.Windows.Forms.DateTimePicker.Format%2A> プロパティを使用すると、<xref:System.Windows.Forms.DateTimePickerFormat>に示されている定義済みの形式から選択できます。 これらのいずれも目的に適していない場合は、<xref:System.Windows.Forms.DateTimePicker.CustomFormat%2A>に記載されている書式設定文字を使用して独自の書式スタイルを作成できます。  
  
### <a name="to-display-a-custom-format"></a>カスタム書式を表示するには  
  
1. <xref:System.Windows.Forms.DateTimePicker.Format%2A> プロパティを `DateTimePickerFormat.Custom` に設定します。  
  
2. <xref:System.Windows.Forms.DateTimePicker.CustomFormat%2A> プロパティを書式設定文字列に設定します。  
  
    ```vb  
    DateTimePicker1.Format = DateTimePickerFormat.Custom  
    ' Display the date as "Mon 27 Feb 2012".  
    DateTimePicker1.CustomFormat = "ddd dd MMM yyyy"  
    ```  
  
    ```csharp  
    dateTimePicker1.Format = DateTimePickerFormat.Custom;  
    // Display the date as "Mon 27 Feb 2012".  
    dateTimePicker1.CustomFormat = "ddd dd MMM yyyy";  
    ```  
  
    ```cpp  
    dateTimePicker1->Format = DateTimePickerFormat::Custom;  
    // Display the date as "Mon 27 Feb 2012".  
    dateTimePicker1->CustomFormat = "ddd dd MMM yyyy";  
    ```  
  
### <a name="to-add-text-to-the-formatted-value"></a>書式設定された値にテキストを追加するには  
  
1. "M" のような書式指定文字ではない任意の文字、または ":" のような区切り記号を使用するには、単一引用符を使用します。 たとえば、次の書式設定文字列では、英語 (米国) カルチャの現在の日付が "Today は、05:30:31 年3月 02, 2012" の形式で表示されます。  
  
    ```vb  
    DateTimePicker1.CustomFormat = "'Today is:' hh:mm:ss dddd MMMM dd, yyyy"  
    ```  
  
    ```csharp  
    dateTimePicker1.CustomFormat = "'Today is:' hh:mm:ss dddd MMMM dd, yyyy";  
    ```  
  
    ```cpp  
    dateTimePicker1->CustomFormat =  
       "'Today is:' hh:mm:ss dddd MMMM dd, yyyy";  
    ```  
  
     カルチャの設定によっては、単一引用符で囲まれていない文字は変更される可能性があります。 たとえば、上記の書式指定文字列は、現在の日付を英語 (米国) カルチャの "Today は05:30:31 年3月 02, 2012" の形式で表示します。 先頭のコロンは、"hh: mm: ss" のように区切り文字として使用されないため、単一引用符で囲まれていることに注意してください。 別のカルチャでは、"今日は: 05.30.31 金曜日3月 02, 2012" と表示される場合があります。  
  
## <a name="see-also"></a>参照

- [DateTimePicker コントロール](datetimepicker-control-windows-forms.md)
- [方法: Windows フォームの DateTimePicker コントロールを使用して日付を設定および取得する](how-to-set-and-return-dates-with-the-windows-forms-datetimepicker-control.md)
