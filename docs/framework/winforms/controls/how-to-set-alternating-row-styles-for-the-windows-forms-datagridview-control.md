---
title: DataGridView コントロールに交互の行のスタイルを設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], row styles
- data grids [Windows Forms], row styles
- rows [Windows Forms], data grids
ms.assetid: 699ef759-458c-426d-ac87-7c7e71b018ae
ms.openlocfilehash: b87ad50ef7e5118a95998949bc62299955856b27
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747135"
---
# <a name="how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールに交互の行のスタイルを設定する
表形式のデータは、多くの場合、行の背景色が交互に別の色になった、帳簿のような形式でユーザーに表示されます。 この形式を使用すると、多数の列がある幅の広いテーブルで、ユーザーが各行にあるセルを簡単に識別できるようになります。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールを使用すると、1 行おきの完全なスタイル情報を指定できます。 これにより、背景色だけでなく、前景色とフォントなどのスタイルの特性を使用して、交互の行を区別することができます。  
  
 Visual Studio では、このタスクに対するサポートが用意されています。  「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールに交互の行のスタイルを設定する](set-alternating-row-styles-for-the-datagrid-using-the-designer.md)」も参照してください。  
  
### <a name="to-set-alternating-row-styles-programmatically"></a>交互の行のスタイルをプログラムで設定する  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle> の <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> プロパティと <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A> プロパティにより返される <xref:System.Windows.Forms.DataGridView> オブジェクトのプロパティを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#068](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#068)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#068](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#068)]  
  
    > [!NOTE]
    > <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> プロパティと <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A> プロパティを使用して指定したスタイルは、列と <xref:System.Windows.Forms.DataGridView> のレベルで指定されたスタイルをオーバーライドしますが、個別の行とセルのレベルで設定されたスタイルによってオーバーライドされます。 詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Drawing?displayProperty=nameWithType>、および <xref:System.Windows.Forms?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 最大限のスケーラビリティを実現するには、各要素のスタイルのプロパティを個別に設定するのではなく、同じスタイルを使用する複数の行、列、またはセルで <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを共有してください。 詳細については、「 [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのフォントと色のスタイルを設定する](how-to-set-font-and-color-styles-in-the-windows-forms-datagridview-control.md)
