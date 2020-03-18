---
title: ComboBox コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- ComboBox
helpviewer_keywords:
- drop-down lists [Windows Forms], Windows Forms
- ComboBox control [Windows Forms], about ComboBox control
- drop-down lists [Windows Forms], ComboBox control
- combo boxes [Windows Forms], about combo boxes
ms.assetid: a58b393f-a614-45d1-8961-857a024b5acd
ms.openlocfilehash: 9fb270d7787902872a32a87c140316f756bd48aa
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743837"
---
# <a name="combobox-control-overview-windows-forms"></a>ComboBox コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.ComboBox> コントロールは、ドロップダウンコンボボックスにデータを表示するために使用されます。 既定では、<xref:System.Windows.Forms.ComboBox> コントロールは2つの部分に表示されます。一番上の部分は、ユーザーがリスト項目を入力できるテキストボックスです。 2番目の部分は、ユーザーが選択できる項目の一覧を表示するリストボックスです。 コンボボックスのその他のスタイルの詳細については、「 [When To Use a ListBox ではなく Windows フォーム ComboBox を使用する場合](when-to-use-a-windows-forms-combobox-instead-of-a-listbox.md)」を参照してください。  
  
 <xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> プロパティは、選択されたリスト項目に対応する整数値を返します。 コード内の <xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> 値を変更することで、選択した項目をプログラムで変更できます。リスト内の対応する項目が、コンボボックスのテキストボックスの部分に表示されます。 項目が選択されていない場合、<xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> 値は-1 になります。 リスト内の最初の項目が選択されている場合、<xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> 値は0になります。 <xref:System.Windows.Forms.ComboBox.SelectedItem%2A> プロパティは <xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> に似ていますが、項目自体 (通常は文字列値) を返します。 <xref:System.Windows.Forms.ComboBox.ObjectCollection.Count%2A> プロパティには、リスト内の項目の数が反映されます。また、<xref:System.Windows.Forms.ComboBox.ObjectCollection.Count%2A> プロパティの値は、使用可能な最大の <xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> 値を表します。これは、<xref:System.Windows.Forms.ComboBox.SelectedIndex%2A> が0から始まるためです。  
  
 <xref:System.Windows.Forms.ComboBox> コントロールの項目を追加または削除するには、<xref:System.Windows.Forms.ComboBox.ObjectCollection.Add%2A>、<xref:System.Windows.Forms.ComboBox.ObjectCollection.Insert%2A>、<xref:System.Windows.Forms.ComboBox.ObjectCollection.Clear%2A>、または <xref:System.Windows.Forms.ComboBox.ObjectCollection.Remove%2A> メソッドを使用します。 または、デザイナーの [<xref:System.Windows.Forms.ComboBox.Items%2A>] プロパティを使用して、リストに項目を追加することもできます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ComboBox>
- [ListBox コントロールの概要](listbox-control-overview-windows-forms.md)
- [ListBox の代わりに Windows フォーム ComboBox を使用する場合](when-to-use-a-windows-forms-combobox-instead-of-a-listbox.md)
- [方法: Windows フォームの ComboBox、ListBox、または CheckedListBox コントロールに項目を追加または削除する](add-and-remove-items-from-a-wf-combobox.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールを並べ替える](sort-the-contents-of-a-wf-combobox-listbox-or-checkedlistbox-control.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールの特定の項目にアクセスする](access-specific-items-in-a-wf-combobox-listbox-or-checkedlistbox.md)
- [方法: Windows フォームの ComboBox または ListBox コントロールをデータにバインドする](how-to-bind-a-windows-forms-combobox-or-listbox-control-to-data.md)
- [オプションのリストを表示するための Windows フォーム コントロール](windows-forms-controls-used-to-list-options.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールのルックアップ テーブルを作成する](create-a-lookup-table-for-a-wf-combobox-listbox.md)
