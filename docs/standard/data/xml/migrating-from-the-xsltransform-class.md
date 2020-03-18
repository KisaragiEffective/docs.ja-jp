---
title: XslTransform クラスからの移行
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 9404d758-679f-4ffb-995d-3d07d817659e
ms.openlocfilehash: 8383d8cb3e5819c46a0716c59323e492bb9add8e
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937990"
---
# <a name="migrating-from-the-xsltransform-class"></a>XslTransform クラスからの移行

XSLT アーキテクチャは、Visual Studio 2005 リリースで設計が変更されました。 <xref:System.Xml.Xsl.XslTransform> クラスは <xref:System.Xml.Xsl.XslCompiledTransform> クラスで置き換えられました。

以降では、<xref:System.Xml.Xsl.XslCompiledTransform> クラスと <xref:System.Xml.Xsl.XslTransform> クラスの主な相違点について説明します。

## <a name="performance"></a>パフォーマンス

<xref:System.Xml.Xsl.XslCompiledTransform> クラスでは多くのパフォーマンスの向上が図られています。 新しい XSLT プロセッサは XSLT スタイル シートを、共通言語ランタイム (CLR) が他のプログラム言語で行うのと同様に、共通の中間形式にコンパイルします。 スタイル シートがコンパイルされると、キャッシュされ再利用できるようになります。

<xref:System.Xml.Xsl.XslCompiledTransform> クラスには、このクラスを <xref:System.Xml.Xsl.XslTransform> クラスよりも大幅に高速化する他の最適化も含まれています。

