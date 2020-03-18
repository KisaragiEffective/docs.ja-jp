---
title: '方法 : バインドされたデータを変換する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- converting [WPF], bound data
- data binding [WPF], converting bound data
- binding data [WPF], converting bound data
ms.assetid: b00aaa19-c6df-4c3b-a9fd-88a0b488df2b
ms.openlocfilehash: f9ad390626092d481bf47f017f643a29302c1b29
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458858"
---
# <a name="how-to-convert-bound-data"></a>方法 : バインドされたデータを変換する
この例では、バインディングで使用されるデータへの変換を適用する方法を示します。  
  
 バインド時にデータを変換するには、<xref:System.Windows.Data.IValueConverter> インターフェイスを実装するクラスを作成する必要があります。これには、<xref:System.Windows.Data.IValueConverter.Convert%2A> メソッドと <xref:System.Windows.Data.IValueConverter.ConvertBack%2A> メソッドが含まれます。  
  
## <a name="example"></a>例  
 次の例は、年、月、および日のみが表示されるように、渡された日付値を変換する日付コンバーターの実装を示しています。 <xref:System.Windows.Data.IValueConverter> インターフェイスを実装する場合は、次の例のように、実装を <xref:System.Windows.Data.ValueConversionAttribute> 属性で装飾して、変換に関係するデータ型を開発ツールに示すことをお勧めします。  
  
 [!code-csharp[DataBindingLab#18](~/samples/snippets/csharp/VS_Snippets_Wpf/DataBindingLab/CSharp/DateConverter.cs#18)]
 [!code-vb[DataBindingLab#18](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataBindingLab/VisualBasic/DateConverter.vb#18)]  
  
 コンバーターを作成したら、それをリソースとして [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルに追加できます。 次の例では、 *src*は*DateConverter*が定義されている名前空間にマップされます。  
  
 [!code-xaml[DataBindingLab#15](~/samples/snippets/csharp/VS_Snippets_Wpf/DataBindingLab/CSharp/DataBindingLabApp.xaml#15)]  
  
 最後に、次の構文を使用して、バインドでコンバーターを使用できます。 次の例では、<xref:System.Windows.Controls.TextBlock> のテキストコンテンツが、外部データソースのプロパティである*StartDate*にバインドされています。  
  
 [!code-xaml[DataBindingLab#17](~/samples/snippets/csharp/VS_Snippets_Wpf/DataBindingLab/CSharp/DataBindingLabApp.xaml#17)]  
  
 上の例で参照されているスタイルリソースは、このトピックに記載されていないリソースセクションで定義されています。  
  
## <a name="see-also"></a>関連項目

- [バインディングの検証の実装](how-to-implement-binding-validation.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
