---
title: '方法 : コードでバインディングを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binding data [WPF], creating
- data binding [WPF], creating
ms.assetid: 1a606db9-cf5f-42ed-a1c5-9e4722ec77a0
ms.openlocfilehash: 616487a16ebbe6e23fe067fb7ce72644aa3f919f
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458850"
---
# <a name="how-to-create-a-binding-in-code"></a>方法 : コードでバインディングを作成する
この例では、コードで <xref:System.Windows.Data.Binding> を作成して設定する方法を示します。  
  
## <a name="example"></a>例  
 <xref:System.Windows.FrameworkElement> クラスと <xref:System.Windows.FrameworkContentElement> クラスはどちらも `SetBinding` メソッドを公開します。 これらのクラスのいずれかを継承する要素をバインドする場合は、<xref:System.Windows.FrameworkElement.SetBinding%2A> メソッドを直接呼び出すことができます。  
  
 次の例では、`MyDataProperty`という名前のプロパティを含む、`MyData`という名前のクラスを作成します。  
  
 [!code-csharp[CodeOnlyBinding#DataObject](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/MyData.cs#dataobject)]
 [!code-vb[CodeOnlyBinding#DataObject](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/MyData.vb#dataobject)]  
  
 バインドオブジェクトを作成してバインディングのソースを設定する方法を次の例に示します。  この例では、<xref:System.Windows.FrameworkElement.SetBinding%2A> を使用して、<xref:System.Windows.Controls.TextBlock> コントロールである `myText`の <xref:System.Windows.Controls.TextBlock.Text%2A> プロパティを `MyDataProperty`にバインドします。  
  
 [!code-csharp[CodeOnlyBinding#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/binding.cs#1)]
 [!code-vb[CodeOnlyBinding#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/App.vb#1)]  
  
 完全なコードサンプルについては、「[コードのみのバインディングのサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771500(v=vs.90))」を参照してください。  
  
 <xref:System.Windows.FrameworkElement.SetBinding%2A>を呼び出す代わりに、<xref:System.Windows.Data.BindingOperations> クラスの <xref:System.Windows.Data.BindingOperations.SetBinding%2A> 静的メソッドを使用できます。 次の例では、`myText` を `myDataProperty`にバインドするために、<xref:System.Windows.FrameworkElement.SetBinding%2A?displayProperty=nameWithType> ではなく <xref:System.Windows.Data.BindingOperations.SetBinding%2A?displayProperty=nameWithType> を呼び出します。  
  
 [!code-csharp[CodeOnlyBinding#BOSetBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/binding.cs#bosetbinding)]
 [!code-vb[CodeOnlyBinding#BOSetBinding](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/App.vb#bosetbinding)]  
  
## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
