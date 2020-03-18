---
title: DataGridView コントロールのセルスタイル
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], cell styles
- cells [Windows Forms], styles
- data grids [Windows Forms], cell styles
ms.assetid: dbb75ed6-8804-4232-8382-f9920c2e380c
ms.openlocfilehash: fe56033a5adbe7719c64695c8f9ebc4f3644fc65
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746157"
---
# <a name="cell-styles-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでのセルのスタイル
<xref:System.Windows.Forms.DataGridView> コントロール内の各セルは、テキスト形式、背景色、前景色、フォントなど、独自のスタイルを持つことができます。 ただし、通常は、複数のセルで特定のスタイル特性を共有します。  
  
 スタイルを共有するセルのグループには、特定の行または列内のすべてのセル、特定の値を含むすべてのセル、またはコントロール内のすべてのセルを含めることができます。 これらのグループが重なっているため、各セルのスタイル情報が複数の場所から取得される場合があります。 たとえば、<xref:System.Windows.Forms.DataGridView> コントロール内のすべてのセルが同じフォントを使用するようにする必要がありますが、通貨の列のセルだけが通貨形式を使用するようにし、負の数値を持つ通貨セルのみが赤の前景色を使用するようにすることができます。  
  
## <a name="the-datagridviewcellstyle-class"></a>DataGridViewCellStyle クラス  
 <xref:System.Windows.Forms.DataGridViewCellStyle> クラスには、visual スタイルに関連する次のプロパティが含まれています。  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.BackColor%2A> および <xref:System.Windows.Forms.DataGridViewCellStyle.ForeColor%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.SelectionBackColor%2A> および <xref:System.Windows.Forms.DataGridViewCellStyle.SelectionForeColor%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.Font%2A>  
  
 このクラスには、書式設定に関連する次のプロパティも含まれています。  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A> および <xref:System.Windows.Forms.DataGridViewCellStyle.FormatProvider%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.NullValue%2A> および <xref:System.Windows.Forms.DataGridViewCellStyle.DataSourceNullValue%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.Alignment%2A>  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle.Padding%2A>  
  
 これらのプロパティおよびその他のセルスタイルのプロパティの詳細については、以下の「関連項目」セクションに記載されている <xref:System.Windows.Forms.DataGridViewCellStyle> リファレンスドキュメントとトピックを参照してください。  
  
## <a name="using-datagridviewcellstyle-objects"></a>DataGridViewCellStyle オブジェクトの使用  
 <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトは、<xref:System.Windows.Forms.DataGridView>、<xref:System.Windows.Forms.DataGridViewColumn>、<xref:System.Windows.Forms.DataGridViewRow>、および <xref:System.Windows.Forms.DataGridViewCell> クラスとその派生クラスのさまざまなプロパティから取得できます。 これらのプロパティのいずれかがまだ設定されていない場合は、その値を取得すると、新しい <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトが作成されます。 また、独自の <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトをインスタンス化して、これらのプロパティに割り当てることもできます。  
  
 複数の <xref:System.Windows.Forms.DataGridView> 要素間で <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを共有することによって、スタイル情報の不要な重複を回避できます。 コントロール、列、および行の各レベルで設定されたスタイルによって、各レベルがセルレベルまでフィルター処理されるため、上記のレベルとは異なる各レベルのスタイルプロパティのみを設定することにより、スタイルの重複を回避することもできます。 詳細については、後の「スタイルの継承」セクションを参照してください。  
  
 次の表に、<xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを取得または設定する主なプロパティを示します。  
  
