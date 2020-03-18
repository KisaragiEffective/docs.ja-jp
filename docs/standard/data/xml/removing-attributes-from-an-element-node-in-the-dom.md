---
title: DOM の要素ノードからの属性の削除
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 7ede6f9e-a3ac-49a4-8488-ab8360a44aa4
ms.openlocfilehash: bb18e712d5ed2cd06c7ae0e867b19ca8a9ad2513
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710350"
---
# <a name="removing-attributes-from-an-element-node-in-the-dom"></a>DOM の要素ノードからの属性の削除
属性を削除するには、さまざまな方法があります。 その 1 つとして、属性コレクションから属性を削除する方法があります。 属性コレクションから属性を削除するには、次の手順を実行します。  
  
1. `XmlAttributeCollection attrs = elem.Attributes;` を使用して要素から属性コレクションを取得します。  
  
2. 属性コレクションから属性を削除するには、次の 3 つのメソッドのいずれかを使用します。  
  
    - <xref:System.Xml.XmlAttributeCollection.Remove%2A> メソッドを使用して、特定の属性を削除する。  
  
    - <xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> メソッドを使用して、コレクションからずべての属性を削除し、属性のない要素を残す。  
  
    - <xref:System.Xml.XmlAttributeCollection.RemoveAt%2A> メソッドを使用して、コレクションのインデックス番号を使用して 1 つの属性を属性コレクションから削除する。  
  
 要素ノードから属性を削除するには、次のメソッドを使用します。  
  
- <xref:System.Xml.XmlElement.RemoveAllAttributes%2A> メソッドを使用して、属性のコレクションを削除する。  
  
- <xref:System.Xml.XmlElement.RemoveAttribute%2A> メソッドを使用して、名前を指定して単一の属性をコレクションから削除する。  
  
- <xref:System.Xml.XmlElement.RemoveAttributeAt%2A> メソッドを使用して、インデックス番号を指定して単一の属性をコレクションから削除する。  
  
 もう 1 つの方法として、要素を取得し、属性コレクションから属性を取得して、属性ノードを直接削除する方法もあります。 属性コレクションから属性を取得するには、名前 (`XmlAttribute attr = attrs["attr_name"];`)、インデックス (`XmlAttribute attr = attrs[0];`)、または名前空間を指定した完全修飾名 (`XmlAttribute attr = attrs["attr_localName", "attr_namespace"]`) を使用できます。  
  
 既定の属性としてドキュメント型定義 (DTD) に定義されている属性を削除するときは、使用するメソッドにかかわらず、特別な制限が適用されます。 既定の属性は、その属性が含まれている要素を削除しない限り削除できません。 既定の属性が宣言されている要素には、常に既定の属性が存在します。 <xref:System.Xml.XmlAttributeCollection> または <xref:System.Xml.XmlElement> から既定の属性を削除すると、代わりの属性が要素の <xref:System.Xml.XmlAttributeCollection> に挿入され、宣言されている既定値に初期化されます。 たとえば、`<book att1="1" att2="2" att3="3"></book>` として定義された要素がある場合、`book` 要素には、宣言された 3 つの既定の属性が設定されます。 XML ドキュメント オブジェクト モデル (DOM) の実装によって、この `book`要素が存在する限り、`att1`、`att2`、および `att3` という 3 つの既定の属性が存在することが保証されます。  
  
 <xref:System.Xml.XmlAttribute> を呼び出した場合、<xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> メソッドにより、属性の値が String.Empty に設定されます。これは、値のない属性が存在できないためです。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
