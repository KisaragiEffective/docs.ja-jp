---
title: Imports ステートメント-XML 名前空間
ms.date: 07/20/2015
f1_keywords:
- vb.ImportsXmlns
helpviewer_keywords:
- XML namespace [Visual Basic], importing
- imports [Visual Basic]
- Imports statement [Visual Basic]
- namespaces [Visual Basic], importing
ms.assetid: 1f4d50a6-08c7-4c2e-8206-ccae35fcd1b4
ms.openlocfilehash: 52864e4d1c8183b6243025e72368d23627049c84
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351060"
---
# <a name="imports-statement-xml-namespace"></a>Imports ステートメント (XML 名前空間)

Xml リテラルおよび XML 軸プロパティで使用する XML 名前空間プレフィックスをインポートします。

## <a name="syntax"></a>構文

```vb
Imports <xmlns:xmlNamespacePrefix = "xmlNamespaceName">
```

## <a name="parts"></a>指定項目

`xmlNamespacePrefix`  
省略可能。 XML 要素と属性が `xmlNamespaceName`を参照できる文字列です。 `xmlNamespacePrefix` が指定されていない場合、インポートされた XML 名前空間が既定の XML 名前空間になります。 有効な XML 識別子である必要があります。 詳細については、「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」を参照してください。

`xmlNamespaceName`  
必須。 インポートされる XML 名前空間を識別する文字列。

## <a name="remarks"></a>コメント

`Imports` ステートメントを使用しようすると、XML リテラルや XML 軸プロパティで使用できるグローバル XML 名前空間の定義や、`GetXmlNamespace` 演算子に渡されるパラメーターとしての定義ができます。 (`Imports` ステートメントを使用して、コードで型名が使用されている場合に使用できるエイリアスをインポートする方法については、「[Imports ステートメント (.NET 名前空間および型)](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)」を参照してください。`Imports` ステートメントを使用して XML 名前空間を宣言する構文は、XML で使用される構文と同じです。 したがって、XML ファイルから名前空間宣言をコピーし、それを `Imports` ステートメントで使用することができます。

XML 名前空間プレフィックスは、同じ名前空間にある XML 要素を繰り返し作成する場合に便利です。 `Imports` ステートメントで宣言された XML 名前空間プレフィックスは、ファイル内のすべてのコードで使用できるという意味でグローバルです。 XML 要素リテラルを作成するとき、および XML 軸プロパティにアクセスするときに使用できます。 詳細については、「[XML 要素リテラル](../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)」および「[XML 軸のプロパティ](../../../visual-basic/language-reference/xml-axis/index.md)」を参照してください。

名前空間プレフィックスを使用せずにグローバル XML 名前空間を定義した場合 (`Imports <xmlns="http://SomeNameSpace>"`など)、その名前空間は既定の XML 名前空間と見なされます。 既定の XML 名前空間は、名前空間を明示的に指定していない XML 要素リテラルまたは XML 属性軸プロパティに使用されます。 既定の名前空間は、指定された名前空間が空の名前空間 (つまり、`xmlns=""`) の場合にも使用されます。 既定の XML 名前空間は、xml リテラルの XML 属性、または名前空間を持たない xml 属性軸プロパティには適用されません。

Xml リテラルで定義されている xml 名前空間 (*ローカル xml 名前空間*と呼ばれます) は、`Imports` ステートメントでグローバルとして定義されている xml 名前空間よりも優先されます。 `Imports` ステートメントで定義された XML 名前空間は、Visual Basic プロジェクト用にインポートされた XML 名前空間よりも優先されます。 Xml リテラルで XML 名前空間が定義されている場合、そのローカル名前空間は埋め込み式には適用されません。

グローバル XML 名前空間は .NET Framework 名前空間と同じスコープおよび定義規則に従います。 その結果、.NET Framework 名前空間をインポートできる場所であればどこでも、`Imports` ステートメントを使用してグローバル XML 名前空間を定義できます。 これには、コードファイルとプロジェクトレベルでインポートされた名前空間の両方が含まれます。 プロジェクトレベルでインポートされた名前空間の詳細については、「[プロジェクトデザイナー (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)」を参照してください。

各ソースファイルには、任意の数の `Imports` ステートメントを含めることができます。 これらは、`Option Strict` ステートメントなどのオプション宣言に従う必要があります。また、`Module` や `Class` ステートメントなどのプログラミング要素の宣言の前に記述する必要があります。

## <a name="example"></a>例

次の例では、既定の XML 名前空間と、プレフィックス `ns`で識別される XML 名前空間をインポートします。 次に、両方の名前空間を使用する XML リテラルを作成します。

[!code-vb[VbXMLSamples#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/Module1.vb#45)]

このコードを実行すると、次のテキストが表示されます。

```xml
<ns:outer xmlns="http://DefaultNamespace"
          xmlns:ns="http://NewNamespace">
  <ns:innerElement></ns:innerElement>
  <siblingElement></siblingElement>
  <innerElement />
</ns:outer>
```

## <a name="example"></a>例

次の例では、XML 名前空間プレフィックス `ns`をインポートします。 次に、名前空間プレフィックスを使用する XML リテラルを作成し、要素の最終的な形式を表示します。

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

コンパイラによって、XML 名前空間プレフィックスがグローバルプレフィックスからローカルプレフィックス定義に変換されたことに注意してください。

## <a name="example"></a>例

次の例では、XML 名前空間プレフィックス `ns`をインポートします。 その後、この名前空間のプレフィックスを使用して XML リテラルを作成し、修飾名 `ns:name` を持つ最初の子ノードにアクセスします。

[!code-vb[VbXMLSamples#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples8.vb#19)]

このコードを実行すると、次のテキストが表示されます。

`Patrick Hines`

## <a name="see-also"></a>参照

- [XML 要素リテラル](../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
- [XML 軸プロパティ](../../../visual-basic/language-reference/xml-axis/index.md)
- [宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
- [GetXmlNamespace 演算子](../../../visual-basic/language-reference/operators/getxmlnamespace-operator.md)
