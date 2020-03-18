---
title: DataGridView コントロールの列の型
ms.date: 03/30/2017
helpviewer_keywords:
- columns [Windows Forms], types
- DataGridView control [Windows Forms], column types
- data grids [Windows Forms], columns
ms.assetid: f0a0a9f1-8757-4bfd-891f-d7d12870dbed
ms.openlocfilehash: e918394cca6350854074d4c1567890138b2a1462
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743853"
---
# <a name="column-types-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールの列型
<xref:System.Windows.Forms.DataGridView> コントロールでは、いくつかの列の型を使用して情報を表示し、ユーザーが情報を変更または追加できるようにします。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールをバインドし、<xref:System.Windows.Forms.DataGridView.AutoGenerateColumns%2A> プロパティを `true`に設定すると、バインドされたデータソースに含まれるデータ型に適した既定の列型を使用して列が自動的に生成されます。  
  
 また、任意の列クラスのインスタンスを自分で作成し、<xref:System.Windows.Forms.DataGridView.Columns%2A> プロパティによって返されるコレクションに追加することもできます。 これらのインスタンスは、バインドされていない列として使用するように作成することも、手動でバインドすることもできます。 手動でバインドされた列は、ある型の自動生成された列を別の型の列に置き換える場合などに便利です。  
  
 次の表では、<xref:System.Windows.Forms.DataGridView> コントロールで使用できるさまざまな列クラスについて説明します。  
  
|クラス|[説明]|  
|-----------|-----------------|  
|<xref:System.Windows.Forms.DataGridViewTextBoxColumn>|テキストベースの値と共に使用されます。 数値および文字列にバインドするときに自動的に生成されます。|  
|<xref:System.Windows.Forms.DataGridViewCheckBoxColumn>|<xref:System.Boolean> および <xref:System.Windows.Forms.CheckState> 値と共に使用されます。 これらの型の値にバインドするときに自動的に生成されます。|  
|<xref:System.Windows.Forms.DataGridViewImageColumn>|画像を表示するために使用します。 バイト配列、<xref:System.Drawing.Image> オブジェクト、または <xref:System.Drawing.Icon> オブジェクトにバインドするときに自動的に生成されます。|  
|<xref:System.Windows.Forms.DataGridViewButtonColumn>|セルにボタンを表示するために使用します。 バインド時に自動的に生成されません。 通常、非バインド列として使用されます。|  
|<xref:System.Windows.Forms.DataGridViewComboBoxColumn>|セル内のドロップダウンリストを表示するために使用します。 バインド時に自動的に生成されません。 通常、データバインドは手動で行います。|  
|<xref:System.Windows.Forms.DataGridViewLinkColumn>|セル内のリンクを表示するために使用します。 バインド時に自動的に生成されません。 通常、データバインドは手動で行います。|  
|カスタム列の型|<xref:System.Windows.Forms.DataGridViewColumn> クラスまたはその派生クラスを継承することによって独自の列クラスを作成し、カスタムの外観、動作、またはホストされたコントロールを提供できます。 詳細については、「[方法: 動作と外観を拡張して Windows フォーム DataGridView コントロールのセルと列をカスタマイズする](customize-cells-and-columns-in-the-datagrid-by-extending-behavior.md)」を参照してください。|  
  
 これらの列の型については、以降のセクションで詳しく説明します。  
  
## <a name="datagridviewtextboxcolumn"></a>DataGridViewTextBoxColumn  
 <xref:System.Windows.Forms.DataGridViewTextBoxColumn> は、数値や文字列などのテキストベースの値で使用する汎用的な列型です。 編集モードでは、アクティブセルに <xref:System.Windows.Forms.TextBox> コントロールが表示され、ユーザーはセルの値を変更できるようになります。  
  
 セル値は、自動的に文字列に変換されて表示されます。 ユーザーが入力または変更した値は、適切なデータ型のセル値を作成するために自動的に解析されます。 これらの変換は、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントと <xref:System.Windows.Forms.DataGridView.CellParsing> イベントを処理することによってカスタマイズできます。  
  
 列のセル値のデータ型は、列の <xref:System.Windows.Forms.DataGridViewColumn.ValueType%2A> プロパティで指定されます。  
  
