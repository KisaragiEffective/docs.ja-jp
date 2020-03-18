---
title: XML の関数型変換 (C#)
ms.date: 07/20/2015
ms.assetid: 0ccb9251-38d7-44e3-9b84-1b5fe25e4b59
ms.openlocfilehash: 83ecd97f9319027dc50f346abf7a9888b5c23862
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75336795"
---
# <a name="functional-transformation-of-xml-c"></a>XML の関数型変換 (C#)
このトピックでは、XML ドキュメントを変更するための純粋関数型変換の方法について説明し、手続き型の方法と比較します。  
  
## <a name="modifying-an-xml-document"></a>XML ドキュメントの変更  
 XML プログラマにとって最も一般的なタスクの 1 つが、XML の形式の変換です。 XML ドキュメントの形式とはドキュメントの構造のことで、次のものが含まれます。  
  
- ドキュメントによって表される階層。  
  
- 要素名および属性名。  
  
- 要素と属性のデータ型。  
  
 一般に、XML の形式を変換するために最も効果的なのは、純粋関数型変換の方法です。 この方法におけるプログラマの主なタスクは、XML ドキュメント全体 (または 1 つ以上の厳密に定義されたノード) に適用する変換を作成することです。 関数型変換は、(プログラマがこの方法をいったん理解すれば) コードの記述が最も簡単で、最も保守しやすいコードが得られ、多くの場合、その他の方法よりもコンパクトです。  
  
### <a name="xml-functional-transformational-technologies"></a>XML の関数型変換のテクノロジ  
 Microsoft では、XML ドキュメントで使用する関数型変換のテクノロジを 2 つ用意しています。XSLT と LINQ to XML です。 XSLT は、<xref:System.Xml.Xsl> マネージド名前空間と、MSXML のネイティブな COM 実装でサポートされています。 XSLT は XML ドキュメントを操作するための堅牢なテクノロジですが、XSLT 言語とそれに対応する API に関する専門知識を必要とします。  
  
 LINQ to XML には、純粋関数型変換のコードを C# または Visual Basic のコード内にさまざまな表現で効果的に記述できるツールが用意されています。 たとえば、LINQ to XML のドキュメントに記載されている多くの例で、純粋関数型の方法が使用されています。 また、「[チュートリアル: WordprocessingML ドキュメント内のコンテンツの操作 (C#)](./shape-of-wordprocessingml-documents.md)」では、LINQ to XML を関数型の方法で使用し、Microsoft Word 文書の情報を操作しています。  
  
 LINQ to XML とその他の Microsoft XML テクノロジの比較の詳細については、「[LINQ to XML およびその他の XML テクノロジ](./linq-to-xml-vs-other-xml-technologies.md)」を参照してください。  
  
ソース ドキュメントの構造が標準的でない場合、ドキュメント中心の変換には XSLT を使用することをお勧めします。 ただし、LINQ to XML でもドキュメント中心の変換を実行できます。 詳細については、「[注釈を使用して XSLT スタイルの LINQ to XML ツリーを変換する方法 (C#)](./how-to-use-annotations-to-transform-linq-to-xml-trees-in-an-xslt-style.md)」を参照してください。
  
## <a name="see-also"></a>関連項目

- [純粋関数型変換の概要 (C#)](./introduction-to-pure-functional-transformations.md)
- [チュートリアル: WordprocessingML ドキュメント内のコンテンツの操作 (C#)](./shape-of-wordprocessingml-documents.md)
- [LINQ to XML とその他の XML テクノロジ](./linq-to-xml-vs-other-xml-technologies.md)
