---
title: .NET 正規表現でのアンカー
description: 正規表現パターンでアンカーを使用する方法について説明します。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- atomic zero-width assertions
- regular expressions, anchors
- regular expressions, atomic zero-width assertions
- anchors, in regular expressions
- metacharacters, atomic zero-width assertions
- metacharacters, anchors
- .NET Framework regular expressions, anchors
- .NET Framework regular expressions, atomic zero-width assertions
ms.assetid: 336391f6-2614-499b-8b1b-07a6837108a7
ms.openlocfilehash: c4853a6854f5da1a3217c976a03ddbde3b528560
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159664"
---
# <a name="anchors-in-regular-expressions"></a>正規表現のアンカー
アンカー (アトミック ゼロ幅アサーション) は、文字列が一致する位置を指定します。 検索式でアンカーを使用した場合、正規表現エンジンは、後方の文字列を読み込んだり、文字に一致させたりすることはしません。指定された位置での一致のみが検索されます。 たとえば、 `^` は、行または文字列の先頭に一致する必要があることを指定します。 したがって、正規表現 `^http:` は、"http:" が行の先頭にある場合にのみ一致します。 次の表は、.NET の正規表現でサポートされているアンカーの一覧です。  
  
|アンカー|説明|  
|------------|-----------------|  
|`^`|既定では、文字列の先頭で一致する必要があります。複数行モードでは、行の先頭で一致する必要があります。 詳細については、「 [文字列または行の先頭](#start-of-string-or-line-)」を参照してください。|  
|`$`|既定では、文字列の末尾で一致するか、文字列の末尾にある `\n` の前で一致する必要があります。複数行モードでは、行の末尾で一致するか、行の末尾にある `\n` の前で一致する必要があります。 詳細については、「 [文字列または行の末尾](#end-of-string-or-line-)」を参照してください。|  
|`\A`|文字列の先頭にのみ一致します (複数行はサポートされません)。 詳細については、「 [文字列の先頭のみ](#start-of-string-only-a)」を参照してください。|  
|`\Z`|文字列の末尾で一致するか、文字列の末尾にある `\n` の前に一致します。 詳細については、「 [文字列の末尾または末尾の改行の前](#end-of-string-or-before-ending-newline-z)」を参照してください。|  
|`\z`|文字列の末尾にのみ一致します。 詳細については、「 [文字列の末尾のみ](#end-of-string-only-z)」を参照してください。|  
|`\G`|前回の一致が終了した位置に一致します。 詳細については、「 [連続一致](#contiguous-matches-g)」を参照してください。|  
|`\b`|ワード境界に一致します。 詳細については、「 [ワード境界](#word-boundary-b)」を参照してください。|  
|`\B`|ワード境界以外に一致します。 詳細については、「 [ワード境界以外](#non-word-boundary-b)」を参照してください。|  

## <a name="start-of-string-or-line-"></a>文字列または行の先頭: ^  
 既定では、`^` アンカーは、その後に続くパターンが、文字列の最初の文字位置から始まる必要があることを指定します。 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションを指定して `^` を使用した場合は (「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」を参照)、各行の先頭に一致します。  
  
 次の例では、正規表現で `^` アンカーを使用して、プロ野球チームが存続した年数に関する情報を抽出します。 この例では、 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドの 2 つのオーバーロードを呼び出しています。  
  
- <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29> オーバーロードの呼び出しでは、入力文字列内で正規表現パターンに一致する最初の部分文字列が検索されます。  
  
- <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29> パラメーターを `options` に設定した <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オーバーロードの呼び出しでは、5 つのすべての部分文字列が検索されます。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring1.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring1.vb#1)]  
  
 正規表現パターン `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+` は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`^`|入力文字列の先頭から (または、 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションを指定してメソッドが呼び出された場合は行の先頭から) 照合を開始します。|  
|`((\w+(\s?)){2,}`|1 個以上の単語文字の後に 0 個または 1 個の空白が続くパターンが少なくとも 2 回繰り返される部分に一致します。 これが最初のキャプチャ グループです。 この式では、2 番目と 3 番目のキャプチャ グループも定義されます。2 番目はキャプチャ ワードで、3 番目はキャプチャされた空白で構成されます。|  
|`,\s`|コンマおよびそれに続く空白文字に一致します。|  
|`(\w+\s\w+)`|1 個以上の単語文字の後に 1 個の空白が続き、さらに 1 個以上の単語文字が続くパターンに一致します。 これが 4 番目のキャプチャ グループです。|  
|`,`|コンマに一致します。|  
|`\s\d{4}`|1 個の空白の後に 4 個の 10 進数字が続くパターンに一致します。|  
|<code>(-(\d{4}&#124;present))?</code>|1 個のハイフンの後に 4 個の 10 進数字または文字列 "present" が続くパターンの、0 回または 1 回の繰り返しに一致します。 これが 6 番目のキャプチャ グループです。 7 番目のキャプチャ グループも含まれています。|  
|`,?`|コンマの 0 回または 1 回の繰り返しに一致します。|  
|<code>(\s\d{4}(-(\d{4}&#124;present))?,?)+</code>|1 個のスペース、4 個の 10 進数字、1 個のハイフンの後に 4 個の 10 進数字または文字列 "present" が続くパターンの 0 回または 1 回の繰り返し、0 個または 1 個のコンマが並んだパターンの、1 回以上の繰り返しに一致します。 これが 5 番目のキャプチャ グループです。|

## <a name="end-of-string-or-line-"></a>文字列または行の末尾: $  
 `$` アンカーは、その前にあるパターンが、入力文字列の末尾、または入力文字列の末尾にある `\n` の前で一致する必要があることを指定します。  
  
 `$` オプションを指定して <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> を使用した場合は、行の末尾にも一致します。 `$` は、 `\n` に一致しますが、 `\r\n` (復帰文字と改行文字の組み合わせ、つまり CR/LF) には一致しないことに注意してください。 CR/LF 文字の組み合わせに一致させるには、正規表現パターンに `\r?$` を含めます。  
  
 次の例では、「 `$` 文字列または行の先頭 [」の例で使用した正規表現パターンに](#start-of-string-or-line-) アンカーを追加しています。 5 つのテキスト行を含む元の入力文字列に対して使用した場合、 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドは一致を見つけることができません。これは、最初の行の末尾が `$` パターンに一致しないためです。 元の入力文字列を文字列配列に分割すると、 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドは 5 つの各行の一致に成功します。 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> パラメーターを `options` に設定して <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>メソッドを呼び出すと、一致は見つかりません。これは、正規表現パターンで復帰要素 (\u+000D) が考慮されないためです。 ただし、 `$` を `\r?$`に置き換え、 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> パラメーターを `options` に設定して <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> メソッドを呼び指すと、正規表現パターンが変更され、再び 5 つの一致が検索されるようになります。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring1.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring1.vb#2)]

## <a name="start-of-string-only-a"></a>文字列の先頭のみ: \A  
 `\A` アンカーは、入力文字列の先頭に一致する必要があることを指定します。 これは `^` アンカーと同じですが、 `\A` では <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションが無視される点が異なります。 したがって、複数行の入力文字列でも最初の行の先頭にのみ一致することができます。  
  
 次の例は、 `^` アンカーおよび `$` アンカーの例と似ています。 この例では、正規表現で `\A` アンカーを使用して、プロ野球チームが存続した年数に関する情報を抽出します。 入力文字列には 5 つの行が含まれます。 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> メソッドを呼び出すと、入力文字列内で正規表現パターンに一致する最初の部分文字列のみが検索されます。 この例からわかるように、 <xref:System.Text.RegularExpressions.RegexOptions.Multiline> オプションの効果はありません。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring2.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring2.vb#3)]

## <a name="end-of-string-or-before-ending-newline-z"></a>文字列の末尾または末尾の改行の前: \Z  
 `\Z` アンカーは、入力文字列の末尾、または入力文字列の末尾にある `\n` の前に一致する必要があることを指定します。 これは `$` アンカーと同じですが、 `\Z` では <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションが無視される点が異なります。 したがって、複数行文字列では、最後の行の末尾か、最後の行の `\n`の前にのみ一致することができます。  
  
 `\Z` は `\n` に一致しますが、 `\r\n` (CR/LF 文字の組み合わせ) には一致しないことに注意してください。 CR/LF と一致させるには、 `\r?\Z` を正規表現パターンに含めます。  
  
 次の例では、正規表現で `\Z` アンカーを使用して、プロ野球チームが存続した年数に関する情報を抽出します。この正規表現は、「 [文字列または行の先頭](#start-of-string-or-line-) 」で取り上げた例に似ています。 正規表現 `\r?\Z` 内の部分式 `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\Z` は、文字列の末尾に一致するほか、 `\n` または `\r\n`で終了する文字列にも一致します。 結果として、配列内の各要素が正規表現パターンに一致します。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring2.cs#4)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring2.vb#4)]

## <a name="end-of-string-only-z"></a>文字列の末尾のみ: \z  
 `\z` アンカーは、入力文字列の末尾に一致する必要があることを指定します。 `$` 言語要素と同様に、 `\z` では <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションが無視されます。 `\Z` 言語要素とは異なり、 `\z` は文字列末尾にある `\n` 文字には一致しません。 したがって、入力文字列の最後の行にのみ一致することができます。  
  
 次の例では、正規表現で `\z` アンカーを使用して、プロ野球チームが存続した年数に関する情報を抽出します。この正規表現は、アンカーを除けば前のセクションで取り上げた例と同じです。 この例は、文字列配列の 5 つの各要素と正規表現パターン `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\z`の一致を試みます。 これらのうち、2 つの文字列は復帰文字と改行文字で終わり、別の 1 つの文字列は改行文字で終わっています。残りの 2 つの文字列の末尾には、復帰文字も改行文字もありません。 出力結果が示すように、復帰文字も改行文字もない文字列だけがパターンに一致します。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring3.cs#5)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring3.vb#5)]

## <a name="contiguous-matches-g"></a>連続一致: \G  
 `\G` アンカーは、前回の一致が終了した位置に一致する必要があることを指定します。 このアンカーを <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドと共に使用すると、すべての一致が連続することになります。  
  
 次の例では、正規表現を使用して、コンマ区切りの文字列からげっ歯類の名称を抽出します。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/contiguous1.cs#6)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/contiguous1.vb#6)]  
  
 この正規表現 `\G(\w+\s?\w*),?` の解釈を次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\G`|最後の一致の終了位置から開始します。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`\s?`|0 個または 1 個の空白に一致します。|  
|`\w*`|0 個以上の単語に使用される文字に一致します。|  
|`(\w+\s?\w*)`|1 個以上の単語文字の後に 0 個または 1 個の空白が続き、さらに 0 個以上の単語文字が続くパターンに一致します。 これが最初のキャプチャ グループです。|  
|`,?`|リテラルのコンマ文字の 0 回または 1 回の繰り返しに一致します。|

## <a name="word-boundary-b"></a>ワード境界: \b  
 `\b` アンカーは、単語文字 ( `\w` 言語要素) と単語以外の文字 ( `\W` 言語要素) の境界に一致する必要があることを示します。 単語文字は英数字とアンダースコアで構成され、単語以外の文字は、英数字でもアンダースコアでもない任意の文字で構成されます 詳細については、「[文字クラス](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)」を参照してください。文字列の先頭または末尾にあるワード境界にも一致する可能性があります。  
  
 `\b` アンカーは、部分式を単語の先頭または末尾ではなく単語全体に一致させる目的で頻繁に使用されます。 次の例の正規表現 `\bare\w*\b` は、この使用方法を示しています。 これは、部分文字列 "are" で始まる任意の単語に一致します。 この例の出力から、 `\b` は入力文字列の先頭と末尾の両方に一致することもわかります。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/word1.cs#7)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/word1.vb#7)]  
  
 この正規表現パターンの解釈を次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`are`|部分文字列 "are" に一致します。|  
|`\w*`|0 個以上の単語に使用される文字に一致します。|  
|`\b`|ワード境界で照合を終了します。|  

## <a name="non-word-boundary-b"></a>ワード境界以外: \B  
 `\B` アンカーは、ワード境界には一致しないことを指定します。 これは、 `\b` アンカーと逆の働きをします。  
  
 次の例では、 `\B` アンカーを使用して、単語内で部分文字列 "qu" が出現する位置を見つけます。 正規表現パターン `\Bqu\w+` は、"qu" で始まり (単語の先頭ではない)、単語の終わりまでの部分文字列に一致します。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/nonword1.cs#8)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/nonword1.vb#8)]  
  
 この正規表現パターンの解釈を次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\B`|ワード境界では照合を開始しません。|  
|`qu`|部分文字列 "qu" と一致します。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
  
## <a name="see-also"></a>関連項目

- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)
