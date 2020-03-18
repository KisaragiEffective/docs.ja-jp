---
title: ドキュメントの保存と書き込み
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 097b0cb1-5743-4c3a-86ef-caf5cbe6750d
ms.openlocfilehash: 0af160b720b9eddd9e72689c920316bffdc6d21e
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710220"
---
# <a name="saving-and-writing-a-document"></a>ドキュメントの保存と書き込み
<xref:System.Xml.XmlDocument> を読み込んで保存した場合、保存されたドキュメントは、元のドキュメントとは次のように異なる可能性があります。  
  
- <xref:System.Xml.XmlDocument.PreserveWhitespace%2A> メソッドを呼び出す前に `true` プロパティが <xref:System.Xml.XmlDocument.Save%2A> に設定されている場合は、ドキュメント内の空白が出力でも保持されますが、`false` に設定されている場合は、<xref:System.Xml.XmlDocument> が出力を自動的にインデントします。  
  
- 属性間のすべての空白は 1 つの空白文字になります。  
  
- 要素間の空白が変更されます。 有意の空白は保持され、意味のない空白は保持されません。 ただし、ドキュメントを保存すると、既定では <xref:System.Xml.XmlTextWriter> の**インデント**モードが使用され、出力が読みやすくなります。  
  
- 属性値を囲む引用符は、既定で二重引用符に変更されます。 <xref:System.Xml.XmlTextReader.QuoteChar%2A> の <xref:System.Xml.XmlTextWriter> プロパティを使用すると、引用符を二重引用符または一重引用符のいずれかに設定できます。  
  
- 既定では、`{` のような数値エンティティは展開されます。  
  
- 入力ドキュメントのバイト順マークは保持されません。 別のエンコーディングを指定する XML 宣言を明示的に作成しない限り、UCS-2 は UTF-8 として保存されます。  
  
- <xref:System.Xml.XmlDocument> をファイルまたはストリームに書き出す場合、書き出される出力はドキュメントのコンテンツと同じになります。 つまり、<xref:System.Xml.XmlDeclaration> が書き出されるのは、ドキュメントに が 1 つ含まれていて、ドキュメントの出力時に使われるエンコーディングが宣言ノードで指定されたエンコーディングと同じ場合だけです。  
  
## <a name="writing-an-xmldeclaration"></a>XmlDeclaration の書き込み  
 <xref:System.Xml.XmlDocument> および <xref:System.Xml.XmlDeclaration> の <xref:System.Xml.XmlNode.OuterXml%2A> メソッドに加えて、<xref:System.Xml.XmlNode.InnerXml%2A>、<xref:System.Xml.XmlNode.WriteTo%2A>、および <xref:System.Xml.XmlDocument> の <xref:System.Xml.XmlDocument.Save%2A> および <xref:System.Xml.XmlDocument.WriteContentTo%2A> メンバーは XML 宣言を作成します。  
  
 <xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlNode.OuterXml%2A> の <xref:System.Xml.XmlDocument.InnerXml%2A> プロパティ、<xref:System.Xml.XmlDocument.Save%2A>、<xref:System.Xml.XmlDocument.WriteTo%2A> メソッド、および <xref:System.Xml.XmlDocument.WriteContentTo%2A> メソッドについて、XML 宣言に書き出されるエンコーディングは <xref:System.Xml.XmlDeclaration> ノードから取り出されます。 <xref:System.Xml.XmlDeclaration> ノードがない場合、<xref:System.Xml.XmlDeclaration> は書き込まれません。<xref:System.Xml.XmlDeclaration> ノードにエンコーディングがない場合、エンコードは XML 宣言に書き出されません。  
  
 <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> および <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> メソッドは、常に <xref:System.Xml.XmlDeclaration> を書き出します。 これらのメソッドは、書き込みを行っているライターからエンコーディングを取得します。 つまり、ライターのエンコーディングの値によって、ドキュメントと <xref:System.Xml.XmlDeclaration> オブジェクトのエンコーディングがオーバーライドされます。 たとえば、次のコードでは、出力ファイル `out.xml` の XML 宣言にはエンコーディングは書き込まれません。  
  
```vb  
Dim doc As New XmlDocument()  
Dim tw As XmlTextWriter = New XmlTextWriter("out.xml", Nothing)  
doc.Load("text.xml")  
doc.Save(tw)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlTextWriter tw = new XmlTextWriter("out.xml", null);  
doc.Load("text.xml");  
doc.Save(tw);  
```  
  
 <xref:System.Xml.XmlDocument.Save%2A> メソッドでは、<xref:System.Xml.XmlWriter.WriteStartDocument%2A> クラスの <xref:System.Xml.XmlWriter> メソッドを使用して XML 宣言が書き出されます。 そのため、<xref:System.Xml.XmlWriter.WriteStartDocument%2A> メソッドをオーバーライドすると、ドキュメントの先頭部分の出力方法が変わります。  
  
 <xref:System.Xml.XmlNode.OuterXml%2A>、<xref:System.Xml.XmlDeclaration.WriteTo%2A>、および <xref:System.Xml.XmlNode.InnerXml%2A>の <xref:System.Xml.XmlDeclaration> メンバーに対して、<xref:System.Xml.XmlDeclaration.Encoding%2A> プロパティが設定されていない場合、エンコードは書き込まれません。それ以外の場合、XML 宣言で記述されたエンコーディングは、<xref:System.Xml.XmlDeclaration.Encoding%2A> プロパティにあるエンコーディングと同じになります。  
  
## <a name="writing-document-content-using-the-outerxml-property"></a>OuterXml プロパティを使用したドキュメント内容の書き込み  
 <xref:System.Xml.XmlNode.OuterXml%2A> プロパティは、W3C XML ドキュメント オブジェクト モデル (DOM) 標準に対するマイクロソフトの拡張機能です。 <xref:System.Xml.XmlNode.OuterXml%2A> プロパティは、XML ドキュメント全体のマークアップ、または 1 つのノードとその子ノードのマークアップだけを取得するために使用されます。 <xref:System.Xml.XmlNode.OuterXml%2A> は、指定されたノードとそのすべての子ノードを表すマークアップを返します。  
  
 ドキュメント全体を文字列として保存するコード サンプルを次に示します。  
  
```vb  
Dim mydoc As New XmlDocument()  
' Perform application needs here, like mydoc.Load("myfile");  
' Now save the entire document to a string variable called "xml".  
Dim xml As String = mydoc.OuterXml  
```  
  
```csharp  
XmlDocument mydoc = new XmlDocument();  
// Perform application needs here, like mydoc.Load("myfile");  
// Now save the entire document to a string variable called "xml".  
string xml = mydoc.OuterXml;  
```  
  
 ドキュメント要素だけを保存するコード サンプルを次に示します。  
  
```vb  
' For the content of the Document Element only.  
Dim xml As String = mydoc.DocumentElement.OuterXml  
```  
  
```csharp  
// For the content of the Document Element only.  
string xml = mydoc.DocumentElement.OuterXml;  
```  
  
 一方、子ノードの内容が必要な場合は、<xref:System.Xml.XmlNode.InnerText%2A> プロパティを使用します。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
