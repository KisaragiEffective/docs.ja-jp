---
title: 親の属性を検索する方法 (XPath-LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: dbef9d89-a5c4-431f-80cc-7a2ebf323f86
ms.openlocfilehash: bfe7554a5c767adde5e7170c8e1ea0537155f6df
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141176"
---
# <a name="how-to-find-an-attribute-of-the-parent-xpath-linq-to-xml-c"></a>親の属性を検索する方法 (XPath-LINQ to XML) (C#)

このトピックでは、親要素に移動してその属性を検索する方法を示します。

XPath 式を次に示します。

`../@id`

## <a name="example"></a>例

この例では、まず `Author` 要素を検索します。 次に、親要素の `id` 属性を検索します。

この例では、「[サンプル XML ファイル: 書籍 (LINQ to XML)](./sample-xml-file-books-linq-to-xml.md)」の XML ドキュメントを使用します。

```csharp
XDocument books = XDocument.Load("Books.xml");

XElement author =
    books
    .Root
    .Element("Book")
    .Element("Author");

// LINQ to XML query
XAttribute att1 =
    author
    .Parent
    .Attribute("id");

// XPath expression
XAttribute att2 = ((IEnumerable)author.XPathEvaluate("../@id")).Cast<XAttribute>().First();

if (att1 == att2)
    Console.WriteLine("Results are identical");
else
    Console.WriteLine("Results differ");
Console.WriteLine(att1);
```

この例を実行すると、次の出力が生成されます。

```output
Results are identical
id="bk101"
```
