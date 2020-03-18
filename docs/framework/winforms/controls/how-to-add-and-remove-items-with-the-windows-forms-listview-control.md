---
title: ListView コントロールを使用して項目を追加および削除する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView control [Windows Forms], populating
- list views [Windows Forms], adding list items
- ListView control [Windows Forms], adding list items
ms.assetid: 1b35a80a-edd8-495f-a807-a28c4aae52c6
ms.openlocfilehash: bbfe99db857ebe3a80bf99926f3ce0bec38a1f3f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743136"
---
# <a name="how-to-add-and-remove-items-with-the-windows-forms-listview-control"></a>方法 : Windows フォーム ListView コントロールで項目を追加および削除する
項目を Windows フォーム <xref:System.Windows.Forms.ListView> コントロールに追加するプロセスは、主に項目を指定し、プロパティを割り当てることによって構成されます。 リスト項目の追加または削除は、いつでも行うことができます。  
  
### <a name="to-add-items-programmatically"></a>プログラムによって項目を追加するには  
  
1. <xref:System.Windows.Forms.ListView.Items%2A> プロパティの <xref:System.Windows.Forms.ListView.ListViewItemCollection.Add%2A> メソッドを使用します。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#11](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#11)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#11](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#11)]  
  
### <a name="to-remove-items-programmatically"></a>プログラムによって項目を削除するには  
  
1. <xref:System.Windows.Forms.ListView.Items%2A> プロパティの <xref:System.Windows.Forms.ListView.ListViewItemCollection.RemoveAt%2A> または <xref:System.Windows.Forms.ListView.ListViewItemCollection.Clear%2A> メソッドを使用します。 <xref:System.Windows.Forms.ListView.ListViewItemCollection.RemoveAt%2A> メソッドは、1つの項目を削除します。<xref:System.Windows.Forms.ListView.ListViewItemCollection.Clear%2A> メソッドは、リストからすべての項目を削除します。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#12](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#12)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#12](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#12)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListView>
- [ListView コントロール](listview-control-windows-forms.md)
- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
