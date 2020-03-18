---
title: Operator ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.operator
helpviewer_keywords:
- operators [Visual Basic]
- procedures [Visual Basic], operator
- Narrowing keyword [Visual Basic], conversion operators
- Visual Basic code, operators
- Widening keyword [Visual Basic], conversion operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- overloaded operators [Visual Basic]
- operator overloading
- operator procedures
- Operator statement [Visual Basic]
- CType function [Visual Basic], Operator statement
ms.assetid: b12ec4af-1ad7-4a17-865b-c5ee96320ae5
ms.openlocfilehash: aa6ae3977977ded05e47d12dabe72f09251f262d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353806"
---
# <a name="operator-statement"></a>Operator ステートメント

クラスまたは構造体に演算子プロシージャを定義する演算子記号、オペランド、およびコードを宣言します。

## <a name="syntax"></a>構文

```vb
[ <attrlist> ] Public [ Overloads ] Shared [ Shadows ] [ Widening | Narrowing ]
Operator operatorsymbol ( operand1 [, operand2 ]) [ As [ <attrlist> ] type ]
    [ statements ]
    [ statements ]
    Return returnvalue
    [ statements ]
End Operator
```

## <a name="parts"></a>指定項目

`attrlist`  
省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。

`Public`  
必須。 この演算子プロシージャに [Public](../../../visual-basic/language-reference/modifiers/public.md) アクセスがあることを示します。

`Overloads`  
省略可。 「[Overloads](../../../visual-basic/language-reference/modifiers/overloads.md)」を参照してください。

`Shared`  
必須。 この演算子プロシージャが [Shared](../../../visual-basic/language-reference/modifiers/shared.md) プロシージャであることを示します。

`Shadows`  
省略可。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。

`Widening`  
`Narrowing`を指定しない限り、変換演算子に対しては必須です。 この演算子プロシージャが [Widening](../../../visual-basic/language-reference/modifiers/widening.md) 変換を定義することを示します。 このヘルプページの「Widening and Narrowing Conversions」を参照してください。

`Narrowing`  
`Widening`を指定しない限り、変換演算子に対しては必須です。 この演算子プロシージャが[Narrowing](../../../visual-basic/language-reference/modifiers/narrowing.md)変換を定義することを示します。 このヘルプページの「Widening and Narrowing Conversions」を参照してください。

`operatorsymbol`  
必須。 この演算子プロシージャが定義する演算子のシンボルまたは識別子。

`operand1`  
必須。 単項演算子 (変換演算子を含む) または二項演算子の左オペランドの1つのオペランドの名前と型。

`operand2`  
二項演算子の場合に必要です。 二項演算子の右オペランドの名前と型。

`operand1` と `operand2` には、次の構文と部分があります。

`[ ByVal ] operandname [ As operandtype ]`

|要素|説明|
|----------|-----------------|
|`ByVal`|省略可能ですが、引渡し方法は常に [ByVal](../../../visual-basic/language-reference/modifiers/byval.md) である必要があります。|
|`operandname`|必須。 このオペランドを表す変数の名前。 「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
|`operandtype`|`Option Strict` が `On`場合を除き、省略可能です。 このオペランドのデータ型。|

`type`  
`Option Strict` が `On`場合を除き、省略可能です。 演算子プロシージャが返す値のデータ型。

`statements`  
省略可。 演算子プロシージャによって実行されるステートメントのブロック。

`returnvalue`  
必須。 演算子プロシージャが呼び出し元のコードに返す値。

`End` `Operator`  
必須。 この演算子プロシージャの定義を終了します。

## <a name="remarks"></a>コメント

`Operator` は、クラスまたは構造体でのみ使用できます。 つまり、演算子の*宣言コンテキスト*をソースファイル、名前空間、モジュール、インターフェイス、プロシージャ、またはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。

すべての演算子は `Public Shared`である必要があります。 どちらのオペランドに対しても `ByRef`、`Optional`、または `ParamArray` を指定することはできません。

戻り値を保持するために、演算子記号や識別子を使用することはできません。 `Return` ステートメントを使用する必要があります。また、値を指定する必要があります。 任意の数の `Return` ステートメントをプロシージャ内の任意の場所に記述できます。

このように演算子を定義することは、`Overloads` キーワードを使用するかどうかにかかわらず、*演算子のオーバーロード*と呼ばれます。 定義可能な演算子を次の表に示します。

