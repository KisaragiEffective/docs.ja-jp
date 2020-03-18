---
title: DOM の拡張
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: b5489c96-4afd-439a-a25d-fc82eb4a148d
ms.openlocfilehash: 11c7e8c8d2ea3b49fe73ab4dde4e2ccc8bc917ff
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159677"
---
# <a name="extending-the-dom"></a>DOM の拡張

Microsoft .NET Framework は、XML ドキュメント オブジェクト モデル (DOM) の実装を提供するクラスの基本セットを備えています。 <xref:System.Xml.XmlNode> およびその派生クラスのメソッドとプロパティを利用して、XML ドキュメントの内容および構造の中で移動し、それらに対してクエリを実行し、それらを変更することができます。

DOM を使用して XML コンテンツをメモリに読み込むと、作成されたノードには、ノード名やノード型などの情報が格納されます。 場合によっては、基本クラスによっては提供されない特定のノード情報が必要になることもあります。 たとえば、ノードの行番号と位置が必要な場合です。 このような場合は、既存の DOM クラスから新しいクラスを派生させ、追加機能を持たせることができます。

新しいクラスを派生させるときの一般的なガイドラインは、次の 2 つです。

- <xref:System.Xml.XmlNode> クラスからはクラスを派生させないことをお勧めします。 その代わりに、目的とするノード型に対応するクラスからクラスを派生させることをお勧めします。 たとえば、属性ノードの追加情報を返すようにする場合は、<xref:System.Xml.XmlAttribute> クラスからクラスを派生させます。

- ノード作成メソッド以外の関数をオーバーライドするときは、常にその関数の基本バージョンを呼び出してから、他の処理を追加することをお勧めします。

## <a name="creating-your-own-node-instances"></a>独自のノード インスタンスの作成

<xref:System.Xml.XmlDocument> クラスは、ノード作成メソッドを持っています。 XML ファイルが読み込まれると、ノード作成メソッドが呼び出され、ノードが作成されます。 これらのメソッドをオーバーライドすると、ドキュメントが読み込まれるときに独自のノード インスタンスを作成できます。 たとえば、<xref:System.Xml.XmlElement> クラスを拡張すると、<xref:System.Xml.XmlDocument> クラスが継承され、<xref:System.Xml.XmlDocument.CreateElement%2A> メソッドがオーバーライドされます。

<xref:System.Xml.XmlDocument.CreateElement%2A> メソッドをオーバーライドし、<xref:System.Xml.XmlElement> クラスの独自の実装を返す方法を次の例に示します。

```vb
Class LineInfoDocument
    Inherits XmlDocument
        Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
        Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
        Return elem
    End Function 'CreateElement
End Class 'LineInfoDocument
```

```csharp
class LineInfoDocument : XmlDocument
{
    public override XmlElement CreateElement(string prefix, string localname, string nsURI)
    {
        LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this);
        return elem;
    }
}
```

## <a name="extending-a-class"></a>クラスの拡張

クラスを拡張するには、既存の DOM クラスの 1 つから独自のクラスを派生させます。 その後、基本クラスの仮想メソッドやプロパティをオーバーライドしたり、独自のメソッドやプロパティを追加します。

次の例では、<xref:System.Xml.XmlElement> クラスと <xref:System.Xml.IXmlLineInfo> インターフェイスを実装する新しいクラスを作成しています。 行番号を収集できるようにする追加のメソッドとプロパティが定義されます。

```vb
Class LineInfoElement
   Inherits XmlElement
   Implements IXmlLineInfo
   Private lineNumber As Integer = 0
   Private linePosition As Integer = 0

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub

   Public Sub SetLineInfo(linenum As Integer, linepos As Integer)
      lineNumber = linenum
      linePosition = linepos
   End Sub

   Public ReadOnly Property LineNumber() As Integer
      Get
         Return lineNumber
      End Get
   End Property

   Public ReadOnly Property LinePosition() As Integer
      Get
         Return linePosition
      End Get
   End Property

   Public Function HasLineInfo() As Boolean
      Return True
   End Function
End Class ' End LineInfoElement class.
```

