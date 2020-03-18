---
title: Select...Case ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Select
- vb.Case
helpviewer_keywords:
- Select statement [Visual Basic]
- Case statement [Visual Basic]
- Select...Case statements
- conditional statements [Visual Basic], Select Case
- control flow [Visual Basic], branching
- Else keyword [Visual Basic], in Select...Case statements
- execution [Visual Basic], conditional
- To keyword [Visual Basic], in Select...Case statements
- Select Case statement [Visual Basic], Select...Case
- Select statement [Visual Basic], Select...Case
- Is operator [Visual Basic], in Select...Case statements
- branching [Visual Basic], conditional
- Case Else statement [Visual Basic], Select...Case
- End keyword [Visual Basic], Select Case statements
- Case statement [Visual Basic], Select...Case
ms.assetid: 68877b65-5419-4bf0-a465-20cd0e4c7d44
ms.openlocfilehash: 4dddfe5fbf7092c911291ffc6841e76f8c2748e9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352828"
---
# <a name="selectcase-statement-visual-basic"></a>Select...Case ステートメント (Visual Basic)
式の値に応じて、いくつかのステートメントグループのうちの1つを実行します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Select [ Case ] testexpression  
    [ Case expressionlist  
        [ statements ] ]  
    [ Case Else  
        [ elsestatements ] ]  
End Select  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|定義|  
|---|---|  
|`testexpression`|必須。 条件. は、基本データ型 (`Boolean`、`Byte`、`Char`、`Date`、`Double`、`Decimal`、`Integer`、`Long`、`Object`、`SByte`、`Short`、`Single`) のいずれかに評価される必要があります。`String``UInteger``ULong``UShort`|  
|`expressionlist`|`Case` ステートメントでは必須です。 `testexpression`の一致値を表す式の句の一覧。 複数の式句は、コンマで区切ります。 各句には、次のいずれかの形式を使用できます。<br /><br /> -   *expression1* `To` *expression2*<br />-[`Is`] *comparisonoperator* *式*<br />-   *式*<br /><br /> `To` キーワードを使用して、`testexpression`の一致値の範囲の境界を指定します。 `expression1` の値は `expression2`の値以下である必要があります。<br /><br /> 比較演算子 (`=`、`<>`、`<`、`<=`、`>`、または `>=`) と共に `Is` キーワードを使用して、`testexpression`の一致値に制限を指定します。 `Is` キーワードが指定されていない場合は、 *comparisonoperator*の前に自動的に挿入されます。<br /><br /> `expression` のみを指定するフォームは、`Is` 形式の特殊なケースとして扱われます。ここで、 *comparisonoperator*は等号 (`=`) です。 このフォームは `testexpression` = `expression`として評価されます。<br /><br /> `expressionlist` の式は、任意のデータ型にすることができます。 `testexpression` の型に暗黙的に変換可能であり、適切な `comparisonoperator` が使用されている2つの型に対して有効であることが条件となります。|  
|`statements`|任意。 `testexpression` が `expressionlist`内のいずれかの句と一致する場合に実行される `Case` に続く1つ以上のステートメント。|  
|`elsestatements`|任意。 `testexpression` が `Case` ステートメントの `expressionlist` 内のどの句とも一致しない場合に実行される `Case Else` に続く1つ以上のステートメント。|  
|`End Select`|`Select`...`Case` の構築の定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 `testexpression` が `Case` `expressionlist` 句と一致する場合、その `Case` ステートメントに続くステートメントは、次の `Case`、`Case Else`、または `End Select` ステートメントまで実行されます。 次に、コントロールは `End Select`に続くステートメントにを渡します。 `testexpression` が1つ以上の `Case` 句の `expressionlist` 句と一致する場合、最初の一致を実行した後のステートメントのみが実行されます。  
  
 `Case Else` ステートメントを使用して、他の `Case` ステートメントで `testexpression` と `expressionlist` 句の間に一致が検出されなかった場合に実行する `elsestatements` を導入します。 必須ではありませんが、予期しない `testexpression` 値を処理するために、`Select Case` の構築に `Case Else` ステートメントを用意することをお勧めします。 `Case` `expressionlist` 句が `testexpression` に一致せず、`Case Else` ステートメントが存在しない場合、制御は `End Select`に続くステートメントに渡されます。  
  
 各 `Case` 句では、複数の式または範囲を使用できます。 たとえば、次の行は有効です。  
  
 `Case 1 To 4, 7 To 9, 11, 13, Is > maxNumber`  
  
> [!NOTE]
> `Case` および `Case Else` ステートメントで使用されている `Is` キーワードは、オブジェクト参照の比較に使用される[Is 演算子](../../../visual-basic/language-reference/operators/is-operator.md)と同じではありません。  
  
 文字列の範囲と複数の式を指定できます。 次の例では、`Case` は、"リンゴ" と完全に等価な任意の文字列と、アルファベット順の "ナット" と "スープ" の間の値を持つか、`testItem`の現在の値とまったく同じ値を含んでいます。  
  
 `Case "apples", "nuts" To "soup", testItem`  
  
 `Option Compare` の設定は、文字列比較に影響を与える可能性があります。 `Option Compare Text`では、文字列 "りんご" と "りんご" は等しいとして比較されますが、`Option Compare Binary`の下では、これらは等しくありません。  
  
> [!NOTE]
> 複数の句を持つ `Case` ステートメントでは、*ショートサーキット*と呼ばれる動作を使用できます。 Visual Basic では、句が左から右に評価されます。一方が `testexpression`と一致する場合、残りの句は評価されません。 ショートサーキットはパフォーマンスを向上させることができますが、`expressionlist` 内のすべての式を評価する必要がある場合は、予期しない結果が生じる可能性があります。 ショートサーキットの詳細については、「[ブール式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)」を参照してください。  
  
 `Case` または `Case Else` ステートメントブロック内のコードが、ブロック内のステートメントを実行する必要がない場合は、`Exit Select` ステートメントを使用してブロックを終了できます。 これにより、`End Select`に続くステートメントに制御が直ちに移ります。  
  
 `Select Case` の構造は入れ子にすることができます。 入れ子になった `Select Case` 構築にはそれぞれ、一致する `End Select` ステートメントが必要です。また、入れ子になっている外側の `Select Case` 構築の1つの `Case` または `Case Else` ステートメントブロック内に完全に含まれている必要があります。  
  
## <a name="example"></a>例  
 次の例では、`Select Case` 構築を使用して、変数 `number`の値に対応する行を書き込みます。 2番目の `Case` ステートメントには、`number`の現在の値と一致する値が含まれています。そのため、"Between 6 ~ 8" を含むステートメントが実行されます。  
  
 [!code-vb[VbVbalrStatements#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#54)]  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Interaction.Choose%2A>
- [End ステートメント](../../../visual-basic/language-reference/statements/end-statement.md)
- [If...Then...Else ステートメント](../../../visual-basic/language-reference/statements/if-then-else-statement.md)
- [Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