|プロパティ|クラス|[説明]|  
|--------------|-------------|-----------------|  
|`DefaultCellStyle`|<xref:System.Windows.Forms.DataGridView>、<xref:System.Windows.Forms.DataGridViewColumn>、<xref:System.Windows.Forms.DataGridViewRow>、および派生クラス|コントロール全体 (ヘッダーセルを含む)、列、または行内のすべてのセルで使用される既定のスタイルを取得または設定します。|  
|<xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A>|<xref:System.Windows.Forms.DataGridView>|コントロールのすべての行で使用される既定のセルスタイルを取得または設定します。 これには、ヘッダーセルは含まれません。|  
|<xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A>|<xref:System.Windows.Forms.DataGridView>|コントロールの交互の行で使用される既定のセルスタイルを取得または設定します。 元帳に似た効果を作成するために使用します。|  
|<xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A>|<xref:System.Windows.Forms.DataGridView>|コントロールの行ヘッダーによって使用される既定のセルスタイルを取得または設定します。 Visual スタイルが有効になっている場合、現在のテーマによってオーバーライドされます。|  
|<xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A>|<xref:System.Windows.Forms.DataGridView>|コントロールの列ヘッダーによって使用される既定のセルスタイルを取得または設定します。 Visual スタイルが有効になっている場合、現在のテーマによってオーバーライドされます。|  
|<xref:System.Windows.Forms.DataGridViewCell.Style%2A>|<xref:System.Windows.Forms.DataGridViewCell> と派生クラス|セルレベルで指定されたスタイルを取得または設定します。 これらのスタイルは、上位レベルから継承されたスタイルよりも優先されます。|  
|`InheritedStyle`|<xref:System.Windows.Forms.DataGridViewCell>、<xref:System.Windows.Forms.DataGridViewRow>、<xref:System.Windows.Forms.DataGridViewColumn>、および派生クラス|上位レベルから継承されたスタイルを含む、セル、行、または列に現在適用されているすべてのスタイルを取得します。|  
  
 前述のように、プロパティが以前に設定されていない場合、スタイルプロパティの値を取得すると、新しい <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトが自動的にインスタンス化されます。 これらのオブジェクトを不必要に作成しないようにするために、行クラスと列クラスには <xref:System.Windows.Forms.DataGridViewBand.HasDefaultCellStyle%2A> のプロパティがあり、<xref:System.Windows.Forms.DataGridViewBand.DefaultCellStyle%2A> プロパティが設定されているかどうかを確認できます。 同様に、セルクラスには、<xref:System.Windows.Forms.DataGridViewCell.Style%2A> プロパティが設定されているかどうかを示す <xref:System.Windows.Forms.DataGridViewCell.HasStyle%2A> プロパティがあります。  
  
 各スタイルプロパティには、<xref:System.Windows.Forms.DataGridView> コントロールに対応する*PropertyName*`Changed` イベントがあります。 行、列、およびセルの各プロパティの場合、イベントの名前は "`Row`"、"`Column`"、または "`Cell`" で始まります (たとえば、<xref:System.Windows.Forms.DataGridView.RowDefaultCellStyleChanged>)。 これらの各イベントは、対応するスタイルプロパティが別の <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトに設定されている場合に発生します。 これらのイベントは、スタイルプロパティから <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを取得し、そのプロパティ値を変更した場合には発生しません。 セルスタイルオブジェクト自体への変更に応答するには、<xref:System.Windows.Forms.DataGridView.CellStyleContentChanged> イベントを処理します。  
  
## <a name="style-inheritance"></a>スタイルの継承  
 各 <xref:System.Windows.Forms.DataGridViewCell> は、<xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A> プロパティから外観を取得します。 このプロパティによって返される <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトは、<xref:System.Windows.Forms.DataGridViewCellStyle>型のプロパティの階層からその値を継承します。 これらのプロパティは、ヘッダー以外のセルの <xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A> が値を取得する順序で下に一覧表示されます。  
  
1. <xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType>  
  
2. <xref:System.Windows.Forms.DataGridViewRow.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
3. <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType> (インデックス番号が奇数の行のセルのみ)  
  
4. <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>  
  
5. <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
6. <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
 行と列のヘッダーセルの場合、<xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A> プロパティは、指定された順序で、次のソースプロパティのリストの値によって設定されます。  
  
1. <xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType>  
  
2. <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A?displayProperty=nameWithType> または <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A?displayProperty=nameWithType>  
  
3. <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
 このプロセスを説明する図を次に示します。  
  
 ![DataGridViewCellStyle 型のプロパティ](./media/cell-styles-in-the-windows-forms-datagridview-control/datagridviewcells-inheritance-diagram.gif "DataGridViewCells 継承ダイアグラム")  
  
 また、特定の行と列によって継承されたスタイルにアクセスすることもできます。 列 <xref:System.Windows.Forms.DataGridViewColumn.InheritedStyle%2A> プロパティは、次のプロパティからその値を継承します。  
  
1. <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
2. <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
 Row <xref:System.Windows.Forms.DataGridViewRow.InheritedStyle%2A> プロパティは、次のプロパティからその値を継承します。  
  
1. <xref:System.Windows.Forms.DataGridViewRow.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
2. <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType> (インデックス番号が奇数の行のセルのみ)  
  
3. <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>  
  
4. <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>  
  
 `InheritedStyle` プロパティによって返される <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトの各プロパティについて、プロパティ値は、対応するプロパティが <xref:System.Windows.Forms.DataGridViewCellStyle> クラスの既定値以外の値に設定されている、適切なリストの最初のセルスタイルから取得されます。  
  
 次の表は、例のセルの <xref:System.Windows.Forms.DataGridViewCellStyle.ForeColor%2A> プロパティ値が、その親列から継承される方法を示しています。  
  
