---
title: XElement オブジェクトと XDocument オブジェクトの有効なコンテンツ
ms.date: 07/20/2015
ms.assetid: 0d253586-2b97-459f-b1a7-f30f38f3ed9f
ms.openlocfilehash: 1ad5b18e3bbc2143a56f9c8e7b34354761b4e42f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69590939"
---
# <a name="valid-content-of-xelement-and-xdocument-objects"></a>XElement オブジェクトと XDocument オブジェクトの有効なコンテンツ
ここでは、コンストラクターやメソッドに渡すことができる有効な引数について説明します。これらのコンストラクターやメソッドは、コンテンツを要素やドキュメントに追加するためのものです。  
  
## <a name="valid-content"></a>有効なコンテンツ  
 多くのクエリは、<xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> または <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XAttribute> に評価されます。 <xref:System.Xml.Linq.XElement> オブジェクトまたは <xref:System.Xml.Linq.XAttribute> オブジェクトのコレクションを <xref:System.Xml.Linq.XElement> コンストラクターに渡すことができます。 したがって、XML ツリーの設定に使用するメソッドとコンストラクターに対して、クエリの結果をコンテンツとして渡すと便利です。  
  
 単純コンテンツを追加するときに、さまざまな型をこのメソッドに渡すことができます。 有効な型は、次のとおりです。  
  
- <xref:System.String>  
  
- <xref:System.Double>  
  
- <xref:System.Single>  
  
- <xref:System.Decimal>  
  
- <xref:System.Boolean>  
  
- <xref:System.DateTime>  
  
- <xref:System.TimeSpan>  
  
- <xref:System.DateTimeOffset>  
  
- `Object.ToString` を実装する任意の型  
  
- <xref:System.Collections.Generic.IEnumerable%601> を実装する任意の型  
  
 複合コンテンツを追加するときは、次のような型をこのメソッドに渡すことができます。  
  
- <xref:System.Xml.Linq.XObject>  
  
- <xref:System.Xml.Linq.XNode>  
  
- <xref:System.Xml.Linq.XAttribute>  
  
- <xref:System.Collections.Generic.IEnumerable%601> を実装するすべての型  
  
 オブジェクトが <xref:System.Collections.Generic.IEnumerable%601> を実装している場合、オブジェクト内のコレクションが列挙され、コレクション内のすべての項目が追加されます。 コレクションに <xref:System.Xml.Linq.XNode> オブジェクトまたは <xref:System.Xml.Linq.XAttribute> オブジェクトが含まれている場合、コレクション内の各項目が個別に追加されます。 コレクションにテキスト (またはテキストに変換されるオブジェクト) が含まれている場合、コレクション内のテキストが連結され、1 つのテキスト ノードとして追加されます。  
  
 コンテンツが `null` の場合は、何も追加されません。 コレクションを渡すとき、コレクション内の項目は `null` にできます。 コレクション内の `null` 項目は、ツリーに影響を与えません。  
  
 追加する属性の名前は、そのコンテナー要素内で一意であることが必要です。  
  
 <xref:System.Xml.Linq.XNode> オブジェクトや <xref:System.Xml.Linq.XAttribute> オブジェクトを追加するとき、新しいコンテンツに親がない場合、単にオブジェクトが XML ツリーにアタッチされます。 新しいコンテンツに既に親があり、別の XML ツリーの一部となっている場合は、新しいコンテンツが複製され、新しく複製されたコンテンツが XML ツリーにアタッチされます。  
  
## <a name="valid-content-for-documents"></a>ドキュメントの有効なコンテンツ  
 属性と単純コンテンツをドキュメントに追加することはできません。  
  
 <xref:System.Xml.Linq.XDocument> の作成が必要となるシナリオは多くありません。 通常、その代わりに <xref:System.Xml.Linq.XElement> ルート ノードを使用して XML ツリーを作成できます。 ドキュメントを作成する要件 (処理命令とコメントを最上位に作成する必要がある、ドキュメント型をサポートする必要がある、など) が特にない限り、<xref:System.Xml.Linq.XElement> をルート ノードとして使用する方が便利です。  
  
 ドキュメントの有効なコンテンツは次のとおりです。  
  
- 0 個または 1 個の <xref:System.Xml.Linq.XDocumentType> オブジェクト。 ドキュメントの型は、要素の前に置く必要があります。  
  
- 0 個または 1 個の要素。  
  
- 0 個以上のコメント。  
  
- 0 個以上の処理命令。  
  
- 空白のみを含む、0 個以上のテキスト ノード。  
  
## <a name="constructors-and-functions-that-allow-adding-content"></a>コンテンツの追加が可能なコンストラクターと関数  
 次のメソッドを使用すると、<xref:System.Xml.Linq.XElement> または <xref:System.Xml.Linq.XDocument> に子コンテンツを追加できます。  
  
|方法|[説明]|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.%23ctor%2A>|<xref:System.Xml.Linq.XElement> を構築します。|  
|<xref:System.Xml.Linq.XDocument.%23ctor%2A>|<xref:System.Xml.Linq.XDocument> を構築します。|  
|<xref:System.Xml.Linq.XContainer.Add%2A>|<xref:System.Xml.Linq.XElement> または <xref:System.Xml.Linq.XDocument> の子コンテンツの末尾に追加します。|  
|<xref:System.Xml.Linq.XNode.AddAfterSelf%2A>|<xref:System.Xml.Linq.XNode> の後にコンテンツを追加します。|  
|<xref:System.Xml.Linq.XNode.AddBeforeSelf%2A>|<xref:System.Xml.Linq.XNode> の前にコンテンツを追加します。|  
|<xref:System.Xml.Linq.XContainer.AddFirst%2A>|<xref:System.Xml.Linq.XContainer> の子コンテンツの冒頭にコンテンツを追加します。|  
|<xref:System.Xml.Linq.XElement.ReplaceAll%2A>|<xref:System.Xml.Linq.XElement> のすべてのコンテンツ (子ノードおよび属性) を置き換えます。|  
|<xref:System.Xml.Linq.XElement.ReplaceAttributes%2A>|<xref:System.Xml.Linq.XElement> の属性を置き換えます。|  
|<xref:System.Xml.Linq.XContainer.ReplaceNodes%2A>|子ノードを新しいコンテンツに置き換えます。|  
|<xref:System.Xml.Linq.XNode.ReplaceWith%2A>|ノードを新しいコンテンツに置き換えます。|  
  
## <a name="see-also"></a>参照

- [XML ツリーの作成 (C#)](./linq-to-xml-overview.md)
