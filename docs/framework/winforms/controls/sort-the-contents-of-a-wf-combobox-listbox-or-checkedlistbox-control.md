---
title: ComboBox、ListBox、または CheckedListBox コントロールの内容を並べ替える
ms.date: 03/30/2017
helpviewer_keywords:
- CheckedListBox control [Windows Forms], sorting
- ComboBox control [Windows Forms], sorting contents
- combo boxes [Windows Forms], sorting contents
- list boxes [Windows Forms], sorting contents
- ListBox control [Windows Forms], sorting contents
ms.assetid: c268e387-3d1d-4d86-a940-19f6673c8d06
ms.openlocfilehash: dee969d777edf76434622fe632f7e934987a1dc3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742953"
---
# <a name="how-to-sort-the-contents-of-a-windows-forms-combobox-listbox-or-checkedlistbox-control"></a>方法 : Windows フォーム ComboBox、ListBox、または CheckedListBox コントロールを並べ替える
Windows フォームコントロールは、データバインドされている場合は並べ替えられません。 並べ替えられたデータを表示するには、並べ替えをサポートするデータソースを使用し、データソースの並べ替えを行います。 並べ替えをサポートするデータソースは、データビュー、データビューマネージャー、および並べ替えられた配列です。  
  
 コントロールがデータバインドされていない場合は、並べ替えることができます。  
  
### <a name="to-sort-the-list"></a>リストを並べ替えるには  
  
1. `Sorted` プロパティを `true` に設定します。  
  
     この設定により、既存のすべてのリスト項目が並べ替えられた順序で再配置されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ComboBox>
- <xref:System.Windows.Forms.ListBox>
- <xref:System.Windows.Forms.CheckedListBox>
- [Windows フォームでのデータ バインディング](../windows-forms-data-binding.md)
- [方法: Windows フォームの ComboBox、ListBox、または CheckedListBox コントロールに項目を追加または削除する](add-and-remove-items-from-a-wf-combobox.md)
- [ListBox の代わりに Windows フォーム ComboBox を使用する場合](when-to-use-a-windows-forms-combobox-instead-of-a-listbox.md)
- [オプションのリストを表示するための Windows フォーム コントロール](windows-forms-controls-used-to-list-options.md)
