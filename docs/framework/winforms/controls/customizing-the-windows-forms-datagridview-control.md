---
title: DataGridView コントロールのカスタマイズ
ms.date: 03/30/2017
helpviewer_keywords:
- data grids [Windows Forms], customization
- DataGridView control [Windows Forms], customization
ms.assetid: 01ea5d4c-a736-4596-b0e9-a67a1b86e15f
ms.openlocfilehash: 348c78d091679418f2452326555d49229bd2a8ea
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744028"
---
# <a name="customizing-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールのカスタマイズ
`DataGridView` コントロールには、セル、行、および列の外観と基本動作 (ルックアンドフィール) を調整するために使用できるいくつかのプロパティが用意されています。 ただし、<xref:System.Windows.Forms.DataGridViewCellStyle> クラスの機能を超える特別なニーズがある場合は、コントロールのオーナー描画を実装したり、カスタムセル、列、および行を作成してその機能を拡張したりすることもできます。  
  
 セルと行を自分で描画するには、さまざまな `DataGridView` の描画イベントを処理できます。 既存の機能を変更したり、新しい機能を提供したりするには、既存の `DataGridViewCell`、`DataGridViewColumn`、および `DataGridViewRow` 型から派生した独自の型を作成できます。 また、セルが編集モードのときに選択したコントロールを表示する派生型を作成することによって、新しい編集機能を提供することもできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法 : Windows フォームの DataGridView コントロールのセルの外観をカスタマイズする](customize-the-appearance-of-cells-in-the-datagrid.md)  
 セルを手動で描画するために <xref:System.Windows.Forms.DataGridView.CellPainting> イベントを処理する方法について説明します。  
  
 [方法: Windows フォームの DataGridView コントロールの行の外観をカスタマイズする](customize-the-appearance-of-rows-in-the-datagrid.md)  
 カスタム、グラデーションの背景、および複数の列にまたがるコンテンツを使用して行を描画するために、<xref:System.Windows.Forms.DataGridView.RowPrePaint> イベントと <xref:System.Windows.Forms.DataGridView.RowPostPaint> イベントを処理する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールのセルと列を、それぞれの動作と外観を拡張してカスタマイズする](customize-cells-and-columns-in-the-datagrid-by-extending-behavior.md)  
 マウスポインターがセル上にあるときにセルを強調表示するために `DataGridViewCell` および `DataGridViewColumn` から派生したカスタム型を作成する方法について説明します。  
  
 [方法: Windows フォーム DataGridView コントロールのボタン列にあるボタンを無効にする](disable-buttons-in-a-button-column-in-the-datagrid.md)  
 ボタン列に無効なボタンを表示するために <xref:System.Windows.Forms.DataGridViewButtonCell> および <xref:System.Windows.Forms.DataGridViewButtonColumn> から派生したカスタム型を作成する方法について説明します。  
  
 [方法: Windows フォーム DataGridView Cells でコントロールをホストする](how-to-host-controls-in-windows-forms-datagridview-cells.md)  
 セルが編集モードのときに <xref:System.Windows.Forms.DateTimePicker> コントロールを表示するために、`IDataGridViewEditingControl` インターフェイスを実装し、`DataGridViewCell` および `DataGridViewColumn` から派生したカスタム型を作成する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.DataGridView>  
 <xref:System.Windows.Forms.DataGridView> コントロールのリファレンス ドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridViewCell>  
 <xref:System.Windows.Forms.DataGridViewCell> クラスのリファレンスドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridViewRow>  
 <xref:System.Windows.Forms.DataGridViewRow> クラスのリファレンスドキュメントを提供します。  
  
 <xref:System.Windows.Forms.DataGridViewColumn>  
 <xref:System.Windows.Forms.DataGridViewColumn> クラスのリファレンスドキュメントを提供します。  
  
 <xref:System.Windows.Forms.IDataGridViewEditingControl>  
 <xref:System.Windows.Forms.IDataGridViewEditingControl> インターフェイスのリファレンスドキュメントを提供します。  
  
## <a name="related-sections"></a>関連項目  
 [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)  
 コントロールの基本の外観およびセル データの書式設定を変更する方法を説明するトピックを示します。  
  
## <a name="see-also"></a>参照

- [DataGridView コントロール](datagridview-control-windows-forms.md)
- [Windows フォーム DataGridView コントロールの列型](column-types-in-the-windows-forms-datagridview-control.md)
