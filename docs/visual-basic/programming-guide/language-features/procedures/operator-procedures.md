---
title: 演算子プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- overloaded operators [Visual Basic]
- operator overloading
- operator procedures
ms.assetid: 8c513d38-246b-4fb7-8b75-29e1364e555b
ms.openlocfilehash: b395f5fcf1b89bb49e55e207c4910e95f2aae69d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345999"
---
# <a name="operator-procedures-visual-basic"></a>演算子プロシージャ (Visual Basic)

演算子プロシージャは、定義したクラスまたは構造体の標準演算子 (`*`、`<>`、`And`など) の動作を定義する一連の Visual Basic ステートメントです。 これは、*演算子のオーバーロード*とも呼ばれます。

## <a name="when-to-define-operator-procedures"></a>演算子プロシージャを定義する場合

クラスまたは構造体を定義したら、そのクラスまたは構造体の型として変数を宣言できます。 このような変数は、式の一部として操作に参加する必要がある場合があります。 これを行うには、演算子のオペランドである必要があります。

Visual Basic は、基本データ型に対してのみ演算子を定義します。 1つまたは両方のオペランドがクラスまたは構造体の型である場合は、演算子の動作を定義できます。

詳細については、「[Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)」を参照してください。

## <a name="types-of-operator-procedure"></a>演算子プロシージャの種類

演算子プロシージャには、次のいずれかの型を指定できます。

- 引数がクラスまたは構造体の型である単項演算子の定義。

- 少なくとも1つの引数がクラスまたは構造体の型である二項演算子の定義。

- 引数がクラスまたは構造体の型である変換演算子の定義。

- クラスまたは構造体の型を返す変換演算子の定義。

 変換演算子は常に単項演算であり、定義する演算子として常に `CType` を使用します。

## <a name="declaration-syntax"></a>宣言の構文

演算子プロシージャを宣言する構文は次のとおりです。

```vb
Public Shared [Widening | Narrowing] Operator operatorsymbol ( operand1 [,  operand2 ]) As datatype

' Statements of the operator procedure.

End Operator
```

`Widening` または `Narrowing` キーワードは、型変換演算子でのみ使用します。 演算子シンボルは、型変換演算子の場合は常に[CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)です。

2つのオペランドを宣言して二項演算子を定義し、1つのオペランドを宣言して、単項演算子 (型変換演算子を含む) を定義します。 すべてのオペランドは `ByVal`として宣言する必要があります。

各オペランドは、[Sub プロシージャ](./sub-procedures.md)のパラメーターを宣言するのと同じ方法で宣言します。

### <a name="data-type"></a>データ型

定義したクラスまたは構造体に演算子を定義しているため、少なくとも1つのオペランドがそのクラスまたは構造体のデータ型である必要があります。 型変換演算子の場合、オペランドまたは戻り値の型は、クラスまたは構造体のデータ型である必要があります。

詳細については、「[Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)」を参照してください。

## <a name="calling-syntax"></a>呼び出し構文

演算子プロシージャを暗黙的に呼び出すには、式の中で演算子記号を使用します。 オペランドは、定義済みの演算子に対して実行するのと同じ方法で指定します。

演算子プロシージャへの暗黙的な呼び出しの構文は次のとおりです。

`Dim testStruct As`*structurename*

`Dim testNewStruct As`*structurename*`= testStruct`*演算子シンボル*`10`

### <a name="illustration-of-declaration-and-call"></a>宣言と呼び出しの図

次の構造体は、上位および下位の要素として符号付き128ビット整数値を格納します。 2つの `veryLong` 値を加算し、結果として得られる `veryLong` 値を生成する `+` 演算子を定義します。

[!code-vb[VbVbcnProcedures#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#23)]

次の例は、`veryLong`で定義されている `+` 演算子の一般的な呼び出しを示しています。

[!code-vb[VbVbcnProcedures#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#24)]

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)
- [方法 : 演算子を定義する](./how-to-define-an-operator.md)
- [方法 : 変換演算子を定義する](./how-to-define-a-conversion-operator.md)
- [方法 : 演算子プロシージャを呼び出す](./how-to-call-an-operator-procedure.md)
- [方法: 演算子を定義するクラスを使用する](./how-to-use-a-class-that-defines-operators.md)