|`DataGridViewCellStyle` 型のプロパティ|取得したオブジェクトの `ForeColor` 値の例|  
|----------------------------------------------|----------------------------------------------------|  
|<xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.Empty?displayProperty=nameWithType>|  
|<xref:System.Windows.Forms.DataGridViewRow.DefaultCellStyle%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.Red%2A?displayProperty=nameWithType>|  
|<xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.Empty?displayProperty=nameWithType>|  
|<xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.Empty?displayProperty=nameWithType>|  
|<xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.DarkBlue%2A?displayProperty=nameWithType>|  
|<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>|<xref:System.Drawing.Color.Black%2A?displayProperty=nameWithType>|  
  
 この場合、セルの行の <xref:System.Drawing.Color.Red%2A?displayProperty=nameWithType> 値は、リストの最初の実際の値です。 これは、セルの <xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A>の <xref:System.Windows.Forms.DataGridViewCellStyle.ForeColor%2A> プロパティ値になります。  
  
 次の図は、さまざまな <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティがさまざまな場所から値を継承する方法を示しています。  
  
 ![DataGridView プロパティ&#45;値の継承](./media/cell-styles-in-the-windows-forms-datagridview-control/datagridviewcells-value-inheritance-diagram.gif "DataGridViewCells 値の継承ダイアグラム")  
  
 スタイルの継承を利用することで、複数の場所で同じ情報を指定しなくても、コントロール全体に適したスタイルを提供できます。  
  
 説明のとおり、ヘッダーセルはスタイル継承に参加しますが、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A> および <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A> プロパティによって返されるオブジェクトには、<xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A> プロパティによって返されるオブジェクトのプロパティ値をオーバーライドする初期プロパティ値があります。 <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A> プロパティによって返されるオブジェクトに対して設定されたプロパティを行ヘッダーと列ヘッダーに適用する場合は、<xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A> および <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A> プロパティによって返されるオブジェクトの対応するプロパティを、<xref:System.Windows.Forms.DataGridViewCellStyle> クラスに指定されている既定値に設定する必要があります。  
  
> [!NOTE]
> 視覚スタイルが有効になっている場合、行ヘッダーと列ヘッダー (<xref:System.Windows.Forms.DataGridView.TopLeftHeaderCell%2A>を除く) は、現在のテーマによって自動的にスタイルが設定され、これらのプロパティによって指定されたスタイルはオーバーライドされます。  
  
 <xref:System.Windows.Forms.DataGridViewButtonColumn>、<xref:System.Windows.Forms.DataGridViewImageColumn>、および <xref:System.Windows.Forms.DataGridViewCheckBoxColumn> 型は、列 <xref:System.Windows.Forms.DataGridViewColumn.DefaultCellStyle%2A> プロパティによって返されるオブジェクトの一部の値も初期化します。 詳細については、これらの型のリファレンスドキュメントを参照してください。  
  
## <a name="setting-styles-dynamically"></a>動的なスタイル設定  
 特定の値を持つセルのスタイルをカスタマイズするには、<xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType> イベントのハンドラーを実装します。 このイベントのハンドラーは、<xref:System.Windows.Forms.DataGridViewCellFormattingEventArgs> 型の引数を受け取ります。 このオブジェクトには、書式設定されているセルの値と、<xref:System.Windows.Forms.DataGridView> コントロール内のその位置を確認できるプロパティが含まれています。 このオブジェクトには、書式設定されるセルの <xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A> プロパティの値に初期化される <xref:System.Windows.Forms.DataGridViewCellFormattingEventArgs.CellStyle%2A> プロパティも含まれています。 セルスタイルのプロパティを変更して、セルの値と場所に適したスタイル情報を指定できます。  
  
> [!NOTE]
> <xref:System.Windows.Forms.DataGridView.RowPrePaint> イベントと <xref:System.Windows.Forms.DataGridView.RowPostPaint> イベントは、イベントデータ内の <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトも受け取りますが、その場合は読み取り専用の行 <xref:System.Windows.Forms.DataGridViewRow.InheritedStyle%2A> プロパティのコピーであるため、変更内容はコントロールに影響しません。  
  
 また、<xref:System.Windows.Forms.DataGridView.CellMouseEnter?displayProperty=nameWithType> イベントや <xref:System.Windows.Forms.DataGridView.CellMouseLeave> イベントなどのイベントに応じて、個々のセルのスタイルを動的に変更することもできます。 たとえば、<xref:System.Windows.Forms.DataGridView.CellMouseEnter> イベントのハンドラーでは、セルの背景色の現在の値 (セルの <xref:System.Windows.Forms.DataGridViewCell.Style%2A> プロパティを通じて取得されます) を格納した後、マウスを置いたときにセルを強調表示する新しい色に設定できます。 <xref:System.Windows.Forms.DataGridView.CellMouseLeave> イベントのハンドラーでは、背景色を元の値に戻すことができます。  
  
> [!NOTE]
> 特定のスタイル値が設定されているかどうかに関係なく、セルの <xref:System.Windows.Forms.DataGridViewCell.Style%2A> プロパティに格納されている値をキャッシュすることが重要です。 スタイル設定を一時的に置き換える場合は、元の "設定なし" の状態に復元することで、そのセルが上位レベルからスタイル設定を継承するようになります。 スタイルが継承されているかどうかに関係なく、セルに対して実際に有効なスタイルを決定する必要がある場合は、セルの <xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A> プロパティを使用します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewBand.InheritedStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewRow.InheritedStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.InheritedStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewBand.DefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCell.InheritedStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCell.Style%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.CellStyleContentChanged?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowPrePaint?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowPostPaint?displayProperty=nameWithType>
- [Windows フォームの DataGridView コントロールの基本的な書式設定およびスタイル設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの既定のセル スタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータの書式設定](data-formatting-in-the-windows-forms-datagridview-control.md)
