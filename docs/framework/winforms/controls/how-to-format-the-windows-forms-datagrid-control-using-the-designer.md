---
title: デザイナーを使用した DataGrid コントロールの書式設定
ms.date: 03/30/2017
helpviewer_keywords:
- columns [Windows Forms], DataGrid controls
- colors [Windows Forms], applying to DataGrid controls
- DataGrid control [Windows Forms], formatting
- DataGrid control [Windows Forms], default styles
- tables [Windows Forms], formatting in DataGrid control
- formatting [Windows Forms]
ms.assetid: 533b9814-6124-49dc-9fda-085f1502609f
ms.openlocfilehash: 548acac0fc7724490bfe89927ec0662b3488c230
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736797"
---
# <a name="how-to-format-the-windows-forms-datagrid-control-using-the-designer"></a>方法 : デザイナーを使って Windows フォーム DataGrid コントロールの書式を設定する

> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。

<xref:System.Windows.Forms.DataGrid> コントロールのさまざまな部分に異なる色を適用すると、情報を読みやすく、解釈しやすくなります。 色を行と列に適用できます。 行と列は、自由に表示したり、表示したりすることもできます。

<xref:System.Windows.Forms.DataGrid> コントロールの書式設定には、次の3つの基本的な側面があります。

- プロパティを設定して、データを表示する既定のスタイルを設定できます。

- そのベースから、実行時に特定のテーブルを表示する方法をカスタマイズできます。

- 最後に、データグリッドに表示される列と、表示される色やその他の書式を変更できます。

データグリッドを書式設定するための最初の手順として、<xref:System.Windows.Forms.DataGrid> 自体のプロパティを設定できます。 これらの色と形式の選択により、表示されるデータテーブルと列に応じて変更を行うことができるベースが形成されます。

次の手順では、<xref:System.Windows.Forms.DataGrid> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。 Visual Studio 2005 では、<xref:System.Windows.Forms.DataGrid> コントロールは、既定では**ツールボックス**に含まれていません。 詳細については、「[方法: ツールボックスに項目を追加](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms165355(v=vs.100))する」を参照してください。

### <a name="to-establish-a-default-style-for-the-datagrid-control"></a>DataGrid コントロールの既定のスタイルを設定するには

1. <xref:System.Windows.Forms.DataGrid> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、必要に応じて次のプロパティを設定します。

    |プロパティ|[説明]|
    |--------------|-----------------|
    |<xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A>|`BackColor` プロパティは、グリッドの偶数行の色を定義します。 <xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A> プロパティを別の色に設定すると、他のすべての行がこの新しい色 (行1、3、5など) に設定されます。|
    |<xref:System.Windows.Forms.DataGrid.BackColor%2A>|グリッドの偶数行の背景色 (行0、2、4、6など) です (行0、2、4、6など)。|
    |<xref:System.Windows.Forms.DataGrid.BackgroundColor%2A>|<xref:System.Windows.Forms.DataGrid.BackColor%2A> と <xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A> のプロパティによってグリッド内の行の色が決まりますが、<xref:System.Windows.Forms.DataGrid.BackgroundColor%2A> プロパティによって、行領域の外側の領域の色が決まります。これは、グリッドが一番下にスクロールしたときにのみ表示されるか、またはグリッドに含まれる行が少ない場合にのみ表示されます。|
    |<xref:System.Windows.Forms.DataGrid.BorderStyle%2A>|<xref:System.Windows.Forms.BorderStyle> 列挙値の1つである、グリッドの境界線スタイル。|
    |<xref:System.Windows.Forms.DataGrid.CaptionBackColor%2A>|グリッドのすぐ上に表示されるグリッドのウィンドウキャプションの背景色。|
    |<xref:System.Windows.Forms.DataGrid.CaptionFont%2A>|グリッドの上部にあるキャプションのフォント。|
    |<xref:System.Windows.Forms.DataGrid.CaptionForeColor%2A>|グリッドのウィンドウキャプションの背景色。|
    |<xref:System.Windows.Forms.Control.Font%2A>|グリッド内のテキストを表示するために使用されるフォント。|
    |<xref:System.Windows.Forms.DataGrid.ForeColor%2A>|データグリッドの行のデータによって表示されるフォントの色。|
    |<xref:System.Windows.Forms.DataGrid.GridLineColor%2A>|データグリッドのグリッド線の色。|
    |<xref:System.Windows.Forms.DataGrid.GridLineStyle%2A>|グリッドのセルを区切る線のスタイル。 <xref:System.Windows.Forms.DataGridLineStyle> 列挙値のいずれかです。|
    |<xref:System.Windows.Forms.DataGrid.HeaderBackColor%2A>|行ヘッダーと列ヘッダーの背景色。|
    |<xref:System.Windows.Forms.DataGrid.HeaderFont%2A>|列ヘッダーに使用されるフォントです。|
    |<xref:System.Windows.Forms.DataGrid.HeaderForeColor%2A>|グリッドの列ヘッダーの前景色。列ヘッダーのテキスト、プラス記号 (+) とマイナス記号 (-) の各グリフを含み、複数の関連テーブルが表示されたときに行を展開したり折りたたんだりします。|
    |<xref:System.Windows.Forms.DataGrid.LinkColor%2A>|子テーブルへのリンク、リレーションシップ名など、データグリッド内のすべてのリンクのテキストの色。|
    |<xref:System.Windows.Forms.DataGrid.ParentRowsBackColor%2A>|子テーブルでは、親行の背景色です。|
    |<xref:System.Windows.Forms.DataGrid.ParentRowsForeColor%2A>|子テーブルでは、これが親行の前景色です。|
    |<xref:System.Windows.Forms.DataGrid.ParentRowsLabelStyle%2A>|<xref:System.Windows.Forms.DataGridParentRowsLabelStyle> 列挙体を使って、テーブル名と列名を親行に表示するかどうかを決定します。|
    |<xref:System.Windows.Forms.DataGrid.PreferredColumnWidth%2A>|グリッドの列の既定の幅 (ピクセル単位)。 <xref:System.Windows.Forms.DataGrid.DataSource%2A> プロパティと <xref:System.Windows.Forms.DataGrid.DataMember%2A> プロパティ (個別に、または <xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッド) をリセットする前にこのプロパティを設定するか、またはプロパティを無効にします。<br /><br /> プロパティを0未満の値に設定することはできません。|
    |<xref:System.Windows.Forms.DataGrid.PreferredRowHeight%2A>|グリッド内の行の高さ (ピクセル単位)。 <xref:System.Windows.Forms.DataGrid.DataSource%2A> プロパティと <xref:System.Windows.Forms.DataGrid.DataMember%2A> プロパティ (個別に、または <xref:System.Windows.Forms.DataGrid.SetDataBinding%2A> メソッド) をリセットする前にこのプロパティを設定するか、またはプロパティを無効にします。<br /><br /> プロパティを0未満の値に設定することはできません。|
    |<xref:System.Windows.Forms.DataGrid.RowHeaderWidth%2A>|グリッドの行ヘッダーの幅。|
    |<xref:System.Windows.Forms.DataGrid.SelectionBackColor%2A>|行またはセルが選択されている場合、これが背景色になります。|
    |<xref:System.Windows.Forms.DataGrid.SelectionForeColor%2A>|行またはセルが選択されている場合、これは前景色です。|

    > [!NOTE]
    > コントロールの色をカスタマイズする場合、色の選択が不適切であるため (赤や緑など)、コントロールにアクセスできないようにすることができます。 この問題を回避するには、**システムカラー**パレットで使用可能な色を使用します。

    次の手順では、データテーブルにバインドされた <xref:System.Windows.Forms.DataGrid> コントロールが必要です。 詳細については、「[方法: Windows フォーム DataGrid コントロールをデータソースにバインドする](how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)」を参照してください。

