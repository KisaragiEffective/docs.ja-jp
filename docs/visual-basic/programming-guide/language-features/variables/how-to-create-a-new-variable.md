---
title: '方法 : 新しい変数を作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- Dim statement [Visual Basic]
- variables [Visual Basic], creating
ms.assetid: 35300be3-77b0-4bef-a156-034d3cdedde0
ms.openlocfilehash: 2a2b5b8bef3b66f9727f0e65b61882186c007e94
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353631"
---
# <a name="how-to-create-a-new-variable-visual-basic"></a>方法: 新しい変数を作成する (Visual Basic)

変数を作成するには、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用します。

### <a name="to-create-a-new-variable"></a>新しい変数を作成するには

1. `Dim` ステートメントで変数を宣言します。

    ```vb
    Dim newCustomer
    ```

2. [Private](../../../../visual-basic/language-reference/modifiers/private.md)、 [Static](../../../../visual-basic/language-reference/modifiers/static.md)、 [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)、 [WithEvents](../../../../visual-basic/language-reference/modifiers/withevents.md)など、変数の特性の仕様を含めます。 詳細については、「[宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)」を参照してください。

    ```vb
    Public Static newCustomer
    ```

    宣言で他のキーワードを使用する場合、`Dim` キーワードは必要ありません。

3. 仕様に従って変数の名前を指定します。これは Visual Basic の規則と規則に従う必要があります。 詳細については、「[宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

    ```vb
    Public Static newCustomer
    ```

4. 名前の後に [As](../../../../visual-basic/language-reference/statements/as-clause.md) 句を使用して、変数のデータ型を指定します。

    ```vb
    Public Static newCustomer As Customer
    ```

    データ型を指定しない場合、既定値の `Object`が使用されます。

5. `As` 句に等号 (`=`) を付けて、等号に続けて変数の初期値を指定します。

    Visual Basic は、`Dim` ステートメントを実行するたびに、指定された値を変数に代入します。 初期値を指定しなかった場合、`Dim` ステートメントを含むコードを最初に入力したときに、変数のデータ型の既定の初期値が Visual Basic に割り当てられます。

    変数が参照型の場合は、`As` 句に [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md) キーワードを含めることで、そのクラスのインスタンスを作成できます。 `New`を使用しない場合、変数の初期値は [Nothing](../../../../visual-basic/language-reference/nothing.md) となります。

    ```vb
    Public Static newCustomer As New Customer
    ```

## <a name="see-also"></a>参照

- [変数](../../../../visual-basic/programming-guide/language-features/variables/index.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [宣言された要素の名前](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [ステートメント](../../../../visual-basic/language-reference/statements/index.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Option Infer ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)
