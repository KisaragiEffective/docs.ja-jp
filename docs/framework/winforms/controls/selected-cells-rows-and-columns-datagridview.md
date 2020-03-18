---
title: DataGridView コントロールで選択されたセル、行、および列を取得します。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- selection [Windows Forms], DataGridView control [Windows Forms]
- DataGridView control [Windows Forms], getting selection
- getting selection [Windows Forms], DataGridView control [Windows Forms]
ms.assetid: d93c4b5b-498e-49bc-982a-2229d61778e4
ms.openlocfilehash: 0079c590ee6e4732523fd50be157bd58ec1bfb1b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743073"
---
# <a name="how-to-get-the-selected-cells-rows-and-columns-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの選択されたセル、行、および列を取得する
選択したセル、行、または列は、対応するプロパティ (<xref:System.Windows.Forms.DataGridView.SelectedCells%2A>、<xref:System.Windows.Forms.DataGridView.SelectedRows%2A>、および <xref:System.Windows.Forms.DataGridView.SelectedColumns%2A>) を使用して、<xref:System.Windows.Forms.DataGridView> コントロールから取得できます。 次の手順では、選択したセルを取得し、そのセルの行インデックスと列インデックスを <xref:System.Windows.Forms.MessageBox>に表示します。  
  
### <a name="to-get-the-selected-cells-in-a-datagridview-control"></a>DataGridView コントロール内の選択されたセルを取得するには  
  
- <xref:System.Windows.Forms.DataGridView.SelectedCells%2A> プロパティを使用します。  
  
    > [!NOTE]
    > 多くのセルが表示されないようにするには、<xref:System.Windows.Forms.DataGridView.AreAllCellsSelected%2A> メソッドを使用します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSelectedCollections#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/CS/DataGridViewSelectedCollections.cs#10)]
     [!code-vb[System.Windows.Forms.DataGridViewSelectedCollections#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/VB/DataGridViewSelectedCollections.vb#10)]  
  
### <a name="to-get-the-selected-rows-in-a-datagridview-control"></a>DataGridView コントロールで選択された行を取得するには  
  
- <xref:System.Windows.Forms.DataGridView.SelectedRows%2A> プロパティを使用します。 ユーザーが行を選択できるようにするには、<xref:System.Windows.Forms.DataGridView.SelectionMode%2A> プロパティを <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect> または <xref:System.Windows.Forms.DataGridViewSelectionMode.RowHeaderSelect>に設定する必要があります。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSelectedCollections#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/CS/DataGridViewSelectedCollections.cs#20)]
     [!code-vb[System.Windows.Forms.DataGridViewSelectedCollections#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/VB/DataGridViewSelectedCollections.vb#20)]  
  
### <a name="to-get-the-selected-columns-in-a-datagridview-control"></a>DataGridView コントロールで選択されている列を取得するには  
  
- <xref:System.Windows.Forms.DataGridView.SelectedColumns%2A> プロパティを使用します。 ユーザーが列を選択できるようにするには、<xref:System.Windows.Forms.DataGridView.SelectionMode%2A> プロパティを <xref:System.Windows.Forms.DataGridViewSelectionMode.FullColumnSelect> または <xref:System.Windows.Forms.DataGridViewSelectionMode.ColumnHeaderSelect>に設定する必要があります。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSelectedCollections#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/CS/DataGridViewSelectedCollections.cs#30)]
     [!code-vb[System.Windows.Forms.DataGridViewSelectedCollections#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSelectedCollections/VB/DataGridViewSelectedCollections.vb#30)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- `selectedCellsButton`、`selectedRowsButton`、および `selectedColumnsButton`という名前のコントロールを <xref:System.Windows.Forms.Button> します。各コントロールには、アタッチされている <xref:System.Windows.Forms.Control.Click> イベントのハンドラーがあります。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Windows.Forms?displayProperty=nameWithType>、および <xref:System.Text?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 このトピックで説明するコレクションは、多数のセル、行、または列が選択されている場合は、効率的には実行されません。 大量のデータでこれらのコレクションを使用する方法の詳細については、「 [Windows フォーム DataGridView コントロールのスケーリングに関するベストプラクティス](best-practices-for-scaling-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.SelectionMode%2A>
- <xref:System.Windows.Forms.DataGridView.AreAllCellsSelected%2A>
- <xref:System.Windows.Forms.DataGridView.SelectedCells%2A>
- <xref:System.Windows.Forms.DataGridView.SelectedRows%2A>
- <xref:System.Windows.Forms.DataGridView.SelectedColumns%2A>
- [Windows フォーム DataGridView コントロールでの選択およびクリップボードの使用](selection-and-clipboard-use-with-the-windows-forms-datagridview-control.md)
