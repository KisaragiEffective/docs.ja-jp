---
title: DataGridView コントロールの選択モード
ms.date: 03/30/2017
helpviewer_keywords:
- selection [Windows Forms], modes in DataGridView control
- DataGridView control [Windows Forms], selection mode
ms.assetid: a3ebfd3d-0525-479d-9d96-d9e017289b36
ms.openlocfilehash: e20bf6307d77bf189b698e847c6b855c249dc3c1
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743043"
---
# <a name="selection-modes-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールの選択モード

場合によっては、アプリケーションが <xref:System.Windows.Forms.DataGridView> コントロール内のユーザー選択に基づいてアクションを実行する必要があります。 アクションによっては、可能な選択の種類を制限することができます。 たとえば、現在選択されているレコードのレポートをアプリケーションで印刷できるとします。 この場合は、行内の任意の場所をクリックすると常に行全体が選択されるように <xref:System.Windows.Forms.DataGridView> コントロールを構成し、一度に1行だけを選択できるようにすることができます。

<xref:System.Windows.Forms.DataGridView.SelectionMode%2A?displayProperty=nameWithType> プロパティを次の <xref:System.Windows.Forms.DataGridViewSelectionMode> 列挙値のいずれかに設定することによって、許可される選択を指定できます。

|DataGridViewSelectionMode 値|[説明]|
|-------------------------------------|-----------------|
|<xref:System.Windows.Forms.DataGridViewSelectionMode.CellSelect>|セルをクリックすると、そのセルが選択されます。 行と列のヘッダーを選択に使用することはできません。|
|<xref:System.Windows.Forms.DataGridViewSelectionMode.ColumnHeaderSelect>|セルをクリックすると、そのセルが選択されます。 列ヘッダーをクリックすると、列全体が選択されます。 列ヘッダーを並べ替えに使用することはできません。|
|<xref:System.Windows.Forms.DataGridViewSelectionMode.FullColumnSelect>|セルまたは列ヘッダーをクリックすると、列全体が選択されます。 列ヘッダーを並べ替えに使用することはできません。|
|<xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect>|セルまたは行ヘッダーをクリックすると、行全体が選択されます。|
|<xref:System.Windows.Forms.DataGridViewSelectionMode.RowHeaderSelect>|既定の選択モード。 セルをクリックすると、そのセルが選択されます。 行ヘッダーをクリックすると、行全体が選択されます。|

> [!NOTE]
> 実行時に選択モードを変更すると、現在の選択範囲が自動的にクリアされます。

既定では、複数の行、列、またはセルを選択してマウスでドラッグしたり、CTRL キーまたは SHIFT キーを押しながら選択範囲を拡張または変更したり、左上のヘッダーセルをクリックしてコントロール内のすべてのセルを選択したりすることができます。 この動作を回避するには、<xref:System.Windows.Forms.DataGridView.MultiSelect%2A> プロパティを `false`に設定します。

<xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect> モードと <xref:System.Windows.Forms.DataGridViewSelectionMode.RowHeaderSelect> モードでは、ユーザーが行を選択して DEL キーを押すことにより、行を削除できます。 ユーザーが行を削除できるのは、現在のセルが編集モードではない場合、<xref:System.Windows.Forms.DataGridView.AllowUserToDeleteRows%2A> プロパティが `true`に設定されていて、基になるデータソースがユーザードリブンの行の削除をサポートしている場合のみです。 これらの設定では、プログラムによる行の削除が妨げられないことに注意してください。

## <a name="programmatic-selection"></a>プログラムによる選択

現在の選択モードでは、ユーザーが選択するだけでなく、プログラムによる選択の動作が制限されます。 <xref:System.Windows.Forms.DataGridView> コントロールに存在するセル、行、または列の `Selected` プロパティを設定することにより、現在の選択内容をプログラムで変更できます。 選択モードに応じて、<xref:System.Windows.Forms.DataGridView.SelectAll%2A> メソッドを使用してコントロール内のすべてのセルを選択することもできます。 選択範囲をクリアするには、<xref:System.Windows.Forms.DataGridView.ClearSelection%2A> メソッドを使用します。

<xref:System.Windows.Forms.DataGridView.MultiSelect%2A> プロパティが `true`に設定されている場合は、要素の `Selected` プロパティを変更することによって、<xref:System.Windows.Forms.DataGridView> 要素を選択項目に追加または削除できます。 それ以外の場合、1つの要素に対して [`Selected`] プロパティを `true` に設定すると、その他の要素が選択範囲から自動的に削除されます。

<xref:System.Windows.Forms.DataGridView.CurrentCell%2A> プロパティの値を変更しても、現在の選択内容は変更されないことに注意してください。

現在選択されているセル、行、または列のコレクションを取得するには、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.SelectedCells%2A>、<xref:System.Windows.Forms.DataGridView.SelectedRows%2A>、および <xref:System.Windows.Forms.DataGridView.SelectedColumns%2A> の各プロパティを使用します。 コントロールのすべてのセルが選択されている場合、これらのプロパティへのアクセスは非効率的です。 この場合、パフォーマンスが低下しないようにするには、最初に <xref:System.Windows.Forms.DataGridView.AreAllCellsSelected%2A> メソッドを使用します。 また、選択したセル、行、または列の数を決定するために、これらのコレクションにアクセスするのは効率的ではありません。 代わりに、<xref:System.Windows.Forms.DataGridView.GetCellCount%2A>、<xref:System.Windows.Forms.DataGridViewRowCollection.GetRowCount%2A>、または <xref:System.Windows.Forms.DataGridViewColumnCollection.GetColumnCount%2A> メソッドを使用して、<xref:System.Windows.Forms.DataGridViewElementStates.Selected> の値を渡す必要があります。

> [!TIP]
> 選択したセルのプログラムによる使用方法を示すコード例については、「<xref:System.Windows.Forms.DataGridView> クラスの概要」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.MultiSelect%2A>
- <xref:System.Windows.Forms.DataGridView.SelectionMode%2A>
- <xref:System.Windows.Forms.DataGridViewSelectionMode>
- [Windows フォーム DataGridView コントロールでの選択およびクリップボードの使用](selection-and-clipboard-use-with-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの選択モードを設定する](how-to-set-the-selection-mode-of-the-windows-forms-datagridview-control.md)
