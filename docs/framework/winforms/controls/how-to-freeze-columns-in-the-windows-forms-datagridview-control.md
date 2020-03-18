---
title: DataGridView コントロールの列の固定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- columns [Windows Forms], freezing
- DataGridView control [Windows Forms], freezing columns
- DataGridView control [Windows Forms], columns always in view
ms.assetid: 2ef8b1de-782e-4867-af8d-58171ab5c106
ms.openlocfilehash: 6d1a98e5c4332078c6012cb7c9ed836108abd86c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736751"
---
# <a name="how-to-freeze-columns-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの列を固定する
ユーザーが Windows フォームの <xref:System.Windows.Forms.DataGridView> コントロールに表示されるデータを確認するときに、1 つの列または列のセットを頻繁に参照しなければならないことがあります。 たとえば、多数の列を含む顧客情報のテーブルを表示するとき、顧客名を常に表示して、その他の列は表示領域外にスクロールするようにできると便利です。  
  
 この動作を実現するために、コントロールの列を固定することができます。 列を固定すると、左側 (右から左へ記述する言語のスクリプトでは右側) のすべての列も同様に固定されます。 固定された列は表示されたままになり、その他のすべての列はスクロールできます。  
  
> [!NOTE]
> 列の並べ替えが有効な場合、固定された列は、固定されていない列とは異なるグループとして扱われます。 ユーザーは、どちらかのグループに列を再配置できますが、1 つのグループから別のグループに列を移動することはできません。  
  
 列の <xref:System.Windows.Forms.DataGridViewColumn.Frozen%2A> プロパティは、列が常にグリッド内で表示されるかどうかを決定します。  
  
 Visual Studio では、このタスクに対するサポートが用意されています。  「[方法: デザイナーを使用して Windows フォーム DataGridView コントロールの列を固定する](freeze-columns-in-the-datagrid-using-the-designer.md)」も参照してください。  
  
### <a name="to-freeze-a-column-programmatically"></a>プログラムで列を固定するには  
  
- <xref:System.Windows.Forms.DataGridViewColumn.Frozen%2A?displayProperty=nameWithType> プロパティを `true` に設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#061](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#061)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#061](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#061)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の列を含む `dataGridView1` という名前の `AddToCartButton` コントロール。  
  
- <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridViewColumn.Frozen%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでの列、行、およびセルの基本機能](basic-column-row-and-cell-features-wf-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの列の並べ替えを有効にする](how-to-enable-column-reordering-in-the-windows-forms-datagridview-control.md)
