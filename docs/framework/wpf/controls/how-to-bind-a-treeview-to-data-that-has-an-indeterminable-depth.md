---
title: '方法 : 深度がわからないデータに TreeView をバインドする'
ms.date: 03/30/2017
helpviewer_keywords:
- TreeView control [WPF], binding to data of indeterminate depth
ms.assetid: daddcd74-1b0f-4ffd-baeb-ec934c5e0f53
ms.openlocfilehash: cd9a1ee015ebb707a7a06d1c062a1bb3003c96e8
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458613"
---
# <a name="how-to-bind-a-treeview-to-data-that-has-an-indeterminable-depth"></a>方法 : 深度がわからないデータに TreeView をバインドする
深さが不明なデータソースに <xref:System.Windows.Controls.TreeView> をバインドすることが必要になる場合があります。  これは、ファイルシステムなどのデータが再帰的に行われている場合、フォルダーにフォルダーを格納できる場合、または従業員が直属の部下として他の従業員を持っている企業の組織構造の場合に発生します。  
  
 データソースには、階層オブジェクトモデルが必要です。 たとえば、`Employee` クラスには、従業員の直属の部下である Employee オブジェクトのコレクションが含まれている場合があります。 データが階層化されていない方法で表されている場合は、データの階層表現を作成する必要があります。  
  
 <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A?displayProperty=nameWithType> プロパティを設定し、<xref:System.Windows.Controls.ItemsControl> が各子項目の <xref:System.Windows.Controls.ItemsControl> を生成した場合、子 <xref:System.Windows.Controls.ItemsControl> は親と同じ <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> を使用します。 たとえば、データバインド <xref:System.Windows.Controls.TreeView>の <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> プロパティを設定した場合、生成される各 <xref:System.Windows.Controls.TreeViewItem> は、<xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> の <xref:System.Windows.Controls.TreeView>プロパティに割り当てられた <xref:System.Windows.DataTemplate> を使用します。  
  
 <xref:System.Windows.HierarchicalDataTemplate> を使用すると、データテンプレートで <xref:System.Windows.Controls.TreeViewItem>または <xref:System.Windows.Controls.HeaderedItemsControl>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> を指定できます。 <xref:System.Windows.HierarchicalDataTemplate.ItemsSource%2A?displayProperty=nameWithType> プロパティを設定すると、<xref:System.Windows.HierarchicalDataTemplate> が適用されるときにその値が使用されます。 <xref:System.Windows.HierarchicalDataTemplate>を使用すると、<xref:System.Windows.Controls.TreeView>内の各 <xref:System.Windows.Controls.TreeViewItem> の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> を再帰的に設定できます。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Windows.Controls.TreeView> を階層データにバインドし、<xref:System.Windows.HierarchicalDataTemplate> を使用して各 <xref:System.Windows.Controls.TreeViewItem>の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> を指定する方法を示します。  <xref:System.Windows.Controls.TreeView> は、会社の従業員を表す XML データにバインドされます。  各 `Employee` 要素には、だれに報告するかを示す他の `Employee` 要素を含めることができます。 データが再帰的であるため、<xref:System.Windows.HierarchicalDataTemplate> を各レベルに適用できます。  
  
 [!code-xaml[TreeViewWithUnknownDepth#1](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewWithUnknownDepth/CS/Window1.xaml#1)]  
  
## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [データ テンプレートの概要](../data/data-templating-overview.md)
