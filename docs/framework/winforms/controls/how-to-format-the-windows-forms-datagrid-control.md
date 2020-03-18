---
title: DataGrid コントロールの書式設定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- columns [Windows Forms], DataGrid control
- colors [Windows Forms], applying to DataGrid controls
- DataGrid control [Windows Forms], formatting
- columns [Windows Forms], formatting in DataGrid control
- DataGrid control [Windows Forms], default styles
- tables [Windows Forms], formatting in DataGrid control
- formatting [Windows Forms]
ms.assetid: a50fcc3b-8abf-47ec-9029-7f268af4ddb1
ms.openlocfilehash: 30342a89f3176255fa7217ccbbbd91c166ff7f35
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732962"
---
# <a name="how-to-format-the-windows-forms-datagrid-control"></a>方法 : Windows フォーム DataGrid コントロールの書式を設定する
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。  
  
 <xref:System.Windows.Forms.DataGrid> コントロールのさまざまな部分に異なる色を適用すると、情報を読みやすく、解釈しやすくなります。 色を行と列に適用できます。 行と列は、自由に表示したり、表示したりすることもできます。  
  
 <xref:System.Windows.Forms.DataGrid> コントロールの書式設定には、3つの基本的な側面があります。 プロパティを設定して、データを表示する既定のスタイルを設定できます。 そのベースから、実行時に特定のテーブルを表示する方法をカスタマイズできます。 最後に、データグリッドに表示される列と、表示される色やその他の書式を変更できます。  
  
 データグリッドを書式設定するための最初の手順として、<xref:System.Windows.Forms.DataGrid> 自体のプロパティを設定できます。 これらの色と形式の選択により、表示されるデータテーブルと列に応じて変更を行うことができるベースが形成されます。  
  
### <a name="to-establish-a-default-style-for-the-datagrid-control"></a>DataGrid コントロールの既定のスタイルを設定するには  
  
1. 必要に応じて、次のプロパティを設定します。  
  
    |プロパティ|[説明]|  
    |--------------|-----------------|  
    |<xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A>|<xref:System.Windows.Forms.DataGrid.BackColor%2A> プロパティは、グリッドの偶数行の色を定義します。 <xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A> プロパティを別の色に設定すると、他のすべての行がこの新しい色 (行1、3、5など) に設定されます。|  
    |<xref:System.Windows.Forms.DataGrid.BackColor%2A>|グリッドの偶数行の背景色 (行0、2、4、6など) です (行0、2、4、6など)。|  
    |<xref:System.Windows.Forms.DataGrid.BackgroundColor%2A>|<xref:System.Windows.Forms.DataGrid.BackColor%2A> プロパティと <xref:System.Windows.Forms.DataGrid.AlternatingBackColor%2A> プロパティによってグリッド内の行の色が決まりますが、<xref:System.Windows.Forms.DataGrid.BackgroundColor%2A> プロパティは、グリッドが一番下にスクロールしたときにのみ表示される非行領域の色を決定します。また、グリッドに含まれる行が少ない場合にのみ表示されます。|  
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
    |<xref:System.Windows.Forms.DataGrid.HeaderForeColor%2A>|列ヘッダーテキストとプラス/マイナスグリフを含む、グリッドの列ヘッダーの前景色。複数の関連テーブルが表示されている場合は、行を展開します。|  
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
    > コントロールの色をカスタマイズする場合は、色の選択が不適切であるため (赤や緑など)、コントロールにアクセスできないようにする必要があることに注意してください。 この問題を回避するには、**システムカラー**パレットで使用可能な色を使用します。  
  
     次の手順では、フォームにデータテーブルにバインドされた <xref:System.Windows.Forms.DataGrid> コントロールがあることを前提としています。 詳細については、「[データソースへの Windows フォーム DataGrid コントロールのバインド](how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)」を参照してください。  
  
### <a name="to-set-the-table-and-column-style-of-a-data-table-programmatically"></a>データテーブルのテーブルと列のスタイルをプログラムで設定するには  
  
1. 新しいテーブルスタイルを作成し、そのプロパティを設定します。  
  
2. 列スタイルを作成し、そのプロパティを設定します。  
  
3. 列スタイルをテーブルスタイルの列スタイルのコレクションに追加します。  
  