```csharp
class LineInfoElement : XmlElement, IXmlLineInfo {
   int lineNumber = 0;
   int linePosition = 0;
   internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ) : base( prefix, localname, nsURI, doc ) {
       ( (LineInfoDocument)doc ).IncrementElementCount();
  }
  public void SetLineInfo( int linenum, int linepos ) {
      lineNumber = linenum;
      linePosition = linepos;
  }
  public int LineNumber {
     get {
       return lineNumber;
     }
  }
  public int LinePosition {
      get {
        return linePosition;
      }
  }
  public bool HasLineInfo() {
    return true;
  }
} // End LineInfoElement class.
```

### <a name="example"></a>例

XML ドキュメントの要素数を数える例を次に示します。

```vb
Imports System.Xml
Imports System.IO

Class LineInfoDocument
   Inherits XmlDocument

   Private elementCount As Integer

   Friend Sub New()
      elementCount = 0
   End Sub

   Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
      Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
      Return elem
   End Function

   Public Sub IncrementElementCount()
      elementCount += 1
   End Sub

   Public Function GetCount() As Integer
      Return elementCount
   End Function
End Class 'End LineInfoDocument class.

Class LineInfoElement
   Inherits XmlElement

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub 'New
End Class 'LineInfoElement
 _ 'End LineInfoElement class.

Public Class Test

   Private filename As [String] = "book.xml"

   Public Shared Sub Main()

      Dim doc As New LineInfoDocument()
      doc.Load(filename)
      Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount())
   End Sub
End Class
```

```csharp
using System;
using System.Xml;
using System.IO;

class LineInfoDocument : XmlDocument {

  int elementCount;
  internal LineInfoDocument():base() {
    elementCount = 0;
  }

  public override XmlElement CreateElement( string prefix, string localname, string nsURI) {
    LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this );
    return elem;
  }

  public void IncrementElementCount() {
     elementCount++;
  }

  public int GetCount() {
     return elementCount;
  }
} // End LineInfoDocument class.

class LineInfoElement:XmlElement {

    internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ):base( prefix,localname,nsURI, doc ){
      ((LineInfoDocument)doc).IncrementElementCount();
    }
} // End LineInfoElement class.

public class Test {

  const String filename = "book.xml";
  public static void Main() {

     LineInfoDocument doc =new LineInfoDocument();
     doc.Load(filename);
     Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount());

  }
}
```

#### <a name="input"></a>入力

book.xml

```xml
<!--sample XML fragment-->
<book genre='novel' ISBN='1-861001-57-5' misc='sale-item'>
  <title>The Handmaid's Tale</title>
  <price>14.95</price>
</book>
```

#### <a name="output"></a>Output

```console
Number of elements in book.xml: 3
```

## <a name="node-event-handler"></a>ノード イベント ハンドラー

.NET Framework による DOM の実装には、XML ドキュメントのノードが変更されたときにイベントを受け取って処理できるようにする、イベント システムも含まれています。 <xref:System.Xml.XmlNodeChangedEventHandler> クラスと <xref:System.Xml.XmlNodeChangedEventArgs> クラスを使用して、`NodeChanged` イベント、`NodeChanging` イベント、`NodeInserted` イベント、`NodeInserting` イベント、`NodeRemoved` イベント、および `NodeRemoving` イベントをキャプチャできます。

イベント処理プロセスは、派生クラスでも、元の DOM クラスとまったく同じように動作します。

ノード イベント処理については、「[イベント](../../../../docs/standard/events/index.md)」と「<xref:System.Xml.XmlNodeChangedEventHandler>」を参照してください。

## <a name="default-attributes-and-the-createelement-method"></a>既定の属性と CreateElement メソッド

派生クラスの <xref:System.Xml.XmlDocument.CreateElement%2A> メソッドをオーバーライドした場合は、ドキュメントの編集中に新しい要素を作成しても、既定の属性は追加されません。 これは編集中だけの問題です。 <xref:System.Xml.XmlDocument.CreateElement%2A> メソッドが既定の属性を <xref:System.Xml.XmlDocument> に追加する機能を実行するため、この機能は <xref:System.Xml.XmlDocument.CreateElement%2A> メソッドにコーディングする必要があります。 既定の属性が含まれた <xref:System.Xml.XmlDocument> を読み込めば、既定の属性が正しく処理されます。 既定の属性の詳細については、「[DOM の要素に対する新しい属性の作成](creating-new-attributes-for-elements-in-the-dom.md)」を参照してください。

## <a name="see-also"></a>参照

- [XML ドキュメント オブジェクト モデル (DOM)](xml-document-object-model-dom.md)
