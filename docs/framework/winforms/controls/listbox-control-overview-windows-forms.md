---
title: ListBox コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- ListBox
helpviewer_keywords:
- list boxes [Windows Forms], about list boxes
- ListBox control [Windows Forms], about ListBox control
ms.assetid: 37ea226b-6fc8-4c70-936a-c6af4e0cad4c
ms.openlocfilehash: 1abf8bea5c786f2ce326631370fa294ada09a92c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745181"
---
# <a name="listbox-control-overview-windows-forms"></a>ListBox コントロールの概要 (Windows フォーム)
Windows フォームの <xref:System.Windows.Forms.ListBox> コントロールには、ユーザーが1つ以上の項目を選択できる一覧が表示されます。 項目の合計数が表示可能な数を超えると、スクロールバーが <xref:System.Windows.Forms.ListBox> コントロールに自動的に追加されます。 <xref:System.Windows.Forms.ListBox.MultiColumn%2A> プロパティが `true`に設定されている場合、リストボックスは複数の列に項目を表示し、水平スクロールバーが表示されます。 <xref:System.Windows.Forms.ListBox.MultiColumn%2A> プロパティが `false`に設定されている場合、リストボックスには項目が1つの列に表示され、垂直スクロールバーが表示されます。 <xref:System.Windows.Forms.ListBox.ScrollAlwaysVisible%2A> が `true`に設定されている場合、アイテムの数に関係なく、スクロールバーが表示されます。 <xref:System.Windows.Forms.ListBox.SelectionMode%2A> プロパティは、一度に選択できるリスト項目の数を決定します。  
  
## <a name="ways-to-change-the-listbox-control"></a>ListBox コントロールを変更する方法  
 <xref:System.Windows.Forms.ListBox.SelectedIndex%2A> プロパティは、リストボックスで最初に選択された項目に対応する整数値を返します。 コード内の <xref:System.Windows.Forms.ListBox.SelectedIndex%2A> 値を変更することで、選択した項目をプログラムで変更できます。リスト内の対応する項目が Windows フォームで強調表示されます。 項目が選択されていない場合、<xref:System.Windows.Forms.ListBox.SelectedIndex%2A> 値は-1 になります。 リスト内の最初の項目が選択されている場合、<xref:System.Windows.Forms.ListBox.SelectedIndex%2A> 値は0です。 複数の項目が選択されている場合、<xref:System.Windows.Forms.ListBox.SelectedIndex%2A> の値は、一覧の最初に表示される選択された項目を反映します。 <xref:System.Windows.Forms.ListBox.SelectedItem%2A> プロパティは <xref:System.Windows.Forms.ListBox.SelectedIndex%2A>に似ていますが、項目自体 (通常は文字列値) を返します。 <xref:System.Windows.Forms.ListBox.ObjectCollection.Count%2A> プロパティには、リスト内の項目の数が反映されます。また、<xref:System.Windows.Forms.ListBox.ObjectCollection.Count%2A> プロパティの値は、使用可能な最大の <xref:System.Windows.Forms.ListBox.SelectedIndex%2A> 値を表します。これは、<xref:System.Windows.Forms.ListBox.SelectedIndex%2A> が0から始まるためです。  
  
 <xref:System.Windows.Forms.ListBox> コントロールの項目を追加または削除するには、<xref:System.Windows.Forms.ListBox.ObjectCollection.Add%2A>、<xref:System.Windows.Forms.ListBox.ObjectCollection.Insert%2A>、<xref:System.Windows.Forms.ListBox.ObjectCollection.Clear%2A>、または <xref:System.Windows.Forms.ListBox.ObjectCollection.Remove%2A> メソッドを使用します。 または、デザイン時に <xref:System.Windows.Forms.ListBox.Items%2A> プロパティを使用して、リストに項目を追加することもできます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListBox>
- [方法: Windows フォームの ComboBox、ListBox、または CheckedListBox コントロールに項目を追加または削除する](add-and-remove-items-from-a-wf-combobox.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールを並べ替える](sort-the-contents-of-a-wf-combobox-listbox-or-checkedlistbox-control.md)
- [方法: Windows フォームの ComboBox または ListBox コントロールをデータにバインドする](how-to-bind-a-windows-forms-combobox-or-listbox-control-to-data.md)
- [ComboBox コントロールの概要](combobox-control-overview-windows-forms.md)
- [CheckedListBox コントロールの概要](checkedlistbox-control-overview-windows-forms.md)
- [オプションのリストを表示するための Windows フォーム コントロール](windows-forms-controls-used-to-list-options.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールのルックアップ テーブルを作成する](create-a-lookup-table-for-a-wf-combobox-listbox.md)
