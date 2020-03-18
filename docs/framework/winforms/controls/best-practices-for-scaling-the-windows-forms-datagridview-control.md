---
title: DataGridView コントロールのスケーリングのベストプラクティス
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], row sharing
- data grids [Windows Forms], best practices
- DataGridView control [Windows Forms], shared rows
- DataGridView control [Windows Forms], best practices
- best practices [Windows Forms], dataGridView control
- DataGridView control [Windows Forms], scaling
ms.assetid: 8321a8a6-6340-4fd1-b475-fa090b905aaf
ms.openlocfilehash: 63500a79def89510b4bc7a436abc4620a7265a79
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744123"
---
# <a name="best-practices-for-scaling-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールを拡張するための推奨される手順

<xref:System.Windows.Forms.DataGridView> コントロールは、最大のスケーラビリティを提供するように設計されています。 大量のデータを表示する必要がある場合は、このトピックで説明されているガイドラインに従って、大量のメモリを消費したり、ユーザーインターフェイス (UI) の応答性を低下させたりすることは避けてください。 このトピックでは、次の問題について説明します。

- セルスタイルの効率的な使用

- ショートカットメニューの効率的な使用

- 自動サイズ変更の効率的な使用

- 選択したセル、行、および列のコレクションを効率的に使用する

- 共有行の使用

- 行が非共有になるのを防ぐ

