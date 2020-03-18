---
title: 比較演算子
ms.date: 07/20/2015
f1_keywords:
- vb.<>
- vb.>=
- vb.<=
- vb.>
- vb.<
helpviewer_keywords:
- greater than or equal to operator [Visual Basic]
- '>= operator [Visual Basic]'
- = operator [Visual Basic]
- < operator [Visual Basic]
- less than operator [Visual Basic]
- relational operators [Visual Basic], syntax
- Like operator [Visual Basic]
- <> operator [Visual Basic]
- '> operator [Visual Basic]'
- equal operator [Visual Basic]
- less than or equal to operator [Visual Basic]
- symbols, operators
- greater than operator [Visual Basic]
- comparing values [Visual Basic]
- operators [Visual Basic], relational
- string comparison [Visual Basic]
- not equal to comparison operator [Visual Basic]
- <= operator [Visual Basic]
- operators [Visual Basic], comparison
- Is operator [Visual Basic]
- comparison operators [Visual Basic], Visual Basic
ms.assetid: d6cb12a8-e52e-46a7-8aaf-f804d634a825
ms.openlocfilehash: ea7604626ede66da818e4bc22fe4922bc752dc2c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74336079"
---
# <a name="comparison-operators-visual-basic"></a>比較演算子 (Visual Basic)
Visual Basic で定義されている比較演算子を次に示します。

 `<` 演算子

 `<=` 演算子

 `>` 演算子

 `>=` 演算子

 `=` 演算子

 `<>` 演算子

 [Is 演算子](../../../visual-basic/language-reference/operators/is-operator.md)

 [IsNot 演算子](../../../visual-basic/language-reference/operators/isnot-operator.md)

 [Like 演算子](../../../visual-basic/language-reference/operators/like-operator.md)

 これらの演算子は、2つの式を比較して、両者が等しいかどうかを判断します。存在しない場合は、両者がどのように異なるかを判断します。 `Is`、`IsNot`、および `Like` の詳細については、個別のヘルプページを参照してください。 このページでは、関係比較演算子について詳しく説明します。

## <a name="syntax"></a>構文
  
```vb
result = expression1 comparisonoperator expression2  
result = object1 [Is | IsNot] object2  
result = string Like pattern  
```  
  
## <a name="parts"></a>指定項目
 `result`  
 必須。 比較の結果を表す `Boolean` 値。

 `expression1`, `expression2`  
 必須。 任意の式。

 `comparisonoperator`  
 必須。 任意の関係比較演算子。

 `object1`, `object2`  
 必須。 参照オブジェクトの名前。

 `string`  
 必須。 任意のブール型 ( `String` ) の式を指定します。

 `pattern`  
 必須。 任意の `String` 式または文字の範囲。

## <a name="remarks"></a>コメント
 次の表に、関係比較演算子と、`result` が `True` か `False`かを決定する条件の一覧を示します。

|演算子|`True`:|`False`:|
|--------------|---------------|----------------|
|`<` (より小さい)|`expression1` < `expression2`|`expression1` >= `expression2`|
|`<=` (以下)|`expression1` <= `expression2`|`expression1` > `expression2`|
|`>` (より大きい)|`expression1` > `expression2`|`expression1` <= `expression2`|
|`>=` (以上)|`expression1` >= `expression2`|`expression1` < `expression2`|
|`=` (等しい)|`expression1` = `expression2`|`expression1` <> `expression2`|
|`<>` (等しくない)|`expression1` <> `expression2`|`expression1` = `expression2`|

> [!NOTE]
> [= 演算子](../../../visual-basic/language-reference/operators/assignment-operator.md)は、代入演算子としても使用されます。

 `Is` 演算子、`IsNot` 演算子、および `Like` 演算子には、前の表の演算子とは異なる特定の比較機能があります。

## <a name="comparing-numbers"></a>数値の比較
 `Single` 型の式を `Double`型の1つと比較すると、`Single` 式が `Double`に変換されます。 この動作は Visual Basic 6 とは逆の動作です。

 同様に、`Decimal` 型の式を `Single` または `Double`型の式と比較すると、`Decimal` 式は `Single` または `Double`に変換されます。 `Decimal` 式の場合、1E-28 未満の小数値は失われる可能性があります。 このような小数値の損失によって、2つの値が等しくない場合に等しいと見なされることがあります。 このため、等値 (`=`) を使用して2つの浮動小数点変数を比較する場合は注意が必要です。 2つの数値の差の絶対値が許容範囲の最小値より小さいかどうかをテストする方が安全です。

