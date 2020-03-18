---
title: LINQ to XML およびDOM (C#)
ms.date: 07/20/2015
ms.assetid: 51c0e3d2-c047-4e6a-a423-d61a882400b7
ms.openlocfilehash: 92d0da494829d57517d52fe93a3cbcf1398fdbe4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168390"
---
# <a name="linq-to-xml-vs-dom-c"></a>LINQ to XML およびDOM (C#)
このセクションでは、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] と、現在主流の XML プログラミング API である W3C ドキュメント オブジェクト モデル (DOM) との主な違いについて説明します。  
  
## <a name="new-ways-to-construct-xml-trees"></a>XML ツリーを構築するための新しい方法  
 W3C DOM では、XML ツリーをボトムアップ方式で作成します。つまり、ドキュメントを作成し、要素を作成して、要素をドキュメントに追加することによって作成します。  
  
 たとえば、Microsoft の DOM の実装である <xref:System.Xml.XmlDocument> を使用して XML ツリーを作成する場合、一般的な方法は次のようになります。  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlElement name = doc.CreateElement("Name");  
name.InnerText = "Patrick Hines";  
XmlElement phone1 = doc.CreateElement("Phone");  
phone1.SetAttribute("Type", "Home");  
phone1.InnerText = "206-555-0144";
XmlElement phone2 = doc.CreateElement("Phone");  
phone2.SetAttribute("Type", "Work");  
phone2.InnerText = "425-555-0145";
XmlElement street1 = doc.CreateElement("Street1");
street1.InnerText = "123 Main St";  
XmlElement city = doc.CreateElement("City");  
city.InnerText = "Mercer Island";  
XmlElement state = doc.CreateElement("State");  
state.InnerText = "WA";  
XmlElement postal = doc.CreateElement("Postal");  
postal.InnerText = "68042";  
XmlElement address = doc.CreateElement("Address");  
address.AppendChild(street1);  
address.AppendChild(city);  
address.AppendChild(state);  
address.AppendChild(postal);  
XmlElement contact = doc.CreateElement("Contact");  
contact.AppendChild(name);  
contact.AppendChild(phone1);  
contact.AppendChild(phone2);  
contact.AppendChild(address);  
XmlElement contacts = doc.CreateElement("Contacts");  
contacts.AppendChild(contact);  
doc.AppendChild(contacts);  
```  
  
 このコーディング スタイルでは、XML ツリーの構造の多くを視覚的に認識できません。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、このような XML ツリーの構築方法をサポートしていますが、別の方法として*関数型構築*もサポートしています。 関数型構築では、<xref:System.Xml.Linq.XElement> コンストラクターと <xref:System.Xml.Linq.XAttribute> コンストラクターを使用して XML ツリーを構築します。  
  
 上記の例と同じ XML ツリーを [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の関数型構築を使用して構築すると、次のようになります。  
  
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
  
 このように、XML ツリーを構築するコードのインデントにより、基になる XML の構造が示されます。  
  
 詳細については、「[XML ツリーの作成 (C#)](./linq-to-xml-overview.md)」を参照してください。  
  
## <a name="working-directly-with-xml-elements"></a>XML 要素の直接操作  
 一般に、XML によるプログラミングで重視されるのは XML 要素であり、その属性が重要となる場合が多くあります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、XML 要素と XML 属性を直接操作できます。 たとえば、次のようなことが可能です。  
  
- ドキュメント オブジェクトを一切使用せずに XML 要素を作成する。 これにより、XML ツリーのフラグメントを操作する際のプログラミングが単純化されます。  
  
- `T:System.Xml.Linq.XElement` オブジェクトを直接 XML ファイルから読み込む。  
  
- `T:System.Xml.Linq.XElement` オブジェクトをファイルやストリームにシリアル化する。  
  
 これに対して、XML ドキュメントが XML ツリーの論理的コンテナーとして使用される W3C DOM では、 XML ノード (要素や属性を含む) は XML ドキュメントのコンテキストで作成する必要があります。 DOM で name 要素を作成するコード フラグメントを以下に示します。  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlElement name = doc.CreateElement("Name");  
name.InnerText = "Patrick Hines";  
doc.AppendChild(name);  
```  
  
 要素を複数のドキュメントで使用する場合は、ノードをドキュメント間でインポートする必要があります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、こうした複雑さを回避できます。  
  
 LINQ to XML を使用する場合、<xref:System.Xml.Linq.XDocument> クラスを使用するのは、ドキュメントのルート レベルにコメントや処理命令を追加する場合だけです。  
  
