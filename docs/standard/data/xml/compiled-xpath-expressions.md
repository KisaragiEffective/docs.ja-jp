---
title: コンパイルされた XPath 式
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: e25dd95f-b64c-4d8b-a3a4-379e1aa0ad55
ms.openlocfilehash: b4675765849299050eb6cddeaaa497bc6cdc620a
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75711104"
---
# <a name="compiled-xpath-expressions"></a>コンパイルされた XPath 式
<xref:System.Xml.XPath.XPathExpression> オブジェクトは、<xref:System.Xml.XPath.XPathExpression.Compile%2A> クラスの静的 <xref:System.Xml.XPath.XPathExpression> メソッドまたは <xref:System.Xml.XPath.XPathNavigator.Compile%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> メソッドから返されるコンパイル済み XPath クエリを表します。  
  
## <a name="the-xpathexpression-class"></a>XPathExpression クラス  
 <xref:System.Xml.XPath.XPathExpression> オブジェクトにより表されるコンパイル済み XPath クエリは、同じ XPath クエリが複数回使用されるときに有用です。  
  
 たとえば、<xref:System.Xml.XPath.XPathNavigator.Select%2A> メソッドを複数回呼び出すとき、毎回 XPath クエリを表す文字列を使用する代わりに、再使用とパフォーマンスの向上のために、<xref:System.Xml.XPath.XPathExpression.Compile%2A> クラスの <xref:System.Xml.XPath.XPathExpression> メソッド、または <xref:System.Xml.XPath.XPathNavigator.Compile%2A> クラスの <xref:System.Xml.XPath.XPathNavigator> メソッドを使用して、XPath クエリを <xref:System.Xml.XPath.XPathExpression> オブジェクトにコンパイルしてキャッシュします。  
  
 いったんコンパイルされると、<xref:System.Xml.XPath.XPathExpression> オブジェクトは、XPath クエリから返される型に応じて、次の <xref:System.Xml.XPath.XPathNavigator> クラス メソッドの入力として使用することができます。  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Matches%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
 次の表では、W3C XPath の戻り型、それに等価の Microsoft .NET Framework 型、および戻り型に応じて <xref:System.Xml.XPath.XPathExpression> オブジェクトで使用できるメソッドについて説明します。  
  
|W3C XPath の戻り値の型|.NET Framework の等価の型|説明|メソッド|  
|---------------------------|------------------------------------|-----------------|-------------|  
|`Node set`|<xref:System.Xml.XPath.XPathNodeIterator>|重複がなく順序付けされていない、ドキュメント順に作成されたノードのコレクション。|<xref:System.Xml.XPath.XPathNavigator.Select%2A> または <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`Boolean`|<xref:System.Boolean>|`true` または `false` の値。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A><br /><br /> <xref:System.Xml.XPath.XPathNavigator.Matches%2A>|  
|`Number`|<xref:System.Double>|浮動小数点数。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`String`|<xref:System.String>|UCS 文字のシーケンス。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator.Matches%2A> メソッドは、XPath 式をパラメーターとして受け取ります。 <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> メソッドは W3C XPath の戻り型の 1 つではなく、1 つの <xref:System.Xml.XPath.XPathNavigator> オブジェクトを返します。  
  
### <a name="the-returntype-property"></a>戻り型のプロパティ  
 XPath クエリが <xref:System.Xml.XPath.XPathExpression> オブジェクトにコンパイルされた後、<xref:System.Xml.XPath.XPathExpression.ReturnType%2A> オブジェクトの <xref:System.Xml.XPath.XPathExpression> プロパティを使用して、XPath クエリで何が返されるかを知ることができます。  
  
 <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> プロパティは、W3C XPath 戻り値を表す、次の <xref:System.Xml.XPath.XPathResultType> 列挙値の 1 つを返します。  
  
- <xref:System.Xml.XPath.XPathResultType.Any>  
  
- <xref:System.Xml.XPath.XPathResultType.Boolean>  
  
