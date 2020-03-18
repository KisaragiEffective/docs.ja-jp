---
title: XML での埋め込み式
ms.date: 07/20/2015
f1_keywords:
- vb.XmlEmbeddedExpression
helpviewer_keywords:
- embedded expressions [Visual Basic]
- LINQ to XML [Visual Basic], embedded expressions
- XML literals [Visual Basic], embedded expressions
ms.assetid: bf2eb779-b751-4b7c-854f-9f2161482352
ms.openlocfilehash: 0cdb960160457108ddf18c554dae5f5993269833
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74332354"
---
# <a name="embedded-expressions-in-xml-visual-basic"></a>XML での埋め込み式 (Visual Basic)
埋め込み式を使用すると、実行時に評価される式を含む XML リテラルを作成できます。 埋め込み式の構文は `%>``expression` `<%=` ます。これは、ASP.NET で使用される構文と同じです。  
  
 たとえば、XML 要素リテラルを作成し、リテラルテキストコンテンツと共に埋め込み式を組み合わせることができます。  
  
 [!code-vb[VbXMLSamples#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#27)]  
  
 `isbnNumber` に整数12345が含まれ、`modifiedDate` に日付3/5/2006 が含まれている場合、このコードが実行されると `book` の値は次のようになります。  
  
```xml  
<book category="fiction" isbn="12345">  
  <modifiedDate>3/5/2006</modifiedDate>  
</book>  
```  
  
## <a name="embedded-expression-location-and-validation"></a>埋め込み式の位置と検証  
 埋め込み式は、XML リテラル式内の特定の場所でのみ使用できます。 式の場所は、式が返すことができる型と `Nothing` の処理方法を制御します。 次の表では、埋め込み式の許可される場所と型について説明します。  
  
|リテラル内の場所|式の種類|`Nothing` の処理|  
|---|---|---|  
|XML 要素名|<xref:System.Xml.Linq.XName>|エラー|  
|XML 要素のコンテンツ|`Object` または `Object` の配列|無視|  
|XML 要素の属性名|<xref:System.Xml.Linq.XName>|属性値も `Nothing` ない限り、エラーが発生します。|  
|XML 要素の属性値|`Object`|属性宣言が無視されました|  
|XML 要素属性|<xref:System.Xml.Linq.XAttribute> または <xref:System.Xml.Linq.XAttribute> のコレクション|無視|  
|XML ドキュメントのルート要素|<xref:System.Xml.Linq.XElement> または1つの <xref:System.Xml.Linq.XElement> オブジェクトと、任意の数の <xref:System.Xml.Linq.XProcessingInstruction> および <xref:System.Xml.Linq.XComment> オブジェクトのコレクション|無視|  
  
- XML 要素名の埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#32)]  
  
- XML 要素のコンテンツ内の埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#33)]  
  
- XML 要素の属性名に含まれる埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#34)]  
  
- XML 要素の属性値に含まれる埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#35)]  
  
- XML 要素属性の埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#36)]  
  
- XML ドキュメントのルート要素内の埋め込み式の例を次に示します。  
  
     [!code-vb[VbXMLSamples#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#37)]  
  
 `Option Strict`を有効にした場合、コンパイラは、埋め込み式の型が必要な型に拡大変換されるかどうかをチェックします。 唯一の例外は、コードの実行時に検証される XML ドキュメントのルート要素です。 `Option Strict`せずにコンパイルする場合は `Object` 型の式を埋め込むことができ、その型は実行時に検証されます。  
  
 コンテンツが省略可能な場所では、`Nothing` を含む埋め込み式は無視されます。 これは、XML リテラルを使用する前に、要素の内容、属性値、および配列要素が `Nothing` ないことを確認する必要がないことを意味します。 要素名や属性名などの必須の値を `Nothing`することはできません。  
  
 特定の型のリテラルで埋め込み式を使用する方法の詳細については、「[XML ドキュメントリテラル](../../../../visual-basic/language-reference/xml-literals/xml-document-literal.md)、 [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)」を参照してください。  
  
## <a name="scoping-rules"></a>スコープの規則  
 コンパイラは、各 XML リテラルを適切なリテラル型のコンストラクター呼び出しに変換します。 XML リテラル内のリテラルコンテンツと埋め込み式は、引数としてコンストラクターに渡されます。 これは、XML リテラルで使用できるすべての Visual Basic プログラミング要素も、その埋め込み式で使用できることを意味します。  
  
 XML リテラル内では、`Imports` ステートメントで宣言された XML 名前空間プレフィックスにアクセスできます。 `xmlns` 属性を使用して、新しい XML 名前空間プレフィックスを宣言したり、既存の XML 名前空間プレフィックスを要素内にシャドウしたりすることができます。 新しい名前空間は、その要素の子ノードでは使用できますが、埋め込み式の XML リテラルには使用できません。  
  
> [!NOTE]
> `xmlns` namespace 属性を使用して XML 名前空間プレフィックスを宣言する場合、属性値は定数文字列である必要があります。 この点で、`xmlns` 属性の使用は、`Imports` ステートメントを使用して XML 名前空間を宣言するのと似ています。 埋め込み式を使用して XML 名前空間の値を指定することはできません。  
  
## <a name="see-also"></a>参照

- [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML ドキュメント リテラル](../../../../visual-basic/language-reference/xml-literals/xml-document-literal.md)
- [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [XML リテラルの概要](../../../../visual-basic/programming-guide/language-features/xml/xml-literals-overview.md)
