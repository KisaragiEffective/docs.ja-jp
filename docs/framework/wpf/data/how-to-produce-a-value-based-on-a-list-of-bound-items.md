---
title: '方法 : バインドされた項目の一覧に基づいて値を生成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], MultiBinding
- Multibinding [WPF]
ms.assetid: b3d06378-b511-4181-95aa-316d60c9229b
ms.openlocfilehash: da183a34eb85de54b1e3f54f8d14c09e25640165
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459693"
---
# <a name="how-to-produce-a-value-based-on-a-list-of-bound-items"></a>方法 : バインドされた項目の一覧に基づいて値を生成する
<xref:System.Windows.Data.MultiBinding> を使用すると、バインディングターゲットプロパティをソースプロパティのリストにバインドし、指定された入力で値を生成するロジックを適用できます。 この例では、<xref:System.Windows.Data.MultiBinding>を使用する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、`NameListData` は、`PersonName` オブジェクトのコレクションを参照します。このオブジェクトは、`firstName` と `lastName` の 2 つのプロパティを含みます。 次の例では、姓が最初の人の姓と名を示す <xref:System.Windows.Controls.TextBlock> を生成します。  
  
 [!code-xaml[MultiBinding#Resources1](~/samples/snippets/csharp/VS_Snippets_Wpf/MultiBinding/CSharp/Window1.xaml#resources1)]  
[!code-xaml[MultiBinding#Resources2](~/samples/snippets/csharp/VS_Snippets_Wpf/MultiBinding/CSharp/Window1.xaml#resources2)]  
[!code-xaml[MultiBinding#MultiBindingTextBox2](~/samples/snippets/csharp/VS_Snippets_Wpf/MultiBinding/CSharp/Window1.xaml#multibindingtextbox2)]  
[!code-xaml[MultiBinding#Window](~/samples/snippets/csharp/VS_Snippets_Wpf/MultiBinding/CSharp/Window1.xaml#window)]  
  
 姓が先の形式を生成する方法を理解するには、次に示す `NameConverter` の実装をご覧ください。  
  
 [!code-csharp[MultiBinding#3](~/samples/snippets/csharp/VS_Snippets_Wpf/MultiBinding/CSharp/NameConverter.cs#3)]
 [!code-vb[MultiBinding#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MultiBinding/VisualBasic/NameConverter.vb#3)]  
  
 `NameConverter` は、<xref:System.Windows.Data.IMultiValueConverter> インターフェイスを実装します。 `NameConverter` は、個々のバインディングから値を受け取り、それらを値のオブジェクト配列に格納します。 <xref:System.Windows.Data.Binding> 要素が <xref:System.Windows.Data.MultiBinding> 要素の下に表示される順序は、これらの値が配列に格納される順序です。 <xref:System.Windows.Data.MultiBinding.ConverterParameter%2A> 属性の値は、<xref:System.Windows.Data.MultiBinding.Converter%2A> メソッドのパラメーター引数によって参照されます。このメソッドは、パラメーターに対してスイッチを実行し、名前の書式設定方法を決定します。  
  
## <a name="see-also"></a>関連項目

- [バインドされたデータを変換する](how-to-convert-bound-data.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
