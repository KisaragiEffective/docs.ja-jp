---
title: XDocument クラスの概要 (C#)
ms.date: 07/20/2015
ms.assetid: 63305603-ab54-49fc-84e4-f76eecc59549
ms.openlocfilehash: de49dc071d22dd77dddea29ca114663261e3edda
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69590845"
---
# <a name="xdocument-class-overview-c"></a>XDocument クラスの概要 (C#)
このトピックでは、<xref:System.Xml.Linq.XDocument> クラスについて説明します。  
  
## <a name="overview-of-the-xdocument-class"></a>XDocument クラスの概要  
 <xref:System.Xml.Linq.XDocument> クラスには、有効な XML ドキュメントに必要な情報が含まれています。 これには、XML 宣言、処理命令、コメントが含まれます。  
  
 <xref:System.Xml.Linq.XDocument> クラスが提供する特定の機能が必要な場合は、<xref:System.Xml.Linq.XDocument> オブジェクトを作成するだけで済みます。 多くの場合、<xref:System.Xml.Linq.XElement> を直接操作できます。 <xref:System.Xml.Linq.XElement> を直接操作するのは、比較的単純なプログラミング モデルです。  
  
 <xref:System.Xml.Linq.XDocument> は <xref:System.Xml.Linq.XContainer> から派生しています。 したがって子ノードを含めることができます。 ただし、<xref:System.Xml.Linq.XDocument> オブジェクトに格納できる子 <xref:System.Xml.Linq.XElement> ノードは 1 つだけです。 これは、XML ドキュメントにルート要素を 1 つしか持てないという XML 標準を反映しています。  
  
## <a name="components-of-xdocument"></a>XDocument のコンポーネント  
 <xref:System.Xml.Linq.XDocument> には、次の要素を含めることができます。  
  
- 1 つの <xref:System.Xml.Linq.XDeclaration> オブジェクト。 <xref:System.Xml.Linq.XDeclaration> では、XML 宣言の関連部分である XML バージョン、ドキュメントのエンコード、および XML ドキュメントがスタンドアロンかどうかを指定できます。  
  
- 1 つの <xref:System.Xml.Linq.XElement> オブジェクト。 これは XML ドキュメントのルート ノードです。  
  
- 任意の数の <xref:System.Xml.Linq.XProcessingInstruction> オブジェクト。 処理命令は、XML を処理するアプリケーションに情報を伝達します。  
  
- 任意の数の <xref:System.Xml.Linq.XComment> オブジェクト。 コメントは、ルート要素の兄弟になります。 XML ドキュメントをコメントで始めることは無効であるため、<xref:System.Xml.Linq.XComment> オブジェクトをリストの最初の引数にすることはできません。  
  
- DTD 用の 1 つの <xref:System.Xml.Linq.XDocumentType>。  
  
 <xref:System.Xml.Linq.XDocument> をシリアル化すると、`XDocument.Declaration` が `null` である場合でも、作成者が `Writer.Settings.OmitXmlDeclaration` を `false` (既定値) に設定していれば、出力には XML 宣言が含まれます。  
  
 既定では、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] によってバージョンが "1.0" に、エンコードが "utf-8" に設定されます。  
  
## <a name="using-xelement-without-xdocument"></a>XDocument なしでの XElement の使用  
 既に説明したように、<xref:System.Xml.Linq.XElement> クラスは [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] プログラミング インターフェイスのメイン クラスです。 多くの場合、アプリケーションはドキュメントの作成を必要としません。 <xref:System.Xml.Linq.XElement> クラスを使用することで、XML ツリーを作成し、そのツリーに他の XML ツリーを追加し、その XML ツリーを変更し、さらにそのツリーを保存できます。  
  
## <a name="using-xdocument"></a>XDocument の使用  
 <xref:System.Xml.Linq.XDocument> を構築するには、<xref:System.Xml.Linq.XElement> オブジェクトを構築する場合と同様に関数型構築を使用します。  
  
 次のコードは、<xref:System.Xml.Linq.XDocument> オブジェクトおよび関連する格納されるオブジェクトを作成しています。  
  
```csharp  
XDocument d = new XDocument(  
    new XComment("This is a comment."),  
    new XProcessingInstruction("xml-stylesheet",  
        "href='mystyle.css' title='Compact' type='text/css'"),  
    new XElement("Pubs",  
        new XElement("Book",  
            new XElement("Title", "Artifacts of Roman Civilization"),  
            new XElement("Author", "Moreno, Jordao")  
        ),  
        new XElement("Book",  
            new XElement("Title", "Midieval Tools and Implements"),  
            new XElement("Author", "Gazit, Inbar")  
        )  
    ),  
    new XComment("This is another comment.")  
);  
d.Declaration = new XDeclaration("1.0", "utf-8", "true");  
Console.WriteLine(d);  
  
d.Save("test.xml");  
```  
  
 ファイル test.xml を調べると、次の出力が生成されます。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<!--This is a comment.-->  
<?xml-stylesheet href='mystyle.css' title='Compact' type='text/css'?>  
<Pubs>  
  <Book>  
    <Title>Artifacts of Roman Civilization</Title>  
    <Author>Moreno, Jordao</Author>  
  </Book>  
  <Book>  
    <Title>Midieval Tools and Implements</Title>  
    <Author>Gazit, Inbar</Author>  
  </Book>  
</Pubs>  
<!--This is another comment.-->  
```  
  
## <a name="see-also"></a>参照

- [LINQ to XML プログラミングの概要 (C#)](./linq-to-xml-overview.md)
