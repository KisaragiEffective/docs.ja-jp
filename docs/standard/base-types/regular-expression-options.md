---
title: 正規表現のオプション
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, options
- constructs, options
- .NET Framework regular expressions, options
- inline option constructs
- options parameter
ms.assetid: c82dc689-7e82-4767-a18d-cd24ce5f05e9
ms.openlocfilehash: a53d7517485d2a0b02b6f11928f478a7da3f9503
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73972114"
---
# <a name="regular-expression-options"></a>正規表現のオプション

正規表現パターンでの入力文字列とリテラル文字列の比較では、大文字と小文字が区別されます。正規表現パターンに含まれる空白は、リテラルの空白文字として解釈されます。正規表現で使用されるキャプチャ グループは、暗黙的に指定される場合と明示的に指定される場合があります。これらはすべて、正規表現の既定の動作です。 正規表現のオプションを指定することで、これらの正規表現の既定の動作とそのいくつかの側面を変更できます。 次の表に示す各オプションは、正規表現パターンの一部としてインラインで記述することも、<xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> クラス コンストラクターまたは静的パターン一致メソッドに <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 列挙値として渡すこともできます。

|RegexOptions のメンバー|インライン文字|効果|
|-------------------------|----------------------|------------|
|<xref:System.Text.RegularExpressions.RegexOptions.None>|使用できません|既定の動作を使用します。 詳細については、「[既定のオプション](#default-options)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase>|`i`|大文字と小文字を区別しない一致を使用します。 詳細については、「[大文字と小文字を区別しない一致](#case-insensitive-matching)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.Multiline>|`m`|複数行モードを使用します。`^` と `$` は、(入力文字列の先頭および末尾ではなく) 各行の先頭および末尾と一致します。 詳細については、「[複数行モード](#multiline-mode)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.Singleline>|`s`|単一行モードを使用します。このモードでは、ピリオド (.) は任意の 1 文字と一致します (`\n` を除くすべての文字の代用)。 詳細については、「[単一行モード](#single-line-mode)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture>|`n`|名前のないグループをキャプチャしません。 `(?<`*name*`>` *subexpression*`)` という形式で、明示的に名前または番号が付加されたグループのみを有効なキャプチャ対象とします。 詳細については、「[明示的なキャプチャのみ](#explicit-captures-only)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.Compiled>|使用できません|正規表現をアセンブリにコンパイルします。 詳細については、「[コンパイルされた正規表現](#compiled-regular-expressions)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace>|`x`|エスケープされていない空白をパターンから除外し、シャープ記号 (`#`) の後ろのコメントを有効にします。 詳細については、「[空白を無視](#ignore-white-space)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft>|使用できません|検索の方向を変更します。 左から右ではなく、右から左に検索します。 詳細については、「[右から左モード](#right-to-left-mode)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript>|使用できません|式の ECMAScript 準拠の動作を有効にします。 詳細については、「[ECMAScript 一致の動作](#ecmascript-matching-behavior)」を参照してください。|
|<xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant>|使用できません|言語のカルチャの違いを無視します。 詳細については、「[インバリアント カルチャを使用した比較](#comparison-using-the-invariant-culture)」を参照してください。|

## <a name="specifying-the-options"></a>オプションの指定

正規表現のオプションは、次の 3 種類の方法のいずれかで指定できます。

- `options` や <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> など、`Shared` クラス コンストラクターまたは、静的 (Visual Basic の場合は <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType>) パターン一致メソッドの <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> パラメーターで指定します。 `options` パラメーターは、<xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 列挙値のビットごとの OR の組み合わせです。

  クラス コンストラクターの `options` パラメーターを使用して、オプションが <xref:System.Text.RegularExpressions.Regex> インスタンスに指定されると、それらのオプションは <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> プロパティに割り当てられます。 ただし、<xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> プロパティは、正規表現パターン自体でのインライン オプションを反映しません。

  具体的な例を次に示します。 `options` メソッドの <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> パラメーターを使用して、文字 "d" で始まる単語を識別するときに、大文字と小文字を区別しない一致を有効にすると同時に、パターンの空白を無視します。

  [!code-csharp[Conceptual.Regex.Language.Options#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#6)]
  [!code-vb[Conceptual.Regex.Language.Options#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#6)]

- `(?imnsx-imnsx)` という構文で、インライン オプションを正規表現パターンに適用します。 この場合、オプションが定義されているパターンの先頭から、パターンの末尾を含むポイントまで、または別のインライン オプションでオプションが定義されていないポイントまでを範囲として、オプションがパターンに適用されます。 <xref:System.Text.RegularExpressions.Regex> インスタンスの <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> プロパティは、これらのインライン オプションを反映しないことに注意してください。 詳しくは、「[その他の構成体](../../../docs/standard/base-types/miscellaneous-constructs-in-regular-expressions.md)」のトピックをご覧ください。

  具体的な例を次に示します。 インライン オプションを使用して、文字 "d" で始まる単語を識別するときに、大文字と小文字を区別しない一致を有効にすると同時に、パターンの空白を無視します。

  [!code-csharp[Conceptual.Regex.Language.Options#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#7)]
  [!code-vb[Conceptual.Regex.Language.Options#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#7)]

- `(?imnsx-imnsx:`*subexpression*`)` という構文で、特定のグループ化構成体のインライン オプションを正規表現パターンに適用します。 オプション セットの前にマイナス記号を付けないとそのオプション セットはオンになり、マイナス記号を付けるとオフになります (`?` は言語構成要素の構文の固定部分であり、オプションが有効であるか無効であるかにかかわらず、必要になります)。オプションは、そのグループに対してのみ適用されます。 詳しくは、「[正規表現でのグループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

  具体的な例を次に示します。 グループ化構成体のインライン オプションを使用して、文字 "d" で始まる単語を識別するときに、大文字と小文字を区別しない一致を有効にすると同時に、パターンの空白を無視します。

  [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
  [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]

オプションをインラインで指定した場合は、オプションまたはオプション セットの前にマイナス記号 (`-`) を付けると、そのオプションはオフになります。 たとえば、インライン構成体 `(?ix-ms)` は <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションおよび <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> オプションをオンにし、<xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションおよび <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションをオフにします。 既定では、すべての正規表現のオプションがオフです。

> [!NOTE]
> コンストラクターまたはメソッド呼び出しの `options` パラメーターで指定した正規表現オプションが正規表現パターンのインラインで指定したオプションと競合した場合は、インラインで指定したオプションが使用されます。

次に示す 5 種類の正規表現オプションは、options パラメーターとインラインの両方で設定できます。

- <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType>

次に示す 5 種類の正規表現オプションは `options` パラメーターを使用して設定することはできますが、インラインで設定することはできません。

- <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType>

## <a name="determining-the-options"></a>オプションの確認

インスタンス化されたときに <xref:System.Text.RegularExpressions.Regex> オブジェクトに渡されたオプションの種類を確認するには、読み取り専用の <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> プロパティの値を取得します。 このプロパティは、<xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> メソッドによって作成された、コンパイルされた正規表現に定義されているオプションを確認する場合に特に役立ちます。

<xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> を除くオプションが存在するかどうかをテストするには、目的の <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> プロパティの値と <xref:System.Text.RegularExpressions.RegexOptions> の値を使用して AND 演算を実行します。 次に、この結果が <xref:System.Text.RegularExpressions.RegexOptions> の値と等しいかどうかをテストします。 次の例は、<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションが設定されているかどうかをテストします。

[!code-csharp[Conceptual.Regex.Language.Options#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#19)]
[!code-vb[Conceptual.Regex.Language.Options#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#19)]

<xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> をテストするには、次の例に示すように、<xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> プロパティの値が <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> と等しいかどうかを確認します。

[!code-csharp[Conceptual.Regex.Language.Options#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#20)]
[!code-vb[Conceptual.Regex.Language.Options#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#20)]

以降のセクションでは、.NET の正規表現でサポートされているオプションについて説明します。

## <a name="default-options"></a>既定のオプション

<xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> オプションは、オプションが指定されていないことを示します。正規表現エンジンは、このオプションの既定の動作を使用します。 次に例を示します。

- このパターンは、ECMAScript 正規表現ではなく、標準の形式として解釈されます。

- 正規表現パターンは、左から右に入力文字列と照合されます。

- 比較では大文字と小文字が区別されます。

- `^` 言語要素および `$` 言語要素は、入力文字列の先頭および末尾と一致します。

- `.` 言語要素は、`\n` を除く任意の 1 文字と一致します。

- 正規表現パターンに含まれる空白は、リテラルの空白文字として解釈されます。

- パターンを入力文字列と比較するときに、現在のカルチャの規則が使用されます。

- 正規表現パターンのキャプチャ グループは、暗黙的に指定される場合と明示的に指定される場合があります。

> [!NOTE]
> <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> オプションには、等価なインライン オプションは存在しません。 正規表現オプションがインラインで適用されたときに、特定のオプションをオフにすると、既定の動作がオプションごとに復元されます。 たとえば、`(?i)` は大文字と小文字を区別しない比較をオンにし、`(?-i)` は既定の動作 (大文字と小文字を区別する比較) を復元します。

<xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> オプションは正規表現エンジンの既定の動作を表しているので、メソッド呼び出しで明示的に指定されることはほとんどありません。 代わりに、`options` パラメーターを使用せずにコンストラクターまたは静的パターン一致メソッドが呼び出されます。

## <a name="case-insensitive-matching"></a>大文字と小文字を区別しない一致

<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase> オプションまたは `i` インライン オプションを指定すると、大文字と小文字を区別しない一致が実行されます。 既定では、現在のカルチャの大文字と小文字の表記規則が使用されます。

次の例では、"the" で始まるすべての単語と一致する正規表現パターン `\bthe\w*\b` を定義します。 <xref:System.Text.RegularExpressions.Regex.Match%2A> メソッドの最初の呼び出しでは既定の大文字と小文字を区別する比較を使用しているので、結果の出力から、文の先頭の文字列 "The" は一致として処理されていないことがわかります。 これが一致として処理されるのは、オプションを <xref:System.Text.RegularExpressions.Regex.Match%2A> に設定して <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase> メソッドが呼び出された場合です。

[!code-csharp[Conceptual.Regex.Language.Options#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case1.cs#1)]
[!code-vb[Conceptual.Regex.Language.Options#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case1.vb#1)]

次の例では、前の例の正規表現パターンを変更し、`options` パラメーターを使用する代わりに、インライン オプションを使用して、大文字と小文字を区別しない比較を行っています。 最初のパターンでは、文字列 "the" の文字 "t" のみに適用されるよう、グループ化構成体の大文字と小文字を区別しないオプションを定義しています。 オプションの構成体がパターンの先頭にあるので、2 番目のパターンでは、大文字と小文字を区別しないオプションが正規表現全体に適用されています。

[!code-csharp[Conceptual.Regex.Language.Options#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case2.cs#2)]
[!code-vb[Conceptual.Regex.Language.Options#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case2.vb#2)]

## <a name="multiline-mode"></a>複数行モード

<xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションまたは `m` インライン オプションを指定すると、正規表現エンジンでは、複数行で構成される入力文字列の処理が有効になります。 具体的には、`^` 言語要素および `$` 言語要素の解釈を変更して、入力文字列の先頭および末尾ではなく、行の先頭および末尾に一致するものとします。

既定では、`$` は入力文字列の末尾とのみ一致します。 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションを指定した場合は、改行文字 (`\n`) または入力文字列の末尾と一致します。 ただし、復帰とライン フィード文字の組み合わせとは一致しません。 この組み合わせと正常に一致させるには、`\r?$` を単独で使用する代わりに、部分式 `$` を使用します。

次の例では、ボウリング参加者の名前とスコアを抽出し、降順に並べ替えて、<xref:System.Collections.Generic.SortedList%602> コレクションに追加しています。 <xref:System.Text.RegularExpressions.Regex.Matches%2A> メソッドは 2 回呼び出されています。 最初のメソッド呼び出しでは、`^(\w+)\s(\d+)$` という正規表現が使用され、オプションは設定されていません。 出力結果が示すように、正規表現エンジンは入力パターンを入力文字列の先頭および末尾と一致させることができないので、一致は検出されません。 2 番目のメソッド呼び出しでは、正規表現は `^(\w+)\s(\d+)\r?$` に変更されており、オプションは <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> に設定されています。 出力結果が示すように、名前とスコアの照合は正常に行われ、スコアは降順で表示されています。

[!code-csharp[Conceptual.Regex.Language.Options#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline1.cs#3)]
[!code-vb[Conceptual.Regex.Language.Options#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline1.vb#3)]

正規表現パターン `^(\w+)\s(\d+)\r*$` は、次の表に示すように定義されています。

|パターン|説明|
|-------------|-----------------|
|`^`|行の先頭から始まります。|
|`(\w+)`|1 つ以上の単語文字に一致します。 これが最初のキャプチャ グループです。|
|`\s`|空白文字と一致します。|
|`(\d+)`|1 個以上の 10 進数と一致します。 これが 2 番目のキャプチャ グループです。|
|`\r?`|0 個または 1 個の復帰文字と一致します。|
|`$`|行の末尾で終了します。|

次の例は、前の例と等価ですが、インライン オプション `(?m)` を使用して複数行オプションを設定している点が異なります。

[!code-csharp[Conceptual.Regex.Language.Options#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline2.cs#4)]
[!code-vb[Conceptual.Regex.Language.Options#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline2.vb#4)]

## <a name="single-line-mode"></a>単一行モード

<xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションまたは `s` インライン オプションを指定すると、正規表現エンジンでは、入力文字列が単一行で構成されているかのように処理されます。 具体的には、ピリオド (`.`) 言語要素の動作を変更して、改行文字 (`\n` または \u000A) を除く任意の文字ではなく、改行文字を含む任意の 1 文字と一致するようにします。

`.` オプションを使用したときに、<xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> 言語要素の動作が変化する例を次に示します。 正規表現 `^.+` は文字列の先頭から開始し、すべての文字と一致します。 既定では、照合は 1 行目の末尾で終了します。正規表現パターンは復帰文字 `\r` (\u000D) と一致しますが、`\n` とは一致しません。 <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションは入力文字列全体を単一行として解釈するので、`\n` を含む入力文字列内のすべての文字と一致します。

[!code-csharp[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any2.cs#5)]
[!code-vb[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any2.vb#5)]

次の例は、前の例と等価ですが、インライン オプション `(?s)` を使用して単一行モードを有効にしている点が異なります。

[!code-csharp[Conceptual.Regex.Language.Options#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/singleline1.cs#5)]
[!code-vb[Conceptual.Regex.Language.Options#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/singleline1.vb#5)]

## <a name="explicit-captures-only"></a>明示的なキャプチャのみ

既定では、キャプチャ グループは正規表現パターンでかっこを使用することで定義されます。 名前付きのグループには `(?<`*name*`>`*subexpression*`)` 言語オプションで名前または番号が与えられるのに対して、名前のないグループにはインデックスでアクセスできます。 <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトでは、名前のないグループが名前付きのグループよりも優先されます。

グループ化構成体は通常、複数の言語要素に量指定子を適用する場合にのみ使用され、キャプチャされた部分文字列は対象になりません。 たとえば、次の正規表現について考えます。

`\b\(?((\w+),?\s?)+[\.!?]\)?`

この正規表現は、ドキュメントから文末がピリオド、感嘆符、または疑問符である文を抽出することのみを目的とし、結果の文 (<xref:System.Text.RegularExpressions.Match> オブジェクトで表される) のみを対象としています。 コレクション内の個々の単語は対象ではありません。

正規表現エンジンで <xref:System.Text.RegularExpressions.GroupCollection> コレクション オブジェクトと <xref:System.Text.RegularExpressions.CaptureCollection> コレクション オブジェクトの両方を設定する必要があるので、キャプチャ グループが以後、使用されない場合は、この設定の処理が無駄になる可能性があります。 別の方法としては、<xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> オプションまたは `n` インライン オプションを使用して、`(?<`*name*`>` *subexpression*`)` 構成体によって指定された明示的な名前または番号付きのグループのみを有効なキャプチャ対象として指定する方法が挙げられます。

次の例は、`\b\(?((\w+),?\s?)+[\.!?]\)?` 正規表現パターンによって返された一致に関する情報を示しています (<xref:System.Text.RegularExpressions.Regex.Match%2A> メソッドが <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> オプションを使用して呼び出された場合、および使用せずに呼び出された場合)。 最初のメソッド呼び出しの出力結果が示すように、正規表現エンジンでは、キャプチャした部分文字列に関する情報に基づいて、<xref:System.Text.RegularExpressions.GroupCollection> コレクション オブジェクトおよび <xref:System.Text.RegularExpressions.CaptureCollection> コレクション オブジェクトが完全に設定されています。 2 番目のメソッドは、`options` を <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> に設定して呼び出されているので、グループに関する情報をキャプチャしません。

[!code-csharp[Conceptual.Regex.Language.Options#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit1.cs#9)]
[!code-vb[Conceptual.Regex.Language.Options#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit1.vb#9)]

正規表現パターン `\b\(?((?>\w+),?\s?)+[\.!?]\)?` は、次の表に示すように定義されています。

|パターン|説明|
|-------------|-----------------|
|`\b`|ワード境界から始まります。|
|`\(?`|左かっこ ("(") の 0 回または 1 回の繰り返しと一致します。|
|`(?>\w+),?`|1 個以上の単語文字の後に 0 個または 1 個のコンマが続くパターンと一致します。 単語文字の照合中にバックトラックは実行されません。|
|`\s?`|0 個または 1 個の空白文字と一致します。|
|`((\w+),?\s?)+`|1 個以上の単語文字、0 個または 1 個のコンマ、および 0 個または 1 個の空白文字が 1 回以上続くパターンと一致します。|
|`[\.!?]\)?`|3 種類の区切り記号のいずれかの後に 0 個または 1 個の右かっこ (")") が続くパターンと一致します。|

`(?n)` インライン要素を使用して、自動的なキャプチャを抑制することもできます。 次の例では、前の例の正規表現パターンを変更して、`(?n)` インライン要素を <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> オプションの代わりに使用しています。

[!code-csharp[Conceptual.Regex.Language.Options#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit2.cs#10)]
[!code-vb[Conceptual.Regex.Language.Options#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit2.vb#10)]

最後に、`(?n:)` インライン グループ要素を使用して、グループごとに自動的なキャプチャを抑制することもできます。 次の例では、前のパターンを変更して、外部グループ `((?>\w+),?\s?)` で名前のないキャプチャを抑制しています。 この処理では、内部グループでの名前のないキャプチャも抑制されることに注意してください。

[!code-csharp[Conceptual.Regex.Language.Options#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit3.cs#11)]
[!code-vb[Conceptual.Regex.Language.Options#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit3.vb#11)]

## <a name="compiled-regular-expressions"></a>コンパイルされた正規表現

既定では、.NET の正規表現は解釈の対象になります。 <xref:System.Text.RegularExpressions.Regex> オブジェクトがインスタンス化されるか、静的 <xref:System.Text.RegularExpressions.Regex> メソッドが呼び出されたときに、正規表現パターンはカスタム オペコードのセットに解析され、インタープリターがこのオペコードに基づいて正規表現を実行します。 この場合、正規表現エンジンの初期化処理を優先すると、実行時のパフォーマンスが低下するというトレードオフが伴います。

正規表現を逐次解釈する代わりに、<xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> オプションを使用してコンパイルされた正規表現を使用できます。 この場合、パターンは正規表現エンジンに渡され、オペコードのセットに解析されてから、Microsoft Intermediate Language (MSIL) に変換されます。変換されたコードは、共通言語ランタイムに直接渡すことができます。 コンパイルされた正規表現を使用すると、初期化処理に時間を要しますが、実行時のパフォーマンスは向上します。

> [!NOTE]
> 正規表現をコンパイルするには、<xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 値を `options` クラス コンストラクターまたは静的パターン一致メソッドの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡す必要があります。 インライン オプションとしては使用できません。

コンパイルされた正規表現は、静的正規表現とインスタンス正規表現の両方の呼び出しに使用できます。 静的正規表現では、<xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> オプションは正規表現パターン一致メソッドの `options` パラメーターに渡されます。 インスタンス正規表現では、`options` クラス コンストラクターの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡されます。 どちらの場合も、パフォーマンスが向上します。

ただし、パフォーマンスが向上するのは、次の条件を満たしている場合に限定されます。

- 特定の正規表現を表す <xref:System.Text.RegularExpressions.Regex> オブジェクトが正規表現パターン一致メソッドの複数の呼び出しで使用されている。

- <xref:System.Text.RegularExpressions.Regex> オブジェクトがスコープの外に出ることが許可されておらず、再利用できる。

- 静的正規表現が正規表現パターン一致メソッドの複数の呼び出しで使用されている (静的メソッド呼び出しで使用した正規表現は正規表現エンジンによってキャッシュされるので、パフォーマンスの向上が可能になります)。

> [!NOTE]
> <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> オプションは <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> メソッドとは関係なく、定義済みのコンパイルされた正規表現を含む、特殊な目的のアセンブリを作成します。

## <a name="ignore-white-space"></a>空白を無視

既定では、正規表現パターンに含まれる空白には重要な意味があり、正規表現エンジンでは、入力文字列内の空白文字との照合が強制されます。 この結果、正規表現 "`\b\w+\s`" および "`\b\w+`" は、ほぼ等価な正規表現であると言えます。 さらに、シャープ記号 (#) が正規表現パターンに含まれている場合は、照合する対象のリテラル文字として解釈されます。

<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> オプションまたは `x` インライン オプションを指定すると、この既定の動作は次のように変更されます。

- 正規表現パターンでエスケープされていない空白は無視されます。 空白文字を正規表現パターンの一部に含めるには、エスケープする必要があります (たとえば、`\s` や "`\`")。

- シャープ記号 (#) は、リテラル文字ではなく、コメントの先頭として解釈されます。 # 記号から文字列の末尾まで、正規表現パターンに含まれるすべてのテキストは、コメントとして解釈されます。

ただし、次に説明する状況では、<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> オプションを使用している場合でも、正規表現の空白文字は無視されません。

- 文字クラス内の空白は常に、空白として解釈されます。 たとえば、正規表現パターン `[ .,;:]` は、空白文字、ピリオド、コンマ、セミコロン、またはコロンの任意の 1 文字と一致します。

- かっこで囲まれた量指定子 (`{`*n*`}`、`{`*n*`,}`、`{`*n*`,`*m*`}` など) には空白を使用できません。 たとえば、正規表現パターン `\d{1, 3}` は、空白文字が含まれているため、1 ～ 3 桁の数値のどのシーケンスにも一致しません。

- 言語要素を導入する文字シーケンス内では空白を使用できません。 次に例を示します。

  - 言語要素 `(?:`*subexpression*`)` は非キャプチャ グループを表し、要素の `(?:` 部分には空白を埋め込むことはできません。 パターン `(? :`*subexpression*`)` は、正規表現エンジンで解析できないため実行時に <xref:System.ArgumentException> をスローします。パターン `( ?:`*subexpression*`)` は *subexpression* に一致しません。

  - Unicode カテゴリまたは名前付きブロックを表す言語要素 `\p{`*name*`}` の `\p{` 部分には、空白を埋め込むことはできません。 空白を追加すると、この要素は実行時に <xref:System.ArgumentException> をスローします。

このオプションを有効にすると、多くの場合、解析や理解が困難な正規表現を簡略化できます。 正規表現が読みやすくなり、説明も記述できます。

次の例では、正規表現パターンを定義しています。

`\b \(? ( (?>\w+) ,?\s? )+  [\.!?] \)? # Matches an entire sentence.`

このパターンは、「[明示的なキャプチャのみ](#explicit-captures-only)」セクションで定義したパターンと似ています。ただし、<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> オプションを使用して、パターンの空白を無視している点が異なります。

[!code-csharp[Conceptual.Regex.Language.Options#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace1.cs#12)]
[!code-vb[Conceptual.Regex.Language.Options#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace1.vb#12)]

次の例では、`(?x)` インライン オプションを使用して、パターンの空白を無視しています。

[!code-csharp[Conceptual.Regex.Language.Options#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace2.cs#13)]
[!code-vb[Conceptual.Regex.Language.Options#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace2.vb#13)]

## <a name="right-to-left-mode"></a>右から左モード

既定では、正規表現エンジンは左から右の方向に検索します。 この検索の方向を反転するには、<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> オプションを使用します。 検索は、文字列の最後の文字位置から自動的に開始されます。 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.Int32%29?displayProperty=nameWithType> など、開始位置のパラメーターを含むパターン一致メソッドの場合は、検索の開始位置である右端の文字位置のインデックスが開始位置になります。

> [!NOTE]
> 右から左モードを使用するには、<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> 値を `options` クラス コンストラクターまたは静的パターン一致メソッドの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡す必要があります。 インライン オプションとしては使用できません。

<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> オプションは、検索の方向のみを変更します。このオプションを指定すると、正規表現パターンが右から左に解釈されるわけではありません。 たとえば、正規表現 `\bb\w+\s` は、文字 "b" で始まる単語とそれに続く空白文字と一致します。 次の例では、入力文字列は、1 文字以上の "b" を含む 3 つの単語で構成されています。 最初の単語は "b" で始まり、2 番目の単語は "b" で終わり、3 番目の単語は語内に 2 文字の "b" を含んでいます。 この例の出力結果が示すように、最初の単語のみが正規表現パターンと一致しています。

[!code-csharp[Conceptual.Regex.Language.Options#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft1.cs#17)]
[!code-vb[Conceptual.Regex.Language.Options#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft1.vb#17)]

先読みアサーション (`(?=`*subexpression*`)` 言語要素) と後読みアサーション (`(?<=`*subexpression*`)` 言語要素) では、方向が変更されないことにも注意してください。 先読みアサーションでは右方向へ、後読みアサーションでは左方向へ参照が行われます。 たとえば、正規表現 `(?<=\d{1,2}\s)\w+,?\s\d{4}` は後読みアサーションを使用して、月の名前の前にある日付をテストします。 次に、この正規表現は、月と年を照合しています。 先読みアサーションと後読みアサーションについて詳しくは、「[グループ化構成体](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)」をご覧ください。

[!code-csharp[Conceptual.Regex.Language.Options#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft2.cs#18)]
[!code-vb[Conceptual.Regex.Language.Options#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft2.vb#18)]

正規表現パターンは、次の表に示すように定義されています。

|パターン|説明|
|-------------|-----------------|
|`(?<=\d{1,2}\s)`|一致の先頭の前には、1 桁または 2 桁の 10 進数とそれに続く空白が必要です。|
|`\w+`|1 つ以上の単語文字に一致します。|
|`,?`|0 個または 1 個のコンマと一致します。|
|`\s`|空白文字と一致します。|
|`\d{4}`|4 桁の 10 進数と一致します。|

## <a name="ecmascript-matching-behavior"></a>ECMAScript 一致の動作

既定では、正規表現パターンを入力テキストに照合するときに、正規表現エンジンは標準の動作を使用します。 ただし、<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> オプションを使用することで、ECMAScript 一致の動作を使用するように正規表現エンジンに指示できます。

> [!NOTE]
> ECMAScript 準拠の動作を使用するには、<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> 値を `options` クラス コンストラクターまたは静的パターン一致メソッドの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡す必要があります。 インライン オプションとしては使用できません。

<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> オプションと同時に使用できるのは、<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> オプションおよび <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> オプションだけです。 これ以外のオプションと同時に正規表現で使用すると、<xref:System.ArgumentOutOfRangeException> が発生します。

ECMAScript と標準正規表現は、文字クラスの構文、自己参照キャプチャ グループ、および 8 進数と前方参照の解釈という 3 つの点で動作が異なります。

- 文字クラスの構文。 標準正規表現が Unicode をサポートしているのに対して、ECMAScript は Unicode をサポートしていないので、ECMAScript の文字クラスの構文には制限が多く、文字クラスの言語要素によっては意味が異なります。 たとえば ECMAScript は、Unicode カテゴリやブロック要素 (`\p` および `\P`) などの言語要素をサポートしていません。 同様に、単語文字と一致する `\w` 要素は、ECMAScript を使用した場合は `[a-zA-Z_0-9]` 文字クラスと等価になり、標準の動作を使用した場合は `[\p{Ll}\p{Lu}\p{Lt}\p{Lo}\p{Nd}\p{Pc}\p{Lm}]` と等価になります。 詳しくは、「[文字クラス](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)」をご覧ください。

  次の例は、標準パターン一致と ECMAScript パターン一致の違いを示しています。 この例では、単語とそれに続く空白文字と一致する正規表現 `\b(\w+\s*)+` を定義しています。 入力は 2 つの文字列で構成され、一方の文字列ではラテン語文字セットが使用され、もう一方の文字列ではキリル文字セットが使用されています。 出力結果が示すように、ECMAScript 一致を使用した <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> メソッドの呼び出しではキリル文字の単語が照合されないのに対して、標準一致を使用したメソッドの呼び出しではキリル文字の単語が照合されています。

  [!code-csharp[Conceptual.Regex.Language.Options#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript1.cs#16)]
  [!code-vb[Conceptual.Regex.Language.Options#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript1.vb#16)]

- 自己参照キャプチャ グループ。 正規表現キャプチャ クラスが、それ自体への前方参照を持っている場合は、キャプチャの反復処理のたびに更新する必要があります。 次の例に示すように、この機能では、正規表現 `((a+)(\1) ?)+` による入力文字列 " aa aaaa aaaaaa "との照合は、ECMAScript を使用した場合は有効になり、標準一致を使用した場合は無効になります。

  [!code-csharp[Conceptual.Regex.Language.Options#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript2.cs#21)]
  [!code-vb[Conceptual.Regex.Language.Options#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript2.vb#21)]

  正規表現は、次の表に示すように定義されています。

  |パターン|説明|
  |-------------|-----------------|
  |(a+)|文字 "a" と 1 回以上一致します。 これが 2 番目のキャプチャ グループです。|
  |(\1)|最初のキャプチャ グループによってキャプチャされた部分文字列と一致します。 これが 3 番目のキャプチャ グループです。|
  |?|0 個または 1 個の空白文字と一致します。|
  |((a+)(\1) ?)+|1 文字以上の "a"、最初のキャプチャ グループと一致する文字列、および 0 個または 1 個の空白文字が 1 回以上続くパターンと一致します。 これが最初のキャプチャ グループです。|

- 8 進数エスケープと前方参照のあいまいさの解決方法。 標準正規表現と ECMAScript 正規表現による 8 進数と前方参照の解釈の違いの概要を次の表に示します。

  |正規表現|標準の動作|ECMAScript の動作|
  |------------------------|------------------------|-------------------------|
  |`\0` の後に 0 ～ 2 桁の 8 進数字が続く場合|8 進数として解釈されます。 たとえば、`\044` は常に 8 進数値として解釈され、"$" を意味します。|同じ動作です。|
  |`\` の後に 1 ～ 9 の数字が続き、その後に 10 進数字が続かない場合|前方参照として解釈されます。 たとえば、9 番目のキャプチャ グループが存在しない場合でも、`\9` は常に前方参照 9 です。 キャプチャ グループが存在しない場合は、正規表現パーサーは <xref:System.ArgumentException> をスローします。|単一の 10 進数字のキャプチャ グループが存在する場合は、その数字への前方参照です。 それ以外の場合は、値はリテラルとして解釈されます。|
  |`\` の後に 1 ～ 9 の数字が続き、その後に 10 進数字が続く場合|数字は 10 進数値として解釈されます。 そのキャプチャ グループが存在する場合は、式は前方参照として解釈されます。<br /><br /> それ以外の場合は、先行する数字が 377 までの範囲で 8 進数として解釈されます。つまり、値の下位 8 ビットのみが処理の対象になります。 残りの数字はリテラルとして解釈されます。 たとえば、式 `\3000` では、キャプチャ グループ 300 が存在する場合は前方参照 300 として解釈されます。キャプチャ グループ 300 が存在しない場合は 8 進数 300 とそれに続く 0 として解釈されます。|キャプチャを参照できる範囲で、できるだけ多くの桁数が 10 進値に変換され、前方参照として解釈されます。 変換できる数字がない場合は、先行する数字が 377 までの範囲で 8 進数として解釈され、残りの数字はリテラルとして解釈されます。|

## <a name="comparison-using-the-invariant-culture"></a>インバリアント カルチャを使用した比較

既定では、大文字と小文字を区別しない比較を実行するときに、正規表現エンジンは現在のカルチャの大文字と小文字の表記規則を使用して、等価な大文字および小文字を決定します。

ただし、この動作は、比較の種類によっては望ましくない場合があります。具体的な例としては、ユーザー入力と、パスワード、ファイル、URL など、システム リソースの名前を比較する場合が挙げられます。 この場合に該当する例を次に示します。 このコードは、URL が **FILE://** で始まる、任意のリソースへのアクセスをブロックすることを目的としています。 `$FILE://` という正規表現を使用して、文字列に対する大文字と小文字を区別しない一致が試みられます。 ただし、現在のシステム カルチャが tr-TR (トルコ語 (トルコ)) である場合、"I" は "i" の大文字表現には該当しません。 結果として、<xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドの呼び出しでは、`false` が返され、ファイルへのアクセスは許可されます。

[!code-csharp[Conceptual.Regex.Language.Options#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#14)]
[!code-vb[Conceptual.Regex.Language.Options#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#14)]

> [!NOTE]
> 大文字と小文字を区別する文字列比較とインバリアント カルチャを使用する文字列比較の詳細については、「[文字列を使用するためのベスト プラクティス](../../../docs/standard/base-types/best-practices-strings.md)」を参照してください。

現在のカルチャで大文字と小文字を区別しない比較を行う代わりに、<xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> オプションを指定すると、言語のカルチャの違いを無視して、インバリアント カルチャの規則を使用できます。

> [!NOTE]
> インバリアント カルチャを使用して比較を行うには、<xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> 値を `options` クラス コンストラクターまたは静的パターン一致メソッドの <xref:System.Text.RegularExpressions.Regex> パラメーターに渡す必要があります。 インライン オプションとしては使用できません。

次の例は前の例と同じものですが、<xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> を含むオプションを指定して、静的メソッド <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> が呼び出されている点が異なります。 現在のカルチャがトルコ語 (トルコ) に設定されている場合でも、正規表現エンジンでは "FILE" と "file" が正常に一致し、ファイル リソースへのアクセスが拒否されます。

[!code-csharp[Conceptual.Regex.Language.Options#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#15)]
[!code-vb[Conceptual.Regex.Language.Options#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#15)]

## <a name="see-also"></a>関連項目

- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
