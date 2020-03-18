---
title: '方法 : バインディングの方向を指定する'
ms.date: 03/30/2017
helpviewer_keywords:
- direction of binding [WPF]
- binding direction [WPF]
- data binding [WPF], direction of binding
ms.assetid: 37334478-028b-4514-86c9-1420709f4818
ms.openlocfilehash: 8f7d843382ee3409047d7cf9549267d582883984
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459099"
---
# <a name="how-to-specify-the-direction-of-the-binding"></a>方法: バインディングの方向を指定する

この例では、バインドの更新先 (バインディング ターゲット (ターゲット) のプロパティのみ、バインディング ソース (ソース) のプロパティのみ、またはターゲットのプロパティとソースのプロパティの両方) を指定する方法を示します。  
  
## <a name="example"></a>例  
 バインディングの方向を指定するには、<xref:System.Windows.Data.Binding.Mode%2A?displayProperty=nameWithType> プロパティを使用します。 更新プログラムのバインドに使用できるオプションを次に示します。  
  
- ターゲットプロパティまたはソースプロパティが変更されるたびに、<xref:System.Windows.Data.BindingMode.TwoWay?displayProperty=nameWithType> はターゲットプロパティまたはプロパティを更新します。  
  
- <xref:System.Windows.Data.BindingMode.OneWay?displayProperty=nameWithType> は、ソースプロパティが変更された場合にのみターゲットプロパティを更新します。  
  
- <xref:System.Windows.Data.BindingMode.OneTime?displayProperty=nameWithType> は、アプリケーションが起動したとき、または <xref:System.Windows.FrameworkElement.DataContext%2A> が変更されたときにのみ、ターゲットプロパティを更新します。  
  
- ターゲットプロパティが変更されると、<xref:System.Windows.Data.BindingMode.OneWayToSource?displayProperty=nameWithType> によって source プロパティが更新されます。  
  
- <xref:System.Windows.Data.BindingMode.Default?displayProperty=nameWithType> を指定すると、target プロパティの既定の <xref:System.Windows.Data.Binding.Mode%2A> 値が使用されます。  
  
 詳細については、<xref:System.Windows.Data.BindingMode> 列挙型のページをご覧ください。  
  
 <xref:System.Windows.Data.Binding.Mode%2A> プロパティを設定する方法を次の例に示します。  
  
 [!code-xaml[DirectionalBinding#4](~/samples/snippets/csharp/VS_Snippets_Wpf/DirectionalBinding/CSharp/Page1.xaml#4)]  
  
 ソースの変更を検出するには (<xref:System.Windows.Data.BindingMode.OneWay> および <xref:System.Windows.Data.BindingMode.TwoWay> バインドに適用)、ソースは <xref:System.ComponentModel.INotifyPropertyChanged>などの適切なプロパティ変更通知メカニズムを実装する必要があります。 <xref:System.ComponentModel.INotifyPropertyChanged> 実装の例については、「[プロパティ変更通知の実装](how-to-implement-property-change-notification.md)」を参照してください。  
  
 <xref:System.Windows.Data.BindingMode.TwoWay> または <xref:System.Windows.Data.BindingMode.OneWayToSource> のバインドの場合、<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> プロパティを設定することによって、ソースの更新のタイミングを制御できます。 詳細については、「<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A>」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.Binding>
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
