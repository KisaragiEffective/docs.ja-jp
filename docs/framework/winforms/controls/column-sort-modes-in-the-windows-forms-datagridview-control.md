---
title: DataGridView コントロールの列の並べ替えモード
ms.date: 03/30/2017
helpviewer_keywords:
- data grids [Windows Forms], sort modes
- DataGridView control [Windows Forms], sort mode
ms.assetid: 43715887-2df9-4da7-bcf1-b9c7c842b2bf
ms.openlocfilehash: d1e2f582c9759332e0ed963cb7f88ee052616c45
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744194"
---
# <a name="column-sort-modes-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロール内の列の並べ替えモード
<xref:System.Windows.Forms.DataGridView> 列には3つの並べ替えモードがあります。 各列の並べ替えモードは、列の <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティによって指定されます。これは、次のいずれかの <xref:System.Windows.Forms.DataGridViewColumnSortMode> 列挙値に設定できます。  
  
|`DataGridViewColumnSortMode` 値|[説明]|  
|----------------------------------------|-----------------|  
|<xref:System.Windows.Forms.DataGridViewColumnSortMode.Automatic>|テキストボックスの列の既定値。 列ヘッダーを選択に使用しない限り、列ヘッダーをクリックすると、この列によって <xref:System.Windows.Forms.DataGridView> が自動的に並べ替えられ、並べ替え順序を示すグリフが表示されます。|  
|<xref:System.Windows.Forms.DataGridViewColumnSortMode.NotSortable>|テキストボックス以外の列の既定値。 この列はプログラムで並べ替えることができます。ただし、並べ替えのためのものではないため、並べ替えグリフ用の領域は予約されていません。|  
|<xref:System.Windows.Forms.DataGridViewColumnSortMode.Programmatic>|この列はプログラムで並べ替えることができ、領域は並べ替えグリフ用に予約されています。|  
  
 意味のある順序付けが可能な値が含まれている場合は、既定値が <xref:System.Windows.Forms.DataGridViewColumnSortMode.NotSortable> 列の並べ替えモードを変更することができます。 たとえば、アイテムの状態を表す数値を含むデータベース列がある場合は、データベース列に画像列をバインドすることで、これらの数値を対応するアイコンとして表示できます。 その後、<xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType> イベントのハンドラーで、数値セルの値を画像表示値に変更できます。 この場合、<xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティを <xref:System.Windows.Forms.DataGridViewColumnSortMode.Automatic> に設定すると、ユーザーが列を並べ替えることができます。 自動並べ替えを使用すると、ユーザーは、数値に対応する状態に自然な順序が設定されていない場合でも、同じ状態の項目をグループ化できます。 チェックボックスの列は、同じ状態の項目をグループ化するために自動並べ替えが役立つもう1つの例です。  
  
 <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> の設定に関係なく、任意の列の値または複数の列の値によって、<xref:System.Windows.Forms.DataGridView> プログラムによって並べ替えを行うことができます。 プログラムによる並べ替えは、並べ替えに独自のユーザーインターフェイス (UI) を提供したり、カスタムの並べ替えを実装したりする場合に便利です。 列ヘッダーの選択を有効にするように <xref:System.Windows.Forms.DataGridView> 選択モードを設定する場合などに、独自の並べ替え UI を用意すると便利です。 この場合は、列ヘッダーを並べ替えに使用することはできませんが、ヘッダーに適切な並べ替えグリフが表示されるようにするには、<xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティを <xref:System.Windows.Forms.DataGridViewColumnSortMode.Programmatic>に設定します。  
  
 プログラムによる並べ替えモードに設定されている列は、並べ替えグリフを自動的に表示しません。 これらの列に対しては、<xref:System.Windows.Forms.DataGridViewColumnHeaderCell.SortGlyphDirection%2A?displayProperty=nameWithType> プロパティを設定して、自分でグリフを表示する必要があります。 これは、カスタムの並べ替えの柔軟性を必要とする場合に必要です。 たとえば、複数の列で <xref:System.Windows.Forms.DataGridView> を並べ替える場合は、複数の並べ替えグリフを表示するか、並べ替えグリフを使用しないようにすることができます。  
  
 プログラムによって任意の列で <xref:System.Windows.Forms.DataGridView> を並べ替えることができますが、ボタン列などの一部の列には、意味のある順序を指定できる値が含まれていない場合があります。 これらの列の場合、<xref:System.Windows.Forms.DataGridViewColumnSortMode.NotSortable> の <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティ設定は、並べ替えに使用されないことを示します。そのため、並べ替えグリフのヘッダーに領域を予約する必要はありません。  
  
 <xref:System.Windows.Forms.DataGridView> が並べ替えられている場合は、<xref:System.Windows.Forms.DataGridView.SortedColumn%2A> の値と <xref:System.Windows.Forms.DataGridView.SortOrder%2A> プロパティを確認することで、並べ替え列と並べ替え順序の両方を確認できます。 これらの値は、カスタムの並べ替え操作の後に意味がありません。 カスタム並べ替えの詳細については、このトピックで後述する「カスタム並べ替え」セクションを参照してください。  
  
 バインド列とバインドされていない列の両方を含む <xref:System.Windows.Forms.DataGridView> コントロールが並べ替えられている場合、バインドされていない列の値は自動的に保持されません。 これらの値を維持するには、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティを `true` に設定し、<xref:System.Windows.Forms.DataGridView.CellValueNeeded> および <xref:System.Windows.Forms.DataGridView.CellValuePushed> イベントを処理することによって、仮想モードを実装する必要があります。 詳細については、「[方法: Windows フォーム DataGridView コントロールで仮想モードを実装](how-to-implement-virtual-mode-in-the-windows-forms-datagridview-control.md)する」を参照してください。 バインドモードでの非バインド列による並べ替えはサポートされていません。  
  
