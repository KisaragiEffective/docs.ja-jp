---
title: XslTransform クラスを使用した XSLT 変換
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 500335af-f9b5-413b-968a-e6d9a824478c
ms.openlocfilehash: e03eb08c71ff2d031ac61a702683e3950d94f2be
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78160236"
---
# <a name="xslt-transformations-with-the-xsltransform-class"></a>XslTransform クラスを使用した XSLT 変換

> [!NOTE]
> .NET Framework 2.0 では <xref:System.Xml.Xsl.XslTransform> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。 詳細については、「[XslCompiledTransform クラスの使用](using-the-xslcompiledtransform-class.md)」と「[XslTransform クラスからの移行](migrating-from-the-xsltransform-class.md)」を参照してください。

XSLT の目的は、たとえば、XML を Web サイトで使われる HTML に変換したり、XML ドキュメントをアプリケーションが必要とするフィールドのみが含まれたドキュメントに変換するなど、ソース XML ドキュメントの内容を形式や構造の異なる別のドキュメントに変換することです。 この変換処理の仕様は、W3C (World Wide Web Consortium) [勧告 XSLT バージョン 1.0](https://www.w3.org/TR/1999/REC-xslt-19991116) で規定されています。 .NET Framework では、<xref:System.Xml.Xsl.XslTransform> 名前空間にある <xref:System.Xml.Xsl> クラスが、この仕様の機能を実装する XSLT プロセッサです。 W3C 勧告『XSLT Version 1.0』から実装された機能の他に、「[XslTransform からの出力](outputs-from-an-xsltransform.md)」に記載されている機能がいくつかあります。 .NET Framework の変換アーキテクチャを次の図に示します。

## <a name="overview"></a>概要

![XSLT 変換アーキテクチャを示す図。](./media/xslt-transformations-with-the-xsltransform-class/xslt-transformation-architecture.gif)

XSLT 勧告では、XPath (XML Path Language) を使って XML ドキュメントの一部を選択します。XPath とは、ドキュメント ツリーのノード間を移動するのに使われるクエリ言語です。 図に示すように、XPath の .NET Framework の実装は、<xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlDataDocument>、<xref:System.Xml.XPath.XPathDocument> など、複数のクラスに格納されている XML の一部を選択するのに使用されます。 <xref:System.Xml.XPath.XPathDocument> は最適化された XSLT データ ストアであり、これを <xref:System.Xml.Xsl.XslTransform> と共に使用することで、パフォーマンスの高い XSLT 変換を実行できます。

<xref:System.Xml.Xsl.XslTransform> と XPath を使用するときによく使われるクラスと、それぞれの機能を次の表に示します。

|クラスまたはインターフェイス|Function|
|------------------------|--------------|
|<xref:System.Xml.XPath.XPathNavigator>|ストア内で移動するためのカーソル スタイルのモデルを提供し、XPath クエリをサポートする API です。 元になるストアを編集する機能は提供しません。 編集には <xref:System.Xml.XmlDocument> クラスを使用します。|
|<xref:System.Xml.XPath.IXPathNavigable>|`CreateNavigator` に <xref:System.Xml.XPath.XPathNavigator> メソッドを提供するストア用のインターフェイスです。|
|<xref:System.Xml.XmlDocument>|対象のドキュメントの編集を有効にします。 このクラスは <xref:System.Xml.XPath.IXPathNavigable> を実装しているため、後で XSLT 変換が必要になるドキュメントの編集が可能です。 詳細については、「[XslTransform への XmlDocument の入力](xmldocument-input-to-xsltransform.md)」を参照してください。|
|<xref:System.Xml.XmlDataDocument>|これは <xref:System.Xml.XmlDocument> の派生クラスです。 このクラスは、<xref:System.Data.DataSet> を使用してリレーショナル環境と XML 環境との橋渡しをし、<xref:System.Data.DataSet> で指定された対応付けに従って XML ドキュメント内の構造化データのストレージを最適化します。 このクラスは <xref:System.Xml.XPath.IXPathNavigable> を実装しているため、データベースから取得したリレーショナル データに対して XSLT 変換を実行できます。 詳細については、「[XML とリレーショナル データおよび ADO.NET との統合](xml-integration-with-relational-data-and-adonet.md)」を参照してください。|
|<xref:System.Xml.XPath.XPathDocument>|このクラスは、<xref:System.Xml.Xsl.XslTransform> の処理および XPath クエリ用に最適化されており、パフォーマンスの高い読み取り専用キャッシュを提供します。 このクラスは <xref:System.Xml.XPath.IXPathNavigable> を実装しており、XSLT 変換に使用するストアとして適しています。|
|<xref:System.Xml.XPath.XPathNodeIterator>|XPath ノード セット内を移動できるようにします。 <xref:System.Xml.XPath.XPathNavigator> のすべての XPath 選択メソッドは <xref:System.Xml.XPath.XPathNodeIterator> を返します。 同じストアに対して、選択されているノード セットを表す複数の <xref:System.Xml.XPath.XPathNodeIterator> オブジェクトを作成できます。|

## <a name="msxml-xslt-extensions"></a>MSXML XSLT 拡張機能

`msxsl:script` クラスがサポートしている Microsoft XML コア サービス (MSXML) XSLT 拡張機能は、`msxsl:node-set` 関数と <xref:System.Xml.Xsl.XslTransform> 関数のみです。

## <a name="example"></a>例

XSL スタイル シートを読み込み、mydata.xml というファイルを <xref:System.Xml.XPath.XPathDocument> に読み込み、myStyleSheet.xsl という架空のファイルに格納されているデータの変換を実行し、書式設定された出力をコンソールに送信するコード サンプルを次に示します。

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.XPath
Imports System.Xml.Xsl

Public Class Sample
    Private filename As [String] = "mydata.xml"
    Private stylesheet As [String] = "myStyleSheet.xsl"

    Public Shared Sub Main()
        Dim xslt As New XslTransform()
        xslt.Load(stylesheet)
        Dim xpathdocument As New XPathDocument(filename)
        Dim writer As New XmlTextWriter(Console.Out)
        writer.Formatting = Formatting.Indented

        xslt.Transform(xpathdocument, Nothing, writer, Nothing)
    End Sub 'Main
End Class 'Sample
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.XPath;
using System.Xml.Xsl;

public class Sample
{
    private const String filename = "mydata.xml";
    private const String stylesheet = "myStyleSheet.xsl";

    public static void Main()
    {
        XslTransform xslt = new XslTransform();
        xslt.Load(stylesheet);
        XPathDocument xpathdocument = new XPathDocument(filename);
        XmlTextWriter writer = new XmlTextWriter(Console.Out);
        writer.Formatting = Formatting.Indented;

        xslt.Transform(xpathdocument, null, writer, null);
    }
}
```

## <a name="see-also"></a>参照

- <xref:System.Xml.Xsl.XslTransform>
- [XslTransform クラスによる XSLT プロセッサの実装](xsltransform-class-implements-the-xslt-processor.md)
- [XslTransform クラスの随意動作の実装](implementation-of-discretionary-behaviors-in-the-xsltransform-class.md)
- [変換における XPathNavigator](xpathnavigator-in-transformations.md)
- [変換における XPathNodeIterator](xpathnodeiterator-in-transformations.md)
- [XslTransform への XPathDocument の入力](xpathdocument-input-to-xsltransform.md)
- [XslTransform への XmlDataDocument の入力](xmldatadocument-input-to-xsltransform.md)
- [XslTransform への XmlDocument の入力](xmldocument-input-to-xsltransform.md)
