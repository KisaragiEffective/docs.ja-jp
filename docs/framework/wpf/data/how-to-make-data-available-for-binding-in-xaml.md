---
title: '方法 : XAML でデータをバインディング可能にする'
ms.date: 01/29/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], making data available for binding
- binding data [WPF], making data available for
ms.assetid: 7103c2e8-0e31-4a13-bf12-ca382221a8d5
ms.openlocfilehash: 97e878e4932ca9122bf27f76c32d1a56e69f253a
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73740608"
---
# <a name="how-to-make-data-available-for-binding-in-xaml"></a>方法 : XAML でデータをバインディング可能にする
このトピックでは、アプリケーションのニーズに応じて、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]でデータをバインドできるようにするさまざまな方法について説明します。  
  
## <a name="example"></a>例  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]からバインドする共通言語ランタイム (CLR) オブジェクトがある場合、オブジェクトをバインドに使用できるようにするには、そのオブジェクトをリソースとして定義し、それに `x:Key`を指定します。 次の例では、`PersonName`という名前の文字列プロパティを持つ `Person` オブジェクトがあります。 `Person` オブジェクト (`<src>` 要素を含む強調表示されている行) は、`SDKSample`という名前空間で定義されています。  
  
 [!code-xaml[SimpleBinding#Instantiation](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Page1.xaml?highlight=9,37)]  
  
 次に、`<TextBlock>` 要素を含む強調表示された行に表示されているように、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]内のオブジェクトに <xref:System.Windows.Controls.TextBlock> コントロールをバインドできます。 
  
 または、次の例のように <xref:System.Windows.Data.ObjectDataProvider> クラスを使用することもできます。  
  
 [!code-xaml[ObjectDataProvider}](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Page1.xaml?highlight=10-14,42)]  
  
 `<TextBlock>` 要素を含む強調表示された行に示すように、同じ方法でバインディングを定義します。  
  
 この例では、結果は同じです。テキストコンテンツ `Joe`の <xref:System.Windows.Controls.TextBlock> があります。 ただし、<xref:System.Windows.Data.ObjectDataProvider> クラスには、メソッドの結果にバインドする機能などの機能が用意されています。 提供される機能が必要な場合は、<xref:System.Windows.Data.ObjectDataProvider> クラスの使用を選択できます。  
  
 ただし、既に作成されているオブジェクトにバインドする場合は、次の例に示すように、コードで `DataContext` を設定する必要があります。  
  
 [!code-csharp[ADODataSet#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ADODataSet/CSharp/Window1.xaml.cs#1)]
 [!code-vb[ADODataSet#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ADODataSet/VisualBasic/Window1.xaml.vb#1)]  
  
 <xref:System.Windows.Data.XmlDataProvider> クラスを使用してバインドする XML データにアクセスする方法については、「 [XMLDataProvider と XPath クエリを使用して Xml データにバインド](how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md)する」を参照してください。 <xref:System.Windows.Data.ObjectDataProvider> クラスを使用してバインドする XML データにアクセスするには、「 [XDocument、XElement、または LINQ FOR xml のクエリ結果へのバインド](how-to-bind-to-xdocument-xelement-or-linq-for-xml-query-results.md)」を参照してください。  
  
 バインド先のデータを指定するさまざまな方法の詳細については、「[バインディングソースを指定](how-to-specify-the-binding-source.md)する」を参照してください。 バインドできるデータの種類や、独自の共通言語ランタイム (CLR) オブジェクトをバインド用に実装する方法の詳細については、「[バインディングソースの概要](binding-sources-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
