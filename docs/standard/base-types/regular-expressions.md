---
title: .NET Framework の正規表現
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- pattern-matching with regular expressions, about pattern-matching
- substrings
- searching with regular expressions, about regular expressions
- pattern-matching with regular expressions
- searching with regular expressions
- parsing text with regular expressions
- regular expressions [.NET Framework], about regular expressions
- regular expressions [.NET Framework]
- .NET Framework regular expressions, about
- characters [.NET Framework], regular expressions
- parsing text with regular expressions, overview
- .NET Framework regular expressions
- strings [.NET Framework], regular expressions
ms.assetid: 521b3f6d-f869-42e1-93e5-158c54a6895d
ms.openlocfilehash: ac034ff37b0b39f41d6f58381286706f9a9ac602
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73121697"
---
# <a name="net-regular-expressions"></a>.NET の正規表現
正規表現を使用すると、強力、柔軟、そして効率的な方法でテキストを処理できます。 正規表現の広範なパターン一致表記法を使用することで、大量のテキストをすばやく解析して特定の文字パターンを検索したり、決められたパターン (メール アドレスなど) と照らしてテキストを検証したりできるほか、テキストの部分文字列を抽出、編集、置換、または削除したり、抽出した文字列をコレクションに追加してレポートを生成したりすることもできます。 文字列処理や大量のテキストを解析する多くのアプリケーションにとって、正規表現は欠くことのできないツールです。  
  
## <a name="how-regular-expressions-work"></a>正規表現の動作  
 正規表現を使ったテキスト処理の最も重要な部分は、.NET の <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> オブジェクトによって表される正規表現エンジンです。 正規表現を使ったテキスト処理では、正規表現エンジンに対し、最低でも次の 2 つの情報を与える必要があります。  
  
- テキストを識別する正規表現パターン。  
  
     .NET では、正規表現のパターンが特殊な構文または言語で定義されます。この構文または言語には、Perl 5 の正規表現と互換性があるほか、右から左への一致処理など、いくつかの機能が追加されています。 詳細については、「[正規表現言語 - クイック リファレンス](regular-expression-language-quick-reference.md)」をご覧ください。  
  
- 正規表現パターンの解析対象となるテキスト。  
  
 <xref:System.Text.RegularExpressions.Regex> クラスのメソッドを使用すると、次のような処理を実行できます。  
  
- 入力されたテキストに特定の正規表現パターンが出現するかどうかを調べるには、<xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドを呼び出します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> メソッドを使用してテキストを検証する例については、「[方法:文字列が有効な電子メール形式であるかどうかを検証する](how-to-verify-that-strings-are-in-valid-email-format.md)」をご覧ください。  
  
- 正規表現パターンと一致したテキストを 1 つまたは全部取得するには、<xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドを呼び出します。 前者は、一致したテキストの情報を保持する <xref:System.Text.RegularExpressions.Match?displayProperty=nameWithType> オブジェクトを返します。 後者は、解析対象のテキストに見つかった各一致につき 1 つの <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトを含む <xref:System.Text.RegularExpressions.Match?displayProperty=nameWithType> オブジェクトを返します。  
  
- 正規表現パターンと一致したテキストを置換するには、<xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドを呼び出します。 <xref:System.Text.RegularExpressions.Regex.Replace%2A> メソッドを使用して日付形式を変更したり文字列から無効な文字を削除したりする例については、「[方法:文字列から無効な文字を取り除く](how-to-strip-invalid-characters-from-a-string.md)」と「[例:日付形式の変更](regular-expression-example-changing-date-formats.md)」を参照してください。  
  
 正規表現のオブジェクト モデルの概要については、「[正規表現のオブジェクト モデル](the-regular-expression-object-model.md)」をご覧ください。  
  
 正規表現の言語について詳しくは、「[正規表現言語 - クイック リファレンス](regular-expression-language-quick-reference.md)」を参照するか、次の資料のいずれかをダウンロードして印刷してください。  
  
 [Word (.docx) 形式のクイック リファレンス](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)  
 [PDF (.pdf) 形式のクイック リファレンス](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)  
  
## <a name="regular-expression-examples"></a>正規表現の例  
 <xref:System.String> クラスには、文字列内のリテラル文字列を検索する際に使用できる文字列の検索メソッドと置換メソッドが数多く含まれています。 正規表現は、次の例に示すように、文字列内の部分文字列のいずれかを検索する場合、または文字列内のパターンを識別する場合に最も役立ちます。  
  
