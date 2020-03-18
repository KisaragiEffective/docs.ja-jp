---
title: スタイル シート パラメーターと拡張オブジェクト用の XsltArgumentList
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: de2f0dce-6b98-4908-bba7-ed150cc50355
ms.openlocfilehash: 34ffb9923337bbad90b2170a16d610d26c7f6f23
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78160197"
---
# <a name="xsltargumentlist-for-style-sheet-parameters-and-extension-objects"></a>スタイル シート パラメーターと拡張オブジェクト用の XsltArgumentList
<xref:System.Xml.Xsl.XsltArgumentList> クラスには、XSLT (Extensible Stylesheet Language for Transformations) パラメーターと XSLT 拡張オブジェクトが含まれています。 これらのパラメーターと拡張オブジェクトは、<xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡すことで、スタイル シートから呼び出せるようになります。  
  
> [!NOTE]
> .NET Framework 2.0 では、<xref:System.Xml.Xsl.XslTransform> クラスと <xref:System.Xml.Xsl.XsltArgumentList> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT 変換を実行できます。 詳細については、「[XslCompiledTransform クラスの使用](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md)」と「[XslTransform クラスからの移行](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md)」を参照してください。  
  
 <xref:System.Xml.Xsl.XsltArgumentList> クラスには、XSLT パラメーターと XSLT 拡張オブジェクトが含まれています。 これらのパラメーターと拡張オブジェクトは、<xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡すことで、スタイル シートから呼び出せるようになります。  
  
 埋め込みスクリプトを使用するのではなく、オブジェクトを渡す利点を次に示します。  
  
- クラスをより効果的にカプセル化および再利用できます。  
  
- スタイル シートを小さくすることができ、管理が簡単になります。  
  
- サポートされている <xref:System> 名前空間のセット内で定義されているもの以外の名前空間に属しているクラスのメソッドを呼び出すことができます。  
  
- <xref:System.Xml.XPath.XPathNodeIterator> を使用して結果ツリー フラグメントをスタイル シートに渡す操作がサポートされます。  
  
## <a name="xslt-style-sheet-parameters"></a>XSLT スタイル シートのパラメーター  
 XSLT パラメーターを <xref:System.Xml.Xsl.XsltArgumentList> に追加するには、<xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> メソッドを使用します。 パラメーターが追加された時点で、修飾名と名前空間 URI (Uniform Resource Identifier) がそのパラメーター オブジェクトに関連付けられます。  
  
 パラメーター オブジェクトは、W3C (World Wide Web Consortium) 型に対応している必要があります。 対応する W3C 型、それと同等の .NET Framework のクラス (型)、および W3C 型が XPath (XML Path Language) 型または XSLT 型のどちらであるかを次の表に示します。  
  
|W3C 型|対応する .NET Framework クラス (型)|XPath 型または XSLT 型|  
|--------------|----------------------------------------------|-----------------------------|  
|String|System.String|XPath|  
|Boolean|System.Boolean|XPath|  
|Number|System.Double|XPath|  
|Result Tree Fragment|System.Xml.XPath.XPathNavigator|XSLT (XSLT)|  
|Node Set|System.Xml.XPath.XPathNodeIterator|XPath|  
  
 パラメーター オブジェクトが上に示したクラスでない場合は、クラスの種類に応じて、Double または String に強制的に変換されます。 Int16、UInt16、Int32、UInt32、Int64、UInt64、Single、Decimal の各型は、強制的に Double に変換されます。 その他すべての型は、`ToString` メソッドを使用して強制的に文字列に変換されます。  
  
#### <a name="to-use-the-xslt-parameter-the-user-needs-to-do-the-following"></a>XSLT パラメーターを使用するために必要な処理  
  
1. <xref:System.Xml.Xsl.XsltArgumentList> を作成し、<xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> を使用してオブジェクトを追加します。  
  
2. スタイル シートからパラメーターを呼び出します。  
  
3. <xref:System.Xml.Xsl.XsltArgumentList> を <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡します。  
  
### <a name="example"></a>例  
 算出された割引日を保持するパラメーターを <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> メソッドを使用して作成する例を次に示します。 割引日は、発注日から 20 日後として算出されます。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public class Sample  
  
   Private Const filename As String = "order.xml"  
   Private Const stylesheet As String = "discount.xsl"  
  
   Public Shared Sub Main()  
  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Calculate the discount date.  
    Dim today As DateTime = DateTime.Now  
    Dim d As DateTime = today.AddDays(20)  
    xslArg.AddParam("discount", "", d.ToString())  
  
    'Create an XmlTextWriter to handle the output.  
    Dim writer As XmlTextWriter = New XmlTextWriter("orderout.xml", Nothing)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
  
    writer.Close()  
  
  End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "order.xml";  
   private const String stylesheet = "discount.xsl";  
  
   public static void Main() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Calculate the discount date.  
    DateTime today = DateTime.Now;  
    DateTime d = today.AddDays(20);  
    xslArg.AddParam("discount", "", d.ToString());  
  
    //Create an XmlTextWriter to handle the output.  
    XmlTextWriter writer = new XmlTextWriter("orderout.xml", null);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  }  
}  
```  
  
### <a name="input"></a>入力  
 order.xml  
  
```xml  
<!--Represents a customer order-->  
<order>  
  <book ISBN='10-861003-324'>  
    <title>The Handmaid's Tale</title>  
    <price>19.95</price>  
  </book>  
  <cd ISBN='2-3631-4'>  
    <title>Americana</title>  
    <price>16.95</price>  
  </cd>  
