---
title: ListView コントロールを使用して列にサブ項目を表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- list items
- Details view
- ListView control [Windows Forms], adding ListSubItems
- subitems
ms.assetid: e465f044-cde7-4fd9-a687-788a73a0f554
ms.openlocfilehash: 5c6d807410ad4ee0198d6334844bd65b148edff4
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745545"
---
# <a name="how-to-display-subitems-in-columns-with-the-windows-forms-listview-control"></a>方法 : Windows フォーム ListView コントロールの列にサブ項目を表示する
Windows フォーム <xref:System.Windows.Forms.ListView> コントロールでは、詳細ビューの各項目に対して追加のテキスト (サブ項目) を表示できます。 最初の列には、従業員番号などの項目のテキストが表示されます。 2番目、3番目、およびそれ以降の列には、最初、2番目、およびそれ以降の関連するサブ項目が表示されます。  
  
### <a name="to-add-subitems-to-a-list-item"></a>リスト項目にサブ項目を追加するには  
  
1. 必要な列を追加します。 最初の列には項目の <xref:System.Windows.Forms.ListView.Text%2A> プロパティが表示されるので、サブ項目の数よりも1つ多い列が必要です。 列の追加の詳細については、「[方法: Windows フォーム ListView コントロールに列を追加](how-to-add-columns-to-the-windows-forms-listview-control.md)する」を参照してください。  
  
2. 項目の <xref:System.Windows.Forms.ListViewItem.SubItems%2A> プロパティによって返されるコレクションの <xref:System.Windows.Forms.ListViewItem.ListViewSubItemCollection.Add%2A> メソッドを呼び出します。 次のコード例では、リストアイテムの従業員名と部署を設定します。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#61](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#61)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#61](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#61)]  
  
## <a name="see-also"></a>参照

- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
- [方法: Windows フォーム ListView コントロールで項目を追加および削除する](how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールに列を追加する](how-to-add-columns-to-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールのアイコンを表示する](how-to-display-icons-for-the-windows-forms-listview-control.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
