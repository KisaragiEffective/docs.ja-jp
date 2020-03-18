---
title: DataGridView コントロールの基本的な書式設定とスタイル設定
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], formatting and styling
- data grids [Windows Forms], formatting
ms.assetid: b9b90836-1f56-4aa9-8db8-edc78fe830e8
ms.openlocfilehash: d295718b7bd4f3bc4c5f63b6e6cb911f733525fe
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731996"
---
# <a name="basic-formatting-and-styling-in-the-windows-forms-datagridview-control"></a>Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定
`DataGridView` コントロールを使用すると、セルの基本的な外観と、セル値の表示形式を簡単に定義できます。 さまざまな `DataGridView` コントロールのプロパティを使用してアクセスする `DataGridViewCellStyle` オブジェクトのプロパティを設定することによって、個々のセル、特定の列や行のセル、またはコントロール内のすべてのセルの外観と書式設定スタイルを定義できます。 また、`CellFormatting` イベントを処理することによって、セルの値などの要因に基づいて、これらのスタイルを動的に変更することもできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法: Windows フォーム DataGridView コントロールの境界線とグリッド線のスタイルを変更する](change-the-border-and-gridline-styles-in-the-datagrid.md)  
 コントロールの境界線とセル間の境界線の外観を定義する `DataGridView` プロパティを設定する方法について説明します。  
  
 [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)  
 `DataGridViewCellStyle` クラスと、その型のプロパティがコントロールでのセルの表示方法を定義する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールの既定のセル スタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)  
 `DataGridViewCellStyle` のプロパティを使用して、特定の行と列、およびコントロール全体のセルの既定の外観を定義する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールのデータの書式を設定する](how-to-format-data-in-the-windows-forms-datagridview-control.md)  
 `DataGridViewCellStyle` のプロパティを使用して、セルの表示値の書式を設定する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールのフォントと色のスタイルを設定する](how-to-set-font-and-color-styles-in-the-windows-forms-datagridview-control.md)  
 `DefaultCellStyle` プロパティを使用して、コントロールのすべてのセルの基本的な表示特性を設定する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールに交互の行のスタイルを設定する](how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control.md)  
 別の行で表示される行を交互に使用して、コントロールに元帳に似た効果を作成する方法について説明します。  
  
 [方法: 行テンプレートを使用して Windows フォーム DataGridView コントロールの行をカスタマイズする](use-the-row-template-to-customize-rows-in-the-datagrid.md)  
 `RowTemplate` プロパティを使用して、コントロールのすべての行に使用される行のプロパティを設定する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.DataGridView>  
 <xref:System.Windows.Forms.DataGridView> コントロールのリファレンス ドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridViewCellStyle>  
 <xref:System.Windows.Forms.DataGridViewCellStyle> クラスのリファレンスドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridView.CellFormatting>  
 <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントのリファレンスドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridView.RowTemplate%2A>  
 <xref:System.Windows.Forms.DataGridView.RowTemplate%2A> プロパティのリファレンスドキュメントを提供します。  
  
## <a name="related-sections"></a>関連項目  
 [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)  
 <xref:System.Windows.Forms.DataGridView> のセルおよび行のカスタム描画と、セル、列、および行の派生型の作成について説明するトピックを示します。  
  
 [Windows フォーム DataGridView コントロールでの列、行、およびセルの基本機能](basic-column-row-and-cell-features-wf-datagridview-control.md)  
 一般的に使用されるセル、行、および列のプロパティについて説明するトピックを示します。  
  
## <a name="see-also"></a>参照

- [DataGridView コントロール](datagridview-control-windows-forms.md)
