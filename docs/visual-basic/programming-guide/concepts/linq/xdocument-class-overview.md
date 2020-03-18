---
title: XDocument クラスの概要
ms.date: 07/20/2015
ms.assetid: 45cb7e71-196a-47da-bfe9-7a5589db1eed
ms.openlocfilehash: cbc1ccca53978da07f31c0ba7e54eca9f06b0e72
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349290"
---
# <a name="xdocument-class-overview-visual-basic"></a>XDocument クラスの概要 (Visual Basic)
このトピックでは、<xref:System.Xml.Linq.XDocument> クラスについて説明します。  
  
## <a name="overview-of-the-xdocument-class"></a>XDocument クラスの概要  
 <xref:System.Xml.Linq.XDocument> クラスには、有効な XML ドキュメントに必要な情報が含まれています。 これには、XML 宣言、処理命令、コメントが含まれます。  
  
 <xref:System.Xml.Linq.XDocument> クラスが提供する特定の機能が必要な場合は、<xref:System.Xml.Linq.XDocument> オブジェクトを作成するだけで済みます。 多くの場合、<xref:System.Xml.Linq.XElement> を直接操作できます。 <xref:System.Xml.Linq.XElement> を直接操作するのは、比較的単純なプログラミング モデルです。  
  
 <xref:System.Xml.Linq.XDocument> は、<xref:System.Xml.Linq.XContainer> から派生します。 したがって子ノードを含めることができます。 ただし、<xref:System.Xml.Linq.XDocument> オブジェクトに格納できる子 <xref:System.Xml.Linq.XElement> ノードは 1 つだけです。 これは、XML ドキュメントにルート要素を 1 つしか持てないという XML 標準を反映しています。  
  
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
  
```vb  
Dim doc As XDocument = <?xml version="1.0" encoding="utf-8"?>  
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
doc.Save("test.xml")  
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

- [LINQ to XML プログラミングの概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-programming-overview.md)
