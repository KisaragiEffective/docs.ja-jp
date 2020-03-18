---
title: DateTimePicker コントロールで日付を設定して返す
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- dates [Windows Forms], setting in DateTimePicker
- DateTimePicker control [Windows Forms], setting and returning dates
- examples [Windows Forms], DateTimePicker control
ms.assetid: a8a48d68-e4b5-426e-9764-51230fc9acd2
ms.openlocfilehash: 1e0aa58e98748ccde9411f0f4871adbae3a5f14d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747103"
---
# <a name="how-to-set-and-return-dates-with-the-windows-forms-datetimepicker-control"></a>方法 : Windows フォームの DateTimePicker コントロールを使用して日付を設定および取得する
Windows フォーム <xref:System.Windows.Forms.DateTimePicker> コントロールで現在選択されている日付または時刻は、<xref:System.Windows.Forms.DateTimePicker.Value%2A> プロパティによって決定されます。 コントロールが表示される前 (デザイン時またはフォームの <xref:System.Windows.Forms.DateTimePicker.Value%2A> イベントなど) に <xref:System.Windows.Forms.Form.Load> プロパティを設定して、コントロールで最初に選択される日付を決定します。 既定では、コントロールの <xref:System.Windows.Forms.DateTimePicker.Value%2A> は現在の日付に設定されます。 コントロールの <xref:System.Windows.Forms.DateTimePicker.Value%2A> をコードで変更するには、フォームでコントロールが新しい設定を反映するよう自動的に更新されます。  
  
 <xref:System.Windows.Forms.DateTimePicker.Value%2A> プロパティは、値として <xref:System.DateTime> 構造を返します。 表示される日付に関する特定の情報を返す <xref:System.DateTime> 構造のプロパティはいくつかあります。 これらのプロパティは値を返す貯めにのみ使用でき、値の設定には使用しません。  
  
- 日付値の場合、<xref:System.DateTime.Month%2A>、<xref:System.DateTime.Day%2A>、および <xref:System.DateTime.Year%2A> の各プロパティは、選択した日付の時間単位の整数値を返します。 <xref:System.DateTime.DayOfWeek%2A> プロパティは、選択した日の曜日を示す値を返します (指定できる値は <xref:System.DayOfWeek> 列挙型にリストされます)。  
  
- 時間値の場合、<xref:System.DateTime.Hour%2A>、<xref:System.DateTime.Minute%2A>、<xref:System.DateTime.Second%2A>、および <xref:System.DateTime.Millisecond%2A> の各プロパティは、時間単位の整数値を返します。 時間を表示するようにコントロールを構成する方法については、「[方法: DateTimePicker コントロールを使用](how-to-display-time-with-the-datetimepicker-control.md)して時刻を表示する」を参照してください。  
  
### <a name="to-set-the-date-and-time-value-of-the-control"></a>コントロールの日付と時刻の値を設定するには  
  
- <xref:System.Windows.Forms.DateTimePicker.Value%2A> プロパティを日付または時刻の値に設定します。  
  
    ```vb  
    DateTimePicker1.Value = New DateTime(2001, 10, 20)  
    ```  
  
    ```csharp  
    dateTimePicker1.Value = new DateTime(2001, 10, 20);  
    ```  
  
    ```cpp  
    dateTimePicker1->Value = DateTime(2001, 10, 20);  
    ```  
  
### <a name="to-return-the-date-and-time-value"></a>日付と時刻の値を返すには  
  
- <xref:System.Windows.Forms.DateTimePicker.Text%2A> プロパティを呼び出して、コントロールで書式設定する全体の値を返すか、または <xref:System.Windows.Forms.DateTimePicker.Value%2A> プロパティの適切なメソッドを呼び出して、値の一部を返します。 <xref:System.Windows.Forms.DateTimePicker.ToString%2A> を使用して、ユーザーに表示される文字列に情報を変換します。  
  
    ```vb  
    MessageBox.Show("The selected value is ", DateTimePicker1.Text)  
    MessageBox.Show("The day of the week is ",   
       DateTimePicker1.Value.DayOfWeek.ToString)  
    MessageBox.Show("Millisecond is: ",   
       DateTimePicker1.Value.Millisecond.ToString)  
    ```  
  
    ```csharp  
    MessageBox.Show ("The selected value is " +   
       dateTimePicker1.Text);  
    MessageBox.Show ("The day of the week is " +   
       dateTimePicker1.Value.DayOfWeek.ToString());  
    MessageBox.Show("Millisecond is: " +   
       dateTimePicker1.Value.Millisecond.ToString());  
    ```  
  
    ```cpp  
    MessageBox::Show (String::Concat("The selected value is ",  
       dateTimePicker1->Text));  
    MessageBox::Show (String::Concat("The day of the week is ",  
       dateTimePicker1->Value.DayOfWeek.ToString()));  
    MessageBox::Show(String::Concat("Millisecond is: ",  
       dateTimePicker1->Value.Millisecond.ToString()));  
    ```  
  
## <a name="see-also"></a>参照

- [DateTimePicker コントロール](datetimepicker-control-windows-forms.md)
- [方法: Windows フォームの DateTimePicker コントロールを使用してカスタム形式で日付を表示する](display-a-date-in-a-custom-format-with-wf-datetimepicker-control.md)
