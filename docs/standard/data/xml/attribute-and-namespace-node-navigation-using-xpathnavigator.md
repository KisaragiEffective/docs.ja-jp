---
title: XPathNavigator を使用する属性と名前空間のナビゲーション
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 23975f88-e0af-4b88-93de-9e20e11880ad
ms.openlocfilehash: 6809a2a47a9ca25a16a9be75a0a8a8b03f98a21d
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75711156"
---
# <a name="attribute-and-namespace-node-navigation-using-xpathnavigator"></a>XPathNavigator を使用する属性と名前空間のナビゲーション
<xref:System.Xml.XPath.XPathNavigator> クラスは、2 セットの移動メソッドを提供します。「[XPathNavigator を使用するノード セットのナビゲーション](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)」に記載されている最初のセットは <xref:System.Xml.XPath.XPathDocument> または <xref:System.Xml.XmlDocument> オブジェクト内の*ノード セット*の移動に使用されます。 ここに記載されている 2 つ目のセットは、<xref:System.Xml.XPath.XPathDocument> または <xref:System.Xml.XmlDocument> オブジェクト内の*属性ノードと名前空間ノード*の移動に使用されます。  
  
## <a name="attribute-node-navigation"></a>属性ノードの移動  
 属性は要素のプロパティであり、要素の子ではありません。 兄弟ノード、親ノード、および子ノードの移動には <xref:System.Xml.XPath.XPathNavigator> クラスのメソッドが使用されるため、この区別が重要になります。  
  
 たとえば、<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> と <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> メソッドは、要素から属性への移動や属性間の移動には使われません。 代わりに、属性には異なる移動メソッドがあります。  
  
 以下は、<xref:System.Xml.XPath.XPathNavigator> クラスの属性移動メソッドです。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNextAttribute%2A>  
  
 現在のノードが要素のとき、その要素に関連付けられている属性があるかどうかを調べるには、<xref:System.Xml.XPath.XPathNavigator.HasAttributes%2A> プロパティを使用できます。 要素に属性がある場合は、各種のメソッドで属性にアクセスできます。 要素から 1 つの属性を取り出すには、<xref:System.Xml.XPath.XPathNavigator.GetAttribute%2A> メソッドを使用します。 <xref:System.Xml.XPath.XPathNavigator> を特定の属性に移動するには、<xref:System.Xml.XPath.XPathNavigator.MoveToAttribute%2A> メソッドを使用します。 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstAttribute%2A> メソッドに続けて複数の <xref:System.Xml.XPath.XPathNavigator.MoveToNextAttribute%2A> メソッド呼び出しを使用し、要素の各属性について繰り返すこともできます。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> オブジェクトが属性ノードまたは名前空間ノードの位置にある場合、<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>、および <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> メソッドは常に `false` を返し、<xref:System.Xml.XPath.XPathNavigator> の位置には影響を与えません。 この例外は、<xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>、および <xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A> メソッドです。  
  
## <a name="namespace-node-navigation"></a>名前空間ノードの移動  
 各要素は関連する名前空間ノードの集合を持ちます。名前空間ノードは、要素のスコープ内にある名前空間 URI に関連付けられた異なる名前空間プレフィックス (`http://www.w3.org/XML/1998/namespace` に関連付けられた XML プレフィックスを含みます。XML プレフィックスはすべての XML ドキュメントで暗黙に宣言されます) それぞれについて 1 つ、さらに要素のスコープ内に既定の名前空間がある場合は、それに 1 つあります。 要素はこれら名前空間ノードの親です。ただし、名前空間ノードはその親要素の子ではありません。  
  
 属性の場合と同様に、<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> および <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> メソッドは、要素から名前空間ノードへの移動や名前空間ノード間の移動には使われません。 代わりに、名前空間ノードには異なる移動メソッドがあります。  
  
 以下は、<xref:System.Xml.XPath.XPathNavigator> クラスの名前空間移動メソッドです。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNamespace%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A>  
  
 XML ドキュメント内のすべての要素には常に少なくとも 1 つの名前空間ノードがあります。 これは、プレフィックス `xml` および名前空間 URI `http://www.w3.org/XML/1998/namespace` を持つ名前空間ノードです。 特定のプレフィックスを指定してスコープ内の名前空間 URI を取り出すには、<xref:System.Xml.XPath.XPathNavigator.GetNamespace%2A> メソッドを使用します。 <xref:System.Xml.XPath.XPathNavigator> オブジェクトを特定の名前空間ノードに移動するには、<xref:System.Xml.XPath.XPathNavigator.MoveToNamespace%2A> メソッドを使用します。 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> メソッドに続けて複数の <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> メソッド呼び出しを使用し、要素のスコープ内の各名前空間ノードについて繰り返すこともできます。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> オブジェクトが属性ノードまたは名前空間ノードの位置にある場合、<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>、および <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> メソッドは常に `false` を返し、<xref:System.Xml.XPath.XPathNavigator> の位置には影響を与えません。 この例外は、<xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>、および <xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A> メソッドです。  
  
