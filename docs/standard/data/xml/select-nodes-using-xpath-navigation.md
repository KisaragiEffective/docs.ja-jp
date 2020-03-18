---
title: XPath ナビゲーションによるノードの選択
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 8e4450dc-56b3-472b-b467-32f5694f83ad
ms.openlocfilehash: 58f1487486c2802a2c64b51afcecb01c76dd291a
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155608"
---
# <a name="select-nodes-using-xpath-navigation"></a>XPath ナビゲーションによるノードの選択
XML ドキュメント オブジェクト モデル (DOM) には、DOM 内の情報を照会するための XPath (XML Path Language) ナビゲーションに使用できるメソッドが含まれています。 XPath を使用すると、特定の単一ノードを見つけたり、条件に一致するすべてのノードを検索したりできます。  
  
## <a name="xpath-select-methods"></a>XPath の選択メソッド  
 DOM クラスは、XPath による選択に、<xref:System.Xml.XmlNode.SelectSingleNode%2A> および <xref:System.Xml.XmlNode.SelectNodes%2A> という 2 つのメソッドを提供します。 <xref:System.Xml.XmlNode.SelectSingleNode%2A> メソッドは、選択基準に一致した最初のノードを返します。 <xref:System.Xml.XmlNode.SelectNodes%2A> メソッドは、一致したノードを含む <xref:System.Xml.XmlNodeList> を返します。  
  
 次の例では、<xref:System.Xml.XmlNode.SelectSingleNode%2A> メソッドを使用して、著者の姓が指定した条件と一致する最初の `book` ノードを選択しています。 bookstore.xml ファイル (このトピックの最後にあります) を入力ファイルとして使用します。  
  
```vb  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select and display the first node in which the author's
' last name is Kingsolver.  
Dim node As XmlNode = root.SelectSingleNode( _  
     "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr)  
Console.WriteLine(node.InnerXml)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select and display the first node in which the author's
// last name is Kingsolver.  
XmlNode node = root.SelectSingleNode(  
    "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr);  
Console.WriteLine(node.InnerXml);  
```  
  
 次の例では、<xref:System.Xml.XmlNode.SelectNodes%2A> メソッドを使用して、指定した値より高い価格の book ノードをすべて選択しています。 その後、選択されたリストに含まれる各書籍の価格を、プログラムで 10% 安くしています。 最後に、更新したファイルをコンソールに書き出します。 bookstore.xml ファイル (このトピックの最後にあります) を入力ファイルとして使用します。  
  
```vb  
' Load the document and set the root element.  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select all nodes where the book price is greater than 10.00.  
Dim nodeList As XmlNodeList = root.SelectNodes( _  
     "descendant::bk:book[bk:price>10.00]", nsmgr)  
For Each book As XmlNode In nodeList  
     Dim price As Double  
     price = Math.Round(Convert.ToSingle( _  
          book.LastChild.InnerText) * 0.9, 2)  
     book.LastChild.InnerText = price.ToString()  
Next  
  
' Display the updated document.  
doc.Save(Console.Out)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select all nodes where the book price is greater than 10.00.  
XmlNodeList nodeList = root.SelectNodes(  
     "descendant::bk:book[bk:price>10.00]", nsmgr);  
foreach (XmlNode book in nodeList)  
{  
     // Discount prices by 10%.  
     double price;  
     price = Math.Round(Convert.ToSingle(  
          book.LastChild.InnerText) * 0.9, 2);  
     book.LastChild.InnerText = price.ToString();  
}  
  
// Display the updated document.  
doc.Save(Console.Out);  
```  
  
 上の例で、XPath クエリはドキュメント要素から開始されます。 XPath クエリの始点を設定すると、コンテキスト ノードが設定され、XPath クエリの開始点になります。 ドキュメント要素からではなく、ドキュメント要素の最初の子から開始するには、次のように選択ステートメントを記述します。  
  
```vb  
doc.DocumentElement.FirstChild.SelectNodes(. . . )  
```  
  
```csharp  
this doc.DocumentElement.FirstChild.SelectNodes(. . .);  
```  
  
 すべての <xref:System.Xml.XmlNodeList> オブジェクトは、元のドキュメントに同期されます。 そのため、ノード リストを反復処理してノードの値を変更すると、そのノードは元のドキュメントにおいても更新されます。 前の例で、選択した <xref:System.Xml.XmlNodeList> でノードが変更されると、基になっているドキュメントも変更されることに注意してください。  
  
> [!NOTE]
> 元のドキュメントが変更された場合は、選択を再実行するのが適切です。 変更されたノードが、以前はノード リストに含まれておらず、ノード リストに追加されるものであったり、ノード リストから削除されるものであったりした場合は、ノード リストの正確さは保証されなくなります。  
  
## <a name="namespaces-in-xpath-expressions"></a>XPath 式の名前空間  
 XPath 式は名前空間を含むことができます。 名前空間の解決は <xref:System.Xml.XmlNamespaceManager> を使用してサポートされます。 XPath 式にプレフィックスが含まれる場合は、プレフィックスと名前空間 URI のペアを <xref:System.Xml.XmlNamespaceManager> に追加して、<xref:System.Xml.XmlNamespaceManager> を <xref:System.Xml.XmlNode.SelectNodes%28System.String%2CSystem.Xml.XmlNamespaceManager%29> メソッド、または <xref:System.Xml.XmlNode.SelectSingleNode%28System.String%2CSystem.Xml.XmlNamespaceManager%29> メソッドに渡す必要があります。 前のコード例では、<xref:System.Xml.XmlNamespaceManager> を使用して bookstore.xml ドキュメントの名前空間を解決しています。  
  
> [!NOTE]
> XPath 式にプレフィックスが含まれない場合は、名前空間 URI は空の名前空間であると仮定されます。 XML に既定の名前空間が含まれる場合でも、プレフィックスと名前空間 URI を <xref:System.Xml.XmlNamespaceManager> に追加する必要があります。そうしないと、ノードは選択されません。  
  
#### <a name="input-file"></a>入力ファイル  
 次に示すのは、このトピックの例で入力ファイルとして使用している bookstore.xml ファイルです。  
  
```xml  
<?xml version='1.0'?>  
<bookstore xmlns="urn:newbooks-schema">  
  <book genre="novel" style="hardcover">  
    <title>The Handmaid's Tale</title>  
    <author>  
      <first-name>Margaret</first-name>  
      <last-name>Atwood</last-name>  
    </author>  
    <price>19.95</price>  
  </book>  
  <book genre="novel" style="other">  
    <title>The Poisonwood Bible</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>11.99</price>  
  </book>  
  <book genre="novel" style="paperback">  
    <title>The Bean Trees</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>5.99</price>  
  </book>  
</bookstore>  
```  
  
## <a name="see-also"></a>参照

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
