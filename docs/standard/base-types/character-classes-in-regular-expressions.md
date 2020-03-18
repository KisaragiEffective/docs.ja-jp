---
title: .NET 正規表現での文字クラス
description: 文字クラスを使用して、.NET 正規表現での文字のセットを表す方法について説明します。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- character classes
- regular expressions, character classes
- characters, matching syntax
- .NET Framework regular expressions, character classes
ms.assetid: 0f8bffab-ee0d-4e0e-9a96-2b4a252bb7e4
ms.openlocfilehash: 07bd63c90bc8d78c9831e2007695a232a85111b1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159339"
---
# <a name="character-classes-in-regular-expressions"></a>正規表現での文字クラス

文字クラスは、いずれかが入力文字列に含まれると一致と見なされる文字のセットを定義します。 .NET の正規表現言語では、次の文字クラスがサポートされます。  
  
- 文字グループの肯定。 入力文字列内の文字が指定した文字のセットのいずれかと一致する必要があります。 詳細については、「[文字グループの肯定](#PositiveGroup)」を参照してください。  
  
- 文字グループの否定。 入力文字列内の文字が指定した文字のセットのいずれかと一致しない必要があります。 詳細については、「[文字グループの否定](#NegativeGroup)」を参照してください。  
  
- 任意の文字。 正規表現の `.` (ドットまたはピリオド) 文字は、`\n` を除く任意の文字と一致するワイルドカード文字です。 詳細については、「[任意の文字](#AnyCharacter)」を参照してください。  
  
- Unicode 一般カテゴリまたは名前付きブロック。 入力文字列内の文字が一致と見なされるには、その文字が特定の Unicode カテゴリのメンバーであるか、または Unicode 文字の連続した範囲内に含まれる必要があります。 詳細については、「[Unicode カテゴリまたは Unicode ブロック](#CategoryOrBlock)」を参照してください。  
  
- Unicode 一般カテゴリまたは名前付きブロックの否定。 入力文字列内の文字が一致と見なされるには、その文字が特定の Unicode カテゴリのメンバーでないか、または Unicode 文字の連続した範囲内に含まれない必要があります。 詳細については、「[Unicode カテゴリまたは Unicode ブロックの否定](#NegativeCategoryOrBlock)」を参照してください。  
  
- 単語に使用される文字。 入力文字列内の文字が、単語内の文字に適した Unicode カテゴリのいずれかに属することができます。 詳細については、「[単語に使用される文字](#WordCharacter)」を参照してください。  
  
- 単語に使用されない文字。 入力文字列内の文字が、単語に使用される文字ではない Unicode カテゴリのいずれかに属することができます。 詳細については、「[単語に使用されない文字](#NonWordCharacter)」を参照してください。  
  
- 空白文字。 入力文字列内の文字が、Unicode 区切り記号および各種制御文字のいずれかです。 詳細については、「[空白文字](#WhitespaceCharacter)」を参照してください。  
  
- 空白以外の文字。 入力文字列内の文字が、空白文字以外の文字のいずれかです。 詳細については、「[空白以外の文字](#NonWhitespaceCharacter)」を参照してください。  
  
- 10 進数。 入力文字列内の文字が、Unicode 10 進数に分類される各種文字のいずれかです。 詳細については、「[10 進数字](#DigitCharacter)」を参照してください。  
  
- 10 進数字以外の文字。 入力文字列内の文字が、Unicode 10 進数以外の文字のいずれかです。 詳細については、「[10 進数字](#NonDigitCharacter)」を参照してください。  
  
 .NET は、文字クラスの減算式をサポートしています。これにより、ある文字クラスから別の文字クラスを除外した結果を文字のセットとして定義できます。 詳細については、「[文字クラス減算](#CharacterClassSubtraction)」を参照してください。  
  
> [!NOTE]
> カテゴリ別の文字に一致する文字クラス (単語文字に一致する [\w](#WordCharacter)、Unicode カテゴリに一致する [\p{}](#CategoryOrBlock) など) は、<xref:System.Globalization.CharUnicodeInfo> クラスを使用して文字カテゴリに関する情報を提供します。  .NET Framework 4.6.2 以降の文字カテゴリは、[Unicode 標準バージョン 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/) に基づいています。 .NET Framework 4 から .NET Framework 4.6.1 では、[Unicode Standard バージョン 6.3.0](https://www.unicode.org/versions/Unicode6.3.0/) に基づいています。  
  
<a name="PositiveGroup"></a>
## <a name="positive-character-group--"></a>文字グループの肯定: [ ]  
 文字グループの肯定では、いずれかが入力文字列に含まれると一致と見なされる文字の一覧を指定します。 この文字の一覧は、個別に指定されることも範囲として指定されることも、その両方であることもあります。  
  
 個別の文字の一覧を指定する構文は次のとおりです。  

`[*character_group*]`

 ここで、*character_group* は、入力文字列に含まれるなら一致と見なされる個別の文字の一覧です。 *character_group* は、リテラル文字、[エスケープ文字](../../../docs/standard/base-types/character-escapes-in-regular-expressions.md)、または文字クラスを 1 つ以上組み合わせて構成されます。  
  
 文字の範囲を指定する構文は次のとおりです。  
  
`[firstCharacter-lastCharacter]`  
  
 ここで、*firstCharacter* は範囲の最初の文字で、*lastCharacter* は範囲の最後の文字です。 文字範囲は連続する一連の文字で、範囲の最初の文字、ハイフン (-)、および範囲の最後の文字を指定することで定義されます。 2 つの文字の Unicode コード ポイントが隣接している場合、それらの文字は連続しています。 *firstCharacter* は、より低いコード ポイントを持つ文字にする必要があります。*lastCharacter* はより高いコード ポイントを持つ文字にする必要があります。

> [!NOTE]
> 正の文字グループには文字セットと文字範囲の両方を含めることができるため、ハイフン文字 (`-`) は、グループの最初の文字または最後の文字でない限り、常に範囲の区切り文字として解釈されます。

文字クラスの肯定を含む一般的な正規表現パターンをいくつか次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`[aeiou]`|すべての母音と一致します。|  
|`[\p{P}\d]`|すべての句読点および 10 進数字と一致します。|  
|`[\s\p{P}]`|すべての空白および句読点と一致します。|  
  
 次の例では、"a" および "e" という文字を含む文字グループの肯定を定義し、入力文字列内で "grey" または "gray" という語の後に別の語が続くと一致と見なされるようにします。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/positivecharclasses.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/positivecharclasses.vb#1)]  
  
 正規表現パターン `gr[ae]y\s\S+?[\s|\p{P}]` は、次のように定義されます。  
  
|パターン|説明|  
|-------------|-----------------|  
|`gr`|リテラル文字 "gr" と一致します。|  
|`[ae]`|"a" または "e" と一致します。|  
|`y\s`|リテラル文字 "y" の後に空白文字が続く語と一致します。|  
|`\S+?`|1 つ以上 (ただし、できるだけ少ない数) の空白以外の文字と一致します。|  
|`[\s\p{P}]`|空白文字または句読点と一致します。|  
  
 次の例は、大文字で始まる語と一致します。 部分式 `[A-Z]` を使用して、A から Z の範囲の大文字を表します。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/range.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/range.vb#3)]  
  
 正規表現 `\b[A-Z]\w*\b` は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`[A-Z]`|A から Z の任意の大文字と一致します。|  
|`\w*`|0 個以上の単語に使用される文字に一致します。|  
|`\b`|ワード境界に一致します。|  
  
<a name="NegativeGroup"></a>
## <a name="negative-character-group-"></a>文字グループの否定: [^]  
 文字グループの否定では、入力文字列に含まれなければ一致と見なされる文字の一覧を指定します。 この文字の一覧は、個別に指定されることも範囲として指定されることも、その両方であることもあります。  
  
個別の文字の一覧を指定する構文は次のとおりです。  

`[*^character_group*]`

 ここで、*character_group* は、入力文字列に含まれない場合に一致と見なされる個別の文字の一覧です。 *character_group* は、リテラル文字、[エスケープ文字](../../../docs/standard/base-types/character-escapes-in-regular-expressions.md)、または文字クラスを 1 つ以上組み合わせて構成されます。  
  
 文字の範囲を指定する構文は次のとおりです。  

`[^*firstCharacter*-*lastCharacter*]`

ここで、*firstCharacter* は範囲の最初の文字で、*lastCharacter* は範囲の最後の文字です。 文字範囲は連続する一連の文字で、範囲の最初の文字、ハイフン (-)、および範囲の最後の文字を指定することで定義されます。 2 つの文字の Unicode コード ポイントが隣接している場合、それらの文字は連続しています。 *firstCharacter* は、より低いコード ポイントを持つ文字にする必要があります。*lastCharacter* はより高いコード ポイントを持つ文字にする必要があります。

> [!NOTE]
> 負の文字グループには文字セットと文字範囲の両方を含めることができるため、ハイフン文字 (`-`) は、グループの最初の文字または最後の文字でない限り、常に範囲の区切り文字として解釈されます。
  
 複数の文字範囲を連結することもできます。 たとえば、"0" ～ "9" の範囲の 10 進数、"a" ～ "f" の範囲の小文字、および "A" ～ "F" の範囲の大文字を指定するには、`[0-9a-fA-F]` を使用します。  
  
 文字グループの否定における先頭のキャレット文字 (`^`) は、文字グループが文字グループの肯定ではなく文字グループの否定であることを示し、省略できません。  
  
> [!IMPORTANT]
> 大規模な正規表現パターンにおける文字グループの否定は、ゼロ幅アサーションではありません。 つまり、正規表現エンジンは、文字グループの否定を評価した後に、入力文字列内で 1 文字進みます。  
  
 文字グループの否定を含む一般的な正規表現パターンをいくつか次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`[^aeiou]`|母音を除くすべての文字と一致します。|  
|`[^\p{P}\d]`|句読点および 10 進数字を除くすべての文字と一致します。|  
  
 次の例は、"th" という文字で始まってその後に "o" が続かない語と一致します。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/negativecharclasses.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/negativecharclasses.vb#2)]  
  
 正規表現 `\bth[^o]\w+\b` は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`th`|リテラル文字 "th" と一致します。|  
|`[^o]`|"o" 以外の任意の文字と一致します。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`\b`|ワード境界で終了します。|  
  
<a name="AnyCharacter"></a>
## <a name="any-character-"></a>任意の文字: .  
 ピリオド文字 (.) は、`\n` (改行文字、\u000A) を除く任意の文字と一致しますが、次の 2 つの制限があります。  
  
- 正規表現パターンが <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションで修飾されている場合、または `.` 文字クラスを含むパターンの一部が `s` オプションで修飾されている場合は、`.` は任意の文字と一致します。 詳細については、「 [正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」を参照してください。  
  
     `.` 文字クラスの既定の動作と <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションが指定されている場合の動作の違いの例を次に示します。 正規表現 `^.+` は文字列の先頭から開始し、すべての文字と一致します。 既定では、照合は 1 行目の末尾で終了します。正規表現パターンは復帰文字 `\r` (\u000D) と一致しますが、`\n` とは一致しません。 <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> オプションは入力文字列全体を単一行として解釈するので、`\n` を含む入力文字列内のすべての文字と一致します。  
  
     [!code-csharp[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any2.cs#5)]
     [!code-vb[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any2.vb#5)]  
  
> [!NOTE]
> `.` 文字クラスは `\n` を除く任意の文字と一致するので、このクラスも `\r` (復帰文字、\u000D) と一致します。  
  
- 文字グループの肯定または文字グループの否定に含まれているピリオドは、文字クラスではなくリテラルのピリオド文字として扱われます。 詳細については、このトピックで前述した「[文字グループの肯定](#PositiveGroup)」および「[文字グループの否定](#NegativeGroup)」を参照してください。 ピリオド文字 (`.`) を文字クラスとしても文字グループの肯定のメンバーとしても含む正規表現を定義する例を次に示します。 正規表現 `\b.*[.?!;:](\s|\z)` はワード境界から開始し、ピリオドを含む 5 つの句読点のいずれかが検出されるまで任意の文字と一致し、空白文字または文字列の末尾と一致します。  
  
     [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any1.cs#4)]
     [!code-vb[Conceptual.RegEx.Language.CharacterClasses#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any1.vb#4)]  
  
> [!NOTE]
> `.` 言語要素は任意の文字と一致するので、正規表現パターンが任意の文字と複数回一致する場合に最短一致の量指定子と共によく使用されます。 詳細については、「 [量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」を参照してください。  
  
<a name="CategoryOrBlock"></a>
## <a name="unicode-category-or-unicode-block-p"></a>Unicode カテゴリまたは Unicode ブロック: \p{}  
 Unicode 規格では、各文字に一般カテゴリが割り当てられています。 たとえば、特定の文字は、英大文字 (`Lu` カテゴリで表されます)、10 進数 (`Nd` カテゴリ)、数学記号 (`Sm` カテゴリ)、または段落区切り記号 (`Zl` カテゴリ) に分類できます。 また、Unicode 規格の特定の文字セットは、特定の範囲またはブロックの連続したコード ポイントに対応します。 たとえば、基本的なラテン語文字セットは \u0000 ～ \u007F で、アラビア語文字セットは \u0600 ～ \u06FF です。  
  
 正規表現の構成要素  
  
 `\p{` *name* `}`  
  
 Unicode 一般カテゴリまたは名前付きブロックに属する任意の文字と一致します。ここで、*name* はカテゴリの省略形または名前付きブロックの名前です。 カテゴリの省略形の一覧については、このトピックで後述する「[サポートされている Unicode 一般カテゴリ](#SupportedUnicodeGeneralCategories)」を参照してください。 名前付きブロックの一覧については、このトピックで後述する「[サポートされている名前付きブロック](#SupportedNamedBlocks)」を参照してください。  
  
 `\p{`*name*`}` 構成要素を使用して Unicode 一般カテゴリ (この場合は `Pd` (Punctuation, Dash: 句読点、ダッシュ) カテゴリ) と名前付きブロック (`IsGreek` 名前付きブロックおよび `IsBasicLatin` 名前付きブロック) の両方を照合する例を次に示します。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/category1.cs#6)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/category1.vb#6)]  
  
 正規表現 `\b(\p{IsGreek}+(\s)?)+\p{Pd}\s(\p{IsBasicLatin}+(\s)?)+` は、次の表に示すように定義されています。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`\p{IsGreek}+`|1 つ以上のギリシャ文字と一致します。|  
|`(\s)?`|0 個または 1 個の空白文字と一致します。|  
|`(\p{IsGreek}+(\s)?)+`|1 つ以上のギリシャ文字の後に 0 個または 1 個の空白文字が 1 回以上続くパターンに一致します。|  
|`\p{Pd}`|Punctuation, Dash (句読点、ダッシュ) 文字と一致します。|  
|`\s`|空白文字と一致します。|  
|`\p{IsBasicLatin}+`|1 つ以上の基本的なラテン文字と一致します。|  
|`(\s)?`|0 個または 1 個の空白文字と一致します。|  
|`(\p{IsBasicLatin}+(\s)?)+`|1 つ以上の基本的なラテン文字の後に 0 個または 1 個の空白文字が 1 回以上続くパターンに一致します。|  
  
<a name="NegativeCategoryOrBlock"></a>
## <a name="negative-unicode-category-or-unicode-block-p"></a>Unicode カテゴリまたは Unicode ブロックの否定: \P{}  
 Unicode 規格では、各文字に一般カテゴリが割り当てられています。 たとえば、特定の文字は、英大文字 (`Lu` カテゴリで表されます)、10 進数 (`Nd` カテゴリ)、数学記号 (`Sm` カテゴリ)、または段落区切り記号 (`Zl` カテゴリ) に分類できます。 また、Unicode 規格の特定の文字セットは、特定の範囲またはブロックの連続したコード ポイントに対応します。 たとえば、基本的なラテン語文字セットは \u0000 ～ \u007F で、アラビア語文字セットは \u0600 ～ \u06FF です。  
  
 正規表現の構成要素  
  
 `\P{` *name* `}`  
  
 Unicode 一般カテゴリにも名前付きブロックにも属さない任意の文字と一致します。ここで、*name* はカテゴリの省略形または名前付きブロックの名前です。 カテゴリの省略形の一覧については、このトピックで後述する「[サポートされている Unicode 一般カテゴリ](#SupportedUnicodeGeneralCategories)」を参照してください。 名前付きブロックの一覧については、このトピックで後述する「[サポートされている名前付きブロック](#SupportedNamedBlocks)」を参照してください。  
  
 `\P{`*name*`}` 構成要素を使用して通貨記号 (この場合は `Sc` (Symbol, Currency: 記号、通貨) カテゴリ) を数値文字列から削除する例を次に示します。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/notcategory1.cs#7)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/notcategory1.vb#7)]  
  
 正規表現パターン `(\P{Sc})+` は、通貨記号以外の 1 つ以上の文字と一致し、実質的に結果文字列から通貨記号を削除します。  
  
<a name="WordCharacter"></a>
## <a name="word-character-w"></a>単語に使用される文字: \w  
 `\w` は、単語に使用される任意の文字と一致します。 単語に使用される文字は、次の表に示す Unicode カテゴリのメンバーです。  
  
|カテゴリ|説明|  
|--------------|-----------------|  
|Ll|Letter, Lowercase (字、小文字)|  
|Lu|Letter, Uppercase (字、大文字)|  
|Lt|Letter, Titlecase (字、タイトル文字)|  
|Lo|Letter, Other (字、その他)|  
|Lm|Letter, Modifier (字、修飾)|  
|Mn|Mark, Nonspacing (結合文字、幅なし)|  
|Nd|Number, Decimal Digit (数、10 進数字)|  
|Pc|Punctuation, Connector (句読点、接続)。 このカテゴリには 10 文字が含まれ、そのうち最もよく使用される文字は LOWLINE 文字 (_)、u+005F です。|  
  
 ECMAScript 準拠の動作が指定された場合、`\w` は `[a-zA-Z_0-9]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
> [!NOTE]
> `\w` 言語要素は単語に使用される任意の文字と一致するので、正規表現パターンが単語に使用される任意の文字の後に特定の単語に使用される文字が続く語と複数回一致する場合に最短一致の量指定子と共によく使用されます。 詳細については、「 [量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」を参照してください。  
  
 `\w` 言語要素を使用して単語内の重複する文字を照合する例を次に示します。 この例では、次のように解釈できる正規表現パターン `(\w)\1` を定義しています。  
  
|要素|説明|  
|-------------|-----------------|  
|(\w)|単語に使用される文字と一致します。 これが最初のキャプチャ グループです。|  
|\1|最初のキャプチャの値と一致します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/wordchar1.cs#8)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/wordchar1.vb#8)]  
  
<a name="NonWordCharacter"></a>
## <a name="non-word-character-w"></a>単語に使用されない文字: \W  
 `\W` は、単語に使用される文字以外の任意の文字と一致します。 \W 言語要素は、次の文字クラスと同じ結果をもたらします。  
  
`[^\p{Ll}\p{Lu}\p{Lt}\p{Lo}\p{Nd}\p{Pc}\p{Lm}]`  
  
 つまり、次の表に示す Unicode カテゴリの文字を除く任意の文字と一致します。  
  
|カテゴリ|説明|  
|--------------|-----------------|  
|Ll|Letter, Lowercase (字、小文字)|  
|Lu|Letter, Uppercase (字、大文字)|  
|Lt|Letter, Titlecase (字、タイトル文字)|  
|Lo|Letter, Other (字、その他)|  
|Lm|Letter, Modifier (字、修飾)|  
|Mn|Mark, Nonspacing (結合文字、幅なし)|  
|Nd|Number, Decimal Digit (数、10 進数字)|  
|Pc|Punctuation, Connector (句読点、接続)。 このカテゴリには 10 文字が含まれ、そのうち最もよく使用される文字は LOWLINE 文字 (_)、u+005F です。|  
  
 ECMAScript 準拠の動作が指定された場合、`\W` は `[^a-zA-Z_0-9]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
> [!NOTE]
> `\W` 言語要素は単語に使用されない任意の文字と一致するので、正規表現パターンが単語に使用されない任意の文字の後に特定の単語に使用されない文字が続く語と複数回一致する場合に最短一致の量指定子と共によく使用されます。 詳細については、「[正規表現での量指定子](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」を参照してください。  
  
 `\W` 文字クラスの例を次に示します。  この例では、単語の後に 1 つまたは 2 つの単語に使用されない文字 (空白や句読点など) が続く場合に一致する正規表現パターン `\b(\w+)(\W){1,2}` を定義しています。 この正規表現の解釈を次の表に示します。  
  
|要素|説明|  
|-------------|-----------------|  
|\b|ワード境界から照合を開始します。|  
|(\w+)|1 つ以上の単語文字に一致します。 これが最初のキャプチャ グループです。|  
|(\W){1,2}|単語に使用されない文字と 1 回または 2 回一致します。 これが 2 番目のキャプチャ グループです。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nonwordchar1.cs#9)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nonwordchar1.vb#9)]  
  
 2 番目のキャプチャ グループの <xref:System.Text.RegularExpressions.Group> オブジェクトには、キャプチャされた単語に使用されない文字が 1 つだけ含まれるので、この例では、<xref:System.Text.RegularExpressions.CaptureCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> オブジェクトから、キャプチャされたすべての単語に使用されない文字を取得します。  
  
<a name="WhitespaceCharacter"></a>
## <a name="whitespace-character-s"></a>空白文字: \s  
 `\s` は任意の空白文字と一致します。 次の表に示すエスケープ シーケンスおよび Unicode カテゴリと同じ結果をもたらします。  
  
|カテゴリ|説明|  
|--------------|-----------------|  
|`\f`|フォーム フィード文字 (\u000C)。|  
|`\n`|改行文字 (\u000A)。|  
|`\r`|復帰文字 (\u000D)。|  
|`\t`|タブ文字 (\u0009)。|  
|`\v`|垂直タブ文字 (\u000B)。|  
|`\x85`|省略記号または NEXT LINE (NEL) 文字 (…) (\u0085)。|  
|`\p{Z}`|任意の区切り記号と一致します。|  
  
 ECMAScript 準拠の動作が指定された場合、`\s` は `[ \f\n\r\t\v]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
 `\s` 文字クラスの例を次に示します。 この例では、"s" または "es" で終わる単語の後に空白文字または入力文字列の末尾が続く場合に一致する正規表現パターン `\b\w+(e)?s(\s|$)` を定義しています。 この正規表現の解釈を次の表に示します。  
  
|要素|説明|  
|-------------|-----------------|  
|\b|ワード境界から照合を開始します。|  
|\w+|1 つ以上の単語文字に一致します。|  
|(e)?|"e" と 0 回または 1 回一致します。|  
|s|"s" と一致します。|  
|(\s&#124;$)|空白文字または入力文字列の末尾と一致します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/whitespace1.cs#10)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/whitespace1.vb#10)]  
  
<a name="NonWhitespaceCharacter"></a>
## <a name="non-whitespace-character-s"></a>空白以外の文字: \S  
 `\S` は、空白文字以外の任意の文字と一致します。 `[^\f\n\r\t\v\x85\p{Z}]` 正規表現パターン、または空白文字と一致する `\s` に相当する正規表現パターンの逆と同じ結果をもたらします。 詳細については、「[空白文字: \s](#WhitespaceCharacter)」を参照してください。  
  
 ECMAScript 準拠の動作が指定された場合、`\S` は `[^ \f\n\r\t\v]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
 `\S` 言語要素の例を次に示します。 正規表現パターン `\b(\S+)\s?` は、空白文字で区切られた文字列と一致します。 一致部分の <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトの 2 番目の要素に一致する文字列が含まれます。 この正規表現の解釈を次の表に示します。  
  
|要素|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(\S+)`|1 つ以上の空白以外の文字と一致します。 これが最初のキャプチャ グループです。|  
|`\s?`|0 個または 1 個の空白文字と一致します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nonwhitespace1.cs#11)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nonwhitespace1.vb#11)]  
  
<a name="DigitCharacter"></a>
## <a name="decimal-digit-character-d"></a>10 進数字: \d  
 `\d` は、10 進数字と一致します。 標準の 10 進数 0 ～ 9 およびその他の各種文字セットの 10 進数を含む `\p{Nd}` 正規表現パターンと同じ結果をもたらします。  
  
 ECMAScript 準拠の動作が指定された場合、`\d` は `[0-9]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
 `\d` 言語要素の例を次に示します。 この例では、入力文字列が米国およびカナダの有効な電話番号を表すかどうかをテストします。 正規表現パターン `^(\(?\d{3}\)?[\s-])?\d{3}-\d{4}$` は、次の表に示すように定義されています。  
  
|要素|説明|  
|-------------|-----------------|  
|`^`|入力文字列の先頭から照合を開始します。|  
|`\(?`|0 個または 1 個のリテラル "(" 文字と一致します。|  
|`\d{3}`|3 個の 10 進数と一致します。|  
|`\)?`|0 個または 1 個のリテラル ")" 文字と一致します。|  
|`[\s-]`|ハイフンまたは空白文字と一致します。|  
|`(\(?\d{3}\)?[\s-])?`|省略可能な左かっこの後に 3 個の 10 進数が続く部分、省略可能な右かっこ、および空白文字またはハイフンと 0 回または 1 回一致します。 これが最初のキャプチャ グループです。|  
|`\d{3}-\d{4}`|3 個の 10 進数の後にハイフンおよび 4 個以上の 10 進数が続く場合に一致します。|  
|`$`|入力文字列の末尾と一致します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/digit1.cs#12)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/digit1.vb#12)]  
  
<a name="NonDigitCharacter"></a>
## <a name="non-digit-character-d"></a>数字以外の文字: \D  
 `\D` は、数字以外の文字と一致します。 `\P{Nd}` 正規表現パターンと同じ結果をもたらします。  
  
 ECMAScript 準拠の動作が指定された場合、`\D` は `[^0-9]` と同じになります。 ECMAScript 正規表現の詳細については、「[正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」の「ECMAScript 一致の動作」のセクションを参照してください。  
  
 \D 言語要素の例を次に示します。 部品番号などの文字列が 10 進数および 10 進数以外の文字を適切に組み合わせて構成されているかどうかをテストします。 正規表現パターン `^\D\d{1,5}\D*$` は、次の表に示すように定義されています。  
  
|要素|説明|  
|-------------|-----------------|  
|`^`|入力文字列の先頭から照合を開始します。|  
|`\D`|数字以外の文字と一致します。|  
|`\d{1,5}`|1 ～ 5 個の 10 進数と一致します。|  
|`\D*`|0 個または 1 個以上の 10 進数以外の文字と一致します。|  
|`$`|入力文字列の末尾と一致します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nondigit1.cs#13)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nondigit1.vb#13)]  
  
<a name="SupportedUnicodeGeneralCategories"></a>
## <a name="supported-unicode-general-categories"></a>サポートされている Unicode 一般カテゴリ  
 Unicode は、次の表に示されている一般カテゴリを定義しています。 詳細については、「[Unicode Character Database (Unicode 文字データベース)](https://www.unicode.org/reports/tr44/)」内の「UCD File Format (UCD ファイル形式)」および「General Category Values (一般カテゴリの値)」を参照してください。  
  
|カテゴリ|説明|  
|--------------|-----------------|  
|`Lu`|Letter, Uppercase (字、大文字)|  
|`Ll`|Letter, Lowercase (字、小文字)|  
|`Lt`|Letter, Titlecase (字、タイトル文字)|  
|`Lm`|Letter, Modifier (字、修飾)|  
|`Lo`|Letter, Other (字、その他)|  
|`L`|すべてのアルファベット文字。 これには、`Lu`、`Ll`、`Lt`、`Lm`、および `Lo` の各文字が含まれます。|  
|`Mn`|Mark, Nonspacing (結合文字、幅なし)|  
|`Mc`|Mark, Spacing Combining (結合文字、幅あり)|  
|`Me`|Mark, Enclosing (結合文字、囲み)|  
|`M`|すべての分音記号。 これには、`Mn`、`Mc`、および `Me` の各カテゴリが含まれます。|  
|`Nd`|Number, Decimal Digit (数、10 進数字)|  
|`Nl`|Number, Letter (数、字)|  
|`No`|Number, Other (数、その他)|  
|`N`|すべての数。 これには、`Nd`、`Nl`、および `No` の各カテゴリが含まれます。|  
|`Pc`|Punctuation, Connector (句読点、接続)|  
|`Pd`|Punctuation, Dash (句読点、ダッシュ)|  
|`Ps`|Punctuation, Open (句読点、開き)|  
|`Pe`|Punctuation, Close (句読点、閉じ)|  
|`Pi`|Punctuation, Initial quote (句読点、開始引用符。使用状況に応じて Ps または Pe のように動作)|  
|`Pf`|Punctuation, Final quote (句読点、終了引用符。使用状況に応じて Ps または Pe のように動作)|  
|`Po`|Punctuation, Other (句読点、その他)|  
|`P`|すべての句読点。 これには、`Pc`、`Pd`、`Ps`、`Pe`、`Pi`、`Pf`、および `Po` の各カテゴリが含まれます。|  
|`Sm`|Symbol, Math (記号、数学)|  
|`Sc`|Symbol, Currency (記号、通貨)|  
|`Sk`|Symbol, Modifier (記号、修飾)|  
|`So`|Symbol, Other (記号、その他)|  
|`S`|すべての記号。 これには、`Sm`、`Sc`、`Sk`、および `So` の各カテゴリが含まれます。|  
|`Zs`|Separator, Space (区切り、空白)|  
|`Zl`|Separator, Line (区切り、行)|  
|`Zp`|Separator, Paragraph (区切り、段落)|  
|`Z`|すべての区切り記号。 これには、`Zs`、`Zl`、および `Zp` の各カテゴリが含まれます。|  
|`Cc`|Other, Control (区切り、制御)|  
|`Cf`|Other, Format (その他、書式)|  
|`Cs`|Other, Surrogate (その他、サロゲート)|  
|`Co`|Other, Private Use (その他、プライベート用途)|  
|`Cn`|Other, Not Assigned (その他、未割り当て。このプロパティを持つ文字はありません)|  
|`C`|すべての制御文字。 これには、`Cc`、`Cf`、`Cs`、`Co`、および `Cn` の各カテゴリが含まれます。|  
  
 特定の文字の Unicode カテゴリを確認するには、その文字を <xref:System.Char.GetUnicodeCategory%2A> メソッドに渡します。 <xref:System.Char.GetUnicodeCategory%2A> メソッドを使用して、選択したラテン文字を含む配列の各要素のカテゴリを確認する例を次に示します。  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/getunicodecategory1.cs#14)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/getunicodecategory1.vb#14)]  
  
<a name="SupportedNamedBlocks"></a>
## <a name="supported-named-blocks"></a>サポートされている名前付きブロック

.NET には、次の表に示す名前付きブロックが用意されています。 サポートされている一連の名前付きブロックは、Unicode 4.0 および Perl 5.6 に基づいています。 名前付きブロックを使用する正規表現については、「[Unicode カテゴリまたは Unicode ブロック: \\p{}](#unicode-category-or-unicode-block-p)」を参照してください。  
  
|コード ポイント範囲|ブロック名|  
|----------------------|----------------|  
|0000 ～ 007F|`IsBasicLatin`|  
|0080 ～ 00FF|`IsLatin-1Supplement`|  
|0100 ～ 017F|`IsLatinExtended-A`|  
|0180 ～ 024F|`IsLatinExtended-B`|  
|0250 ～ 02AF|`IsIPAExtensions`|  
|02B0 ～ 02FF|`IsSpacingModifierLetters`|  
|0300 ～ 036F|`IsCombiningDiacriticalMarks`|  
|0370 ～ 03FF|`IsGreek`<br /><br /> \- または -<br /><br /> `IsGreekandCoptic`|  
|0400 ～ 04FF|`IsCyrillic`|  
|0500 ～ 052F|`IsCyrillicSupplement`|  
|0530 ～ 058F|`IsArmenian`|  
|0590 ～ 05FF|`IsHebrew`|  
|0600 ～ 06FF|`IsArabic`|  
|0700 ～ 074F|`IsSyriac`|  
|0780 ～ 07BF|`IsThaana`|  
|0900 ～ 097F|`IsDevanagari`|  
|0980 ～ 09FF|`IsBengali`|  
|0A00 ～ 0A7F|`IsGurmukhi`|  
|0A80 ～ 0AFF|`IsGujarati`|  
|0B00 ～ 0B7F|`IsOriya`|  
|0B80 ～ 0BFF|`IsTamil`|  
|0C00 ～ 0C7F|`IsTelugu`|  
|0C80 ～ 0CFF|`IsKannada`|  
|0D00 ～ 0D7F|`IsMalayalam`|  
|0D80 ～ 0DFF|`IsSinhala`|  
|0E00 ～ 0E7F|`IsThai`|  
|0E80 ～ 0EFF|`IsLao`|  
|0F00 ～ 0FFF|`IsTibetan`|  
|1000 ～ 109F|`IsMyanmar`|  
|10A0 ～ 10FF|`IsGeorgian`|  
|1100 ～ 11FF|`IsHangulJamo`|  
|1200 ～ 137F|`IsEthiopic`|  
|13A0 ～ 13FF|`IsCherokee`|  
|1400 ～ 167F|`IsUnifiedCanadianAboriginalSyllabics`|  
|1680 ～ 169F|`IsOgham`|  
|16A0 ～ 16FF|`IsRunic`|  
|1700 ～ 171F|`IsTagalog`|  
|1720 ～ 173F|`IsHanunoo`|  
|1740 ～ 175F|`IsBuhid`|  
|1760 ～ 177F|`IsTagbanwa`|  
|1780 ～ 17FF|`IsKhmer`|  
|1800 ～ 18AF|`IsMongolian`|  
|1900 ～ 194F|`IsLimbu`|  
|1950 ～ 197F|`IsTaiLe`|  
|19E0 ～ 19FF|`IsKhmerSymbols`|  
|1D00 ～ 1D7F|`IsPhoneticExtensions`|  
|1E00 ～ 1EFF|`IsLatinExtendedAdditional`|  
|1F00 ～ 1FFF|`IsGreekExtended`|  
|2000 ～ 206F|`IsGeneralPunctuation`|  
|2070 ～ 209F|`IsSuperscriptsandSubscripts`|  
|20A0 ～ 20CF|`IsCurrencySymbols`|  
|20D0 ～ 20FF|`IsCombiningDiacriticalMarksforSymbols`<br /><br /> \- または -<br /><br /> `IsCombiningMarksforSymbols`|  
|2100 ～ 214F|`IsLetterlikeSymbols`|  
|2150 ～ 218F|`IsNumberForms`|  
|2190 ～ 21FF|`IsArrows`|  
|2200 ～ 22FF|`IsMathematicalOperators`|  
|2300 ～ 23FF|`IsMiscellaneousTechnical`|  
|2400 ～ 243F|`IsControlPictures`|  
|2440 ～ 245F|`IsOpticalCharacterRecognition`|  
|2460 ～ 24FF|`IsEnclosedAlphanumerics`|  
|2500 ～ 257F|`IsBoxDrawing`|  
|2580 ～ 259F|`IsBlockElements`|  
|25A0 ～ 25FF|`IsGeometricShapes`|  
|2600 ～ 26FF|`IsMiscellaneousSymbols`|  
|2700 ～ 27BF|`IsDingbats`|  
|27C0 ～ 27EF|`IsMiscellaneousMathematicalSymbols-A`|  
|27F0 ～ 27FF|`IsSupplementalArrows-A`|  
|2800 ～ 28FF|`IsBraillePatterns`|  
|2900 ～ 297F|`IsSupplementalArrows-B`|  
|2980 ～ 29FF|`IsMiscellaneousMathematicalSymbols-B`|  
|2A00 ～ 2AFF|`IsSupplementalMathematicalOperators`|  
|2B00 ～ 2BFF|`IsMiscellaneousSymbolsandArrows`|  
|2E80 ～ 2EFF|`IsCJKRadicalsSupplement`|  
|2F00 ～ 2FDF|`IsKangxiRadicals`|  
|2FF0 ～ 2FFF|`IsIdeographicDescriptionCharacters`|  
|3000 ～ 303F|`IsCJKSymbolsandPunctuation`|  
|3040 ～ 309F|`IsHiragana`|  
|30A0 ～ 30FF|`IsKatakana`|  
|3100 ～ 312F|`IsBopomofo`|  
|3130 ～ 318F|`IsHangulCompatibilityJamo`|  
|3190 ～ 319F|`IsKanbun`|  
|31A0 ～ 31BF|`IsBopomofoExtended`|  
|31F0 ～ 31FF|`IsKatakanaPhoneticExtensions`|  
|3200 ～ 32FF|`IsEnclosedCJKLettersandMonths`|  
|3300 ～ 33FF|`IsCJKCompatibility`|  
|3400 ～ 4DBF|`IsCJKUnifiedIdeographsExtensionA`|  
|4DC0 ～ 4DFF|`IsYijingHexagramSymbols`|  
|4E00 ～ 9FFF|`IsCJKUnifiedIdeographs`|  
|A000 ～ A48F|`IsYiSyllables`|  
|A490 ～ A4CF|`IsYiRadicals`|  
|AC00 ～ D7AF|`IsHangulSyllables`|  
|D800 ～ DB7F|`IsHighSurrogates`|  
|DB80 ～ DBFF|`IsHighPrivateUseSurrogates`|  
|DC00 ～ DFFF|`IsLowSurrogates`|  
|E000 ～ F8FF|`IsPrivateUse` または `IsPrivateUseArea`|  
|F900 ～ FAFF|`IsCJKCompatibilityIdeographs`|  
|FB00 ～ FB4F|`IsAlphabeticPresentationForms`|  
|FB50 ～ FDFF|`IsArabicPresentationForms-A`|  
|FE00 ～ FE0F|`IsVariationSelectors`|  
|FE20 ～ FE2F|`IsCombiningHalfMarks`|  
|FE30 ～ FE4F|`IsCJKCompatibilityForms`|  
|FE50 ～ FE6F|`IsSmallFormVariants`|  
|FE70 ～ FEFF|`IsArabicPresentationForms-B`|  
|FF00 ～ FFEF|`IsHalfwidthandFullwidthForms`|  
|FFF0 ～ FFFF|`IsSpecials`|  
  
<a name="CharacterClassSubtraction"></a>
## <a name="character-class-subtraction-base_group---excluded_group"></a>文字クラスの減算: [base_group - [excluded_group]]  
 文字クラスは、文字のセットを定義します。 文字クラス減算によって、ある文字クラスから別の文字クラスの文字を除外した文字セットが生成されます。  
  
 文字クラス減算式の形式は次のとおりです。  
  
 `[` *base_group* `-[` *excluded_group* `]]`  
  
 角かっこ (`[]`) とハイフン (`-`) は省略できません。 *base_group* は、[文字グループの肯定](#PositiveGroup)または[文字グループの否定](#NegativeGroup)です。 *excluded_group* は、別の文字グループの肯定または文字グループの否定、あるいは別の文字クラス減算式です (つまり文字クラス減算式は入れ子にすることができます)。  
  
 たとえば、"a" ～ "z" の文字範囲で構成される基本グループがあるとします。 "m" を除外した基本グループで構成される文字のセットを定義するには、`[a-z-[m]]` を使用します。 "d"、"j" および "p" の文字を除外した基本グループで構成される文字のセットを定義するには、`[a-z-[djp]]` を使用します。 "m" ～ "p" の文字範囲を除外した基本グループで構成される文字のセットを定義するには、`[a-z-[m-p]]` を使用します。  
  
 入れ子になった文字クラス減算式 `[a-z-[d-w-[m-o]]]` について考えてみます。 この式は、最も内部の文字範囲から順に外側へと評価されます。 まず、"m" ～ "o" の文字範囲が "d" ～ "w" の文字範囲から減算されて、"d" ～ "l" および "p" ～ "w" の文字セットが生成されます。 さらにこのセットが "a" ～ "z" の文字範囲から減算されて、`[abcmnoxyz]` という文字セットが生成されます。  
  
 文字クラス減算では、任意の文字クラスを使用できます。 \u0000 ～ \uFFFF の Unicode 文字から空白文字 (`\s`)、句読点一般カテゴリの文字 (`\p{P}`)、`IsGreek` 名前付きブロック内の文字 (`\p{IsGreek}`)、および Unicode NEXT LINE 制御文字 (\x85) を除いた文字のセットを定義するには、`[\u0000-\uFFFF-[\s\p{P}\p{IsGreek}\x85]]` を使用します。  
  
 有効な結果を生成する文字クラス減算式の文字クラスを選択します。 どの文字にも一致しない空の文字セットを生成する式、または元の基本グループと同じになる式は避けてください。 たとえば、`[\p{IsBasicLatin}-[\x00-\x7F]]` という式は、`IsBasicLatin` 一般カテゴリから `IsBasicLatin` 文字範囲のすべての文字を減算して空のセットを生成します。 同様に、`[a-z-[0-9]]` という式は元の基本グループと同じセットを生成します。  これは、"a" ～ "z" の文字範囲である基本グループに、"0" ～ "9" という 10 進数字の文字範囲から成る除外対象グループ内の文字が含まれないためです。  
  
 入力文字列内の 0 および奇数と一致する正規表現 `^[0-9-[2468]]+$` を定義する例を次に示します。  この正規表現の解釈を次の表に示します。  
  
|要素|説明|  
|-------------|-----------------|  
|^|入力文字列の先頭から照合を開始します。|  
|`[0-9-[2468]]+`|2、4、6、および 8 を除く 0 ～ 9 の文字の 1 回以上の出現と一致します。 つまり、0 または奇数の 1 回以上の出現と一致します。|  
|$|入力文字列の末尾で照合を終了します。|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/classsubtraction1.cs#15)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/classsubtraction1.vb#15)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Char.GetUnicodeCategory%2A>
- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)
