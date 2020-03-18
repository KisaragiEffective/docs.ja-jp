---
title: Like 演算子
ms.date: 07/20/2015
f1_keywords:
- Like
- vb.Like
helpviewer_keywords:
- similar to
- pattern matching
- Like operator [Visual Basic]
- '? symbol, wildcard character'
- string comparison [Visual Basic], Like operator
- strings [Visual Basic], comparing
- comparison operators [Visual Basic]
- symbols, wildcard
- wildcards, Like operator
- strings [Visual Basic], matching
- string comparison [Visual Basic], sorting data
- data [Visual Basic], sorting
- text [Visual Basic], comparing
- operators [Visual Basic], pattern-matching
- data [Visual Basic], string comparisons
- string comparison [Visual Basic], Like operators
ms.assetid: 966283ec-80e2-4294-baa8-c75baff804f9
ms.openlocfilehash: 5db9488bbec716156a3ab464042c0853241a82b1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350946"
---
# <a name="like-operator-visual-basic"></a>Like 演算子 (Visual Basic)
文字列をパターンと比較します。  

> [!IMPORTANT]
> `Like` 演算子は、現在、.NET Core および .NET Standard プロジェクトではサポートされていません。

## <a name="syntax"></a>構文  
  
```vb  
result = string Like pattern  
```  
  
## <a name="parts"></a>指定項目  
 `result`  
 必須。 任意の `Boolean` 変数。 結果は、`string` が `pattern`を満たすかどうかを示す `Boolean` 値です。  
  
 `string`  
 必須。 任意のブール型 ( `String` ) の式を指定します。  
  
 `pattern`  
 必須。 「解説」で説明されているパターン一致規則に準拠した任意の `String` 式。  
  
## <a name="remarks"></a>コメント  
 `string` の値が `pattern`に含まれるパターンを満たす場合、`result` は `True`です。 文字列がパターンを満たさない場合は、`result` が `False`ます。 `string` と `pattern` の両方が空の文字列の場合、結果は `True`ます。  
  
## <a name="comparison-method"></a>比較方法  
 `Like` 演算子の動作は、 [Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)によって異なります。 各ソースファイルの既定の文字列比較方法は `Option Compare Binary`です。  
  
## <a name="pattern-options"></a>パターンオプション  
 組み込みのパターン照合では、文字列比較に使用できるさまざまなツールが用意されています。 パターンマッチング機能を使用すると、`string` 内の各文字を、特定の文字、ワイルドカード文字、文字リスト、または文字範囲に対して一致させることができます。 次の表に、`pattern` で使用できる文字と、それらが一致する文字を示します。  
  
|`pattern` 内の文字|`string` 内の一致|  
|-----------------------------|-------------------------|  
|`?`|任意の 1 文字|  
|`*`|0個以上の文字|  
|`#`|任意の1桁 (0 ~ 9)|  
|`[charlist]`|`charlist` 内の任意の1文字|  
|`[!charlist]`|`charlist` にない任意の1文字|  
  
## <a name="character-lists"></a>文字リスト  
 角かっこ (`[ ]`) で囲まれた1つ以上の文字 (`charlist`) のグループを使用して `string` 内の任意の1文字と一致させることができ、数字を含むほぼすべての文字コードを含めることができます。  
  
 `charlist` の先頭にある感嘆符 (`!`) は、`charlist` 内の文字を除く任意の文字が `string`で見つかった場合に一致することを意味します。 かっこの外側で使用すると、感嘆符自体が一致します。  
  
## <a name="special-characters"></a>特殊文字  
 特殊文字の左角かっこ (`[`)、疑問符 (`?`)、番号記号 (`#`)、アスタリスク (`*`) と一致させるには、角かっこで囲みます。 右角かっこ (`]`) は、それ自体に一致するグループ内では使用できませんが、グループの外側で個別の文字として使用することはできます。  
  
 文字シーケンス `[]` は、長さが0の文字列 (`""`) と見なされます。 ただし、角かっこで囲まれた文字リストの一部にすることはできません。 `string` 内の位置に文字のグループのいずれかが含まれているかどうかを確認したい場合は、`Like` を2回使用できます。 例については、「[方法: 文字列をパターンに一致させる](../../../visual-basic/programming-guide/language-features/operators-and-expressions/how-to-match-a-string-against-a-pattern.md)」を参照してください。  
  