</order>  
```  
  
 discount.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
  <xsl:param name="discount"/>  
  <xsl:template match="/">  
    <order>  
      <xsl:variable name="sub-total" select="sum(//price)"/>  
      <total><xsl:value-of select="$sub-total"/></total>  
      15% discount if paid by: <xsl:value-of select="$discount"/>  
    </order>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Output  
  
```xml  
<order>  
   <total>36.9</total>
   15% discount if paid by: 5/6/2001 5:01:15 PM
</order>  
```  
  
## <a name="xslt-extension-objects"></a>XSLT 拡張オブジェクト  
 XSLT 拡張オブジェクトを <xref:System.Xml.Xsl.XsltArgumentList> に追加するには、<xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> メソッドを使用します。 その時点で、修飾名と名前空間 URI がその拡張オブジェクトに関連付けられます。  
  
 オブジェクトを追加する場合、<xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> の呼び出し元は、セキュリティ ポリシーで完全に信頼されている必要があります。 呼び出し元の信頼性が低いと、処理は失敗します。  
  
 オブジェクトは正常に追加されますが、正常に実行されるかどうかは保証されません。 <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドを呼び出すと、<xref:System.Xml.Xsl.XslTransform.Load%2A> の実行時に指定された証拠に基づいてアクセス許可が計算され、そのアクセス許可セットが変換処理全体に割り当てられます。 拡張オブジェクトが、アクセス許可セットにないアクセス許可を必要とする処理を実行しようとすると、例外がスローされます。  
  
 拡張オブジェクトが返すデータ型は、4 つの基本 XPath 型である数値、文字列、ブール、ノード セットのうちのいずれかになります。  
  
#### <a name="to-use-the-xslt-extension-object-the-user-needs-to-do-the-following"></a>XSLT 拡張オブジェクトを使用するために必要な処理  
  
1. <xref:System.Xml.Xsl.XsltArgumentList> を作成し、<xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> を使用して拡張オブジェクトを追加します。  
  
2. スタイル シートから拡張オブジェクトを呼び出します。  
  
3. <xref:System.Xml.Xsl.XsltArgumentList> を <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡します。  
  
### <a name="example"></a>例  
 半径が指定された円の円周を算出する例を次に示します。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public Class Sample  
   Private Const filename As String = "number.xml"  
   Private Const stylesheet As String = "circle.xsl"  
  
   Public Shared Sub Main()  
        Dim test As Sample = New Sample  
   End Sub  
  
  Public Sub New()  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Add an object to calculate the circumference of the circle.  
    Dim obj As Calculate = New Calculate  
    xslArg.AddExtensionObject("urn:myObj", obj)  
  
    'Create an XmlTextWriter to output to the console.  
    Dim writer As XmlTextWriter = New XmlTextWriter(Console.Out)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
    writer.Close()  
  
  End Sub  
  
  'Calculates the circumference of a circle given the radius.  
  Public Class Calculate  
  
    Private circ As double = 0  
  
    Public Function Circumference(radius As Double) As Double  
       circ = Math.PI*2*radius  
       Return circ  
    End Function  
  End Class  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "number.xml";  
   private const String stylesheet = "circle.xsl";  
  
   public static void Main() {  
  
        Sample test = new Sample();  
    }  
  
  public Sample() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Add an object to calculate the circumference of the circle.  
    Calculate obj = new Calculate();  
    xslArg.AddExtensionObject("urn:myObj", obj);  
  
    //Create an XmlTextWriter to output to the console.  
    XmlTextWriter writer = new XmlTextWriter(Console.Out);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  
  }  
  
  //Calculates the circumference of a circle given the radius.  
  public class Calculate {  
  
    private double circ = 0;  
  
    public double Circumference(double radius){  
       circ = Math.PI*2*radius;  
       return circ;  
    }  
  }  
}  
```  
  
### <a name="input"></a>入力  
 number.xml  
  
```xml  
<?xml version='1.0'?>  
<data>  
  <circle>  
    <radius>12</radius>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
  </circle>  
</data>
```  
  
 circle.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
    xmlns:myObj="urn:myObj">  
  
  <xsl:template match="data">  
  <circles>  
  <xsl:for-each select="circle">  
    <circle>  
    <xsl:copy-of select="node()"/>  
       <circumference>  
          <xsl:value-of select="myObj:Circumference(radius)"/>
       </circumference>  
    </circle>  
  </xsl:for-each>  
  </circles>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Output  
 `<circles xmlns:myObj="urn:myObj">`  
  
 `<circle>`  
  
 `<radius>12</radius>`  
  
 `<circumference>75.398223686155</circumference>`  
  
 `</circle>`  
  
 `<circle>`  
  
 `<radius>37.5</radius>`  
  
 `<circumference>235.61944901923448</circumference>`  
  
 `</circle>`  
  
 `</circles>`  
  
## <a name="see-also"></a>参照

- [XslTransform クラスによる XSLT プロセッサの実装](../../../../docs/standard/data/xml/xsltransform-class-implements-the-xslt-processor.md)
