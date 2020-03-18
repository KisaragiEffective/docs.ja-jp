---
title: XslCompiledTransform クラスの出力オプション
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 91ce8cba-386c-411e-bb38-0891a0393c0a
ms.openlocfilehash: 504057bd5e10498d39b2bce908742fc20b112c52
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710506"
---
# <a name="output-options-on-the-xslcompiledtransform-class"></a>XslCompiledTransform クラスの出力オプション
このトピックでは、XSLT で使用できる出力オプションについて説明します。 出力オプションは、スタイル シート内で、または <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドで指定できます。  
  
## <a name="xsloutput-element"></a>xsl:output 要素  
 `xsl:output` 要素は出力のオプションを指定します。 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドで指定する出力の種類により、`xsl:output` オプションの動作が決定されます。  
  
 次の表では、出力の種類がストリームか `xsl:output` の場合に <xref:System.IO.TextWriter> 要素で使用できる各属性の動作について説明します。  
  
|［属性名］:|動作|  
|--------------------|--------------|  
|メソッド|サポートあり。|  
|のバージョン|無視。 バージョンは常に XML では 1.0、そして HTML では 4.0 です。|  
|encoding|<xref:System.IO.TextWriter> への出力時には無視されます。 <xref:System.IO.TextWriter.Encoding%2A?displayProperty=nameWithType> プロパティが代わりに使用されます。|  
|omit-xml-declaration|サポートあり。|  
|standalone|サポートあり。|  
|doctype-public|サポートあり。|  
|doctype-system|サポートあり。|  
|cdata-section-elements|サポートあり。|  
|indent|サポートあり。|  
|media-type|サポートあり。|  
  
#### <a name="sending-output-to-an-xmlwriter"></a>XmlWriter への出力の送出  
 スタイル シートで `xsl:output` 要素が使用され、出力の種類が <xref:System.Xml.XmlWriter> オブジェクトの場合は、<xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A?displayProperty=nameWithType> オブジェクトを作成する際に <xref:System.Xml.XmlWriter> プロパティを使用します。 <xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A?displayProperty=nameWithType> プロパティは、コンパイル済みスタイル シートの <xref:System.Xml.XmlWriterSettings> 要素から派生した情報を含む `xsl:output` オブジェクトを返します。 この <xref:System.Xml.XmlWriterSettings> オブジェクトを <xref:System.Xml.XmlWriter.Create%2A?displayProperty=nameWithType> メソッドに渡して、正しい設定の <xref:System.Xml.XmlWriter> オブジェクトを作成することができます。  
  
## <a name="output-types"></a>出力の種類  
 次の一覧では、<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> コマンドで使用できる出力の種類について説明します。  
  
#### <a name="xmlwriter"></a>XmlWriter  
 <xref:System.Xml.XmlWriter> クラスは XML ストリームまたはファイルを書き出します。 <xref:System.Xml.XmlWriter> クラスを使用して、出力オプションも含め、<xref:System.Xml.XmlWriterSettings> オブジェクトでサポートする機能を指定できます。 <xref:System.Xml.XmlWriter> クラスは <xref:System.Xml> フレームワークの重要な一部分です。 出力結果を別の XML プロセスにパイプラインして送るには、この出力を使用します。  
  
#### <a name="string"></a>文字列型  
 出力ファイルの URI を指定するには、この出力を使用します。  
  
#### <a name="stream"></a>Stream  
 ストリームとは、ファイル、入出力デバイス、プロセス間通信のパイプ、または TCP/IP ソケットなどのバイト シーケンスを抽象化したものです。 <xref:System.IO.Stream> クラスとその派生クラスは、これら各種の入出力にジェネリックな視点を提供し、プログラマがオペレーティング システムや下位のデバイスに固有の詳細を考慮する必要をなくします。  
  
 <xref:System.IO.FileStream>、<xref:System.IO.MemoryStream>、または出力ストリーム (`Response.OutputStream`) にデータを送るには、この出力の種類を使用します。  
  
#### <a name="textwriter"></a>TextWriter  
 <xref:System.IO.TextWriter> は、文字シーケンスを書き出します。 これは <xref:System.IO.StringWriter> クラスおよび <xref:System.IO.StreamWriter> クラスに実装され、それぞれ文字を文字列に、またはストリームに書き出します。 文字列に出力する場合は、この出力の種類を使用します。  
  
## <a name="notes"></a>メモ  
  
- 空要素タグを書き出すとき、要素名の最後の文字とバック主ラッシュとの間に 1 つのスペースが書かれます。たとえば `<myElement />` です。 これにより、生成された HTML ページが古いブラウザーで正しく表示されます。  
  
## <a name="see-also"></a>関連項目

- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
