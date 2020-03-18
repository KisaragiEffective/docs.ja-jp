---
title: '方法 : 値の変わらない変数を作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], read-only
- variables [Visual Basic], constant value
ms.assetid: 86b59266-25df-4635-ae15-9b59c411d036
ms.openlocfilehash: d5d8a6b066ae7e8795afd2f788b60823d8efdafa
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348643"
---
# <a name="how-to-create-a-variable-that-does-not-change-in-value-visual-basic"></a>方法: 値の変わらない変数を作成する (Visual Basic)

値を変更しない変数の概念は、矛盾しているように見えることがあります。 しかし、定数を使用できない場合は、固定値を持つ変数を使用すると便利です。 このような場合は、 [ReadOnly](../../../../visual-basic/language-reference/modifiers/readonly.md)キーワードを使用してメンバー変数を定義できます。

次のような状況では、 [Const ステートメント](../../../../visual-basic/language-reference/statements/const-statement.md)を使用して定数値を宣言して割り当てることはできません。

- `Const` ステートメントは、使用するデータ型を受け入れません。

- コンパイル時に値がわからない

- コンパイル時に定数値を計算できません

### <a name="to-create-a-variable-that-does-not-change-in-value"></a>値を変更しない変数を作成するには

1. モジュールレベルで、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用してメンバー変数を宣言し、 [ReadOnly](../../../../visual-basic/language-reference/modifiers/readonly.md)キーワードを含めます。

    ```vb
    Dim ReadOnly timeStarted
    ```

    `ReadOnly` は、メンバー変数に対してのみ指定できます。 これは、プロシージャの外部でモジュールレベルで変数を定義する必要があることを意味します。

2. コンパイル時に1つのステートメントで値を計算できる場合は、`Dim` ステートメントで初期化句を使用します。 [As](../../../../visual-basic/language-reference/statements/as-clause.md)句に続けて等号 (`=`) を指定し、その後に式を指定します。 コンパイラがこの式を定数値に評価できることを確認してください。

    ```vb
    Dim ReadOnly timeStarted As Date = Now
    ```

    `ReadOnly` 変数に値を割り当てることができるのは1回だけです。 その後、コードで値を変更することはできません。

    コンパイル時に値がわからない場合、またはコンパイル時に1つのステートメントで値を計算できない場合でも、コンストラクターで実行時に割り当てることができます。 これを行うには、クラスまたは構造レベルで `ReadOnly` 変数を宣言する必要があります。 そのクラスまたは構造体のコンストラクターで、変数の固定値を計算し、コンストラクターから戻る前に変数に代入します。

## <a name="see-also"></a>参照

- [WriteOnly](../../../../visual-basic/language-reference/modifiers/writeonly.md)
- [Const ステートメント](../../../../visual-basic/language-reference/statements/const-statement.md)