## <a name="datagridviewcheckboxcolumn"></a>DataGridViewCheckBoxColumn  
 <xref:System.Windows.Forms.DataGridViewCheckBoxColumn> は、<xref:System.Boolean> と <xref:System.Windows.Forms.CheckState> 値と共に使用されます。 <xref:System.Boolean> の値は、<xref:System.Windows.Forms.DataGridViewCheckBoxColumn.ThreeState%2A> プロパティの値に応じて、2つの状態または3つの状態のチェックボックスとして表示されます。 列が <xref:System.Windows.Forms.CheckState> 値にバインドされている場合は、<xref:System.Windows.Forms.DataGridViewCheckBoxColumn.ThreeState%2A> プロパティ値が既定で `true` されます。  
  
 通常、チェックボックスのセル値は、他のデータと同様にストレージ用、または一括操作を実行する目的で使用されます。 ユーザーがチェックボックスのセルをクリックしたときにすぐに応答する場合は、<xref:System.Windows.Forms.DataGridView.CellClick> イベントを処理できますが、このイベントはセルの値が更新される前に発生します。 クリック時に新しい値が必要な場合は、現在の値に基づいて期待される値を計算する方法があります。 もう1つの方法は、変更をすぐにコミットし、それに応答するために <xref:System.Windows.Forms.DataGridView.CellValueChanged> イベントを処理することです。 セルがクリックされたときに変更をコミットするには、<xref:System.Windows.Forms.DataGridView.CurrentCellDirtyStateChanged> イベントを処理する必要があります。 ハンドラーで、現在のセルがチェックボックスセルの場合は、<xref:System.Windows.Forms.DataGridView.CommitEdit%2A> メソッドを呼び出し、<xref:System.Windows.Forms.DataGridViewDataErrorContexts.Commit> の値を渡します。  
  
## <a name="datagridviewimagecolumn"></a>DataGridViewImageColumn  
 <xref:System.Windows.Forms.DataGridViewImageColumn> は、イメージの表示に使用されます。 画像列は、データソースから自動的に入力したり、バインドされていない列に対して手動で設定したり、<xref:System.Windows.Forms.DataGridView.CellFormatting> イベントのハンドラーに動的に設定したりすることができます。  
  
 データソースからのイメージ列の自動作成では、さまざまなイメージ形式のバイト配列を使用します。これには、<xref:System.Drawing.Image> クラスでサポートされるすべての形式と、Microsoft® Access および Northwind サンプルデータベースで使用される OLE 画像形式が含まれます。  
  
 画像列を手動で作成することは、<xref:System.Windows.Forms.DataGridViewButtonColumn>の機能をカスタマイズした外観で提供する場合に便利です。 <xref:System.Windows.Forms.DataGridView.CellClick?displayProperty=nameWithType> イベントを処理して、画像セル内のクリックに応答することができます。  
  
 <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントのハンドラーでイメージ列のセルを設定すると、計算値または非イメージ形式の値のイメージを提供する場合に便利です。 たとえば、アイコンとして表示する `"high"`、`"middle"`、`"low"` などの文字列値を含む "リスク" 列があるとします。 また、イメージのバイナリコンテンツではなく、読み込む必要のあるイメージの場所を含む "イメージ" 列がある場合もあります。  
  
## <a name="datagridviewbuttoncolumn"></a>DataGridViewButtonColumn  
 <xref:System.Windows.Forms.DataGridViewButtonColumn>を使用すると、ボタンを含むセルの列を表示できます。 これは、ユーザーが特定のレコードに対して簡単にアクションを実行できるようにする場合に便利です。たとえば、注文を配置したり、子レコードを別のウィンドウに表示したりすることができます。  
  
 ボタン列は、データバインディングが <xref:System.Windows.Forms.DataGridView> コントロールに対して自動的に生成されることはありません。 ボタン列を使用するには、それらを手動で作成し、<xref:System.Windows.Forms.DataGridView.Columns%2A?displayProperty=nameWithType> プロパティによって返されるコレクションに追加する必要があります。  
  
 ボタンセルのユーザークリックに応答するには、<xref:System.Windows.Forms.DataGridView.CellClick?displayProperty=nameWithType> イベントを処理します。  
  
