---
title: DOM の属性へのアクセス
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: ce2df341-a1a4-4e97-8e1b-cd45b8e3e71e
ms.openlocfilehash: dd3292620cafc4e5d2494b3b5e18e04691910dc4
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75711195"
---
# <a name="accessing-attributes-in-the-dom"></a>DOM の属性へのアクセス

属性は要素のプロパティであり、要素の子ではありません。 この区別は、XML ドキュメント オブジェクト モデル (DOM) の兄弟ノード、親ノード、および子ノードの間の移動に使用するメソッドで重要な意味を持ちます。 たとえば、**PreviousSibling** メソッドと **NextSibling** メソッドは、要素から属性への移動や属性間の移動には使われません。 属性は要素のプロパティであり、要素によって所有されているため、**OwnerElement** プロパティを持ちますが、**parentNode** プロパティはありません。また、移動には専用のメソッドを使います。

現在のノードが要素のとき、その要素に関連付けられている属性があるかどうかを調べるには、**HasAttribute** メソッドを使用します。 要素に属性がある場合は、各種のメソッドで属性にアクセスできます。 **XmlElement** の **GetAttribute** メソッドと**GetAttributeNode** メソッドを使用して要素から 1 つの属性を取得するか、すべての属性をコレクションとして取得することができます。 コレクションに対して反復処理を行う必要がある場合は、コレクションを取得すると便利です。 要素のすべての属性が必要な場合は、要素の **Attributes** プロパティを使用して、すべての属性をコレクションに取得できます。

## <a name="retrieving-all-attributes-into-a-collection"></a>コレクションへのすべての属性の取得

要素ノードのすべての属性をコレクションに取得するには、**XmlElement.Attributes** プロパティを呼び出します。 **XmlElement.Attributes** プロパティにより、要素のすべての属性を含む XmlAttributeCollection を取得できます。 **XmlAttributeCollection** クラスは **XmlNamedNode** マップから継承されます。 このため、コレクションで使用できるメソッドとプロパティには、**ItemOf** プロパティや **Append** メソッドのような **XmlAttributeCollection** クラスに固有のメソッドやプロパティだけでなく、名前付きノード マップで使用できるメソッドとプロパティも含まれます。 属性コレクション内の各項目は、**XmlAttribute** ノードを表します。 要素の属性の数を調べるには、**XmlAttributeCollection** を取得し、**Count** プロパティを使用して、そのコレクションに含まれる **XmlAttribute** ノードの数を調べます。

属性コレクションを取得し、**Count** メソッドをループ インデックスとして使用して、コレクションに対して反復処理を行うコード サンプルを次に示します。 このコードは、その後、コレクションから属性を 1 つ取得して、その値を表示します。

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As XmlDocument = New XmlDocument()
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" & _
               "<title>The Handmaid's Tale</title>" & _
               "<price>14.95</price>" & _
               "</book>")

        ' Move to an element.
        Dim myElement As XmlElement = doc.DocumentElement

        ' Create an attribute collection from the element.
        Dim attrColl As XmlAttributeCollection = myElement.Attributes

        ' Show the collection by iterating over it.
        Console.WriteLine("Display all the attributes in the collection...")
        Dim i As Integer
        For i = 0 To attrColl.Count - 1
            Console.Write("{0} = ", attrColl.ItemOf(i).Name)
            Console.Write("{0}", attrColl.ItemOf(i).Value)
            Console.WriteLine()
        Next

        ' Retrieve a single attribute from the collection; specifically, the
        ' attribute with the name "misc".
        Dim attr As XmlAttribute = attrColl("misc")

        ' Retrieve the value from that attribute.
        Dim miscValue As String = attr.InnerXml

        Console.WriteLine("Display the attribute information.")
        Console.WriteLine(miscValue)

    End Sub
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;

public class Sample
{

