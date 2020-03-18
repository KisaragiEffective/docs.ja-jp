---
title: 変換での結果ツリー フラグメントの処理
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: df363480-ba02-4233-9ddf-8434e421c4f1
ms.openlocfilehash: e454c1194e8c280042857f106e22d0d0509417e3
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156362"
---
# <a name="result-tree-fragment-in-transformations"></a>変換での結果ツリー フラグメントの処理

> [!NOTE]
> .NET Framework 2.0 では <xref:System.Xml.Xsl.XslTransform> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。 詳細については、「[XslCompiledTransform クラスの使用](using-the-xslcompiledtransform-class.md)」と「[XslTransform クラスからの移行](migrating-from-the-xsltransform-class.md)」を参照してください。

 結果ツリー フラグメントは、ノード セットの特殊な型にすぎません。 ノード フラグメントでは、ノード セットで実行できる任意の関数を実行できます。 `node-set()` 関数を使用して結果ツリー フラグメントをノード セットに変換し、ノード セットを使用できる任意の場所でそれを使用することもできます。

 結果ツリー フラグメントは、スタイル シートで `<xsl:variable>` 要素または `<xsl:param>` 要素を特定の方法で使用した結果として作成されます。 `variable` 要素と `parameter` 要素の構文は次のとおりです。

```xml
<xsl:param name=Qname select= XPath Expression >
    template body
</xsl:param>

<xsl:variable name=Qname select=XPath Expression >
    template body
</xsl:variable>
```

`parameter` 要素では、いくつかの方法で修飾名 (`Qname`) に値を割り当てます。 パラメーターに既定値を割り当てるには、`select` 属性の XPath (XML Path Language) 式から内容を返すか、テンプレート本体の内容をパラメーターに割り当てます。

`variable` 要素でも、いくつかの方法で値が割り当てられます。 `select` 属性の XPath 式から内容を返すか、テンプレート本体の内容を割り当てることができます。

`parameter` 要素でも `variable` 要素でも、XPath 式で値を割り当てた場合は、4 つの基本 XPath 型であるブール値、文字列、数値、ノード セットのうちのいずれかが返されます。 テンプレート本体で値を指定した場合は、XPath 型でないデータ型である結果ツリー フラグメントが返されます。

