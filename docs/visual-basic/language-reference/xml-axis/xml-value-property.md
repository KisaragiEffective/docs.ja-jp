---
title: XML Value プロパティ
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyExtensionValue
helpviewer_keywords:
- Value property [Visual Basic]
- Visual Basic code, accessing XML
- XML axis [Visual Basic], Value
- XML Value property [Visual Basic]
ms.assetid: 7ddd057a-a195-4e9b-ad8b-2ee0e615a20f
ms.openlocfilehash: 571d9130ef69df580bbba5d90bc8758b4b627196
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349418"
---
# <a name="xml-value-property-visual-basic"></a>XML Value プロパティ (Visual Basic)

<xref:System.Xml.Linq.XElement> オブジェクトのコレクションの最初の要素の値へのアクセスを提供します。

## <a name="syntax"></a>構文

```vb
object.Value
```

## <a name="parts"></a>指定項目

|用語|Definition|  
|---|---|  
|`object`|必須。 <xref:System.Xml.Linq.XElement> オブジェクトのコレクション。|  

## <a name="return-value"></a>戻り値

 コレクションの最初の要素の値を格納している `String`。コレクションが空の場合は `Nothing`。

## <a name="remarks"></a>コメント

 <xref:System.Xml.Linq.XElement.Value%2A> プロパティを使用すると、<xref:System.Xml.Linq.XElement> オブジェクトのコレクション内の最初の要素の値に簡単にアクセスできます。 このプロパティは、まず、コレクションに少なくとも1つのオブジェクトが含まれているかどうかを確認します。 コレクションが空の場合、このプロパティは `Nothing`を返します。 それ以外の場合、このプロパティは、コレクション内の最初の要素の <xref:System.Xml.Linq.XElement.Value%2A> プロパティの値を返します。

> [!NOTE]
> '\@' 識別子を使用して XML 属性の値にアクセスすると、属性値が `String` として返されるため、<xref:System.Xml.Linq.XAttribute.Value%2A> プロパティを明示的に指定する必要はありません。

 コレクション内の他の要素にアクセスするには、XML 拡張インデクサープロパティを使用できます。 詳細については、「[拡張インデクサープロパティ](extension-indexer-property.md)」を参照してください。

## <a name="inheritance"></a>継承

 ほとんどのユーザーは <xref:System.Collections.Generic.IEnumerable%601>を実装する必要がないため、このセクションを無視できます。

 <xref:System.Xml.Linq.XElement.Value%2A> プロパティは、`IEnumerable(Of XElement)`を実装する型の拡張プロパティです。 この拡張プロパティのバインディングは、拡張メソッドのバインディングに似ています。型がインターフェイスの1つを実装し、"Value" という名前のプロパティを定義する場合、そのプロパティは拡張プロパティよりも優先されます。 言い換えると、`IEnumerable(Of XElement)`を実装するクラスで新しいプロパティを定義することで、この <xref:System.Xml.Linq.XElement.Value%2A> プロパティをオーバーライドできます。

## <a name="example"></a>例

 次の例では、<xref:System.Xml.Linq.XElement.Value%2A> プロパティを使用して、<xref:System.Xml.Linq.XElement> オブジェクトのコレクション内の最初のノードにアクセスする方法を示します。 この例では、子軸プロパティを使用して、`contact` オブジェクト内にある `phone` という名前のすべての子ノードのコレクションを取得します。

 [!code-vb[VbXMLSamples#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#15)]

 このコードを実行すると、次のテキストが表示されます。

 `Phone number: 206-555-0144`

## <a name="example"></a>例

 次の例は、<xref:System.Xml.Linq.XAttribute> オブジェクトのコレクションから XML 属性の値を取得する方法を示しています。 この例では、属性軸プロパティを使用して、すべての `phone` 要素の `type` 属性の値を表示します。

 [!code-vb[VbXMLSamples#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#16)]

 このコードを実行すると、次のテキストが表示されます。

 ```console
 home
 work
```

## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- <xref:System.Collections.Generic.IEnumerable%601>
- [XML 軸プロパティ](index.md)
- [XML リテラル](../xml-literals/index.md)
- [Visual Basic での XML の作成](../../programming-guide/language-features/xml/creating-xml.md)
- [拡張メソッド](../../programming-guide/language-features/procedures/extension-methods.md)
- [拡張インデクサー プロパティ](extension-indexer-property.md)
- [XML 子軸プロパティ](xml-child-axis-property.md)
- [XML 属性軸プロパティ](xml-attribute-axis-property.md)
