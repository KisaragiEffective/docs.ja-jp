---
title: XML リテラルの概要
ms.date: 07/20/2015
helpviewer_keywords:
- XML literals [Visual Basic], about XML literals
- declaring XML literals [Visual Basic]
- LINQ to XML [Visual Basic], XML literals
- literals [Visual Basic], XML
ms.assetid: 37987c15-4ab8-471b-bd45-399816bfb57f
ms.openlocfilehash: 4eaa9399ca0038e3142886abf2161266f8c77782
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636082"
---
# <a name="xml-literals-overview-visual-basic"></a>XML リテラルの概要 (Visual Basic)
*Xml リテラル*を使用すると、xml を Visual Basic コードに直接組み込むことができます。 XML リテラルの構文は [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを表し、XML 1.0 の構文に似ています。 これにより、コードが最終的な XML と同じ構造を持つため、XML 要素とドキュメントをプログラムによって簡単に作成できます。  
  
 Visual Basic は、XML リテラルを [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトにコンパイルします。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] には、XML を作成および操作するための簡単なオブジェクトモデルが用意されています。このモデルは、統合言語クエリ (LINQ) と統合されています。 詳細については、「 <xref:System.Xml.Linq.XElement>」を参照してください。  
  
 XML リテラルに Visual Basic 式を埋め込むことができます。 実行時に、アプリケーションは、埋め込み式の値を組み込んで、各リテラルの [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを作成します。 これにより、XML リテラル内に動的コンテンツを指定できます。 詳細については、「[XML での埋め込み式](../../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)」を参照してください。  
  
 Xml リテラル構文と XML 1.0 構文の違いの詳細については、「 [Xml リテラルと xml 1.0 仕様](../../../../visual-basic/programming-guide/language-features/xml/xml-literals-and-the-xml-1-0-specification.md)」を参照してください。  
  
## <a name="simple-literals"></a>単純なリテラル  
 Visual Basic コードに [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを作成するには、有効な XML を入力するか、貼り付けます。 XML 要素リテラルは <xref:System.Xml.Linq.XElement> オブジェクトを返します。 詳細については、「[XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)」と「[XML リテラルと XML 1.0 仕様](../../../../visual-basic/programming-guide/language-features/xml/xml-literals-and-the-xml-1-0-specification.md)」を参照してください。 次の例では、複数の子要素を持つ XML 要素を作成します。  
  
 [!code-vb[VbXMLSamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#5)]  
  
 次の例に示すように、`<?xml version="1.0"?>`で XML リテラルを開始することで、XML ドキュメントを作成できます。 XML ドキュメントリテラルは、<xref:System.Xml.Linq.XDocument> オブジェクトを返します。 詳細については、「[XML ドキュメントリテラル](../../../../visual-basic/language-reference/xml-literals/xml-document-literal.md)」を参照してください。  
  
 [!code-vb[VbXMLSamples#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#6)]  
  
> [!NOTE]
> Visual Basic の XML リテラル構文は、XML 1.0 仕様の構文と同一ではありません。 詳細については、「 [Xml リテラル」と「xml 1.0 仕様](../../../../visual-basic/programming-guide/language-features/xml/xml-literals-and-the-xml-1-0-specification.md)」を参照してください。  
  
## <a name="line-continuation"></a>行継続  
 XML リテラルは、行連結文字を使用せずに、複数の行にまたがることができます (スペース-アンダースコア-入力シーケンス)。 これにより、コード内の XML リテラルを XML ドキュメントと比較しやすくなります。  
  
 コンパイラは、行連結文字を XML リテラルの一部として扱います。 そのため、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトに属している場合にのみ、スペース-アンダースコア-enter シーケンスを使用する必要があります。  
  
 ただし、埋め込み式に複数行式がある場合は、行連結文字が必要になります。 詳細については、「[XML での埋め込み式](../../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)」を参照してください。  
  
## <a name="embedding-queries-in-xml-literals"></a>XML リテラルへのクエリの埋め込み  
 埋め込み式では、クエリを使用できます。 この操作を行うと、クエリによって返される要素が XML 要素に追加されます。 これにより、ユーザーのクエリの結果などの動的なコンテンツを XML リテラルに追加できます。  
  
 たとえば、次のコードでは、埋め込みクエリを使用して `phoneNumbers2` 配列のメンバーから XML 要素を作成し、それらの要素を `contact2`の子として追加しています。  
  
 [!code-vb[VbXMLSamples#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#7)]  
  
## <a name="how-the-compiler-creates-objects-from-xml-literals"></a>コンパイラが XML リテラルからオブジェクトを作成する方法  
 Visual Basic コンパイラは、XML リテラルを同等の [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] コンストラクターの呼び出しに変換し、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを構築します。 たとえば、Visual Basic コンパイラは、次のコード例を XML バージョン命令の <xref:System.Xml.Linq.XProcessingInstruction> コンストラクターの呼び出しに変換し、`<contact>`、`<name>`、および `<phone>` の要素に対して <xref:System.Xml.Linq.XElement> コンストラクターを呼び出し、<xref:System.Xml.Linq.XAttribute> 属性の `type` コンストラクターを呼び出します。 具体的には、次のサンプルの属性を指定すると、Visual Basic コンパイラは、<xref:System.Xml.Linq.XAttribute.%23ctor%28System.Xml.Linq.XName%2CSystem.Object%29> コンストラクターを2回呼び出します。 最初のは、`name` パラメーターの値 `type` と `value` パラメーターの値 `home` を渡します。 2番目のパラメーターは、`name` パラメーターの値 `type` も渡しますが、`value` パラメーターの値 `work` を渡します。  
  
 [!code-vb[VbXMLSamples#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#6)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.Linq.XElement>
- [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML での埋め込み式](../../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [XML ドキュメント リテラル](../../../../visual-basic/language-reference/xml-literals/xml-document-literal.md)
- [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
- [XML リテラル](../../../../visual-basic/language-reference/xml-literals/index.md)
