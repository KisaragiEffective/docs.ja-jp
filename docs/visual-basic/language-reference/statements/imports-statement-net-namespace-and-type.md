---
title: Imports ステートメント-.NET 名前空間と型
ms.date: 07/20/2015
f1_keywords:
- vb.Imports
- imports
helpviewer_keywords:
- declared element names [Visual Basic], qualification
- imports [Visual Basic]
- Imports statement [Visual Basic]
- aliases [Visual Basic], Imports statement
- container elements [Visual Basic]
- namespaces [Visual Basic], importing
- Imports statement [Visual Basic], syntax
- import aliases [Visual Basic]
- aliases [Visual Basic], import
- declared elements [Visual Basic], container elements
ms.assetid: 7062f8aa-d890-4232-9eed-92836e13fb6e
ms.openlocfilehash: 39fa4e74f973bcb575b5751c387c0b879f4e398d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351066"
---
# <a name="imports-statement-net-namespace-and-type"></a>Imports ステートメント (.NET 名前空間および型)

名前空間の修飾なしで型名を参照できるようにします。

## <a name="syntax"></a>構文

```vb
Imports [ aliasname = ] namespace
' -or-
Imports [ aliasname = ] namespace.element
```

## <a name="parts"></a>指定項目

|用語|定義|
|---|---|
|`aliasname`|任意。 完全修飾文字列ではなく、コードが `namespace` を参照できる*インポートエイリアス*または名前。 「[宣言された要素の名前](../../programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
|`namespace`|必須。 インポートされる名前空間の完全修飾名。 には、任意のレベルに入れ子になった名前空間の文字列を指定できます。|
|`element`|任意。 名前空間で宣言されているプログラミング要素の名前。 任意のコンテナー要素を指定できます。|

## <a name="remarks"></a>コメント

`Imports` ステートメントを使用すると、特定の名前空間に含まれている型を直接参照できます。

1つの名前空間名または入れ子になった名前空間の文字列を指定できます。 入れ子になった各名前空間は、次の例に示すように、ピリオド (`.`) で次の上位レベルの名前空間から分離されます。

```vb
Imports System.Collections.Generic
```

各ソースファイルには、任意の数の `Imports` ステートメントを含めることができます。 これらは、`Option Strict` ステートメントなど、任意のオプション宣言に従う必要があり、`Module` や `Class` ステートメントなどのプログラミング要素の宣言の前に記述する必要があります。

`Imports` は、ファイルレベルでのみ使用できます。 つまり、インポートの宣言コンテキストはソースファイルである必要があり、名前空間、クラス、構造体、モジュール、インターフェイス、プロシージャ、またはブロックにすることはできません。

`Imports` ステートメントでは、プロジェクトで他のプロジェクトおよびアセンブリの要素を使用できないことに注意してください。 インポートでは、参照の設定は行われません。 これにより、プロジェクトで既に使用可能な名前を修飾する必要がなくなります。 詳細については、「[宣言された要素の参照](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)」の「コンテナー要素のインポート」を参照してください。

> [!NOTE]
> 暗黙の `Imports` ステートメントを定義するには、[[参照] ページの [プロジェクトデザイナー] (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)を使用します。 詳細については、「[方法: インポートされた名前空間を追加または削除する (Visual Basic)](/visualstudio/ide/how-to-add-or-remove-imported-namespaces-visual-basic)」を参照してください。

## <a name="import-aliases"></a>インポート エイリアス

*インポートエイリアス*は、名前空間または型のエイリアスを定義します。 インポートエイリアスは、1 つまたは複数の名前空間で宣言されているものと同じ名前の項目を使用する必要がある場合に便利です。 詳細と例については、「[宣言された要素の参照](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)」の「要素名の修飾」を参照してください。

モジュールレベルで `aliasname`と同じ名前のメンバーを宣言することはできません。 この場合、Visual Basic コンパイラは、宣言されたメンバーに対してのみ `aliasname` を使用し、それをインポートエイリアスとして認識しなくなります。

インポートエイリアスの宣言に使用される構文は、XML 名前空間プレフィックスのインポートに使用される構文と似ていますが、結果は異なります。 インポートエイリアスは、コード内で式として使用できます。一方、XML 名前空間プレフィックスは、xml リテラルまたは XML 軸プロパティで、修飾された要素名または属性名のプレフィックスとしてのみ使用できます。

### <a name="element-names"></a>要素の名前

`element`を指定する場合は、*コンテナー要素*、つまり、他の要素を含むことができるプログラミング要素を表す必要があります。 コンテナー要素には、クラス、構造体、モジュール、インターフェイス、および列挙体が含まれます。

`Imports` ステートメントで使用できる要素のスコープは、`element`を指定したかどうかによって異なります。 `namespace`だけを指定した場合、その名前空間の一意の名前を持つすべてのメンバーと、その名前空間内のコンテナー要素のメンバーは、修飾なしで使用できます。 `namespace` と `element`の両方を指定すると、その要素のメンバーだけが修飾なしで使用できます。

## <a name="example"></a>例

次の例では、<xref:System.IO.DirectoryInfo> クラスを使用して、 *C:\\* ディレクトリ内のすべてのフォルダーを返します。

コードには、ファイルの先頭に `Imports` ステートメントがありません。 したがって、<xref:System.IO.DirectoryInfo>、<xref:System.Text.StringBuilder>、および <xref:Microsoft.VisualBasic.ControlChars.CrLf> 参照はすべて、名前空間で完全修飾されています。

[!code-vb[VbVbalrStatements#152](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#152)]

## <a name="example"></a>例

次の例には、参照先の名前空間の `Imports` ステートメントが含まれています。 そのため、型は名前空間で完全修飾されている必要はありません。

[!code-vb[VbVbalrStatements#153](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#153)]

[!code-vb[VbVbalrStatements#154](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#154)]
  
## <a name="example"></a>例

次の例には、参照先の名前空間のエイリアスを作成する `Imports` ステートメントが含まれています。 型はエイリアスで修飾されます。

[!code-vb[VbVbalrStatements#155](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#155)]

[!code-vb[VbVbalrStatements#156](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#156)]

## <a name="example"></a>例

次の例には、参照される型のエイリアスを作成する `Imports` ステートメントが含まれています。 エイリアスは、型を指定するために使用されます。

[!code-vb[VbVbalrStatements#157](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#157)]

[!code-vb[VbVbalrStatements#158](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class12.vb#158)]
  
## <a name="see-also"></a>関連項目

- [Namespace ステートメント](namespace-statement.md)
- [Visual Basic 内の名前空間](../../programming-guide/program-structure/namespaces.md)
- [参照と Imports ステートメント](../../programming-guide/program-structure/references-and-the-imports-statement.md)
- [Imports ステートメント (XML 名前空間)](imports-statement-xml-namespace.md)
- [宣言された要素の参照](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
