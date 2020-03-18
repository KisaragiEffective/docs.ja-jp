---
title: '方法 : 階層データでマスター詳細パターンを使用する'
ms.date: 03/30/2017
helpviewer_keywords:
- data binding [WPF], Master-Detail data paradigm
- Master-Detail data paradigm
ms.assetid: 11429b9e-058d-4084-bfb6-2cf209c8ddf7
ms.openlocfilehash: e4183ddc3868a1568662853b46e05348df129092
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73733479"
---
# <a name="how-to-use-the-master-detail-pattern-with-hierarchical-data"></a>方法 : 階層データでマスター詳細パターンを使用する
この例では、マスター/詳細シナリオを実装する方法を示します。  
  
## <a name="example"></a>例  
 この例では、`LeagueList` は `Leagues`のコレクションです。 各 `League` には `Divisions`の `Name` とコレクションがあり、各 `Division` には名前と `Teams`のコレクションがあります。 各 `Team` にはチーム名があります。  
  
 [!code-xaml[MasterDetail#HowTo1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MasterDetail/VisualBasic/Page1.xaml#howto1)]  
[!code-xaml[MasterDetail#HowTo2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MasterDetail/VisualBasic/Page1.xaml#howto2)]  
  
 次に示すのは、この例のスクリーンショットです。 `Divisions` <xref:System.Windows.Controls.ListBox> によって、`Leagues` <xref:System.Windows.Controls.ListBox> の選択が自動的に追跡され、対応するデータが表示されます。 `Teams` <xref:System.Windows.Controls.ListBox> は、他の2つの <xref:System.Windows.Controls.ListBox> コントロールでの選択を追跡します。  
  
 ![マスター&#45;詳細シナリオの例を示すスクリーンショット。](./media/how-to-use-the-master-detail-pattern-with-hierarchical-data/databinding-master-detail-scenario.png)  
  
 この例では、次の2つの点に注意してください。  
  
1. 3つの <xref:System.Windows.Controls.ListBox> コントロールは、同じソースにバインドされます。 バインドの <xref:System.Windows.Data.Binding.Path%2A> プロパティを設定して、<xref:System.Windows.Controls.ListBox> に表示するデータのレベルを指定します。  
  
2. <xref:System.Windows.Controls.Primitives.Selector.IsSynchronizedWithCurrentItem%2A> プロパティは、追跡する選択範囲の <xref:System.Windows.Controls.ListBox> コントロールで `true` するように設定する必要があります。 このプロパティを設定すると、選択した項目が常に <xref:System.Windows.Controls.ItemCollection.CurrentItem%2A>として設定されます。 また、<xref:System.Windows.Controls.ListBox> が <xref:System.Windows.Data.CollectionViewSource>からデータを取得する場合は、選択と通貨が自動的に同期されます。  
  
 XML データを使用する場合、この手法は少し異なります。 例については、「[階層 XML データでマスター詳細パターンを使用する](how-to-use-the-master-detail-pattern-with-hierarchical-xml-data.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.HierarchicalDataTemplate>
- [コレクションにバインドして選択に基づく情報を表示する](how-to-bind-to-a-collection-and-display-information-based-on-selection.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [データ テンプレートの概要](data-templating-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
