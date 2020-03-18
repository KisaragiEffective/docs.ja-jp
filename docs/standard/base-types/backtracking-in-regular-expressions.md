---
title: .NET 正規表現におけるバックトラッキング
description: 正規表現のパターン マッチングでバックトラッキングを制御する方法について説明します。
ms.date: 11/12/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework regular expressions, backtracking
- alternative matching patterns
- optional matching patterns
- searching with regular expressions, backtracking
- pattern-matching with regular expressions, backtracking
- backtracking
- regular expressions [.NET Framework], backtracking
- strings [.NET Framework], regular expressions
- parsing text with regular expressions, backtracking
ms.assetid: 34df1152-0b22-4a1c-a76c-3c28c47b70d8
ms.openlocfilehash: 1b61cc88de4f73abfe6d8e77f8f32c2c71e70a9d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78158065"
---
# <a name="backtracking-in-regular-expressions"></a>正規表現におけるバックトラッキング
バックトラッキングは、正規表現パターンに省略可能な[量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)または[代替構成体](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md)が含まれている場合に発生します。この場合、正規表現エンジンは、一致の検索を継続するために、以前に保存した状態に戻ります。 バックトラッキングは、正規表現を強力にするための中心的な機能で、これにより、非常に複雑なパターンを照合できる強力かつ柔軟な正規表現を作成できるようになります。 その一方で、バックトラッキングにはマイナス面もあり、 多くの場合、正規表現エンジンのパフォーマンスを左右する最大の要因になります。 さいわい、正規表現エンジンの動作とバックトラッキングの使用方法は開発者が制御できます。 ここでは、バックトラッキングの動作のしくみと、バックトラッキングを制御する方法について説明します。  
  
> [!NOTE]
> 一般に、.NET 正規表現エンジンのような非決定性有限オートマトン (NFA: Nondeterministic Finite Automaton) エンジンでは、効率的かつ高速な正規表現を作成する責任は開発者にあります。  

## <a name="linear-comparison-without-backtracking"></a>バックトラッキングを使用しない直線的な比較  
 正規表現パターンに省略可能な量指定子や代替構成体が含まれていない場合、正規表現エンジンは線形時間で実行されます。 つまり、パターンの最初の言語要素を入力文字列のテキストと照合し、パターンの次の言語要素を入力文字列の次の文字または文字グループと照合するという操作が、 照合が最終的に成功または失敗するまで繰り返されます。 いずれの場合も、正規表現エンジンは入力文字列内を 1 文字ずつ進みます。  
  
 具体的な例を次に示します。 正規表現 `e{2}\w\b` は、文字 "e" が 2 回出現した後に任意の単語文字とワード境界が続くパターンを検索します。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking1.vb#1)]  
  
 この正規表現には量指定子 `{2}`が含まれていますが、この量指定子は直線的に評価されます。 正規表現エンジンがバックトラックしないのは、 `{2}` が省略可能な量指定子ではないからです。この量指定子は、前の部分式が一致する必要がある正確な回数 (可変の回数ではなく) を指定しています。 結果として、正規表現エンジンは、次の表に示すような方法でこの正規表現パターンを入力文字列と照合します。  
  
|操作|パターン内の位置|文字列内の位置|結果|  
|---------------|-------------------------|------------------------|------------|  
|1|e|"needing a reed" (インデックス 0)|一致なし。|  
|2|e|"eeding a reed" (インデックス 1)|一致候補。|  
|3|e{2}|"eding a reed" (インデックス 2)|一致候補。|  
|4|\w|"ding a reed" (インデックス 3)|一致候補。|  
|5|\b|"ing a reed" (インデックス 4)|一致候補の失敗。|  
|6|e|"eding a reed" (インデックス 2)|一致候補。|  
|7|e{2}|"ding a reed" (インデックス 3)|一致候補の失敗。|  
|8|e|"ding a reed" (インデックス 3)|照合の失敗。|  
|9|e|"ing a reed" (インデックス 4)|一致なし。|  
|10|e|"ng a reed" (インデックス 5)|一致なし。|  
|11|e|"g a reed" (インデックス 6)|一致なし。|  
|12|e|" a reed" (インデックス 7)|一致なし。|  
|13|e|"a reed" (インデックス 8)|一致なし。|  
|14|e|" reed" (インデックス 9)|一致なし。|  
|16|e|"reed" (インデックス 10)|一致なし。|  
|16|e|"eed" (インデックス 11)|一致候補。|  
|17|e{2}|"ed" (インデックス 12)|一致候補。|  
|18|\w|"d" (インデックス 13)|一致候補。|  
|19|\b|"" (インデックス 14)|一致。|  
  
 正規表現パターンに省略可能な量指定子や代替構成体が含まれていない場合、正規表現パターンと入力文字列の照合に必要な比較の最大数は、入力文字列の文字数にほぼ等しくなります。 この場合、正規表現エンジンは、この 13 文字の文字列の一致候補を特定するために 19 回の比較を使用します。  つまり、正規表現エンジンは、省略可能な量指定子や代替構成体が含まれていない場合にはほぼ線形時間で実行されます。

