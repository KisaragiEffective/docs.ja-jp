---
title: 行テンプレートを使用して DataGridView コントロールの行をカスタマイズする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data grids [Windows Forms], customizing rows
- DataGridView control [Windows Forms], customizing rows
ms.assetid: 6db61607-7e57-4a84-8d63-9d6a7ed7f9ff
ms.openlocfilehash: 35cb95f22c0caa654bf149b5fc4fd0395696a411
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728385"
---
# <a name="how-to-use-the-row-template-to-customize-rows-in-the-windows-forms-datagridview-control"></a>方法 : 行テンプレートを使用して Windows フォーム DataGridView コントロールの行をカスタマイズする
<xref:System.Windows.Forms.DataGridView> コントロールでは、データバインディングを使用するか、使用する既存の行を指定せずに <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A?displayProperty=nameWithType> メソッドを呼び出すと、コントロールに追加されるすべての行の基礎として行テンプレートが使用されます。  
  
 行テンプレートを使用すると、<xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> プロパティによって提供される行の外観と動作をより細かく制御できます。 行テンプレートを使用すると、<xref:System.Windows.Forms.DataGridViewRow.DefaultCellStyle%2A>を含む任意の <xref:System.Windows.Forms.DataGridViewRow> プロパティを設定できます。  
  
 場合によっては、行テンプレートを使用して特定の効果を実現する必要があります。 たとえば、行の高さ情報を <xref:System.Windows.Forms.DataGridViewCellStyle>に格納することはできません。そのため、行テンプレートを使用して、すべての行で使用される既定の高さを変更する必要があります。 行テンプレートは、<xref:System.Windows.Forms.DataGridViewRow> から派生した独自のクラスを作成する場合にも役立ちます。新しい行がコントロールに追加されるときに、カスタム型を使用する必要があります。  
  
> [!NOTE]
> 行テンプレートは、行が追加された場合にのみ使用されます。 行テンプレートを変更して既存の行を変更することはできません。  
  
### <a name="to-use-the-row-template"></a>行テンプレートを使用するには  
  
- <xref:System.Windows.Forms.DataGridView.RowTemplate%2A?displayProperty=nameWithType> プロパティから取得したオブジェクトのプロパティを設定します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.RowTemplate#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.RowTemplate/CPP/datagridviewrowtemplate.cpp#1)]
     [!code-csharp[System.Windows.Forms.DataGridView.RowTemplate#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.RowTemplate/CS/datagridviewrowtemplate.cs#1)]
     [!code-vb[System.Windows.Forms.DataGridView.RowTemplate#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.RowTemplate/VB/datagridviewrowtemplate.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Windows.Forms.DataGridView> という名前の `dataGridView1` コントロール。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Drawing?displayProperty=nameWithType>、および <xref:System.Windows.Forms?displayProperty=nameWithType> の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- <xref:System.Windows.Forms.DataGridViewRow>
- <xref:System.Windows.Forms.DataGridView.RowTemplate%2A?displayProperty=nameWithType>
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
