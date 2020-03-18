---
title: LINQ to XML 軸の概要
ms.date: 07/20/2015
ms.assetid: 9161f151-cfa8-4408-94ba-08a9ba3a486d
ms.openlocfilehash: 0cf3c20266d0ca9d861eec963afda8f2e71a55a3
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636485"
---
# <a name="linq-to-xml-axes-overview-visual-basic"></a>LINQ to XML 軸の概要 (Visual Basic)
XML ツリーを作成した後、または XML ドキュメントを XML ツリーに読み込んだ後は、クエリを実行して要素や属性を調べたり、その値を取得したりできます。 コレクションの取得には、*軸メソッド* (*軸*とも呼ぶ) を使用します。 一部の軸は、<xref:System.Xml.Linq.XElement> コレクションを返す、<xref:System.Xml.Linq.XDocument> クラスおよび <xref:System.Collections.Generic.IEnumerable%601> クラスのメソッドです。 一部の軸は、<xref:System.Xml.Linq.Extensions> クラスの拡張メソッドです。 拡張メソッドとして実装されている軸は、コレクションに対して機能し、コレクションを返します。  
  
 「[XElement クラスの概要](../../../../visual-basic/programming-guide/concepts/linq/xelement-class-overview.md)」で説明しているように、<xref:System.Xml.Linq.XElement> オブジェクトは単一の要素ノードを表します。 要素のコンテンツは、複合要素 (構造化コンテンツとも呼ばれる) または単純要素です。 単純要素は、空の場合と値を含む場合があります。 ノードに構造化コンテンツが含まれている場合、さまざまな軸メソッドを使用して子孫要素の列挙を取得できます。 最も一般的に使用される軸メソッドは、<xref:System.Xml.Linq.XContainer.Elements%2A> および <xref:System.Xml.Linq.XContainer.Descendants%2A> です。  
  
 コレクションを返す軸メソッド以外に、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] クエリでよく使用されるメソッドが 2 つあります。 <xref:System.Xml.Linq.XContainer.Element%2A> メソッドは、1 つの <xref:System.Xml.Linq.XElement> を返します。 <xref:System.Xml.Linq.XElement.Attribute%2A> メソッドは、1 つの <xref:System.Xml.Linq.XAttribute> を返します。  
  
 LINQ クエリは、多くの目的で、ツリーを調べてデータを抽出して変換するための最も強力な方法を提供します。 LINQ クエリは <xref:System.Collections.Generic.IEnumerable%601>を実装するオブジェクトに対して動作し、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 軸は <xref:System.Xml.Linq.XElement> コレクションの <xref:System.Collections.Generic.IEnumerable%601> と <xref:System.Collections.Generic.IEnumerable%601> コレクションの <xref:System.Xml.Linq.XAttribute> を返します。 クエリを実行するには、これらのコレクションが必要です。  
  
 要素と属性のコレクションを取得する軸メソッドに加えて、ツリーを詳細に反復処理するための軸メソッドもあります。 たとえば、要素と属性を処理する代わりに、ツリーのノードを操作できます。 ノードは、要素や属性よりも細かい粒度レベルです。 ノードの操作時には、XML コメント、テキスト ノード、処理命令などを調べることができます。 この機能は、たとえば、文書を XML として保存できるワード プロセッサを作成する場合に重要です。 ただし、大多数の XML プログラマが主に扱うのは、要素、属性、およびその値です。  
  
## <a name="methods-for-retrieving-a-collection-of-elements"></a>要素のコレクションを取得するメソッド  
 <xref:System.Xml.Linq.XElement> で呼び出して要素のコレクションを返す <xref:System.Xml.Linq.XElement> クラス (またはその基本クラス) のメソッドを、次にまとめて示します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XNode.Ancestors%2A?displayProperty=nameWithType>|この要素の祖先の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ祖先の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XContainer.Descendants%2A?displayProperty=nameWithType>|この要素の子孫の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ子孫の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>|この要素の子要素の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ子要素の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XNode.ElementsAfterSelf%2A?displayProperty=nameWithType>|この要素の後にある要素の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ、この要素の後にある要素の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A?displayProperty=nameWithType>|この要素の前にある要素の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ、この要素の前にある要素の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XElement.AncestorsAndSelf%2A?displayProperty=nameWithType>|この要素とその祖先の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ要素の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
|<xref:System.Xml.Linq.XElement.DescendantsAndSelf%2A?displayProperty=nameWithType>|この要素とその子孫の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement> を返します。 オーバーロードでは、指定された <xref:System.Collections.Generic.IEnumerable%601> を持つ要素の <xref:System.Xml.Linq.XElement> の <xref:System.Xml.Linq.XName> を返します。|  
  
## <a name="method-for-retrieving-a-single-element"></a>1 つの要素を取得するメソッド  
 次のメソッドは、<xref:System.Xml.Linq.XElement> オブジェクトから 1 つの子を取得します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XContainer.Element%2A?displayProperty=nameWithType>|指定された <xref:System.Xml.Linq.XElement> を持つ最初の子 <xref:System.Xml.Linq.XName> オブジェクトを返します。|  
  
## <a name="method-for-retrieving-a-collection-of-attributes"></a>属性のコレクションを取得するメソッド  
 次のメソッドは、<xref:System.Xml.Linq.XElement> オブジェクトから属性を取得します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.Attributes%2A?displayProperty=nameWithType>|すべての属性の <xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XAttribute> を返します。|  
  
## <a name="method-for-retrieving-a-single-attribute"></a>1 つの属性を取得するメソッド  
 次のメソッドは、<xref:System.Xml.Linq.XElement> オブジェクトから 1 つの属性を取得します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.Attribute%2A?displayProperty=nameWithType>|指定された <xref:System.Xml.Linq.XAttribute> を持つ <xref:System.Xml.Linq.XName> を返します。|  
  
## <a name="see-also"></a>関連項目

- [LINQ to XML 軸 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-axes.md)
