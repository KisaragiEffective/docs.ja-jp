---
title: '方法 : 簡単なバインディングを作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- simple binding [WPF], creating
- data binding [WPF], creating simple bindings
- binding data [WPF], creating
ms.assetid: 69b80f72-6259-44cb-8294-5bdcebca1e08
ms.openlocfilehash: faef59ed426059eb2d488d0584d3325c8d46d415
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73453500"
---
# <a name="how-to-create-a-simple-binding"></a>方法 : 簡単なバインディングを作成する
この例では、単純な <xref:System.Windows.Data.Binding>を作成する方法を示します。  
  
## <a name="example"></a>例  
 この例では、`PersonName`という名前の文字列プロパティを持つ `Person` オブジェクトがあります。 `Person` オブジェクトは、`SDKSample`と呼ばれる名前空間で定義されています。  
  
 次の例の `<src>` 要素を含む強調表示された行では、`Joe`の `PersonName` プロパティ値を使用して `Person` オブジェクトをインスタンス化します。 これは `Resources` セクションで行い、`x:Key`を割り当てます。  
  
 [!code-xaml[SimpleBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Page1.xaml?highlight=9,37)]  
  
 `<TextBlock>` 要素を含む強調表示された行は、<xref:System.Windows.Controls.TextBlock> コントロールを `PersonName` プロパティにバインドします。 その結果、<xref:System.Windows.Controls.TextBlock> に "Joe" という値が表示されます。  
  
## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