## <a name="datagridviewcomboboxcolumn"></a>System.windows.forms.datagridviewcomboboxcolumn>  
 <xref:System.Windows.Forms.DataGridViewComboBoxColumn>を使用すると、ドロップダウンリストボックスを含むセルの列を表示できます。 これは、Northwind サンプルデータベース内の Products テーブルの Category 列など、特定の値のみを含むことができるフィールドのデータ入力に便利です。  
  
 すべてのセルに使用されるドロップダウンリストは、<xref:System.Windows.Forms.DataGridViewComboBoxColumn.Items%2A> プロパティによって返されるコレクションを使用して手動で、または <xref:System.Windows.Forms.DataGridViewComboBoxColumn.DataSource%2A>、<xref:System.Windows.Forms.DataGridViewComboBoxColumn.DisplayMember%2A>、および <xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A> プロパティを使用してデータソースにバインドすることによって、<xref:System.Windows.Forms.ComboBox> ドロップダウンリストに入力するのと同じ方法で設定できます。 詳細については、「 [ComboBox コントロール](combobox-control-windows-forms.md)」を参照してください。  
  
 <xref:System.Windows.Forms.DataGridViewComboBoxColumn?displayProperty=nameWithType>の <xref:System.Windows.Forms.DataGridViewColumn.DataPropertyName%2A> プロパティを設定することによって、<xref:System.Windows.Forms.DataGridView> コントロールによって使用されるデータソースに実際のセル値をバインドできます。  
  
 コンボボックスの列は、データバインディングが <xref:System.Windows.Forms.DataGridView> コントロールによって自動的に生成されることはありません。 コンボボックス列を使用するには、それらを手動で作成し、<xref:System.Windows.Forms.DataGridView.Columns%2A> プロパティによって返されるコレクションに追加する必要があります。  
  
## <a name="datagridviewlinkcolumn"></a>DataGridViewLinkColumn  
 <xref:System.Windows.Forms.DataGridViewLinkColumn>を使用すると、ハイパーリンクを含むセルの列を表示できます。 これは、データソース内の URL 値や、子レコードを含むウィンドウを開くなどの特殊な動作のボタン列の代わりに使用できます。  
  
 リンク列は、データバインディングが <xref:System.Windows.Forms.DataGridView> コントロールに対して自動的に生成されることはありません。 リンク列を使用するには、それらを手動で作成し、<xref:System.Windows.Forms.DataGridView.Columns%2A> プロパティによって返されるコレクションに追加する必要があります。  
  
 <xref:System.Windows.Forms.DataGridView.CellContentClick> イベントを処理することによって、リンク上のユーザークリックに応答できます。 このイベントは、ユーザーがセル内の任意の場所をクリックしたときに発生する、<xref:System.Windows.Forms.DataGridView.CellClick> イベントと <xref:System.Windows.Forms.DataGridView.CellMouseClick> イベントとは異なります。  
  
 <xref:System.Windows.Forms.DataGridViewLinkColumn> クラスには、リンクの表示を変更するためのいくつかのプロパティが用意されています。これらのプロパティは、クリックされる前、中、およびクリックした後に  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn>
- <xref:System.Windows.Forms.DataGridViewButtonColumn>
- <xref:System.Windows.Forms.DataGridViewCheckBoxColumn>
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn>
- <xref:System.Windows.Forms.DataGridViewImageColumn>
- <xref:System.Windows.Forms.DataGridViewTextBoxColumn>
- <xref:System.Windows.Forms.DataGridViewLinkColumn>
- [DataGridView コントロール](datagridview-control-windows-forms.md)
- [方法: Windows フォーム DataGridView コントロールのセルにイメージを表示する](how-to-display-images-in-cells-of-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのイメージ列を操作する](how-to-work-with-image-columns-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)
