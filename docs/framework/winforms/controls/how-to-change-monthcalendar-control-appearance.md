---
title: MonthCalendar コントロールの外観を変更する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- examples [Windows Forms], calendar controls
- MonthCalendar control [Windows Forms], formatting display
ms.assetid: d09b95c9-e108-4608-9b31-b9100c0677bf
ms.openlocfilehash: ded9059ede4ad03f637c0e697b880c41a9a8ba32
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746655"
---
# <a name="how-to-change-the-windows-forms-monthcalendar-controls-appearance"></a>方法 : Windows フォームの MonthCalendar コントロールの外観を変更する
Windows フォーム <xref:System.Windows.Forms.MonthCalendar> コントロールを使用すると、さまざまな方法でカレンダーの外観をカスタマイズできます。 たとえば、配色を設定し、週番号と現在の日付を表示または非表示にすることができます。  
  
### <a name="to-change-the-month-calendars-color-scheme"></a>月の予定表の配色を変更するには  
  
- <xref:System.Windows.Forms.MonthCalendar.TitleBackColor%2A>、<xref:System.Windows.Forms.MonthCalendar.TitleForeColor%2A>、<xref:System.Windows.Forms.MonthCalendar.TrailingForeColor%2A>などのプロパティを設定します。 <xref:System.Windows.Forms.MonthCalendar.TitleBackColor%2A> プロパティによって、曜日のフォントの色も決定されます。 <xref:System.Windows.Forms.MonthCalendar.TrailingForeColor%2A> プロパティは、表示される月または月の前と後の日付の色を決定します。  
  
    ```vb  
    MonthCalendar1.TitleBackColor = System.Drawing.Color.Blue  
    MonthCalendar1.TrailingForeColor = System.Drawing.Color.Red  
    MonthCalendar1.TitleForeColor = System.Drawing.Color.Yellow  
    ```  
  
    ```csharp  
    monthCalendar1.TitleBackColor = System.Drawing.Color.Blue;  
    monthCalendar1.TrailingForeColor = System.Drawing.Color.Red;  
    monthCalendar1.TitleForeColor = System.Drawing.Color.Yellow;  
    ```  
  
    ```cpp  
    monthCalendar1->TitleBackColor = System::Drawing::Color::Blue;  
    monthCalendar1->TrailingForeColor = System::Drawing::Color::Red;  
    monthCalendar1->TitleForeColor = System::Drawing::Color::Yellow;  
    ```  
  
    > [!NOTE]
    > Windows Vista 以降では、テーマによっては、一部のプロパティを設定しても予定表の外観が変更されない場合があります。 たとえば、Windows が Aero テーマを使用するように設定されている場合、<xref:System.Windows.Forms.MonthCalendar.BackColor%2A>、<xref:System.Windows.Forms.MonthCalendar.TitleBackColor%2A>、<xref:System.Windows.Forms.MonthCalendar.TitleForeColor%2A>、または <xref:System.Windows.Forms.MonthCalendar.TrailingForeColor%2A> プロパティを設定しても効果はありません。 これは、更新されたバージョンのカレンダーが、現在のオペレーティングシステムのテーマから実行時に生成された外観でレンダリングされるためです。 これらのプロパティを使用して、カレンダーの以前のバージョンを有効にする場合は、アプリケーションの視覚スタイルを無効にすることができます。 視覚スタイルを無効にすると、アプリケーション内の他のコントロールの外観と動作に影響を与える可能性があります。 Visual Basic で視覚スタイルを無効にするには、プロジェクトデザイナーを開き、[ **XP visual スタイルを有効に**する] チェックボックスをオフにします。 でC#視覚スタイルを無効にするには、Program.cs を開き、`Application.EnableVisualStyles();`をコメントアウトします。 視覚スタイルの詳細については、「 [Visual スタイルの有効化](/windows/desktop/controls/cookbook-overview)」を参照してください。  
  
### <a name="to-display-the-current-date-at-the-bottom-of-the-control"></a>コントロールの下部に現在の日付を表示するには  
  
- <xref:System.Windows.Forms.MonthCalendar.ShowToday%2A> プロパティを `true` に設定します。 次の例では、フォームがダブルクリックされたときの今日の日付の表示と非表示を切り替えます。  
  
    ```vb  
    Private Sub Form1_DoubleClick(ByVal sender As Object, _  
    ByVal e As System.EventArgs) Handles MyBase.DoubleClick  
       ' Toggle between True and False.  
       MonthCalendar1.ShowToday = Not MonthCalendar1.ShowToday  
    End Sub  
    ```  
  
    ```csharp  
    private void Form1_DoubleClick(object sender, System.EventArgs e)  
    {  
       // Toggle between True and False.  
       monthCalendar1.ShowToday = !monthCalendar1.ShowToday;  
    }  
    ```  
  
    ```cpp  
    private:  
       System::Void Form1_DoubleClick(System::Object ^  sender,  
          System::EventArgs ^  e)  
       {  
          // Toggle between True and False.  
          monthCalendar1->ShowToday = !monthCalendar1->ShowToday;  
       }  
    ```  
  
     (ビジュアルC#、ビジュアルC++)フォームのコンストラクターに次のコードを配置して、イベントハンドラーを登録します。  
  
    ```csharp  
    this.DoubleClick += new System.EventHandler(this.Form1_DoubleClick);  
    ```  
  
    ```cpp  
    this->DoubleClick += gcnew System::EventHandler(this,  
       &Form1::Form1_DoubleClick);  
    ```  
  
### <a name="to-display-week-numbers"></a>週番号を表示するには  
  
- <xref:System.Windows.Forms.MonthCalendar.ShowWeekNumbers%2A> プロパティを `true` に設定します。 このプロパティは、コードまたはプロパティウィンドウで設定できます。  
  
     週番号は、週の最初の曜日の左側にある別の列に表示されます。  
  
    ```vb  
    MonthCalendar1.ShowWeekNumbers = True  
    ```  
  
    ```csharp  
    monthCalendar1.ShowWeekNumbers = true;  
    ```  
  
    ```cpp  
    monthCalendar1->ShowWeekNumbers = true;  
    ```  
  
## <a name="see-also"></a>参照

- [MonthCalendar コントロール](monthcalendar-control-windows-forms.md)
- [方法: Windows フォームの MonthCalendar コントロールで日付の範囲を選択する](how-to-select-a-range-of-dates-in-the-windows-forms-monthcalendar-control.md)
- [方法: Windows フォームの MonthCalendar コントロールを使用して特定の日付を太字で表示する](display-specific-days-in-bold-with-wf-monthcalendar-control.md)
- [方法: Windows フォームの MonthCalendar コントロールにおいて複数の月を表示する](display-more-than-one-month-wf-monthcalendar-control.md)
