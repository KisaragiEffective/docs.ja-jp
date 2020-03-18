---
title: 演算子の優先順位
ms.date: 07/20/2015
helpviewer_keywords:
- arithmetic operators [Visual Basic], precedence
- operator precedence
- logical operators [Visual Basic], precedence
- operators [Visual Basic], associativity
- operators [Visual Basic], resolution
- associativity of operators [Visual Basic]
- operators [Visual Basic], precedence
- precedence [Visual Basic], of operators
- comparison operators [Visual Basic], precedence
- math operators [Visual Basic]
- order of precedence
ms.assetid: cbbdb282-f572-458e-a520-008a675f8063
ms.openlocfilehash: 318fcc3f35276ba0b2061ba9677c5fde29429f6f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348276"
---
# <a name="operator-precedence-in-visual-basic"></a>Visual Basic における演算子の優先順位
式の中で複数の操作が発生した場合、各部分は、演算子の*優先順位*と呼ばれる事前に定義された順序で評価および解決されます。

## <a name="precedence-rules"></a>優先順位の規則
 式に複数のカテゴリの演算子が含まれている場合は、次の規則に従って評価されます。

- 算術演算子と連結演算子の優先順位は次のセクションで説明しています。また、すべての演算子は、比較演算子、論理演算子、ビットごとの演算子よりも優先順位が高くなります。

- すべての比較演算子の優先順位は同じで、すべての比較演算子は論理演算子とビット処理演算子よりも優先順位が高くなりますが、算術演算子と連結演算子より優先順位は低くなります。

- 論理演算子とビット処理演算子は、次のセクションで説明する優先順位を持ち、算術演算子、連結演算子、および比較演算子よりも優先順位が低くなります。

- 優先順位が同じ演算子は、式に出現する順序で左から右に評価されます。

## <a name="precedence-order"></a>優先順位
 演算子は、次の優先順位で評価されます。

### <a name="await-operator"></a>Await 演算子
 Await

### <a name="arithmetic-and-concatenation-operators"></a>算術演算子と連結演算子
 累乗 (`^`)

 単項 id と否定 (`+`、`–`)

 乗算および浮動小数点除算 (`*`、`/`)

 整数除算 (`\`)

 モジュール式 (`Mod`)

 加算および減算 (`+`、`–`)

 文字列の連結 (`&`)

 算術ビットシフト (`<<`、`>>`)

### <a name="comparison-operators"></a>比較演算子
 すべての比較演算子 (`=`、`<>`、`<`、`<=`、`>`、`>=`、`Is`、`IsNot`、`Like`、`TypeOf`)`Is`

### <a name="logical-and-bitwise-operators"></a>論理演算子とビット処理演算子
 否定 (`Not`)

 組み合わせ (`And`、`AndAlso`)

 包括和 (`Or`、`OrElse`)

 排他的和 (`Xor`)

### <a name="comments"></a>コメント
 `=` 演算子は等値比較演算子であり、代入演算子ではありません。

 文字列連結演算子 (`&`) は算術演算子ではありませんが、優先順位の高い演算子でグループ化されています。

 `Is` 演算子と `IsNot` 演算子は、オブジェクト参照の比較演算子です。 2つのオブジェクトの値を比較しません。2つのオブジェクト変数が同じオブジェクトインスタンスを参照しているかどうかを確認するだけです。

## <a name="associativity"></a>結合規則
 同じ優先順位の演算子が乗算や除算などの式に一緒に表示される場合、コンパイラは各操作を左から右に検出するたびに評価します。 これを次の例に示します。

```vb
Dim n1 As Integer = 96 / 8 / 4
Dim n2 As Integer = (96 / 8) / 4
Dim n3 As Integer = 96 / (8 / 4)
```

 最初の式では、除算 96/8 (結果 12) と除算 12/4 が評価されます。これは3つになります。 コンパイラでは `n1` の操作が左から右に評価されるため、この順序が `n2`に明示的に指定されている場合、評価は同じになります。 `n1` と `n2` の両方の結果が3になります。 これに対して、`n3` の結果は48になります。これは、かっこによってコンパイラによって最初に 8/4 が評価されるためです。

 この動作により、Visual Basic では、演算子が*左結合*されていると言います。

## <a name="overriding-precedence-and-associativity"></a>優先順位と結合規則のオーバーライド
 かっこを使用して、式の一部の部分を他の式の前に評価するように強制できます。 これにより、優先順位と左結合規則の両方がオーバーライドされる可能性があります。 Visual Basic は、かっこで囲まれた操作を外部のの前に常に実行します。 ただし、かっこで囲まれている場合は、かっこ内でかっこを使用しない限り、通常の優先順位と結合規則が維持されます。 これを次の例に示します。

```vb
Dim a, b, c, d, e, f, g As Double
a = 8.0
b = 3.0
c = 4.0
d = 2.0
e = 1.0
f = a - b + c / d * e
' The preceding line sets f to 7.0. Because of natural operator
' precedence and associativity, it is exactly equivalent to the
' following line.
f = (a - b) + ((c / d) * e)
' The following line overrides the natural operator precedence
' and left associativity.
g = (a - (b + c)) / (d * e)
' The preceding line sets g to 0.5.
```

## <a name="see-also"></a>参照

- [= 演算子](../../../visual-basic/language-reference/operators/assignment-operator.md)
- [Is 演算子](../../../visual-basic/language-reference/operators/is-operator.md)
- [IsNot 演算子](../../../visual-basic/language-reference/operators/isnot-operator.md)
- [Like 演算子](../../../visual-basic/language-reference/operators/like-operator.md)
- [TypeOf 演算子](../../../visual-basic/language-reference/operators/typeof-operator.md)
- [Await 演算子](../../../visual-basic/language-reference/operators/await-operator.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