## <a name="simplified-handling-of-names-and-namespaces"></a>名前と名前空間の処理の単純化  
 一般に XML プログラミングでは、名前、名前空間、および名前空間プレフィックスの処理が複雑になります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、名前空間プレフィックスを処理する必要がなくなるため、名前と名前空間が単純化されます。 必要に応じて名前空間プレフィックスを制御することはできますが、 明示的に制御しないようにすることもできます。その場合、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] がシリアル中に名前空間プレフィックスを必要に応じて割り当てます。必要ない場合は、既定の名前空間を使用してシリアル化します。 既定の名前空間が使用された場合は、結果のドキュメントには名前空間プレフィックスは含まれません。 詳細については、「[名前空間の概要 (LINQ to XML)](namespaces-overview-linq-to-xml.md)」を参照してください。  
  
 DOM にはその他に、ノードの名前を変更できないという問題もあります。 ノード名の変更が必要な場合は、新しいノードを作成し、そこにすべての子ノードをコピーする必要があります。この場合、元のノード固有の特性は失われます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、この問題を回避するために、ノードに対して <xref:System.Xml.Linq.XName> プロパティを設定できます。  
  
## <a name="static-method-support-for-loading-xml"></a>XML を読み込むための静的メソッドのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、インスタンス メソッドの代わりに静的メソッドを使用して XML を読み込むことができます。 これにより、読み込みと解析が簡略化されます。 詳細については、「[ファイルから XML を読み込む方法 (C#)](./how-to-load-xml-from-a-file.md)」を参照してください。  
  
## <a name="removal-of-support-for-dtd-constructs"></a>DTD の構成要素に関するサポートの削除  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、XML プログラミングのさらなる簡略化のために、エンティティとエンティティ参照のサポートが削除されています。 エンティティの管理は複雑で、ほとんど利用されていません。 これらのサポートを削除することにより、パフォーマンスが向上し、プログラミング インターフェイスが簡略化されます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] ツリーが設定されると、すべての DTD エンティティが展開されます。  
  
## <a name="support-for-fragments"></a>フラグメントのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] には、`XmlDocumentFragment` クラスに対応する要素はありません。 ただし、`XmlDocumentFragment` の概念は、多くの場合、<xref:System.Xml.Linq.XNode> の <xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Xml.Linq.XElement> の <xref:System.Collections.Generic.IEnumerable%601> として型指定されたクエリの結果によって処理できます。  
  
## <a name="support-for-xpathnavigator"></a>XPathNavigator のサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、<xref:System.Xml.XPath?displayProperty=nameWithType> 名前空間の拡張メソッドによって <xref:System.Xml.XPath.XPathNavigator> がサポートされています。 詳細については、「<xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>」を参照してください。  
  
## <a name="support-for-white-space-and-indentation"></a>空白とインデントのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、空白の処理が DOM より単純化されています。  
  
 一般的なシナリオでは、インデントされた XML を読み取り、メモリ内に空白のテキスト ノードなしで (つまり空白を維持せずに) XML ツリーを作成し、XML に対して何らかの操作を実行し、インデント付きで XML を保存します。 書式を設定して XML をシリアル化する場合は、XML ツリー内の有意の空白のみが維持されます。 これが [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の既定の動作です。  
  
 もう 1 つのよくあるシナリオは、意図的にインデントされた XML を読み取って変更する場合です。 場合によっては、このインデントを一切変更しないようにする必要があります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] でこれを実現するには、XML を読み込む際または解析する際に空白を維持し、XML をシリアル化するときに書式設定を無効にします。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、空白は <xref:System.Xml.Linq.XText> ノードとして格納されます。DOM とは違って、特殊なノード型である <xref:System.Xml.XmlNodeType.Whitespace> は使用されません。  
  
## <a name="support-for-annotations"></a>注釈のサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 要素は、注釈の拡張可能なセットをサポートしています。 このサポートは、スキーマの情報、要素が UI にバインドされているかどうかの情報、またはアプリケーション固有のその他の情報など、要素に関するさまざまな情報を追跡する場合に利用できます。 詳細については、「[LINQ to XML 注釈](./linq-to-xml-annotations.md)」を参照してください。  
  
## <a name="support-for-schema-information"></a>スキーマ情報のサポート  
[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、<xref:System.Xml.Schema?displayProperty=nameWithType> 名前空間の拡張メソッドによって XSD 検証がサポートされています。 これにより、XML ツリーが XSD に準拠しているかどうかを検証できます。 また、スキーマ検証後の情報セット (PSVI) を使用して XML ツリーを設定できます。 詳細については、「[XSD を使用して検証する方法](./how-to-validate-using-xsd-linq-to-xml.md)」および「<xref:System.Xml.Schema.Extensions>」を参照してください。
  
## <a name="see-also"></a>関連項目

- [はじめに (LINQ to XML)](./linq-to-xml-overview.md)
