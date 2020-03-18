---
title: Mod 演算子
ms.date: 04/24/2018
f1_keywords:
- vb.Mod
helpviewer_keywords:
- remainder (Mod operator)
- division operator [Visual Basic], Mod operator
- modulus operator [Visual Basic], Visual Basic
- Mod operator [Visual Basic]
- operators [Visual Basic], division
- arithmetic operators [Visual Basic], Mod
- math operators [Visual Basic]
ms.assetid: 6ff7e40e-cec8-4c77-bff6-8ddd2791c25b
ms.openlocfilehash: b7552550d4b0496d6ad7ee76a7327054d544b874
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350915"
---
# <a name="mod-operator-visual-basic"></a>Mod 演算子 (Visual Basic)

2つの数値を除算し、剰余だけを返します。

## <a name="syntax"></a>構文

```vb
result = number1 Mod number2
```

## <a name="parts"></a>指定項目

`result` \
必須。 任意の数値変数またはプロパティ。

`number1` \
必須。 任意の数式。

`number2` \
必須。 任意の数式。

## <a name="supported-types"></a>サポートされている型

すべての数値型。 これには、符号なしおよび浮動小数点型と `Decimal`が含まれます。

## <a name="result"></a>結果

結果は、`number1` が `number2`で除算された後の剰余になります。 たとえば、式 `14 Mod 4` は2に評価されます。

> [!NOTE]
> 算術の*剰余*と*剰余*の違いはありますが、負の数値の結果は異なります。 Visual Basic の `Mod` 演算子、.NET Framework `op_Modulus` 演算子、および基になる[rem](<xref:System.Reflection.Emit.OpCodes.Rem>) IL 命令はすべて、剰余演算を実行します。

`Mod` 操作の結果では、被除数の符号 (`number1`) が保持されるため、正または負のどちらの場合でもかまいません。 結果は常に (-`number2`、`number2`) の範囲内にあります。 例 :

```vb
Public Module Example
   Public Sub Main()
      Console.WriteLine($" 8 Mod  3 = {8 Mod 3}")
      Console.WriteLine($"-8 Mod  3 = {-8 Mod 3}")
      Console.WriteLine($" 8 Mod -3 = {8 Mod -3}")
      Console.WriteLine($"-8 Mod -3 = {-8 Mod -3}")
   End Sub
End Module
' The example displays the following output:
'       8 Mod  3 = 2
'      -8 Mod  3 = -2
'       8 Mod -3 = 2
'      -8 Mod -3 = -2
```

## <a name="remarks"></a>コメント

`number1` または `number2` のいずれかが浮動小数点値の場合は、除算の浮動小数点の剰余が返されます。 結果のデータ型は、`number1` および `number2`のデータ型との除算によって得られるすべての値を保持できる最小のデータ型です。

`number1` または `number2` が[Nothing](../../../visual-basic/language-reference/nothing.md)と評価された場合、0として扱われます。

関連する演算子は次のとおりです。

- [\ 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)は、除算の整数の商を返します。 たとえば、式 `14 \ 4` は3に評価されます。

- [/演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/floating-point-division-operator.md)は、剰余を含む完全な商を浮動小数点数として返します。 たとえば、式 `14 / 4` は3.5 に評価されます。

## <a name="attempted-division-by-zero"></a>0による除算を試行しました

`number2` が0に評価される場合、`Mod` 演算子の動作は、オペランドのデータ型によって異なります。

- コンパイル時に `number2` を特定できず、コンパイル時にゼロに評価され `number2` た場合 `BC30542 Division by zero occurred while evaluating this expression` コンパイル時エラーを生成する場合、整数除算は <xref:System.DivideByZeroException> 例外をスローします。
- 浮動小数点除算は <xref:System.Double.NaN?displayProperty=nameWithType>を返します。

## <a name="equivalent-formula"></a>同等の式

`a Mod b` 式は、次の数式のいずれかに相当します。

`a - (b * (a \ b))`

`a - (b * Fix(a / b))`

## <a name="floating-point-imprecision"></a>浮動小数点おける誤差

浮動小数点数を使用する場合は、メモリ内で常に正確な10進表現が使用されるわけではないことに注意してください。 これにより、値の比較や `Mod` 演算子など、特定の操作によって予期しない結果が発生する可能性があります。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」を参照してください。

## <a name="overloading"></a>オーバーロード

`Mod` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体で動作を再定義できます。 コードが、そのようなオーバーロードを含むクラスまたは構造体のインスタンスに `Mod` 適用される場合は、再定義された動作を理解していることを確認してください。 詳細については、「 [演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。

## <a name="example"></a>例

次の例では、`Mod` 演算子を使用して2つの数値を除算し、剰余だけを返します。 どちらかの数値が浮動小数点数の場合、結果は剰余を表す浮動小数点数になります。

[!code-vb[VbVbalrOperators#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#31)]

## <a name="example"></a>例

次の例は、浮動小数点オペランドのおける誤差の可能性を示しています。 最初のステートメントでは、オペランドが `Double`、0.2 は0.20000000000000001 の値が格納された無限の連続するバイナリ部分です。 2番目のステートメントでは、リテラル型文字 `D` は両方のオペランドを強制的に `Decimal`し、0.2 には正確な表現が含まれています。

[!code-vb[VbVbalrOperators#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#32)]

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Conversion.Int%2A>
- <xref:Microsoft.VisualBasic.Conversion.Fix%2A>
- [算術演算子](../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [トラブルシューティング (データ型)](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Visual Basic の算術演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [\ 演算子 (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)
