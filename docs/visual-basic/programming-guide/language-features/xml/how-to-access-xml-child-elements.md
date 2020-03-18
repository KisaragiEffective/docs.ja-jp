---
title: '方法: XML 子要素にアクセスする'
ms.date: 07/20/2015
helpviewer_keywords:
- XML axis [Visual Basic], child
- child axis property [Visual Basic]
- XML child axis property [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: 6689eb36-c471-469f-a82d-099ab8197b25
ms.openlocfilehash: 32bdb1ba476a954bdad1f23c3ecc6129c90ccaac
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347174"
---
# <a name="how-to-access-xml-child-elements-visual-basic"></a>方法 : XML 子要素にアクセスする (Visual Basic)
この例では、子軸プロパティを使用して、XML 要素内で指定された名前を持つすべての XML 子要素にアクセスする方法を示します。 具体的には、<xref:System.Xml.Linq.XElement.Value%2A> プロパティを使用して、`name` 子軸プロパティが返すコレクション内の最初の要素の値を取得します。 子軸プロパティ `name` は、`contact` オブジェクト内の `phone` という名前のすべての子要素を取得します。 また、この例では、`phone` 子軸プロパティを使用して、`contact` オブジェクトに含まれる `phone` という名前のすべての子要素にアクセスします。  
  
## <a name="example"></a>使用例  
 [!code-vb[VbXMLSamples#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples4.vb#10)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System.Xml.Linq> 名前空間への参照  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>
- [XML 子軸プロパティ](../../../../visual-basic/language-reference/xml-axis/xml-child-axis-property.md)
- [XML Value プロパティ](../../../../visual-basic/language-reference/xml-axis/xml-value-property.md)
- [Visual Basic での XML へのアクセス](../../../../visual-basic/programming-guide/language-features/xml/accessing-xml.md)
- [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)
