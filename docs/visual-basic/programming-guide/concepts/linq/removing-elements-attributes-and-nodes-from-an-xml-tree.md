---
title: XML ツリーからの要素、属性、およびノードの削除
ms.date: 07/20/2015
ms.assetid: 5cf21919-4360-4b49-b29d-58ea3164ac72
ms.openlocfilehash: 4cce1eff469c1f737e18b88cce30155547d9f11b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348947"
---
# <a name="removing-elements-attributes-and-nodes-from-an-xml-tree-visual-basic"></a>XML ツリーからの要素、属性、およびノードの削除 (Visual Basic)
要素、属性、およびその他の種類のノードを削除して、XML ツリーを変更できます。  
  
 1 つの要素または 1 つの属性は XML ドキュメントから簡単に削除できます。 一方、要素または属性のコレクションを削除する場合は、まずコレクションをリストに具体化し、そのリストから要素または属性を削除する必要があります。 最適な方法は、これを自動的に行う <xref:System.Xml.Linq.Extensions.Remove%2A> 拡張メソッドを使用することです。  
  
 このような方法でコレクションの削除を実行する主な理由は、XML ツリーから取得するコレクションのほとんどが遅延実行を使用して生成されるからです。 最初にコレクションをリストに具体化せず、拡張メソッドも使用しない場合、特定の種類のバグが発生する可能性があります。 詳細については、「[宣言型コードと命令型コードの混在のバグ (LINQ to XML)」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/mixed-declarative-code-imperative-code-bugs-linq-to-xml.md)を参照してください。  
  
 ノードおよび属性を XML ツリーから削除するメソッドを次に示します。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XAttribute.Remove%2A?displayProperty=nameWithType>|<xref:System.Xml.Linq.XAttribute> をその親から削除します。|  
|<xref:System.Xml.Linq.XContainer.RemoveNodes%2A?displayProperty=nameWithType>|子ノードを <xref:System.Xml.Linq.XContainer> から削除します。|  
|<xref:System.Xml.Linq.XElement.RemoveAll%2A?displayProperty=nameWithType>|コンテンツおよび属性を <xref:System.Xml.Linq.XElement> から削除します。|  
|<xref:System.Xml.Linq.XElement.RemoveAttributes%2A?displayProperty=nameWithType>|<xref:System.Xml.Linq.XElement> の属性を削除します。|  
|<xref:System.Xml.Linq.XElement.SetAttributeValue%2A?displayProperty=nameWithType>|`null` を値に受け取ると、属性を削除します。|  
|<xref:System.Xml.Linq.XElement.SetElementValue%2A?displayProperty=nameWithType>|`null` を値に受け取ると、子要素を削除します。|  
|<xref:System.Xml.Linq.XNode.Remove%2A?displayProperty=nameWithType>|<xref:System.Xml.Linq.XNode> をその親から削除します。|  
|<xref:System.Xml.Linq.Extensions.Remove%2A?displayProperty=nameWithType>|ソース コレクション内のすべての属性または要素をその親要素から削除します。|  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 この例では、要素を削除する 3 つの方法を示します。 まず、1 つの要素を削除します。 次に、要素のコレクションを取得し、<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 演算子を使用して要素を具体化して、コレクションを削除します。 最後に、要素のコレクションを取得し、<xref:System.Xml.Linq.Extensions.Remove%2A> 拡張メソッドを使用して要素を削除します。  
  
 <xref:System.Linq.Enumerable.ToList%2A> 演算子の詳細については、「[データ型の変換 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/converting-data-types.md)」を参照してください。  
  
### <a name="code"></a>コード  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <Child1>  
            <GrandChild1/>  
            <GrandChild2/>  
            <GrandChild3/>  
        </Child1>  
        <Child2>  
            <GrandChild4/>  
            <GrandChild5/>  
            <GrandChild6/>  
        </Child2>  
        <Child3>  
            <GrandChild7/>  
            <GrandChild8/>  
            <GrandChild9/>  
        </Child3>  
    </Root>  
root.<Child1>.<GrandChild1>.Remove()  
root.<Child2>.Elements().ToList().Remove()  
root.<Child3>.Elements().Remove()  
Console.WriteLine(root)  
```  
  
### <a name="comments"></a>コメント  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <Child1>  
    <GrandChild2 />  
    <GrandChild3 />  
  </Child1>  
  <Child2 />  
  <Child3 />  
</Root>  
```  
  
 最初の孫要素が `Child1` から削除されていることがわかります。 すべての孫要素が、`Child2` と `Child3` から削除されています。  
  
## <a name="see-also"></a>関連項目

- [XML ツリーの変更 (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/modifying-xml-trees-linq-to-xml.md)
