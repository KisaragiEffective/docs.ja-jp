---
title: DataGridView コントロールの既定のセルスタイルを設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], cell styles
- cells [Windows Forms], styles
- data grids [Windows Forms], cell styles
ms.assetid: 1aaaca43-5340-447e-99c0-9177d9776aa1
ms.openlocfilehash: 6b2d7de671e48ae8f55987c262e15717138b3fb4
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744870"
---
# <a name="how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの既定のセル スタイルを設定する
<xref:System.Windows.Forms.DataGridView> コントロールを使用して、コントロール全体、および特定の列と行の既定のセル スタイルを指定できます。 これらは既定でフィルターを下に移動し、コントロール レベルから列レベルへ、次に行レベルへ、その次にセル レベルへ移動します。 特定の <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティがセル レベルで設定されていないと、行レベルで既定のプロパティの設定が使用されます。 行レベルでもプロパティが設定されていない場合、既定の列の設定が使用されます。 最後に、列レベルでもプロパティが設定されていない場合、既定の <xref:System.Windows.Forms.DataGridView> の設定が使用されます。 この設定により、複数のレベルでプロパティの設定を複製する必要がなくなります。 各レベルでは、上位のレベルとは異なるスタイルだけを指定します。 詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 Visual Studio では、このタスクに対する広範なサポートが用意されています。  「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールの既定のセルスタイルとデータ形式を設定する](default-cell-styles-datagridview.md)」も参照してください。  
  
### <a name="to-set-the-default-cell-styles-programmatically"></a>既定値のセル スタイルをプログラムで設定するには  
  
1. <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティによって取得された <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType> のプロパティを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#141](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#141)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#141](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#141)]  
  
2. 複数の行と列によって使用される新しい <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを作成して初期化します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#142](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#142)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#142](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#142)]  
  
3. 特定の行と列の `DefaultCellStyle` プロパティを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#143](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#143)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#143](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#143)]  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#140](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#140)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#140](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#140)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Drawing?displayProperty=nameWithType>、および <xref:System.Windows.Forms?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 非常に大きなデータ セットを処理するときに最大限のスケーラビリティを実現するには、各要素のスタイルのプロパティを個別に設定するのではなく、同じスタイルを使用する複数の行、列、またはセルで <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを共有してください。 さらに、<xref:System.Windows.Forms.DataGridViewRowCollection.SharedRow%2A?displayProperty=nameWithType> プロパティを使用して、共有された行を作成してアクセスする必要があります。 詳細については、「 [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewBand.DefaultCellStyle%2A?displayProperty=nameWithType>
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールに交互の行のスタイルを設定する](how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control.md)