    public static void Main()
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" +
                      "<title>The Handmaid's Tale</title>" +
                      "<price>14.95</price>" +
                      "</book>");

        // Move to an element.
        XmlElement myElement = doc.DocumentElement;

        // Create an attribute collection from the element.
        XmlAttributeCollection attrColl = myElement.Attributes;

        // Show the collection by iterating over it.
        Console.WriteLine("Display all the attributes in the collection...");
        for (int i = 0; i < attrColl.Count; i++)
        {
            Console.Write("{0} = ", attrColl[i].Name);
            Console.Write("{0}", attrColl[i].Value);
            Console.WriteLine();
        }

        // Retrieve a single attribute from the collection; specifically, the
        // attribute with the name "misc".
        XmlAttribute attr = attrColl["misc"];

        // Retrieve the value from that attribute.
        String miscValue = attr.InnerXml;

        Console.WriteLine("Display the attribute information.");
        Console.WriteLine(miscValue);

    }
}
```

この例を実行すると、次の出力が表示されます。

**出力**

コレクション内のすべての属性を表示します。

```console
genre = novel
ISBN = 1-861001-57-5
misc = sale item
Display the attribute information.
sale item
```

属性コレクションの情報は、名前またはインデックス番号によって取得できます。 上の例では、名前を指定してデータを取得しています。 次の例では、インデックス番号を指定してデータを取得します。

**XmlAttributeCollection** はコレクションであり、名前またはインデックスを使って反復処理ができるため、次の例では、ゼロベースのインデックスを使用してコレクションの最初の属性を選択しています。入力には、**baseuri.xml** というファイルを使用しています。

### <a name="input"></a>[入力]

```xml
<!-- XML fragment -->
<book genre="novel">
  <title>Pride And Prejudice</title>
</book>
```

```vb
Option Explicit On
Option Strict On

Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()
        ' Create the XmlDocument.
        Dim doc As New XmlDocument()
        doc.Load("http://localhost/baseuri.xml")

        ' Display information on the attribute node. The value
        ' returned for BaseURI is 'http://localhost/baseuri.xml'.
        Dim attr As XmlAttribute = doc.DocumentElement.Attributes(0)
        Console.WriteLine("Name of the attribute:  {0}", attr.Name)
        Console.WriteLine("Base URI of the attribute:  {0}", attr.BaseURI)
        Console.WriteLine("The value of the attribute:  {0}", attr.InnerText)
    End Sub 'Main
End Class 'Sample
```

```csharp
using System;
using System.IO;
using System.Xml;

public class Sample
{
  public static void Main()
  {
    // Create the XmlDocument.
    XmlDocument doc = new XmlDocument();

    doc.Load("http://localhost/baseuri.xml");

    // Display information on the attribute node. The value
    // returned for BaseURI is 'http://localhost/baseuri.xml'.
    XmlAttribute attr = doc.DocumentElement.Attributes[0];
    Console.WriteLine("Name of the attribute:  {0}", attr.Name);
    Console.WriteLine("Base URI of the attribute:  {0}", attr.BaseURI);
    Console.WriteLine("The value of the attribute:  {0}", attr.InnerText);
  }
}
```

## <a name="retrieving-an-individual-attribute-node"></a>個別の属性ノードの取得

要素から属性ノードを 1 つ取得するには、<xref:System.Xml.XmlElement.GetAttributeNode%2A?displayProperty=nameWithType> メソッドを使用します。 このメソッドは、**XmlAttribute** 型のオブジェクトを返します。 **XmlAttribute** の取得後は、<xref:System.Xml.XmlAttribute?displayProperty=nameWithType> クラスのすべてのメソッドとプロパティを使用できます。たとえば、**OwnerElement** で、どの要素に所属しているかを調べることができます。

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As XmlDocument = New XmlDocument()
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" & _
               "<title>The Handmaid's Tale</title>" & _
               "<price>14.95</price>" & _
               "</book>")

        ' Move to an element.
        Dim root As XmlElement
        root = doc.DocumentElement

        ' Get an attribute.
        Dim attr As XmlAttribute
        attr = root.GetAttributeNode("ISBN")

        ' Display the value of the attribute.
        Dim attrValue As String
        attrValue = attr.InnerXml
        Console.WriteLine(attrValue)

    End Sub
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;

 public class Sample
 {
      public static void Main()
      {
    XmlDocument doc = new XmlDocument();
     doc.LoadXml("<book genre='novel' ISBN='1-861003-78' misc='sale item'>" +
                   "<title>The Handmaid's Tale</title>" +
                   "<price>14.95</price>" +
                   "</book>");

    // Move to an element.
     XmlElement root = doc.DocumentElement;

    // Get an attribute.
     XmlAttribute attr = root.GetAttributeNode("ISBN");

    // Display the value of the attribute.
     String attrValue = attr.InnerXml;
     Console.WriteLine(attrValue);

    }
}
```

前の例で示したように、属性コレクションから 1 つの属性ノードを取得することもできます。 インデックス番号を指定して、XML ドキュメント ツリーのルートから 1 つの属性を取得する処理を 1 行で記述したコード サンプルを次に示します。**DocumentElement** プロパティを使用します。

```csharp
XmlAttribute attr = doc.DocumentElement.Attributes[0];
```

## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
