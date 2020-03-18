---
title: Nothing キーワード
ms.date: 07/20/2015
f1_keywords:
- Nothing
- vb.Nothing
helpviewer_keywords:
- Nothing keyword [Visual Basic]
- Nothing keyword [Visual Basic], syntax
ms.assetid: 06176e2d-bbf7-4a37-afaa-a86ad21ee99f
ms.openlocfilehash: 3bd4681341a33cc8db4ecbc2b284be243db56549
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344174"
---
# <a name="nothing-keyword-visual-basic"></a>Nothing キーワード (Visual Basic)

任意のデータ型の既定値を表します。 参照型の場合、既定値は `null` 参照です。 値型の場合、既定値は、値型が null 値を許容するかどうかによって異なります。

> [!NOTE]
> Null 非許容値型の場合、Visual Basic の `Nothing` はのC#`null` と異なります。 Visual Basic で、null 非許容の値型の変数を `Nothing`に設定すると、変数は宣言された型の既定値に設定されます。 でC#は、null 非許容の値型の変数を `null`に割り当てると、コンパイル時エラーが発生します。

## <a name="remarks"></a>コメント

`Nothing` は、データ型の既定値を表します。 既定値は、変数が値型であるか、参照型であるかによって異なります。


*値型*の変数には、その値が直接含まれています。値型には、すべての数値データ型、`Boolean`、`Char`、`Date`、すべての構造体、すべての列挙が含まれます。*参照型*の変数は、メモリ内のオブジェクトのインスタンスへの参照を格納します。参照型には、クラス、配列、デリゲート、および文字列が含まれます。詳細については、「[値型と参照型](../programming-guide/language-features/data-types/value-types-and-reference-types.md)」を参照してください。

変数が値型の場合、`Nothing` の動作は、変数が null 許容型かどうかによって異なります。Null 許容値型を表すには、型名に `?` 修飾子を追加します。`Nothing` を null 許容変数に割り当てると、値が `null`に設定されます。詳細と例については、「[Null 許容値型](../programming-guide/language-features/data-types/nullable-value-types.md)」を参照してください。


変数が null 値が許容されない値型である場合、その変数に `Nothing` を割り当てると、宣言された型の既定値に設定されます。 その型に変数メンバーが含まれている場合は、すべてが既定値に設定されます。 次の例では、スカラー型について説明します。

[!code-vb[VbVbalrKeywords#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class2.vb#7)]

変数が参照型の場合、変数に `Nothing` を割り当てると、変数の型の `null` 参照に設定されます。 `null` 参照に設定されている変数は、どのオブジェクトにも関連付けられていません。 この動作を次の例で示します。

[!code-vb[VbVbalrKeywords#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class3.vb#8)]

参照 (または null 許容値型) 変数が `null`かどうかを確認する場合は、`= Nothing` または `<> Nothing`を使用しないでください。 常に `Is Nothing` または `IsNot Nothing`を使用します。

Visual Basic 内の文字列の場合、空の文字列は `Nothing`と等しくなります。 したがって、`"" = Nothing` は true になります。

次の例では、`Is` 演算子と `IsNot` 演算子を使用する比較を示します。

[!code-vb[VbVbalrKeywords#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class4.vb#9)]

`As` 句を使用せずに変数を宣言し、それを `Nothing`に設定すると、変数の型は `Object`になります。 この例としては、`Dim something = Nothing`があります。 この場合、`Option Strict` がオンで `Option Infer` がオフになっていると、コンパイル時エラーが発生します。

オブジェクト変数に `Nothing` を割り当てると、オブジェクトインスタンスを参照しなくなります。 変数が以前にインスタンスを参照していた場合、その変数を `Nothing` に設定しても、インスタンス自体は終了しません。 インスタンスが終了し、そのインスタンスに関連付けられているメモリおよびシステムリソースが解放されるのは、ガベージコレクター (GC) によってアクティブな参照が残っていないことが検出された後のみです。

`Nothing` は、初期化されていない variant または存在しないデータベース列を表す <xref:System.DBNull> オブジェクトとは異なります。

## <a name="see-also"></a>参照

- [Dim ステートメント](./statements/dim-statement.md)
- [オブジェクトの有効期間 : オブジェクトの作成と破棄](../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
- [Visual Basic の有効期間](../programming-guide/language-features/declared-elements/lifetime.md)
- [Is 演算子](./operators/is-operator.md)
- [IsNot 演算子](./operators/isnot-operator.md)
- [null 許容値型](../programming-guide/language-features/data-types/nullable-value-types.md)
