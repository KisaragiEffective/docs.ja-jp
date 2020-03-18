---
title: 変換における XPathNodeIterator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 2bc6ddc6-674a-4f75-b264-abc35e4e5857
ms.openlocfilehash: 63beeb3ca9d3f3cb6e6bde418e99ee2bd0a12e20
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709739"
---
# <a name="xpathnodeiterator-in-transformations"></a>変換における XPathNodeIterator
<xref:System.Xml.XPath.XPathNodeIterator> は、XPath (XML Path Language) クエリの結果として作成されたノード セット、または node-set メソッドを使用して結果ツリー フラグメントから変換されたノード セットの反復処理を行うためのメソッドを提供します。 <xref:System.Xml.XPath.XPathNodeIterator> を使用すれば、そのノード セット内のノードの反復処理を実行できます。 ノード セットが取得されると、<xref:System.Xml.XPath.XPathNodeIterator> クラスは、選択されたノード セットへの読み取り専用、前方参照専用のカーソルを提供します。 ノード セットはドキュメント順に作成されるため、このメソッドを呼び出すと、ドキュメント順で次のノードに移動します。 <xref:System.Xml.XPath.XPathNodeIterator> は、セット内のすべてのノードのノード ツリーを構築するわけではありません。 その代わりに、XPathNodeIterator は、データへの単一ノード ウィンドウを提供し、ツリー内での移動に合わせて、自身が指している基になるノードを公開します。 <xref:System.Xml.XPath.XPathNodeIterator> クラスで利用できるメソッドとプロパティを使用すると、現在のノードから情報を取得できます。 メソッドとプロパティの一覧については、「<xref:System.Windows.Forms.ToolBar>」を参照してください。  
  
 <xref:System.Xml.XPath.XPathNodeIterator> は XPath クエリの結果作成されたノード セット内を前方にのみ移動するため、移動するときは <xref:System.Xml.XPath.XPathNodeIterator.MoveNext%2A> メソッドを使用します。 このメソッドの戻り値の型は `Boolean` であり、選択されている次のノードに移動すると `true` が返り、選択されているノードがそれ以上ないと `false` が返ります。 このメソッドが `true` を返した場合は、次のプロパティを使用できます。  
  
- <xref:System.Xml.XPath.XPathNodeIterator.Current%2A>  
  
- <xref:System.Xml.XPath.XPathNodeIterator.CurrentPosition%2A>  
  
- <xref:System.Xml.XPath.XPathNodeIterator.Count%2A>  
  
 ノード セットを初めて参照するときは、<xref:System.Xml.XPath.XPathNodeIterator.MoveNext%2A> を呼び出して、選択されているセットの最初のノードに <xref:System.Xml.XPath.XPathNodeIterator> を移動する必要があります。 これにより、while ループによる書き込みが可能になります。  
  
 <xref:System.Xml.XPath.XPathNodeIterator> を <xref:System.Xml.Xsl.XslTransform> に <xref:System.Xml.Xsl.XsltArgumentList> 内のパラメーターとして渡すコード サンプルを次に示します。 コードへの入力は **books.xml** で、スタイル シートは **text.xsl** です。 ファイル **test.xml** は、<xref:System.Xml.XPath.XPathDocument> です。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.Xsl  
Imports System.Xml.XPath  
Imports System.Text  
  
Public Class sample  
  
   Public Shared Sub Main()  
      Dim Doc As New XPathDocument("books.xml")  
      Dim nav As XPathNavigator = Doc.CreateNavigator()  
      Dim Iterator As XPathNodeIterator = nav.Select("/bookstore/book")  
  
      Dim arg As New XsltArgumentList()  
      arg.AddParam("param1", "", Iterator)  
  
      Dim xslt As New XslTransform()  
      xslt.Load("test.xsl")  
  
      Dim xd As New XPathDocument("test.xml")  
  
      Dim strmTemp = New FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite)  
      xslt.Transform(xd, arg, strmTemp, Nothing)  
   End Sub 'Main  
End Class 'sample  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.Xsl;  
using System.Xml.XPath;  
using System.Text;  
  
public class sample  
{  
    public static void Main()  
    {  
        XPathDocument Doc = new XPathDocument("books.xml");  
        XPathNavigator nav = Doc.CreateNavigator();  
        XPathNodeIterator Iterator = nav.Select("/bookstore/book");  
  
        XsltArgumentList arg = new XsltArgumentList();  
        arg.AddParam("param1", "", Iterator);  
  
        XslTransform xslt = new XslTransform();  
        xslt.Load("test.xsl");  
  
        XPathDocument xd = new XPathDocument("test.xml");  
  
        Stream strmTemp = new FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite);  
        xslt.Transform(xd, arg, strmTemp, null);  
    }  
}  
```  
  
## <a name="booksxml"></a>books.xml  
  
```xml  
<?xml version='1.0'?>  
<!-- This file represents a fragment of a book store inventory database. -->  
<bookstore specialty="novel">  
    <book style="autobiography">  
    <title>Seven Years in Trenton</title>  
        <author>  
            <first-name>Jay</first-name>  
            <last-name>Adams</last-name>  
            <award>Trenton Literary Review Honorable Mention</award>  
            <country>USA</country>  
        </author>  
        <price>12</price>  
    </book>  
    <book style="textbook">  
        <title>History of Trenton</title>  
        <author>  
            <first-name>Kim</first-name>  
            <last-name>Akers</last-name>  
            <publication>  
                Selected Short Stories of  
                <first-name>Scott</first-name>  
                <last-name>Bishop</last-name>  
                <country>US</country>  
            </publication>  
        </author>  
        <price>55</price>  
    </book>  
</bookstore>  
```  
  
## <a name="testxsl"></a>test.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
xmlns:msxsl="urn:schemas-microsoft-com:xslt" exclude-result-prefixes="msxsl">  
  
<xsl:output method="xml" indent="yes"/>  
<xsl:param name="param1"/>  
  
<xsl:template match="/">  
    <out>  
        <xsl:for-each select="$param1/title">  
            <title><xsl:value-of select="."/></title>  
        </xsl:for-each>  
    </out>  
</xsl:template>  
  
</xsl:stylesheet>  
```  
  
## <a name="testxml"></a>test.xml  
  
```xml  
<Title attr="Test">this is a test</Title>  
```  
  
## <a name="output-outxml"></a>出力 (out.xml)  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<out>  
  <title>Seven Years in Trenton</title>  
  <title>History of Trenton</title>  
</out>  
```  
  
## <a name="see-also"></a>関連項目

- [XslTransform クラスによる XSLT プロセッサの実装](../../../../docs/standard/data/xml/xsltransform-class-implements-the-xslt-processor.md)