## <a name="backtracking-with-optional-quantifiers-or-alternation-constructs"></a>省略可能な量指定子または代替構成体によるバックトラッキング  
 正規表現に省略可能な量指定子または代替構成体が含まれている場合、入力文字列の評価は直線的ではなくなります。 NFA エンジンによるパターン一致の動作は、照合する入力文字列内の文字ではなく、正規表現内の言語要素によって決まります。 したがって、正規表現エンジンは、省略可能な部分式や代替の部分式を完全に照合しようとします。 部分式内の次の言語要素に進んで照合が失敗した場合は、正規表現全体を入力文字列と照合するために、それまでに見つかった部分的な一致を放棄して、以前に保存した状態に戻ることもあります。 このように、一致を見つけるために以前に保存した状態に戻るプロセスを、バックトラッキングと呼びます。  
  
 例として、 `.*(es)`という正規表現パターンについて見てみましょう。この正規表現パターンは、文字 "es" と、それに先行するすべての文字に一致します。 次の例に示すように、入力文字列が "Essential services are provided by regular expressions." の場合、このパターンは、"expressions" の "es" までの文字列全体に一致します。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking2.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking2.vb#2)]  
  
 この場合、正規表現エンジンでは次のようにバックトラッキングが使用されます。  
  
- `.*` (任意の文字の 0 回以上の出現に一致します) を入力文字列全体と照合します。  
  
- 正規表現パターンの "e" との照合を試みますが、 入力文字列にはもう照合する文字がありません。  
  
- 最後に成功した照合 ("Essential services are provided by regular expressions.") にバックトラックして、"e" を文末のピリオドと照合します。 照合に失敗します。  
  
- 成功した前の照合へのバックトラッキングを一度に 1 文字ずつ繰り返して、暫定的に一致する部分文字列 "Essential services are provided by regular expr" に到達します。 その後、パターンの "e" を "expressions" の 2 つ目の "e" と比較して、一致を見つけます。  
  
- パターンの "s" を、一致した文字 "e" の後の "s" ("expressions" の 1 つ目の "s") と比較します。 照合に成功します。  
  
 バックトラッキングを使用した場合、この正規表現パターンを 55 文字の入力文字列と照合するのに 67 回の比較操作が必要になります。 一般に、正規表現パターンに 1 つの代替構成体または省略可能な量指定子が含まれている場合、パターンの照合に必要な比較操作の回数は入力文字列の文字数の 2 倍以上になります。

## <a name="backtracking-with-nested-optional-quantifiers"></a>入れ子になった省略可能な量指定子によるバックトラッキング  
 パターンに多数の代替構成体、入れ子になった代替構成体、または (最もよくあるケースとして) 入れ子になった省略可能な量指定子が含まれていると、正規表現パターンの照合に必要な比較操作の回数は指数関数的に増加する可能性があります。 例として、1 つ以上の文字 "a" から成る文字列全体に一致するように作られた `^(a+)+$` という正規表現パターンについて見てみましょう。 次の例では、同じ長さの 2 つの入力文字列が渡されていますが、パターンに一致するのは 1 つ目の文字列だけです。 <xref:System.Diagnostics.Stopwatch?displayProperty=nameWithType> クラスは、照合操作にかかる時間を調べるために使用されています。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking3.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking3.vb#3)]  
  
 出力を見るとわかるように、入力文字列がパターンに一致しない場合の処理時間は一致する場合の約 2 倍になります。 これは、照合の失敗は常に最悪のシナリオに相当するためです。 正規表現エンジンは、照合の失敗という結論に到達するまでに、正規表現を使用してデータのすべての可能なパスを辿らなければなりません。かっこが入れ子になっていると、データのパスは大幅に増加します。 2 つ目の文字列がパターンに一致しないという結論に到達するまでに正規表現エンジンによって行われる操作を以下に示します。  
  
