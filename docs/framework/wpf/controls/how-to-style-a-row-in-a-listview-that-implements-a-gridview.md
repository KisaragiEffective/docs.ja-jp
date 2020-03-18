---
title: '方法 : GridView を実装する ListView で行のスタイルを設定する'
ms.date: 03/30/2017
helpviewer_keywords:
- GridView controls [WPF], styling rows
- styling rows in ListViews implementing GridViews [WPF]
- ListView controls [WPF], styling rows with GridViews
ms.assetid: 2e406ba2-70a0-4e62-841f-0934859de76e
ms.openlocfilehash: ce79899d5c8e825ecb39e14ae8af4e0c33f13db3
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73733545"
---
# <a name="how-to-style-a-row-in-a-listview-that-implements-a-gridview"></a>方法 : GridView を実装する ListView で行のスタイルを設定する
この例では、<xref:System.Windows.Controls.GridView><xref:System.Windows.Controls.ListView.View%2A> モードを実装する <xref:System.Windows.Controls.ListView> コントロールの行のスタイルを指定する方法を示します。  
  
## <a name="example"></a>例  
 <xref:System.Windows.Controls.ListView> コントロールの行のスタイルを設定するには、<xref:System.Windows.Controls.ListView> コントロールで <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> を設定します。 <xref:System.Windows.Controls.ListViewItem> オブジェクトとして表される項目のスタイルを設定します。 <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> は、行の内容を表示するために使用される <xref:System.Windows.Controls.ControlTemplate> オブジェクトを参照します。  
  
 次の例を抽出した完全なサンプルは、XML データベースに格納されている曲情報のコレクションを表示します。 データベースの各曲にはレーティングフィールドがあり、このフィールドの値が曲情報の行を表示する方法を指定します。  
  
 次の例では、song コレクション内の曲を表す <xref:System.Windows.Controls.ListViewItem> オブジェクトの <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> を定義する方法を示します。 <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> は、曲情報の行を表示する方法を指定する <xref:System.Windows.Controls.ControlTemplate> オブジェクトを参照します。  
  
 [!code-xaml[ListViewItemStyleSnippet#ItemContainerStyle](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewItemStyleSnippet/CS/Window1.xaml#itemcontainerstyle)]  
  
 次の例は、テキスト文字列 `"Strongly Recommended"` を行に追加する <xref:System.Windows.Controls.ControlTemplate> を示しています。 このテンプレートは <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> で参照され、曲の評価の値が 5 (5) の場合に表示されます。 <xref:System.Windows.Controls.ControlTemplate> には、<xref:System.Windows.Controls.GridView> ビューモードで定義されている列に行の内容をレイアウトする <xref:System.Windows.Controls.GridViewRowPresenter> オブジェクトが含まれています。  
  
 [!code-xaml[ListViewItemStyleSnippet#ControlTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewItemStyleSnippet/CS/Window1.xaml#controltemplate)]  
  
 次の例では、<xref:System.Windows.Controls.GridView>を定義します。  
  
 [!code-xaml[ListViewItemStyleSnippet#GridView](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewItemStyleSnippet/CS/Window1.xaml#gridview)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Controls.ListView>
- <xref:System.Windows.Controls.GridView>
- [方法トピック](listview-how-to-topics.md)
- [ListView の概要](listview-overview.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
