---
title: ListView コントロールに列を追加する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView control [Windows Forms], adding column headers
- columns [Windows Forms], adding to ListView controls
- list views [Windows Forms], adding columns
ms.assetid: 79174274-12ee-4a5d-80db-6ec02976d010
ms.openlocfilehash: dd438ffbadddfc37ec9eb15e59a908bb58472a45
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744582"
---
# <a name="how-to-add-columns-to-the-windows-forms-listview-control"></a>方法 : Windows フォーム ListView コントロールに列を追加する
詳細ビューでは、<xref:System.Windows.Forms.ListView> コントロールは、各リスト項目に対して複数の列を表示できます。 列を使用して、各リスト項目に関するいくつかの種類の情報をユーザーに表示できます。 たとえば、ファイルの一覧には、ファイル名、ファイルの種類、サイズ、ファイルが最後に変更された日付が表示されます。 作成後に列を設定する方法の詳細については、「[方法: Windows フォーム ListView コントロールを使用して列にサブ項目を表示する](how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)」を参照してください。  
  
### <a name="to-add-columns-programmatically"></a>プログラムによって列を追加するには  
  
1. コントロールの <xref:System.Windows.Forms.ListView.View%2A> プロパティを <xref:System.Windows.Forms.View.Details>に設定します。  
  
2. リストビューの <xref:System.Windows.Forms.ListView.Columns%2A> プロパティの <xref:System.Windows.Forms.ListView.ColumnHeaderCollection.Add%2A> メソッドを使用します。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#31](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#31)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#31](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#31)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListView>
- [ListView コントロール](listview-control-windows-forms.md)
- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
