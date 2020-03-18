---
title: DataGridView コントロールのデータの書式設定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], formatting data
- data [Windows Forms], formatting in DataGridView control
- data grids [Windows Forms], enabling wordwrap
- currency values [Windows Forms], formatting in data grids
- data grids [Windows Forms], currency values
- data grids [Windows Forms], formatting data
- data grids [Windows Forms], text alignment
- data grids [Windows Forms], date values
- cells [Windows Forms], text alignment
ms.assetid: 8c33543c-9c08-4636-a65a-fdf714a529b7
ms.openlocfilehash: 9ee2869cf4085b5acfdf1f8c440151be506dbe3e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736781"
---
# <a name="how-to-format-data-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールのデータの書式を設定する
次の手順では、コントロールの <xref:System.Windows.Forms.DataGridView> コントロールと特定の列の <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A> プロパティを使用して、セル値の基本的な書式を設定する方法について説明します。 高度なデータ書式設定の詳細については、「[方法: Windows フォーム DataGridView コントロールでデータの書式設定をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
### <a name="to-format-currency-and-date-values"></a>通貨と日付の値を書式設定するには  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A> の <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティを設定します。 次のコード例では、列の <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A> プロパティを使用して、特定の列の形式を設定します。 `UnitPrice` 列の値は、現在のカルチャ固有の通貨書式で表示され、負の値はかっこで囲まれます。 `ShipDate` 列の値は、現在のカルチャに固有の短い日付形式で表示されます。 <xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A> 値の詳細については、「[型の書式設定](../../../standard/base-types/formatting-types.md)」を参照してください。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#071](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#071)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#071](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#071)]  
  
### <a name="to-customize-the-display-of-null-database-values"></a>Null データベース値の表示をカスタマイズするには  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.NullValue%2A> の <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティを設定します。 次のコード例では、<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType> プロパティを使用して、<xref:System.DBNull.Value?displayProperty=nameWithType>と等しい値を含むすべてのセルに "no entry" と表示します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#073](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#073)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#073](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#073)]  
  
### <a name="to-enable-wordwrap-in-text-based-cells"></a>テキストベースのセルで折り返しを有効にするには  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle> の <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A> プロパティを <xref:System.Windows.Forms.DataGridViewTriState> 列挙値のいずれかに設定します。 次のコード例では、<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType> プロパティを使用して、コントロール全体のラップモードを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#074](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#074)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#074](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#074)]  
  
### <a name="to-specify-the-text-alignment-of-datagridview-cells"></a>DataGridView セルのテキストの配置を指定するには  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle> の <xref:System.Windows.Forms.DataGridViewCellStyle.Alignment%2A> プロパティを <xref:System.Windows.Forms.DataGridViewContentAlignment> 列挙値のいずれかに設定します。 次のコード例では、列の <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A> プロパティを使用して、特定の列の配置を設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#072](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#072)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#072](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#072)]  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#070](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#070)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#070](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#070)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 これらの例には以下のものが必要です。  
  
- `UnitPrice`という名前の列、`ShipDate`という名前の列、および `CustomerName`という名前の列を含む `dataGridView1` という名前の <xref:System.Windows.Forms.DataGridView> コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Drawing?displayProperty=nameWithType>、および <xref:System.Windows.Forms?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 最大限のスケーラビリティを実現するには、各要素のスタイルプロパティを個別に設定するのではなく、同じスタイルを使用する複数の行、列、またはセルにわたって <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを共有する必要があります。 詳細については、「 [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewBand.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータの書式設定](data-formatting-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのデータの書式設定をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)
- [型の書式設定](../../../standard/base-types/formatting-types.md)
