---
title: DataGridView コントロールのセルの外観をカスタマイズする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data grids [Windows Forms], customizing cells
- DataGridView control [Windows Forms], customizing cells
- cells [Windows Forms], customizing in DataGridView control
ms.assetid: 478b20c9-625c-4116-9c5c-5a16e6f4ec67
ms.openlocfilehash: 754932427a365a7b032c1ac9627d3237d1f7d0c6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744056"
---
# <a name="how-to-customize-the-appearance-of-cells-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォームの DataGridView コントロールのセルの外観をカスタマイズする
<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CellPainting> イベントを処理することで、任意のセルの外観をカスタマイズできます。 <xref:System.Windows.Forms.DataGridViewCellPaintingEventArgs>の <xref:System.Windows.Forms.DataGridViewCellPaintingEventArgs.Graphics%2A> プロパティから <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Drawing.Graphics> を抽出できます。 この <xref:System.Drawing.Graphics>では、<xref:System.Windows.Forms.DataGridView> コントロール全体の外観に影響を与えることができますが、通常は、現在描画されているセルの外観のみに影響を与えます。 <xref:System.Windows.Forms.DataGridViewCellPaintingEventArgs> の <xref:System.Windows.Forms.DataGridViewCellPaintingEventArgs.ClipBounds%2A> プロパティを使用すると、描画操作を現在描画されているセルに制限できます。  
  
 次のコード例では、<xref:System.Windows.Forms.DataGridView> コントロールの配色を使用して、`ContactName` 列のすべてのセルを塗りつぶします。 各セルのテキストコンテンツは <xref:System.Drawing.Color.Crimson%2A>で描画され、埋め込み四角形は、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.GridColor%2A> プロパティと同じ色で描画されます。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewCellPainting#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCellPainting/CS/form1.cs#10)]
 [!code-vb[System.Windows.Forms.DataGridViewCellPainting#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewCellPainting/VB/form1.vb#10)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- Northwind サンプルデータベースの Customers テーブルにあるような `ContactName` 列を持つ `dataGridView1` という名前の <xref:System.Windows.Forms.DataGridView> コントロール。  
  
- System、System.Windows.Forms、および System.Drawing の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.CellPainting>
- [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)
