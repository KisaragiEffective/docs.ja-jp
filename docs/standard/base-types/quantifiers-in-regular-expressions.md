---
title: 量指定子 (正規表現)
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, quantifiers
- metacharacters, quantifiers
- minimal matching quantifiers
- quantifiers in regular expressions
- .NET Framework regular expressions, quantifiers
- quantifiers
- lazy quantifiers
ms.assetid: 36b81212-6511-49ed-a8f1-ff080415312f
ms.openlocfilehash: f1627248cbed0f03c6fb76ce660f9b2bf7764781
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160015"
---
# <a name="quantifiers-in-regular-expressions"></a>量指定子 (正規表現)
量指定子は、一致と見なされるために入力中に存在する必要がある文字、グループ、または文字クラスの出現数を指定します。  次の表に、.NET でサポートされている量指定子の一覧を示します。  
  
|最長一致の量指定子|最短一致の量指定子|説明|  
|-----------------------|---------------------|-----------------|  
|`*`|`*?`|0 回以上の繰り返しに一致します。|  
|`+`|`+?`|1 回以上の繰り返しに一致します。|  
|`?`|`??`|0 回または 1 回の繰り返しに一致します。|  
|`{` *n* `}`|`{` *n* `}?`|*n* 回の繰り返しに一致します。|  
|`{` *n* `,}`|`{` *n* `,}?`|*n* 回以上の繰り返しに一致します。|  
|`{` *n* `,` *m* `}`|`{` *n* `,` *m* `}?`|*n* 回から *m* 回の繰り返しに一致します。|  
  
 量 `n` および `m` は整数の定数です。 通常、量指定子は最長一致です。最長一致の場合、正規表現エンジンでは、特定のパターンの繰り返しができるだけ多くなるように照合が行われます。 量指定子に `?` 文字を付けると最短一致になります。最短一致の場合、正規表現エンジンでは、特定のパターンの繰り返しができるだけ少なくなるように照合が行われます。 最長一致と最短一致の量指定子の違いの詳細については、このトピックの「[最長一致と最短一致の量指定子](#Greedy)」のセクションをご覧ください。  
  
> [!IMPORTANT]
> 量指定子を入れ子にすると (たとえば、正規表現パターン `(a*)*` など)、入力文字列の文字数に応じて指数関数的に、正規表現エンジンで実行する必要がある比較の回数が増加する可能性があります。 この動作と回避方法の詳細については、[バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)に関するページを参照してください。  
  
## <a name="regular-expression-quantifiers"></a>正規表現の量指定子  
 以降のセクションでは、.NET の正規表現でサポートされている量指定子について説明します。  
  
> [!NOTE]
> 正規表現エンジンでは、正規表現パターンで *、+、?、{、および } の各文字を検出すると、[文字クラス](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)に含まれているもの以外は量指定子または量指定子コンストラクトの一部として解釈します。 文字クラスの外側でこれらをリテラル文字として解釈するには、文字の前に円記号を付けてエスケープする必要があります。 たとえば、正規表現パターン内の `\*` という文字列は、リテラルのアスタリスク ("\*") 文字と解釈されます。  
  
### <a name="match-zero-or-more-times-"></a>0 回以上の繰り返しに一致: *  
 `*` 量指定子は、直前の要素の 0 回以上の繰り返しに一致します。 これは `{0,}` 量指定子と同じです。 `*` は最長一致の量指定子であり、最短一致でこれに対応するのは `*?` です。  
  
 次の例は、この正規表現を示しています。 入力文字列の 9 個の数字のうち、5 個がパターンに一致し、4 個 (`95`、`929`、`9219`、および `9919`) が一致しません。  
  
 [!code-csharp[RegularExpressions.Quantifiers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#1)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`91*`|"9" の後に文字 "1" が 0 個以上続くパターンに一致します。|  
|`9*`|0 個以上の文字 "9" に一致します。|  
|`\b`|ワード境界で終了します。|  
  
### <a name="match-one-or-more-times-"></a>1 回以上の繰り返しに一致: +  
 `+` 量指定子は、直前の要素の 1 回以上の繰り返しに一致します。 これは `{1,}` と同じです。 `+` は最長一致の量指定子であり、最短一致でこれに対応するのは `+?` です。  
  
 たとえば、正規表現 `\ban+\w*?\b` は、文字 `a` で始まり、文字 `n` が 1 回以上繰り返されるすべての単語に一致を試みます。 次の例は、この正規表現を示しています。 この正規表現は、単語 `an`、`annual`、`announcement`、および `antique` に一致し、`autumn` と `all` では不一致となります。  
  
 [!code-csharp[RegularExpressions.Quantifiers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#2)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`an+`|"a" の後に文字 "n" が 1 個以上続くパターンに一致します。|  
|`\w*?`|単語に含まれる文字の 0 回以上の繰り返しのうち、最も少ない繰り返しに一致します。|  
|`\b`|ワード境界で終了します。|  
  
### <a name="match-zero-or-one-time-"></a>0 回または 1 回の繰り返しに一致: ?  
 `?` 量指定子は、直前の要素の 0 回または 1 回の繰り返しに一致します。 これは `{0,1}` と同じです。 `?` は最長一致の量指定子であり、最短一致でこれに対応するのは `??` です。  
  
 たとえば、正規表現 `\ban?\b` は、文字 `a` で始まり、文字 `n` が 0 回または 1 回繰り返されるすべての単語に一致を試みます。 つまり、単語 `a` と `an` に一致を試みます。 次の例は、この正規表現を示しています。  
  
 [!code-csharp[RegularExpressions.Quantifiers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#3)]
 [!code-vb[RegularExpressions.Quantifiers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#3)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`an?`|"a" の後に文字 "n" が 0 個または 1 個続くパターンに一致します。|  
|`\b`|ワード境界で終了します。|  
  
### <a name="match-exactly-n-times-n"></a>n 回の繰り返しに一致: {n}  
 `{`*n*`}` 量指定子は、直前の要素の *n* 回の繰り返しに一致します。ここで、*n* は任意の整数です。 `{`*n*`}` は最長一致の量指定子であり、最短一致でこれに対応するのは `{`*n*`}?` です。  
  
 たとえば、正規表現 `\b\d+\,\d{3}\b` は、ワード境界、1 個以上の 10 進数、3 個の 10 進数、ワード境界の順に続く文字に一致を試みます。 次の例は、この正規表現を示しています。  
  
 [!code-csharp[RegularExpressions.Quantifiers#4](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#4)]
 [!code-vb[RegularExpressions.Quantifiers#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#4)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`\d+`|1 個以上の 10 進数と一致します。|  
|`\,`|コンマ文字と一致します。|  
|`\d{3}`|3 個の 10 進数と一致します。|  
|`\b`|ワード境界で終了します。|  
  
### <a name="match-at-least-n-times-n"></a>n 回以上の繰り返しに一致: {n,}  
 `{`*n*`,}` 量指定子は、直前の要素の *n* 回以上の繰り返しに一致します。ここで、*n* は任意の整数です。 `{`*n*`,}` は最長一致の量指定子であり、最短一致でこれに対応するのは `{`*n*`,}?` です。  
  
 たとえば、正規表現 `\b\d{2,}\b\D+` は、ワード境界、2 個以上の 10 進数、ワード境界、数字以外の文字の順に続く文字に一致を試みます。 次の例は、この正規表現を示しています。 この正規表現は `"7 days"` という語句には一致しません。これは、10 進数が 1 個しか含まれていないためです。しかし、`"10 weeks and 300 years"` という語句には正常に一致します。  
  
 [!code-csharp[RegularExpressions.Quantifiers#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#5)]
 [!code-vb[RegularExpressions.Quantifiers#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#5)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`\d{2,}`|2 個以上の 10 進数と一致します。|  
|`\b`|ワード境界に一致します。|  
|`\D+`|1 個以上の 10 進数以外の数字と一致します。|  
  
### <a name="match-between-n-and-m-times-nm"></a>n 回から m 回までの繰り返しに一致: {n,m}  
 `{`*n*`,`*m*`}` 量指定子は、直前の要素の *n* 回以上、*m* 回以下の繰り返しに一致します。ここで、*n* と *m* は整数です。 `{`*n*`,`*m*`}` は最長一致の量指定子であり、最短一致でこれに対応するのは `{`*n*`,`*m*`}?` です。  
  
 次の例では、正規表現 `(00\s){2,4}` は、2 個の 0 と 1 個のスペースが、2 回以上 4 回以下だけ現れる文字列に一致を試みます。 入力文字列の最後の部分には、このパターンが最大の 4 回ではなく 5 回含まれていることに注意してください。 この部分文字列の最初の部分 (スペースと 5 つ目の 0 のペアまで) だけが正規表現パターンに一致します。  
  
 [!code-csharp[RegularExpressions.Quantifiers#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#6)]
 [!code-vb[RegularExpressions.Quantifiers#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#6)]  
  
### <a name="match-zero-or-more-times-lazy-match-"></a>0 回以上の繰り返しに一致 (最短一致): *?  
 `*?` 量指定子は、直前の要素の 0 回以上の繰り返しのうち、最も少ない繰り返しに一致します。 これは、最長一致の量指定子 `*` に対応する、最短一致の量指定子です。  
  
 次の例では、正規表現 `\b\w*?oo\w*?\b` は、文字列 `oo` を含むすべての単語に一致します。  
  
 [!code-csharp[RegularExpressions.Quantifiers#7](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#7)]
 [!code-vb[RegularExpressions.Quantifiers#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#7)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`\w*?`|単語に含まれる 0 個以上の文字のうち、最も少ない文字に一致します。|  
|`oo`|文字列 "oo" と一致します。|  
|`\w*?`|単語に含まれる 0 個以上の文字のうち、最も少ない文字に一致します。|  
|`\b`|ワード境界で終了します。|  
  
### <a name="match-one-or-more-times-lazy-match-"></a>1 回以上の繰り返しに一致 (最短一致): +?  
 `+?` 量指定子は、直前の要素の 1 回以上の繰り返しのうち、最も少ない繰り返しに一致します。 これは、最長一致の量指定子 `+` に対応する、最短一致の量指定子です。  
  
 たとえば、正規表現 `\b\w+?\b` は、ワード境界で区切られた 1 つ以上の文字に一致します。 次の例は、この正規表現を示しています。  
  
 [!code-csharp[RegularExpressions.Quantifiers#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#8)]
 [!code-vb[RegularExpressions.Quantifiers#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#8)]  
  
### <a name="match-zero-or-one-time-lazy-match-"></a>0 回または 1 回の繰り返しに一致 (最短一致): ??  
 `??` 量指定子は、直前の要素の 0 回または 1 回の繰り返しのうち、最も少ない繰り返しに一致します。 これは、最長一致の量指定子 `?` に対応する、最短一致の量指定子です。  
  
 たとえば、正規表現 `^\s*(System.)??Console.Write(Line)??\(??` は、文字列 "Console.Write" または "Console.WriteLine" に一致を試みます。 文字列には、"Console" の前に "System." が含まれていてもよく、最後に左かっこがあってもかまいません。 文字列は行の先頭にある必要がありますが、文字列の前に空白があってもかまいません。 次の例は、この正規表現を示しています。  
  
 [!code-csharp[RegularExpressions.Quantifiers#9](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#9)]
 [!code-vb[RegularExpressions.Quantifiers#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#9)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|入力ストリームの先頭と一致します。|  
|`\s*`|0 個以上の空白文字と一致します。|  
|`(System.)??`|文字列 "System." の 0 回または 1 回の繰り返しに一致します。|  
|`Console.Write`|文字列 "Console.Write" と一致します。|  
|`(Line)??`|文字列 "Line" の 0 回または 1 回の繰り返しに一致します。|  
|`\(??`|左かっこの 0 回または 1 回の繰り返しに一致します。|  
  
### <a name="match-exactly-n-times-lazy-match-n"></a>n 回の繰り返しに一致 (最短一致): {n}?  
 `{`*n*`}?` 量指定子は、直前の要素の `n` 回の繰り返しに一致します。ここで、*n* は任意の整数です。 これは、最長一致の量指定子 `{`*n*`}` に対応する、最短一致の量指定子です。  
  
 次の例では、正規表現 `\b(\w{3,}?\.){2}?\w{3,}?\b` を使用して Web サイトのアドレスを識別します。 "www.microsoft.com" と "msdn.microsoft.com" には一致しますが、"mywebsite" や "mycompany.com" には一致しない点に注意してください。  
  
 [!code-csharp[RegularExpressions.Quantifiers#10](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#10)]
 [!code-vb[RegularExpressions.Quantifiers#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#10)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`(\w{3,}?\.)`|単語に含まれる 3 個以上の文字のうち、最も少ない文字の後に、ドット (ピリオド) 文字が続くパターンに一致します。 これが最初のキャプチャ グループです。|  
|`(\w{3,}?\.){2}?`|最初のグループのパターンの 2 回の繰り返しのうち、最も少ない繰り返しに一致します。|  
|`\b`|ワード境界で照合を終了します。|  
  
### <a name="match-at-least-n-times-lazy-match-n"></a>n 回以上の繰り返しに一致 (最短一致): {n,}?  
 `{`*n*`,}?` 量指定子は、直前の要素の `n` 回以上の繰り返しのうち、最も少ない繰り返しに一致します。ここで、*n* は任意の整数です。 これは、最長一致の量指定子 `{`*n*`,}` に対応する、最短一致の量指定子です。  
  
 `{`*n*`}?` 量指定子の例については、前のセクションを参照してください。 この例の正規表現は、3 文字以上の後にピリオドが続く文字列に一致させるために `{`*n*`,}` 量指定子を使用しています。  
  
### <a name="match-between-n-and-m-times-lazy-match-nm"></a>n 回から m 回までの繰り返しに一致 (最短一致): {n,m}?  
 `{`*n*`,`*m*`}?` 量指定子は、直前の要素の `n` 回から `m` 回までの繰り返しのうち、最も少ない繰り返しに一致します。ここで、*n* と *m* は整数です。 これは、最長一致の量指定子 `{`*n*`,`*m*`}` に対応する、最短一致の量指定子です。  
  
 次の例では、正規表現 `\b[A-Z](\w*?\s*?){1,10}[.!?]` は、1 個以上 10 個以下の単語が含まれる文に一致します。 18 個の単語が含まれている 1 つの文を除き、入力文字列中のすべての文に一致します。  
  
 [!code-csharp[RegularExpressions.Quantifiers#12](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#12)]
 [!code-vb[RegularExpressions.Quantifiers#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#12)]  
  
 正規表現パターンは、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`[A-Z]`|A から Z の大文字と一致します。|  
|`(\w*?\s*?)`|0 個以上の単語文字の後に 1 個以上の空白文字続くパターンのうち、最も少ない繰り返しに一致します。 これが最初のキャプチャ グループです。|  
|`{1,10}`|前のパターンの 1 回以上 10 回以下に一致します。|  
|`[.!?]`|区切り文字 "."、"!"、"?" のいずれか 1 文字と一致します。|  
  
<a name="Greedy"></a>
## <a name="greedy-and-lazy-quantifiers"></a>最長一致と最短一致の量指定子  
 いくつかの量指定子には次の 2 つのバージョンがあります。  
  
- 最長一致バージョン。  
  
     最長一致の量指定子は、要素をできるだけ多く一致させようとします。  
  
- 最短一致バージョン。  
  
     最短一致の量指定子は、要素をできるだけ少なく一致させようとします。 最長一致の量指定子に `?` を追加するだけで最短一致の量指定子にすることができます。  
  
 クレジット カード番号などの数値の列から最後の 4 桁の数字を取り出す、単純な正規表現について考えてみます。 最長一致の `*` 量指定子を使用するバージョンの正規表現は、`\b.*([0-9]{4})\b` です。 しかし、2 個の数値が含まれている文字列の場合、この正規表現は、次の例に示すように 2 番目の数値の最後の 4 桁の数字だけに一致します。  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#1)]  
  
 この正規表現は最初の数値の一致に失敗します。`*` 量指定子では、直前の要素をできるだけ多く繰り返して文字列全体に一致させようとして、文字列の最後まで一致と見なされるためです。  
  
 これは期待した動作ではありません。 代わりに `*?` 最短一致の量指定子を使用すると、次の例のように両方の数値から数字を抽出できます。  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#2)]  
  
 多くの場合、最長一致と最短一致の量指定子を使用した正規表現は、同じ一致結果を返します。 異なる結果を返すことが最も多いのは、任意の文字に一致するワイルドカード (`.`) のメタ文字を使用した場合です。  
  
## <a name="quantifiers-and-empty-matches"></a>量指定子と空一致  
 量指定子 `*`、`+`、および `{`*n*`,`*m*`}` と、これらに対応する最短一致の量指定子は、最小回数のキャプチャが見つかった場合、空一致の後には繰り返されません。 この規則により、可能なグループ キャプチャの最大回数が無限またはほぼ無限のときに、量指定子が空の部分式一致の無限ループに入ることを回避できます。  
  
 たとえば、次のコードは、0 個または 1 個の文字 "a" と 0 回以上一致する正規表現パターン `(a?)*` を指定して <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> メソッドを呼び出した結果を表示します。 単一のキャプチャ グループによって各 "a" と <xref:System.String.Empty?displayProperty=nameWithType> がキャプチャされますが、2 番目の空一致はないことに注意してください。これは、最初の空一致により量指定子が繰り返しを停止するためです。  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch1.vb#1)]  
  
 最小回数と最大回数のキャプチャを定義するキャプチャ グループと固定回数のキャプチャを定義するキャプチャ グループとの間の実際の違いを確認するために、正規表現パターンの `(a\1|(?(1)\1)){0,2}` と `(a\1|(?(1)\1)){2}` について検討します。 どちらの正規表現も、次の表に示すように定義される単一のキャプチャ グループで構成されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`(a\1`|"a" と最初のキャプチャ グループの値に一致します …|  
|<code>&#124;(?(1)</code>|… または、最初のキャプチャ グループが定義されているかどうかをテストします。 (`(?(1)` コンストラクトではキャプチャ グループは定義されないことに注意してください。)|  
|`\1))`|最初のキャプチャ グループが存在する場合、その値と一致します。 グループが存在しない場合、そのグループは <xref:System.String.Empty?displayProperty=nameWithType> と一致します。|  
  
 最初の正規表現では 0 ～ 2 回、このパターンとの照合が行われます。2 番目の正規表現では、厳密に 2 回です。 最初のパターンは <xref:System.String.Empty?displayProperty=nameWithType> の最初のキャプチャでキャプチャの最小回数に達するため、`a\1` との照合は繰り返されません。`{0,2}` 量指定子では、最後の繰り返しでの空一致だけが許可されます。 一方、2 番目の正規表現では、2 回目の `a\1` が評価されるため、"a" に一致します。繰り返しの最小回数は 2 で、空一致の後でエンジンが繰り返さなければならない回数になります。  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch4.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch4.vb#2)]  
  
## <a name="see-also"></a>関連項目

- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)
