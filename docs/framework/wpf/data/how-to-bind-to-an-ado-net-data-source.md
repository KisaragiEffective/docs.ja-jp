---
title: '方法 : ADO.NET データ ソースにバインドする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], binding to ADO.NET data sources
- ADO.NET data sources [WPF], binding to
- binding [WPF], to ADO.NET data sources
ms.assetid: a70c6d7b-7b38-4fdf-b655-4804db7c8315
ms.openlocfilehash: 0b3d7147f45bee5e8abd95bdc51c5f5f695cf830
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458406"
---
# <a name="how-to-bind-to-an-adonet-data-source"></a>方法 : ADO.NET データ ソースにバインドする

この例では、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] <xref:System.Windows.Controls.ListBox> コントロールを ADO.NET `DataSet`にバインドする方法を示します。

## <a name="example"></a>例

この例では、データ ソース (接続文字列で指定された `OleDbConnection` ファイル) に接続するために、`Access MDB` オブジェクトを使用します。 接続が確立されると、`OleDbDataAdapter` オブジェクトが作成されます。 `OleDbDataAdapter` オブジェクトは、select 構造化照会言語 (SQL) ステートメントを実行して、データベースからレコードセットを取得します。 SQL コマンドの結果は、`OleDbDataAdapter`の `Fill` メソッドを呼び出すことによって、`DataSet` の `DataTable` に格納されます。 この例の `DataTable` には、`BookTable` という名前が付いています。 次に、<xref:System.Windows.Controls.ListBox> の <xref:System.Windows.FrameworkElement.DataContext%2A> プロパティを `DataSet` オブジェクトに設定します。

[!code-csharp[ADODataSet#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ADODataSet/CSharp/Window1.xaml.cs#1)]
[!code-vb[ADODataSet#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ADODataSet/VisualBasic/Window1.xaml.vb#1)]

次に、<xref:System.Windows.Controls.ListBox> の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティを `DataSet`の `BookTable` にバインドできます。

[!code-xaml[ADODataSet#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ADODataSet/CSharp/Window1.xaml#2)]

`BookItemTemplate` は、データの表示方法を定義する <xref:System.Windows.DataTemplate> です。

[!code-xaml[ADODataSet#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ADODataSet/CSharp/Window1.xaml#3)]

`IntColorConverter` は、`int` を 1 つの色に変換します。 このコンバーターを使用すると、`NumPages` の値が350未満の場合は、3番目の <xref:System.Windows.Controls.TextBlock> の <xref:System.Windows.Controls.TextBlock.Background%2A> の色が緑色で表示されます。それ以外の場合は赤で表示されます。 コンバーターの実装は、ここでは示されていません。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.BindingListCollectionView>
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
