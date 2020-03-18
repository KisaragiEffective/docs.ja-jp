---
title: XslTransform クラスによる XSLT プロセッサの実装
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 88373fe2-4a6b-44f9-8a62-8a3e348e3a46
ms.openlocfilehash: 73a432db9a3fcb6587184e27e6dfe9ba49010e92
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709609"
---
# <a name="xsltransform-class-implements-the-xslt-processor"></a>XslTransform クラスによる XSLT プロセッサの実装

> [!NOTE]
> .NET Framework 2.0 では <xref:System.Xml.Xsl.XslTransform> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。 詳しくは、「[XslCompiledTransform クラスの使用](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md)」および「[XslTransform クラスからの移行](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md)」をご覧ください。

<xref:System.Xml.Xsl.XslTransform> クラスは、『XSL Transformations (XSLT) Version 1.0』勧告を実装する XSLT プロセッサです。 <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドはスタイル シートを検索して読み込み、<xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは渡されたソース ドキュメントを変換します。 <xref:System.Xml.XPath.IXPathNavigable> インターフェイスを実装している任意のストアを <xref:System.Xml.Xsl.XslTransform> のソース ドキュメントとして使用できます。 .NET Framework では、現在、<xref:System.Xml.XPath.IXPathNavigable> インターフェイスを <xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlDataDocument>、および <xref:System.Xml.XPath.XPathDocument> に実装しているので、これらすべてを変換用の入力ソース ドキュメントとして使用できます。

