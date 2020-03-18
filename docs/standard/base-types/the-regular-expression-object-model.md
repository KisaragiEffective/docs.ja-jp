---
title: 正規表現のオブジェクト モデル
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, backreferences
- Regex class
- Match class
- pattern-matching with regular expressions, backreferences
- .NET Framework regular expressions, classes
- CaptureCollection class
- Group class
- characters [.NET Framework], backreferences
- substrings
- .NET Framework regular expressions, backreferences
- searching with regular expressions, classes
- backreferences
- Capture class
- repeating groups of characters
- MatchCollection class
- parsing text with regular expressions, backreferences
- regular expressions [.NET Framework]
- characters [.NET Framework], regular expressions
- classes [.NET Framework], regular expression
- regular expressions [.NET Framework], classes
- characters [.NET Framework], metacharacters
- metacharacters, regular expression classes
- metacharacters, backreferences
- parsing text with regular expressions, classes
- regular expressions [.NET Framework], backreferences
- strings [.NET Framework], regular expressions
- pattern-matching with regular expressions, classes
- GroupCollection class
ms.assetid: 49a21470-64ca-4b5a-a889-8e24e3c0af7e
ms.openlocfilehash: 8956be3cf8f96a8dd255f378d4927404c172c908
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160002"
---
# <a name="the-regular-expression-object-model"></a>正規表現のオブジェクト モデル
<a name="introduction"></a> ここでは、.NET の正規表現を扱うときに使用するオブジェクト モデルについて説明します。 次のセクションが含まれます。  
  
