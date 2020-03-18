---
title: '方法 : ListView の列の水平方向の配置を変更する'
ms.date: 03/30/2017
helpviewer_keywords:
- ListView controls [WPF], horizontal alignment [WPF]
ms.assetid: b9573e44-9dad-4d14-939c-7859ca372758
ms.openlocfilehash: 5447f1a73b008b2ed4f345eba00f4d631e11e257
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458602"
---
# <a name="how-to-change-the-horizontal-alignment-of-a-column-in-a-listview"></a>方法 : ListView の列の水平方向の配置を変更する
既定では、<xref:System.Windows.Controls.ListViewItem> 内の各列の内容は左揃えになっています。 各列の配置を変更するには、<xref:System.Windows.DataTemplate> を指定し、<xref:System.Windows.DataTemplate>内の要素の <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> プロパティを設定します。 このトピックでは、<xref:System.Windows.Controls.ListView> が既定でコンテンツを配置する方法と、<xref:System.Windows.Controls.ListView>内の1つの列の配置を変更する方法について説明します。  
  
## <a name="example"></a>例  
 次の例では、`Title` 列と `ISBN` 列のデータが左揃えになっています。  
  
 [!code-xaml[ListViewHowTos#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#1)]  
[!code-xaml[ListViewHowTos#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#2)]  
  
 `ISBN` 列の配置を変更するには、各 <xref:System.Windows.Controls.ListViewItem> の <xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A> プロパティを <xref:System.Windows.HorizontalAlignment.Stretch>するように指定する必要があります。これにより、各 <xref:System.Windows.Controls.ListViewItem> 内の要素は、各列の幅全体にわたって、または配置することができます。 <xref:System.Windows.Controls.ListView> はデータソースにバインドされているため、<xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A>を設定するスタイルを作成する必要があります。 次に、<xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> プロパティを使用する代わりに、<xref:System.Windows.DataTemplate> を使用してコンテンツを表示する必要があります。 各テンプレートの `ISBN` を表示するために、<xref:System.Windows.DataTemplate> には <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> プロパティが <xref:System.Windows.HorizontalAlignment.Right>に設定されている <xref:System.Windows.Controls.TextBlock> のみを含めることができます。  
  
 次の例では、`ISBN` 列を右に揃えるために必要なスタイルと <xref:System.Windows.DataTemplate> を定義し、<xref:System.Windows.DataTemplate>を参照するように <xref:System.Windows.Controls.GridViewColumn> を変更します。  
  
 [!code-xaml[ListViewHowTos#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#3)]  
[!code-xaml[ListViewHowTos#4](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewHowTos/CSharp/Window1.xaml#4)]  
  
## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [データ テンプレートの概要](../data/data-templating-overview.md)
- [XMLDataProvider と XPath クエリを使用して XML データにバインドする](../data/how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md)
- [ListView の概要](listview-overview.md)
