---
title: XML Attribute Axis Property
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyAttributeAxis
helpviewer_keywords:
- attribute axis property [Visual Basic]
- Visual Basic code, accessing XML
- XML attribute axis property [Visual Basic]
- XML axis [Visual Basic], attribute
- XML [Visual Basic], accessing
ms.assetid: 7a4777e1-0618-4de9-9510-fb9ace2bf4db
ms.openlocfilehash: 109c4b45a5e3ed4e3e4db49687df5cb127a5e0c6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352672"
---
# <a name="xml-attribute-axis-property-visual-basic"></a>XML 属性軸プロパティ (Visual Basic)
<xref:System.Xml.Linq.XElement> オブジェクトの属性値、または <xref:System.Xml.Linq.XElement> オブジェクトのコレクション内の最初の要素へのアクセスを提供します。  
  
## <a name="syntax"></a>構文  
  
```vb  
object.@attribute  
' -or-  
object.@<attribute>  
```  
  
## <a name="parts"></a>指定項目  
 `object`  
 必須。 <xref:System.Xml.Linq.XElement> オブジェクトまたは <xref:System.Xml.Linq.XElement> オブジェクトのコレクション。  
  
 .@  
 必須。 属性軸プロパティの開始を示します。  
  
 <  
 省略可。 `attribute` が Visual Basic の有効な識別子ではない場合に、属性の名前の先頭を示します。  
  
 `attribute`  
 必須。 アクセスする属性の名前。 [`prefix`:]`name`の形式で指定します。  
  
|要素|説明|  
|----------|-----------------|  
|`prefix`|省略可。 属性の XML 名前空間プレフィックス。 `Imports` ステートメントを使用して定義されているグローバル XML 名前空間を指定する必要があります。|  
|`name`|必須。 ローカル属性名です。 「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」を参照してください。|  
  
 \>  
 省略可。 `attribute` が Visual Basic の有効な識別子ではない場合に、属性の名前の末尾を示します。  
  
## <a name="return-value"></a>戻り値  
 `attribute`の値を格納している文字列。 属性名が存在しない場合は `Nothing` が返されます。  
  
## <a name="remarks"></a>コメント  
 XML 属性軸プロパティを使用すると、<xref:System.Xml.Linq.XElement> オブジェクトまたは <xref:System.Xml.Linq.XElement> オブジェクトのコレクション内の最初の要素から、名前を指定して属性の値にアクセスできます。 名前を指定して属性値を取得することも、@ identifier の前に新しい名前を指定することによって新しい属性を要素に追加することもできます。  
  
 @ Identifier を使用して XML 属性を参照する場合、属性値は文字列として返されるため、<xref:System.Xml.Linq.XAttribute.Value%2A> プロパティを明示的に指定する必要はありません。  
  
 XML 属性の名前付け規則は、Visual Basic 識別子の名前付け規則とは異なります。 有効な Visual Basic 識別子ではない名前を持つ XML 属性にアクセスするには、山かっこ (\< と >) で名前を囲みます。  
  
## <a name="xml-namespaces"></a>XML 名前空間  
 属性軸プロパティの名前は、`Imports` ステートメントを使用してグローバルに宣言された XML 名前空間プレフィックスのみを使用できます。 XML 要素リテラル内でローカルに宣言されている XML 名前空間プレフィックスは使用できません。 詳細については、「[Imports ステートメント (XML 名前空間)](../../../visual-basic/language-reference/statements/imports-statement-xml-namespace.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例は、`phone`という名前の XML 要素のコレクションから、`type` という名前の XML 属性の値を取得する方法を示しています。  
  
 [!code-vb[VbXMLSamples#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#12)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
 `<phoneTypes>`  
  
 `<type>home</type>`  
  
 `<type>work</type>`  
  
 `</phoneTypes>`  
  
## <a name="example"></a>例  
 次の例では、xml 要素の属性を宣言によって XML の一部として、また <xref:System.Xml.Linq.XElement> オブジェクトのインスタンスに属性を追加することによって動的に作成する方法を示します。 `type` 属性は宣言によって作成され、`owner` 属性は動的に作成されます。  
  
 [!code-vb[VbXMLSamples#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#44)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
```xml  
<phone type="home" owner="Harris, Phyllis">206-555-0144</phone>  
```  
  
## <a name="example"></a>例  
 次の例では、山かっこ構文を使用して `number-type`という名前の XML 属性の値を取得しますが、これは Visual Basic の有効な識別子ではありません。  
  
 [!code-vb[VbXMLSamples#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#13)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
 `Phone type: work`  
  
## <a name="example"></a>例  
 次の例では、`ns` を名前空間プレフィックスとして宣言します。 次に、名前空間のプレフィックスを使用して XML リテラルを作成し、修飾名 "`ns:name`" を持つ最初の子ノードにアクセスします。  
  
 [!code-vb[VbXMLSamples#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples6.vb#14)]  
  
 このコードを実行すると、次のテキストが表示されます。  
  
 `Phone type: home`  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- [XML 軸プロパティ](../../../visual-basic/language-reference/xml-axis/index.md)
- [XML リテラル](../../../visual-basic/language-reference/xml-literals/index.md)
- [Visual Basic での XML の作成](../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
