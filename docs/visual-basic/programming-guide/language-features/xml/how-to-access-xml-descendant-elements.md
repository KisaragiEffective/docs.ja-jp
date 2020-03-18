---
title: '方法: XML 子孫要素にアクセスする'
ms.date: 07/20/2015
helpviewer_keywords:
- XML descendent axis property [Visual Basic]
- XML axis [Visual Basic], descendent
- descendent axis property [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: aabfa258-4112-4e7e-bab9-403f96072ef7
ms.openlocfilehash: cc045114c67ee2917ef672e734bc852c40d408ac
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347154"
---
# <a name="how-to-access-xml-descendant-elements-visual-basic"></a>方法 : XML 子孫要素にアクセスする (Visual Basic)
この例では、子孫軸プロパティを使用して、指定された名前を持ち、XML 要素に含まれているすべての XML 要素にアクセスする方法を示します。 具体的には、`Value` プロパティを使用して、`name` 子孫軸プロパティが返すコレクション内の最初の要素の値を取得します。 `name` の子孫軸プロパティは、`contacts` オブジェクトに含まれる `name` という名前のすべての要素を取得します。 また、この例では、`phone` の子孫軸プロパティを使用して、`contacts` オブジェクトに含まれている `phone` という名前のすべての子孫にアクセスします。  
  
## <a name="example"></a>使用例  
 [!code-vb[VbXMLSamples#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#31)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Xml.Linq> 名前空間への参照  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.Linq.XContainer.Descendants%2A?displayProperty=nameWithType>
- [XML 子孫軸プロパティ](../../../../visual-basic/language-reference/xml-axis/xml-descendant-axis-property.md)
- [XML Value プロパティ](../../../../visual-basic/language-reference/xml-axis/xml-value-property.md)
- [Visual Basic での XML へのアクセス](../../../../visual-basic/programming-guide/language-features/xml/accessing-xml.md)
- [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)
