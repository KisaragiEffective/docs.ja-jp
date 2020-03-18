---
title: テーブルの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- flow content elements [WPF], Table
- documents [WPF], tables
- tables [WPF]
ms.assetid: 5e1105f4-8fc4-473a-ba55-88c8e71386e6
ms.openlocfilehash: fa7106207c69f19b647ba80ab7e724e9aad174c1
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005079"
---
# <a name="table-overview"></a>テーブルの概要
<xref:System.Windows.Documents.Table> は、フロードキュメントコンテンツのグリッドベースの表現をサポートするブロックレベルの要素です。 この要素は、その柔軟性により非常に便利ですが、正しく理解して使用するのが難しいとも言えます。  
  
 このトピックの内容は次のとおりです。  
  
- [テーブルの基本](#table_basics)  
  
- [テーブルとグリッドの相違点](#table_vs_Grid)  
  
- [テーブルの基本構造](#basic_table_structure)  
  
- [テーブルの内容](#table_containment)  
  
- [行グループ](#row_groupings)  
  
- [背景のレンダリングの優先順位](#rendering_precedence)  
  
- [複数の行または列にまたがるセル](#spanning_rows_or_columns)  
  
- [テーブルとコードのバインディング](#building_a_table_with_code)  
  
- 関連トピック 
  
<a name="table_basics"></a>   
## <a name="table-basics"></a>テーブルの基礎  
  
<a name="table_vs_Grid"></a>   
### <a name="how-is-table-different-then-grid"></a>テーブルとグリッドの相違点  
 <xref:System.Windows.Documents.Table> と <xref:System.Windows.Controls.Grid> はいくつかの一般的な機能を共有しますが、それぞれが異なるシナリオに最適です。 <xref:System.Windows.Documents.Table> は、フローコンテンツ内で使用するように設計されています (フローコンテンツの詳細については、「[フロードキュメントの概要](flow-document-overview.md)」を参照してください)。 グリッドは、フォーム内で最適に使用される (基本的に任意の場所以外のフロー コンテンツ)。 <xref:System.Windows.Documents.FlowDocument>内では、<xref:System.Windows.Documents.Table> は改ページ位置、列の折り返し、コンテンツの選択などのフローコンテンツの動作をサポートしていますが、<xref:System.Windows.Controls.Grid> ではサポートされていません。 一方、<xref:System.Windows.Controls.Grid> は <xref:System.Windows.Documents.FlowDocument> の外部で最もよく使用されます。たとえば、行インデックスと列インデックスに基づいて要素が追加される <xref:System.Windows.Controls.Grid> などですが、<xref:System.Windows.Documents.Table> はできません。 <xref:System.Windows.Controls.Grid> 要素を使用すると、1つの "セル" 内に複数の要素を持つことができるように、子コンテンツをレイヤー化できます。 <xref:System.Windows.Documents.Table> は、レイヤーをサポートしていません。 <xref:System.Windows.Controls.Grid> の子要素は、"セル" の境界の領域を基準として絶対に配置できます。 <xref:System.Windows.Documents.Table> では、この機能はサポートされていません。 最後に、<xref:System.Windows.Controls.Grid> に必要な <xref:System.Windows.Documents.Table> リソースが少なくなるため、パフォーマンスを向上させるために <xref:System.Windows.Controls.Grid> の使用を検討してください。  
  
<a name="basic_table_structure"></a>   
### <a name="basic-table-structure"></a>テーブルの基本構造  
 <xref:System.Windows.Documents.Table> には、列 (<xref:System.Windows.Documents.TableColumn> の要素で表される) と行 (<xref:System.Windows.Documents.TableRow> 要素で表される) で構成されるグリッドベースのプレゼンテーションが用意されています。 <xref:System.Windows.Documents.TableColumn> 要素はコンテンツをホストしません。列および列の特性を定義するだけです。 <xref:System.Windows.Documents.TableRow> の要素は、テーブルの行のグループ化を定義する <xref:System.Windows.Documents.TableRowGroup> 要素でホストされている必要があります。 テーブルによって表示される実際のコンテンツを含む <xref:System.Windows.Documents.TableCell> 要素は、<xref:System.Windows.Documents.TableRow> の要素でホストされる必要があります。 <xref:System.Windows.Documents.TableCell> には、<xref:System.Windows.Documents.Block>から派生した要素のみを含めることができます。  <xref:System.Windows.Documents.TableCell> の有効な子要素にはが含まれます。  
  
- <xref:System.Windows.Documents.BlockUIContainer>  
  
- <xref:System.Windows.Documents.List>  
  
- <xref:System.Windows.Documents.Paragraph>  
  
- <xref:System.Windows.Documents.Section>  
  
- <xref:System.Windows.Documents.Table>  
  
> [!NOTE]
> <xref:System.Windows.Documents.TableCell> 要素は、テキストコンテンツを直接ホストすることはできません。 <xref:System.Windows.Documents.TableCell>のようなフローコンテンツ要素の含有規則の詳細については、「[フロードキュメントの概要](flow-document-overview.md)」を参照してください。  
  
> [!NOTE]
> <xref:System.Windows.Documents.Table> は <xref:System.Windows.Controls.Grid> 要素に似ていますが、より多くの機能を備えているため、リソースのオーバーヘッドが大きくなります。  
  
 次の例では、XAML を使用して単純な 2 x 3 テーブルを定義します。  
  
 [!code-xaml[TableSnippets2#_Table_BasicLayout](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets2/CSharp/Window1.xaml#_table_basiclayout)]  
  
 この例の表示結果を次の図に示します。  
  
 ![基本的なテーブルのレンダリング方法を示すスクリーンショット。](./media/table-overview/basic-table-render-example.png)  
  
<a name="table_containment"></a>   
### <a name="table-containment"></a>テーブルの内容  
 <xref:System.Windows.Documents.Table> は <xref:System.Windows.Documents.Block> 要素から派生し、<xref:System.Windows.Documents.Block> レベルの要素の一般的な規則に従います。  <xref:System.Windows.Documents.Table> 要素は、次のいずれかの要素に含めることができます。  
  
- <xref:System.Windows.Documents.FlowDocument>  
  
- <xref:System.Windows.Documents.TableCell>  
  
- <xref:System.Windows.Controls.ListBoxItem>  
  
- <xref:System.Windows.Controls.ListViewItem>  
  
- <xref:System.Windows.Documents.Section>  
  
- <xref:System.Windows.Documents.Floater>  
  
- <xref:System.Windows.Documents.Figure>  
  
<a name="row_groupings"></a>   
### <a name="row-groupings"></a>行グループ  
 <xref:System.Windows.Documents.TableRowGroup> 要素は、テーブル内の任意の行を任意にグループ化する方法を提供します。テーブル内のすべての行は、行グループに属している必要があります。  多くの場合、行グループ内の行は共通の目的を共有しており、1 つのグループとしてスタイルを設定できます。  一般に、行のグループ化は、テーブルに格納された主要コンテンツから、特別な目的を持つ行 (タイトル行、ヘッダー行、フッター行など) を分離するために使用します。  
  
 次の例では、XAML を使用して、スタイルが設定されたヘッダー行とフッター行を含むテーブルを定義します。  
  
 [!code-xaml[TableSnippets2#_Table_RowGroups](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets2/CSharp/Window1.xaml#_table_rowgroups)]  
  
 この例の表示結果を次の図に示します。  
  
 ![スクリーンショット: テーブル行グループ](./media/table-rowgroups.png "Table_RowGroups")  
  
<a name="rendering_precedence"></a>   
### <a name="background-rendering-precedence"></a>背景のレンダリングの優先順位  
 テーブル要素は、次の順序でレンダリングされます (優先順位の低い項目から高い項目の z オーダー)。 この順序は変更できません。 たとえば、これらの要素には、確立された順序をオーバーライドするための "Z オーダー" プロパティは用意されていません。  
  
1. <xref:System.Windows.Documents.Table>  
  
2. <xref:System.Windows.Documents.TableColumn>  
  
3. <xref:System.Windows.Documents.TableRowGroup>  
  
4. <xref:System.Windows.Documents.TableRow>  
  
5. <xref:System.Windows.Documents.TableCell>  
  
 テーブル内のこれらの各要素に対して背景色を定義する例を次に示します。  
  
 [!code-xaml[TableSnippets2#_Table_ZOrder](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets2/CSharp/Window1.xaml#_table_zorder)]  
  
 次の図は、この例の表示結果 (背景色のみ表示) を示したものです。  
  
 ![スクリーンショット: テーブル z オーダー](./media/table-zorder.png "Table_ZOrder")  
  
<a name="spanning_rows_or_columns"></a>   
### <a name="spanning-rows-or-columns"></a>複数の行または列にまたがるセル  
 テーブルセルは、<xref:System.Windows.Documents.TableCell.RowSpan%2A> または <xref:System.Windows.Documents.TableCell.ColumnSpan%2A> 属性をそれぞれ使用して、複数の行または列にまたがるように構成できます。  
  
 3 つの列にまたがるセルの例を次に示します。  
  
 [!code-xaml[TableSnippets2#_Table_ColumnSpan](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets2/CSharp/Window1.xaml#_table_columnspan)]  
  
 この例の表示結果を次の図に示します。  
  
 ![スクリーンショット: 3 つの列すべてにまたがるセル](./media/table-columnspan.png "Table_ColumnSpan")  
  
<a name="building_a_table_with_code"></a>   
## <a name="building-a-table-with-code"></a>テーブルとコードのバインディング  
 次の例では、プログラムを使用して <xref:System.Windows.Documents.Table> を作成し、コンテンツを設定する方法を示します。 テーブルの内容は、5つの行 (<xref:System.Windows.Documents.Table.RowGroups%2A> オブジェクトに含まれる <xref:System.Windows.Documents.TableRow> オブジェクトによって表されます) と6つの列 (<xref:System.Windows.Documents.TableColumn> オブジェクトで表される) に分配されます。 行はさまざまな表示目的で利用されます。たとえば、テーブル全体にタイトルを付けるタイトル行、テーブル内のデータの列を表すヘッダー行、まとめ情報が入るフッター行があります。  "タイトル" 行、"ヘッダー" 行、"フッター" 行という概念はテーブルに固有のものではありません。特性が異なる行にすぎません。 テーブルのセルには実際のコンテンツが含まれます。これは、テキスト、画像、またはその他の [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] 要素で構成できます。  
  
 まず、<xref:System.Windows.Documents.Table>をホストする <xref:System.Windows.Documents.FlowDocument> が作成され、新しい <xref:System.Windows.Documents.Table> が作成され、<xref:System.Windows.Documents.FlowDocument>の内容に追加されます。  
  
 [!code-csharp[TableSnippets#_TableCreate](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tablecreate)]
 [!code-vb[TableSnippets#_TableCreate](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tablecreate)]  
  
 次に、いくつかの書式設定を適用して、6つの <xref:System.Windows.Documents.TableColumn> オブジェクトが作成され、テーブルの <xref:System.Windows.Documents.Table.Columns%2A> コレクションに追加されます。  
  
> [!NOTE]
> テーブルの <xref:System.Windows.Documents.Table.Columns%2A> コレクションでは、0から始まる標準のインデックス作成が使用されていることに注意してください。  
  
 [!code-csharp[TableSnippets#_TableCreateColumns](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tablecreatecolumns)]
 [!code-vb[TableSnippets#_TableCreateColumns](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tablecreatecolumns)]  
  
 次に、タイトル行が作成され、テーブルに追加されます。いくつかの書式設定が適用されます。  タイトル行には、テーブルの 6 つの列にまたがる 1 つのセルが格納されます。  
  
 [!code-csharp[TableSnippets#_TableAddTitleRow](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tableaddtitlerow)]
 [!code-vb[TableSnippets#_TableAddTitleRow](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tableaddtitlerow)]  
  
 次に、ヘッダー行が作成され、テーブルに追加されます。ヘッダー行のセルが作成され、内容が入力されます。  
  
 [!code-csharp[TableSnippets#_TableAddHeaderRow](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tableaddheaderrow)]
 [!code-vb[TableSnippets#_TableAddHeaderRow](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tableaddheaderrow)]  
  
 次に、データの行が作成され、テーブルに追加されます。この行のセルが作成され、内容が入力されます。  この行の作成はヘッダー行の作成に似ていますが、適用する書式が少し異なります。  
  
 [!code-csharp[TableSnippets#_TableAddDataRow](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tableadddatarow)]
 [!code-vb[TableSnippets#_TableAddDataRow](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tableadddatarow)]  
  
 最後に、フッター行が作成され、追加され、書式設定されます。  タイトル行と同様に、フッター行にはテーブルの 6 つの列にまたがる 1 つのセルが格納されます。  
  
 [!code-csharp[TableSnippets#_TableAddFooterRow](~/samples/snippets/csharp/VS_Snippets_Wpf/TableSnippets/CSharp/Table.cs#_tableaddfooterrow)]
 [!code-vb[TableSnippets#_TableAddFooterRow](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TableSnippets/VisualBasic/Table.vb#_tableaddfooterrow)]  
  
## <a name="see-also"></a>参照

- [フロー ドキュメントの概要](flow-document-overview.md)
- [XAML を使用してテーブルを定義する](how-to-define-a-table-with-xaml.md)
- [WPF のドキュメント](documents-in-wpf.md)
- [フロー コンテンツ要素を使用する](how-to-use-flow-content-elements.md)
