---
title: '方法: DataGrid コントロールでデータをグループ化、並べ替え、およびフィルター処理する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGrid [WPF], sort
- DataGrid [WPF], group
- DataGrid [WPF], filter
ms.assetid: 03345e85-89e3-4aec-9ed0-3b80759df770
ms.openlocfilehash: 622b64fd7738b02cd72131e7e9fe91c04314b1d0
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559475"
---
# <a name="how-to-group-sort-and-filter-data-in-the-datagrid-control"></a>方法: DataGrid コントロールでデータをグループ化、並べ替え、およびフィルター処理する

多くの場合、データをグループ化、並べ替え、およびフィルター処理することで、<xref:System.Windows.Controls.DataGrid> のデータをさまざまな方法で表示すると便利です。 <xref:System.Windows.Controls.DataGrid>内のデータをグループ化、並べ替え、およびフィルター処理するには、これらの関数をサポートする <xref:System.Windows.Data.CollectionView> にバインドします。 その後、基になるソースデータに影響を与えることなく、<xref:System.Windows.Data.CollectionView> のデータを操作できます。 コレクションビューでの変更は、<xref:System.Windows.Controls.DataGrid> ユーザーインターフェイス (UI) に反映されます。

<xref:System.Windows.Data.CollectionView> クラスは、<xref:System.Collections.IEnumerable> インターフェイスを実装するデータソースに対してグループ化および並べ替え機能を提供します。 <xref:System.Windows.Data.CollectionViewSource> クラスを使用すると、XAML から <xref:System.Windows.Data.CollectionView> のプロパティを設定できます。

この例では、`Task` オブジェクトのコレクションが <xref:System.Windows.Data.CollectionViewSource>にバインドされています。 <xref:System.Windows.Data.CollectionViewSource> は、<xref:System.Windows.Controls.DataGrid>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> として使用されます。 グループ化、並べ替え、およびフィルター処理は <xref:System.Windows.Data.CollectionViewSource> で実行され、<xref:System.Windows.Controls.DataGrid> UI に表示されます。

![DataGrid でのグループ化されたデータ](././media/wpf-datagridgroups.png "WPF_DataGridGroups")DataGrid でのグループ化されたデータ

## <a name="using-a-collectionviewsource-as-an-itemssource"></a>ItemsSource としての CollectionViewSource の使用

<xref:System.Windows.Controls.DataGrid> コントロールのデータをグループ化、並べ替え、およびフィルター処理するには、これらの関数をサポートする <xref:System.Windows.Data.CollectionView> に <xref:System.Windows.Controls.DataGrid> をバインドします。 この例では、<xref:System.Windows.Controls.DataGrid> は `Task` オブジェクトの <xref:System.Collections.Generic.List%601> に対してこれらの関数を提供する <xref:System.Windows.Data.CollectionViewSource> にバインドされています。

### <a name="to-bind-a-datagrid-to-a-collectionviewsource"></a>DataGrid を CollectionViewSource にバインドするには

