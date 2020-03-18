---
title: XML ドキュメント リテラル
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralDocument
helpviewer_keywords:
- XML document literal [Visual Basic]
- XML literals [Visual Basic], document
- XML documents [Visual Basic], creating
- document literal [Visual Basic]
ms.assetid: f7bbee56-0911-41de-b907-96f20450137b
ms.openlocfilehash: db77cccd26c87e271d6db45ce514ab6dabbc53e3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349382"
---
# <a name="xml-document-literal-visual-basic"></a>XML ドキュメント リテラル (Visual Basic)
<xref:System.Xml.Linq.XDocument> オブジェクトを表すリテラル。  
  
## <a name="syntax"></a>構文  
  
```xml  
<?xml version="1.0" [encoding="encoding"] [standalone="standalone"] ?>  
[ piCommentList ]  
rootElement  
[ piCommentList ]  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`encoding`|省略可。 ドキュメントが使用するエンコーディングを宣言するリテラルテキスト。|  
|`standalone`|省略可。 リテラルテキスト。 "Yes" または "no" にする必要があります。|  
|`piCommentList`|省略可。 XML 処理命令と XML コメントの一覧。 は、次の形式をとります。<br /><br /> `piComment [` `piComment` `... ]`<br /><br /> 各 `piComment` は、次のいずれかになります。<br /><br /> [XML 処理命令リテラル](../../../visual-basic/language-reference/xml-literals/xml-processing-instruction-literal.md)を -   します。<br />[XML コメントリテラル](../../../visual-basic/language-reference/xml-literals/xml-comment-literal.md)を -   します。|  
|`rootElement`|必須。 ドキュメントのルート要素。 形式は、次のいずれかになります。<br /><br /> <ul><li>[XML 要素リテラル](../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)。</li><li>`elementExp` `%>``<%=` フォームの埋め込み式。 `elementExp` は、次のいずれかを返します。<br /><br /> <ul><li><xref:System.Xml.Linq.XElement> オブジェクト。</li><li>1つの <xref:System.Xml.Linq.XElement> オブジェクトと、任意の数の <xref:System.Xml.Linq.XProcessingInstruction> および <xref:System.Xml.Linq.XComment> オブジェクトを含むコレクション。</li></ul></li></ul><br /> 詳細については、「 [XML での埋め込み式](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)」を参照してください。|  
  
## <a name="return-value"></a>戻り値  
 <xref:System.Xml.Linq.XDocument> オブジェクト。  
  
## <a name="remarks"></a>コメント  
 XML ドキュメントリテラルは、リテラルの先頭にある XML 宣言によって識別されます。 各 XML ドキュメントリテラルは1つのルート XML 要素を持つ必要がありますが、xml 処理命令と XML コメントは、任意の数を持つことができます。  
  
 Xml ドキュメントリテラルは XML 要素には記述できません。  
  
> [!NOTE]
> XML リテラルは、行連結文字を使用せずに、複数の行にまたがることができます。 これにより、XML ドキュメントからコンテンツをコピーし、Visual Basic プログラムに直接貼り付けることができます。  
  
 Visual Basic コンパイラは、XML ドキュメントリテラルを、<xref:System.Xml.Linq.XDocument.%23ctor%2A> コンストラクターと <xref:System.Xml.Linq.XDeclaration.%23ctor%2A> コンストラクターの呼び出しに変換します。  
  
## <a name="example"></a>例  
 次の例では、xml 宣言、処理命令、コメント、および別の要素を含む要素を含む XML ドキュメントを作成します。  
  
 [!code-vb[VbXMLSamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#30)]  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- <xref:System.Xml.Linq.XProcessingInstruction>
- <xref:System.Xml.Linq.XComment>
- <xref:System.Xml.Linq.XDocument>
- [XML 処理命令リテラル](../../../visual-basic/language-reference/xml-literals/xml-processing-instruction-literal.md)
- [XML コメント リテラル](../../../visual-basic/language-reference/xml-literals/xml-comment-literal.md)
- [XML 要素リテラル](../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
- [XML リテラル](../../../visual-basic/language-reference/xml-literals/index.md)
- [Visual Basic での XML の作成](../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML での埋め込み式](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)
