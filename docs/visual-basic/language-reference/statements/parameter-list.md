---
title: パラメーター リスト
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- parameters [Visual Basic], Visual Basic
- parameters [Visual Basic], lists
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- arguments [Visual Basic], Visual Basic
- procedures [Visual Basic], parameter lists
ms.assetid: 5d737319-0c34-4df9-a23d-188fc840becd
ms.openlocfilehash: ec4ce0f12b540478d889832fb18f1ef008613f1f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346480"
---
# <a name="parameter-list-visual-basic"></a>パラメーター リスト (Visual Basic)

プロシージャが呼び出されるときに予期されるパラメーターを指定します。 複数のパラメーターはコンマで区切られます。 1つのパラメーターの構文を次に示します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ Optional ] [{ ByVal | ByRef }] [ ParamArray ]
parametername[( )] [ As parametertype ] [ = defaultvalue ]
```

## <a name="parts"></a>指定項目

`attributelist`  
省略可。 このパラメーターに適用される属性の一覧。 [属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)は山かっこ ("`<`" と "`>`") で囲む必要があります。

`Optional`  
省略可。 プロシージャを呼び出すときに、このパラメーターが必要ないことを指定します。

`ByVal`  
省略可。 プロシージャが、呼び出し元のコードの対応する引数の基になる変数要素を置換または再割り当てできないことを指定します。

`ByRef`  
省略可。 プロシージャが、呼び出し元のコード自体と同じように、呼び出し元のコード内の基になる変数要素を変更できることを指定します。

`ParamArray`  
省略可。 パラメーターリストの最後のパラメーターが、指定されたデータ型の要素のオプションの配列であることを指定します。 これにより、呼び出し元のコードは、プロシージャに任意の数の引数を渡すことができます。

`parametername`  
必須。 パラメーターを表すローカル変数の名前。

`parametertype`  
`Option Strict` が `On`の場合は必須です。 パラメーターを表すローカル変数のデータ型。

`defaultvalue`  
`Optional` パラメーターに必要です。 パラメーターのデータ型に評価される定数または定数式。 型が `Object`、クラス、インターフェイス、配列、または構造体の場合、既定値は `Nothing`のみ可能です。

## <a name="remarks"></a>コメント

パラメーターは、かっこで囲まれ、コンマで区切られます。 パラメーターは任意のデータ型で宣言できます。 `parametertype`を指定しない場合、既定で `Object`が使用されます。

呼び出し元のコードがプロシージャを呼び出すと、必要な各パラメーターに*引数*が渡されます。 詳細については、「[パラメーターと引数の違い](../../../visual-basic/programming-guide/language-features/procedures/differences-between-parameters-and-arguments.md)」を参照してください。

呼び出し元のコードが各パラメーターに渡す引数は、呼び出し元のコード内の基になる要素へのポインターです。 この要素が*不変* (定数、リテラル、列挙型、または式) の場合、どのコードでも変更することはできません。 *変数*要素 (宣言された変数、フィールド、プロパティ、配列要素、または構造体要素) の場合は、呼び出し元のコードで変更できます。 詳細については、「[変更できる引数と変更できない引数の違い](../../../visual-basic/programming-guide/language-features/procedures/differences-between-modifiable-and-nonmodifiable-arguments.md)」を参照してください。

変数要素が `ByRef`渡された場合は、プロシージャでも変更できます。 詳細については、「[引数を値で渡す方法と参照渡しの違い](../../../visual-basic/programming-guide/language-features/procedures/differences-between-passing-an-argument-by-value-and-by-reference.md)」を参照してください。

## <a name="rules"></a>ルール

- **かっこ。 ** パラメーターリストを指定する場合は、かっこで囲む必要があります。 パラメーターがない場合でも、空のリストを囲むかっこを使用できます。 これにより、要素がプロシージャであることを明確にすることで、コードの読みやすさが向上します。

- **省略可能なパラメーター。** パラメーターで `Optional` 修飾子を使用する場合は、リスト内の後続のすべてのパラメーターも省略可能であり、`Optional` 修飾子を使用して宣言されている必要があります。

     省略可能なすべてのパラメーター宣言では、`defaultvalue` 句を指定する必要があります。

     詳細については、「[省略可能なパラメーター](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)」を参照してください。

- **パラメーター配列。** `ParamArray` パラメーターには `ByVal` を指定する必要があります。

     同じパラメーターリストで `Optional` と `ParamArray` の両方を使用することはできません。

     詳細については、「[パラメーター配列](../../../visual-basic/programming-guide/language-features/procedures/parameter-arrays.md)」を参照してください。

- **引渡し方法。** すべての引数の既定の機構は `ByVal`です。これは、プロシージャが基になる変数要素を変更できないことを意味します。 ただし、要素が参照型の場合、このプロシージャでは、オブジェクト自体を置き換えることも再割り当てできない場合でも、基になるオブジェクトの内容やメンバーを変更できます。

- **パラメーター名。** パラメーターのデータ型が配列である場合は、`parametername` の直後にかっこを入力します。 パラメーター名の詳細については、「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

## <a name="example"></a>例

次の例は、2つのパラメーターを定義する `Function` プロシージャを示しています。

[!code-vb[VbVbalrStatements#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#2)]

## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)
- [方法 : コード内でステートメントを分割および連結する](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
