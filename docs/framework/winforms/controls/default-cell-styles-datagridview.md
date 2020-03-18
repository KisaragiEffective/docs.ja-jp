---
title: デザイナーを使用して DataGridView コントロールの既定のセルスタイルとデータ形式を設定する
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], cell styles
- cells [Windows Forms], setting styles
- data formats
- data [Windows Forms], setting formats
ms.assetid: fc6da49f-8942-41da-b49f-b2afc38cc656
ms.openlocfilehash: ca602fa15e4648550bfa171a9c3abd057e930eca
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731357"
---
# <a name="how-to-set-default-cell-styles-and-data-formats-for-the-windows-forms-datagridview-control-using-the-designer"></a>方法: デザイナーを使用して Windows フォーム DataGridView コントロールの既定のセル スタイルとデータ形式を設定する

<xref:System.Windows.Forms.DataGridView> コントロールを使用すると、コントロール全体、特定の列、行ヘッダー、列ヘッダー、および行と列のヘッダーに対して、既定のセルスタイルとセルデータ形式を指定できます。また、行を交互に使用して、元帳効果を作成することもできます。 コントロール全体に対して設定されている既定のスタイルは、列および交互の行に対して設定された既定のスタイルによって上書きされます。 また、個々の行およびセルのコードで設定したスタイルは、既定のスタイルを上書きします。

セルスタイルの詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。 交互の行のスタイルを設定するには、「[方法:デザイナー](set-alternating-row-styles-for-the-datagrid-using-the-designer.md)を使用して、Windows フォーム DataGridView コントロールに交互の行のスタイルを設定します。

また、<xref:System.Windows.Forms.DataGridView.RowTemplate%2A> プロパティを使用してスタイルを設定し、コントロールに追加されるすべての行に影響を与えることもできます。 行テンプレートの詳細については、「[方法:行テンプレートを使用して、Windows フォーム DataGridView コントロール](use-the-row-template-to-customize-rows-in-the-datagrid.md)の行をカスタマイズします。

次の手順では、<xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法:Windows フォームアプリケーションプロジェクトを作成し、次の操作方法を ](/visualstudio/ide/step-1-create-a-windows-forms-application-project) [ます。Windows フォーム](how-to-add-controls-to-windows-forms.md)にコントロールを追加します。

### <a name="to-set-default-styles-for-all-cells-in-the-control"></a>コントロール内のすべてのセルに既定のスタイルを設定するには

1. デザイナーで <xref:System.Windows.Forms.DataGridView> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A>、<xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A>、または <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A> プロパティの横にある省略記号ボタン ([.. プロパティウィンドウ.]) をクリックし、[、、または] プロパティの横にある [...] を![します](./media/visual-studio-ellipsis-button.png)。 **[CellStyle ビルダー]** ダイアログボックスが表示されます。

3. **プレビュー**ウィンドウを使用して選択内容を確認し、プロパティを設定してスタイルを定義します。

> [!NOTE]
> 視覚スタイルが有効になっている場合、行と列のヘッダー (<xref:System.Windows.Forms.DataGridView.TopLeftHeaderCell%2A>を除く) は、現在のテーマによって自動的にスタイル設定され、<xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A> と <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A> のプロパティ値をオーバーライドします。
>
> デザイナーを使用して、選択した複数の <xref:System.Windows.Forms.DataGridView> コントロールのセルスタイルを設定できますが、変更するセルスタイルプロパティに同じ値が設定されている場合に限ります。 そのプロパティのセルスタイルが異なる場合、 **[CellStyle ビルダー]** ダイアログボックスの **[プロパティ]** ウィンドウは空白になります。

### <a name="to-set-default-styles-for-cells-in-individual-columns"></a>個々の列のセルに既定のスタイルを設定するには

1. デザイナーで <xref:System.Windows.Forms.DataGridView> コントロールを右クリックし、 **[列の編集]** を選択します。

2. **[選択された列]** ボックスの一覧から列を選択します。

3. **[列のプロパティ]** グリッドで、<xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A> プロパティの横にある省略記号ボタン ([...]) をクリックします (![Visual Studio のプロパティウィンドウの [...] ](./media/visual-studio-ellipsis-button.png)を)。 **[CellStyle ビルダー]** ダイアログボックスが表示されます。

4. **プレビュー**ウィンドウを使用して選択内容を確認し、プロパティを設定してスタイルを定義します。

### <a name="to-format-data-in-cells"></a>セルのデータの書式を設定するには

1. 前の手順のいずれかを使用して、既定のセルスタイルプロパティに関連する **[CellStyle ビルダー]** ダイアログボックスを表示します。

2. **[CellStyle ビルダー]** ダイアログボックスで、<xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A> プロパティの横にある省略記号ボタン (![参照ボタン ([...]) をクリックします (Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))。 **[書式文字列]** ダイアログボックスが表示されます。

3. 書式の種類を選択してから、 **[サンプル]** ボックスを使用して選択内容を確認し、種類の詳細 (表示する小数点以下の桁数など) を変更します。

4. Null 値が含まれる可能性のあるデータソースに <xref:System.Windows.Forms.DataGridView> コントロールをバインドする場合は、 **[Null 値]** ボックスに入力します。 この値は、セルの値が null 参照 (Visual Basic では`Nothing`) または <xref:System.DBNull.Value?displayProperty=nameWithType>の場合に表示されます。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [方法: デザイナーを使用して、Windows フォーム DataGridView コントロールに交互の行のスタイルを設定し](set-alternating-row-styles-for-the-datagrid-using-the-designer.md)
- [方法: Windows フォーム アプリケーション プロジェクトの作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [方法: Windows フォーム](how-to-add-controls-to-windows-forms.md) にコントロールを追加する
