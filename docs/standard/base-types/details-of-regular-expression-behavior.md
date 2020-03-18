---
title: 正規表現の動作
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, behavior
- .NET Framework regular expressions, behavior
ms.assetid: 0ee1a6b8-caac-41d2-917f-d35570021b10
ms.openlocfilehash: 504e315dda4e76f56a88d97149b1515b6743668b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77124352"
---
# <a name="details-of-regular-expression-behavior"></a>正規表現の動作の詳細

.NET Framework の正規表現エンジンはバックトラッキング型の正規表現マッチャーであり、Perl、Python、Emacs、および Tcl で使われているのと同じ従来型の非決定性有限オートマトン (NFA) エンジンを採用しています。 このエンジンは、awk、egrep、または lex に見られるような、より高速であるが制限が多い、純粋な正規表現決定性有限オートマトン (DFA) エンジンとは異なります。 また、標準化されているが低速な POSIX NFA とも異なります。 次のセクションでは、これら 3 種類の正規表現エンジンについて説明し、.NET Framework の正規表現が従来型 NFA エンジンを使って実装されている理由について説明します。

## <a name="benefits-of-the-nfa-engine"></a>NFA エンジンの利点

 DFA エンジンがパターン一致を実行する場合、その処理順序は入力文字列によって決定されます。 このエンジンは入力文字列の先頭で開始し、順番に進んで、次の文字が正規表現パターンと一致するかどうかを判断します。 このエンジンでは、想定され得る最長の文字列を確実に検索できます。 同じ文字が 2 回テストされることはないため、DFA エンジンはバックトラッキングをサポートしません。 ただし、DFA エンジンには有限状態しか含まれないため、前方参照を使用してパターンを検索することはできません。また、明示的な展開が作成されないため、部分式をキャプチャできません。

 DFA エンジンとは異なり、従来型 NFA エンジンがパターン一致を実行する場合、その処理順序は正規表現パターンによって決定されます。 特定の言語要素を処理するときに、エンジンは最長一致を使用します。つまり、できるだけ多くの入力文字列と一致するようにします。 しかし、部分式の一致が見つかった後の状態も保存します。 最終的に一致が見つからなかった場合、エンジンは保存した状態に戻ることができるため、さらに照合を試行できます。 正規表現の後の言語要素も照合できるようにするために、この見つかった部分式の一致を破棄するプロセスを "*バックトラッキング*" と呼びます。 NFA エンジンは、バックトラッキングを使用して、ある正規表現で可能なすべての展開を特定の順序でテストし、最初に一致した文字列を採用します。 従来型 NFA エンジンでは、見つかった一致文字列の正規表現に固有の展開が作成されるため、部分式に一致する文字列と、一致する前方参照をキャプチャできます。 しかし、従来型 NFA ではバックトラックが行われるため、1 つの状態に到達する経路が複数ある場合には、同じ状態に何度も到達する可能性があります。 その結果、最悪の場合には指数関数的に実行速度が遅くなることがあります。 従来型 NFA エンジンでは、最初に見つかった一致文字列が採用されるため、その他の (おそらく、より長い) 一致文字列が見つからないままになる場合もあります。

 POSIX NFA エンジンは従来型 NFA エンジンと似ていますが、一致する最長の文字列が確実に見つかるまでバックトラックが継続される点が異なります。 その結果、POSIX NFA エンジンは従来型 NFA エンジンよりも実行速度が遅くなります。また、POSIX NFA エンジンを使用する場合は、バックトラッキング検索の順序を変更して、より短い一致文字列を長い一致文字列よりも優先させることはできません。

 従来型 NFA エンジンは、DFA エンジンや POSIX NFA エンジンよりも文字列の一致をより厳密に制御するため、プログラマに人気があります。 NFA エンジンは、最悪の場合には実行速度が遅くなることもありますが、あいまいさを少なくし、バックトラッキングを制限するパターンを使用すると、一致する文字列を線形時間または多項式時間で見つけるように調整できます。 言い換えると、NFA エンジンはパフォーマンスと引き換えに能力と柔軟性を向上させますが、ほとんどの場合、正規表現が適切に記述されていれば十分に許容できるパフォーマンスを実現でき、バックトラッキングによってパフォーマンスが指数関数的に低下する状況は回避されます。

> [!NOTE]
> 過度なバックトラッキングによって発生するパフォーマンスの低下と、そのような問題を回避する正規表現の作成方法については、「[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)」をご覧ください。

