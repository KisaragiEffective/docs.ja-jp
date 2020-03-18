---
title: XslCompiledTransform クラスへの入力
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 834049f1-ab41-449e-9f10-4a1d0701bc48
ms.openlocfilehash: 9aae85aa4516dc0555e959358ba1b7db3002145d
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710740"
---
# <a name="inputs-to-the-xslcompiledtransform-class"></a>XslCompiledTransform クラスへの入力
<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドには、<xref:System.Xml.XPath.IXPathNavigable> インターフェイスを実装するオブジェクト、ソース ドキュメントを読み取る <xref:System.Xml.XmlReader> オブジェクト、文字列 URI という 3 種類のソース ドキュメントを入力できます。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslCompiledTransform> クラスは既定で空白を維持します。 このことは、[W3C XSLT 1.0 勧告のセクション 3.4](https://www.w3.org/TR/xslt.html#strip) に準拠しています。  
  
## <a name="ixpathnavigable-interface"></a>IXPathNavigable インターフェイス  
 <xref:System.Xml.XPath.IXPathNavigable> インターフェイスは、<xref:System.Xml.XmlNode> および <xref:System.Xml.XPath.XPathDocument> クラスに実装されています。 これらのクラスは XML データのメモリ内のキャッシュを表します。  
  
- <xref:System.Xml.XmlNode> クラスは W3C ドキュメント オブジェクト モデル (DOM) を基礎とし、編集機能も含んでいます。  
  
- <xref:System.Xml.XPath.XPathDocument> クラスは、XPath データ モデルに基づいた読み取り専用のデータ ストアです。 <xref:System.Xml.XPath.XPathDocument> は、XSLT 処理に推奨されるクラスです。 これは、<xref:System.Xml.XmlNode> クラスと比較して、より高速なパフォーマンスを提供します。  
  
> [!NOTE]
> 変換はドキュメント全体に対して行われます。 つまり、ドキュメント ルート ノード以外のノードを指定しても、変換処理では、読み込んだドキュメントのすべてのノードがアクセスされます。 ノード フラグメントを変換するには、ノード フラグメントだけを含むオブジェクトを作成し、そのオブジェクトを <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドに渡します。 詳細については、「[方法 : ノード フラグメントを変換する](../../../../docs/standard/data/xml/how-to-transform-a-node-fragment.md)」を参照してください。  
  
 次の例では、<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> メソッドにより、transform.xsl スタイル シートを使用して books.xml ファイルを books.html ファイルに変換しています。 books.xml ファイルと transform.xsl ファイルは、トピック「[方法: アセンブリを使用して XSLT 変換を実行する](../../../../docs/standard/data/xml/how-to-perform-an-xslt-transformation-by-using-an-assembly.md)」にあります。  
  
 [!code-csharp[XslCompiledTransform.Transform2#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#1)]
 [!code-vb[XslCompiledTransform.Transform2#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#1)]  
  
## <a name="xmlreader-object"></a>XmlReader オブジェクト  
 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドは、<xref:System.Xml.XmlReader> の現在のノードから、そのすべての子を通して読み込みます。 これにより、ドキュメントの一部をコンテキスト ドキュメントとして使用することができます。 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドから帰った後、<xref:System.Xml.XmlReader> は、コンテキスト ドキュメントの終了後の次のノード上に位置します。 ドキュメントの末尾に到達すると、<xref:System.Xml.XmlReader> はファイルの末尾 (EOF) に位置します。  
  
 次の例では、<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> メソッドにより、transform.xsl スタイル シートを使用して books.xml ファイルを books.html ファイルに変換しています。 books.xml ファイルと transform.xsl ファイルは、トピック「[方法: アセンブリを使用して XSLT 変換を実行する](../../../../docs/standard/data/xml/how-to-perform-an-xslt-transformation-by-using-an-assembly.md)」にあります。  
  
 [!code-csharp[XslCompiledTransform.Transform2#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#2)]
 [!code-vb[XslCompiledTransform.Transform2#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#2)]  
  
## <a name="string-uri"></a>文字列 URI  
 XSLT の入力としてソース ドキュメントの URI を指定することもできます。 URI の解決には <xref:System.Xml.XmlResolver> が使用されます。 <xref:System.Xml.XmlResolver> を <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドに渡して、使用するリゾルバーを指定することができます。 <xref:System.Xml.XmlResolver> が指定されていない場合、<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドは既定の <xref:System.Xml.XmlUrlResolver> を資格情報なしで使用します。  
  
 次の例では、<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> メソッドにより、transform.xsl スタイル シートを使用して books.xml ファイルを books.html ファイルに変換しています。 books.xml ファイルと transform.xsl ファイルは、トピック「[方法: アセンブリを使用して XSLT 変換を実行する](../../../../docs/standard/data/xml/how-to-perform-an-xslt-transformation-by-using-an-assembly.md)」にあります。  
  
 [!code-csharp[XslCompiledTransform.Transform2#3](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#3)]
 [!code-vb[XslCompiledTransform.Transform2#3](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#3)]  
  
 詳細については、「[XSLT 処理中の外部リソースの解決](../../../../docs/standard/data/xml/resolving-external-resources-during-xslt-processing.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