変数が、4 つの基本 XPath データ型のうちの 1 つではなく、結果ツリー フラグメントに関連付けられたときにだけ、XPath クエリからは 4 つの基本 XPath オブジェクト型以外の型が返されます。 結果ツリー フラグメントとその動作については、[W3C (World Wide Web Consortium) 仕様](https://www.w3.org/TR/xslt-10/)の[セクション 11.1「Result Tree Fragments」](https://www.w3.org/TR/xslt-10/#section-Result-Tree-Fragments)から[セクション 11.6「Passing Parameters to Templates」](https://www.w3.org/TR/xslt-10/#section-Passing-Parameters-to-Templates)に記述されています。 [セクション 1「Introduction」](https://www.w3.org/TR/xslt-10/#section-Introduction)でも、結果ツリー フラグメントを返したり結果ツリー フラグメントを作成したりする XSLT 名前空間の要素をテンプレートに含める方法について説明しています。

結果ツリー フラグメントは、概念的には、1 つのルート ノードだけを持つノード セットと同じ動作をします。 ただし、返されるその他のノードは子ノードになります。 プログラムで子ノードを参照するには、`<xsl:copy-of>` 要素を使用して結果ツリー フラグメントを結果ツリーにコピーします。 copy-of を実行すると、すべての子ノードも結果ツリーに順番にコピーされます。 `copy` または `copy-of` を使用するまで、結果ツリー フラグメントは、結果ツリーの一部や変換の出力の一部にはなりません。

結果ツリー フラグメントの返されたノードを反復処理するには、<xref:System.Xml.XPath.XPathNavigator> を使用します。 XML を含む `fragment` パラメーターを指定して関数を呼び出すことにより、スタイル シート内で結果ツリー フラグメントを作成するコード サンプルを次に示します。

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                xmlns:msxsl="urn:schemas-microsoft-com:xslt"
                xmlns:user="http://www.adventure-works.com"
                version="1.0">
    <xsl:var name="fragment">
        <node1>
            <node2/>
        </node1>
    <xsl:var>

  <msxsl:script language="C#" implements-prefix="user">
    function NodeFragment(XPathNavigator nav)
    {
      if (nav.HasSelection == false)
        XPathNavigator.MoveToNext();
      return;
    }
  </msxsl:script>

    <xsl:template match="/">
            <xsl:value-of select="user:NodeFragment(msxml:node-set($fragment))"/>
    </xsl:template>
</xsl:stylesheet>
```

結果ツリー フラグメントの一種である RTF (Rich Text Format) 形式の変数がノード セットに変換されない例を次に示します。 この場合、変数はスクリプト関数に渡され、ノード間の移動には <xref:System.Xml.XPath.XPathNavigator> が使用されます。

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:msxsl="urn:schemas-microsoft-com:xslt"
        xmlns:user="urn:books"
        exclude-result-prefixes="msxsl">

<xsl:output method="xml" indent="yes" omit-xml-declaration="yes"/>

<xsl:variable name="node-fragment">
    <book>Book1</book>
    <book>Book2</book>
    <book>Book3</book>
    <book>Book4</book>
</xsl:variable>

<msxsl:script implements-prefix="user" language="c#">

<![CDATA[
    string func(XPathNavigator nav)
    {
        bool b = nav.MoveToFirstChild();
        if (b)
            return nav.Value;
        else
            return "Does not exist";
    }

]]>

</msxsl:script>

<xsl:template match="/">
    <first_book>
        <xsl:value-of select="user:func($node-fragment)"/>
    </first_book>
</xsl:template>

</xsl:stylesheet>
```

このスタイル シートを使用して XML を変換した結果を次に示します。

## <a name="output"></a>Output

```xml
<first_book xmlns:user="urn:books">Book1</first_book>
```

上で説明したように、`node-set` 関数を使用すると、結果ツリー フラグメントをノード セットに変換できます。 結果として得られるノードには、ツリーのルート ノードである単一のノードが常に含まれます。 結果ツリー フラグメントから変換したノード セットは、for-each ステートメントや `select` 属性の値など、標準ノード セットを使用できる任意の場所で使用できます。 フラグメントをノード セットに変換し、ノード セットとして使用するコードを次に示します。

`<xsl:for-each select="msxsl:node-set($node-fragment)">`

`<xsl:value-of select="user:func(msxsl:node-set($node-fragment))"/>`

フラグメントをノード セットに変換した後は、ノード間の移動に <xref:System.Xml.XPath.XPathNavigator> を使用しません。 ノード セットでは、<xref:System.Xml.XPath.XPathNodeIterator> を使用します。

`$var` という変数がスタイル シート内のノード ツリーである例を次に示します。 for-each ステートメントを `node-set` 関数と組み合わせて使用すると、このツリーをノード セットとして反復処理できます。

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                xmlns:msxsl="urn:schemas-microsoft-com:xslt"
                xmlns:user="http://www.adventure-works.com"
                version="1.0">
    <xsl:variable name="states">
        <node1>AL</node1>
        <node1>CA</node1>
        <node1>CO</node1>
        <node1>WA</node1>
    </xsl:variable>

    <xsl:template match="/">
            <xsl:for-each select="msxsl:node-set($states)"/>
    </xsl:template>
</xsl:stylesheet>
```

次の例も、結果ツリー フラグメントの一種である RTF 形式の変数ですが、この場合は、変数をノード セットに変換した後、XPathNodeIterator としてスクリプト関数に渡しています。

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:msxsl="urn:schemas-microsoft-com:xslt"
        xmlns:user="urn:books"
        exclude-result-prefixes="msxsl">

<xsl:output method="xml" indent="yes" omit-xml-declaration="yes"/>

<xsl:variable name="node-fragment">
    <book>Book1</book>
    <book>Book2</book>
    <book>Book3</book>
    <book>Book4</book>
</xsl:variable>

<msxsl:script implements-prefix="user" language="c#">

<![CDATA[
    string func(XPathNodeIterator it)
    {
        it.MoveNext();
        return it.Current.Value;
        //it.Current returns XPathNavigator positioned on the current node
    }

]]>

</msxsl:script>
<xsl:template match="/">
    <books>
        <xsl:value-of select="user:func(msxsl:node-set($node-fragment))"/>
    </books>
</xsl:template>

</xsl:stylesheet>
```

このスタイル シートを使用して XML を変換した結果を次に示します。

```xml
<books xmlns:user="urn:books">Book1Book2Book3Book4</books>
```

## <a name="see-also"></a>参照

- <xref:System.Xml.XPath.XPathNodeIterator>
- [XslTransform クラスを使用した XSLT 変換](xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform クラスによる XSLT プロセッサの実装](xsltransform-class-implements-the-xslt-processor.md)