.NET Framework の <xref:System.Xml.Xsl.XslTransform> オブジェクトは、次の名前空間で定義されている XSLT 1.0 仕様のみをサポートしています。

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
```

<xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドを使用して、次のいずれかのクラスからスタイル シートを読み込むことができます。

- XPathNavigator

- XmlReader

- URL を表す文字列

これらの入力クラスには、それぞれに対応する別々の <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドがあります。 これらのクラスの 1 つと <xref:System.Xml.XmlResolver> クラスの組み合わせを引数として受け取るメソッドもあります。 <xref:System.Xml.XmlResolver> は、スタイル シート内の `<xsl:import>` または `<xsl:include>` によって参照されているリソースを検索します。 次に示すメソッドは、文字列、<xref:System.Xml.XmlReader>、または <xref:System.Xml.XPath.XPathNavigator> を入力として受け取ります。

```vb
Overloads Public Sub Load(String)
```

```csharp
public void Load(string);
```

```vb
Overloads Public Sub Load(String, XmlResolver)
```

```csharp
public void Load(string, XmlResolver);
```

```vb
Overloads Public Sub Load(XmlReader, XmlResolver, Evidence)
```

```csharp
public void Load(XmlReader, XmlResolver, Evidence);
```

```vb
Overloads Public Sub Load(XPathNavigator, XmlResolver, Evidence)
```

```csharp
public void Load(XPathNavigator, XmlResolver, Evidence);
```

上に示した <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドの大半は、<xref:System.Xml.XmlResolver> をパラメーターとして受け取ります。 <xref:System.Xml.XmlResolver> は、スタイル シートおよび xsl:import 要素と xsl:include 要素で参照されているスタイル シートの読み込みに使用します。

大半の <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドは証拠もパラメーターとして受け取ります。 証拠パラメーターは、スタイル シートに関連付けられている <xref:System.Security.Policy.Evidence> です。 スタイル シートのセキュリティ レベルに応じて、スタイル シートに含まれているスクリプト、スタイル シートで使用されている `document()` 関数、<xref:System.Xml.Xsl.XsltArgumentList> で使用されている拡張オブジェクトなど、そのスタイル シートが参照しているリソースのセキュリティ レベルも変わります。

URL パラメーターが含まれた <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドを使用してスタイル シートを読み込んだ場合、証拠が指定されていなければ、指定された URL およびそのサイトとゾーンを組み合わせてスタイル シートの証拠が計算されます。

URI も証拠も指定されていない場合は、スタイル シートに対して設定されている証拠が完全に信頼されます。 信頼されていないソースからスタイル シートを読み込んだり、信頼されていない拡張オブジェクトを <xref:System.Xml.Xsl.XsltArgumentList> に追加したりしないでください。

セキュリティ レベルと証拠、それがスクリプトに及ぼす影響の詳細については、「[XSLT Stylesheet Scripting Using \<msxsl:script>](../../../../docs/standard/data/xml/xslt-stylesheet-scripting-using-msxsl-script.md)」(<msxsl:script> を使用する XSLT スタイルシート スクリプト) を参照してください。 セキュリティ レベルと証拠、それが拡張オブジェクトに与える影響の詳細については、「[スタイル シート パラメーターと拡張オブジェクト用の XsltArgumentList](../../../../docs/standard/data/xml/xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md)」を参照してください。

セキュリティ レベルと証拠、それが `document()` 関数に及ぼす影響については、「[外部の XSLT スタイル シートとドキュメントの解決](../../../../docs/standard/data/xml/resolving-external-xslt-style-sheets-and-documents.md)」を参照してください。

スタイル シートに対しては、多くの入力パラメーターを指定できます。 スタイル シートでは、拡張オブジェクトの関数を呼び出すこともできます。 パラメーターおよび拡張オブジェクトのいずれも <xref:System.Xml.Xsl.XsltArgumentList> クラスを使用してスタイル シートに渡されます。 <xref:System.Xml.Xsl.XsltArgumentList> の詳細については、「<xref:System.Xml.Xsl.XsltArgumentList>」を参照してください。

## <a name="recommended-secure-use-of-xsltransform-class"></a>XslTransform クラスの安全な使用方法

スタイル シートのセキュリティ特権は、指定されている証拠によって異なります。 次の表では、スタイル シートの場所と、指定する証拠の種類を説明します。

- XSLT スタイル シートに外部参照がない場合。またはスタイル シートが信頼できるコード ベースにある場合。

  - アセンブリの証拠を指定します。

    ```vb
    Dim xslt = New XslTransform() xslt.Load(stylesheet, resolver, Me.GetType().Assembly.Evidence)

    XsltTransform xslt = new XslTransform();  xslt.Load(stylesheet, resolver, this.GetType().Assembly.Evidence);
    ```

- XSLT スタイル シートが外部ソースにある場合。 ソースの出所が知られており、検証可能な URI がある。

  - URI を使用して証拠を作成します。

    ```vb
    Dim xslt As New XslTransform() Dim ev As Evidence = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri) xslt.Load(stylesheet, resolver, evidence)

    XslTransform xslt = new XslTransform(); Evidence ev = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri); xslt.Load(stylesheet, resolver, evidence);
    ```

- XSLT スタイル シートが外部ソースにある場合。 ソースの出所は不明。

  - 証拠を `null` に設定します。 スクリプト ブロックは処理されません。XSLT `document()` 関数はサポートされません。特権を持つ拡張オブジェクトは許可されません。

    `resolver` パラメーターを `null` に設定することもできます。そうすれば、`xsl:import` 要素と `xsl:include` 要素は処理されません。

- XSLT スタイル シートが外部ソースにある場合。 ソースの出所が不明であるが、スクリプトのサポートが必要。

  - 呼び出し元の証拠を要求します。

## <a name="transformation-of-xml-data"></a>XML データの変換

スタイル シートが読み込まれた後で、いずれかの <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドを呼び出し、入力ソース ドキュメントを渡すと、変換が開始されます。 <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは、さまざまな変換出力を提供できるように、オーバーロードされます。 変換の結果として得られる出力形式を次に示します。

- <xref:System.Xml.XmlReader>

- <xref:System.Xml.XmlWriter>

- <xref:System.IO.TextWriter>

- <xref:System.IO.Stream>

- ファイルの文字列 URL

最後の文字列 URL 形式は、URL 上にある入力ドキュメントを変換し、そのドキュメントを出力 URL に書き込む場合によく使用されます。 この <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは、ファイルから XML ドキュメントを読み込み、XSLT 変換を実行し、出力をファイルに書き込む場合に便利です。 これにより、入力ソース ドキュメントを作成および読み込んでからファイル ストリームに書き込む必要がなくなります。 文字列 URL を入出力として使う <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドを使用するコード サンプルを次に示します。

```vb
Dim xsltransform As XslTransform = New XslTransform()
xsltransform.Load("favorite.xsl")
xsltransform.Transform("MyDocument.Xml", "TransformResult.xml", Nothing)
```

```csharp
XslTransform xsltransform = new XslTransform();
xsltransform.Load("favorite.xsl");
xsltransform.Transform("MyDocument.xml", "TransformResult.xml", null);
```

## <a name="transforming-a-section-of-an-xml-document"></a>XML ドキュメントのセクションの変換

変換はドキュメント全体に対して行われます。 つまり、ドキュメント ルート ノード以外のノードを指定しても、変換処理では、読み込んだドキュメントのすべてのノードがアクセスされます。 結果ツリー フラグメントを変換するには、結果ツリー フラグメントだけが含まれた <xref:System.Xml.XmlDocument> を作成し、その <xref:System.Xml.XmlDocument> を <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡します。 結果ツリー フラグメントの変換を実行する例を次に示します。

```vb
Dim xslt As New XslTransform()
xslt.Load("print_root.xsl")
Dim doc As New XmlDocument()
doc.Load("library.xml")
' Create a new document containing just the result tree fragment.
Dim testNode As XmlNode = doc.DocumentElement.FirstChild
Dim tmpDoc As New XmlDocument()
tmpDoc.LoadXml(testNode.OuterXml)
' Pass the document containing the result tree fragment
' to the Transform method.
Console.WriteLine(("Passing " + tmpDoc.OuterXml + " to print_root.xsl"))
xslt.Transform(tmpDoc, Nothing, Console.Out, Nothing)
```

```csharp
XslTransform xslt = new XslTransform();
xslt.Load("print_root.xsl");
XmlDocument doc = new XmlDocument();
doc.Load("library.xml");
// Create a new document containing just the result tree fragment.
XmlNode testNode = doc.DocumentElement.FirstChild;
XmlDocument tmpDoc = new XmlDocument();
tmpDoc.LoadXml(testNode.OuterXml);
// Pass the document containing the result tree fragment
// to the Transform method.
Console.WriteLine("Passing " + tmpDoc.OuterXml + " to print_root.xsl");
xslt.Transform(tmpDoc, null, Console.Out, null);
```

この例では、ライブラリの .xml ファイルを入力として print_root し、次のコードをコンソールに出力します。

```console
Passing <book genre="novel" ISBN="1-861001-57-5"><title>Pride And Prejudice</title></book> to print_root.xsl
Root node is book.
```

library.xml

```xml
<library>
  <book genre='novel' ISBN='1-861001-57-5'>
     <title>Pride And Prejudice</title>
  </book>
  <book genre='novel' ISBN='1-81920-21-2'>
     <title>Hook</title>
  </book>
