---
title: '方法 : XMLDataProvider と XPath クエリを使用して XML データにバインドする'
ms.date: 03/30/2017
helpviewer_keywords:
- XmlDataProvider [WPF], binding to XML data
- data binding [WPF], binding to XML data using XmlDataProvider queries
- binding [WPF], to XML data using XmlDataProvider queries
ms.assetid: 7dcd018f-16aa-4870-8e47-c1b4ea31e574
ms.openlocfilehash: a5ad7d8bce9bc0a760868e483278d1836f9472af
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559704"
---
# <a name="how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries"></a>方法 : XMLDataProvider と XPath クエリを使用して XML データにバインドする
この例では、<xref:System.Windows.Data.XmlDataProvider>を使用して XML データにバインドする方法を示します。  
  
 <xref:System.Windows.Data.XmlDataProvider>を使用すると、アプリケーションのデータバインディングを介してアクセスできる基になるデータを、XML ノードの任意のツリーにすることができます。 つまり、<xref:System.Windows.Data.XmlDataProvider> は、XML ノードの任意のツリーをバインディングソースとして使用する便利な方法を提供します。  
  
## <a name="example"></a>使用例  
 次の例では、データは <xref:System.Windows.FrameworkElement.Resources%2A> セクション内に XML*データアイランド*として直接埋め込まれています。 XML データアイランドは `<x:XData>` タグでラップする必要があり、常に1つのルートノードを持ちます。この例では、*インベントリ*です。  
  
> [!NOTE]
> XML データのルートノードには、XML 名前空間を空の文字列に設定する**xmlns**属性があります。 これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページ内にインラインで配置されるデータ アイランドに XPath クエリを適用する場合に必須です。 このインラインの場合、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]、つまりデータアイランドは、<xref:System.Windows> 名前空間を継承します。 このため、<xref:System.Windows> 名前空間で XPath クエリを修飾しないように、名前空間を空に設定する必要があります。これにより、クエリが誤って送信されます。  
  
 [!code-xaml[XMLDataSource#1](~/samples/snippets/csharp/VS_Snippets_Wpf/XmlDataSource/CS/Window1.xaml#1)]  
  
 この例に示すように、同じバインディング宣言を属性構文で作成するには、特殊文字を適切にエスケープする必要があります。 詳しくは、「[XML Character Entities and XAML](../../../desktop-wpf/xaml-services/xml-character-entities.md)」(XML 文字エンティティと XAML) をご覧ください。  
  
 この例を実行すると、<xref:System.Windows.Controls.ListBox> によって次の項目が表示されます。 これらは、*Books* の下のすべての要素のうち、*Stock* の値が "*out*" か、*Number* の値が 3 に等しいか 8 以上のものの *Title* です。 <xref:System.Windows.Data.XmlDataProvider> に設定された <xref:System.Windows.Data.XmlDataProvider.XPath%2A> 値によって、*書籍*の要素のみが公開されることが示されているため (基本的にフィルターを設定する)、 *CD*項目は返されません。  
  
 ![4冊の書籍のタイトルを示す XPath の例のスクリーンショット。](./media/how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries/xpath-example-listbox-details.png)  
  
 この例では、<xref:System.Windows.DataTemplate> 内の <xref:System.Windows.Controls.TextBlock> のバインドの <xref:System.Windows.Data.Binding.XPath%2A> が "*Title*" に設定されているため、書籍のタイトルが表示されます。 属性の値 ( *ISBN*など) を表示する場合は、その <xref:System.Windows.Data.Binding.XPath%2A> 値を "`@ISBN`" に設定します。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 内の **XPath** プロパティは、XmlNode.SelectNodes メソッドによって処理されます。 別の結果を得るために、**XPath** クエリを変更できます。 次に、前の例のバインドされた <xref:System.Windows.Controls.ListBox> に対する <xref:System.Windows.Data.Binding.XPath%2A> クエリの例をいくつか示します。  
  
- `XPath="Book[1]"` は、最初の書籍要素 ("XML in Action") を返します。 **XPath** のインデックスが 0 ではなく 1 から開始することにご注意ください。  
  
- `XPath="Book[@*]"` は、任意の属性を持つすべての書籍要素を返します。  
  
- `XPath="Book[last()-1]"` は、最後から 2 番目の書籍要素 ("Introducing Microsoft .NET") を返します。  
  
- `XPath="*[position()>3]"` は、最初の 3 つを除くすべての書籍要素を返します。  
  
 **XPath**クエリを実行すると、<xref:System.Xml.XmlNode> または XmlNodes の一覧が返されます。 <xref:System.Xml.XmlNode> は共通言語ランタイム (CLR) オブジェクトです。つまり、<xref:System.Windows.Data.Binding.Path%2A> プロパティを使用して、共通言語ランタイム (CLR) のプロパティにバインドできます。 前の例をもう一度考えてみます。 この例の残りの部分が変わらず、<xref:System.Windows.Controls.TextBlock> バインドを次のように変更すると、<xref:System.Windows.Controls.ListBox>に返された XmlNodes の名前が表示されます。 この場合、返されたノードの名前はすべて "*Book*" です。  
  
 [!code-xaml[XmlDataSourceVariation#XmlNodePath](~/samples/snippets/csharp/VS_Snippets_Wpf/XmlDataSourceVariation/CS/Page1.xaml#xmlnodepath)]  
  
 アプリケーションによっては、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページのソース内にデータアイランドとして XML を埋め込むことは、コンパイル時にデータの正確な内容を認識する必要があるため、不便な場合があります。 したがって、次の例に示すように、外部 XML ファイルからデータを取得することもできます。  
  
 [!code-xaml[XMLDataSource2#XmlFileExample](~/samples/snippets/csharp/VS_Snippets_Wpf/XmlDataSource2/CS/Window1.xaml#xmlfileexample)]  
  
 XML データがリモート XML ファイルに存在する場合は、次のように <xref:System.Windows.Data.XmlDataProvider.Source%2A> 属性に適切な URL を割り当てることによって、データへのアクセスを定義します。  
  
```xml  
<XmlDataProvider x:Key="BookData" Source="http://MyUrl" XPath="Books"/>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.ObjectDataProvider>
- [XDocument、XElement、または LINQ for XML クエリの結果にバインドする](how-to-bind-to-xdocument-xelement-or-linq-for-xml-query-results.md)
- [階層 XML データでマスター詳細パターンを使用する](how-to-use-the-master-detail-pattern-with-hierarchical-xml-data.md)
- [バインディング ソースの概要](binding-sources-overview.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法のトピック](data-binding-how-to-topics.md)