## <a name="net-framework-engine-capabilities"></a>.NET Framework エンジンの機能

 従来型の NFA エンジンの長所を利用するために、.NET Framework の正規表現エンジンには、プログラマがバックトラッキング エンジンを調整できるようにするための構成体セットが組み込まれています。 それらの構成体を使用すると、高速検索を実行したり、他の展開よりも特定の展開を優先させたりできます。

 .NET Framework の正規表現エンジンのその他の機能は次のとおりです。

- 最短一致の量指定子: `??`、`*?`、`+?`、`{`*n*`,`*m*`}?`。 これらの構成体は、バックトラッキング エンジンに対し、繰り返しの回数が最も少ない文字列を最初に検索するように指示します。 逆に、通常の最長一致の量指定子は、繰り返しの回数が最も多い文字列を最初に検索しようとします。 2 つの量指定子の動作の違いを次の例に示します。 正規表現は、数字で終わる文を照合し、キャプチャ グループはその数字を抽出します。 正規表現 `.+(\d+)\.` には最長一致の量指定子 `.+` が含まれます。これにより、正規表現エンジンは数字の最後の桁のみをキャプチャします。 対照的に、正規表現 `.+?(\d+)\.` には最短一致の量指定子 `.+?` が含まれます。これにより、正規表現エンジンは数字全体をキャプチャします。

     [!code-csharp[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lazy1.cs#1)]
     [!code-vb[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lazy1.vb#1)]

     この正規表現の最長一致バージョンと最短一致バージョンは、次の表に示すように定義されています。

    |パターン|説明|
    |-------------|-----------------|
    |`.+` (最長一致の量指定子)|任意の文字の 1 回以上の出現に一致します。 これにより、正規表現エンジンは文字列全体を照合してから、必要に応じてバックトラックし、パターンの残りの部分を照合します。|
    |`.+?` (最短一致の量指定子)|任意の文字の 1 回以上の出現 (ただし、可能な限り少ない回数) に一致します。|
    |`(\d+)`|1 文字以上の数字と一致し、その文字を最初のキャプチャ グループに代入します。|
    |`\.`|ピリオドと一致します。|

     最短一致の量指定子について詳しくは、「[限定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」をご覧ください。

- 肯定先読み: `(?=`*subexpression*`)`。 この機能により、バックトラッキング エンジンは部分式と一致する文字列を見つけた後で、テキスト内の同じ位置に戻ることができます。 同じ位置から開始する複数のパターンを確認してテキスト全体を検索する場合に便利です。 また、エンジンは、一致するテキストに部分文字列を含めずに、一致文字列の末尾に部分文字列が存在することを検証できます。 次の例では、肯定先読みを使用して、後に区切り記号が続かない文中の単語を抽出します。

     [!code-csharp[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead1.cs#2)]
     [!code-vb[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead1.vb#2)]

     正規表現 `\b[A-Z]+\b(?=\P{P})` は、次の表に示すように定義されています。

    |パターン|説明|
    |-------------|-----------------|
    |`\b`|ワード境界から照合を開始します。|
    |`[A-Z]+`|任意の英字と 1 回以上、一致します。 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションを使って呼び出されているため、比較では大文字と小文字が区別されません。|
    |`\b`|ワード境界で照合を終了します。|
    |`(?=\P{P})`|先読みして次の文字が区切り記号かどうかを判定します。 区切り記号でない場合は、一致と見なされます。|

     肯定先読みアサーションについて詳しくは、「[グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

- 否定先読み: `(?!`*subexpression*`)`。 この機能により、部分式に一致する文字列が見つからなかった場合にのみ、表現に一致できるようになります。 ある文字列を除外する表現の方が、含める表現よりも単純になることが多いため、この機能は検索を簡略化する場合に特に力を発揮します。 たとえば、"non" で始まらない単語を表す表現を記述するのは簡単ではありません。 次の例では、否定先読みを使用してこれらを除外します。

     [!code-csharp[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead2.cs#3)]
     [!code-vb[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead2.vb#3)]

     正規表現パターン `\b(?!non)\w+\b` は、次の表に示すように定義されています。

    |パターン|説明|
    |-------------|-----------------|
    |`\b`|ワード境界から照合を開始します。|
    |`(?!non)`|先読みして、現在の文字列が "non" で始まらないことを確認します。 "non" で始まる場合は、一致と見なされません。|
    |`(\w+)`|1 つ以上の単語文字に一致します。|
    |`\b`|ワード境界で照合を終了します。|

     否定先読みアサーションについて詳しくは、「[グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

- 条件付き評価: `(?(`*expression*`)`*yes*`|`*no*`)` および`(?(`*name*`)`*yes*`|`*no*`)`。ここで、*expression* は照合する部分式、*name* はキャプチャ グループの名前、*yes* は、*expression* が一致するか、または *name* が空でない有効なキャプチャ グループである場合に照合する文字列、*no* は、*expression* が一致しないか、または *name* が空でない有効なキャプチャ グループではない場合に照合する部分式です。 この機能により、エンジンは直前の部分式の一致結果またはゼロ幅アサーションの結果に従って、複数の代替パターンを使用した検索を実行できます。 そのため、より強力な前方参照が可能になります。たとえば、直前の部分式が一致したかどうかに基づいて部分式を照合できます。 次の例の正規表現は、パブリック使用と内部使用の両方を目的とした段落と一致します。 内部使用のみを目的とした段落は `<PRIVATE>` タグで始まります。 正規表現パターン `^(?<Pvt>\<PRIVATE\>\s)?(?(Pvt)((\w+\p{P}?\s)+)|((\w+\p{P}?\s)+))\r?$` は、条件付き評価を使用して、パブリック使用と内部使用を目的とした段落の内容を別のキャプチャ グループに代入します。 これらの段落は、異なる方法で処理できます。

     [!code-csharp[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/conditional1.cs#4)]
     [!code-vb[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/conditional1.vb#4)]

     正規表現パターンは、次の表に示すように定義されています。

    |パターン|説明|
    |-------------|-----------------|
    |`^`|行の先頭から照合を開始します。|
    |`(?<Pvt>\<PRIVATE\>\s)?`|文字列 `<PRIVATE>` の後に空白文字が続くパターンの 0 回または 1 回の出現と一致します。 一致文字列を `Pvt` という名前のキャプチャ グループに代入します。|
    |`(?(Pvt)((\w+\p{P}?\s)+)`|`Pvt` キャプチャ グループが存在する場合は、1 個以上の単語文字の後に 0 個または 1 個の区切り記号と 1 つの空白文字が続くパターンの 1 回以上の出現と一致します。 部分文字列を最初のキャプチャ グループに代入します。|
    |<code>&#124;((\w+\p{P}?\s)+))</code>|`Pvt` キャプチャ グループが存在しない場合は、1 個以上の単語文字の後に 0 個または 1 個の区切り記号と 1 つの空白文字が続くパターンの 1 回以上の出現と一致します。 部分文字列を 3 番目のキャプチャ グループに代入します。|
    |`\r?$`|行末または文字列の末尾と一致します。|

     条件付き評価について詳しくは、「[代替構成体](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md)」をご覧ください。

- グループ定義の均等化: `(?<`*name1*`-`*name2*`>` *subexpression*`)`。 この機能により、正規表現エンジンは、かっこや左右の角かっこなどの入れ子になった構成体を追跡できます。 例については、「[グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

- アトミック グループ: `(?>`*subexpression*`)`。 この機能により、バックトラッキング エンジンは、部分式と最初に一致した文字列だけを確実に検索できるようになります。この場合、表現は、部分式を含む表現とは関係ないように処理されます。 この構成体を使用しない場合は、より大きな表現によるバックトラッキング検索時に、部分式の動作が変化する可能性があります。 たとえば、正規表現 `(a+)\w` は、1 つ以上の "a" の文字と、"a" の文字のシーケンスに続く単語文字と一致し、最初のキャプチャ グループに "a" の文字のシーケンスを割り当てます。 ただし、入力文字列の最後の文字も "a" である場合は、`\w` 言語要素で照合され、キャプチャされたグループには含まれません。

     [!code-csharp[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking2.vb#7)]

     正規表現 `((?>a+))\w` では、この動作は回避されます。 連続するすべての "a" 文字はバックトラッキングなしで照合されるため、最初のキャプチャ グループにはすべての連続する "a" 文字が含まれます。 "a" 文字の後に "a" 以外の文字が少なくとも 1 つ続かない場合は、一致と見なされません。

     [!code-csharp[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking1.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking1.vb#8)]

     アトミック グループの詳細については、[グループ化構造体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)に関する記事を参照してください。

- 右から左への一致。<xref:System.Text.RegularExpressions.Regex> クラス コンストラクターまたは静的インスタンス一致メソッドに <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> オプションを設定すると指定されます。 この機能は、左から右ではなく右から左に向かって検索する場合や、パターンの左側ではなく右側で検索を開始した方が効果的な場合に便利です。 次の例に示すように、右から左への一致を使用すると、最長一致の量指定子の動作を変更できます。 この例では、数字で終わる文に対して 2 つの検索を実行します。 最長一致の量指定子 `+` を使用する左から右への検索では、文中の 6 桁の数字の 1 つと一致しますが、右から左への検索では 6 桁の数字すべてと一致します。 正規表現パターンの説明については、このセクションで前に示した最短一致の量指定子の例を参照してください。

     [!code-csharp[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/rtl1.cs#6)]
     [!code-vb[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/rtl1.vb#6)]

     右から左への一致の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」をご覧ください。

- 肯定および否定後読み: 肯定後読みの場合は `(?<=`*subexpression*`)`、否定後読みの場合は `(?<!`*subexpression*`)`。 この機能は、このトピックで前に説明した先読みと同様です。 正規表現エンジンでは、完全な右から左への一致を実行できるため、正規表現では制限のない後読みが可能です。 肯定および否定後読みを使用して、入れ子になった部分式が外側の式のスーパーセットである場合に、入れ子の量指定子を回避することもできます。 そのような入れ子の量指定子を使用した正規表現は、多くの場合にパフォーマンスを低下させます。 たとえば、次の例では、文字列が英数字で始まって英数字で終わること、および文字列内の他の文字がより大きなサブセットの 1 つであることを検証します。 電子メール アドレスの検証に使用される正規表現の一部になります。詳しくは、「[方法:文字列が有効な電子メール形式であるかどうかを検証する](../../../docs/standard/base-types/how-to-verify-that-strings-are-in-valid-email-format.md)」をご覧ください。

     [!code-csharp[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookbehind1.cs#5)]
     [!code-vb[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookbehind1.vb#5)]

     正規表現 ``^[A-Z0-9]([-!#$%&'.*+/=?^`{}|~\w])*(?<=[A-Z0-9])$`` は、次の表に示すように定義されています。

    |パターン|説明|
    |-------------|-----------------|
    |`^`|文字列の先頭から照合を開始します。|
    |`[A-Z0-9]`|任意の数字または英数字と一致します。 (比較では、大文字と小文字を区別しません。)|
    |<code>([-!#$%&'.*+/=?^\`{}&#124;~\w])\*</code>|任意の単語文字の 0 回以上の繰り返し、または次の文字のいずれかと一致します: -、!、#、$、%、&、'、.、\*、+、/、=、?、^、&#96;、{、}、&#124;、~。|
    |`(?<=[A-Z0-9])`|前の文字を後読みします。これは数字または英数字である必要があります。 (比較では、大文字と小文字を区別しません。)|
    |`$`|入力文字列の末尾で照合を終了します。|

     肯定および否定後読みについて詳しくは、「[グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

## <a name="related-articles"></a>関連記事

|Title|説明|
|-----------|-----------------|
|[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)|正規表現のバックトラッキングを使用して、分岐処理によって別の一致を検索する方法について説明します。|
|[コンパイルと再利用](../../../docs/standard/base-types/compilation-and-reuse-in-regular-expressions.md)|パフォーマンスを向上させるための正規表現のコンパイルと再利用について説明します。|
|[スレッド セーフ](../../../docs/standard/base-types/thread-safety-in-regular-expressions.md)|正規表現のスレッド セーフの詳細と、正規表現オブジェクトへのアクセスを同期することが必要なケースについて説明します。|
|[.NET Framework 正規表現](../../../docs/standard/base-types/regular-expressions.md)|正規表現のプログラミング言語的な面の概要について説明します。|
|[正規表現のオブジェクト モデル](../../../docs/standard/base-types/the-regular-expression-object-model.md)|正規表現クラスの使用方法について詳しく説明し、コード例を示します。|
|[正規表現の例](../../../docs/standard/base-types/regular-expression-examples.md)|一般的なアプリケーションで正規表現を使用するときのコード例を示します。|
|[正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)|正規表現の定義に使用できる一連の文字、演算子、および構成体について説明します。|

## <a name="reference"></a>関連項目

- <xref:System.Text.RegularExpressions?displayProperty=nameWithType>
