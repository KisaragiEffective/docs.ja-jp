---
title: '方法 : データ コレクションの既定のビューを取得する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data collections [WPF], creating views of
- data binding [WPF], creating views of data collections
ms.assetid: b641e96c-c2f6-42ea-9c5d-bac81176ad65
ms.openlocfilehash: e82d252ed82e4d2e6d641e8b60e890cc93bb0427
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459116"
---
# <a name="how-to-get-the-default-view-of-a-data-collection"></a>方法 : データ コレクションの既定のビューを取得する
ビューでは、並べ替え、フィルター処理、グループ化の基準に応じて、同じデータコレクションをさまざまな方法で表示できます。 すべてのコレクションには共有された既定のビューが1つあります。これは、バインディングがソースとしてコレクションを指定するときに実際のバインディングソースとして使用されます。 この例は、コレクションの既定のビューを取得する方法を示しています。  
  
## <a name="example"></a>例  
 ビューを作成するには、コレクションへのオブジェクト参照が必要です。 このデータオブジェクトを取得するには、データコンテキストを取得するか、データソースのプロパティを取得するか、またはバインディングのプロパティを取得して、独自の分離コードオブジェクトを参照します。 この例では、データオブジェクトの <xref:System.Windows.FrameworkElement.DataContext%2A> を取得し、それを使用してこのコレクションの既定のコレクションビューを直接取得する方法を示します。  
  
 [!code-csharp[CollectionView#2](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionView/CSharp/Page1.xaml.cs#2)]
 [!code-vb[CollectionView#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CollectionView/VisualBasic/Page1.xaml.vb#2)]  
  
 この例では、ルート要素は <xref:System.Windows.Controls.StackPanel>です。 <xref:System.Windows.FrameworkElement.DataContext%2A> が*Mydatasource*に設定されています。これは、 *Order*オブジェクトの <xref:System.Collections.ObjectModel.ObservableCollection%601> であるデータプロバイダーを参照します。  
  
 [!code-xaml[CollectionView#CollectionViewDataContext](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionView/CSharp/Page1.xaml#collectionviewdatacontext)]  
  
 または、<xref:System.Windows.Data.CollectionViewSource> クラスを使用して、独自のコレクションビューをインスタンス化し、バインドすることもできます。 このコレクションビューは、直接バインドするコントロールによってのみ共有されます。 例については、「[データバインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」の「ビューを作成する方法」セクションを参照してください。  
  
 コレクションビューで提供される機能の例については、「[ビュー内のデータの並べ替え](how-to-sort-data-in-a-view.md)」、「[ビューのデータのフィルター処理](how-to-filter-data-in-a-view.md)」、および「[データ CollectionView 内のオブジェクト間の移動](how-to-navigate-through-the-objects-in-a-data-collectionview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XAML でビューを使用してデータの並べ替えおよびグループ化を行う](how-to-sort-and-group-data-using-a-view-in-xaml.md)
- [方法トピック](data-binding-how-to-topics.md)
