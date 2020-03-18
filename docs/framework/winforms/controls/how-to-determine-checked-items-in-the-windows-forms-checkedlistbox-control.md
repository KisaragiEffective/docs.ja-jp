---
title: CheckedListBox コントロールでチェックされている項目を確認する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- check boxes [Windows Forms], determining checked state
- CheckedListBox control [Windows Forms], determining checked state
ms.assetid: 178b477d-27c9-489c-8914-44a9623a4d41
ms.openlocfilehash: 5854f7e6be759daeb604458ea8554d3c98ed39c2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743241"
---
# <a name="how-to-determine-checked-items-in-the-windows-forms-checkedlistbox-control"></a>方法 : Windows フォーム CheckedListBox コントロールでオンになっている項目を判断する
Windows フォーム <xref:System.Windows.Forms.CheckedListBox> コントロールにデータを表示する場合は、<xref:System.Windows.Forms.CheckedListBox.CheckedItems%2A> プロパティに格納されているコレクションを反復処理するか、<xref:System.Windows.Forms.CheckedListBox.GetItemChecked%2A> メソッドを使用してリストをステップ実行し、どの項目がチェックされるかを判断します。 <xref:System.Windows.Forms.CheckedListBox.GetItemChecked%2A> メソッドは、項目のインデックス番号を引数として受け取り、`true` または `false`を返します。 期待しているものとは異なり、<xref:System.Windows.Forms.ListBox.SelectedItems%2A> と <xref:System.Windows.Forms.ListBox.SelectedIndices%2A> のプロパティでは、どの項目がチェックされるかは決定されません。どの項目が強調表示されるかを決定します。  
  
### <a name="to-determine-checked-items-in-a-checkedlistbox-control"></a>CheckedListBox コントロールでチェックされている項目を確認するには  
  
1. コレクションが0から始まるため、<xref:System.Windows.Forms.CheckedListBox.CheckedItems%2A> コレクションを反復処理して0から開始します。 このメソッドは、リスト全体ではなく、チェックされた項目の一覧に項目番号を提供します。 このため、リストの最初の項目がチェックされず、2番目の項目がオンになっている場合は、次のコードに "Checked Item 1 = MyListItem2" のようなテキストが表示されます。  
  
    ```vb  
    ' Determine if there are any items checked.  
    If CheckedListBox1.CheckedItems.Count <> 0 Then  
       ' If so, loop through all checked items and print results.  
       Dim x As Integer  
       Dim s As String = ""  
       For x = 0 To CheckedListBox1.CheckedItems.Count - 1  
          s = s & "Checked Item " & (x + 1).ToString & " = " & CheckedListBox1.CheckedItems(x).ToString & ControlChars.CrLf  
       Next x  
       MessageBox.Show(s)  
    End If  
    ```  
  
    ```csharp  
    // Determine if there are any items checked.  
    if(checkedListBox1.CheckedItems.Count != 0)  
    {  
       // If so, loop through all checked items and print results.  
       string s = "";  
       for(int x = 0; x < checkedListBox1.CheckedItems.Count ; x++)  
       {  
          s = s + "Checked Item " + (x+1).ToString() + " = " + checkedListBox1.CheckedItems[x].ToString() + "\n";  
       }  
       MessageBox.Show(s);  
    }  
    ```  
  
    ```cpp  
    // Determine if there are any items checked.  
    if(checkedListBox1->CheckedItems->Count != 0)  
    {  
       // If so, loop through all checked items and print results.  
       String ^ s = "";  
       for(int x = 0; x < checkedListBox1->CheckedItems->Count; x++)  
       {  
          s = String::Concat(s, "Checked Item ", (x+1).ToString(),  
             " = ", checkedListBox1->CheckedItems[x]->ToString(),  
             "\n");  
       }  
       MessageBox::Show(s);  
    }  
    ```  
  
     - \- または -  
  
2. コレクションが0から始まるため、<xref:System.Windows.Forms.CheckedListBox.Items%2A> コレクションをステップ実行し、各項目に対して <xref:System.Windows.Forms.CheckedListBox.GetItemChecked%2A> メソッドを呼び出します。 この方法では、一覧の項目番号が表示されるので、リスト内の最初の項目がチェックされず、2番目の項目がオンになっている場合は、"Item 2 = MyListItem2" のような内容が表示されます。  
  
    ```vb  
    Dim i As Integer  
    Dim s As String  
    s = "Checked Items:" & ControlChars.CrLf  
    For i = 0 To (CheckedListBox1.Items.Count - 1)  
       If CheckedListBox1.GetItemChecked(i) = True Then  
          s = s & "Item " & (i + 1).ToString & " = " & CheckedListBox1.Items(i).ToString & ControlChars.CrLf  
       End If  
    Next  
    MessageBox.Show(s)  
    ```  
  
    ```csharp  
    int i;  
    string s;   
    s = "Checked items:\n" ;  
    for (i = 0; i <= (checkedListBox1.Items.Count-1); i++)  
    {  
       if (checkedListBox1.GetItemChecked(i))  
       {  
          s = s + "Item " + (i+1).ToString() + " = " + checkedListBox1.Items[i].ToString() + "\n";  
       }  
    }  
    MessageBox.Show (s);  
    ```  
  
    ```cpp  
    int i;  
    String ^ s;   
    s = "Checked items:\n" ;  
    for (i = 0; i <= (checkedListBox1->Items->Count-1); i++)  
    {  
       if (checkedListBox1->GetItemChecked(i))  
       {  
          s = String::Concat(s, "Item ", (i+1).ToString(), " = ",  
             checkedListBox1->Item[i]->ToString(), "\n");  
       }  
    }  
    MessageBox::Show(s);  
    ```  
  
## <a name="see-also"></a>参照

- [オプションのリストを表示するための Windows フォーム コントロール](windows-forms-controls-used-to-list-options.md)
