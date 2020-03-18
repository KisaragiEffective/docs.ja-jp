---
title: ComboBox と ListBox
ms.date: 03/30/2017
helpviewer_keywords:
- ListBox control [Windows Forms], adding and removing items
- ListBox control [Windows Forms], vs. ComboBox
- bound controls [Windows Forms], combo boxes
- Windows Forms controls, data binding
- ComboBox control [Windows Forms], compared to ListBox
- combo boxes [Windows Forms], compared to list boxes
- ListBox control [Windows Forms], accessing items
- ListCount property
ms.assetid: 7bcaea58-1cfa-46db-9baf-b75a69d8f9ec
ms.openlocfilehash: 7087760a393bb58d83d899c1741c745fb28585bb
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739931"
---
# <a name="when-to-use-a-windows-forms-combobox-instead-of-a-listbox"></a>ListBox の代わりに Windows フォーム ComboBox を使用する場合
<xref:System.Windows.Forms.ComboBox> と <xref:System.Windows.Forms.ListBox> のコントロールには同様の動作があり、場合によっては交換可能である場合もあります。 ただし、タスクにより適切なものがある場合もあります。  
  
 一般的に、コンボボックスは、推奨される選択肢のリストがある場合に適しています。また、リストボックスは、入力を一覧の内容に限定する場合に適しています。 コンボボックスにはテキストボックスフィールドが含まれているので、一覧にない選択肢を入力することもできます。 例外は、<xref:System.Windows.Forms.ComboBox.DropDownStyle%2A> プロパティが <xref:System.Windows.Forms.ComboBoxStyle.DropDownList>に設定されている場合です。 この場合、最初の文字を入力すると、コントロールによって項目が選択されます。  
  
 また、コンボボックスでは、フォーム上の領域が節約されます。 ユーザーが下矢印をクリックするまでは完全な一覧が表示されないため、コンボボックスは、リストボックスが収まらない小さなスペースに簡単に収めることができます。 例外は、<xref:System.Windows.Forms.ComboBox.DropDownStyle%2A> プロパティが <xref:System.Windows.Forms.ComboBoxStyle.Simple>に設定されている場合です。完全な一覧が表示されます。コンボボックスは、リストボックスの場合よりも多くの領域を必要とします。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ComboBox>
- <xref:System.Windows.Forms.ListBox>
- [方法: Windows フォームの ComboBox、ListBox、または CheckedListBox コントロールに項目を追加または削除する](add-and-remove-items-from-a-wf-combobox.md)
- [方法: Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールを並べ替える](sort-the-contents-of-a-wf-combobox-listbox-or-checkedlistbox-control.md)
- [オプションのリストを表示するための Windows フォーム コントロール](windows-forms-controls-used-to-list-options.md)
