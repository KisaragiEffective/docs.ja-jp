---
title: シンボルと演算子のリファレンス
description: F#プログラミング言語で使用される記号と演算子について説明します。
ms.date: 02/11/2019
ms.openlocfilehash: 0c177242991c39aa1a2b7566415b50385f3403d1
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424061"
---
# <a name="symbol-and-operator-reference"></a>シンボルと演算子のリファレンス

> [!NOTE]
> この記事の API リファレンスのリンクをクリックすると MSDN に移動します。  docs.microsoft.com API リファレンスは完全ではありません。

このトピックでは、F# 言語で使用するシンボルと演算子の表を示します。

## <a name="table-of-symbols-and-operators"></a>シンボルと演算子の表

次の表では、F# 言語で使用されるシンボルについて説明し、より詳しい情報が提供されるトピックへのリンクと、シンボルの使用方法の一部の簡単な説明を示します。 シンボルは、ASCII 文字セットの順序に従って並べられています。

|シンボルまたは演算子|リンク|説明|
|------------------|-----|-----------|
|`!`|[参照セル](../reference-cells.md)<br /><br />[コンピュテーション式](../computation-expressions.md)|<ul><li>参照セルを逆参照します。<br /></li><li>キーワードの後に付いて、ワークフローで制御されるキーワードの動作の変更済みバージョンを示します。<br /></li></ul>|
|`!=`|該当しない。|<ul><li>F# では使用されません。 非等値演算には `<>` を使用します。<br /></li></ul>|
|`"`|[リテラル](../literals.md)<br /><br />[文字列](../strings.md)|<ul><li>テキスト文字列を区切ります。<br /></li></ul>|
|`"""`|[文字列](../strings.md)|逐語的テキスト文字列を区切ります。 文字列内で単一引用符を使用することで引用符文字を示すことができる `@"..."` とは異なります。|
|`#`|[コンパイラ ディレクティブ](../compiler-directives.md)<br /><br />[フレキシブル型](../flexible-types.md)|<ul><li>`#light` のように、プリプロセッサまたはコンパイラ ディレクティブの前に付けられます。<br /></li><li>型で使用されている場合は、*フレキシブル型*を示します。これは、型またはその派生型のいずれかを指します。<br /></li></ul>|
|`$`|詳細情報はありません。|<ul><li>コンパイラで生成された特定の変数名と関数名に、内部的に使用されます。<br /></li></ul>|
|`%`|[算術演算子](arithmetic-operators.md)<br /><br />[コード クォート](../code-quotations.md)|<ul><li>整数の剰余を計算します。<br /></li><li>型指定されたコード クォートに式をスプライスするために使用されます。<br /></li></ul>|
|`%%`|[コード クォート](../code-quotations.md)|<ul><li>型指定されていないコード クォートに式をスプライスするために使用されます。<br /></li></ul>|
|`%?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合に、整数の剰余を計算します。<br /></li></ul>|
|`&`|[match 式](../match-expressions.md)|<ul><li>他の言語と相互運用するときに使用するための、変更可能な値のアドレスを計算します。<br /></li><li>AND パターンで使用されます。<br /></li></ul>|
|`&&`|[ブール演算子](boolean-operators.md)|<ul><li>ブール値の AND 演算を実行します。<br /></li></ul>|
|`&&&`|[ビット処理演算子](bitwise-operators.md)|<ul><li>ビットごとの AND 演算を実行します。<br /></li></ul>|
|`'`|[リテラル](../literals.md)<br /><br />[自動ジェネリック化](../generics/automatic-generalization.md)|<ul><li>1 文字のリテラルを区切ります。<br /></li><li>ジェネリック型パラメーターを示します。<br /></li></ul>|
|<code>&#96;&#96;...&#96;&#96;</code>|詳細情報はありません。|<ul><li>言語のキーワードなど、区切らなければ有効な識別子にならない識別子を区切ります。<br /></li></ul>|
|`( )`|[Unit 型](../unit-type.md)|<ul><li>unit 型の 1 つの値を表します。<br /></li></ul>|
|`(...)`|[タプル](../tuples.md)<br /><br />[演算子のオーバーロード](../operator-overloading.md)|<ul><li>式が評価される順序を示します。<br /></li><li>タプルを区切ります。<br /></li><li>演算子の定義で使用されます。<br /></li></ul>|
|`(*...*)`||<ul><li>複数行にわたる可能性のあるコメントを区切ります。<br /></li></ul>|
|<code>(&#124;...&#124;)</code>|[アクティブ パターン](../active-patterns.md)|<ul><li>アクティブ パターンを区切ります。 *バナナ クリップ*とも呼ばれます。<br /></li></ul>|
|`*`|[算術演算子](arithmetic-operators.md)<br /><br />[タプル](../tuples.md)<br /><br />[測定単位](../units-of-measure.md)|<ul><li>二項演算子として使用されている場合は、左辺と右辺を乗算します。<br /></li><li>型では、タプルのペアリングを示します。<br /></li><li>測定単位の型で使用されます。<br /></li></ul>|
|`*?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、左辺と右辺を乗算します。<br /></li></ul>|
|`**`|[算術演算子](arithmetic-operators.md)|<ul><li>指数演算を実行します (`x ** y` は `x` の `y` 乗を意味します)。<br /></li></ul>|
|`+`|[算術演算子](arithmetic-operators.md)|<ul><li>二項演算子として使用されている場合は、左辺と右辺を加算します。<br /></li><li>単項演算子として使用されている場合は、正数を示します。 (正式には、同じ値を符号を変更しないで生成します。)<br /></li></ul>|
|`+?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、左辺と右辺を加算します。<br /></li></ul>|
|`,`|[タプル](../tuples.md)|<ul><li>タプルの要素または型パラメーターを区切ります。<br /></li></ul>|
|`-`|[算術演算子](arithmetic-operators.md)|<ul><li>二項演算子として使用されている場合は、左辺から右辺を減算します。<br /></li><li>単項演算子として使用されている場合は、否定演算を実行します。<br /></li></ul>|
|`-?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、左辺から右辺を減算します。<br /></li></ul>|
|`->`|[関数](../functions/index.md)<br /><br />[match 式](../match-expressions.md)|<ul><li>関数の型では、引数と戻り値を区切ります。<br /></li><li>(シーケンス式の) 式を生成します。`yield` キーワードと等価です。<br /></li><li>match 式で使用されます。<br /></li></ul>|
|`.`|[メンバー](../members/index.md)<br /><br />[プリミティブ型](../basic-types.md)|<ul><li>メンバーにアクセスし、完全修飾名の個々の名前を区切ります。<br /></li><li>浮動小数点数の小数点を示します。<br /></li></ul>|
|`..`|[ループ: `for...in` 式](../loops-for-in-expression.md)|<ul><li>範囲を指定します。<br /></li></ul>|
|`.. ..`|[ループ: `for...in` 式](../loops-for-in-expression.md)|<ul><li>インクリメントと共に範囲を指定します。<br /></li></ul>|
|`.[...]`|[配列](../arrays.md)|<ul><li>配列要素にアクセスします。<br /></li></ul>|
|`/`|[算術演算子](arithmetic-operators.md)<br /><br />[測定単位](../units-of-measure.md)|<ul><li>左辺 (分子) を右辺 (分母) で除算します。<br /></li><li>測定単位の型で使用されます。<br /></li></ul>|
|`/?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、左辺を右辺で除算します。<br /></li></ul>|
|`//`||<ul><li>単一行コメントの先頭を示します。<br /></li></ul>|
|`///`|[XML に関するドキュメント](../xml-documentation.md)|<ul><li>XML コメントを示します。<br /></li></ul>|
|`:`|[関数](../functions/index.md)|<ul><li>型注釈では、パラメーター名またはメンバー名とその型を区切ります。<br /></li></ul>|
|`::`|[リスト](../lists.md)<br /><br />[match 式](../match-expressions.md)|<ul><li>リストを作成します。 左辺の要素が右辺のリストに追加されます。<br /></li><li>リストの各部分を区切るためにパターン マッチで使用されます。<br /></li></ul>|
|`:=`|[参照セル](../reference-cells.md)|<ul><li>参照セルに値を割り当てます。<br /></li></ul>|
|`:>`|[キャストと変換](../casting-and-conversions.md)|<ul><li>型を階層の上位の型に変換します。<br /></li></ul>|
|`:?`|[match 式](../match-expressions.md)|<ul><li>値が指定した型と一致する (サブタイプである場合を含む) 場合は `true` を返します。それ以外の場合は `false` を返します (型テスト演算子)。<br /></li></ul>|
|`:?>`|[キャストと変換](../casting-and-conversions.md)|<ul><li>型を階層の下位にある型に変換します。<br /></li></ul>|
|`;`|[冗語構文](../verbose-syntax.md)<br /><br />[リスト](../lists.md)<br /><br />[レコード](../records.md)|<ul><li>式を区切ります (ほとんどの場合、冗語構文で使用されます)。<br /></li><li>リストの要素を区切ります。<br /></li><li>レコードのフィールドを区切ります。<br /></li></ul>|
|`<`|[算術演算子](arithmetic-operators.md)|<ul><li>小なり演算を実行します。<br /></li></ul>|
|`<?`|[Null 許容の演算子](nullable-operators.md)|右辺が null 許容型である場合は、小なり演算を実行します。|
|`<<`|[関数](../functions/index.md)|<ul><li>2 つの関数を逆順で合成します。2 つ目の関数が先に実行されます (後方合成演算子)。<br /></li></ul>|
|`<<<`|[ビット処理演算子](bitwise-operators.md)|<ul><li>左辺にある数のビットを、右辺に指定されたビット数だけ左にシフトします。<br /></li></ul>|
|`<-`|[値](../values/index.md)|<ul><li>値を変数に代入します。<br /></li></ul>|
|`<...>`|[自動ジェネリック化](../generics/automatic-generalization.md)|<ul><li>型パラメーターを区切ります。<br /></li></ul>|
|`<>`|[算術演算子](arithmetic-operators.md)|<ul><li>左辺が右辺と等しくない場合は `true` を返します。それ以外の場合は false を返します。<br /></li></ul>|
|`<>?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、"等しくない" 演算を実行します。<br /></li></ul>|
|`<=`|[算術演算子](arithmetic-operators.md)|<ul><li>左辺が右辺以下である場合は `true` を返します。それ以外の場合は `false` を返します。<br /></li></ul>|
|`<=?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、"以下" 演算を実行します。<br /></li></ul>|
|<code>&lt;&#124;</code>|[関数](../functions/index.md)|<ul><li>右辺の式の結果を左辺の関数に渡します (後方パイプ演算子)。<br /></li></ul>|
|<code>&lt;&#124;&#124;</code>|[Operators.&#40; &#60;&#124;&#124; &#41;&#60;'T1,'T2,'U&#62; 関数](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-%5bhh-%5d%5b%27t1%2c%27t2%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>右辺にある 2 つの引数のタプルを左辺の関数に渡します。<br /></li></ul>|
|<code>&lt;&#124;&#124;&#124;</code>|[Operators.&#40; &#60;&#124;&#124;&#124; &#41;&#60;'T1,'T2,'T3,'U&#62; 関数](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-%5bhhh-%5d%5b%27t1%2c%27t2%2c%27t3%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>右辺にある 3 つの引数のタプルを左辺の関数に渡します。<br /></li></ul>|
|`<@...@>`|[コード クォート](../code-quotations.md)|<ul><li>型指定されたコード クォートを区切ります。<br /></li></ul>|
|`<@@...@@>`|[コード クォート](../code-quotations.md)|<ul><li>型指定されていないコード クォートを区切ります。<br /></li></ul>|
|`=`|[算術演算子](arithmetic-operators.md)|<ul><li>左辺が右辺と等しい場合は `true` を返します。それ以外の場合は `false` を返します。<br /></li></ul>|
|`=?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、"等しい" 演算を実行します。<br /></li></ul>|
|`==`|該当しない。|<ul><li>F# では使用されません。 等値演算には `=` を使用します。<br /></li></ul>|
|`>`|[算術演算子](arithmetic-operators.md)|<ul><li>左辺が右辺より大きい場合は `true` を返します。それ以外の場合は `false` を返します。<br /></li></ul>|
|`>?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、"より大きい" 演算を実行します。<br /></li></ul>|
|`>>`|[関数](../functions/index.md)|<ul><li>2 つの関数を合成します (前方合成演算子)。<br /></li></ul>|
|`>>>`|[ビット処理演算子](bitwise-operators.md)|<ul><li>左辺にある数のビットを、右辺に指定された桁数だけ右にシフトします。<br /></li></ul>|
|`>=`|[算術演算子](arithmetic-operators.md)|<ul><li>左辺が右辺以上の場合に `true` を返します。それ以外の場合は `false`を返します。<br /></li></ul>|
|`>=?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>右辺が null 許容型である場合は、"以上" 演算を実行します。<br /></li></ul>|
|`?`|[パラメーターと引数](../parameters-and-arguments.md)|<ul><li>省略可能な引数を指定します。<br /></li><li>動的メソッドや動的プロパティの呼び出しのための演算子として使用されます。 独自の実装を提供する必要があります。<br /></li></ul>|
|`? ... <- ...`|詳細情報はありません。|<ul><li>動的プロパティを設定するための演算子として使用されます。 独自の実装を提供する必要があります。<br /></li></ul>|
|`?>=`、`?>`、`?<=`、`?<`、`?=`、`?<>`、`?+`、`?-`、`?*`、`?/`|[Null 許容の演算子](nullable-operators.md)|<ul><li>null 許容型が左辺にある場合、 ? プレフィックスがない、対応する演算子と等価です。<br /></li></ul>|
|`>=?`、`>?`、`<=?`、`<?`、`=?`、`<>?`、`+?`、`-?`、`*?`、`/?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>null 許容型が左辺にある場合、 ? サフィックスがない、対応する演算子と等価です。<br /></li></ul>|
|`?>=?`、`?>?`、`?<=?`、`?<?`、`?=?`、`?<>?`、`?+?`、`?-?`、`?*?`、`?/?`|[Null 許容の演算子](nullable-operators.md)|<ul><li>両辺が null 許容型である場合、前後に疑問符がない、対応する演算子と等価です。<br /></li></ul>|
|`@`|[リスト](../lists.md)<br /><br />[文字列](../strings.md)|<ul><li>2 つのリストを連結します。<br /></li><li>文字列リテラルの前にある場合、文字列は逐語的に解釈され、エスケープ文字は解釈されないことを示します。<br /></li></ul>|
|`[...]`|[リスト](../lists.md)|<ul><li>リストの要素を区切ります。<br /></li></ul>|
|<code>[&#124;...&#124;]</code>|[配列](../arrays.md)|<ul><li>配列の要素を区切ります。<br /></li></ul>|
|`[<...>]`|[属性](../attributes.md)|<ul><li>属性を区切ります。<br /></li></ul>|
|`\`|[文字列](../strings.md)|<ul><li>次の文字をエスケープします。文字や文字列リテラルで使用されます。<br /></li></ul>|
|`^`|[静的に解決される型パラメーター](../generics/statically-resolved-type-parameters.md)<br /><br />[文字列](../strings.md)|<ul><li>実行時ではなくコンパイル時に解決する必要がある型パラメーターを指定します。<br /></li><li>文字列を連結します。<br /></li></ul>|
|`^^^`|[ビット処理演算子](bitwise-operators.md)|<ul><li>ビットごとの排他的 OR 演算を実行します。<br /></li></ul>|
|`_`|[match 式](../match-expressions.md)<br /><br />[ジェネリック](../generics/index.md)|<ul><li>ワイルドカード パターンを示します。<br /></li><li>匿名ジェネリック パラメーターを指定します。<br /></li></ul>|
|<code>&#96;</code>|[自動ジェネリック化](../generics/automatic-generalization.md)|<ul><li>ジェネリック型パラメーターを示すために内部的に使用されます。<br /></li></ul>|
|`{...}`|[シーケンス](../sequences.md)<br /><br />[レコード](../records.md)|<ul><li>シーケンス式とコンピュテーション式を区切ります。<br /></li><li>レコードの定義で使用されます。<br /></li></ul>|
|<code>&#124;</code>|[match 式](../match-expressions.md)|<ul><li>個々の一致ケース、個々の判別共用体ケース、および列挙値を区切ります。<br /></li></ul>|
|<code>&#124;&#124;</code>|[ブール演算子](boolean-operators.md)|<ul><li>ブール型の OR 演算を実行します。<br /></li></ul>|
|<code>&#124;&#124;&#124;</code>|[ビット処理演算子](bitwise-operators.md)|<ul><li>ビットごとの OR 演算を実行します。<br /></li></ul>|
|<code>&#124;></code>|[関数](../functions/index.md)|<ul><li>左辺の結果を関数を右辺の関数に渡します (前方パイプ演算子)。<br /></li></ul>|
|<code>&#124;&#124;></code>|[Operators.&#40; &#124;&#124;&#62; &#41;&#60;'T1,'T2,'U&#62; 関数](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-hh%5d-%5d%5b%27t1%2c%27t2%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>左辺の 2 つの引数のタプルを右辺の関数に渡します。<br /></li></ul>|
|<code>&#124;&#124;&#124;></code>|[Operators.&#40; &#124;&#124;&#124;&#62; &#41;&#60;'T1,'T2,'T3,'U&#62; 関数](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-hhh%5d-%5d%5b%27t1%2c%27t2%2c%27t3%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>左辺の 3 つの引数のタプルを右辺の関数に渡します。<br /></li></ul>|
|`~~`|[演算子のオーバーロード](../operator-overloading.md)|<ul><li>単項否定演算子のオーバー ロードを宣言するために使用されます。<br /></li></ul>|
|`~~~`|[ビット処理演算子](bitwise-operators.md)|<ul><li>ビットごとの NOT 演算を実行します。<br /></li></ul>|
|`~-`|[演算子のオーバーロード](../operator-overloading.md)|<ul><li>単項マイナス演算子のオーバー ロードを宣言するために使用されます。<br /></li></ul>|
|`~+`|[演算子のオーバーロード](../operator-overloading.md)|<ul><li>単項プラス演算子のオーバー ロードを宣言するために使用されます。<br /></li></ul>|

## <a name="operator-precedence"></a>演算子の優先順位

次の表に、F# 言語で使用される演算子とその他の式のキーワードの優先順位を、低いものから順に示します。 該当する場合には、結合規則も示します。

|演算子|結合規則|
|--------|-------------|
|`as`|右|
|`when`|右|
|<code>&#124;</code> (パイプ)|左|
|`;`|右|
|`let`|非結合|
|`function`, `fun`, `match`, `try`|非結合|
|`if`|非結合|
|`not`|右|
|`->`|右|
|`:=`|右|
|`,`|非結合|
|`or`、 <code>&#124;&#124;</code>|左|
|`&`、 `&&`|左|
|`:>`、 `:?>`|右|
|`<`*op*、`>`*op*、`=`、<code>&#124;</code>*op*、`&`*op*、`&`<br /><br />(`<<<`、`>>>`、<code>&#124;&#124;&#124;</code>、`&&&` を含む)|左|
|`^`*op*<br /><br />(`^^^` を含む)|右|
|`::`|右|
|`:?`|非結合|
|`-`*op*、`+`*op*|これらのシンボルを挿入辞として使用するために適用|
|`*`*op*、`/`*op*、`%`*op*|左|
|`**`*op*|右|
|`f x` (関数適用)|左|
|<code>&#124;</code> (パターン一致)|右|
|前置演算子 (`+`*op*、`-`*op*、`%`、`%%`、`&`、`&&`、`!`*op*、`~`*op*)|左|
|`.`|左|
|`f(x)`|左|
|`f<`*types*`>`|左|

F# はカスタム演算子のオーバー ロードをサポートしています。 これは、独自の演算子を定義できることを意味します。 上記の表では、*op* に、組み込みまたはユーザー定義の有効な (場合によっては空の) 演算子文字シーケンスを指定できます。 つまり、この表を使用して、カスタム演算子に使用する文字のシーケンスを決定し、目的のレベルの優先順位を実現することができます。 先行する `.` 文字は、コンパイラが優先順位を決定する場合は無視されます。

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](../index.md)
- [演算子のオーバーロード](../operator-overloading.md)