- <xref:System.Xml.XPath.XPathResultType.Error>  
  
- <xref:System.Xml.XPath.XPathResultType.Navigator>  
  
- <xref:System.Xml.XPath.XPathResultType.NodeSet>  
  
- <xref:System.Xml.XPath.XPathResultType.Number>  
  
- <xref:System.Xml.XPath.XPathResultType.String>  
  
 次の例は、<xref:System.Xml.XPath.XPathExpression> オブジェクトを使用して、`books.xml` ファイルから 1 つの数値とノード セットを返します。 各 <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> オブジェクトの <xref:System.Xml.XPath.XPathExpression> プロパティと共に、<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A> および <xref:System.Xml.XPath.XPathNavigator.Select%2A> メソッドからの結果がコンソールに出力されます。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Returns a number.  
Dim query1 As XPathExpression = navigator.Compile("bookstore/book/price/text()*10")  
Console.WriteLine(query1.ReturnType)  
  
Dim number As Double = CType(navigator.Evaluate(query1), Double)  
Console.WriteLine(number)  
  
' Returns a node set.  
Dim query2 As XPathExpression = navigator.Compile("bookstore/book/price")  
Console.WriteLine(query2.ReturnType)  
  
Dim nodes As XPathNodeIterator = navigator.Select(query2)  
nodes.MoveNext()  
Console.WriteLine(nodes.Current.Value)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Returns a number.  
XPathExpression query1 = navigator.Compile("bookstore/book/price/text()*10");  
Console.WriteLine(query1.ReturnType);  
  
Double number = (Double)navigator.Evaluate(query1);  
Console.WriteLine(number);  
  
// Returns a node set.  
XPathExpression query2 = navigator.Compile("bookstore/book/price");  
Console.WriteLine(query2.ReturnType);  
  
XPathNodeIterator nodes = navigator.Select(query2);  
nodes.MoveNext();  
Console.WriteLine(nodes.Current.Value);  
```  
  
 この例は、`books.xml` ファイルを入力として使用します。  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
### <a name="higher-performance-xpath-expressions"></a>XPath 式のパフォーマンスの向上  
 パフォーマンスを向上させるには、クエリで可能な限り特定した XPath 式を使用します。 たとえば、`book` が `bookstore` ノードの子ノードであり `bookstore` ノードが XML ドキュメントで最上位のノードの場合は、XPath 式 `/bookstore/book` を使用すると `//book` を使用した場合より高速です。 `//book` XPath 式は、XML ツリーのすべてのノードをスキャンしてノードとの一致を調べます。  
  
 さらに、選択基準が単純な場合は <xref:System.Xml.XPath.XPathNavigator> クラスが提供するノード セット ナビゲーション メソッドは、<xref:System.Xml.XPath.XPathNavigator> クラスが提供する選択メソッドに比べてパフォーマンスが高いことがあります。 たとえば、現在のノードの最初の子を選択する場合、<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A> メソッドを使用した方が `child::*[1]` XPath 式と <xref:System.Xml.XPath.XPathNavigator.Select%2A> メソッドを使用するよりも高速です。  
  
 <xref:System.Xml.XPath.XPathNavigator> クラスのノード セット ナビゲーション メソッドの詳細については「[XPathNavigator を使用するノード セットのナビゲーション](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用した XML データの選択](../../../../docs/standard/data/xml/select-xml-data-using-xpathnavigator.md)
- [XPathNavigator による XPath 式の評価](../../../../docs/standard/data/xml/evaluate-xpath-expressions-using-xpathnavigator.md)
- [XPathNavigator によるノードの一致](../../../../docs/standard/data/xml/matching-nodes-using-xpathnavigator.md)
- [XPath クエリで認識されるノード型](../../../../docs/standard/data/xml/node-types-recognized-with-xpath-queries.md)
- [XPath クエリおよび名前空間](../../../../docs/standard/data/xml/xpath-queries-and-namespaces.md)
