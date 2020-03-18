---
title: ListView コントロールのアイコンを表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView control [Windows Forms], displaying icons
- icons [Windows Forms], displaying for ListView controls
- lists [Windows Forms], displaying icons
- ImageList component [Windows Forms], with ListView control
- list views [Windows Forms], displaying icons
ms.assetid: 9d577542-8595-429b-99e5-078770ec9d35
ms.openlocfilehash: 5fc54c448dae95cab50cdafa8403769fb421dffa
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744507"
---
# <a name="how-to-display-icons-for-the-windows-forms-listview-control"></a>方法 : Windows フォーム ListView コントロールのアイコンを表示する
Windows フォーム <xref:System.Windows.Forms.ListView> コントロールは、3つのイメージリストのアイコンを表示できます。 List、Details、および SmallIcon の各ビューには、[<xref:System.Windows.Forms.ListView.SmallImageList%2A>] プロパティで指定したイメージリストの画像が表示されます。 LargeIcon ビューには、<xref:System.Windows.Forms.ListView.LargeImageList%2A> プロパティで指定されたイメージリストのイメージが表示されます。 リストビューには、大きいアイコンまたは小さいアイコンの横にある [<xref:System.Windows.Forms.ListView.StateImageList%2A>] プロパティで設定された追加のアイコンのセットを表示することもできます。 イメージリストの詳細については、「 [Imagelist コンポーネント](imagelist-component-windows-forms.md)」および「[方法: Windows フォーム imagelist コンポーネントを使用してイメージを追加または削除する](how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)」を参照してください。  
  
### <a name="to-display-images-in-a-list-view"></a>リストビューに画像を表示するには  
  
1. 適切なプロパティ (<xref:System.Windows.Forms.ListView.SmallImageList%2A>、<xref:System.Windows.Forms.ListView.LargeImageList%2A>、または <xref:System.Windows.Forms.ListView.StateImageList%2A>) を、使用する既存の <xref:System.Windows.Forms.ImageList> コンポーネントに設定します。  
  
     これらのプロパティは、デザイナーでプロパティウィンドウまたはコードで設定できます。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#41](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#41)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#41](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#41)]  
  
2. アイコンが関連付けられている各リスト項目の <xref:System.Windows.Forms.ListViewItem.ImageIndex%2A> または <xref:System.Windows.Forms.ListViewItem.StateImageIndex%2A> プロパティを設定します。  
  
     これらのプロパティは、コードで設定することも、 **ListViewItem Collection エディター**内で設定することもできます。 **ListViewItem Collection エディター**を開くには、省略記号ボタン ( **[プロパティ]** ウィンドウの [<xref:System.Windows.Forms.ListView.Items%2A>] プロパティの横に](./media/visual-studio-ellipsis-button.png)プロパティウィンドウある省略記号ボタン ([...]) をクリックします (![)。  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#42](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#42)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#42](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#42)]  
  
## <a name="see-also"></a>参照

- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
- [方法: Windows フォーム ListView コントロールで項目を追加および削除する](how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
- [方法: Windows フォーム ListView コントロールに列を追加する](how-to-add-columns-to-the-windows-forms-listview-control.md)
- [方法: TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加する](add-custom-information-to-a-treeview-or-listview-control-wf.md)
- [ImageList コンポーネント](imagelist-component-windows-forms.md)
