---
title: XslTransform クラスの随意動作の実装
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: d2758ea1-03f6-47bd-88d2-0fb7ccdb2fab
ms.openlocfilehash: b37cb0f4bf9a85053d70d549ae005c7d50a50bc0
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710805"
---
# <a name="implementation-of-discretionary-behaviors-in-the-xsltransform-class"></a>XslTransform クラスの随意動作の実装

> [!NOTE]
> .NET Framework 2.0 では <xref:System.Xml.Xsl.XslTransform> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。 詳しくは、「[XslCompiledTransform クラスの使用](using-the-xslcompiledtransform-class.md)」および「[XslTransform クラスからの移行](migrating-from-the-xsltransform-class.md)」をご覧ください。

随意動作とは、[W3C (World Wide Web Consortium) 勧告『XSL Transformations (XSLT) Version 1.0』](https://www.w3.org/TR/1999/REC-xslt-19991116)で列挙されている動作で、ある状況に対処する手段として、実装プロバイダーが複数のオプションから 1 つ選択するものです。 たとえば、W3C Recommendation は、セクション 7.3「Creating Processing Instructions」で、`xsl:processing-instruction` の内容をインスタンス化したときに、テキスト ノード以外のノードが作成されるのはエラーであるとしています。 いくつかの問題に関しては、プロセッサがエラー状態から回復するときにどのような対処をするべきかを、W3C が規定しています。 セクション 7.3 に記述されている問題に関しては、W3C では、作成されたノードとその内容を無視することで、このエラーから回復できるとしています。

W3C で許可されている随意動作について、.NET Framework の <xref:System.Xml.Xsl.XslTransform> クラスに実装されている随意動作と、その問題について記述している W3C 勧告『XSLT 1.0』のセクションを次の表に示します。

|Problem|動作|セクション|
|-------------|--------------|-------------|
|テキスト ノードが `xsl:strip-space` と `xsl:preserve-space` の両方に適合している。|回復|3.4|
|ソース ノードが複数のテンプレート規則に適合している。|回復|5.5|
|1 つの名前空間 URI (Uniform Resource Identifier) が同じインポート優先順位を持つ複数の名前空間 URI のエイリアスとして宣言されている。|回復|7.1.1|
|属性値テンプレートから生成された `xsl:attribute` と `xsl:element` 内の名前属性が有効な QName (修飾名) ではない。|例外をスロー|7.1.2 および 7.1.3|
|要素ノードに子ノードが既に追加されているにもかかわらず、その要素に属性が追加される。|回復|7.1.3|
|要素ノード以外のノードに属性が追加される。|回復|7.1.3|
|`xsl:attribute` 要素のコンテンツをインスタンス化してもテキスト ノードが作成されない。|回復|7.1.3|
|2 つの属性セットが同じインポート優先順位と展開名を持っている。 両方が同じ属性を持っており、同じ名前を持ち、より重要度の高い共通の属性が含まれた属性セットが他にない。|回復|7.1.4|
|`xsl:processing-instruction` 名前属性が NCName (コロンが含まれていない名前) も処理命令ターゲットも生成しない。|回復|7.3|
|`xsl:processing-instruction` の内容をインスタンス化すると、テキスト ノード以外のノードが作成される。|回復|7.3|
|`xsl:processing-instruction` の内容をインスタンス化した結果に文字列 "`?>`" が含まれている。|回復|7.3|
|`xsl:comment` の内容をインスタンス化した結果に文字列 "--" が含まれているか、結果が "-" で終了している。|回復|7.4|
|`xsl:comment` の内容をインスタンス化した結果、テキスト ノード以外のノードが作成される。|回復|7.4|
|変数バインディング要素内のテンプレートが属性ノードまたは名前空間ノードを返す。|回復|11.2|
|document 関数に渡された URI からのリソースの取得時にエラーが発生する。|例外をスロー|12.1|
|document 関数内の URI 参照にフラグメント ID が含まれており、その ID を処理するとエラーが発生する。|例外をスロー|12.1|
|同じ名前 (`cdata-section-elements` の `xls:output` 以外) を持つ複数の属性があり、これらの属性が同じインポート優先順位を持つ。|回復|16|
|プロセッサが `encoding` 要素の `xsl:output` 属性で指定されている文字エンコード値をサポートしない。|回復|16.1|
|`disable-output-escaping` がテキスト ノードに使用されており、そのテキスト ノードを使用して、結果ツリーにテキスト ノード以外のものが作成されている。|`disable-output-escaping` 属性を無視|16.4|
|出力エスケープが有効に設定されているテキスト ノードが結果ツリー フラグメントに含まれている場合に、そのフラグメントが数値や文字列に変換される。|無視|16.4|
|XSLT プロセッサが出力に使用しているエンコーディングで表すことができない文字に対して、出力エスケープが無効に設定される。|無視|16.4|
|要素に子または属性が追加された後で、その要素に名前空間ノードが追加される。|回復|正誤表 e25|
|`xsl:number` が NaN、無限、または 0.5 未満である。|回復|正誤表 e24|
|document 関数への 2 番目の引数ノード セットが空であり、URI 参照が相対 URI 参照である。|回復|正誤表 e14|

エラーのセクションは、W3C 『[XSL Transformations (XSLT) Version 1.0 Specification Errata](https://www.w3.org/1999/11/REC-xslt-19991116-errata/)』にあります。

## <a name="custom-defined-implementation-behaviors"></a>カスタム定義の動作の実装

<xref:System.Xml.Xsl.XslTransform> クラスの実装には固有の動作があります。 このセクションでは、`xsl:sort` のプロバイダー固有の実装と、<xref:System.Xml.Xsl.XslTransform> クラスでサポートされているオプション機能について説明します。

## <a name="xslsort"></a>xsl:sort

W3C 勧告『XSLT 1.0』には、変換を使用した並べ替えに関する留意事項の記述があります。 サポートしているシステムは次のとおりです。

- 2 つの XSLT プロセッサの両方が勧告に準拠している場合でも、それらの並べ替え動作は異なることがあります。

- すべての XSLT プロセッサが同じ言語をサポートするわけではありません。

- 言語については、`xsl:sort.` で指定されていない特定の言語での並べ替え動作がプロセッサによって異なる場合があります。

<xref:System.Xml.Xsl.XslTransform> を使用した変換の .NET Framework の実装で各データ型に対して実装されている並べ替え動作を次の表に示します。

|[データ型]|並べ替え動作|
|---------------|----------------------|
|テキスト|データは、共通言語ランタイム (CLR: Common Language Runtime) の String.Compare メソッドとカルチャ ロケールを使用して並べ替えられます。 データ型が "テキスト" である場合、<xref:System.Xml.Xsl.XslTransform> クラスの並べ替え動作は、CLR の文字列比較動作と同じです。|
|Number|数値は、XPath (XML Path Language) 数値として処理され、W3C 勧告[『XML Path Language (XPath) Version 1.0』のセクション 3.5](https://www.w3.org/TR/1999/REC-xpath-19991116/#numbers) に記述されている方法に従って並べ替えられます。|

## <a name="optional-features-supported"></a>サポートされているオプション機能

XSLT プロセッサがオプションで実装する機能を次に示します。これらの機能は、<xref:System.Xml.Xsl.XslTransform> クラスに実装されます。

|特性|参照先|メモ|
|-------------|------------------------|-----------|
|`disable-output-escaping` タグと `<xsl:text...>` タグの `<xsl:value-of...>` 属性|W3C 勧告『XSLT 1.0』<br /><br /> セクション 16.4|`disable-output-escaping` 要素または `xsl:text` 要素が `xsl:value-of`、`xsl:comment`、または `xsl:processing-instruction` のいずれかの要素で使用される場合、`xsl:attribute` 属性は無視されます。<br /><br /> テキストが含まれた結果ツリー フラグメントおよびエスケープされたテキスト出力はサポートされません。<br /><br /> <xref:System.Xml.XmlReader> オブジェクトまたは <xref:System.Xml.XmlWriter> オブジェクトへの変換時には、disable-output-escaping 属性が無視されます。|

## <a name="see-also"></a>関連項目

- <xref:System.Xml.Xsl.XslTransform>
- [XslTransform クラスによる XSLT プロセッサの実装](xsltransform-class-implements-the-xslt-processor.md)
- [XslTransform クラスを使用した XSLT 変換](xslt-transformations-with-the-xsltransform-class.md)
- [変換における XPathNavigator](xpathnavigator-in-transformations.md)
- [変換における XPathNodeIterator](xpathnodeiterator-in-transformations.md)
- [XslTransform への XPathDocument の入力](xpathdocument-input-to-xsltransform.md)
- [XslTransform への XmlDataDocument の入力](xmldatadocument-input-to-xsltransform.md)
- [XslTransform への XmlDocument の入力](xmldocument-input-to-xsltransform.md)
