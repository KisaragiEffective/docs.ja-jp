---
title: If...Then...Else ステートメント
ms.date: 04/16/2018
f1_keywords:
- vb.ElseIf
- vb.Then
- vb.If
- vb.EndIf
helpviewer_keywords:
- ElseIf statement [Visual Basic], If...Then...Else
- Then statement [Visual Basic], If...Then...Else
- control flow [Visual Basic], branching
- execution [Visual Basic], conditional
- TypeOf...Is expression, and If...Then...Else statements
- If...Then...Else statements
- If statement [Visual Basic], If...Then...Else
- If statement [Visual Basic]
- Is operator [Visual Basic], in If...Then...Else statements
- If Operator [Visual Basic]
- branching [Visual Basic], conditional
- If function [Visual Basic], and If...Then...Else statements
- Else statement [Visual Basic]
ms.assetid: 790068a2-1307-4e28-8a72-be5ebda099e9
ms.openlocfilehash: f505755caeb9cc3cfeeb1ba83b6de15f48314103
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351163"
---
# <a name="ifthenelse-statement-visual-basic"></a>If...Then...Else ステートメント (Visual Basic)

式の値に応じてステートメント グループを条件付きで実行します。

## <a name="syntax"></a>構文

```vb
' Multiline syntax:
If condition [ Then ]
    [ statements ]
[ ElseIf elseifcondition [ Then ]
    [ elseifstatements ] ]
[ Else
    [ elsestatements ] ]
End If

' Single-line syntax:
If condition Then [ statements ] [ Else [ elsestatements ] ]
```

## <a name="quick-links-to-example-code"></a>コード例へのクイックリンク

この記事には、`If`...`Then`...`Else` ステートメントの使用方法を示す例がいくつか含まれています。

- [複数行構文の例](#multi-line)
- [入れ子になった構文の例](#nested)
- [単一行構文の例](#single-line)

## <a name="parts"></a>指定項目

`condition` \
必須。 条件. は、`True` または `False`、または `Boolean`に暗黙的に変換できるデータ型に評価される必要があります。

式が[Nothing](../../../visual-basic/language-reference/nothing.md)に評価される[null 許容](../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)型の `Boolean` 変数の場合、条件は式が `False`として扱われ、`ElseIf` ブロックが存在する場合は評価され、存在する場合は `Else` ブロックが実行されます。

`Then` \
単一行の構文では必須です。複数行の構文では省略可能です。

`statements` \
省略可。 `condition` が `True`に評価された場合に実行される `If`...`Then` の後に続く1つ以上のステートメント。

`elseifcondition` \
`ElseIf` が存在する場合は必須です。 条件. は、`True` または `False`、または `Boolean`に暗黙的に変換できるデータ型に評価される必要があります。

`elseifstatements` \
省略可。 `elseifcondition` が `True`に評価された場合に実行される `ElseIf`...`Then` の後に続く1つ以上のステートメント。

`elsestatements` \
省略可。 前の `condition` または `elseifcondition` 式が `True`に評価されなかった場合に実行される1つ以上のステートメント。

`End If` \
複数行バージョンの `If`...`Then`...`Else` ブロックを終了します。

## <a name="remarks"></a>コメント

### <a name="multiline-syntax"></a>複数行の構文

`If`...`Then`...`Else` ステートメントが検出されると、`condition` がテストされます。 `condition` が `True`場合、`Then` に続くステートメントが実行されます。 `condition` が `False`場合は、各 `ElseIf` ステートメント (存在する場合) が順番に評価されます。 `True` `elseifcondition` が見つかると、関連付けられた `ElseIf` の直後にあるステートメントが実行されます。 `elseifcondition` が `True`に評価されない場合、または `ElseIf` ステートメントが存在しない場合は、`Else` に続くステートメントが実行されます。 `Then`、`ElseIf`、または `Else`の後に続くステートメントを実行すると、次の `End If`に続くステートメントで実行が続行されます。

`ElseIf` 句と `Else` 句は両方とも省略可能です。 `If``Then`...`Else` ステートメントには、必要な数の `ElseIf` 句を指定できますが、`ElseIf` 句の後に `Else` 句を指定することはできません。 `If`...`Then`...`Else` ステートメントは、入れ子にすることができます。

複数行構文では、`If` ステートメントが1行目の唯一のステートメントである必要があります。 `ElseIf`、`Else`、および `End If` ステートメントの前には、行ラベルだけを付けることができます。 `If`...`Then`...`Else` ブロックは、`End If` ステートメントで終了する必要があります。

> [!TIP]
> [選択...Case ステートメント](../../../visual-basic/language-reference/statements/select-case-statement.md)は、複数の可能な値を含む単一の式を評価する場合に便利です。

### <a name="single-line-syntax"></a>単一行の構文

単一行構文を使用すると、コードを含む単一の条件を使用して、true の場合に実行できます。 ただし、複数行の構文では、より多くの構造と柔軟性が提供され、読み取り、保守、デバッグが簡単になります。

ステートメントが単一行の `If`であるかどうかを判断するために、`Then` キーワードの後に続くものは次のとおりです。 同じ行の `Then` の後にコメント以外のものがある場合、ステートメントは単一行の `If` ステートメントとして扱われます。 `Then` が存在しない場合は、複数行の `If`.`Then`..`Else`を開始する必要があります。

単一行構文では、`If`...`Then` 決定の結果として複数のステートメントを実行できます。 すべてのステートメントは、同じ行にあり、コロンで区切られている必要があります。

## <a name="multiline-syntax-example"></a>複数行構文の例

<a name="multi-line"></a>

次の例では、`If`...`Then`...`Else` ステートメントの複数行構文の使用方法を示します。

[!code-vb[VbVbalrStatements#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#101)]

## <a name="nested-syntax-example"></a>入れ子になった構文の例

<a name="nested"></a>

次の例には、入れ子になった `If`...`Then`...`Else` ステートメントが含まれています。

[!code-vb[VbVbalrStatements#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#102)]

## <a name="single-line-syntax-example"></a>単一行構文の例

<a name="single-line"></a>単一行構文の使用例を次に示します。

[!code-vb[VbVbalrStatements#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#103)]

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Interaction.Choose%2A>
- <xref:Microsoft.VisualBasic.Interaction.Switch%2A>
- [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)
- [Select...Case ステートメント](../../../visual-basic/language-reference/statements/select-case-statement.md)
- [入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)
- [条件判断構造](../../../visual-basic/programming-guide/language-features/control-flow/decision-structures.md)
- [Visual Basic の論理演算子とビット処理演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
- [If 演算子](../../../visual-basic/language-reference/operators/if-operator.md)
