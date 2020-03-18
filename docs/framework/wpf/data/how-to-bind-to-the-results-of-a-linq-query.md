---
title: '方法 : LINQ クエリの結果にバインドする'
ms.date: 03/30/2017
helpviewer_keywords:
- running a LINQ query [WPF], bind to results
- binding to LINQ query results [WPF]
ms.assetid: ff2844d9-17ed-4ea6-aab1-5111af0bc684
ms.openlocfilehash: d21ac5f366e001ea76d6eda64ed62583248796f6
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73454418"
---
# <a name="how-to-bind-to-the-results-of-a-linq-query"></a>方法 : LINQ クエリの結果にバインドする

この例では、LINQ クエリを実行し、その結果にバインドする方法を示します。

## <a name="example"></a>例

次の例では、2つのリストボックスを作成します。 最初のリストボックスには、3つのリスト項目が含まれています。

[!code-xaml[LinqExample#UI](~/samples/snippets/csharp/VS_Snippets_Wpf/LinqExample/CSharp/Window1.xaml#ui)]

最初のリストボックスから項目を選択すると、次のイベントハンドラーが呼び出されます。 この例では、`Tasks` は `Task` オブジェクトのコレクションです。 `Task` クラスには、`Priority`という名前のプロパティがあります。 このイベントハンドラーは、選択した優先順位値を持つ `Task` オブジェクトのコレクションを返す LINQ クエリを実行し、それを <xref:System.Windows.FrameworkElement.DataContext%2A>として設定します。

[!code-csharp[LinqExample#Using](~/samples/snippets/csharp/VS_Snippets_Wpf/LinqExample/CSharp/Window1.xaml.cs#using)]
[!code-csharp[LinqExample#Tasks](~/samples/snippets/csharp/VS_Snippets_Wpf/LinqExample/CSharp/Window1.xaml.cs#tasks)]
[!code-csharp[LinqExample#Handler](~/samples/snippets/csharp/VS_Snippets_Wpf/LinqExample/CSharp/Window1.xaml.cs#handler)]

<xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> 値が `{Binding}`に設定されているため、2番目のリストボックスはそのコレクションにバインドされます。 その結果、返されたコレクションが (`myTaskTemplate` <xref:System.Windows.DataTemplate>に基づいて) 表示されます。

## <a name="see-also"></a>関連項目

- [XAML でデータをバインディング可能にする](how-to-make-data-available-for-binding-in-xaml.md)
- [コレクションにバインドして選択に基づく情報を表示する](how-to-bind-to-a-collection-and-display-information-based-on-selection.md)
- [WPF Version 4.5 の新機能](../getting-started/whats-new.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
