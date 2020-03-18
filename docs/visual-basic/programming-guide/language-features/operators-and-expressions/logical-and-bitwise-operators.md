---
title: 論理演算子とビット処理演算子
ms.date: 07/20/2015
helpviewer_keywords:
- short-circuiting
- Boolean expressions
- logical operators [Visual Basic], Boolean expressions
- operators [Visual Basic], logical
- AndAlso operator [Visual Basic]
- Not operator [Visual Basic], Boolean expressions
- Xor operator [Visual Basic], Boolean expressions
- And operator [Visual Basic], logical operators
- logical operators [Visual Basic]
- expressions [Visual Basic], Boolean
- Or operator [Visual Basic], logical operators
- Visual Basic code, operators
- short-circuiting [Visual Basic], logical operators
- logical operators [Visual Basic], short-circuiting
- Visual Basic code, expressions
- logical operators [Visual Basic], binary
- OrElse operator [Visual Basic]
- logical operators [Visual Basic], unary
ms.assetid: ca474e13-567d-4b1d-a18b-301433705e57
ms.openlocfilehash: 55a246c0d56501a409ebbc7d0d0aa39ae9fa1770
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343595"
---
# <a name="logical-and-bitwise-operators-in-visual-basic"></a>Visual Basic の論理演算子とビット処理演算子
論理演算子は `Boolean` 式を比較し、`Boolean` の結果を返します。 `And`、`Or`、`AndAlso`、`OrElse`、および `Xor` の各演算子は2つのオペランドを受け取りますが、`Not` 演算子は*単項*演算であるため、*二項*演算子です。 これらの演算子の一部では、整数値に対してビットごとの論理演算を実行することもできます。  
  
