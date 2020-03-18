---
title: LINQ to XML と DOM
ms.date: 07/20/2015
ms.assetid: 18c36130-d598-40b7-9007-828232252978
ms.openlocfilehash: b25993fba4d878beb881dfdc46effe5a8ab3485b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331704"
---
# <a name="linq-to-xml-vs-dom-visual-basic"></a>LINQ to XML と DOM (Visual Basic)
このセクションでは、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] と、現在主流の XML プログラミング API である W3C ドキュメント オブジェクト モデル (DOM) との主な違いについて説明します。  
  
## <a name="new-ways-to-construct-xml-trees"></a>XML ツリーを構築するための新しい方法  
 W3C DOM では、XML ツリーをボトムアップ方式で作成します。つまり、ドキュメントを作成し、要素を作成して、要素をドキュメントに追加することによって作成します。  
  
 たとえば、Microsoft の DOM の実装である <xref:System.Xml.XmlDocument> を使用して XML ツリーを作成する場合、一般的な方法は次のようになります。  
  
```vb  
Dim doc As XmlDocument = New XmlDocument()  
Dim name As XmlElement = doc.CreateElement("Name")  
name.InnerText = "Patrick Hines"  
Dim phone1 As XmlElement = doc.CreateElement("Phone")  
phone1.SetAttribute("Type", "Home")  
phone1.InnerText = "206-555-0144"  
Dim phone2 As XmlElement = doc.CreateElement("Phone")  
phone2.SetAttribute("Type", "Work")  
phone2.InnerText = "425-555-0145"  
Dim street1 As XmlElement = doc.CreateElement("Street1")  
street1.InnerText = "123 Main St"  
Dim city As XmlElement = doc.CreateElement("City")  
city.InnerText = "Mercer Island"  
Dim state As XmlElement = doc.CreateElement("State")  
state.InnerText = "WA"  
Dim postal As XmlElement = doc.CreateElement("Postal")  
postal.InnerText = "68042"  
Dim address As XmlElement = doc.CreateElement("Address")  
address.AppendChild(street1)  
address.AppendChild(city)  
address.AppendChild(state)  
address.AppendChild(postal)  
Dim contact As XmlElement = doc.CreateElement("Contact")  
contact.AppendChild(name)  
contact.AppendChild(phone1)  
contact.AppendChild(phone2)  
contact.AppendChild(address)  
Dim contacts As XmlElement = doc.CreateElement("Contacts")  
contacts.AppendChild(contact)  
doc.AppendChild(contacts)  
Console.WriteLine(doc.OuterXml)  
```  
  
 このコーディング スタイルでは、XML ツリーの構造の多くを視覚的に認識できません。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、このような XML ツリーの構築方法をサポートしていますが、別の方法として*関数型構築*もサポートしています。 Visual Basic では、関数型構築は XML リテラルを使用して XML ツリーを構築します。  
  
 上記の例と同じ XML ツリーを [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の関数型構築を使用して構築すると、次のようになります。  
  
```vb  
Dim contacts = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone Type="Home">206-555-0144</Phone>  
            <Phone Type="Work">425-555-0145</Phone>  
            <Address>  
                <Street1>123 Main St</Street1>  
                <City>Mercer Island</City>  
                <State>WA</State>  
                <Postal>68042</Postal>  
            </Address>  
        </Contact>  
    </Contacts>  
```  
  
 このように、XML ツリーを構築するコードのインデントにより、基になる XML の構造が示されます。  
  
 詳細については、「 [XML ツリーの作成 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/creating-xml-trees.md)」を参照してください。  
  
## <a name="working-directly-with-xml-elements"></a>XML 要素の直接操作  
 一般に、XML によるプログラミングで重視されるのは XML 要素であり、その属性が重要となる場合が多くあります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、XML 要素と XML 属性を直接操作できます。 たとえば、次の操作を実行できます。  
  
- ドキュメント オブジェクトを一切使用せずに XML 要素を作成する。 これにより、XML ツリーのフラグメントを操作する際のプログラミングが単純化されます。  
  
- `T:System.Xml.Linq.XElement` オブジェクトを直接 XML ファイルから読み込む。  
  
- `T:System.Xml.Linq.XElement` オブジェクトをファイルやストリームにシリアル化する。  
  
 これに対して、XML ドキュメントが XML ツリーの論理的コンテナーとして使用される W3C DOM では、 XML ノード (要素や属性を含む) は XML ドキュメントのコンテキストで作成する必要があります。 DOM で name 要素を作成するコード フラグメントを以下に示します。  
  
```vb  
Dim doc As XmlDocument = New XmlDocument()  
Dim name As XmlElement = doc.CreateElement("Name")  
name.InnerText = "Patrick Hines"  
doc.AppendChild(name)  
```  
  
 要素を複数のドキュメントで使用する場合は、ノードをドキュメント間でインポートする必要があります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、こうした複雑さを回避できます。  
  
 LINQ to XML を使用する場合、<xref:System.Xml.Linq.XDocument> クラスを使用するのは、ドキュメントのルート レベルにコメントや処理命令を追加する場合だけです。  
  
## <a name="simplified-handling-of-names-and-namespaces"></a>名前と名前空間の処理の単純化  
 一般に XML プログラミングでは、名前、名前空間、および名前空間プレフィックスの処理が複雑になります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、名前空間プレフィックスを処理する必要がなくなるため、名前と名前空間が単純化されます。 必要に応じて名前空間プレフィックスを制御することはできますが、 明示的に制御しないようにすることもできます。その場合、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] がシリアル中に名前空間プレフィックスを必要に応じて割り当てます。必要ない場合は、既定の名前空間を使用してシリアル化します。 既定の名前空間が使用された場合は、結果のドキュメントには名前空間プレフィックスは含まれません。 詳細については、「[名前空間の概要」 (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)を参照してください。  
  
 DOM にはその他に、ノードの名前を変更できないという問題もあります。 ノード名の変更が必要な場合は、新しいノードを作成し、そこにすべての子ノードをコピーする必要があります。この場合、元のノード固有の特性は失われます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、この問題を回避するために、ノードに対して <xref:System.Xml.Linq.XName> プロパティを設定できます。  
  
## <a name="static-method-support-for-loading-xml"></a>XML を読み込むための静的メソッドのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、インスタンス メソッドの代わりに静的メソッドを使用して XML を読み込むことができます。 これにより、読み込みと解析が簡略化されます。 詳細については、「[方法: ファイルから XML を読み込む (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-load-xml-from-a-file.md)」を参照してください。  
  
## <a name="removal-of-support-for-dtd-constructs"></a>DTD の構成要素に関するサポートの削除  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、XML プログラミングのさらなる簡略化のために、エンティティとエンティティ参照のサポートが削除されています。 エンティティの管理は複雑で、ほとんど利用されていません。 これらのサポートを削除することにより、パフォーマンスが向上し、プログラミング インターフェイスが簡略化されます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] ツリーが設定されると、すべての DTD エンティティが展開されます。  
  
## <a name="support-for-fragments"></a>フラグメントのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] には、`XmlDocumentFragment` クラスに対応する要素はありません。 ただし、`XmlDocumentFragment` の概念は、多くの場合、<xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XNode> または <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> として型指定されたクエリの結果によって処理できます。  
  
## <a name="support-for-xpathnavigator"></a>XPathNavigator のサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、<xref:System.Xml.XPath.XPathNavigator> 名前空間の拡張メソッドによって <xref:System.Xml.XPath?displayProperty=nameWithType> がサポートされています。 詳細については、「 <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>」を参照してください。  
  
## <a name="support-for-white-space-and-indentation"></a>空白とインデントのサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、空白の処理が DOM より単純化されています。  
  
 一般的なシナリオでは、インデントされた XML を読み取り、メモリ内に空白のテキスト ノードなしで (つまり空白を維持せずに) XML ツリーを作成し、XML に対して何らかの操作を実行し、インデント付きで XML を保存します。 書式を設定して XML をシリアル化する場合は、XML ツリー内の有意の空白のみが維持されます。 これが [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の既定の動作です。  
  
 もう 1 つのよくあるシナリオは、意図的にインデントされた XML を読み取って変更する場合です。 場合によっては、このインデントを一切変更しないようにする必要があります。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] でこれを実現するには、XML を読み込む際または解析する際に空白を維持し、XML をシリアル化するときに書式設定を無効にします。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、空白は <xref:System.Xml.Linq.XText> ノードとして格納されます。DOM とは違って、特殊なノード型である <xref:System.Xml.XmlNodeType.Whitespace> は使用されません。  
  
## <a name="support-for-annotations"></a>注釈のサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 要素は、注釈の拡張可能なセットをサポートしています。 このサポートは、スキーマの情報、要素が UI にバインドされているかどうかの情報、またはアプリケーション固有のその他の情報など、要素に関するさまざまな情報を追跡する場合に利用できます。 詳細については、「[LINQ to XML 注釈](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-annotations.md)」を参照してください。  
  
## <a name="support-for-schema-information"></a>スキーマ情報のサポート  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、<xref:System.Xml.Schema?displayProperty=nameWithType> 名前空間の拡張メソッドによって XSD 検証がサポートされています。 これにより、XML ツリーが XSD に準拠しているかどうかを検証できます。 また、スキーマ検証後の情報セット (PSVI) を使用して XML ツリーを設定できます。 詳細については、「[方法: XSD を使用して検証する](../../../../visual-basic/programming-guide/concepts/linq/how-to-validate-using-xsd-linq-to-xml.md)」および「<xref:System.Xml.Schema.Extensions>」を参照してください。  
  
## <a name="see-also"></a>参照

- [はじめに (LINQ to XML)](../../../../visual-basic/programming-guide/concepts/linq/getting-started-linq-to-xml.md)
