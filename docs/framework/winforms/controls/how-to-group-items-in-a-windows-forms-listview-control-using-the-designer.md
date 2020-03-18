---
title: デザイナーを使用して ListView コントロール内の項目をグループ化する
ms.date: 03/30/2017
helpviewer_keywords:
- ListView control [Windows Forms], grouping items
- grouping
- groups [Windows Forms], in Windows Forms controls
ms.assetid: 8b615000-69d9-4c64-acaf-b54fa09b69e3
ms.openlocfilehash: 935d8d75517e1028e686ca229f6ada656f58b01e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736685"
---
# <a name="how-to-group-items-in-a-windows-forms-listview-control-using-the-designer"></a>方法 : デザイナーを使って Windows フォーム ListView コントロールの項目をグループ化する

<xref:System.Windows.Forms.ListView> コントロールのグループ化機能を使用すると、関連する項目のセットをグループに表示できます。 これらのグループは、グループタイトルを含む横方向のグループヘッダーによって画面上で区切られます。 <xref:System.Windows.Forms.ListView> グループを使用すると、項目をアルファベット順、日付順、または他の論理グループ別にグループ化することで、大きなリストを簡単に移動できます。 次の図は、グループ化された項目を示しています。

![奇数と偶数のグループに分割された数値。](./media/how-to-group-items-in-a-windows-forms-listview-control-using-the-designer/odd-even-list-view-groups.gif)

次の手順では、<xref:System.Windows.Forms.ListView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

グループ化を有効にするには、最初にデザイナーまたはプログラムを使用して、1つ以上の <xref:System.Windows.Forms.ListViewGroup> オブジェクトを作成する必要があります。 グループを定義したら、それに項目を割り当てることができます。

## <a name="to-add-or-remove-groups-in-the-designer"></a>デザイナーでグループを追加または削除するには

1. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.ListView.Groups%2A>] プロパティの横に**ある省略記号 (省略**記号ボタン ([...]) ボタンをクリックして、[Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] ボタンを![ます。

     **ListViewGroup Collection エディター**が表示されます。

2. グループを追加するには、 **[追加]** ボタンをクリックします。 その後、新しいグループのプロパティ (<xref:System.Windows.Forms.ListViewGroup.Header%2A> や <xref:System.Windows.Forms.ListViewGroup.HeaderAlignment%2A> プロパティなど) を設定できます。 グループを削除するには、グループを選択し、 **[削除]** ボタンをクリックします。

## <a name="to-assign-items-to-groups-in-the-designer"></a>デザイナーで項目をグループに割り当てるには

1. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.ListView.Items%2A>] プロパティの横に**ある省略記号 (省略**記号ボタン ([...]) ボタンをクリックして、[Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] ボタンを![ます。

     **ListViewItem Collection エディター**が表示されます。

2. 新しい項目を追加するには、 **[追加]** ボタンをクリックします。 その後、<xref:System.Windows.Forms.ListViewItem.Text%2A> や <xref:System.Windows.Forms.ListViewItem.ImageIndex%2A> プロパティなど、新しいアイテムのプロパティを設定できます。

3. [<xref:System.Windows.Forms.ListViewItem.Group%2A>] プロパティを選択し、ドロップダウンリストからグループを選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListView>
- <xref:System.Windows.Forms.ListView.Groups%2A>
- <xref:System.Windows.Forms.ListViewGroup>
- [ListView コントロール](listview-control-windows-forms.md)
- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
- [方法: Windows フォーム ListView コントロールで項目を追加および削除する](how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
