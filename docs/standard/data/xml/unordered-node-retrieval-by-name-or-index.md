---
title: 名前またはインデックスによる順序付けられていないノードの取得
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 2038a90b-92af-4a0a-baaa-08e688d95194
ms.openlocfilehash: 55ea0e31bb8a2863dc0e0eb30f6ca5700c3110b8
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155738"
---
# <a name="unordered-node-retrieval-by-name-or-index"></a>名前またはインデックスによる順序付けられていないノードの取得
W3C (World Wide Web Consortium) 仕様で NamedNodeMap として定義されている **XmlNamedNodeMap** は、名前またはインデックスでノードを参照する機能を持っており、順序付けられていないノード セットを処理する必要があります。 **XmlNamedNodeMap** にアクセスできるのは、メソッドまたはプロパティから **XmlNamedNodeMap** が返されたときだけです。 **XmlNamedNodeMap** を返すメソッドやプロパティには、次の 3 つがあります。  
  
- XmlElement.Attributes  
  
- XmlDocumentType.Entities  
  
- XmlDocumentType.Notations  
  
 たとえば、**XmlDocumentType.Entities** プロパティは、ドキュメント型宣言で宣言された **XmlEntity** ノードのコレクションを取得します。 このコレクションは **XmlNamedNodeMap** として返され、**Count** プロパティを使用してコレクションを反復処理し、エンティティ情報を表示できます。 **XmlNamedNodeMap** の反復処理の例については、「<xref:System.Xml.XmlDocumentType.Entities%2A>」を参照してください。  
  
 **XmlAttributeCollection** は **XmlNamedNodeMap** から派生しており、属性だけは変更できますが、記法とエンティティは読み取り専用です。 属性に対して **XmlNamedNodeMap** を使用すると、XML 名に基づいて属性のノードを取得できます。 これは、要素ノードの属性コレクションを操作する簡単な方法です。 **XmlNodeList** にも **IEnumerable** インターフェイスが実装されていますが、文字列ではなくインデックス アクセサーを使用するという点で異なります。 **RemoveNamedItem** メソッドと **SetNamedItem** メソッドは **XmlAttributeCollection** に対してだけ使用します。 属性コレクションに追加したり、属性コレクションから削除すると、**AttributeCollection** または **XmlNamedNodeMap** のどちらの実装を使用した場合でも、要素の属性コレクションが変更されます。 属性を移動する方法と新しい属性を作成する方法を次のコード サンプルに示します。  
  
```vb  
Imports System  
Imports System.Xml  
  
Class test  
  
    Public Shared Sub Main()  
        Dim doc As New XmlDocument()  
        doc.LoadXml("<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> ")  
  
        ' Get the attributes of node "child2 "  
        Dim ac As XmlAttributeCollection = doc.DocumentElement.ChildNodes(1).Attributes  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
        Dim i As Integer  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Get the 'attr1' from child1.  
        Dim attr As XmlAttribute = doc.DocumentElement.ChildNodes(0).Attributes(0)  
  
        ' Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem(attr)  
  
        ''attr1' will be removed from 'child1' and added to 'child2'.  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Create a new attribute and add to the collection.  
        Dim attr2 As XmlAttribute = doc.CreateAttribute("attr4")  
        attr2.Value = "val4"  
        ac.SetNamedItem(attr2)  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
    End Sub 'Main  
End Class 'test  
```  
  
```csharp  
using System;  
using System.Xml;  
class test {  
    public static void Main() {  
        XmlDocument doc = new XmlDocument();  
        doc.LoadXml( "<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> " );  
  
        // Get the attributes of node "child2"  
        XmlAttributeCollection ac = doc.DocumentElement.ChildNodes[1].Attributes;  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );  
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Get the 'attr1' from child1.  
        XmlAttribute attr = doc.DocumentElement.ChildNodes[0].Attributes[0];  
  
        // Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem( attr );  
  
        // 'attr1' will be removed from 'child1' and added to 'child2'.  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Create a new attribute and add to the collection.  
        XmlAttribute attr2 = doc.CreateAttribute( "attr4" );  
        attr2.Value = "val4";  
        ac.SetNamedItem( attr2 );  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
    }  
}  
```  
  
 **AttributeCollection** から属性を削除するコード サンプルについては、「[XmlNamedNodeMap.RemoveNamedItem メソッド](xref:System.Xml.XmlNamedNodeMap.RemoveNamedItem%2A)」を参照してください。 メソッドとプロパティの詳細については、「[XmlNamedNodeMap メンバー](xref:System.Xml.XmlNamedNodeMap)」を参照してください。  
  
## <a name="see-also"></a>参照

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