4. データグリッドのテーブルスタイルのコレクションにテーブルのスタイルを追加します。  
  
5. 次の例では、新しい <xref:System.Windows.Forms.DataGridTableStyle> のインスタンスを作成し、その <xref:System.Windows.Forms.DataGridTableStyle.MappingName%2A> プロパティを設定します。  
  
6. **Gridcolumnstyle**の新しいインスタンスを作成し、その**MappingName** (およびその他のレイアウトと表示プロパティ) を設定します。  
  
7. 作成する各列スタイルについて、手順 2. ~ 6. を繰り返します。  
  
     次の例は、名前が列に表示されるため、<xref:System.Windows.Forms.DataGridTextBoxColumn> の作成方法を示しています。 さらに、列スタイルをテーブルスタイルの <xref:System.Windows.Forms.GridColumnStylesCollection> に追加し、テーブルスタイルをデータグリッドの <xref:System.Windows.Forms.GridTableStylesCollection> に追加します。  
  
    ```vb  
    Private Sub CreateAuthorFirstNameColumn()  
       ' Add a GridTableStyle and set the MappingName   
       ' to the name of the DataTable.  
       Dim TSAuthors As New DataGridTableStyle()  
       TSAuthors.MappingName = "Authors"  
  
       ' Add a GridColumnStyle and set the MappingName   
       ' to the name of a DataColumn in the DataTable.   
       ' Set the HeaderText and Width properties.   
       Dim TCFirstName As New DataGridTextBoxColumn()  
       TCFirstName.MappingName = "AV_FName"  
       TCFirstName.HeaderText = "First Name"  
       TCFirstName.Width = 75  
       TSAuthors.GridColumnStyles.Add(TCFirstName)  
  
       ' Add the DataGridTableStyle instance to   
       ' the GridTableStylesCollection.   
       myDataGrid.TableStyles.Add(TSAuthors)  
    End Sub  
    ```  
  
    ```csharp  
    private void addCustomDataTableStyle()  
    {  
       // Add a GridTableStyle and set the MappingName   
       // to the name of the DataTable.  
       DataGridTableStyle TSAuthors = new DataGridTableStyle();  
       TSAuthors.MappingName = "Authors";  
  
       // Add a GridColumnStyle and set the MappingName   
       // to the name of a DataColumn in the DataTable.   
       // Set the HeaderText and Width properties.   
       DataGridColumnStyle TCFirstName = new DataGridTextBoxColumn();  
       TCFirstName.MappingName = " AV_FName";  
       TCFirstName.HeaderText = "First Name";  
       TCFirstName.Width = 75;  
       TSAuthors.GridColumnStyles.Add(TCFirstName);  
  
       // Add the DataGridTableStyle instance to   
       // the GridTableStylesCollection.   
       dataGrid1.TableStyles.Add(TSAuthors);  
    }  
    ```  
  
    ```cpp  
    private:  
       void addCustomDataTableStyle()  
       {  
          // Add a GridTableStyle and set the MappingName   
          // to the name of the DataTable.  
          DataGridTableStyle^ TSAuthors = new DataGridTableStyle();  
          TSAuthors->MappingName = "Authors";  
  
          // Add a GridColumnStyle and set the MappingName   
          // to the name of a DataColumn in the DataTable.   
          // Set the HeaderText and Width properties.   
          DataGridColumnStyle^ TCFirstName = gcnew DataGridTextBoxColumn();  
          TCFirstName->MappingName = "AV_FName";  
          TCFirstName->HeaderText = "First Name";  
          TCFirstName->Width = 75;  
          TSAuthors->GridColumnStyles->Add(TCFirstName);  
  
          // Add the DataGridTableStyle instance to   
          // the GridTableStylesCollection.   
          dataGrid1->TableStyles->Add(TSAuthors);  
       }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.GridTableStylesCollection>
- <xref:System.Windows.Forms.GridColumnStylesCollection>
- <xref:System.Windows.Forms.DataGrid>
- [方法 : Windows フォーム DataGrid コントロールの列を削除するまたは非表示にする](how-to-delete-or-hide-columns-in-the-windows-forms-datagrid-control.md)
- [DataGrid コントロール](datagrid-control-windows-forms.md)
