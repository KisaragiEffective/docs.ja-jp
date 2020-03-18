---
title: XML 要素リテラル
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralElement
helpviewer_keywords:
- XML element literal [Visual Basic]
- element literal [Visual Basic]
- XML literals [Visual Basic], element
ms.assetid: 95039642-7893-48b7-b23f-45a6c55d8f67
ms.openlocfilehash: d6d900ca6868cfffe6b0e5b349321a79c5716c46
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347027"
---
# <a name="xml-element-literal-visual-basic"></a>XML 要素リテラル (Visual Basic)

<xref:System.Xml.Linq.XElement> オブジェクトを表すリテラル。

## <a name="syntax"></a>構文

```xml
<name [ attributeList ] />
-or-
<name [ attributeList ] > [ elementContents ] </[ name ]>
```

## <a name="parts"></a>指定項目

- `<`

  必須。 開始要素タグを開きます。

- `name`

  必須。 要素名です。 形式は、次のいずれかになります。

  - `[ePrefix:]eName`形式の要素名のリテラルテキスト。次のようになります。

    |要素|説明|
    |---|---|
    |`ePrefix`|省略可。 要素の XML 名前空間プレフィックス。 は、ファイルまたはプロジェクトレベルで `Imports` ステートメントで定義されているグローバル XML 名前空間であるか、またはこの要素または親要素で定義されているローカル XML 名前空間である必要があります。|
    |`eName`|必須。 要素名です。 形式は、次のいずれかになります。<br /><br /> - リテラルテキスト。 「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」 を参照してください。<br />- フォーム `<%= eNameExp %>` の埋め込み式。 `eNameExp` の型は `String` であるか、または <xref:System.Xml.Linq.XName> に暗黙的に変換できる型である必要があります。|

  - `<%= nameExp %>`形式の埋め込み式。 `nameExp` の型は、`String` または <xref:System.Xml.Linq.XName>に暗黙的に変換できる型である必要があります。 埋め込み式は、要素の終了タグでは使用できません。

- `attributeList`

  省略可。 リテラルで宣言された属性のリスト。

  `attribute [ attribute ... ]`

  各 `attribute` には、次のいずれかの構文があります。

  - `[aPrefix:]aName=aValue`形式の属性の割り当て。次のようになります。

    |要素|説明|
    |---|---|
    |`aPrefix`|省略可。 属性の XML 名前空間プレフィックス。 は、`Imports` ステートメント、またはこの要素または親要素で定義されているローカル XML 名前空間で定義されているグローバル XML 名前空間である必要があります。|
    |`aName`|必須。 属性の名前です。 形式は、次のいずれかになります。<br /><br /> - リテラルテキスト。 「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」 を参照してください。<br />- フォーム `<%= aNameExp %>` の埋め込み式。 `aNameExp` の型は `String` であるか、または <xref:System.Xml.Linq.XName> に暗黙的に変換できる型である必要があります。|
    |`aValue`|省略可。 属性の値。 形式は、次のいずれかになります。<br /><br /> -リテラルテキスト (引用符で囲まれています)。<br />- フォーム `<%= aValueExp %>` の埋め込み式。 任意の型を使用できます。|

  - `<%= aExp %>`形式の埋め込み式。

- `/>`

  省略可。 要素がコンテンツのない空の要素であることを示します。

- `>`

  必須。 開始または空の要素タグを終了します。

- `elementContents`

  省略可。 要素の内容。

  `content [ content ... ]`

  各 `content` は、次のいずれかになります。

  - リテラルテキスト。 リテラルテキストがある場合、`elementContents` のすべての空白が有意になります。

  - `<%= contentExp %>`形式の埋め込み式。

  - XML 要素リテラル。

  - XML コメントリテラル。 「[XML コメントリテラル](../../../visual-basic/language-reference/xml-literals/xml-comment-literal.md)」を参照してください。

  - XML 処理命令リテラル。 「[XML 処理命令リテラル](../../../visual-basic/language-reference/xml-literals/xml-processing-instruction-literal.md)」を参照してください。

  - XML CDATA リテラル。 「 [XML CDATA Literal](../../../visual-basic/language-reference/xml-literals/xml-cdata-literal.md)」を参照してください。

- `</[name]>`

  省略可。 要素の終了タグを表します。 埋め込み式の結果である場合、省略可能な `name` パラメーターは使用できません。

## <a name="return-value"></a>戻り値