### <a name="example-1-replacing-substrings"></a>例 1: 部分文字列の置換  
 氏名に敬称 (Mr.、Mrs.、Miss、または Ms.) が付いている場合がある名前が、宛先リストに含まれるとします。 そのリストから封筒のラベルを生成する場合に敬称が含まれないようにするには、次の例に示すように、正規表現を使用して敬称を削除します。  
  
 [!code-csharp[Conceptual.Regex#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example1.cs#2)]
 [!code-vb[Conceptual.Regex#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example1.vb#2)]  
  
 正規表現パターン `(Mr\.? |Mrs\.? |Miss |Ms\.? )` は、"Mr "、"Mr. "、"Mrs "、"Mrs. "、"Miss "、"Ms"、または "Ms. " の出現と一致します。 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドを呼び出すと、一致する文字列が <xref:System.String.Empty?displayProperty=nameWithType> に置き換えられます。つまり、元の文字列から削除されます。  
  
### <a name="example-2-identifying-duplicated-words"></a>例 2:重複する単語の識別  
 記述者が単語を誤って重複入力するというエラーがよくあります。 次の例に示すように、正規表現を使用して重複する単語を識別できます。  
  
 [!code-csharp[Conceptual.Regex#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example2.cs#3)]
 [!code-vb[Conceptual.Regex#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example2.vb#3)]  
  
 正規表現パターン `\b(\w+?)\s\1\b` は、次のように解釈できます。  
  
|||  
|-|-|  
|`\b`|ワード境界から開始します。|  
|(\w+?)|1 つ以上 (ただし、できるだけ少ない文字数) の単語文字と一致します。 同時に、`\1` というグループを形成します。|  
|`\s`|空白文字と一致します。|  
|`\1`|`\1` という名前のグループと等しい部分文字列と一致します。|  
|`\b`|ワード境界に一致します。|  
  
 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドは、正規表現オプションを <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> に設定して呼び出されます。 したがって、照合操作では大文字と小文字が区別されず、この例では部分文字列 "This this" が重複として識別されます。  
  
 入力文字列には部分文字列 "this? This" が含まれています。 ただし、句読点が介在するので、重複として識別されません。  
  
### <a name="example-3-dynamically-building-a-culture-sensitive-regular-expression"></a>例 3:カルチャに依存した正規表現の動的な構築  
 ここでは、正規表現による強力なテキスト処理と、.NET の柔軟なグローバリゼーション機能を組み合わせて使用する例を紹介します。 この例では、システムの現在のカルチャで用いられている通貨値の形式を調べるために、<xref:System.Globalization.NumberFormatInfo> オブジェクトが使用されています。 さらに、その情報を基に、テキストから通貨値を抽出する正規表現を動的に構築します。 検出された一致ごとに、数値文字列のみを含んだサブグループを抽出し、<xref:System.Decimal> 値に変換して、通算の合計を計算します。  
  
 [!code-csharp[Conceptual.Regex#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example.cs#1)]
 [!code-vb[Conceptual.Regex#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example.vb#1)]  
  
 現在 "英語 - 米国" (en-US) カルチャが使用されているコンピューターでは、`\$\s*[-+]?([0-9]{0,3}(,[0-9]{3})*(\.[0-9]+)?)` という正規表現が動的に作成されます。 この正規表現パターンは、次のように解釈できます。  
  
|||  
|-|-|  
|`\$`|入力文字列に含まれる単一のドル記号 (`$`) を検索します。 この正規表現パターン文字列に使用されている円記号は、ドル記号を正規表現のアンカーではなく、文字として扱うことを意味します。 (`$` 記号のみの場合、正規表現エンジンが文字列の最後で一致の開始を試みる必要があることを示します。)現在のカルチャの通貨記号が正規表現記号として解釈されるのを防ぐため、例では、<xref:System.Text.RegularExpressions.Regex.Escape%2A?displayProperty=nameWithType> メソッドを呼び出して文字をエスケープしています。|  
|`\s*`|空白文字の 0 回以上の繰り返しを検索します。|  
|`[-+]?`|正の符号または負の符号の 0 回または 1 回の繰り返しを検索します。|  
|`([0-9]{0,3}(,[0-9]{3})*(\.[0-9]+)?)`|外側の丸かっこで囲まれている表現は、キャプチャ グループまたは部分式として定義されます。 一致が見つかった場合、その一致した文字列の、この部分に関する情報が、<xref:System.Text.RegularExpressions.Group> プロパティから返された <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトの 2 つ目の <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> オブジェクトから取得できます (コレクションの 1 つ目の要素は、一致した文字列全体を表します)。|  
|`[0-9]{0,3}`|10 進数字 (0 ～ 9) の 0 回以上、3 回以下の繰り返しを検索します。|  
|`(,[0-9]{3})*`|桁区切り記号と 3 桁の 10 進数字の 0 回以上の繰り返しを検索します。|  
|`\.`|単一の小数点を検索します。|  
|`[0-9]+`|10 進数字の 1 回以上の繰り返しを検索します。|  
|`(\.[0-9]+)?`|小数点と 1 桁以上の数字の 0 回または 1 回の繰り返しを検索します。|  
  
 以上の各サブパターンが入力文字列内に見つかると一致と判断され、その一致に関する情報を含んだ <xref:System.Text.RegularExpressions.Match> オブジェクトが <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトに追加されます。  
  
## <a name="related-topics"></a>関連トピック  
  
|Title|説明|  
|-----------|-----------------|  
|[正規表現言語 - クイック リファレンス](regular-expression-language-quick-reference.md)|正規表現を定義するために使う一連の文字、演算子、および構成体について説明します。|  
|[正規表現のオブジェクト モデル](the-regular-expression-object-model.md)|正規表現クラスの使用方法について詳しく説明し、コード例を示します。|  
|[正規表現の動作の詳細](details-of-regular-expression-behavior.md)|.NET の正規表現の機能と動作について説明します。|  
|[正規表現の例](regular-expression-examples.md)|正規表現の一般的な使用方法を示すコード例が用意されています。|  
  
## <a name="reference"></a>関連項目  
 <xref:System.Text.RegularExpressions?displayProperty=nameWithType>  
 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>  
 [正規表現 - クイック リファレンス (Word 形式でダウンロード)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)  
 [正規表現 - クイック リファレンス (PDF 形式でダウンロード)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)