- 文字列の先頭にいることを確認し、文字列の最初の 5 文字をパターン `a+`と照合します。 次に、文字 "a" のグループが文字列内にそれ以上ないことを特定します。 最後に、文字列の終わりをテストします。 文字列にまだ文字が 1 文字残っているため、照合は失敗します。 この照合の失敗に必要な比較は 9 回です。 正規表現エンジンは、"a" (以降は照合 1 と呼びます)、"aa" (照合 2)、"aaa" (照合 3)、および "aaaa" (照合 4) の各照合の状態情報を保存します。  
  
- 以前に保存した照合 4 に戻ります。 追加のキャプチャ グループに割り当てる必要がある文字 "a" があと 1 つあることを特定します。 最後に、文字列の終わりをテストします。 文字列にまだ文字が 1 文字残っているため、照合は失敗します。 この照合の失敗に必要な比較は 4 回です。 これまでに実行された比較は合計 13 回になります。  
  
- 以前に保存した照合 3 に戻ります。 追加のキャプチャ グループに割り当てる必要がある文字 "a" があと 2 つあることを特定しますが、 文字列の終わりのテストに失敗します。 その後、照合 3 に戻り、2 つの追加のキャプチャ グループの 2 つの追加の文字 "a" を照合しようとしますが、 ここでも文字列の終わりのテストに失敗します。 これらの照合の失敗に必要な比較は 12 回です。 これまでに実行された比較は合計 25 回になります。  
  
 このようにして、正規表現エンジンがすべての可能な一致の組み合わせを試行し終わるまで入力文字列と正規表現の比較が続けられます。一致しないという結論に到達するのはその後です。 この比較は、入れ子になった量指定子があるため、O(2<sup>n</sup>) (指数演算、*n* は入力文字列の文字数) です。 そのため、最悪のケースでは、30 文字の入力文字列で約 1,073,741,824 回、40 文字の入力文字列では約 1,099,511,627,776 回の比較が必要になります。 使用する文字列の長さがこのレベル以上になると、正規表現パターンに一致しない入力を処理するときに、正規表現メソッドの完了までに膨大な時間がかかる可能性があります。

