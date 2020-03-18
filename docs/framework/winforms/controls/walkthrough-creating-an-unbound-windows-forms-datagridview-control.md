---
title: バインドしていない DataGridView コントロールを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data [Windows Forms], displaying without binding to data source
- DataGridView control [Windows Forms], unbound data
- DataGridView control [Windows Forms], displaying data without binding to a data source
- data [Windows Forms], unbound
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: 5a8d6afa-1b4b-4b24-8db8-501086ffdebe
ms.openlocfilehash: ceb75d4ee845d1f643d4d88d5a9f1bde73edcc70
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740179"
---
# <a name="walkthrough-creating-an-unbound-windows-forms-datagridview-control"></a>チュートリアル : バインドされていない Windows フォーム DataGridView コントロールの作成
データベースから生成されていない表形式のデータを表示することがよくあります。 たとえば、文字列の2次元配列の内容を表示することができます。 <xref:System.Windows.Forms.DataGridView> クラスを使用すると、データソースにバインドせずに、簡単かつ高度にカスタマイズ可能な方法でデータを表示できます。 このチュートリアルでは、<xref:System.Windows.Forms.DataGridView> コントロールにデータを設定し、"非バインド" モードで行の追加と削除を管理する方法について説明します。 既定では、ユーザーは新しい行を追加できます。 行が追加されないようにするには、<xref:System.Windows.Forms.DataGridView.AllowUserToAddRows%2A> プロパティを `false`に設定します。  
  
 このトピックのコードを単一のリストとしてコピーする方法については、「方法: バインドされていない[Windows フォーム DataGridView コントロールを作成](how-to-create-an-unbound-windows-forms-datagridview-control.md)する」を参照してください。  
  
## <a name="creating-the-form"></a>フォームの作成  
  
#### <a name="to-use-an-unbound-datagridview-control"></a>バインドされていない DataGridView コントロールを使用するには  
  
1. <xref:System.Windows.Forms.Form> から派生し、次の変数宣言と `Main` メソッドを含むクラスを作成します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#01](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#01)]
     [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#01](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#01)]  
    [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#02](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#02)]
    [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#02](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#02)]  
  
2. フォームのクラス定義に `SetupLayout` メソッドを実装して、フォームのレイアウトを設定します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#20)]
     [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#20)]  
  
3. <xref:System.Windows.Forms.DataGridView> の列およびプロパティを設定する `SetupDataGridView` メソッドを作成します。  
  
     このメソッドは、まず、フォームの <xref:System.Windows.Forms.Control.Controls%2A> コレクションに <xref:System.Windows.Forms.DataGridView> コントロールを追加します。 次に、<xref:System.Windows.Forms.DataGridView.ColumnCount%2A> プロパティを使用して、表示する列の数を設定します。 列ヘッダーの既定のスタイルは、<xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A> プロパティによって返される <xref:System.Windows.Forms.DataGridViewCellStyle> の <xref:System.Windows.Forms.DataGridViewCellStyle.BackColor%2A>、<xref:System.Windows.Forms.DataGridViewCellStyle.ForeColor%2A>、および <xref:System.Windows.Forms.DataGridViewCellStyle.Font%2A> の各プロパティを設定することによって設定されます。  
  
     レイアウトと外観のプロパティが設定され、列名が割り当てられます。 このメソッドが終了すると、<xref:System.Windows.Forms.DataGridView> コントロールを設定できるようになります。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#30)]
     [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#30)]  
  
4. <xref:System.Windows.Forms.DataGridView> コントロールに行を追加する `PopulateDataGridView` メソッドを作成します。  
  
     各行は、曲とそれに関連付けられた情報を表します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#40](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#40)]
     [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#40](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#40)]  
  
5. ユーティリティメソッドを配置すると、イベントハンドラーをアタッチできます。  
  
     **[追加]** ボタンと **[削除]** ボタンの <xref:System.Windows.Forms.Control.Click> イベント、フォームの <xref:System.Windows.Forms.Form.Load> イベント、および <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントを処理します。  
  
     **[追加]** ボタンの <xref:System.Windows.Forms.Control.Click> イベントが発生すると、新しい空の行が <xref:System.Windows.Forms.DataGridView>に追加されます。  
  
     **[削除]** ボタンの <xref:System.Windows.Forms.Control.Click> イベントが発生すると、選択した行が削除されます。ただし、新しいレコードの行でない限り、ユーザーは新しい行を追加できます。 この行は常に、<xref:System.Windows.Forms.DataGridView> コントロールの最後の行になります。  
  
     フォームの <xref:System.Windows.Forms.Form.Load> イベントが発生すると、`SetupLayout`、`SetupDataGridView`、および `PopulateDataGridView` ユーティリティメソッドが呼び出されます。  
  
     <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントが発生した場合、セルの値を解析できない場合を除き、`Date` 列の各セルは長い日付形式として書式設定されます。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewSimpleUnbound#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/CS/simpleunbound.cs#10)]
     [!code-vb[System.Windows.Forms.DataGridViewSimpleUnbound#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewSimpleUnbound/VB/simpleunbound.vb#10)]  
  
## <a name="testing-the-application"></a>アプリケーションのテスト  
 フォームをテストして、期待どおりに動作することを確認します。  
  
#### <a name="to-test-the-form"></a>フォームをテストするには  
  
- F5 キーを押してアプリケーションを実行します。  
  
     `PopulateDataGridView`に表示されている曲を表示する <xref:System.Windows.Forms.DataGridView> コントロールが表示されます。 **[行の追加]** ボタンを使用して新しい行を追加できます。また、 **[行の削除]** ボタンを使用して、選択した行を削除できます。 バインドされていない <xref:System.Windows.Forms.DataGridView> コントロールはデータストアであり、そのデータは、<xref:System.Data.DataSet> や配列などの外部ソースからは独立しています。  
  
## <a name="next-steps"></a>次の手順  
 このアプリケーションでは、<xref:System.Windows.Forms.DataGridView> コントロールの機能についての基本的な理解を得ることができます。 <xref:System.Windows.Forms.DataGridView> コントロールの外観と動作は、次のいくつかの方法でカスタマイズできます。  
  
- 罫線とヘッダーのスタイルを変更します。 詳細については、「[方法: Windows フォーム DataGridView コントロールの境界線とグリッド線のスタイルを変更する](change-the-border-and-gridline-styles-in-the-datagrid.md)」を参照してください。  
  
- <xref:System.Windows.Forms.DataGridView> コントロールへのユーザー入力を有効または制限します。 詳細については、「[方法: Windows フォーム Datagridview コントロールで行の追加と削除を回避](prevent-row-addition-and-deletion-datagridview.md)する」および「[方法: Windows フォーム Datagridview コントロールで列を読み取り](how-to-make-columns-read-only-in-the-windows-forms-datagridview-control.md)専用にする」を参照してください。  
  
- データベース関連のエラーについては、ユーザー入力を確認してください。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーの処理](handling-errors-that-occur-during-data-entry-in-the-datagrid.md)」を参照してください。  
  
- 仮想モードを使用して非常に大きなデータセットを処理します。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。  
  
- セルの外観をカスタマイズします。 詳細については、「[方法: Windows フォーム Datagridview コントロールのセルの外観をカスタマイズ](customize-the-appearance-of-cells-in-the-datagrid.md)する」および「[方法: Windows フォーム Datagridview コントロールの既定のセルスタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [方法: 連結されていない Windows フォーム DataGridView コントロールを作成する](how-to-create-an-unbound-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)