- [正規表現エンジン](#Engine)  
  
- [MatchCollection オブジェクトと Match オブジェクト](#Match_and_MCollection)  
  
- [グループ コレクション](#GroupCollection)  
  
- [キャプチャ グループ](#the_captured_group)  
  
- [キャプチャ コレクション](#CaptureCollection)  
  
- [個々のキャプチャ](#the_individual_capture)  
  
<a name="Engine"></a>
## <a name="the-regular-expression-engine"></a>正規表現エンジン  
 .NET の正規表現エンジンは、<xref:System.Text.RegularExpressions.Regex> クラスによって表されます。 正規表現エンジンは、正規表現の解析とコンパイル、および正規表現パターンと入力文字列を照合する操作を実行します。 エンジンは、.NET 正規表現のオブジェクト モデルの中核となるコンポーネントです。  
  
 正規表現エンジンは、次のいずれかの方法で使用できます。  
  
- <xref:System.Text.RegularExpressions.Regex> クラスの静的メソッドを呼び出す。 メソッドのパラメーターには、入力文字列と正規表現パターンが含まれます。 静的メソッド呼び出しで使用した正規表現は、正規表現エンジンによってキャッシュされるので、同じ正規表現を使用する静的正規表現メソッドを繰り返し呼び出す場合、パフォーマンスが比較的向上します。  
  
- 正規表現をクラス コンストラクターに渡して <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化する。 この場合、<xref:System.Text.RegularExpressions.Regex> オブジェクトは変更不可 (読み取り専用) で、単一の正規表現と密に結合された正規表現エンジンを表します。 <xref:System.Text.RegularExpressions.Regex> インスタンスによって使用される正規表現はキャッシュされないので、同じ正規表現で <xref:System.Text.RegularExpressions.Regex> オブジェクトを複数回インスタンス化しないでください。  
  
 <xref:System.Text.RegularExpressions.Regex> クラスのメソッドを呼び出すと、次のような処理を実行できます。  
  
- 文字列が正規表現パターンと一致するかどうかを確認する。  
  
- 単一の一致または最初の一致を抽出する。  
  
- すべての一致を抽出する。  
  
- 一致した部分文字列を置換する。  
  
- 単一の文字列を文字列配列に分割する。  
  
 ここでは、これらの操作について説明します。  
  
### <a name="matching-a-regular-expression-pattern"></a>正規表現パターンの照合  
 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドは、文字列がパターンと一致する場合は `true` を返し、一致しない場合は `false` を返します。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> メソッドは、文字列入力を検証する場合によく使用されます。 たとえば、次のコードでは、文字列は米国の有効な社会保障番号と一致します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/validate1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/validate1.vb#1)]  
  
 この正規表現パターン `^\d{3}-\d{2}-\d{4}$` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`^`|入力文字列の先頭と一致します。|  
|`\d{3}`|3 個の 10 進数と一致します。|  
|`-`|ハイフンと一致します。|  
|`\d{2}`|2 桁の 10 進数と一致します。|  
|`-`|ハイフンと一致します。|  
|`\d{4}`|4 桁の 10 進数と一致します。|  
|`$`|入力文字列の末尾と一致します。|  
  
### <a name="extracting-a-single-match-or-the-first-match"></a>単一の一致または最初の一致の抽出  
 <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> メソッドは、正規表現パターンに一致する最初の部分文字列の情報を含む <xref:System.Text.RegularExpressions.Match> オブジェクトを返します。 `Match.Success` プロパティが `true` (一致する文字列が見つかったことを示す) を返す場合は、<xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドを呼び出すと後続の一致する文字列の情報を取得できます。 これらのメソッド呼び出しは、`Match.Success` プロパティによって `false` が返されるまで続行できます。 たとえば、次のコードでは、<xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドを使用して、重複する単語が文字列内で最初に出現する位置を検索します。 次に、<xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドを呼び出してその他の出現位置を検索します。 この例では、メソッドを呼び出すごとに `Match.Success` プロパティを調べ、現在の一致が成功したかどうか、および続けて <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドを呼び出す必要があるかどうかを確認します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match1.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match1.vb#2)]  
  
 この正規表現パターン `\b(\w+)\W+(\1)\b` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(\w+)`|1 つ以上の単語文字に一致します。 これが最初のキャプチャ グループです。|  
|`\W+`|1 個以上の単語文字に使用されない文字と一致します。|  
|`(\1)`|最初にキャプチャされた文字列と一致します。 これが 2 番目のキャプチャ グループです。|  
|`\b`|ワード境界で照合を終了します。|  
  
### <a name="extracting-all-matches"></a>すべての一致の抽出  
 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドは、入力文字列で正規表現エンジンによって検出されたすべての一致文字列の情報を含む <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトを返します。 たとえば、前の例を書き換えて、<xref:System.Text.RegularExpressions.Regex.Matches%2A> メソッドと <xref:System.Text.RegularExpressions.Regex.Match%2A> メソッドではなく <xref:System.Text.RegularExpressions.Match.NextMatch%2A> メソッドを呼び出すことができます。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matches1.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matches1.vb#3)]  
  
### <a name="replacing-a-matched-substring"></a>一致した部分文字列の置換  
 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドは、正規表現パターンに一致した各部分文字列を指定された文字列または正規表現パターンに置き換えて、置換が適用された入力文字列全体を返します。 たとえば、次のコードでは、文字列内の 10 進数の前に米国の通貨記号が追加されます。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/replace1.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/replace1.vb#4)]  
  
 この正規表現パターン `\b\d+\.\d{2}\b` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\d+`|1 個以上の 10 進数と一致します。|  
|`\.`|ピリオドと一致します。|  
|`\d{2}`|2 桁の 10 進数と一致します。|  
|`\b`|ワード境界で照合を終了します。|  
  
 置換パターン `$$$&` の解釈を次の表に示します。  
  
|パターン|置換文字列|  
|-------------|------------------------|  
|`$$`|ドル記号 ($) 文字。|  
|`$&`|一致した部分文字列全体。|  
  
### <a name="splitting-a-single-string-into-an-array-of-strings"></a>単一の文字列の文字列配列への分割  
 <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> メソッドは、正規表現によって定義されている位置で、入力文字列を分割します。 たとえば、次のコードでは、番号付きリストの項目を文字列配列に配置します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/split1.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/split1.vb#5)]  
  
 この正規表現パターン `\b\d{1,2}\.\s` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\d{1,2}`|1 桁または 2 桁の 10 進数と一致します。|  
|`\.`|ピリオドと一致します。|  
|`\s`|空白文字と一致します。|  
  
<a name="Match_and_MCollection"></a>
## <a name="the-matchcollection-and-match-objects"></a>MatchCollection オブジェクトと Match オブジェクト  
 Regex メソッドは、正規表現のオブジェクト モデルに含まれる <xref:System.Text.RegularExpressions.MatchCollection> と <xref:System.Text.RegularExpressions.Match> の 2 つのオブジェクトを返します。  
  
<a name="the_match_collection"></a>
### <a name="the-match-collection"></a>MatchCollection  
 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドは、正規表現エンジンによって検出されたすべての一致文字列を入力文字列に出現する順序で表す <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトを含む <xref:System.Text.RegularExpressions.Match> オブジェクトを返します。 一致文字列がない場合、このメソッドはメンバーを持たない <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトを返します。 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> プロパティを使用すると、コレクションの個々のメンバーに、0 から <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> プロパティの値より 1 小さい値までの範囲のインデックスでアクセスできます。 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A> は、コレクションのインデクサー (C# の場合) および既定のプロパティ (Visual Basic の場合) です。  
  
 既定では、<xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> メソッドを呼び出すと、遅延評価を使用して <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトに値が設定されます。 値の設定が完了しているコレクションを必要とするプロパティ (<xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> プロパティや <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> プロパティなど) にアクセスする場合は、パフォーマンスが低下する可能性があります。 そのため、<xref:System.Collections.IEnumerator> メソッドによって返される <xref:System.Text.RegularExpressions.MatchCollection.GetEnumerator%2A?displayProperty=nameWithType> オブジェクトを使用してコレクションにアクセスすることをお勧めします。 個々の言語には、コレクションの `For Each` インターフェイスをラップする構成体 (Visual Basic の `foreach` や C# の <xref:System.Collections.IEnumerator> など) が用意されています。  
  
 次の例では、<xref:System.Text.RegularExpressions.Regex.Matches%28System.String%29?displayProperty=nameWithType> メソッドを使用して、入力文字列の中で見つかったすべての一致を <xref:System.Text.RegularExpressions.MatchCollection> オブジェクトに設定します。 この例では、コレクションを列挙して一致文字列を文字列配列にコピーし、文字位置を整数配列に記録します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matchcollection1.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matchcollection1.vb#6)]  
  
<a name="the_match"></a>
### <a name="the-match"></a>Match  
 <xref:System.Text.RegularExpressions.Match> クラスは、1 回の正規表現検索に一致した結果を表します。 <xref:System.Text.RegularExpressions.Match> オブジェクトには 2 つの方法でアクセスできます。  
  
- <xref:System.Text.RegularExpressions.MatchCollection> メソッドから返される <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> オブジェクトから取得する。 個々の <xref:System.Text.RegularExpressions.Match> オブジェクトを取得するには、`foreach` 構成体 (C# の場合) または `For Each`...`Next` 構成体 (Visual Basic の場合) を使用してコレクションを反復処理するか、<xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> プロパティを使用してインデックスまたは名前で特定の <xref:System.Text.RegularExpressions.Match> オブジェクトを取得します。 また、0 からコレクションのオブジェクト数より 1 小さい値までの範囲のインデックスでコレクションを反復処理して、コレクションから個々の <xref:System.Text.RegularExpressions.Match> オブジェクトを取得することもできます。 ただし、このメソッドは <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> プロパティにアクセスするので、遅延評価を活用できません。  
  
     次の例では、<xref:System.Text.RegularExpressions.Match> 構成体または <xref:System.Text.RegularExpressions.MatchCollection>...`foreach` 構成体を使用してコレクションを反復処理することで、個々の `For Each` オブジェクトを `Next` オブジェクトから取得します。 この正規表現は、単純に入力文字列内の文字列 "abc" と一致します。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match2.vb#7)]  
  
- 文字列または文字列の一部で最初に一致する文字列を表す <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> オブジェクトを返す <xref:System.Text.RegularExpressions.Match> メソッドを呼び出す。 `Match.Success` プロパティの値を取得すると、一致文字列が見つかったかどうかを確認できます。 後続の一致する文字列を表す <xref:System.Text.RegularExpressions.Match> オブジェクトを取得するには、返された <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> オブジェクトの `Success` プロパティが <xref:System.Text.RegularExpressions.Match> になるまで `false` メソッドを繰り返し呼び出します。  
  
     次の例では、入力文字列内の文字列 "abc" と一致する <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドと <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドを使用します。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match3.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match3.vb#8)]  
  
 <xref:System.Text.RegularExpressions.Match> クラスの 2 つのプロパティによってコレクション オブジェクトが返されます。  
  
- <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> プロパティは、正規表現パターンのキャプチャ グループに一致する部分文字列の情報を含む <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトを返します。  
  
- `Match.Captures` プロパティは、用途が限定された <xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトを返します。 <xref:System.Text.RegularExpressions.Match> オブジェクトの `Success` プロパティが `false` の場合、コレクションに値は設定されません。 それ以外の場合は、<xref:System.Text.RegularExpressions.Capture> オブジェクトと同じ情報を含む単一の <xref:System.Text.RegularExpressions.Match> オブジェクトが格納されます。  
  
 これらのオブジェクトの詳細については、このトピックで後述する「[GroupCollection](#GroupCollection)」セクションおよび「[CaptureCollection](#CaptureCollection)」セクションを参照してください。  
  
 <xref:System.Text.RegularExpressions.Match> クラスの 2 つの追加プロパティに、一致文字列の情報が保持されます。 `Match.Value` プロパティは、正規表現パターンに一致する入力文字列内の部分文字列を返します。 `Match.Index` プロパティは、入力文字列内の一致する文字列の 0 から始まる開始位置を返します。  
  
 <xref:System.Text.RegularExpressions.Match> クラスには、2 つのパターン一致メソッドもあります。  
  
- <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドは、現在の <xref:System.Text.RegularExpressions.Match> オブジェクトで表される一致文字列の後の一致文字列を検索し、その一致文字列を表す <xref:System.Text.RegularExpressions.Match> オブジェクトを返します。  
  
- <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドは、一致した文字列に対して指定された置換操作を実行し、その結果を返します。  
  
 次の例では、<xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドを使用して、2 桁の小数部を含むすべての数値の前に $ 記号および空白を付加します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/result1.cs#9)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/result1.vb#9)]  
  
 正規表現パターン `\b\d+(,\d{3})*\.\d{2}\b` は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\d+`|1 個以上の 10 進数と一致します。|  
|`(,\d{3})*`|コンマの後に 3 桁の 10 進数字が続くパターンの 0 回以上の出現と一致します。|  
|`\.`|小数点文字と一致します。|  
|`\d{2}`|2 桁の 10 進数と一致します。|  
|`\b`|ワード境界で照合を終了します。|  
  
 置換パターン `$$ $&` は、一致した部分文字列がドル記号 ($) (`$$` パターン)、空白、および一致文字列の値 (`$&` パターン) に置き換えられることを示します。  
  
 [先頭に戻る](#introduction)  
  
<a name="GroupCollection"></a>
## <a name="the-group-collection"></a>GroupCollection  
 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> プロパティは、単一の一致でキャプチャされたグループを表す <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトを含む <xref:System.Text.RegularExpressions.Group> オブジェクトを返します。 コレクション内の最初の <xref:System.Text.RegularExpressions.Group> オブジェクト (インデックス 0 の位置にあるオブジェクト) は、一致した文字列全体を表します。 後続の各オブジェクトは、1 つのキャプチャ グループによるキャプチャの結果を表します。  
  
 コレクション内の個々の <xref:System.Text.RegularExpressions.Group> オブジェクトを取得するには、<xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType> プロパティを使用します。 名前のないグループはコレクション内の順序位置で取得でき、名前付きグループは名前または順序位置で取得できます。 名前のないキャプチャはコレクションの最初に位置し、正規表現パターンで定義されている順序で左から右にインデックスが付けられます。 名前付きキャプチャは、名前のないキャプチャの後に、正規表現パターンで定義されている順序で左から右にインデックスが付けられます。 特定の正規表現一致メソッドで返されたコレクションで使用可能な番号付きグループを判別するには、インスタンス <xref:System.Text.RegularExpressions.Regex.GetGroupNumbers%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。 コレクションで使用可能な名前付きグループを判別するには、インスタンス <xref:System.Text.RegularExpressions.Regex.GetGroupNames%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。 どちらのメソッドも、正規表現によって検出された一致を分析する汎用ルーチンで特に役立ちます。  
  
 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType> プロパティは、コレクションのインデクサー (C# の場合) およびコレクション オブジェクトの既定のプロパティ (Visual Basic の場合) です。 つまり、個々の <xref:System.Text.RegularExpressions.Group> オブジェクトには、次のようにインデックスで (または名前付きグループの場合は名前で) アクセスできます。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupsyntax1.cs#13)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupsyntax1.vb#13)]  
  
 次の例では、グループ化構成体を使用して日付の月、日、および年をキャプチャする正規表現を定義します。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupcollection1.cs#10)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupcollection1.vb#10)]  
  
 正規表現パターン `\b(\w+)\s(\d{1,2}),\s(\d{4})\b` は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(\w+)`|1 つ以上の単語文字に一致します。 これが最初のキャプチャ グループです。|  
|`\s`|空白文字と一致します。|  
|`(\d{1,2})`|1 桁または 2 桁の 10 進数と一致します。 これが 2 番目のキャプチャ グループです。|  
|`,`|コンマに一致します。|  
|`\s`|空白文字と一致します。|  
|`(\d{4})`|4 桁の 10 進数と一致します。 これが 3 番目のキャプチャ グループです。|  
|`\b`|ワード境界で照合を終了します。|  
  
 [先頭に戻る](#introduction)  
  
<a name="the_captured_group"></a>
## <a name="the-captured-group"></a>キャプチャ グループ  
 <xref:System.Text.RegularExpressions.Group> クラスは、1 つのキャプチャ グループによるキャプチャの結果を表します。 正規表現で定義されているキャプチャ グループを表すグループ オブジェクトは、<xref:System.Text.RegularExpressions.GroupCollection.Item%2A> プロパティによって返される <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトの <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> プロパティによって返されます。 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> プロパティは、<xref:System.Text.RegularExpressions.Group> クラスのインデクサー (C# の場合) および既定のプロパティ (Visual Basic の場合) です。 `foreach` 構成体または `For Each` 構成体を使用してコレクションを反復処理することで、個々のメンバーを取得することもできます。 例については、前のセクションを参照してください。  
  
 次の例では、入れ子にしたグループ化構成体を使用して部分文字列をキャプチャし、グループ化します。 正規表現パターン `(a(b))c` は、文字列 "abc" と一致します。 部分文字列 "ab" を最初のキャプチャ グループに代入し、部分文字列 "b" を 2 番目のキャプチャ グループに代入します。  
  
 [!code-csharp[RegularExpressions.Classes#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#6)]
 [!code-vb[RegularExpressions.Classes#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#6)]  
  
 次の例では、名前付きグループ化構成体を使用して、"DATANAME:VALUE" 形式のデータを含む文字列から部分文字列をキャプチャします。この正規表現は、コロン (:) でデータを分割します。  
  
 [!code-csharp[RegularExpressions.Classes#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#8)]
 [!code-vb[RegularExpressions.Classes#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#8)]  
  
 正規表現パターン `^(?<name>\w+):(?<value>\w+)` は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`^`|入力文字列の先頭から照合を開始します。|  
|`(?<name>\w+)`|1 つ以上の単語文字に一致します。 このキャプチャ グループの名前は `name` です。|  
|`:`|コロンと一致します。|  
|`(?<value>\w+)`|1 つ以上の単語文字に一致します。 このキャプチャ グループの名前は `value` です。|  
  
 <xref:System.Text.RegularExpressions.Group> クラスのプロパティには、キャプチャ グループの情報が保持されます。キャプチャされた部分文字列が `Group.Value` プロパティに含まれ、キャプチャ グループの入力テキスト内での開始位置が `Group.Index` プロパティによって示され、キャプチャされたテキストの長さが `Group.Length` プロパティに含まれ、部分文字列がキャプチャ グループによって定義されたパターンと一致したかどうかが `Group.Success` プロパティによって示されます。  
  
 量指定子をグループに適用する場合 (詳細については、「[量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」を参照)、キャプチャ グループごとに 1 つのキャプチャという関係が 2 つの方法で変更されます。  
  
- `*` 量指定子または `*?` 量指定子 (0 回以上の一致を指定する) をグループに適用した場合、キャプチャ グループには入力文字列で一致した文字列が含まれない可能性があります。 キャプチャされたテキストがない場合、<xref:System.Text.RegularExpressions.Group> オブジェクトのプロパティは次の表に示すように設定されます。  
  
    |Group プロパティ|値|  
    |--------------------|-----------|  
    |`Success`|`false`|  
    |`Value`|<xref:System.String.Empty?displayProperty=nameWithType>|  
    |`Length`|0|  
  
     次に例を示します。 正規表現パターン `aaa(bbb)*ccc` では、最初のキャプチャ グループ (部分文字列 "bbb") は 0 回以上一致できます。 入力文字列 "aaaccc" はパターンに一致するので、キャプチャ グループには一致文字列が含まれません。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/nocapture1.cs#11)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/nocapture1.vb#11)]  
  
- 量指定子は、キャプチャ グループによって定義されたパターンの複数回の出現と一致できます。 この場合、`Value` オブジェクトの `Length` プロパティと <xref:System.Text.RegularExpressions.Group> プロパティには、最後にキャプチャされた部分文字列の情報のみが保持されます。 たとえば、次の正規表現は、ピリオドで終わる 1 文と一致します。 この正規表現では 2 つのグループ化構成体が使用されています。最初のグループ化構成体は個々の単語および空白文字をキャプチャし、2 番目のグループ化構成体は個々の単語をキャプチャします。 この例の出力結果が示すように、正規表現では文全体が正常にキャプチャされますが、2 番目のキャプチャ グループでは最後の単語のみがキャプチャされます。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/lastcapture1.cs#12)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/lastcapture1.vb#12)]  
  
 [先頭に戻る](#introduction)  
  
<a name="CaptureCollection"></a>
## <a name="the-capture-collection"></a>CaptureCollection  
 <xref:System.Text.RegularExpressions.Group> オブジェクトには、最後のキャプチャの情報のみが格納されます。 ただし、キャプチャ グループによって行われたキャプチャのセット全体は、<xref:System.Text.RegularExpressions.CaptureCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> オブジェクトから取得できます。 コレクションの各メンバーは、キャプチャ グループによって行われたキャプチャを表す <xref:System.Text.RegularExpressions.Capture> オブジェクトです。キャプチャされた順序 (したがって、キャプチャされた文字列が左から右に入力文字列と照合された順序) で並びます。 コレクション内の個々の <xref:System.Text.RegularExpressions.Capture> オブジェクトは、次のいずれかの方法で取得できます。  
  
- `foreach` (C# の場合) や `For Each` (Visual Basic の場合) などの構成体を使用してコレクションを反復処理する。  
  
- <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A?displayProperty=nameWithType> プロパティを使用して特定のオブジェクトをインデックスで取得する。 <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A> プロパティは、<xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトの既定のプロパティ (Visual Basic の場合) またはインデクサー (C# の場合) です。  
  
 量指定子がキャプチャ グループに適用されない場合、<xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトには <xref:System.Text.RegularExpressions.Capture> オブジェクトと同じ一致文字列の情報が保持されるので、関心の低い単一の <xref:System.Text.RegularExpressions.Group> オブジェクトが含まれます。 量指定子がキャプチャ グループに適用される場合、<xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトにはキャプチャ グループによって行われたすべてのキャプチャが含まれ、コレクションの最後のメンバーは <xref:System.Text.RegularExpressions.Group> オブジェクトと同じキャプチャを表します。  
  
 たとえば、正規表現パターン `((a(b))c)+` (量指定子 + は 1 つ以上の文字列が一致することを指定) を使用して文字列 "abcabcabc" から一致する文字列をキャプチャする場合、各 <xref:System.Text.RegularExpressions.CaptureCollection> オブジェクトの <xref:System.Text.RegularExpressions.Group> オブジェクトには、3 個のメンバーが含まれることになります。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capturecollection1.cs#14)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capturecollection1.vb#14)]  
  
 次の例では、正規表現 `(Abc)+` を使用して、文字列 "XYZAbcAbcAbcXYZAbcAb" の中から文字列 "Abc" の連続した出現を 1 つ以上検索します。 この例は、<xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> プロパティを使用して、キャプチャした部分文字列の複数のグループを返す方法を示しています。  
  
 [!code-csharp[RegularExpressions.Classes#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#5)]
 [!code-vb[RegularExpressions.Classes#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#5)]  
  
 [先頭に戻る](#introduction)  
  
<a name="the_individual_capture"></a>
## <a name="the-individual-capture"></a>個々のキャプチャ  
 <xref:System.Text.RegularExpressions.Capture> クラスには、単一の部分式キャプチャの結果が含まれます。 一致したテキストが <xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType> プロパティに含まれ、一致した部分文字列の入力文字列内での開始位置 (起点を 0 とする) が <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType> プロパティによって示されます。  
  
 次の例では、選択した都市の気温の入力文字列を解析します。 都市とその気温を区切るためにコンマ (",") が使用され、各都市のデータを区切るためにセミコロン (";") が使用されています。 入力文字列全体が単一の一致を表します。 文字列の解析に使用される正規表現パターン `((\w+(\s\w+)*),(\d+);)+` では、都市名が 2 番目のキャプチャ グループに代入され、気温が 4 番目のキャプチャ グループに代入されます。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capture1.cs#16)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capture1.vb#16)]  
  
 正規表現は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`(\s\w+)*`|空白文字の後に 1 個以上の単語文字が続くパターンの 0 回以上の出現と一致します。 このパターンは、複数の単語で構成される都市名と一致します。 これが 3 番目のキャプチャ グループです。|  
|`(\w+(\s\w+)*)`|1 個以上の単語文字の後に空白文字および 1 個以上の単語文字の 0 回以上の出現が続くパターンと一致します。 これが 2 番目のキャプチャ グループです。|  
|`,`|コンマに一致します。|  
|`(\d+)`|1 桁以上の数字と一致します。 これが 4 番目のキャプチャ グループです。|  
|`;`|セミコロンと一致します。|  
|`((\w+(\s\w+)*),(\d+);)+`|単語、追加の単語、コンマ、1 桁以上の数字、およびセミコロンが 1 回以上続くパターンと一致します。 これが最初のキャプチャ グループです。|  
  
## <a name="see-also"></a>参照

- <xref:System.Text.RegularExpressions>
- [.NET の正規表現](../../../docs/standard/base-types/regular-expressions.md)
- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
