---
title: '方法: XML リテラルを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- XML literals [Visual Basic], creating
ms.assetid: 573a6db5-b14d-4e42-b356-8cc7e2d77745
ms.openlocfilehash: e3af5185d2c2106e6a696a6569ef59897d0f1fe1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333000"
---
# <a name="how-to-create-xml-literals-visual-basic"></a>方法 : XML リテラルを作成する (Visual Basic)
Xml リテラルを使用して、XML ドキュメント、フラグメント、または要素をコード内で直接作成できます。 このトピックの例では、3つの子要素を持つ XML 要素を作成する方法と、XML ドキュメントを作成する方法を示します。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] Api を使用して、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを作成することもできます。 詳細については、「 <xref:System.Xml.Linq.XElement>」を参照してください。  
  
### <a name="to-create-an-xml-element"></a>XML 要素を作成するには  
  
- Xml リテラル構文を使用して XML インラインを作成します。これは、実際の XML 構文と同じです。  
  
     [!code-vb[VbXMLSamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#5)]  
  
     コードを実行します。 このコードの出力は次のとおりです。  
  
     `<contact>`  
  
     `<name>Patrick Hines</name>`  
  
     `<phone type="home">206-555-0144</phone>`  
  
     `<phone type="work">425-555-0145</phone>`  
  
     `</contact>`  
  
### <a name="to-create-an-xml-document"></a>XML ドキュメントを作成するには  
  
- XML ドキュメントをインラインで作成します。 次のコードでは、リテラル構文、XML 宣言、処理命令、コメント、および別の要素を含む要素を含む XML ドキュメントを作成します。  
  
     [!code-vb[VbXMLSamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#30)]  
  
     コードを実行します。 このコードの出力は次のとおりです。  
  
     `<?xml-stylesheet type="text/xsl" href="show_book.xsl"?>`  
  
     `<!-- Tests that the application works. -->`  
  
     `<books>`  
  
     `<book/>`  
  
     `</books>`  
  
## <a name="see-also"></a>参照

- [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)
- [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
- [XML ドキュメント リテラル](../../../../visual-basic/language-reference/xml-literals/xml-document-literal.md)
