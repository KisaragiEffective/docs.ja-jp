---
title: XPath 名前空間のナビゲーション
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 06cc7abb-7416-415c-9dd6-67751b8cabd5
ms.openlocfilehash: f35318b1439b762bf7c87cff217ed1787e8d007c
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156323"
---
# <a name="xpath-namespace-navigation"></a>XPath 名前空間のナビゲーション
XML ドキュメントで XPath クエリを使用するには、XML 名前空間およびそれらの名前空間に含まれる要素を、正しくアドレス指定する必要があります。 名前空間は、名前が複数のコンテキストで使用される場合に生じる可能性がある、あいまいさを防ぎます。たとえば、`ID` という名前は、XML ドキュメントの複数の異なる要素に関連付けられた複数の ID を参照する場合があります。 名前空間の構文では、XML ドキュメントの各要素を識別する、URI、名前、およびプレフィックスを指定します。  
  
 このトピックの例では、<xref:System.Xml.XPath.XPathNavigator> で XML ドキュメントをナビゲーションする際のプレフィックスの使用方法を示します。 名前空間と構文の詳細については、「 [Xml ファイル: Xml 名前空間](https://docs.microsoft.com/previous-versions/dotnet/articles/bb986013(v=msdn.10))について」を参照してください。  
  
## <a name="namespace-declarations"></a>名前空間の宣言  
 名前空間の宣言によって、<xref:System.Xml.XPath.XPathNavigator> のインスタンスを使用する際に、XML ドキュメントの各要素の識別とアドレス指定が可能になります。 名前空間のプレフィックスでは、アドレス名前空間用の短い構文が指定されます。  
  
 プレフィックスは、`<e:Envelope xmlns:e=http://schemas.xmlsoap.org/soap/envelope/>.` という形式で定義されます。この構文では、プレフィックス "`e`" が、名前空間の正式な URI の省略形になります。 `Body` という構文を使用して、`Envelope` 要素を `e:Body` 名前空間のメンバーとして識別できます。  
  
 次の XML ドキュメントは、次のセクションで示すナビゲーション例で `response.xml` として参照されます。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<e:Envelope xmlns:e="http://schemas.xmlsoap.org/soap/envelope/">  
  <e:Body>  
    <s:Search xmlns:s="http://schemas.microsoft.com/v1/Search">  
      <r:request xmlns:r="http://schemas.microsoft.com/v1/Search/metadata"
                 xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
      </r:request>  
    </s:Search>  
  </e:Body>  
</e:Envelope>  
```  
  
## <a name="navigation-by-namespace-prefix"></a>名前空間プレフィックスによるナビゲーション  
 このセクションで示すコードでは、<xref:System.Xml.XPath.XPathNavigator> オブジェクトと <xref:System.Xml.XmlNamespaceManager> オブジェクトを使用して、前のセクションで示した XML ドキュメントの `Search` 要素を選択します。 `xpath` クエリには、パス内の各要素の名前空間プレフィックスが含まれています。 各要素が含まれている名前空間の正確な ID を指定することによって、`Search` メソッドで <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> 要素に正しく移動できます。  
  
```csharp  
using (XmlReader reader = XmlReader.Create("response.xml"))  
{  
    XPathDocument doc = new XPathDocument(reader);  
    XPathNavigator nav = doc.CreateNavigator();
  
    XmlNamespaceManager nsmgr = new XmlNamespaceManager(nav.NameTable);  
    nsmgr.AddNamespace("e", @"http://schemas.xmlsoap.org/soap/envelope/");  
    nsmgr.AddNamespace("s", @"http://schemas.microsoft.com/v1/Search");  
    nsmgr.AddNamespace("r", @"http://schemas.microsoft.com/v1/Search/metadata");  
    nsmgr.AddNamespace("i", @"http://www.w3.org/2001/XMLSchema-instance");  
  
    string xpath = "/e:Envelope/e:Body/s:Search";  
  
    XPathNavigator element = nav.SelectSingleNode(xpath, nsmgr);  
  
    Console.WriteLine("Element Prefix:" + element.Prefix +
    " Local name:" + element.LocalName);  
    Console.WriteLine("Namespace URI: " + element.NamespaceURI);  
}  
```  
  
 名前空間で完全修飾された名前を使用するのは、利便性のためだけではありません。 前に示したドキュメント定義とコードでは、要素名を完全修飾せずにナビゲーションすると例外がスローされることがわかります。 たとえば、要素の定義が `<Search xmlns="http://schemas.microsoft.com/v1/Search">` で、クエリ文字列が `xpath = "/s:Envelope/s:Body/Search";` の場合、`Search` 要素に名前空間プレフィックスがないので、`null` 要素ではなく `Search` が返されます。  
  
## <a name="see-also"></a>参照

- [XPathNavigator による XML データへのアクセス](../../../../docs/standard/data/xml/accessing-xml-data-using-xpathnavigator.md)
- [XPathNavigator を使用した XML データの選択、評価、および照合](../../../../docs/standard/data/xml/selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
