---
title: 静的にコンパイルされたクエリ (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 3f4825c7-c3b0-48da-ba4e-8e97fb2a2f34
ms.openlocfilehash: e9f56366f1566f831f1e0ea5bd5a06775d698c3d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350574"
---
# <a name="statically-compiled-queries-linq-to-xml-visual-basic"></a>静的にコンパイルされたクエリ (LINQ to XML) (Visual Basic)

<xref:System.Xml.XmlDocument> に対し、LINQ to XML で最も重要なパフォーマンスの利点の 1 つは、XPath のクエリは実行時に解釈する必要がある一方で LINQ to XML のクエリは静的にコンパイルされるという点です。 この機能は LINQ to XML に組み込まれているので、追加の手順を実行することなく利用できますが、その違いを理解しておくと、この 2 つの技術のどちらかを選ぶときに役立ちます。 このトピックでは、相違点について説明します。

## <a name="statically-compiled-queries-vs-xpath"></a>静的にコンパイルされたクエリと XPath

次の例は、指定した名前と指定した値の属性がある子孫要素を取得する方法を示しています。

次に示すのは、これに相当する XPath 式です。

```vb
//Address[@Type='Shipping']
```

```vb
Dim po = XDocument.Load("PurchaseOrders.xml")

Dim list1 = From el In po.Descendants("Address")
            Where el.@Type = "Shipping"

For Each el In list1
    Console.WriteLine(el)
Next
```

この例のクエリ式は、コンパイラによってメソッドベースのクエリ構文に書き換えられています。 メソッドベースのクエリ構文で書かれた次の例では、前の例と同じ結果が生成されます。

```vb
Dim po = XDocument.Load("PurchaseOrders.xml")

Dim list1 As IEnumerable(Of XElement) = po.Descendants("Address").Where(Function(el) el.@Type = "Shipping")

For Each el In list1
    Console.WriteLine(el)
Next
```

<xref:System.Linq.Enumerable.Where%2A> メソッドは拡張メソッドです。 詳細については、「[拡張メソッド](../../../../csharp/programming-guide/classes-and-structs/extension-methods.md)」を参照してください。 <xref:System.Linq.Enumerable.Where%2A> は拡張メソッドであるため、上記のクエリは次のように書かれたかのようにコンパイルされます。

```vb
Dim po = XDocument.Load("PurchaseOrders.xml")

Dim list1 = Enumerable.Where(po.Descendants("Address"), Function(el) el.@Type = "Shipping")

For Each el In list1
    Console.WriteLine(el)
Next
```

この例では、前の 2 つの例と同じ結果が生成されます。 これは、静的にリンクされたメソッド呼び出しにクエリが効果的にコンパイルされたことを示します。 これと反復子の遅延実行セマンティクスが組み合わさることで、パフォーマンスが向上します。 反復子の遅延実行セマンティクスの詳細については、「 [LINQ to XML での遅延実行とレイジー評価」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)を参照してください。

> [!NOTE]
> これらは、コンパイラが書き込むコードの例です。 実際の実装はこれらの例と若干異なる可能性がありますが、パフォーマンスは同じか類似したものになります。

## <a name="executing-xpath-expressions-with-xmldocument"></a>XmlDocument を使用した XPath 式の実行

次の例では、<xref:System.Xml.XmlDocument> を使用して前の例と同じ結果を達成します。

```vb
Dim reader = Xml.XmlReader.Create("PurchaseOrders.xml")
Dim doc As New Xml.XmlDocument()
doc.Load(reader)
Dim nl As Xml.XmlNodeList = doc.SelectNodes(".//Address[@Type='Shipping']")
For Each n As Xml.XmlNode In nl
    Console.WriteLine(n.OuterXml)
Next
reader.Close()
```

このクエリは、LINQ to XML を使用する例と同じ出力を返します。唯一の違いは、出力する XML が LINQ to XML ではインデントされるのに対し、<xref:System.Xml.XmlDocument> ではインデントされないという点です。

ただし、一般に <xref:System.Xml.XmlDocument> の方法では、<xref:System.Xml.XmlNode.SelectNodes%2A> メソッドが呼び出されるたびに内部で次の処理を行わなければならないため、LINQ to XML ほどのパフォーマンスは達成できません。

- XPath 式を含んでいる文字列を解析してトークンに分解します。

- トークンを検証して、XPath 式が有効であることを確認します。

- 式を内部式ツリーに変換します。

- ノードを反復処理し、式の評価に基づいて結果セットのノードを適切に選択します。

この場合、対応する LINQ to XML のクエリよりも処理量がかなり多くなります。 具体的なパフォーマンスの違いはクエリの種類によって異なりますが、一般に LINQ to XML のクエリは処理量が少ないため、<xref:System.Xml.XmlDocument> を使用して XPath 式を評価するよりも良いパフォーマンスが得られます。

## <a name="see-also"></a>関連項目

- [パフォーマンス (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/performance-linq-to-xml.md)
