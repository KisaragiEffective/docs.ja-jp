---
title: DataGridView コントロールの列の並べ替えモードを設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sorting [Windows Forms], data grids
- DataGridView control [Windows Forms], sort mode
- data grids [Windows Forms], sorting data
ms.assetid: 57dfed60-a608-40d5-86f9-d65686ffb325
ms.openlocfilehash: 45ee1e7d82f826cddbd3492fed0f63e7a9c2cf1d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743000"
---
# <a name="how-to-set-the-sort-modes-for-columns-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロール内の列の並べ替えモードを設定する
<xref:System.Windows.Forms.DataGridView> コントロールでは、テキストボックスの列では既定で自動並べ替えが使用され、他の列の型は自動的に並べ替えられません。 場合によっては、これらの既定値をオーバーライドする必要があります。 たとえば、テキスト、数値、または列挙セル値の代わりに画像を表示できます。 画像を並べ替えることはできませんが、それらが表す基になる値を並べ替えることができます。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールでは、列の <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティ値によって並べ替え動作が決まります。  
  
 次の手順では、 [「方法: Windows フォーム DataGridView コントロールでデータの書式設定をカスタマイズする」](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)の `Priority` 列を示します。 この列はイメージ列であり、既定では並べ替え可能ではありません。 これには、文字列である実際のセル値が含まれるため、自動的に並べ替えることができます。  
  
### <a name="to-set-the-sort-mode-for-a-column"></a>列の並べ替えモードを設定するには  
  
- <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A?displayProperty=nameWithType> プロパティを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#066](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#066)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#066](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#066)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の列を含む `dataGridView1` という名前の `Priority` コントロール。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A?displayProperty=nameWithType>
- [Windows フォームの DataGridView コントロールでのデータの並べ替え](sorting-data-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロール内の列の並べ替えモード](column-sort-modes-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの並べ替え機能をカスタマイズする](how-to-customize-sorting-in-the-windows-forms-datagridview-control.md)
