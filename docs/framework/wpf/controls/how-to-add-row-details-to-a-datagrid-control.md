---
title: '方法: DataGrid コントロールに行の詳細を追加する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataTemplate [WPF], DataGrid
- row details [WPF], DataGrid
- DataGrid [WPF], row details
ms.assetid: 0bdc6f50-9b4c-483f-9df6-a47a1fde998b
ms.openlocfilehash: 4db414e1907d42071f7251c390077d4020fa577c
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559678"
---
# <a name="how-to-add-row-details-to-a-datagrid-control"></a>方法: DataGrid コントロールに行の詳細を追加する
<xref:System.Windows.Controls.DataGrid> コントロールを使用する場合は、行の詳細セクションを追加することで、データ表示をカスタマイズできます。 行の詳細セクションを追加すると、必要に応じて表示または縮小されたテンプレートの一部のデータをグループ化できます。 たとえば、<xref:System.Windows.Controls.DataGrid>の各行のデータの概要のみを表示する <xref:System.Windows.Controls.DataGrid> に行の詳細を追加できますが、ユーザーが行を選択すると、より多くのデータフィールドが表示されます。 <xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> プロパティの [行の詳細] セクションのテンプレートを定義します。 次の図は、行の詳細セクションの例を示しています。  
  
 ![行の詳細が表示されている DataGrid](./media/ndp-rowdetails.png "NDP_RowDetails")  
  
 行の詳細テンプレートは、インライン XAML またはリソースとして定義します。 次の手順では、両方の方法を示します。 リソースとして追加されたデータテンプレートは、テンプレートを再作成することなく、プロジェクト全体で使用できます。 インライン XAML として追加されたデータテンプレートには、それが定義されているコントロールからのみアクセスできます。  
  
### <a name="to-display-row-details-by-using-inline-xaml"></a>インライン XAML を使用して行の詳細を表示するには  
  
1. データソースからデータを表示する <xref:System.Windows.Controls.DataGrid> を作成します。  
  
2. <xref:System.Windows.Controls.DataGrid> 要素に、<xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> 要素を追加します。  
  
3. 行の詳細セクションの外観を定義する <xref:System.Windows.DataTemplate> を作成します。  
  
     次の XAML は、<xref:System.Windows.Controls.DataGrid> と、インライン <xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> を定義する方法を示しています。 <xref:System.Windows.Controls.DataGrid> には、各行に3つの値が表示され、行が選択されると3つの値が表示されます。  
  
     [!code-xaml[DataGrid_RowDetails#1](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_rowdetails/cs/mainwindow.xaml#1)]  
  
     次のコードは、<xref:System.Windows.Controls.DataGrid>に表示されるデータを選択するために使用されるクエリを示しています。 この例では、クエリは顧客情報を含むエンティティからデータを選択します。  
  
     [!code-csharp[DataGrid_RowDetails#2](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_rowdetails/cs/mainwindow.xaml.cs#2)]
     [!code-vb[DataGrid_RowDetails#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/datagrid_rowdetails/vb/mainwindow.xaml.vb#2)]  
  
### <a name="to-display-row-details-by-using-a-resource"></a>リソースを使用して行の詳細を表示するには  
  
1. データソースからデータを表示する <xref:System.Windows.Controls.DataGrid> を作成します。  
  
2. <xref:System.Windows.Window> コントロールや <xref:System.Windows.Controls.Page> コントロールなどの <xref:System.Windows.FrameworkElement.Resources%2A> 要素をルート要素に追加するか、app.xaml (または app.xaml) ファイルの <xref:System.Windows.Application> クラスに <xref:System.Windows.Application.Resources%2A> 要素を追加します。  
  
3. Resources 要素で、行の詳細セクションの外観を定義する <xref:System.Windows.DataTemplate> を作成します。  
  
     次の XAML は、<xref:System.Windows.Application> クラスで定義されている <xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> を示しています。  
  
     [!code-xaml[DataGrid_RowDetails#3](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_rowdetails/cs/app.xaml#3)]  
  
4. <xref:System.Windows.DataTemplate>で、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)を、データテンプレートを一意に識別する値に設定します。  
  
5. <xref:System.Windows.Controls.DataGrid> 要素で、<xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> プロパティを前の手順で定義したリソースに設定します。 リソースを静的リソースとして割り当てます。  
  
     次の XAML は、前の例のリソースに設定された <xref:System.Windows.Controls.DataGrid.RowDetailsTemplate%2A> プロパティを示しています。  
  
     [!code-xaml[DataGrid_RowDetails#4](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_rowdetails/cs/window2.xaml#4)]  
  
### <a name="to-set-visibility-and-prevent-horizontal-scrolling-for-row-details"></a>表示を設定し、行の詳細を水平方向にスクロールできないようにするには  
  
1. 必要に応じて、[<xref:System.Windows.Controls.DataGrid.RowDetailsVisibilityMode%2A>] プロパティを <xref:System.Windows.Controls.DataGridRowDetailsVisibilityMode> の値に設定します。  
  
     既定では、この値は <xref:System.Windows.Controls.DataGridRowDetailsVisibilityMode.VisibleWhenSelected> に設定されます。 <xref:System.Windows.Controls.DataGridRowDetailsVisibilityMode.Visible> に設定して、すべての行の詳細を表示し <xref:System.Windows.Controls.DataGridRowDetailsVisibilityMode.Collapsed> たり、すべての行の詳細を非表示にしたりすることができます。  
  
2. 必要に応じて、[<xref:System.Windows.Controls.DataGrid.AreRowDetailsFrozen%2A>] プロパティを `true` に設定して、[行の詳細] セクションが水平方向にスクロールしないようにします。