## <a name="character-ranges"></a>文字範囲  
 ハイフン (`–`) を使用して範囲の下限と上限を区切ることにより、`charlist` は文字の範囲を指定できます。 たとえば、`string` 内の対応する文字位置に `A`–`Z`の範囲内の任意の文字が含まれている場合、`[A–Z]` は一致と見なされます。また、対応する文字位置に範囲外の文字が含まれている場合は、`[!H–L]` – `H`に一致します。`L`  
  
 文字の範囲を指定する場合は、昇順の並べ替え順序、つまり、小さい方から順に表示する必要があります。 したがって、`[A–Z]` は有効なパターンですが、`[Z–A]` は有効ではありません。  
  
### <a name="multiple-character-ranges"></a>複数の文字範囲  
 同じ文字位置に複数の範囲を指定するには、区切り記号を使用せずに同じ角かっこ内に配置します。 たとえば、`string` 内の対応する文字位置に `A`–`C` または `X`~`Z`の範囲内のいずれかの文字が含まれている場合は、`[A–CX–Z]` 結果が一致します。  
  
### <a name="usage-of-the-hyphen"></a>ハイフンの使用  
 ハイフン (`–`) は、先頭 (感嘆符の後)、または `charlist` の末尾に、それ自体に一致させることができます。 その他の場所では、ハイフンは、ハイフンの両側の文字で区切られた文字の範囲を識別します。  
  
## <a name="collating-sequence"></a>照合順序  
 指定された範囲の意味は、実行時の文字の順序によって異なります。 `Option Compare` と、コードが実行されているシステムのロケール設定によって決まります。 `Option Compare Binary`の場合、`[A–E]` 範囲は `A`、`B`、`C`、`D`、および `E`と一致します。 `Option Compare Text`では、`[A–E]` は `A`、`a`、`À`、`à`、`B`、`b`、`C`、`c`、`D`、`d`、`E`、`e`と一致します。 この範囲は `Ê` または `ê` と一致しません。これは、アクセント記号付き文字が並べ替え順序でアクセントが付いていない文字の後になるためです。  
  
## <a name="digraph-characters"></a>Digraph の文字  
 言語によっては、2つの異なる文字を表すアルファベット文字があります。 たとえば、いくつかの言語では、文字 `æ` を使用して `a` と `e` 文字が一緒に表示されます。 `Like` 演算子は、1つの digraph 文字と2つの個別の文字が等価であることを認識します。  
  
 Digraph 文字を使用する言語がシステムのロケール設定で指定されている場合、`pattern` または `string` の1つの digraph 文字が、他の文字列の等価の2文字シーケンスと一致します。 同様に、角かっこで囲まれた `pattern` の digraph 文字 (単独、リスト、または範囲内) は、`string`内の等価の2文字シーケンスと一致します。  
  
## <a name="overloading"></a>オーバーロード  
 `Like` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 コードでこのようなクラスまたは構造体に対してこの演算子を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「 [Operator Procedures](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 この例では、`Like` 演算子を使用して、文字列をさまざまなパターンと比較します。 結果は、各文字列がパターンを満たすかどうかを示す `Boolean` 変数に入ります。  
  
 [!code-vb[VbVbalrOperators#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#30)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Strings.InStr%2A>
- <xref:Microsoft.VisualBasic.Strings.StrComp%2A>
- [比較演算子](../../../visual-basic/language-reference/operators/comparison-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [方法 : 文字列がパターンに一致するかを調べる](../../../visual-basic/programming-guide/language-features/operators-and-expressions/how-to-match-a-string-against-a-pattern.md)