特別なパフォーマンスのニーズがある場合は、仮想モードを実装し、独自のデータ管理操作を提供できます。 詳細については、「 [Windows フォーム DataGridView コントロールのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="using-cell-styles-efficiently"></a>セルスタイルの効率的な使用

各セル、行、および列には、独自のスタイル情報を設定できます。 スタイル情報は <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトに格納されます。 特に大量のデータを使用する場合、多くの個別の <xref:System.Windows.Forms.DataGridView> 要素に対してセルスタイルオブジェクトを作成することは非効率的です。 パフォーマンスへの影響を回避するには、次のガイドラインに従います。

- 個々の <xref:System.Windows.Forms.DataGridViewCell> または <xref:System.Windows.Forms.DataGridViewRow> オブジェクトのセルスタイルプロパティを設定しないようにします。 これには、<xref:System.Windows.Forms.DataGridView.RowTemplate%2A> プロパティによって指定された row オブジェクトも含まれます。 行テンプレートから複製された新しい行はそれぞれ、テンプレートのセルスタイルオブジェクトの独自のコピーを受け取ります。 最大限のスケーラビリティを実現するには、<xref:System.Windows.Forms.DataGridView> レベルでセルスタイルプロパティを設定します。 たとえば、<xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType> プロパティではなく、<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType> プロパティを設定します。

- 既定の書式設定以外の書式設定が必要なセルがある場合は、セル、行、または列のグループ間で同じ <xref:System.Windows.Forms.DataGridViewCellStyle> インスタンスを使用します。 個々のセル、行、および列に対して <xref:System.Windows.Forms.DataGridViewCellStyle> 型のプロパティを直接設定することは避けてください。 セルスタイル共有の例については、「[方法: Windows フォーム DataGridView コントロールの既定のセルスタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)」を参照してください。 <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントハンドラーを処理することによって、セルスタイルを個別に設定するときにパフォーマンスが低下しないようにすることもできます。 例については、「[方法: Windows フォーム DataGridView コントロールでデータの書式をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)」を参照してください。

- セルのスタイルを決定するときは、<xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType> プロパティではなく、<xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A?displayProperty=nameWithType> プロパティを使用します。 プロパティがまだ使用されていない場合は、<xref:System.Windows.Forms.DataGridViewCell.Style%2A> プロパティにアクセスすると、<xref:System.Windows.Forms.DataGridViewCellStyle> クラスの新しいインスタンスが作成されます。 また、行、列、またはコントロールからいくつかのスタイルが継承されている場合、このオブジェクトにはセルの完全なスタイル情報が含まれていない可能性があります。 セルスタイルの継承の詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="using-shortcut-menus-efficiently"></a>ショートカットメニューの効率的な使用

各セル、行、および列には、独自のショートカットメニューを設定できます。 <xref:System.Windows.Forms.DataGridView> コントロールのショートカットメニューは、<xref:System.Windows.Forms.ContextMenuStrip> コントロールによって表されます。 セルスタイルオブジェクトと同様に、多くの個別の <xref:System.Windows.Forms.DataGridView> 要素のショートカットメニューを作成すると、パフォーマンスが低下します。 このようにペナルティを回避するには、次のガイドラインに従います。

- 個々のセルおよび行のショートカットメニューを作成しないようにします。 これには、新しい行がコントロールに追加されたときに、そのショートカットメニューと共に複製される行テンプレートが含まれます。 最大限のスケーラビリティを実現するには、コントロールの <xref:System.Windows.Forms.Control.ContextMenuStrip%2A> プロパティのみを使用して、コントロール全体に1つのショートカットメニューを指定します。

- 複数の行またはセルに複数のショートカットメニューが必要な場合は、<xref:System.Windows.Forms.DataGridView.CellContextMenuStripNeeded> イベントまたは <xref:System.Windows.Forms.DataGridView.RowContextMenuStripNeeded> イベントを処理します。 これらのイベントを使用すると、ショートカットメニューオブジェクトを自分で管理できるため、パフォーマンスを調整できます。

## <a name="using-automatic-resizing-efficiently"></a>自動サイズ変更の効率的な使用

セルの内容全体がクリッピングなしで表示されるように、行、列、およびヘッダーをセルの内容の変更に合わせて自動的にサイズ変更することができます。 サイズ変更モードを変更すると、行、列、およびヘッダーのサイズも変更される可能性があります。 正しいサイズを判断するには、<xref:System.Windows.Forms.DataGridView> コントロールで、対応が必要な各セルの値を確認する必要があります。 大きなデータセットを使用する場合、自動サイズ変更が発生すると、この分析によってコントロールのパフォーマンスが低下する可能性があります。 パフォーマンスの低下を回避するには、次のガイドラインに従います。

- 大量の行を含む <xref:System.Windows.Forms.DataGridView> コントロールでは、自動サイズ設定を使用しないようにしてください。 自動サイズ変更を使用する場合は、表示されている行に基づいてサイズを変更するだけです。 仮想モードでも表示されている行のみを使用します。

  - 行と列の場合は、<xref:System.Windows.Forms.DataGridViewAutoSizeRowsMode>、<xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>、および <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode> 列挙型の `DisplayedCells` フィールドまたは `DisplayedCellsExceptHeaders` フィールドを使用します。

  - 行ヘッダーの場合は、<xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode> 列挙の <xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode.AutoSizeToDisplayedHeaders> または <xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode.AutoSizeToFirstHeader> フィールドを使用します。

- 最大限のスケーラビリティを実現するには、自動サイズ変更をオフにし、プログラムによるサイズ変更を使用します。

詳細については、「 [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="using-the-selected-cells-rows-and-columns-collections-efficiently"></a>選択したセル、行、および列のコレクションを効率的に使用する

<xref:System.Windows.Forms.DataGridView.SelectedCells%2A> コレクションは、大きな選択によって効率的には実行されません。 <xref:System.Windows.Forms.DataGridView.SelectedRows%2A> コレクションと <xref:System.Windows.Forms.DataGridView.SelectedColumns%2A> コレクションも非効率的ですが、一般的な <xref:System.Windows.Forms.DataGridView> コントロールのセルよりも多くの行があり、行よりも多くの列が少ないため、効率が悪くなります。 これらのコレクションを操作するときのパフォーマンスの低下を回避するには、次のガイドラインに従います。

- <xref:System.Windows.Forms.DataGridView.SelectedCells%2A> コレクションの内容にアクセスする前に、<xref:System.Windows.Forms.DataGridView> 内のすべてのセルが選択されているかどうかを確認するには、<xref:System.Windows.Forms.DataGridView.AreAllCellsSelected%2A> メソッドの戻り値を確認します。 ただし、この方法では、行が共有されなくなる可能性があることに注意してください。 詳細については、次のセクションを参照してください。

- <xref:System.Windows.Forms.DataGridViewSelectedCellCollection?displayProperty=nameWithType> の <xref:System.Collections.ICollection.Count%2A> プロパティを使用して、選択したセルの数を決定するのは避けてください。 代わりに、<xref:System.Windows.Forms.DataGridView.GetCellCount%2A?displayProperty=nameWithType> メソッドを使用し、<xref:System.Windows.Forms.DataGridViewElementStates.Selected?displayProperty=nameWithType> の値を渡します。 同様に、選択した行および列のコレクションにアクセスするのではなく、<xref:System.Windows.Forms.DataGridViewRowCollection.GetRowCount%2A?displayProperty=nameWithType> および <xref:System.Windows.Forms.DataGridViewColumnCollection.GetColumnCount%2A?displayProperty=nameWithType> メソッドを使用して、選択した要素の数を決定します。

- セルベースの選択モードは避けてください。 代わりに、<xref:System.Windows.Forms.DataGridView.SelectionMode%2A?displayProperty=nameWithType> プロパティを <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType> または <xref:System.Windows.Forms.DataGridViewSelectionMode.FullColumnSelect?displayProperty=nameWithType>に設定します。

## <a name="using-shared-rows"></a>共有行の使用

効率的なメモリ使用は、共有行によって <xref:System.Windows.Forms.DataGridView> 制御で実現されます。 行は、<xref:System.Windows.Forms.DataGridViewRow> クラスのインスタンスを共有することで、その外観と動作に関する情報を可能な限り共有します。

行インスタンスを共有するとメモリが節約されますが、行は簡単に非共有になります。 たとえば、ユーザーが直接セルを操作すると、その行の共有が解除されます。 このことは避けられないため、このトピックのガイドラインは、非常に大量のデータを扱う場合、およびプログラムを実行するたびにユーザーが比較的小さなデータの一部を操作する場合にのみ役立ちます。

セルに値が含まれている場合、バインドされていない <xref:System.Windows.Forms.DataGridView> コントロールで行を共有することはできません。 <xref:System.Windows.Forms.DataGridView> コントロールが外部データソースにバインドされている場合、または仮想モードを実装して独自のデータソースを指定した場合、セル値はセルオブジェクトではなく、コントロールの外部に格納されるので、行を共有することができます。

行オブジェクトを共有できるのは、すべてのセルの状態が行の状態とセルを含む列の状態から決まる場合だけです。 行と列の状態から推測できないようにセルの状態を変更すると、その行を共有することはできません。

たとえば、次のような状況では、行を共有することはできません。

- この行には、選択された列に含まれていない1つの選択されたセルが含まれています。

- この行には、<xref:System.Windows.Forms.DataGridViewCell.ToolTipText%2A> または <xref:System.Windows.Forms.DataGridViewCell.ContextMenuStrip%2A> プロパティが設定されたセルが含まれています。

- この行には、<xref:System.Windows.Forms.DataGridViewComboBoxCell.Items%2A> プロパティが設定された <xref:System.Windows.Forms.DataGridViewComboBoxCell> が含まれています。

バインドモードまたは仮想モードでは、<xref:System.Windows.Forms.DataGridView.CellToolTipTextNeeded> イベントと <xref:System.Windows.Forms.DataGridView.CellContextMenuStripNeeded> イベントを処理することによって、個々のセルのツールヒントやショートカットメニューを提供できます。

<xref:System.Windows.Forms.DataGridView> コントロールは、行が <xref:System.Windows.Forms.DataGridViewRowCollection>に追加されるたびに、共有行を自動的に使用しようとします。 次のガイドラインを使用して、行が共有されていることを確認します。

- <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A> メソッドの `Add(Object[])` オーバーロードと、<xref:System.Windows.Forms.DataGridView.Rows%2A?displayProperty=nameWithType> コレクションの <xref:System.Windows.Forms.DataGridViewRowCollection.Insert%2A> メソッドの `Insert(Object[])` オーバーロードを呼び出さないでください。 これらのオーバーロードは、自動的に非共有行を作成します。

- 次の場合は、<xref:System.Windows.Forms.DataGridView.RowTemplate%2A?displayProperty=nameWithType> プロパティで指定された行を共有できることを確認してください。

  - `Add()` を呼び出す場合、または <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A> メソッドの `Add(Int32)` オーバーロード、または <xref:System.Windows.Forms.DataGridViewRowCollection.Insert%2A> コレクションの <xref:System.Windows.Forms.DataGridView.Rows%2A?displayProperty=nameWithType> メソッドの `Insert(Int32,Int32)` オーバーロード。

  - <xref:System.Windows.Forms.DataGridView.RowCount%2A?displayProperty=nameWithType> プロパティの値を増やす場合。

  - <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType> プロパティを設定する場合。

- <xref:System.Windows.Forms.DataGridViewRowCollection.InsertCopies%2A> コレクションの <xref:System.Windows.Forms.DataGridViewRowCollection.AddCopy%2A>、<xref:System.Windows.Forms.DataGridViewRowCollection.AddCopies%2A>、<xref:System.Windows.Forms.DataGridViewRowCollection.InsertCopy%2A>、および <xref:System.Windows.Forms.DataGridView.Rows%2A?displayProperty=nameWithType> の各メソッドを呼び出すときに、`indexSource` パラメーターによって示される行を共有できることを確認してください。

- <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A> メソッド、<xref:System.Windows.Forms.DataGridViewRowCollection.AddRange%2A> メソッド、<xref:System.Windows.Forms.DataGridViewRowCollection.Insert%2A> メソッドの `Insert(Int32,DataGridViewRow)` オーバーロード、および <xref:System.Windows.Forms.DataGridViewRowCollection.InsertRange%2A> コレクションの <xref:System.Windows.Forms.DataGridView.Rows%2A?displayProperty=nameWithType> メソッドの `Add(DataGridViewRow)` オーバーロードを呼び出すときに、指定された行を共有できることを確認してください。

行が共有されているかどうかを判断するには、<xref:System.Windows.Forms.DataGridViewRowCollection.SharedRow%2A?displayProperty=nameWithType> メソッドを使用して行オブジェクトを取得し、オブジェクトの <xref:System.Windows.Forms.DataGridViewBand.Index%2A> プロパティを確認します。 共有行には、常に、<xref:System.Windows.Forms.DataGridViewBand.Index%2A> のプロパティ値が-1 に設定されています。

## <a name="preventing-rows-from-becoming-unshared"></a>行が非共有になるのを防ぐ

共有行は、コードまたはユーザー操作の結果として非共有になることがあります。 パフォーマンスへの影響を回避するには、行が共有されなくなるのを避ける必要があります。 アプリケーションの開発中に、<xref:System.Windows.Forms.DataGridView.RowUnshared> のイベントを処理して、行の共有を解除するタイミングを判断できます。 これは、行共有の問題をデバッグする場合に便利です。

行が非共有にならないようにするには、次のガイドラインに従います。

- <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションにインデックスを作成したり、`foreach` ループで反復処理したりしないでください。 通常、行に直接アクセスする必要はありません。 行を操作する <xref:System.Windows.Forms.DataGridView> メソッドは、行インスタンスではなく行インデックス引数を受け取ります。 さらに、行関連のイベントのハンドラーは、行プロパティを持つイベント引数オブジェクトを受け取ります。このオブジェクトを使用して、共有を解除することなく行を操作できます。

- 行オブジェクトにアクセスする必要がある場合は、<xref:System.Windows.Forms.DataGridViewRowCollection.SharedRow%2A?displayProperty=nameWithType> メソッドを使用して、行の実際のインデックスを渡します。 ただし、このメソッドを使用して取得した共有行オブジェクトを変更すると、このオブジェクトを共有するすべての行が変更されることに注意してください。 ただし、新しいレコードの行は他の行と共有されないため、他の行を変更しても影響はありません。 また、共有行によって表される異なる行のショートカットメニューが異なる場合もあります。 共有行インスタンスから正しいショートカットメニューを取得するには、<xref:System.Windows.Forms.DataGridViewRow.GetContextMenuStrip%2A> メソッドを使用して、行の実際のインデックスを渡します。 代わりに、共有行の <xref:System.Windows.Forms.DataGridViewRow.ContextMenuStrip%2A> プロパティにアクセスすると、共有行インデックス-1 が使用され、正しいショートカットメニューは取得されません。

- <xref:System.Windows.Forms.DataGridViewRow.Cells%2A?displayProperty=nameWithType> コレクションのインデックス作成は避けてください。 セルに直接アクセスすると、親の行が非共有になり、新しい <xref:System.Windows.Forms.DataGridViewRow>がインスタンス化されます。 セル関連イベントのハンドラーは、セルプロパティを使用してイベント引数オブジェクトを受け取ります。このオブジェクトを使用すると、行の共有を解除せずにセルを操作できます。 また、<xref:System.Windows.Forms.DataGridView.CurrentCellAddress%2A> プロパティを使用して、セルに直接アクセスせずに、現在のセルの行インデックスと列インデックスを取得することもできます。

- セルベースの選択モードは避けてください。 これらのモードでは、行が共有されなくなります。 代わりに、<xref:System.Windows.Forms.DataGridView.SelectionMode%2A?displayProperty=nameWithType> プロパティを <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType> または <xref:System.Windows.Forms.DataGridViewSelectionMode.FullColumnSelect?displayProperty=nameWithType>に設定します。

- <xref:System.Windows.Forms.DataGridViewRowCollection.CollectionChanged?displayProperty=nameWithType> イベントまたは <xref:System.Windows.Forms.DataGridView.RowStateChanged?displayProperty=nameWithType> イベントは処理しないようにします。 これらのイベントにより、行が共有されなくなります。 また、これらのイベントを発生させる <xref:System.Windows.Forms.DataGridViewRowCollection.OnCollectionChanged%2A?displayProperty=nameWithType> または <xref:System.Windows.Forms.DataGridView.OnRowStateChanged%2A?displayProperty=nameWithType> メソッドを呼び出さないでください。

- <xref:System.Windows.Forms.DataGridView.SelectionMode%2A?displayProperty=nameWithType> プロパティの値が <xref:System.Windows.Forms.DataGridViewSelectionMode.FullColumnSelect>、<xref:System.Windows.Forms.DataGridViewSelectionMode.ColumnHeaderSelect>、<xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect>、または <xref:System.Windows.Forms.DataGridViewSelectionMode.RowHeaderSelect>の場合は、<xref:System.Windows.Forms.DataGridView.SelectedCells%2A?displayProperty=nameWithType> コレクションにアクセスしないでください。 これにより、選択したすべての行の共有が解除されます。

- <xref:System.Windows.Forms.DataGridView.AreAllCellsSelected%2A?displayProperty=nameWithType> メソッドを呼び出さないでください。 このメソッドは、行が共有されなくなる可能性があります。

- <xref:System.Windows.Forms.DataGridView.SelectionMode%2A?displayProperty=nameWithType> プロパティ値が <xref:System.Windows.Forms.DataGridViewSelectionMode.CellSelect>場合は、<xref:System.Windows.Forms.DataGridView.SelectAll%2A?displayProperty=nameWithType> メソッドを呼び出さないでください。 これにより、すべての行が非共有になります。

- 列の対応するプロパティが `true`に設定されている場合は、セルの <xref:System.Windows.Forms.DataGridViewCell.ReadOnly%2A> または <xref:System.Windows.Forms.DataGridViewCell.Selected%2A> プロパティを `false` に設定しないでください。 これにより、すべての行が非共有になります。

- <xref:System.Windows.Forms.DataGridViewRowCollection.List%2A?displayProperty=nameWithType> プロパティにはアクセスしないでください。 これにより、すべての行が非共有になります。

- <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドの `Sort(IComparer)` オーバーロードは呼び出さないでください。 カスタム比較子を使用して並べ替えると、すべての行の共有が解除されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [Windows フォーム DataGridView コントロールでのパフォーマンス チューニング](performance-tuning-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでの仮想モード](virtual-mode-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの既定のセル スタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)