<xref:System.Xml.Linq.XElement> オブジェクト。

## <a name="remarks"></a>コメント

XML 要素リテラル構文を使用すると、コード内に <xref:System.Xml.Linq.XElement> オブジェクトを作成できます。

> [!NOTE]
> XML リテラルは、行連結文字を使用せずに、複数の行にまたがることができます。 この機能を使用すると、XML ドキュメントからコンテンツをコピーして、Visual Basic プログラムに直接貼り付けることができます。

フォーム `<%= exp %>` の埋め込み式を使用すると、XML 要素リテラルに動的な情報を追加できます。 詳細については、「 [XML での埋め込み式](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)」を参照してください。

Visual Basic コンパイラは、XML 要素リテラルを <xref:System.Xml.Linq.XElement.%23ctor%2A> コンストラクターの呼び出しに変換します。また、必要に応じて、<xref:System.Xml.Linq.XAttribute.%23ctor%2A> コンストラクターを呼び出します。

## <a name="xml-namespaces"></a>XML 名前空間

XML 名前空間プレフィックスは、同じ名前空間の要素を持つ XML リテラルをコード内で何回も作成する必要がある場合に便利です。 グローバル XML 名前空間プレフィックスは、`Imports` ステートメントを使用して定義します。または、`xmlns:xmlPrefix="xmlNamespace"` 属性構文を使用して定義するローカルプレフィックスを使用することもできます。 詳細については、「 [Imports ステートメント (XML 名前空間)](../../../visual-basic/language-reference/statements/imports-statement-xml-namespace.md)」を参照してください。

XML 名前空間のスコープ規則に従って、ローカルプレフィックスはグローバルプレフィックスよりも優先されます。 ただし、xml リテラルで XML 名前空間が定義されている場合、その名前空間は、埋め込み式に出現する式では使用できません。 埋め込み式は、グローバル XML 名前空間のみにアクセスできます。

Visual Basic コンパイラは、XML リテラルによって使用される各グローバル XML 名前空間を、生成されたコード内の1つのローカル名前空間定義に変換します。 使用されていないグローバル XML 名前空間は、生成されたコードには表示されません。

## <a name="example"></a>例

次の例は、2つの入れ子になった空の要素を持つ単純な XML 要素を作成する方法を示しています。

[!code-vb[VbXMLSamples#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples9.vb#20)]

この例では、次のテキストが表示されます。 リテラルは空の要素の構造を保持することに注意してください。

```xml
<outer>
  <inner1></inner1>
  <inner2 />
</outer>
```

## <a name="example"></a>例

次の例では、埋め込み式を使用して要素に名前を指定し、属性を作成する方法を示します。

[!code-vb[VbXMLSamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples9.vb#21)]

このコードを実行すると、次のテキストが表示されます。

```xml
<book isbn="1234" author="My Author" year="1999" title="My Book" />
```

## <a name="example"></a>例

次の例では、`ns` を名前空間プレフィックスとして宣言します。 次に、名前空間のプレフィックスを使用して XML リテラルを作成し、要素の最終的な形式を表示します。

[!code-vb[VbXMLSamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples10.vb#22)]

このコードを実行すると、次のテキストが表示されます。

```xml
<ns:outer xmlns:ns="http://SomeNamespace">
  <ns:middle xmlns:ns="http://NewNamespace">
    <ns:inner1 />
    <inner2 xmlns="http://SomeNamespace" />
  </ns:middle>
</ns:outer>
```

コンパイラによって、グローバル XML 名前空間のプレフィックスが XML 名前空間のプレフィックス定義に変換されていることに注意してください。 \<ns: ミドル > 要素は、\<ns: inner1 > 要素の XML 名前空間プレフィックスを再定義します。 ただし、\<ns: inner2 > 要素は、`Imports` ステートメントで定義されている名前空間を使用します。

## <a name="see-also"></a>参照

- <xref:System.Xml.Linq.XElement>
- [宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
- [XML コメント リテラル](../../../visual-basic/language-reference/xml-literals/xml-comment-literal.md)
- [XML CDATA リテラル](../../../visual-basic/language-reference/xml-literals/xml-cdata-literal.md)
- [XML リテラル](../../../visual-basic/language-reference/xml-literals/index.md)
- [Visual Basic での XML の作成](../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML での埋め込み式](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [Imports ステートメント (XML 名前空間)](../../../visual-basic/language-reference/statements/imports-statement-xml-namespace.md)
