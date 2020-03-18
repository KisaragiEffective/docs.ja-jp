---
title: デザイナーを使用して DataGrid コントロールにテーブルと列を追加する
ms.date: 03/30/2017
helpviewer_keywords:
- columns [Windows Forms], adding to DataGrid control
- tables [Windows Forms], adding to DataGrid control
- DataGrid control [Windows Forms], adding tables and columns
ms.assetid: 4a6d1b34-b696-476b-bf8a-57c6230aa9e1
ms.openlocfilehash: fed7465fbe665b1945161653616e3a21640e0b2a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746265"
---
# <a name="how-to-add-tables-and-columns-to-the-windows-forms-datagrid-control-using-the-designer"></a>方法 : デザイナーを使って Windows フォーム DataGrid コントロールにテーブルと列を追加する

> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。

<xref:System.Windows.Forms.DataGridTableStyle> オブジェクトを作成し、それらを <xref:System.Windows.Forms.GridTableStylesCollection> オブジェクトに追加することによって、Windows フォーム <xref:System.Windows.Forms.DataGrid> コントロールにデータを表示できます。これには、<xref:System.Windows.Forms.DataGrid> コントロールの <xref:System.Windows.Forms.DataGrid.TableStyles%2A> プロパティを使用してアクセスします。 各テーブルスタイルには、<xref:System.Windows.Forms.DataGridTableStyle>の <xref:System.Windows.Forms.DataGridTableStyle.MappingName%2A> プロパティに指定されているデータテーブルの内容が表示されます。 既定では、列スタイルが指定されていないテーブルスタイルでは、そのデータテーブル内のすべての列が表示されます。 各 <xref:System.Windows.Forms.DataGridTableStyle>の <xref:System.Windows.Forms.DataGridTableStyle.GridColumnStyles%2A> プロパティを使用してアクセスする <xref:System.Windows.Forms.GridColumnStylesCollection>に <xref:System.Windows.Forms.DataGridColumnStyle> オブジェクトを追加することによって、テーブル内のどの列を表示するかを制限できます。

次の手順では、<xref:System.Windows.Forms.DataGrid> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトを設定する方法については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。 既定では、Visual Studio 2005 では、<xref:System.Windows.Forms.DataGrid> コントロールは**ツールボックス**に含まれていません。 追加の詳細については、「[方法: 項目をツールボックスに追加](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms165355(v=vs.100))する」を参照してください。

### <a name="to-add-a-table-to-the-datagrid-control-in-the-designer"></a>デザイナーで DataGrid コントロールにテーブルを追加するには

1. テーブルにデータを表示するには、最初に <xref:System.Windows.Forms.DataGrid> コントロールをデータセットにバインドする必要があります。 詳細については、「[方法: デザイナーを使用してデータソースに Windows フォーム DataGrid コントロールをバインド](bind-wf-datagrid-control-to-a-data-source-using-the-designer.md)する」を参照してください。

2. プロパティウィンドウで <xref:System.Windows.Forms.DataGrid> コントロールの <xref:System.Windows.Forms.DataGrid.TableStyles%2A> プロパティを選択し、プロパティの横にある省略記号ボタン ([...]) をクリックして、 **DataGridTableStyle Collection エディター**を表示します![(プロパティウィンドウ)。](./media/visual-studio-ellipsis-button.png)

3. コレクションエディターで、 **[追加]** をクリックしてテーブルのスタイルを挿入します。

4. **[OK]** をクリックしてコレクションエディターを閉じ、[<xref:System.Windows.Forms.DataGrid.TableStyles%2A>] プロパティの横にある省略記号ボタンをクリックして再度開きます。

     コレクションエディターを再度開くと、コントロールにバインドされているすべてのデータテーブルが、テーブルスタイルの [<xref:System.Windows.Forms.DataGridTableStyle.MappingName%2A>] プロパティのドロップダウンリストに表示されます。

5. コレクションエディターの **[メンバー]** ボックスで、テーブルのスタイルをクリックします。

6. コレクションエディターの **[プロパティ]** ボックスで、表示するテーブルの <xref:System.Windows.Forms.DataGridTableStyle.MappingName%2A> 値を選択します。

### <a name="to-add-a-column-to-the-datagrid-control-in-the-designer"></a>デザイナーで DataGrid コントロールに列を追加するには

1. **DataGridTableStyle コレクションエディター**の **[メンバー]** ボックスで、適切なテーブルのスタイルを選択します。 コレクションエディターの **[プロパティ]** ボックスで、<xref:System.Windows.Forms.DataGridTableStyle.GridColumnStyles%2A> コレクションを選択してから、省略記号ボタン ([...] プロパティウィンドウ) をクリックします](./media/visual-studio-ellipsis-button.png)。プロパティの横にある [![] をクリックすると、 **system.windows.forms.datagridcolumnstyle> コレクションエディター**が表示されます。

2. コレクションエディターで、 **[追加]** をクリックして列のスタイルを挿入するか、 **[追加]** の横にある下矢印をクリックして列の種類を指定します。

     ドロップダウンボックスで、<xref:System.Windows.Forms.DataGridTextBoxColumn> または <xref:System.Windows.Forms.DataGridBoolColumn> のいずれかの種類を選択できます。

3. [OK] をクリックして**System.windows.forms.datagridcolumnstyle> Collection エディター**を閉じ、[<xref:System.Windows.Forms.DataGridTableStyle.GridColumnStyles%2A>] プロパティの横にある省略記号ボタンをクリックして再度開きます。

     コレクションエディターを再度開くと、バインドされたデータテーブルのデータ列が、列スタイルの [<xref:System.Windows.Forms.DataGridColumnStyle.MappingName%2A>] プロパティのドロップダウンリストに表示されます。

4. コレクションエディターの **[メンバー]** ボックスで、列のスタイルをクリックします。

5. コレクションエディターの **[プロパティ]** ボックスで、表示する列の <xref:System.Windows.Forms.DataGridColumnStyle.MappingName%2A> 値を選択します。

## <a name="see-also"></a>参照

- [DataGrid コントロール](datagrid-control-windows-forms.md)
- [方法 : Windows フォーム DataGrid コントロールの列を削除するまたは非表示にする](how-to-delete-or-hide-columns-in-the-windows-forms-datagrid-control.md)