## <a name="programmatic-sorting"></a>プログラムによる並べ替え  
 <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドを呼び出すことによって、<xref:System.Windows.Forms.DataGridView> をプログラムによって並べ替えることができます。  
  
 <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドの `Sort(DataGridViewColumn,ListSortDirection)` オーバーロードは、<xref:System.Windows.Forms.DataGridViewColumn> と <xref:System.ComponentModel.ListSortDirection> 列挙値をパラメーターとして受け取ります。 このオーバーロードは、意味のある順序を指定できるが、自動並べ替えを構成しない値を持つ列で並べ替える場合に便利です。 このオーバーロードを呼び出し、<xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティ値 <xref:System.Windows.Forms.DataGridViewColumnSortMode.Automatic?displayProperty=nameWithType>を持つ列を渡すと、<xref:System.Windows.Forms.DataGridView.SortedColumn%2A> プロパティと <xref:System.Windows.Forms.DataGridView.SortOrder%2A> プロパティが自動的に設定され、適切な並べ替えグリフが列ヘッダーに表示されます。  
  
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティを設定して、<xref:System.Windows.Forms.DataGridView> コントロールが外部データソースにバインドされている場合、バインドされていない列に対しては `Sort(DataGridViewColumn,ListSortDirection)` メソッドオーバーロードは機能しません。 また、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティが `true`場合は、バインドされた列に対してのみこのオーバーロードを呼び出すことができます。 列がデータバインドされているかどうかを判断するには、<xref:System.Windows.Forms.DataGridViewColumn.IsDataBound%2A> プロパティの値を確認します。 バインドモードでの非バインド列の並べ替えはサポートされていません。  
  
## <a name="custom-sorting"></a>カスタム並べ替え  
 <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドの `Sort(IComparer)` オーバーロードを使用するか、<xref:System.Windows.Forms.DataGridView.SortCompare> イベントを処理することによって、<xref:System.Windows.Forms.DataGridView> をカスタマイズできます。  
  
 `Sort(IComparer)` メソッドオーバーロードは、パラメーターとして <xref:System.Collections.IComparer> インターフェイスを実装するクラスのインスタンスを受け取ります。 このオーバーロードは、カスタムの並べ替えを提供する場合に便利です。たとえば、列の値に自然な並べ替え順序が設定されていない場合や、自然な並べ替え順序が適切でない場合などです。 この場合、自動並べ替えを使用することはできませんが、列ヘッダーをクリックしてユーザーが並べ替えられるようにすることもできます。 列ヘッダーを選択に使用しない場合は、<xref:System.Windows.Forms.DataGridView.ColumnHeaderMouseClick> イベントのハンドラーでこのオーバーロードを呼び出すことができます。  
  
> [!NOTE]
> `Sort(IComparer)` メソッドのオーバーロードは、<xref:System.Windows.Forms.DataGridView> コントロールが外部データソースにバインドされておらず、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティ値が `false`場合にのみ機能します。 外部データソースにバインドされている列の並べ替えをカスタマイズするには、データソースによって提供される並べ替え操作を使用する必要があります。 仮想モードでは、バインドされていない列に対して独自の並べ替え操作を提供する必要があります。  
  
 `Sort(IComparer)` メソッドのオーバーロードを使用するには、<xref:System.Collections.IComparer> インターフェイスを実装する独自のクラスを作成する必要があります。 このインターフェイスでは、クラスが <xref:System.Collections.IComparer.Compare%2A?displayProperty=nameWithType> メソッドを実装する必要があります。このメソッドは、`Sort(IComparer)` メソッドのオーバーロードが呼び出されたときに、<xref:System.Windows.Forms.DataGridView> が <xref:System.Windows.Forms.DataGridViewRow> オブジェクトを入力として渡します。 これにより、任意の列の値に基づいて、正しい行の順序を計算できます。  
  
 `Sort(IComparer)` メソッドのオーバーロードでは <xref:System.Windows.Forms.DataGridView.SortedColumn%2A> と <xref:System.Windows.Forms.DataGridView.SortOrder%2A> のプロパティが設定されないため、並べ替えグリフを表示するには、常に <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.SortGlyphDirection%2A?displayProperty=nameWithType> プロパティを設定する必要があります。  
  
 `Sort(IComparer)` メソッドのオーバーロードの代わりに、<xref:System.Windows.Forms.DataGridView.SortCompare> イベントのハンドラーを実装することによって、カスタムの並べ替えを行うことができます。 このイベントは、自動並べ替え用に構成された列のヘッダーをユーザーがクリックしたとき、または <xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドの `Sort(DataGridViewColumn,ListSortDirection)` のオーバーロードを呼び出したときに発生します。 イベントは、コントロール内の行の各ペアに対して発生し、正しい順序を計算できるようにします。  
  
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティが設定されている場合、または <xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティ値が `true`場合、<xref:System.Windows.Forms.DataGridView.SortCompare> イベントは発生しません。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.Sort%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.SortedColumn%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.SortOrder%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.SortGlyphDirection%2A?displayProperty=nameWithType>
- [Windows フォームの DataGridView コントロールでのデータの並べ替え](sorting-data-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロール内の列の並べ替えモードを設定する](set-the-sort-modes-for-columns-wf-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの並べ替え機能をカスタマイズする](how-to-customize-sorting-in-the-windows-forms-datagridview-control.md)
