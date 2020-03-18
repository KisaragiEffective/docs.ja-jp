---
title: XPath ユーザー向けの LINQ to XML
ms.date: 07/20/2015
ms.assetid: 0e64911c-a7cc-4c20-b927-ca99078b5656
ms.openlocfilehash: 4d5749e72acc8b051db2180b751051696ae04d57
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345465"
---
# <a name="linq-to-xml-for-xpath-users-visual-basic"></a>XPath ユーザーの LINQ to XML (Visual Basic)

次に示す一連のトピックでは、さまざまな XPath 式、およびそれらに関連する [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の式について説明します。  
  
 拡張メソッドによって [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] で使用可能になる <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType> の XPath 機能をすべての例で使用します。 各例では、XPath 式と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 式の両方を実行します。 次に、両方のクエリの結果を比較して、XPath 式と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] クエリが機能的に等価であることを検証します。 両方の種類のクエリは同じ XML ツリーからノードを返すため、クエリ結果は参照 ID を使用して比較されます。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
|トピック|説明|  
|-----------|-----------------|  
|[XPath と LINQ to XML の比較](../../../../csharp/programming-guide/concepts/linq/comparison-of-xpath-and-linq-to-xml.md)|XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の類似点および相違点についての概要を説明します。|  
|[方法: 子要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-a-child-element-xpath-linq-to-xml.md)|XPath の子要素軸と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の <xref:System.Xml.Linq.XContainer.Element%2A> メソッドを比較します。<br /><br /> 関連する XPath 式は `"DeliveryNotes"` です。|  
|[方法: 子要素の一覧を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-a-list-of-child-elements-xpath-linq-to-xml.md)|XPath の子要素軸と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の <xref:System.Xml.Linq.XContainer.Elements%2A> 軸を比較します。<br /><br /> 関連する XPath 式は `"./*"` です。|  
|[方法: ルート要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-the-root-element-xpath-linq-to-xml.md)|ルート要素を取得する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"/PurchaseOrders"` です。|  
|[方法: 子孫要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-descendant-elements-xpath-linq-to-xml.md)|特定の名前を持つ子孫要素を取得する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"//Name"` です。|  
|[方法: 属性をフィルター処理する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-filter-on-an-attribute-xpath-linq-to-xml.md)|指定した名前を持ち、かつ指定した値の属性を持つ子孫要素を取得する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"./Address[@Type='Shipping']"` です。|  
|[方法: 関連する要素 (XPath LINQ to XML) を検索する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-related-elements-xpath-linq-to-xml.md)|別の要素の値によって参照される属性に基づいて要素を取得する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"./Customer[@CustomerID=/Root/Orders/Order[12]/CustomerID]"` です。|  
|[方法: 名前空間内の要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-elements-in-a-namespace.md)|XML 名前空間の操作について、XPath の <xref:System.Xml.XmlNamespaceManager> クラスを使用する方法と、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の <xref:System.Xml.Linq.XName.Namespace%2A> クラスの <xref:System.Xml.Linq.XName> プロパティを使用する方法を比較します。<br /><br /> 関連する XPath 式は `"./aw:*"` です。|  
|[方法: 先行する兄弟を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-preceding-siblings-xpath-linq-to-xml.md)|XPath の `preceding-sibling` 軸と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の子 <xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A?displayProperty=nameWithType> 軸を比較します。<br /><br /> 関連する XPath 式は `"preceding-sibling::*"` です。|  
|[方法: 子要素の子孫を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-descendants-of-a-child-element-xpath-linq-to-xml.md)|特定の名前を持つ子要素の子孫要素を取得する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"./Paragraph//Text/text()"` です。|  
|[方法: 2 つのロケーションパスの和集合を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-a-union-of-two-location-paths-xpath.md)|XPath の UNION 演算子 <code>&#124;</code> と <xref:System.Linq.Enumerable.Concat%2A> の [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 標準クエリ演算子を比較します。<br /><br /> 関連する XPath 式は <code>"//Category&#124;//Price"</code> です。|  
|[方法: 兄弟ノード (XPath LINQ to XML) を検索する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-sibling-nodes-xpath-linq-to-xml.md)|特定の名前を持つノードのすべての兄弟を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"../Book"` です。|  
|[方法: 親の属性を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-an-attribute-of-the-parent-xpath-linq-to-xml.md)|親要素に移動してその属性を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"../@id"` です。|  
|[方法: 特定の名前を持つ兄弟の属性を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-attributes-of-siblings-with-a-specific-name.md)|コンテキスト ノードの兄弟に関する特定の属性を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"../Book/@id"` です。|  
|[方法: 特定の属性を持つ要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-elements-with-a-specific-attribute.md)|特定の属性を持つすべての要素を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"./*[@Select]"` です。|  
|[方法: 位置に基づいて子要素を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-child-elements-based-on-position.md)|相対的位置に基づいて要素を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"Test[position() >= 2 and position() <= 4]"` です。|  
|[方法: 直前の兄弟を検索する (XPath LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-find-the-immediate-preceding-sibling-xpath-linq-to-xml.md)|ノードの直前の兄弟を検索する方法を、XPath と [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の間で比較します。<br /><br /> 関連する XPath 式は `"preceding-sibling::*[1]"` です。|  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.XPath?displayProperty=nameWithType>
- [XML ツリーのクエリ (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/querying-xml-trees.md)
- [XPath データ モデルを使用した XML データの処理](../../../../standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
