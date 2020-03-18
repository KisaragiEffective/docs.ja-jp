---
title: 拡張インデクサー プロパティ
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyExtensionIndexer
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML extension indexer [Visual Basic]
- extension indexer [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: a16a4b13-54be-432c-82b3-a87091464ada
ms.openlocfilehash: 5f91dc8a6b1a0d82daa4891cf826c16e2716839f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352695"
---
# <a name="extension-indexer-property-visual-basic"></a>拡張インデクサー プロパティ (Visual Basic)
コレクション内の個々の要素にアクセスできます。  
  
## <a name="syntax"></a>構文  
  
```vb  
object(index)  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`object`|必須。 クエリ可能なコレクション。 つまり、<xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Linq.IQueryable%601>を実装するコレクションです。|  
|(|必須。 インデクサープロパティの開始を示します。|  
|`index`|必須。 コレクションの要素の0から始まる位置を指定する整数式。|  
|)|必須。 インデクサープロパティの末尾を示します。|  
  
## <a name="return-value"></a>戻り値  
 コレクション内の指定した位置にあるオブジェクト。インデックスが範囲外の場合は `Nothing`。  
  
## <a name="remarks"></a>コメント  
 拡張インデクサープロパティを使用して、コレクション内の個々の要素にアクセスできます。 このインデクサープロパティは、通常、XML 軸プロパティの出力に使用されます。 XML 子および XML 子孫軸プロパティは、<xref:System.Xml.Linq.XElement> オブジェクトまたは属性値のコレクションを返します。  
  
 Visual Basic コンパイラは、拡張インデクサープロパティを `ElementAtOrDefault` メソッドの呼び出しに変換します。 配列インデクサーとは異なり、`ElementAtOrDefault` メソッドは、インデックスが範囲外の場合に `Nothing` を返します。 この動作は、コレクション内の要素の数を簡単に判断できない場合に便利です。  
  
 このインデクサープロパティは、<xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Linq.IQueryable%601>を実装するコレクションの拡張プロパティに似ています。このプロパティは、コレクションにインデクサーまたは default プロパティがない場合にのみ使用されます。  
  
 <xref:System.Xml.Linq.XElement> または <xref:System.Xml.Linq.XAttribute> オブジェクトのコレクション内の最初の要素の値にアクセスするには、XML `Value` プロパティを使用できます。 詳細については、「 [XML 値プロパティ](../../../visual-basic/language-reference/xml-axis/xml-value-property.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、拡張インデクサーを使用して、<xref:System.Xml.Linq.XElement> オブジェクトのコレクション内の2番目の子ノードにアクセスする方法を示します。 コレクションにアクセスするには、子軸プロパティを使用します。このプロパティは、`contact` オブジェクト内の `phone` という名前のすべての子要素を取得します。  
  
 [!code-vb[VbXMLSamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#24)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
 `Second phone number: 425-555-0145`  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- [XML 軸プロパティ](../../../visual-basic/language-reference/xml-axis/index.md)
- [XML リテラル](../../../visual-basic/language-reference/xml-literals/index.md)
- [Visual Basic での XML の作成](../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML Value プロパティ](../../../visual-basic/language-reference/xml-axis/xml-value-property.md)
