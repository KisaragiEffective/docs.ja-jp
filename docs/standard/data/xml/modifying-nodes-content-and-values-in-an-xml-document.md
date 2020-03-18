---
title: XML ドキュメントのノード、コンテンツ、値の変更
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 761773e0-db72-4986-b9f5-a522213d8397
ms.openlocfilehash: 4a53ba4fe16a3653b1be380da49e6b75cb347a28
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710675"
---
# <a name="modifying-nodes-content-and-values-in-an-xml-document"></a>XML ドキュメントのノード、コンテンツ、値の変更
ドキュメントのノードおよびコンテンツを変更するには、さまざまな方法があります。 次の操作を行うことができます。  
  
- <xref:System.Xml.XmlNode.Value%2A> プロパティを使用してノードの値を変更する。  
  
- ノードを新しいノードに置き換えることにより、ノード セット全体を変更します。 これは <xref:System.Xml.XmlNode.InnerXml%2A> プロパティを使用して実行できます。  
  
- <xref:System.Xml.XmlNode.RemoveChild%2A> メソッドを使用して既存のノードを新しいノードに置き換える。  
  
- <xref:System.Xml.XmlCharacterData>、<xref:System.Xml.XmlCharacterData.AppendData%2A>、または <xref:System.Xml.XmlCharacterData.InsertData%2A> メソッドを使用して <xref:System.Xml.XmlCharacterData.ReplaceData%2A> クラスから継承した追加の文字をノードに追加する。  
  
- <xref:System.Xml.XmlCharacterData.DeleteData%2A> から継承したノード型に対し、<xref:System.Xml.XmlCharacterData> メソッドを使用することにより、文字範囲を削除してコンテンツを変更する。  
  
 ノードの値を簡単に変更するには、`node.Value = "new value";` を使用します。 この 1 行のコードを実行できるノード型と、そのノード型で変更されるデータを次の表に示します。  
  
|ノード型|変更されるデータ|  
|---------------|------------------|  
|属性|属性の値。|  
|CDATASection|CDATASection のコンテンツ。|  
|コメント|コメントの内容。|  
|ProcessingInstruction|ターゲットを除くコンテンツ。|  
|テキスト|テキストのコンテンツ。|  
|XmlDeclaration|`<?xml` と `?>` のマークアップを除く、宣言のコンテンツ。|  
|Whitespace|空白の値。 この値は、スペース、タブ、CR、LF という 4 つの認識可能 XML 空白文字のいずれかに設定できます。|  
|SignificantWhitespace|有意の空白の値。 この値は、スペース、タブ、CR、LF という 4 つの認識可能 XML 空白文字のいずれかに設定できます。|  
  
 この表に記載されていないノード型は、値を設定できるノード型ではありません。 これら以外のノード型に値を設定すると、<xref:System.InvalidOperationException> がスローされます。  
  
 <xref:System.Xml.XmlNode.InnerXml%2A> プロパティによって、現在のノードの子ノードのマークアップを変更できます。 このプロパティを設定すると、指定した文字列から解析されたコンテンツで子ノードが置き換えられます。 文字列の解析は、現在の名前空間コンテキストで実行されます。 さらに、<xref:System.Xml.XmlNode.InnerXml%2A> は冗長な名前空間宣言を削除します。 この結果、カット アンド ペースト操作を何度も実行しても、冗長な名前空間宣言によってドキュメント サイズが増加することはありません。 <xref:System.Xml.XmlNode.InnerXml%2A>の操作における名前空間の影響を示すコード サンプルについては、「<xref:System.Xml.XmlDocument.InnerXml%2A>」を参照してください。  
  
 <xref:System.Xml.XmlCharacterData.ReplaceData%2A> メソッドと <xref:System.Xml.XmlNode.RemoveChild%2A> メソッドを使用すると、これらのメソッドによって置換または削除されたノードが返されます。 このノードは XML ドキュメント オブジェクト モデル (DOM) のどこかに再度挿入することができます。 <xref:System.Xml.XmlCharacterData.ReplaceData%2A> メソッドは、ドキュメントに挿入されるノードに対し、2 種類の検証チェックを実行します。 最初のチェックでは、そのノードの親となるノードが、挿入されるノード型の子ノードを持つことができるかどうかを確認します。 2 番目のチェックでは、挿入されるノードが、その親となるノードの祖先でないことを確認します。 これらの条件のいずれかに違反すると、<xref:System.InvalidOperationException> がスローされます。  
  
 編集可能なノードに読み取り専用の子を追加したり、削除したりするのは有効な操作です。 しかし、読み取り専用のノードそのものを変更しようとすると、<xref:System.InvalidOperationException> がスローされます。 この一例として、<xref:System.Xml.XmlEntityReference> ノードの子を変更しようとした場合が当てはまります。 このノードの子は読み取り専用であり、変更できません。 それらを変更しようとすると、<xref:System.InvalidOperationException> がスローされます。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
