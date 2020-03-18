---
title: '方法 : コレクションにバインドして選択に基づく情報を表示する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data collections [WPF], selecting data for views
- data binding [WPF], creating views of data collections
- data binding [WPF], selecting data for views
- data binding [WPF], binding to collections
ms.assetid: 952a7d76-dd29-49e5-86f5-32c4530e70eb
ms.openlocfilehash: 032a1d98e1aa80ea755f5922f79d43a796e9697e
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459150"
---
# <a name="how-to-bind-to-a-collection-and-display-information-based-on-selection"></a>方法 : コレクションにバインドして選択に基づく情報を表示する
単純なマスター/詳細シナリオでは、<xref:System.Windows.Controls.ListBox>などのデータバインド <xref:System.Windows.Controls.ItemsControl> があります。 ユーザーの選択に基づいて、選択した項目に関する詳細情報を表示します。 この例は、このシナリオを実装する方法を示しています。  
  
## <a name="example"></a>例  
 この例では、`People` は `Person` クラスの <xref:System.Collections.ObjectModel.ObservableCollection%601> です。 この `Person` クラスには、`FirstName`、`LastName`、および `HomeTown`の3つのプロパティが含まれています。すべての型 `string`です。  
  
 [!code-xaml[CollectionBinding#Source](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#source)]  
[!code-xaml[CollectionBinding#UI](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#ui)]  
  
 <xref:System.Windows.Controls.ContentControl> は、`Person` の情報の表示方法を定義する次の <xref:System.Windows.DataTemplate> を使用します。  
  
 [!code-xaml[CollectionBinding#DetailTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#detailtemplate)]  
  
 次に、この例で生成される内容のスクリーンショットを示します。 <xref:System.Windows.Controls.ContentControl> には、選択したユーザーの他のプロパティが表示されます。  
  
 ![コレクションへのバインド](./media/databinding-collectionbindingsample.png "DataBinding_CollectionBindingSample")  
  
 この例では、次の2つの点に注意してください。  
  
1. <xref:System.Windows.Controls.ListBox> と <xref:System.Windows.Controls.ContentControl> は同じソースにバインドされます。 両方のコントロールがコレクションオブジェクト全体にバインドされるため、両方のバインドの <xref:System.Windows.Data.Binding.Path%2A> プロパティは指定されません。  
  
2. これを機能させるには、<xref:System.Windows.Controls.Primitives.Selector.IsSynchronizedWithCurrentItem%2A> プロパティを `true` に設定する必要があります。 このプロパティを設定すると、選択した項目が常に <xref:System.Windows.Controls.ItemCollection.CurrentItem%2A>として設定されます。 また、<xref:System.Windows.Controls.ListBox> が <xref:System.Windows.Data.CollectionViewSource>からデータを取得する場合は、選択と通貨が自動的に同期されます。  
  
 `Person` クラスは、次のように `ToString` メソッドをオーバーライドすることに注意してください。 既定では、<xref:System.Windows.Controls.ListBox> は `ToString` を呼び出し、バインドされたコレクション内の各オブジェクトの文字列形式を表示します。 そのため、各 `Person` が <xref:System.Windows.Controls.ListBox>の名として表示されます。  
  
 [!code-csharp[CollectionBinding#ToString](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Data.cs#tostring)]
 [!code-vb[CollectionBinding#ToString](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CollectionBinding/VisualBasic/Person.vb#tostring)]  
  
## <a name="see-also"></a>関連項目

- [階層データでマスター詳細パターンを使用する](how-to-use-the-master-detail-pattern-with-hierarchical-data.md)
- [階層 XML データでマスター詳細パターンを使用する](how-to-use-the-master-detail-pattern-with-hierarchical-xml-data.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [データ テンプレートの概要](data-templating-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
