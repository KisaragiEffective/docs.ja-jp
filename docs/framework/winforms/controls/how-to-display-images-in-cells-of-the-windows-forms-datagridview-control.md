---
title: DataGridView コントロールのセルに画像を表示する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cells [Windows Forms], displaying images
- data grids [Windows Forms], displaying in grids
- DataGridView control [Windows Forms], displaying images
- data grids [Windows Forms], displaying images in cells
ms.assetid: 53b13d31-1b56-476d-9ab4-18bfac138a22
ms.openlocfilehash: e0e125c816877875b80e0f20887d9beee443577a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740276"
---
# <a name="how-to-display-images-in-cells-of-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールのセルにイメージを表示する
画像またはグラフィックは、データ行に表示できる値の1つです。 多くの場合、これらのグラフィックは従業員の写真または会社のロゴの形になります。  
  
 <xref:System.Windows.Forms.DataGridView> コントロール内にデータを表示する場合、画像の組み込みは簡単です。 <xref:System.Windows.Forms.DataGridView> コントロールは、一部のデータベースで使用される OLE 画像形式だけでなく、<xref:System.Drawing.Image> クラスでサポートされているイメージ形式をネイティブで処理します。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールのデータソースに画像の列がある場合は、<xref:System.Windows.Forms.DataGridView> コントロールによって自動的に表示されます。  
  
 次のコード例では、埋め込みリソースからアイコンを抽出し、イメージ列のすべてのセルに表示するビットマップに変換する方法を示します。 テキストのセル値を対応する画像に置き換える別の例については、「[方法: Windows フォーム DataGridView コントロールでデータの書式をカスタマイズ](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)する」を参照してください。  
  
## <a name="example"></a>例  
 [!code-csharp[System.Windows.Forms.DataGridViewMisc#050](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#050)]
 [!code-vb[System.Windows.Forms.DataGridViewMisc#050](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#050)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- `tree.ico`という名前の埋め込みアイコンリソース。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Windows.Forms?displayProperty=nameWithType>、および <xref:System.Drawing?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでの列、行、およびセルの基本機能](basic-column-row-and-cell-features-wf-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのデータの書式設定をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)