### <a name="the-xpathnamespacescope-enumeration"></a>XPathNamespaceScope 列挙体  
 名前空間ノードの移動時には、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> および <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> メソッドを <xref:System.Xml.XPath.XPathNamespaceScope> パラメーターを使用して呼び出すことができます。 これらのメソッドは、パラメーターなしで呼び出された場合と異なる動作をします。 <xref:System.Xml.XPath.XPathNamespaceScope> 列挙体には、<xref:System.Xml.XPath.XPathNamespaceScope.All>、<xref:System.Xml.XPath.XPathNamespaceScope.ExcludeXml>、または <xref:System.Xml.XPath.XPathNamespaceScope.Local> の値があります。  
  
 次の例は、XML ドキュメント内のさまざまなスコープで <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> および <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> メソッドによって返される値を表示します。  
  
```xml  
<root>  
    <element1 xmlns="http://www.contoso.com" xmlns:books="http://www.contoso.com/books">  
        <element2 />  
    </element1>  
</root>  
```  
  
 名前空間の順序 (<xref:System.Xml.XPath.XPathNavigator> メソッドとそれに続く一連の <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> メソッド呼び出し後に <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> が位置する名前空間) は次のとおりです。  
  
- `element2` の位置にある場合 : `xmlns:books="http://www.contoso.com/books"`、`xmlns="http://www.contoso.com"`、`xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- `element1` の位置にある場合 : `xmlns:books="http://www.contoso.com/books"`、`xmlns="http://www.contoso.com"`、`xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- `root` の位置にある場合 : `xmlns:xml="http://www.w3.org/XML/1998/namespace".`  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> クラスは、ドキュメントの逆順で名前空間ノードを返します。 したがって、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> は原則的に、現在のスコープ内の最後の名前空間ノードに移動します。  
  
 次の例は、XML ドキュメント内のさまざまなスコープで、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 列挙体が指定された <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> および <xref:System.Xml.XPath.XPathNamespaceScope> メソッドによって返される値を示します。  
  
```xml  
<root xmlns="http://www.contoso.com" xmlns:a="http://www.contoso.com/a" xmlns:b="http://www.contoso.com/b">  
    <child1 xmlns="" xmlns:a="urn:a">  
        <child2 xmlns:c="urn:c" />  
    </child1>  
</root>  
```  
  
 `child2` の位置にある場合、名前空間の順序 (<xref:System.Xml.XPath.XPathNavigator> メソッドとそれに続く一連の <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> メソッド呼び出し後に <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> が位置する名前空間) は次のとおりです。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.All>: `xmlns:c="urn:c"`、`xmlns:a="urn:a"`、`xmlns=""`、`xmlns:b="http://www.contoso.com/b"`、`xmlns:a="http://www.contoso.com/a"`、`xmlns="http://www.contoso.com"`、および `xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.ExcludeXml>: `xmlns:c="urn:c"`、`xmlns:a="urn:a"`、`xmlns=""`、`xmlns:b="http://www.contoso.com/b"`、`xmlns:a="http://www.contoso.com/a"`、および `xmlns="http://www.contoso.com"`。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.Local>: `xmlns:c="urn:c"`。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> クラスは、ドキュメントの逆順で名前空間ノードを返します。 したがって、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> は原則的に、現在のスコープ内の最後の名前空間ノードに移動します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用するノード セットのナビゲーション](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)
- [XpathNavigator を使用した XML データの抽出](../../../../docs/standard/data/xml/extract-xml-data-using-xpathnavigator.md)
- [厳密に型指定された XML データへの XPathNavigator を使用したアクセス](../../../../docs/standard/data/xml/accessing-strongly-typed-xml-data-using-xpathnavigator.md)
