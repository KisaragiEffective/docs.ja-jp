---
title: LINQ to XML の概要 (C#)
ms.date: 10/30/2018
ms.assetid: 716b94d3-0091-4de1-8e05-41bc069fa9dd
ms.openlocfilehash: 334788a50832b8fe42ecc9a3272dd71f2f2af4ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168416"
---
# <a name="linq-to-xml-overview-c"></a>LINQ to XML の概要 (C#)

LINQ to XML には、.NET 統合言語クエリ (LINQ) フレームワークを利用したメモリ内 XML プログラミング インターフェイスが用意されています。 LINQ to XML では、.NET 機能を利用しており、更新および再設計されたドキュメント オブジェクト モデル (DOM) XML プログラミング インターフェイスと同等の機能を備えています。

XML は、多くのコンテキストでデータを書式設定する方法として広く採用されてきました。 たとえば、Web、構成ファイル、Microsoft Office Word ファイル、データベースで XML が使用されています。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、XML によるプログラミングのために再設計された最新の方法です。 ドキュメント オブジェクト モデル (DOM) のインメモリでのドキュメント変更機能を提供し、LINQ クエリ式をサポートします。 このクエリ式は、XPath と構文は異なりますが、機能が似ています。

## <a name="linq-to-xml-developers"></a>LINQ to XML の開発者

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、さまざまな開発者を対象としています。 何らかの処理を行うだけの平均的な開発者にとっては、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] によって SQL と同じようにクエリを作成できるので、XML の操作がより簡単になります。 プログラマは、短時間の学習で簡潔かつ強力なクエリを、選択したプログラミング言語で記述できるようになります。

熟練した開発者は、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用することで生産性を大きく高めることができます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、より少ないコードで、表現性と簡潔性に優れた強力なコードを記述できます。 また、同時に複数のデータ ドメインからクエリ式を使用できます。

## <a name="what-is-linq-to-xml"></a>LINQ to XML とは

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、.NET Framework プログラミング言語から XML を操作できるようにする、LINQ に対応したメモリ内 XML プログラミング インターフェイスです。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、XML ドキュメントをメモリに読み込むという点で、ドキュメント オブジェクト モデル (DOM) に似ています。 ドキュメントに対するクエリや変更を行うことができ、変更したドキュメントをファイルに保存したり、シリアル化してインターネット経由で送信したりできます。 ただし、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は DOM とは異なります。より軽量で使いやすく、C# の言語機能を利用する、新しいオブジェクト モデルが提供されています。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の最も重要な利点は、統合言語クエリ (LINQ) と統合されていることです。 この統合により、メモリ内の XML ドキュメントに対するクエリを記述して、要素および属性のコレクションを取得できます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] のクエリ機能は、構文は異なりますが、XPath および XQuery と機能面で互換性があります。 LINQ に C# が統合されていることで、厳密な型指定とコンパイル時のチェックが可能となり、デバッガー サポートが強化されます。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] のもう 1 つの利点は、クエリの結果を <xref:System.Xml.Linq.XElement> および <xref:System.Xml.Linq.XAttribute> オブジェクト コンストラクターに対するパラメーターとして使用できるので、XML ツリーを作成するための強力な方法が利用可能になります。 *関数型構築*と呼ばれるこの方法では、開発者が XML ツリーの構造を簡単に変換できます。

たとえば、次の記事で説明されているように、典型的な XML の購買発注書を使用することもできます: 「[サンプル XML ファイル:一般的な購買発注書 (LINQ to XML)](sample-xml-file-typical-purchase-order-linq-to-xml-1.md)」。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用することで、次のクエリを実行して購買発注書のすべての品目要素の部品番号属性を取得できます。

```csharp
// Load the XML file from our project directory containing the purchase orders
var filename = "PurchaseOrder.xml";
var currentDirectory = Directory.GetCurrentDirectory();
var purchaseOrderFilepath = Path.Combine(currentDirectory, filename);

XElement purchaseOrder = XElement.Load(purchaseOrderFilepath);

IEnumerable<string> partNos =  from item in purchaseOrder.Descendants("Item")
                               select (string) item.Attribute("PartNumber");
```

これは、メソッドの構文の形式で書き直すことができます。

```csharp
IEnumerable<string> partNos = purchaseOrder.Descendants("Item").Select(x => (string) x.Attribute("PartNumber"));
```

もう 1 つの例として、金額が $100 を超える品目を部品番号順に並べた一覧が必要であるとします。 この情報を取得するには、次のクエリを実行します。

```csharp
// Load the XML file from our project directory containing the purchase orders
var filename = "PurchaseOrder.xml";
var currentDirectory = Directory.GetCurrentDirectory();
var purchaseOrderFilepath = Path.Combine(currentDirectory, filename);

XElement purchaseOrder = XElement.Load(purchaseOrderFilepath);

IEnumerable<XElement> pricesByPartNos =  from item in purchaseOrder.Descendants("Item")
                                 where (int) item.Element("Quantity") * (decimal) item.Element("USPrice") > 100
                                 orderby (string)item.Element("PartNumber")
                                 select item;
```

これも、メソッドの構文の形式で書き直すことができます。

```csharp
IEnumerable<XElement> pricesByPartNos = purchaseOrder.Descendants("Item")
                                        .Where(item => (int)item.Element("Quantity") * (decimal)item.Element("USPrice") > 100)
                                        .OrderBy(order => order.Element("PartNumber"));
```

これらの LINQ 機能に加え、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では XML プログラミング インターフェイスが機能強化されています。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、次のことを実行できます。

- [ファイル](how-to-load-xml-from-a-file.md)または[ストリーム](how-to-stream-xml-fragments-from-an-xmlreader.md)からの XML の読み込み

- ファイルまたはストリームへの XML のシリアル化

- 関数型構築を使用した XML の新規作成

- XPath に類似した軸を使用した XML に対するクエリの実行

- <xref:System.Xml.Linq.XContainer.Add%2A>、<xref:System.Xml.Linq.XNode.Remove%2A>、<xref:System.Xml.Linq.XNode.ReplaceWith%2A>、<xref:System.Xml.Linq.XElement.SetValue%2A> などのメソッドを使用した、メモリ内の XML ツリーの操作

- XSD を使用した XML ツリーの検証

- 上記の機能を組み合わせて使用した XML ツリーの構造の変換

## <a name="creating-xml-trees"></a>XML ツリーの作成

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] でのプログラミングで最も重要な利点の 1 つは、XML ツリーを簡単に作成できるという点です。 たとえば、小さな XML ツリーを作成するには、次のようにコードを記述します。

```csharp
XElement contacts =
new XElement("Contacts",
    new XElement("Contact",
        new XElement("Name", "Patrick Hines"),
        new XElement("Phone", "206-555-0144",
            new XAttribute("Type", "Home")),
        new XElement("phone", "425-555-0145",
            new XAttribute("Type", "Work")),
        new XElement("Address",
            new XElement("Street1", "123 Main St"),
            new XElement("City", "Mercer Island"),
            new XElement("State", "WA"),
            new XElement("Postal", "68042")
        )
    )
);
```

詳しくは、「[XML ツリーの作成 (C#)](./creating-xml-trees-linq-to-xml-2.md)」をご覧ください。

## <a name="see-also"></a>関連項目

- [リファレンス (LINQ to XML)](./reference-linq-to-xml.md)
- [LINQ to XML とDOM (C#)](./linq-to-xml-vs-dom.md)
- [LINQ to XML とその他の XML テクノロジ](./linq-to-xml-vs-other-xml-technologies.md)
- <xref:System.Xml.Linq>
