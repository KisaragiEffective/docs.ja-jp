---
title: DOM の要素に対する新しい属性の作成
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: dd6dc920-b011-418a-b3db-f1580a7d9251
ms.openlocfilehash: 79a3390933256ed862d35c90db0aab2177cdfc41
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75711013"
---
# <a name="creating-new-attributes-for-elements-in-the-dom"></a>DOM の要素に対する新しい属性の作成

属性はノードではなく、要素ノードのプロパティであり、要素に関連付けられた **XmlAttributeCollection** に格納されるため、新しい属性を作成する方法は、他のノード型を作成する方法と異なります。 属性を作成して要素に追加するには、次の 2 つの方法があります。

- 要素ノードを取得し、**SetAttribute** を使用してその要素の属性コレクションに属性を追加します。

- **CreateAttribute** メソッドを使用して **XmlAttribute** ノードを作成します。要素ノードを取得し、**SetAttributeNode** を使用してその要素の属性コレクションにノードを追加します。

次の例は、 **SetAttribute**メソッドを使用して要素に属性を追加する方法を示しています。

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As New XmlDocument()
        doc.LoadXml("<book xmlns:bk='urn:samples' bk:ISBN='1-861001-57-5'>" & _
                    "<title>Pride And Prejudice</title>" & _
                    "</book>")
        Dim root As XmlElement = doc.DocumentElement

        ' Add a new attribute.
        root.SetAttribute("genre", "urn:samples", "novel")

        Console.WriteLine("Display the modified XML...")
        Console.WriteLine(doc.InnerXml)
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
        var doc = new XmlDocument();
        doc.LoadXml("<book xmlns:bk='urn:samples' bk:ISBN='1-861001-57-5'>" +
                    "<title>Pride And Prejudice</title>" +
                    "</book>");
        XmlElement root = doc.DocumentElement;

        // Add a new attribute.
        root.SetAttribute("genre", "urn:samples", "novel");

        Console.WriteLine("Display the modified XML...");
        Console.WriteLine(doc.InnerXml);
    }
}
```

**CreateAttribute** メソッドを使用して新しい属性を作成する例を次に示します。 属性の作成後、**SetAttributeNode** メソッドを使用して、その属性を **book** 要素の属性コレクションに追加します。

次の XML を使用します。
  
```xml
<book genre='novel' ISBN='1-861001-57-5'>
<title>Pride And Prejudice</title>
</book>
```

新しい属性を作成し、その属性の値を設定します。

```vb
Dim attr As XmlAttribute = doc.CreateAttribute("publisher")
attr.Value = "WorldWide Publishing"
```

```csharp
XmlAttribute attr = doc.CreateAttribute("publisher");
attr.Value = "WorldWide Publishing";
```

作成した属性を要素に追加します。

```vb
doc.DocumentElement.SetAttributeNode(attr)
```

```csharp
doc.DocumentElement.SetAttributeNode(attr);
```

**出力**

```xml
<book genre="novel" ISBN="1-861001-57-5" publisher="WorldWide Publishing">
<title>Pride And Prejudice</title>
</book>
```

完全なコード サンプルは <xref:System.Xml.XmlDocument.CreateAttribute%2A> にあります。

**XmlAttribute** ノードを作成した後、**InsertBefore** メソッドまたは **InsertAfter** メソッドを使用して、コレクション内の適切な位置にそのノードを配置することもできます。 同じ名前の属性が属性コレクションに既に含まれている場合、既存の **XmlAttribute** ノードはコレクションから削除され、新しい **XmlAttribute** ノードが挿入されます。 これは、**SetAttribute** メソッドで行われる処理と同じです。 これらのメソッドは、**InsertBefore** や **InsertAfter** を実行するときに、参照ポイントとなる既存のノードをパラメーターとして受け取ります。 新しいノードの挿入位置を示す参照ノードが指定されていないと、**InsertAfter** メソッドは、既定でコレクションの先頭に新しいノードを挿入します。 参照ノードが指定されなかった場合の **InsertBefore** の既定の挿入位置は、コレクションの末尾です。

属性の**Xmlnamednodemap**を作成した場合は、<xref:System.Xml.XmlNamedNodeMap.SetNamedItem%2A> メソッドを使用して、名前を指定して属性を追加できます。 詳細については、「[NamedNodeMaps と NodeLists のノード コレクション](node-collections-in-namednodemaps-and-nodelists.md)」を参照してください。

## <a name="default-attributes"></a>既定の属性

既定の属性を持つと宣言された要素を作成すると、既定値を持つ新しい既定の属性が XML ドキュメント オブジェクト モデル (DOM) によって作成され、その要素に割り当てられます。 既定の属性の子ノードも同時に作成されます。

## <a name="attribute-child-nodes"></a>属性子ノード

属性ノードの値は、その属性の子ノードになります。 有効な子ノードには、 **XmlText**ノードと**xmlentityreference**ノードの2種類しかありません。 これらのノードは、**FirstChild** や **LastChild** のようなメソッドが子ノードとして処理するという意味で、子ノードと見なされます。 属性が子ノードを持つという点は、属性または属性の子ノードを削除するときに重要になります。 詳細については、「[DOM の要素ノードからの属性の削除](removing-attributes-from-an-element-node-in-the-dom.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](xml-document-object-model-dom.md)
