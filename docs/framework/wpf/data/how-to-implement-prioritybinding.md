---
title: '方法 : PriorityBinding を実装する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], PriorityBinding class
ms.assetid: d63b65ab-b3e9-4322-9aa8-1450f8d89532
ms.openlocfilehash: 343b0aca4736905f3a0cbff5a0f76b170da0c920
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459785"
---
# <a name="how-to-implement-prioritybinding"></a>方法 : PriorityBinding を実装する
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の <xref:System.Windows.Data.PriorityBinding> は、バインドの一覧を指定することによって機能します。 バインドの一覧は、優先順位が最も高い順に並べ替えられます。 最も優先順位の高いバインドが処理時に値を正常に返す場合、リスト内の他のバインドを処理する必要はありません。 最も優先度の高いバインドが評価されるまでに長い時間がかかる場合もありますが、高い優先順位のバインドによって値が正常に返されるまで、値が正常に返される次に高い優先順位が使用されます。  
  
## <a name="example"></a>例  
 <xref:System.Windows.Data.PriorityBinding> のしくみを示すために、`AsyncDataSource` オブジェクトは、`FastDP`、`SlowerDP`、および `SlowestDP`の3つのプロパティを使用して作成されています。  
  
 `FastDP` の get アクセサーは、`_fastDP` データメンバーの値を返します。  
  
 `SlowerDP` の get アクセサーは、`_slowerDP` データメンバーの値を返す前に3秒間待機します。  
  
 `SlowestDP` の get アクセサーは、5秒間待機してから `_slowestDP` データメンバーの値を返します。  
  
> [!NOTE]
> この例は、デモンストレーション目的のみで提供されます。 .NET ガイドラインでは、フィールドセットよりも速度が低いプロパティを定義することをお勧めします。 詳細については、「[プロパティとメソッドの選択](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229054(v=vs.100))」を参照してください。  
  
 [!code-csharp[PriorityBinding#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PriorityBinding/CSharp/Window1.xaml.cs#1)]
 [!code-vb[PriorityBinding#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PriorityBinding/VisualBasic/AsyncDataSource.vb#1)]  
  
 <xref:System.Windows.Controls.TextBlock.Text%2A> プロパティは <xref:System.Windows.Data.PriorityBinding>を使用して上記の `AsyncDS` にバインドされます。  
  
 [!code-xaml[PriorityBinding#2](~/samples/snippets/csharp/VS_Snippets_Wpf/PriorityBinding/CSharp/Window1.xaml#2)]  
  
 バインドエンジンは、<xref:System.Windows.Data.Binding> オブジェクトを処理するときに、最初の <xref:System.Windows.Data.Binding>から開始します。これは `SlowestDP` プロパティにバインドされています。 この <xref:System.Windows.Data.Binding> が処理されると、5秒間スリープ状態になっているため、値は正常に返されません。そのため、次の <xref:System.Windows.Data.Binding> 要素が処理されます。 次の <xref:System.Windows.Data.Binding> では、3秒間スリープ状態になるため、値は正常に返されません。 バインドエンジンは、次の <xref:System.Windows.Data.Binding> 要素に移動します。この要素は、`FastDP` プロパティにバインドされます。 この <xref:System.Windows.Data.Binding> は、値 "Fast Value" を返します。 <xref:System.Windows.Controls.TextBlock> に "Fast Value" という値が表示されるようになりました。  
  
 3秒が経過すると、`SlowerDP` プロパティは値 "低速値" を返します。 <xref:System.Windows.Controls.TextBlock> に、値 "低速値" が表示されます。  
  
 5秒が経過すると、`SlowestDP` プロパティによって "低速値" という値が返されます。 最初にリストされているので、そのバインディングの優先順位は最も高くなります。 <xref:System.Windows.Controls.TextBlock> に "低速値" という値が表示されるようになりました。  
  
 バインドから成功した戻り値と見なされる内容については、「<xref:System.Windows.Data.PriorityBinding>」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.Binding.IsAsync%2A?displayProperty=nameWithType>
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
