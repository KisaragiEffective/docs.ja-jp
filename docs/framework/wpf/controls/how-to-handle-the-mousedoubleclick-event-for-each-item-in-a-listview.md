---
title: '方法 : ListView の各項目の MouseDoubleClick イベントを処理する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView controls [WPF], MouseDoubleClick event
ms.assetid: 81b39369-655a-4585-ac58-4640e5bb8fed
ms.openlocfilehash: 25308ee87fb387787e20c8a8887ae8e4e60742b9
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460223"
---
# <a name="how-to-handle-the-mousedoubleclick-event-for-each-item-in-a-listview"></a>方法 : ListView の各項目の MouseDoubleClick イベントを処理する
<xref:System.Windows.Controls.ListView>内の項目のイベントを処理するには、各 <xref:System.Windows.Controls.ListViewItem>にイベントハンドラーを追加する必要があります。 <xref:System.Windows.Controls.ListView> がデータソースにバインドされている場合は、明示的に <xref:System.Windows.Controls.ListViewItem>を作成する必要はありませんが、<xref:System.Windows.Controls.ListViewItem>のスタイルに <xref:System.Windows.EventSetter> を追加することで、各項目のイベントを処理できます。  
  
## <a name="example"></a>例  
 次の例では、データバインド <xref:System.Windows.Controls.ListView> を作成し、<xref:System.Windows.Style> を作成して、各 <xref:System.Windows.Controls.ListViewItem>にイベントハンドラーを追加します。  
  
 [!code-xaml[ListViewHowTos#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#1)]  
[!code-xaml[ListViewHowTos#5](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#5)]  
[!code-xaml[ListViewHowTos#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#2)]  
  
 次の例では、<xref:System.Windows.Controls.Control.MouseDoubleClick> イベントを処理します。  
  
 [!code-csharp[ListViewHowTos#6](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml.cs#6)]
 [!code-vb[ListViewHowTos#6](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ListViewHowTos/VisualBasic/Window1.xaml.vb#6)]  
  
> [!NOTE]
> <xref:System.Windows.Controls.ListView> をデータソースにバインドするのが最も一般的ですが、<xref:System.Windows.Controls.ListViewItem>を明示的に作成するかどうかにかかわらず、データバインドされていない <xref:System.Windows.Controls.ListView> の各 <xref:System.Windows.Controls.ListViewItem> にイベントハンドラーを追加するには、スタイルを使用します。  明示的かつ暗黙的に作成された <xref:System.Windows.Controls.ListViewItem> コントロールの詳細については、「<xref:System.Windows.Controls.ItemsControl>」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlElement>
- [データ バインディングの概要](../data/data-binding-overview.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [XMLDataProvider と XPath クエリを使用して XML データにバインドする](../data/how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md)
- [ListView の概要](listview-overview.md)
