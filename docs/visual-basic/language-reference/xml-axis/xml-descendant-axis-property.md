---
title: XML Descendant Axis Property
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyDescendantsAxis
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML descendant axis property [Visual Basic]
- descendant axis property [Visual Basic]
- XML axis [Visual Basic], descendant
- XML [Visual Basic], accessing
ms.assetid: a178f85b-5d54-438f-8479-40b62af6fe76
ms.openlocfilehash: b2bf524214fa8ecca215d50c198b23d127e3b400
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349446"
---
# <a name="xml-descendant-axis-property-visual-basic"></a>XML 子孫軸プロパティ (Visual Basic)

<xref:System.Xml.Linq.XElement> オブジェクト、<xref:System.Xml.Linq.XDocument> オブジェクト、<xref:System.Xml.Linq.XElement> オブジェクトのコレクション、または <xref:System.Xml.Linq.XDocument> オブジェクトのコレクションの子孫へのアクセスを提供します。

## <a name="syntax"></a>構文

```vb
object...<descendant>
```

## <a name="parts"></a>指定項目

`object` 必須。 <xref:System.Xml.Linq.XElement> オブジェクト、<xref:System.Xml.Linq.XDocument> オブジェクト、<xref:System.Xml.Linq.XElement> オブジェクトのコレクション、または <xref:System.Xml.Linq.XDocument> オブジェクトのコレクションです。

`...<` 必須。 子孫軸プロパティの開始を示します。

`descendant` 必須。 アクセスする子孫ノードの名前 ([`prefix:]name`)。

|要素|説明|
|----------|-----------------|
|`prefix`|省略可。 子孫ノードの XML 名前空間プレフィックス。 `Imports` ステートメントを使用して定義されているグローバル XML 名前空間である必要があります。|
|`name`|必須。 子孫ノードのローカル名です。 「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」を参照してください。|

`>` 必須。 子孫軸プロパティの末尾を示します。

## <a name="return-value"></a>戻り値

<xref:System.Xml.Linq.XElement> オブジェクトのコレクション。

## <a name="remarks"></a>コメント

XML 子孫軸プロパティを使用して、<xref:System.Xml.Linq.XElement> または <xref:System.Xml.Linq.XDocument> オブジェクトから、または <xref:System.Xml.Linq.XElement> または <xref:System.Xml.Linq.XDocument> オブジェクトのコレクションから、子孫ノードに名前でアクセスできます。 返されたコレクション内の最初の子孫ノードの値にアクセスするには、XML `Value` プロパティを使用します。 詳細については、「[XML 値プロパティ](../../../visual-basic/language-reference/xml-axis/xml-value-property.md)」を参照してください。

Visual Basic コンパイラは、子孫軸のプロパティを <xref:System.Xml.Linq.XContainer.Descendants%2A> メソッドの呼び出しに変換します。

## <a name="xml-namespaces"></a>XML 名前空間

子孫軸プロパティの名前は、`Imports` ステートメントでグローバルに宣言された XML 名前空間のみを使用できます。 XML 要素リテラル内でローカルに宣言された XML 名前空間を使用することはできません。 詳細については、「[Imports ステートメント (XML 名前空間)](../../../visual-basic/language-reference/statements/imports-statement-xml-namespace.md)」を参照してください。

## <a name="example"></a>例

次の例は、`name` という名前の最初の子孫ノードの値と、`contacts` オブジェクトから `phone` という名前のすべての子孫ノードの値にアクセスする方法を示しています。

[!code-vb[VbXMLSamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#25)]

このコードを実行すると、次のテキストが表示されます。

`Name: Patrick Hines`

`Home Phone = 206-555-0144`

## <a name="example"></a>例

次の例では、`ns` を名前空間プレフィックスとして宣言します。 次に、名前空間のプレフィックスを使用して XML リテラルを作成し、修飾名 `ns:name`を持つ最初の子ノードの値にアクセスします。

[!code-vb[VbXMLSamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples12.vb#26)]

このコードを実行すると、次のテキストが表示されます。

`Name: Patrick Hines`

## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- [XML 軸プロパティ](../../../visual-basic/language-reference/xml-axis/index.md)
- [XML リテラル](../../../visual-basic/language-reference/xml-literals/index.md)
- [Visual Basic での XML の作成](../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