> [!NOTE]
> 全体的なパフォーマンスは <xref:System.Xml.Xsl.XslCompiledTransform> クラスの方が <xref:System.Xml.Xsl.XslTransform> クラスより優れていますが、<xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> クラスの <xref:System.Xml.Xsl.XslCompiledTransform> メソッドが変換で初めて呼び出されたときは、<xref:System.Xml.Xsl.XslTransform.Load%2A> クラスの <xref:System.Xml.Xsl.XslTransform> メソッドよりパフォーマンスが劣る場合があります。 これは、XSLT ファイルを読み込む前にコンパイルする必要があるためです。 詳しくは、ブログの投稿「[XslCompiledTransform Slower than XslTransform?](https://docs.microsoft.com/archive/blogs/antosha/xslcompiledtransform-slower-than-xsltransform)」(XslCompiledTransform は XslTransform より遅い?) をご覧ください。

## <a name="security"></a>セキュリティ

既定で、<xref:System.Xml.Xsl.XslCompiledTransform> クラスでは XSLT `document()` 関数と埋め込みスクリプトのサポートが無効になっています。 これらの機能を有効にするには、機能が有効になっている <xref:System.Xml.Xsl.XsltSettings> オブジェクトを作成し、それを <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> メソッドに渡します。 スクリプト作成を有効にして XSLT 変換を実行する方法を次の例に示します。

[!code-csharp[XML_Migration#16](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#16)]
[!code-vb[XML_Migration#16](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#16)]

詳しくは、「[XSLT のセキュリティに関する考慮事項](../../../../docs/standard/data/xml/xslt-security-considerations.md)」をご覧ください。

## <a name="new-features"></a>新機能

### <a name="temporary-files"></a>一時テーブル

XSLT の処理中に一時ファイルが生成されることがあります。 スタイル シートにスクリプト ブロックが含まれる場合、またはデバッグ設定を true に設定してスタイル シートをコンパイルした場合、%TEMP% フォルダーに一時ファイルが作成されます。 タイミングの問題で、一部の一時ファイルが削除されない場合があります。 たとえば、一時ファイルが現在の AppDomain やデバッガーにより使用されると、<xref:System.CodeDom.Compiler.TempFileCollection> オブジェクトの終了処理でそのファイルを削除できなくなります。

クライアントのすべての一時ファイルが削除されるように、<xref:System.Xml.Xsl.XslCompiledTransform.TemporaryFiles%2A> プロパティを使用して追加のクリーンアップを実行することができます。

### <a name="support-for-the-xsloutput-element-and-xmlwriter"></a>xsl:output 要素と XmlWriter のサポート

変換出力が <xref:System.Xml.Xsl.XslTransform> オブジェクトに送られる場合、`xsl:output` クラスでは <xref:System.Xml.XmlWriter> の設定が無視されていました。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスには、スタイル シートの <xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A> 要素から取得された出力情報を含む <xref:System.Xml.XmlWriterSettings> オブジェクトを返す `xsl:output` プロパティがあります。 <xref:System.Xml.XmlWriterSettings> オブジェクトを使用して、適切に設定された <xref:System.Xml.XmlWriter> オブジェクトを作成し、それを <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドに渡すことができます。 次の C# コードに、この処理を示します。

```csharp
// Create the XslTransform object and load the style sheet.
XslCompiledTransform xslt = new XslCompiledTransform();
xslt.Load(stylesheet);

// Load the file to transform.
XPathDocument doc = new XPathDocument(filename);

// Create the writer.
XmlWriter writer = XmlWriter.Create(Console.Out, xslt.OutputSettings);

// Transform the file and send the output to the console.
xslt.Transform(doc, writer);
writer.Close();
```

### <a name="debug-option"></a>デバッグ オプション

<xref:System.Xml.Xsl.XslCompiledTransform> クラスによりデバッグ情報を生成できます。この機能を利用して、Microsoft Visual Studio デバッガーを使用してスタイル シートをデバッグすることができます。 詳細については、「<xref:System.Xml.Xsl.XslCompiledTransform.%23ctor%28System.Boolean%29>」を参照してください。

## <a name="behavioral-differences"></a>動作の違い

### <a name="transforming-to-an-xmlreader"></a>XmlReader への変換

<xref:System.Xml.Xsl.XslTransform> クラスには、変換結果を <xref:System.Xml.XmlReader> オブジェクトとして返す Transform オーバーロードが数種類あります。 このオーバーロードを使用することで、生成される XML ツリーのシリアル化と逆シリアル化によるオーバーヘッドを生じることなく、変換結果をメモり内表現 (<xref:System.Xml.XmlDocument> または <xref:System.Xml.XPath.XPathDocument>) に読み込むことができます。 次の C# コード例で、<xref:System.Xml.XmlDocument> オブジェクトに変換結果を読み込む方法を示します。

```csharp
// Load the style sheet
XslTransform xslt = new XslTransform();
xslt.Load("MyStylesheet.xsl");

// Transform input document to XmlDocument for additional processing
XmlDocument doc = new XmlDocument();
doc.Load(xslt.Transform(input, (XsltArgumentList)null));
```

<xref:System.Xml.Xsl.XslCompiledTransform> クラスでは、<xref:System.Xml.XmlReader> オブジェクトへの変換がサポートされません。 ただし、<xref:System.Xml.XPath.XPathNavigator.CreateNavigator%2A> メソッドを使用して、生成される XML ツリーを <xref:System.Xml.XmlWriter> から直接読み込むことはできます。 次の C# コード例では、同じタスクを <xref:System.Xml.Xsl.XslCompiledTransform> を使用して行う方法を示します。

```csharp
// Transform input document to XmlDocument for additional processing
XmlDocument doc = new XmlDocument();
using (XmlWriter writer = doc.CreateNavigator().AppendChild()) {
    xslt.Transform(input, (XsltArgumentList)null, writer);
}
```

### <a name="discretionary-behavior"></a>随意動作

W3C 勧告『XSL Transformations (XSLT) Version 1.0』には、対処方法を実装者が決定できる事項があります。 このような事項は、随意動作と見なされています。 事項によっては、<xref:System.Xml.Xsl.XslCompiledTransform> クラスと <xref:System.Xml.Xsl.XslTransform> クラスで動作が異なります。 詳しくは、「[XSLT エラーの解決](../../../../docs/standard/data/xml/recoverable-xslt-errors.md)」をご覧ください。

### <a name="extension-objects-and-script-functions"></a>拡張オブジェクトとスクリプト関数

<xref:System.Xml.Xsl.XslCompiledTransform> では、スクリプト関数の使用に関して新たに 2 つの制限が加えられています。

- XPath 式からはパブリック メソッドのみを呼び出すことができる。

- オーバーロードは引数の数に基づいて区別される。 引数の数が同じオーバーロードが複数存在する場合、例外が発生します。

<xref:System.Xml.Xsl.XslCompiledTransform> では、スクリプト関数へのバインド (メソッド名参照) がコンパイル時に実行されます。XslTransform を利用するスタイル シートを <xref:System.Xml.Xsl.XslCompiledTransform> によって読み込むと、例外が発生する場合があります。

<xref:System.Xml.Xsl.XslCompiledTransform> では、`msxsl:using` 要素内に子要素として `msxsl:assembly` および `msxsl:script` を含めることがサポートされます。 `msxsl:using` 要素と `msxsl:assembly` 要素を使用して、スクリプト ブロックで使用する追加の名前空間とアセンブリを宣言できます。 詳しくは、「[msxsl:script を使用したスクリプト ブロック](../../../../docs/standard/data/xml/script-blocks-using-msxsl-script.md)」をご覧ください。

<xref:System.Xml.Xsl.XslCompiledTransform> では、複数のオーバーロードおよびそれと同数の引数を含む拡張オブジェクトは使用できません。

### <a name="msxml-functions"></a>MSXML 関数

<xref:System.Xml.Xsl.XslCompiledTransform> クラスでは、新しい MSXML 関数のサポートが追加されました。 新しい関数または強化された関数は次のとおりです。

- msxsl:node-set: <xref:System.Xml.Xsl.XslTransform> では、[node-set 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256197(v=vs.100))の引数を結果ツリー フラグメントにする必要がありました。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスでは、この要件がありません。

- msxsl:version : この関数は、<xref:System.Xml.Xsl.XslCompiledTransform> でサポートされます。

- XPath 拡張関数: [ms:string-compare 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256114(v=vs.100))、[ms:utc 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256474(v=vs.100))、[ms:namespace-uri 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256231(v=vs.100))、[ms:local-name 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256055(v=vs.100))、[ms:number 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256155(v=vs.100))、[ms:format-date 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256099(v=vs.100))、[ms:format-time 関数](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256467(v=vs.100))がサポートされるようになりました。

- スキーマ関連の XPath 拡張関数 : これは、<xref:System.Xml.Xsl.XslCompiledTransform> ではネイティブでサポートされません。 ただし、拡張関数として実装することはできます。

## <a name="see-also"></a>関連項目

- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
- [XslCompiledTransform クラスの使用](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md)