</library>
```

print_root.xsl

```xml
<stylesheet version="1.0" xmlns="http://www.w3.org/1999/XSL/Transform" >
  <output method="text" />
  <template match="/">
     Root node is  <value-of select="local-name(//*[position() = 1])" />
  </template>
</stylesheet>
```

## <a name="migration-of-xslt-from-net-framework-version-10-to-net-framework-version-11"></a>.NET Framework Version 1.0 から .NET Framework Version 1.1 への XSLT の移行

廃止された .NET Framework バージョン 1.0 と新しい .NET Framework バージョン 1.1 の <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドを次の表に示します。 新しいメソッドでは、証拠を指定することで、スタイル シートのアクセス許可を制限できます。

|廃止された .NET Framework バージョン 1.0 の Load メソッド|新しい .NET Framework バージョン 1.1 の Load メソッド|
|------------------------------------------------------|---------------------------------------------------------|
|Load(XPathNavigator input);<br /><br /> Load(XPathNavigator input, XmlResolver resolver);|Load(XPathNavigator stylesheet, XmlResolver resolver, Evidence evidence);|
|Load(IXPathNavigable stylesheet);<br /><br /> Load(IXPathNavigable stylesheet, XmlResolver resolver);|Load(IXPathNavigable stylesheet, XmlResolver resolver, Evidence evidence);|
|Load(XmlReader stylesheet);<br /><br /> Load(XmlReader stylesheet, XmlResolver resolver);|Load(XmlReader stylesheet, XmlResolver resolver, Evidence evidence);|

廃止された <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドと新しい メソッドを次の表に示します。 新しいメソッドは <xref:System.Xml.XmlResolver> オブジェクトを受け取ります。

|廃止された .NET Framework バージョン 1.0 の Transform メソッド|新しい .NET Framework バージョン 1.1 の Transform メソッド|
|-----------------------------------------------------------|--------------------------------------------------------------|
|XmlReader Transform(XPathNavigator input, XsltArgumentList args)|XmlReader Transform(XPathNavigator input, XsltArgumentList args, XmlResolver resolver)|
|XmlReader Transform(IXPathNavigable input, XsltArgumentList args)|XmlReader Transform(IXPathNavigable input, XsltArgumentList args, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output)|Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, XmlWriter output)|Void Transform(IXpathNavigable input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output)|Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output)|Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, Stream output)|Void Transform(XPathNavigator input, XsltArgumentList args, Stream output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output)|Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output, XmlResolver resolver)|
|Void Transform(String input, String output);|Void Transform(String input, String output, XmlResolver resolver);|

.NET Framework バージョン 1.1 では、<xref:System.Xml.Xsl.XslTransform.XmlResolver%2A?displayProperty=nameWithType> プロパティが廃止されています。 代わりに、<xref:System.Xml.XmlResolver> オブジェクトを受け取る新しい <xref:System.Xml.Xsl.XslTransform.Transform%2A> オーバーロードを使用します。

## <a name="see-also"></a>関連項目

- <xref:System.Xml.Xsl.XslTransform>
- [XslTransform クラスを使用した XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations-with-the-xsltransform-class.md)
- [変換における XPathNavigator](../../../../docs/standard/data/xml/xpathnavigator-in-transformations.md)
- [変換における XPathNodeIterator](../../../../docs/standard/data/xml/xpathnodeiterator-in-transformations.md)
- [XslTransform への XPathDocument の入力](../../../../docs/standard/data/xml/xpathdocument-input-to-xsltransform.md)
- [XslTransform への XmlDataDocument の入力](../../../../docs/standard/data/xml/xmldatadocument-input-to-xsltransform.md)
- [XslTransform への XmlDocument の入力](../../../../docs/standard/data/xml/xmldocument-input-to-xsltransform.md)
