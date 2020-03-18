---
title: XML リテラルでの空白文字
ms.date: 07/20/2015
helpviewer_keywords:
- white space [XML in Visual Basic]
- XML literals [Visual Basic], white space
ms.assetid: dfe3a9ff-d69a-418e-a6b5-476f4ed84219
ms.openlocfilehash: 56ededeb12d07e979bc86b03924e1ae0f0432822
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74336010"
---
# <a name="white-space-in-xml-literals-visual-basic"></a>XML リテラルでの空白文字 (Visual Basic)
Visual Basic コンパイラは、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトを作成するときに、XML リテラルから有意な空白文字のみを組み込みます。 意味のない空白文字は組み込まれていません。  
  
## <a name="significant-and-insignificant-white-space"></a>有意で意味のない空白  
 XML リテラルの空白文字は、3つの領域でのみ重要です。  
  
- 属性値に含まれている場合。  
  
- 要素のテキストコンテンツの一部であり、テキストに他の文字も含まれている場合。  
  
- 要素のテキストコンテンツの埋め込み式に含まれている場合。  
  
 それ以外の場合、コンパイラは空白文字を意味のないものとして扱い、リテラルの [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] オブジェクトには含まれません。  
  
 XML リテラルに意味のない空白を含めるには、空白を含む文字列リテラルを含む埋め込み式を使用します。  
  
> [!NOTE]
> XML 要素リテラルに `xml:space` 属性が含まれている場合、Visual Basic コンパイラは <xref:System.Xml.Linq.XElement> オブジェクトに属性を含めますが、この属性を追加しても、コンパイラが空白を処理する方法は変更されません。  
  
## <a name="examples"></a>使用例  
 次の例には、outer と inner の2つの XML 要素が含まれています。 両方の要素には、テキストコンテンツ内に空白が含まれています。 外側の要素の空白文字は、空白と XML 要素のみが含まれているため、意味がありません。 内部要素の空白は、空白とテキストが含まれているため、重要です。  
  
 [!code-vb[VbXMLSamples#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#29)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
```xml  
<outer>  
  <inner>  
                                          Inner text  
                                      </inner>  
</outer>  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