### <a name="to-set-the-table-and-column-style-of-a-data-table-at-design-time"></a>デザイン時にデータテーブルのテーブルと列のスタイルを設定するには

1. フォームの <xref:System.Windows.Forms.DataGrid> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.DataGrid.TableStyles%2A> プロパティを選択し、**省略記号**(省略記号ボタン ([...]) をクリックします (Visual](./media/visual-studio-ellipsis-button.png)Studio のプロパティウィンドウの![)] ボタンをクリックします。

3. **[DataGridTableStyle コレクションエディター]** ダイアログボックスで、 **[追加]** をクリックして、テーブルスタイルをコレクションに追加します。

     **DataGridTableStyle Collection エディター**では、テーブルスタイルの追加と削除、表示とレイアウトのプロパティの設定、およびテーブルスタイルのマッピング名の設定を行うことができます。

4. <xref:System.Windows.Forms.DataGridTableStyle.MappingName%2A> プロパティに、各テーブルスタイルのマッピング名を設定します。

     マッピング名は、どのテーブルスタイルをどのテーブルに使用するかを指定するために使用されます。

5. **DataGridTableStyle Collection エディター**で、[<xref:System.Windows.Forms.DataGridTableStyle.GridColumnStyles%2A>] プロパティを選択し、省略記号ボタン![(Visual Studio のプロパティウィンドウの省略記号ボタン ([...]) をクリックします。](./media/visual-studio-ellipsis-button.png))。

6. **[System.windows.forms.datagridcolumnstyle> コレクションエディター]** ダイアログボックスで、作成したテーブルのスタイルに列のスタイルを追加します。

     **System.windows.forms.datagridcolumnstyle> Collection エディター**では、列のスタイルの追加と削除、表示とレイアウトのプロパティの設定、およびデータ列のマッピング名と書式設定文字列の設定を行うことができます。

    > [!NOTE]
    > 書式設定文字列の詳細については、「[型の書式設定](../../../standard/base-types/formatting-types.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.GridTableStylesCollection>
- <xref:System.Windows.Forms.GridColumnStylesCollection>
- <xref:System.Windows.Forms.DataGrid>
- [方法 : Windows フォーム DataGrid コントロールの列を削除するまたは非表示にする](how-to-delete-or-hide-columns-in-the-windows-forms-datagrid-control.md)
- [DataGrid コントロール](datagrid-control-windows-forms.md)
