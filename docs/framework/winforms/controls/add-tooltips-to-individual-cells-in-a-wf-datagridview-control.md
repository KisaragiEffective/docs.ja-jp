---
title: DataGridView コントロールの個々のセルにツールヒントを追加する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- tooltips [Windows Forms], adding to data grids
- DataGridView control [Windows Forms], adding tooltips
- data grids [Windows Forms], adding tooltips
ms.assetid: 2a81f9de-d58b-4ea8-bc0b-8d93c2f4cf78
ms.openlocfilehash: ac86db5fa27a95adb20888cd59b5e236941d9177
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732181"
---
# <a name="how-to-add-tooltips-to-individual-cells-in-a-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの各セルにツールヒントを追加する
既定では、ツールヒントを使用して、小さすぎてコンテンツ全体を表示できない <xref:System.Windows.Forms.DataGridView> セルの値が表示されます。 ただし、この動作をオーバーライドして、個々のセルにツールヒントテキストの値を設定することもできます。 これは、セルに関する追加情報をユーザーに表示したり、セルの内容に関する別の説明をユーザーに提供したりする場合に便利です。 たとえば、状態アイコンを表示する行がある場合、ヒントを使用してテキストの説明を入力することができます。  
  
 また、<xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A?displayProperty=nameWithType> プロパティを `false`に設定して、セルレベルのツールヒントの表示を無効にすることもできます。  
  
### <a name="to-add-a-tooltip-to-a-cell"></a>ヒントをセルに追加するには  
  
- <xref:System.Windows.Forms.DataGridViewCell.ToolTipText%2A?displayProperty=nameWithType> プロパティを設定します。  
  
     [!code-cpp[System.Windows.Forms.DataGridViewCell.ToolTipText#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCell.ToolTipText/cpp/datagridviewcell.tooltiptext.cpp#1)]
     [!code-csharp[System.Windows.Forms.DataGridViewCell.ToolTipText#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCell.ToolTipText/CS/datagridviewcell.tooltiptext.cs#1)]
     [!code-vb[System.Windows.Forms.DataGridViewCell.ToolTipText#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCell.ToolTipText/VB/datagridviewcell.tooltiptext.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- この例で必要な要素は次のとおりです。  
  
- 1 ~ 4 個のアスタリスク ("*") 記号の文字列値を表示するための、`Rating` という名前の列を含む `dataGridView1` という名前の <xref:System.Windows.Forms.DataGridView> コントロール。 コントロールの <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントは、例に示すイベントハンドラーメソッドに関連付けられている必要があります。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 <xref:System.Windows.Forms.DataGridView> コントロールを外部データソースにバインドしたり、仮想モードを実装して独自のデータソースを提供したりすると、パフォーマンスの問題が発生する可能性があります。 大量のデータを処理するときにパフォーマンスが低下しないようにするには、複数のセルの <xref:System.Windows.Forms.DataGridViewCell.ToolTipText%2A> プロパティを設定するのではなく、<xref:System.Windows.Forms.DataGridView.CellToolTipTextNeeded> イベントを処理します。 このイベントを処理するときに、セル <xref:System.Windows.Forms.DataGridViewCell.ToolTipText%2A> プロパティの値を取得すると、イベントが発生し、イベントハンドラーに指定されている <xref:System.Windows.Forms.DataGridViewCellToolTipTextNeededEventArgs.ToolTipText%2A?displayProperty=nameWithType> プロパティの値が返されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.CellToolTipTextNeeded?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCell>
- <xref:System.Windows.Forms.DataGridViewCell.ToolTipText%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールのセル、行、および列を使用したプログラミング](programming-with-cells-rows-and-columns-in-the-datagrid.md)
