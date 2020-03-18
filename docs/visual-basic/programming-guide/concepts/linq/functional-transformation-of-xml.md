---
title: XML の関数型変換
ms.date: 07/20/2015
ms.assetid: fdbe5b91-f457-4b4e-a11b-def4bdd77bab
ms.openlocfilehash: 7e029fd562ae3f221a8aaef40f332a1e3fa896eb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353424"
---
# <a name="functional-transformation-of-xml-visual-basic"></a>XML の関数型変換 (Visual Basic)
このトピックでは、XML ドキュメントを変更するための純粋関数型変換の方法について説明し、手続き型の方法と比較します。  
  
## <a name="modifying-an-xml-document"></a>XML ドキュメントの変更  
 XML プログラマにとって最も一般的なタスクの 1 つが、XML の形式の変換です。 XML ドキュメントの形式とはドキュメントの構造のことで、次のものが含まれます。  
  
- ドキュメントによって表される階層。  
  
- 要素名および属性名。  
  
- 要素と属性のデータ型。  
  
 一般に、XML の形式を変換するために最も効果的なのは、純粋関数型変換の方法です。 この方法におけるプログラマの主なタスクは、XML ドキュメント全体 (または 1 つ以上の厳密に定義されたノード) に適用する変換を作成することです。 関数型変換は、(プログラマがこの方法をいったん理解すれば) コードの記述が最も簡単で、最も保守しやすいコードが得られ、多くの場合、その他の方法よりもコンパクトです。  
  
### <a name="xml-functional-transformational-technologies"></a>XML の関数型変換のテクノロジ  
 Microsoft では、XML ドキュメントで使用する関数型変換のテクノロジを 2 つ用意しています。それらは、XSLT と LINQ to XML です。 XSLT は、<xref:System.Xml.Xsl> マネージド名前空間と、MSXML のネイティブな COM 実装でサポートされています。 XSLT は XML ドキュメントを操作するための堅牢なテクノロジですが、XSLT 言語とそれに対応する API に関する専門知識を必要とします。  
  
 LINQ to XML には、純粋関数型変換のコードを C# または Visual Basic のコード内にさまざまな表現で効果的に記述できるツールが用意されています。 たとえば、LINQ to XML のドキュメントに記載されている多くの例で、純粋関数型の方法が使用されています。 また、「[チュートリアル: WordprocessingML ドキュメント内のコンテンツの操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/tutorial-manipulating-content-in-a-wordprocessingml-document.md) 」チュートリアルでは、Microsoft Word 文書の情報を操作するために関数型の方法で LINQ to XML を使用します。  
  
 LINQ to XML と他の Microsoft XML テクノロジの比較の詳細については、「 [LINQ to XML とその他の Xml テクノロジ](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md)」を参照してください。  
  
 ソース ドキュメントの構造が標準的でない場合、ドキュメント中心の変換には XSLT を使用することをお勧めします。 ただし、LINQ to XML でもドキュメント中心の変換を実行できます。 詳細については、「[方法: 注釈を使用して XSLT スタイルの LINQ to XML ツリーを変換する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-use-annotation-trees-to-transform-linq-to-xml-trees-in-an-xslt-style.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [純粋関数型変換の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/introduction-to-pure-functional-transformations.md)
- [LINQ to XML とその他の XML テクノロジ](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md)
