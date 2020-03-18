---
title: XML ノードの種類
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 71d03b78-6898-4ce7-b0fc-1282573f31f7
ms.openlocfilehash: 83b8c09323e73a9b3ba7dea8d272d7d41d03add1
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710090"
---
# <a name="types-of-xml-nodes"></a>XML ノードの種類
XML ドキュメントがノードのツリーとしてメモリに読み込まれると、ノードの作成時にノード型が決まります。 XML ドキュメント オブジェクト モデル (DOM) では複数のノード型が定義されています。これらのノード型は W3C (World Wide Web Consortium) で規定されているもので、セクション 1.1.1「The DOM Structure Model」に記載されています。 ノード型、ノード型に割り当てられるオブジェクト、およびそれぞれの簡単な説明を次の表に示します。  
  
|DOM ノード型|オブジェクト|説明|  
|-------------------|------------|-----------------|  
|ドキュメント|<xref:System.Xml.XmlDocument>|ツリー内のすべてのノードのコンテナーです。 ドキュメントのルートとも呼ばれますが、常にルート要素と一致するとは限りません。|  
|DocumentFragment|<xref:System.Xml.XmlDocumentFragment>|1 つ以上のノードを非ツリー構造で格納する一時的なバッグです。|  
|DocumentType|<xref:System.Xml.XmlDocumentType>|`<!DOCTYPE…>` ノードを表します。|  
|EntityReference|<xref:System.Xml.XmlEntityReference>|展開されていないエンティティ参照テキストを表します。|  
|要素|<xref:System.Xml.XmlElement>|要素ノードを表します。|  
|Attr|<xref:System.Xml.XmlAttribute>|要素の属性です。|  
|ProcessingInstruction|<xref:System.Xml.XmlProcessingInstruction>|処理命令ノードです。|  
|コメント|<xref:System.Xml.XmlComment>|コメント ノードです。|  
|テキスト|<xref:System.Xml.XmlText>|要素または属性に含まれるテキストです。|  
|CDATASection|<xref:System.Xml.XmlCDataSection>|CDATA を表します。|  
|エンティティ|<xref:System.Xml.XmlEntity>|内部ドキュメント型定義 (DTD) のサブセットまたは外部 DTD とパラメーター エンティティから取得され、XML ドキュメントに含まれている `<!ENTITY…>` 宣言を表します。|  
|Notation|<xref:System.Xml.XmlNotation>|DTD で宣言された記法を表します。|  
  
 属性 (*attr*) は、W3C DOM Level 1 のセクション 1.2「Fundamental Interfaces」ではノードとして記載されていますが、どの要素ノードの子とも見なされません。  
  
 W3C では定義されていない追加のノード型を次の表に示します。ただし、これらのノード型は **XmlNodeType** 列挙値としてのみ Microsoft .NET Framework オブジェクト モデルで使用できます。 そのため、これらのノード型に対応する DOM ノード型の列はありません。  
  
|ノード型|説明|  
|---------------|-----------------|  
|<xref:System.Xml.XmlDeclaration>|宣言ノード `<?xml version="1.0"…>` を表します。|  
|<xref:System.Xml.XmlSignificantWhitespace>|有意の空白を表します。これは混合コンテンツの空白です。|  
|<xref:System.Xml.XmlWhitespace>|要素コンテンツ内の空白を表します。|  
|EndElement|**XmlReader** が要素の末尾に達したときに返されます。<br /><br /> XML サンプル: **\</item>**<br /><br /> 詳細については、「<xref:System.Xml.XmlNodeType>」を参照してください。|  
|EndEntity|<xref:System.Xml.XmlReader.ResolveEntity%2A> を呼び出した後、置換するエンティティの末尾に **XmlReader** が達したときに返されます。 詳細については、「<xref:System.Xml.XmlNodeType>」を参照してください。|  
  
 XML を読み込み、case 構成体を使用してノード型を判定し、ノードとその内容についての情報を出力するコード サンプルについては、「<xref:System.Xml.XmlSignificantWhitespace.NodeType%2A>」を参照してください。  
  
 ノード型のオブジェクト階層構造とそれぞれに対応するオブジェクト名の詳細については、「[XML ドキュメント オブジェクト モデル (DOM) の階層構造](../../../../docs/standard/data/xml/xml-document-object-model-dom-hierarchy.md)」を参照してください。 ノード ツリーに作成されるオブジェクトの詳細については、「[オブジェクト階層の XML データへのマップ](../../../../docs/standard/data/xml/mapping-the-object-hierarchy-to-xml-data.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