1. <xref:System.Collections.IEnumerable> インターフェイスを実装するデータコレクションを作成します。

    <xref:System.Collections.Generic.List%601> を使用してコレクションを作成する場合は、<xref:System.Collections.Generic.List%601>のインスタンスをインスタンス化するのではなく、<xref:System.Collections.Generic.List%601> から継承する新しいクラスを作成する必要があります。 これにより、XAML でコレクションにデータをバインドできるようになります。

    > [!NOTE]
    > コレクション内のオブジェクトは、プロパティの変更や編集に正しく応答するため <xref:System.Windows.Controls.DataGrid> に、<xref:System.ComponentModel.INotifyPropertyChanged> 変更されたインターフェイスと <xref:System.ComponentModel.IEditableObject> インターフェイスを実装する必要があります。 詳細については、「[プロパティの変更通知を実装する](../data/how-to-implement-property-change-notification.md)」を参照してください。

    [!code-csharp[DataGrid_GroupSortFilter#101](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#101)]
    [!code-vb[DataGrid_GroupSortFilter#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#101)]

2. XAML で、コレクションクラスのインスタンスを作成し、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)を設定します。

3. XAML で、<xref:System.Windows.Data.CollectionViewSource> クラスのインスタンスを作成し、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)を設定し、コレクションクラスのインスタンスを <xref:System.Windows.Data.CollectionViewSource.Source%2A>として設定します。

    [!code-xaml[DataGrid_GroupSortFilter#201](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/WindowSnips1.xaml#201)]

4. <xref:System.Windows.Controls.DataGrid> クラスのインスタンスを作成し、<xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティを <xref:System.Windows.Data.CollectionViewSource>に設定します。

    [!code-xaml[DataGrid_GroupSortFilter#002](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#002)]

5. コードから <xref:System.Windows.Data.CollectionViewSource> にアクセスするには、<xref:System.Windows.Data.CollectionViewSource.GetDefaultView%2A> メソッドを使用して <xref:System.Windows.Data.CollectionViewSource>への参照を取得します。

    [!code-csharp[DataGrid_GroupSortFilter#102](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#102)]
    [!code-vb[DataGrid_GroupSortFilter#102](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#102)]

## <a name="grouping-items-in-a-datagrid"></a>DataGrid でのアイテムのグループ化

<xref:System.Windows.Controls.DataGrid>で項目をグループ化する方法を指定するには、<xref:System.Windows.Data.PropertyGroupDescription> の種類を使用して、ソースビュー内の項目をグループ化します。

### <a name="to-group-items-in-a-datagrid-using-xaml"></a>XAML を使用して DataGrid の項目をグループ化するには

1. グループ化するプロパティを指定する <xref:System.Windows.Data.PropertyGroupDescription> を作成します。 プロパティは、XAML またはコードで指定できます。

   1. XAML で、<xref:System.Windows.Data.PropertyGroupDescription.PropertyName%2A> をグループ化するプロパティの名前に設定します。

   2. コードで、group by にプロパティの名前をコンストラクターに渡します。

2. <xref:System.Windows.Data.CollectionViewSource.GroupDescriptions%2A?displayProperty=nameWithType> コレクションに <xref:System.Windows.Data.PropertyGroupDescription> を追加します。

3. <xref:System.Windows.Data.CollectionViewSource.GroupDescriptions%2A> コレクションに <xref:System.Windows.Data.PropertyGroupDescription> のインスタンスを追加して、グループ化のレベルを追加します。

    [!code-xaml[DataGrid_GroupSortFilter#012](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#012)]
    [!code-csharp[DataGrid_GroupSortFilter#112](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#112)]
    [!code-vb[DataGrid_GroupSortFilter#112](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#112)]

4. グループを削除するには、<xref:System.Windows.Data.CollectionViewSource.GroupDescriptions%2A> コレクションから <xref:System.Windows.Data.PropertyGroupDescription> を削除します。

5. すべてのグループを削除するには、<xref:System.Windows.Data.CollectionViewSource.GroupDescriptions%2A> コレクションの <xref:System.Collections.ObjectModel.Collection%601.Clear%2A> メソッドを呼び出します。

    [!code-csharp[DataGrid_GroupSortFilter#114](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#114)]
    [!code-vb[DataGrid_GroupSortFilter#114](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#114)]

項目が <xref:System.Windows.Controls.DataGrid>にグループ化されている場合は、各グループの外観を指定する <xref:System.Windows.Controls.GroupStyle> を定義できます。 <xref:System.Windows.Controls.GroupStyle> を適用するには、DataGrid の <xref:System.Windows.Controls.ItemsControl.GroupStyle%2A> コレクションに追加します。 複数のレベルのグループ化を使用している場合は、各グループレベルに異なるスタイルを適用できます。 スタイルは、定義されている順序で適用されます。 たとえば、2つのスタイルを定義した場合、最初のスタイルがトップレベルの行グループに適用されます。 2番目のスタイルは、2番目のレベルのすべての行グループに適用されます。 <xref:System.Windows.Controls.GroupStyle> の <xref:System.Windows.FrameworkElement.DataContext%2A> は、グループが表す <xref:System.Windows.Data.CollectionViewGroup> です。

### <a name="to-change-the-appearance-of-row-group-headers"></a>行グループヘッダーの外観を変更するには

1. 行グループの外観を定義する <xref:System.Windows.Controls.GroupStyle> を作成します。

2. `<DataGrid.GroupStyle>` タグ内に <xref:System.Windows.Controls.GroupStyle> を配置します。

    [!code-xaml[DataGrid_GroupSortFilter#003](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#003)]

## <a name="sorting-items-in-a-datagrid"></a>DataGrid でのアイテムの並べ替え

<xref:System.Windows.Controls.DataGrid>での項目の並べ替え方法を指定するには、<xref:System.ComponentModel.SortDescription> の種類を使用して、ソースビュー内の項目を並べ替えます。

### <a name="to-sort-items-in-a-datagrid"></a>DataGrid 内の項目を並べ替えるには

1. 並べ替えの基準となるプロパティを指定する <xref:System.ComponentModel.SortDescription> を作成します。 プロパティは、XAML またはコードで指定できます。

    1. XAML で、<xref:System.ComponentModel.SortDescription.PropertyName%2A> を、並べ替えの基準となるプロパティの名前に設定します。

    2. コードで、プロパティの名前を渡して、並べ替え、および <xref:System.ComponentModel.ListSortDirection> をコンストラクターに渡します。

2. <xref:System.Windows.Data.CollectionViewSource.SortDescriptions%2A?displayProperty=nameWithType> コレクションに <xref:System.ComponentModel.SortDescription> を追加します。

3. 追加のプロパティで並べ替えるために、<xref:System.ComponentModel.SortDescription> のインスタンスを <xref:System.Windows.Data.CollectionViewSource.SortDescriptions%2A> コレクションに追加します。

    [!code-xaml[DataGrid_GroupSortFilter#011](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#011)]
    [!code-csharp[DataGrid_GroupSortFilter#211](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/WindowSnips1.xaml.cs#211)]
    [!code-vb[DataGrid_GroupSortFilter#211](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#211)]

## <a name="filtering-items-in-a-datagrid"></a>DataGrid での項目のフィルター処理

<xref:System.Windows.Data.CollectionViewSource>を使用して <xref:System.Windows.Controls.DataGrid> 内の項目をフィルター処理するには、<xref:System.Windows.Data.CollectionViewSource.Filter?displayProperty=nameWithType> イベントのハンドラーにフィルター処理ロジックを指定します。

### <a name="to-filter-items-in-a-datagrid"></a>DataGrid の項目をフィルター処理するには

1. <xref:System.Windows.Data.CollectionViewSource.Filter?displayProperty=nameWithType> イベントのハンドラーを追加します。

2. <xref:System.Windows.Data.CollectionViewSource.Filter> イベントハンドラーで、フィルター処理ロジックを定義します。

    フィルターは、ビューが更新されるたびに適用されます。

    [!code-xaml[DataGrid_GroupSortFilter#013](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#013)]
    [!code-csharp[DataGrid_GroupSortFilter#113](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#113)]
    [!code-vb[DataGrid_GroupSortFilter#113](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#113)]

または、フィルター処理ロジックを提供するメソッドを作成し、フィルターを適用するように <xref:System.Windows.Data.CollectionView.Filter%2A?displayProperty=nameWithType> プロパティを設定することによって、<xref:System.Windows.Controls.DataGrid> 内の項目をフィルター処理できます。 このメソッドの例については、「[ビュー内のデータをフィルター処理](../data/how-to-filter-data-in-a-view.md)する」を参照してください。

## <a name="example"></a>使用例

次の例では、<xref:System.Windows.Data.CollectionViewSource> 内の `Task` データをグループ化、並べ替え、およびフィルター処理し、<xref:System.Windows.Controls.DataGrid>でグループ化、並べ替え、およびフィルター選択された `Task` データを表示します。 <xref:System.Windows.Data.CollectionViewSource> は、<xref:System.Windows.Controls.DataGrid>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> として使用されます。 グループ化、並べ替え、およびフィルター処理は <xref:System.Windows.Data.CollectionViewSource> で実行され、<xref:System.Windows.Controls.DataGrid> UI に表示されます。

この例をテストするには、プロジェクト名と一致するように DGGroupSortFilterExample 名を調整する必要があります。 Visual Basic を使用する場合は、<xref:System.Windows.Window> のクラス名を次のように変更する必要があります。

`<Window x:Class="MainWindow"`

[!code-xaml[DataGrid_GroupSortFilter#000](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml#000)]
[!code-csharp[DataGrid_GroupSortFilter#100](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_GroupSortFilter/CS/MainWindow.xaml.cs#100)]
[!code-vb[DataGrid_GroupSortFilter#100](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_GroupSortFilter/VB/MainWindow.xaml.vb#100)]

## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [ObservableCollection を作成およびバインドする](../data/how-to-create-and-bind-to-an-observablecollection.md)
- [ビュー内のデータをフィルター処理する](../data/how-to-filter-data-in-a-view.md)
- [ビュー内のデータの並べ替え](../data/how-to-sort-data-in-a-view.md)
- [XAML でビューを使用してデータの並べ替えおよびグループ化を行う](../data/how-to-sort-and-group-data-using-a-view-in-xaml.md)