### <a name="floating-point-imprecision"></a>浮動小数点おける誤差
 浮動小数点数を使用する場合は、メモリ内に常に正確な表現がないという点に注意してください。 これにより、値の比較や[Mod 演算子](../../../visual-basic/language-reference/operators/mod-operator.md)など、特定の操作によって予期しない結果が生じる可能性があります。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」を参照してください。

## <a name="comparing-strings"></a>文字列の比較
 文字列を比較すると、文字列式はアルファベット順の並べ替え順に基づいて評価されます。これは、`Option Compare` の設定によって異なります。

 `Option Compare Binary` は、文字の内部バイナリ表現から派生した並べ替え順に基づいて文字列を比較します。 並べ替え順序は、コードページによって決まります。 次の例は、一般的なバイナリ並べ替え順序を示しています。

 `A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`

 `Option Compare Text` は、アプリケーションのロケールによって決定される、大文字と小文字を区別しないテキストの並べ替え順に基づいて文字列を比較します。 前の例で `Option Compare Text` を設定して文字を並べ替えると、次のテキストの並べ替え順序が適用されます。

 `(A=a) < (À= à) < (B=b) < (E=e) < (Ê= ê) < (Ø = ø) < (Z=z)`

### <a name="locale-dependence"></a>ロケールの依存関係
 `Option Compare Text`を設定すると、文字列比較の結果は、アプリケーションが実行されているロケールによって異なる場合があります。 2文字は、1つのロケールでは等しいものの、別のロケールでは比較されない場合があります。 ログオン試行を受け入れるかどうかなど、重要な決定を行うために文字列比較を使用している場合は、ロケールの区別に関する警告が表示されます。 ロケールを考慮して、`Option Compare Binary` を設定するか、<xref:Microsoft.VisualBasic.Strings.StrComp%2A>を呼び出すことを検討してください。

## <a name="typeless-programming-with-relational-comparison-operators"></a>関係比較演算子を使用したタイプレスプログラミング
 `Object` 式での関係比較演算子の使用は、`Option Strict On`では許可されていません。 `Option Strict` が `Off`、`expression1` または `expression2` が `Object` 式の場合、実行時の型によって比較方法が決まります。 次の表に、オペランドのランタイム型に応じて、式と比較した結果を示します。

|オペランドがの場合|比較|
|---------------------|-------------------|
|両方の `String`|文字列の並べ替え特性に基づいて並べ替えの比較を行います。|
|両方の数値|`Double`に変換されたオブジェクト。数値比較。|
|1つの数値と1つの `String`|`String` は `Double` に変換され、数値比較が実行されます。 `String` を `Double`に変換できない場合は、<xref:System.InvalidCastException> がスローされます。|
|またはのいずれかまたは両方が `String` 以外の参照型です|<xref:System.InvalidCastException> がスローされます。|

 数値比較では、`Nothing` を0として扱います。 文字列比較では、`Nothing` を `""` (空の文字列) として扱います。

## <a name="overloading"></a>オーバーロード
 関係比較演算子 (`<`。 `<=`、`>`、`>=`、`=`、`<>`) は*オーバーロード*できます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体で動作を再定義できます。 このようなクラスまたは構造体でこれらの演算子のいずれかを使用するコードの場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。

 [= 演算子](../../../visual-basic/language-reference/operators/assignment-operator.md)は、代入演算子としてではなく、関係比較演算子としてのみオーバーロードできることに注意してください。

## <a name="example"></a>例
 次の例では、式を比較するために使用する関係比較演算子のさまざまな用途を示します。 関係比較演算子は、記述された式が `True`に評価されるかどうかを表す `Boolean` の結果を返します。 文字列に `>` 演算子と `<` 演算子を適用すると、文字列の通常のアルファベット順の並べ替え順序を使用して比較が行われます。 この順序は、ロケールの設定によって異なります。 並べ替えで大文字と小文字を区別するかどうかは、[オプションの比較](../../../visual-basic/language-reference/statements/option-compare-statement.md)の設定によって異なります。

 [!code-vb[VbVbalrOperators#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#1)]

 前の例では、最初の比較によって `False` が返され、残りの比較では `True`が返されます。

## <a name="see-also"></a>参照

- <xref:System.InvalidCastException>
- [= 演算子](../../../visual-basic/language-reference/operators/assignment-operator.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [トラブルシューティング (データ型)](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Visual Basic の比較演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
