---
title: XML ツリーからの要素、属性、およびノードの削除 (C#)
ms.date: 07/20/2015
ms.assetid: 07dd06d6-1117-4077-bf98-9120cf51176e
ms.openlocfilehash: badaa6bab35367d62a73f56c5221cb7d6d4a45f7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69591259"
---
# <a name="removing-elements-attributes-and-nodes-from-an-xml-tree-c"></a>XML ツリーからの要素、属性、およびノードの削除 (C#)

要素、属性、およびその他の種類のノードを削除して、XML ツリーを変更できます。

1 つの要素または 1 つの属性は XML ドキュメントから簡単に削除できます。 一方、要素または属性のコレクションを削除する場合は、まずコレクションをリストに具体化し、そのリストから要素または属性を削除する必要があります。 最適な方法は、これを自動的に行う <xref:System.Xml.Linq.Extensions.Remove%2A> 拡張メソッドを使用することです。

このような方法でコレクションの削除を実行する主な理由は、XML ツリーから取得するコレクションのほとんどが遅延実行を使用して生成されるからです。 最初にコレクションをリストに具体化せず、拡張メソッドも使用しない場合、特定の種類のバグが発生する可能性があります。 詳細については、「[宣言型コードと命令型コードの混在のバグ (LINQ to XML) (C#)](./mixed-declarative-code-imperative-code-bugs-linq-to-xml.md)」を参照してください。

ノードおよび属性を XML ツリーから削除するメソッドを次に示します。

|方法|[説明]|
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

### <a name="description"></a>[説明]

この例では、要素を削除する 3 つの方法を示します。 まず、1 つの要素を削除します。 次に、要素のコレクションを取得し、<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 演算子を使用して要素を具体化して、コレクションを削除します。 最後に、要素のコレクションを取得し、<xref:System.Xml.Linq.Extensions.Remove%2A> 拡張メソッドを使用して要素を削除します。

<xref:System.Linq.Enumerable.ToList%2A> 演算子の詳細については、「[データ型の変換 (C#)](./converting-data-types.md)」を参照してください。

### <a name="code"></a>コード

```csharp
XElement root = XElement.Parse(@"<Root>
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
</Root>");
root.Element("Child1").Element("GrandChild1").Remove();
root.Element("Child2").Elements().ToList().Remove();
root.Element("Child3").Elements().Remove();
Console.WriteLine(root);
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