## <a name="unary-logical-operator"></a>単項論理演算子  
 [Not 演算子](../../../../visual-basic/language-reference/operators/not-operator.md)は、`Boolean` 式の論理*否定*を実行します。 これにより、オペランドの論理逆が生成されます。 式が `True`に評価された場合、`Not` は `False`を返します。式が `False`に評価された場合、`Not` は `True`を返します。 次に例を示します。  
  
 [!code-vb[VbVbalrOperators#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#77)]  
  
## <a name="binary-logical-operators"></a>二項論理演算子  
 [And 演算子](../../../../visual-basic/language-reference/operators/and-operator.md)は、2つの `Boolean` 式の論理*積*を実行します。 両方の式が `True`に評価される場合、`And` は `True`を返します。 少なくとも1つの式が `False`に評価された場合、`And` は `False`を返します。  
  
 [Or 演算子](../../../../visual-basic/language-reference/operators/or-operator.md)は、2つの `Boolean` 式に論理*和* *を実行*します。 いずれかの式が `True`に評価されるか、両方とも `True`に評価される場合、`Or` は `True`を返します。 どちらの式も `True`に評価されない場合、`Or` は `False`を返します。  
  
 [Xor 演算子](../../../../visual-basic/language-reference/operators/xor-operator.md)は、2つの `Boolean` 式に対して論理的な*除外*を実行します。 1つの式だけが `True`に評価され、両方ではない場合、`Xor` は `True`を返します。 両方の式が `True` に評価されるか、両方とも `False`に評価される場合、`Xor` は `False`を返します。  
  
 次の例は、`And`、`Or`、および `Xor` 演算子を示しています。  
  
 [!code-vb[VbVbalrOperators#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#78)]  
  
## <a name="short-circuiting-logical-operations"></a>ショートサーキット論理操作  
 [AndAlso 演算子](../../../../visual-basic/language-reference/operators/andalso-operator.md)は、2つの `Boolean` 式に対して論理積も実行するという点で、`And` 演算子と非常によく似ています。 2つの間の主な違いは、`AndAlso` が*ショートサーキット*動作を示す点です。 `AndAlso` 式の最初の式が `False`に評価される場合、最終的な結果を変更することはできず、`AndAlso` は `False`を返すため、2番目の式は評価されません。  
  
 同様に、 [OrElse 演算子](../../../../visual-basic/language-reference/operators/orelse-operator.md)は、2つの `Boolean` 式のショートサーキット論理和を実行します。 `OrElse` 式の最初の式が `True`に評価される場合、最終的な結果を変更することはできず、`OrElse` は `True`を返すため、2番目の式は評価されません。  
  
### <a name="short-circuiting-trade-offs"></a>ショートサーキットトレードオフ  
 ショートサーキットを使用すると、論理演算の結果を変更できない式を評価しないことで、パフォーマンスを向上させることができます。 ただし、その式が追加のアクションを実行する場合、ショートサーキットはこれらのアクションをスキップします。 たとえば、式に `Function` プロシージャへの呼び出しが含まれている場合、式がサーキットの場合、そのプロシージャは呼び出されず、`Function` に含まれる追加のコードは実行されません。 このため、関数は、ときどき実行される場合があり、正しくテストされない可能性があります。 または、プログラムロジックが `Function`のコードに依存している場合もあります。  
  
 次の例は、`And`、`Or`、および対応するショートサーキットの違いを示しています。  
  
 [!code-vb[VbVbalrOperators#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#81)]  
  
 [!code-vb[VbVbalrOperators#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#80)]  
  
 [!code-vb[VbVbalrOperators#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#79)]  
  
 前の例では、`checkIfValid()` 内のいくつかの重要なコードは、サーキットの呼び出しでは実行されないことに注意してください。 最初の `If` ステートメントは、`And` がショートサーキットではないため、`12 > 45` が `False`を返す場合でも `checkIfValid()` を呼び出します。 2番目の `If` ステートメントでは `checkIfValid()`が呼び出されません。これは、`12 > 45` が `False`を返し `AndAlso`、2番目の式をショートサーキットするためです。 3番目の `If` ステートメントは、`Or` がショートサーキットではないため、`12 < 45` が `True`を返す場合でも `checkIfValid()` を呼び出します。 4番目の `If` ステートメントでは `checkIfValid()`が呼び出されません。これは、`12 < 45` が `True`を返し `OrElse`、2番目の式をショートサーキットするためです。  
  
## <a name="bitwise-operations"></a>ビットごとの演算  
 ビットごとの演算では、2つの整数値が binary (base 2) 形式で評価されます。 これらは、対応する位置のビットを比較し、比較に基づいて値を割り当てます。 `And` 演算子の例を次に示します。  
  
 [!code-vb[VbVbalrConcepts#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConcepts/VB/Class1.vb#2)]  
  
 前の例では、`x` の値を1に設定しています。 これは、次の理由で発生します。  
  
- 値はバイナリとして扱われます。  
  
     3バイナリ形式の 3 = 011  
  
     5バイナリ形式 = 101  
  
- `And` 演算子は、一度に1つのバイナリ位置 (ビット) をバイナリ表現と比較します。 指定された位置にある両方のビットが1の場合、結果内のその位置に1が配置されます。 どちらかのビットが0の場合は、結果内のその位置に0が配置されます。 前の例では、これは次のように動作します。  
  
     011 (バイナリ形式の場合は 3)  
  
     101 (バイナリ形式で 5)  
  
     001 (バイナリ形式の結果)  
  
- 結果は decimal として扱われます。 値001は1のバイナリ表現であるため、`x` = 1 です。  
  
 ビットごとの `Or` 演算は似ていますが、比較対象のビットのいずれかまたは両方が1の場合、結果ビットに1が割り当てられる点が異なります。 比較したビットの1つ (両方ではない) が1の場合、`Xor` は結果ビットに1を割り当てます。 `Not` は1つのオペランドを受け取り、符号ビットを含むすべてのビットを反転し、その値を結果に代入します。 つまり、符号付き正の数値の場合、`Not` は常に負の値を返します。負の数値の場合、`Not` は常に正の値または0を返します。  
  
 `AndAlso` 演算子と `OrElse` 演算子では、ビットごとの演算がサポートされていません。  
  
> [!NOTE]
> ビットごとの演算は、整数型に対してのみ実行できます。 浮動小数点値は、ビットごとの演算を続行する前に、整数型に変換する必要があります。  
  
## <a name="see-also"></a>関連項目

- [論理/ビット演算子 (Visual Basic)](../../../../visual-basic/language-reference/operators/logical-bitwise-operators.md)
- [ブール式](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/boolean-expressions.md)
- [Visual Basic の算術演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Visual Basic の比較演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Visual Basic での連結演算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md)
- [演算子の効率のよい組み合わせ](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/efficient-combination-of-operators.md)