## <a name="controlling-backtracking"></a>バックトラッキングの制御  
 バックトラッキングを使用すると、強力かつ柔軟な正規表現を作成できますが、 前のセクションで見たように、受け入れられないほどのパフォーマンスの低下が伴うことがあります。 過度なバックトラッキングを回避するには、 <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化したり静的な正規表現の一致メソッドを呼び出したりするときに、タイムアウト間隔を定義する必要があります。 これについては、次のセクションで説明します。 また、.NET では、バックトラッキングを制限または抑制する 3 つの正規表現言語要素がサポートされています。これらを使用すると、パフォーマンスをほとんど低下させずに複雑な正規表現を使用できます。それらの言語要素とは、[アトミック グループ](#atomic-groups)、[後読みアサーション](#lookbehind-assertions)、および[先読みアサーション](#lookahead-assertions)です。 各言語要素の詳細については、[コンストラクトのグループ化](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)に関するページを参照してください。  

### <a name="defining-a-time-out-interval"></a>タイムアウト間隔の定義  
 .NET Framework 4.5 以降では、試行が中止されて <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 例外がスローされるまでの、正規表現エンジンが単一の一致を検索する最長間隔を表すタイムアウト値を設定できます。 タイムアウト間隔を指定するには、インスタンス正規表現の <xref:System.TimeSpan> コンストラクターに <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType> 値を指定します。 また、各静的パターン一致メソッドには、 <xref:System.TimeSpan> パラメーターを持つオーバーロードがあり、これを使用してタイムアウト値を指定できます。 既定のタイムアウト間隔は <xref:System.Text.RegularExpressions.Regex.InfiniteMatchTimeout?displayProperty=nameWithType> に設定されており、正規表現エンジンはタイムアウトしません。  
  
> [!IMPORTANT]
> 正規表現がバックトラッキングに依存する場合は、常にタイムアウト間隔を設定することをお勧めします。  
  
 <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 例外では、指定されたタイムアウト間隔中に正規表現エンジンで一致を見つけられなかったことを示しますが、例外がスローされた理由は示しません。 原因は、通常は過度なバックトラッキングですが、例外がスローされたときのシステムの負荷に対して、タイムアウト間隔の設定が短すぎた可能性もあります。 例外を処理するときに、入力文字列との一致の検索をやめるか、タイムアウト間隔を延長して一致操作を再試行するかを選択できます。  
  
 たとえば、次のコードは <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType> コンストラクターを呼び出して、1 秒のタイムアウト値を持つ <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化します。 正規表現パターン `(a+)+$`は、行の末尾にある 1 つ以上の "a" 文字の 1 つ以上のシーケンスに一致しますが、過度なバックトラッキングの対象になります。 この例では、 <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> がスローされたら、タイムアウト値を 3 秒の最大間隔まで大きくします。 その後、パターンに一致させる試行を中止します。  
  
 [!code-csharp[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/cs/ctor1.cs#1)]
 [!code-vb[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/vb/ctor1.vb#1)]  

### <a name="atomic-groups"></a>アトミック グループ
 `(?>` *subexpression*`)` 言語要素を使用すると、部分式でのバックトラッキングが抑制されます。 一致すると、後続のバックトラッキングに対して一致したものが破棄されなくなります。 たとえば、パターン `(?>\w*\d*)1` で `1` が一致しない場合、`1` で一致することがわかっていても `\d*` の一致は破棄されません。 アトミック グループは、照合の失敗に関連するパフォーマンスの問題の防止に役立ちます。
  
 次の例は、入れ子になった量指定子を使用している場合にバックトラッキングを抑制すると、いかにパフォーマンスが向上するかを示しています。 この例では、入力文字列が 2 つの正規表現に一致しないことが正規表現エンジンによって確認されるまでの時間を測定しています。 1 つ目の正規表現は、1 つ以上の 16 進数、コロン、1 つ以上の 16 進数、2 つのコロンというパターンを 1 つ以上含む文字列を、バックトラッキングを使用して照合します。 2 つ目の正規表現は、バックトラッキングを無効にする以外は 1 つ目と同じです。 出力を見るとわかるように、バックトラッキングを無効にするとパフォーマンスが大幅に向上します。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking4.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking4.vb#4)]  

### <a name="lookbehind-assertions"></a>後読みアサーション  
 .NET には、入力文字列の前の文字と一致する 2 つの言語要素 `(?<=`*subexpression*`)` と `(?<!`*subexpression*`)` が含まれています。 これらの言語要素は、どちらもゼロ幅アサーションです。つまり、現在の文字の直前にある文字が *subexpression*に一致するかどうかを、前進もバックトラッキングもせずに確認します。  
  
 `(?<=` *subexpression* `)` は肯定後読みアサーションで、現在の位置の前にある文字が *subexpression* に一致する必要があります。 `(?<!`*subexpression*`)` は否定後読みアサーションで、現在の位置の前にある文字が *subexpression*に一致しない必要があります。 否定と肯定のどちらの後読みアサーションも、 *subexpression* が前の部分式のサブセットである場合に特に役立ちます。  
  
 次の例では、メール アドレスのユーザー名を検証する 2 つの同等の正規表現パターンが使われています。 1 つ目のパターンでは、過度なバックトラッキングのためにパフォーマンスが低下します。 2 つ目のパターンでは、1 つ目の正規表現に変更を加えて、入れ子になった量指定子を肯定後読みアサーションに置き換えています。 この例の出力には、 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドの実行時間が表示されます。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking5.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking5.vb#5)]  
  
 最初の正規表現パターン `^[0-9A-Z]([-.\w]*[0-9A-Z])*@`は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|文字列の先頭から照合を開始します。|  
|`[0-9A-Z]`|英数字 1 文字に一致します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションを使用して呼び出されているため、この比較では大文字と小文字が区別されません。|  
|`[-.\w]*`|ハイフン、ピリオド、または単語文字の 0 回以上の出現に一致します。|  
|`[0-9A-Z]`|英数字 1 文字に一致します。|  
|`([-.\w]*[0-9A-Z])*`|0 個以上のハイフン、ピリオド、または単語文字の後に英数字が続く組み合わせの 0 回以上の出現に一致します。 これが最初のキャプチャ グループです。|  
|`@`|アット マーク ("\@") に一致します。|  
  
 2 つ目の正規表現パターン `^[0-9A-Z][-.\w]*(?<=[0-9A-Z])@`では、肯定後読みアサーションが使用されています。 このパターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|文字列の先頭から照合を開始します。|  
|`[0-9A-Z]`|英数字 1 文字に一致します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションを使用して呼び出されているため、この比較では大文字と小文字が区別されません。|  
|`[-.\w]*`|ハイフン、ピリオド、または単語文字の 0 回以上の出現と一致します。|  
|`(?<=[0-9A-Z])`|最後に照合された文字を後読みして、英数字だった場合は照合を継続します。 英数字は、ピリオド、ハイフン、およびすべての単語文字から成るセットのサブセットです。|  
|`@`|アット マーク ("\@") に一致します。|  

### <a name="lookahead-assertions"></a>先読みアサーション  
 .NET には、入力文字列の次の文字と一致する 2 つの言語要素 `(?=`*subexpression*`)` と `(?!`*subexpression*`)` が含まれています。 これらの言語要素は、どちらもゼロ幅アサーションです。つまり、現在の文字の直後にある文字が *subexpression*に一致するかどうかを、前進もバックトラッキングもせずに確認します。  
  
 `(?=` *subexpression* `)` は肯定先読みアサーションで、現在の位置の後にある文字が *subexpression* に一致する必要があります。 `(?!`*subexpression*`)` は否定先読みアサーションで、現在の位置の後にある文字が *subexpression*に一致しない必要があります。 肯定と否定のどちらの先読みアサーションも、 *subexpression* が次の部分式のサブセットである場合に特に役立ちます。  
  
 次の例では、完全修飾型名を検証する 2 つの同等の正規表現パターンが使用されています。 1 つ目のパターンでは、過度なバックトラッキングのためにパフォーマンスが低下します。 2 つ目のパターンでは、1 つ目の正規表現に変更を加えて、入れ子になった量指定子を肯定先読みアサーションに置き換えています。 この例の出力には、 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドの実行時間が表示されます。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking6.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking6.vb#6)]  
  
 最初の正規表現パターン `^(([A-Z]\w*)+\.)*[A-Z]\w*$`は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|文字列の先頭から照合を開始します。|  
|`([A-Z]\w*)+\.`|1 個の英文字 (A-Z) の後に 0 個以上の単語文字が続くパターンの 1 回以上の繰り返しの後にピリオドが続くパターンに一致します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションを使用して呼び出されているため、この比較では大文字と小文字が区別されません。|  
|`(([A-Z]\w*)+\.)*`|前のパターンの 0 回以上の繰り返しに一致します。|  
|`[A-Z]\w*`|1 個の英文字の後に 0 個以上の単語文字が続くパターンに一致します。|  
|`$`|入力文字列の末尾で照合を終了します。|  
  
 2 つ目の正規表現パターン `^((?=[A-Z])\w+\.)*[A-Z]\w*$`では、肯定先読みアサーションが使用されています。 このパターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|文字列の先頭から照合を開始します。|  
|`(?=[A-Z])`|先頭の文字を先読みして、英文字 (A-Z) だった場合は照合を継続します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションを使用して呼び出されているため、この比較では大文字と小文字が区別されません。|  
|`\w+\.`|1 個以上の単語文字の後にピリオドが続くパターンに一致します。|  
|`((?=[A-Z])\w+\.)*`|1 個以上の単語文字の後にピリオドが続くパターンの 0 回以上の繰り返しと一致します。 最初の単語文字は英文字でなければなりません。|  
|`[A-Z]\w*`|1 個の英文字の後に 0 個以上の単語文字が続くパターンに一致します。|  
|`$`|入力文字列の末尾で照合を終了します。|  
  
## <a name="see-also"></a>関連項目

- [.NET の正規表現](../../../docs/standard/base-types/regular-expressions.md)
- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)
- [代替構成体](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md)
- [グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)