|種類|演算子|
|----------|---------------|
|単項|`+`, `-`, `IsFalse`, `IsTrue`, `Not`|
|Binary|`+`, `-`, `*`, `/`, `\`, `&`, `^`, `>>`, `<<`, `=`, `<>`, `>`, `>=`, `<`, `<=`, `And`, `Like`, `Mod`, `Or`, `Xor`|
|変換 (単項)|`CType`|

バイナリリストの `=` 演算子は、代入演算子ではなく、比較演算子であることに注意してください。

`CType`を定義する場合は、`Widening` または `Narrowing`のいずれかを指定する必要があります。

## <a name="matched-pairs"></a>一致したペア

特定の演算子を一致するペアとして定義する必要があります。 このようなペアのいずれかの演算子を定義する場合は、他の演算子も定義する必要があります。 一致するペアは次のとおりです。

- `=` および `<>`

- `>` および `<`

- `>=` および `<=`

- `IsTrue` および `IsFalse`

## <a name="data-type-restrictions"></a>データ型の制限

定義するすべての演算子には、定義するクラスまたは構造体が含まれている必要があります。 これは、クラスまたは構造体が、次のデータ型として表示される必要があることを意味します。

- 単項演算子のオペランド。

- 二項演算子のオペランドのうち、少なくとも1つ。

- 変換演算子のオペランドまたは戻り値の型。

 特定の演算子には、次のような追加のデータ型制限があります。

- `IsTrue` 演算子と `IsFalse` 演算子を定義する場合は、両方とも `Boolean` 型を返す必要があります。

- `<<` 演算子と `>>` 演算子を定義する場合は、`operand2`の `operandtype` の `Integer` の種類を指定する必要があります。

戻り値の型は、どちらのオペランドの型にも対応している必要はありません。 たとえば、`=` や `<>` などの比較演算子は、どちらのオペランドも `Boolean`ない場合でも `Boolean` を返すことができます。

## <a name="logical-and-bitwise-operators"></a>論理演算子とビット処理演算子

`And`、`Or`、`Not`、および `Xor` の各演算子では、Visual Basic で論理操作またはビットごとの演算を実行できます。 ただし、クラスまたは構造体でこれらの演算子のいずれかを定義する場合は、そのビットごとの演算だけを定義できます。

`Operator` ステートメントを使用して、`AndAlso` 演算子を直接定義することはできません。 ただし、次の条件を満たしている場合は、`AndAlso` を使用できます。

- `AndAlso`に使用するのと同じオペランド型に `And` が定義されています。

- `And` の定義により、定義済みのクラスまたは構造体と同じ型が返されます。

- `And`を定義したクラスまたは構造体に `IsFalse` 演算子を定義しました。

同様に、クラスまたは構造体の戻り値の型を使用して同じオペランドで `Or` を定義し、クラスまたは構造体に `IsTrue` を定義している場合は、`OrElse` を使用できます。

## <a name="widening-and-narrowing-conversions"></a>拡大変換と縮小変換

*拡大変換*は実行時に常に成功しますが、*縮小変換*は実行時に失敗する可能性があります。詳細については、「[拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)」を参照してください。


`Widening`する変換プロシージャを宣言する場合、プロシージャコードでエラーが発生しないようにする必要があります。 これは、次のことを意味します。

- 常に `type`型の有効な値を返す必要があります。

- これは、考えられるすべての例外とその他のエラー条件を処理する必要があります。

- このメソッドは、呼び出したすべてのプロシージャからのエラーを処理する必要があります。

変換プロシージャが失敗する可能性がある場合、またはハンドルされない例外が発生する可能性がある場合は、`Narrowing`するように宣言する必要があります。

## <a name="example"></a>例

次のコード例では、`Operator` ステートメントを使用して、`And`、`Or`、`IsFalse`、および `IsTrue` の各演算子の演算子プロシージャを含む構造体のアウトラインを定義します。 `And` と `Or` は、`abc` 型のオペランドを2つ受け取り、戻り値の型 `abc`です。 `IsFalse` と `IsTrue` はそれぞれ `abc` 型の1つのオペランドを受け取り、`Boolean`を返します。 これらの定義により、呼び出し元のコードでは、`And`、`AndAlso`、`Or`、および型 `abc`のオペランドを使用して `OrElse` を使用できます。

[!code-vb[VbVbalrStatements#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#44)]

## <a name="see-also"></a>参照

- [IsFalse 演算子](../../../visual-basic/language-reference/operators/isfalse-operator.md)
- [IsTrue 演算子](../../../visual-basic/language-reference/operators/istrue-operator.md)
- [Widening](../../../visual-basic/language-reference/modifiers/widening.md)
- [Narrowing](../../../visual-basic/language-reference/modifiers/narrowing.md)
- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)
- [方法 : 演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [方法 : 変換演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [方法 : 演算子プロシージャを呼び出す](../../../visual-basic/programming-guide/language-features/procedures/how-to-call-an-operator-procedure.md)
- [方法: 演算子を定義するクラスを使用する](../../../visual-basic/programming-guide/language-features/procedures/how-to-use-a-class-that-defines-operators.md)
