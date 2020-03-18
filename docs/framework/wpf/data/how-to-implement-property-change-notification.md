---
title: '方法 : プロパティの変更通知を実装する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- notifications of change [WPF]
- data binding [WPF], property change notifications
- change notifications [WPF]
- properties [WPF], change notifications
ms.assetid: 30b59d9e-8c3a-4349-aa82-4be837e841cf
ms.openlocfilehash: 4f9ff49a443577e119b0c1079abbe23bd7ede4c4
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459748"
---
# <a name="how-to-implement-property-change-notification"></a>方法 : プロパティの変更通知を実装する
バインディングターゲットプロパティでバインドソースの動的な変更を自動的に反映できるように <xref:System.Windows.Data.BindingMode.OneWay> または <xref:System.Windows.Data.BindingMode.TwoWay> バインディングをサポートするには (たとえば、ユーザーがフォームを編集したときにプレビューウィンドウが自動的に更新されるようにするため)、クラスは次のようにする必要があります。適切なプロパティ変更通知を提供します。 この例では、<xref:System.ComponentModel.INotifyPropertyChanged>を実装するクラスを作成する方法を示します。  
  
## <a name="example"></a>例  
 <xref:System.ComponentModel.INotifyPropertyChanged> を実装するには、<xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> イベントを宣言し、`OnPropertyChanged` メソッドを作成する必要があります。 次に、変更を通知する必要のある各プロパティについて、そのプロパティが更新されるたびに `OnPropertyChanged` を呼び出します。  
  
 [!code-csharp[SimpleBinding#PersonClass](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Person.cs#personclass)]
 [!code-vb[SimpleBinding#PersonClass](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Person.vb#personclass)]  
  
 `Person` クラスを使用して <xref:System.Windows.Data.BindingMode.TwoWay> バインディングをサポートする方法の例については、「 [TextBox テキストでソースを更新するタイミングを制御](how-to-control-when-the-textbox-text-updates-the-source.md)する」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [バインディング ソースの概要](binding-sources-overview.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
