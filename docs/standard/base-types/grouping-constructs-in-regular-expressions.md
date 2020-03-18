---
title: 正規表現でのコンストラクトのグループ化
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lookbehinds
- regular expressions, grouping constructs
- lookaheads
- .NET Framework regular expressions, grouping constructs
- constructs, grouping
- grouping constructs
ms.assetid: 0fc18634-f590-4062-8d5c-f0b71abe405b
ms.openlocfilehash: 5b2ea110837d9d5b905f97ab706af52a594f1c43
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159222"
---
# <a name="grouping-constructs-in-regular-expressions"></a>正規表現でのコンストラクトのグループ化
グループ化構成体は、正規表現の部分式を表し、入力文字列の部分文字列をキャプチャします。 グループ化構成体を使用して、以下を実行できます。  
  
- 入力文字列で繰り返し使用されている部分式を照合する。  
  
- 複数の正規表現言語要素を含む部分式に量指定子を適用する。 量指定子について詳しくは、「 [Quantifiers](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)」をご覧ください。  
  
- <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドおよび <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドによって返される文字列に部分式を含める。  
  
- <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> プロパティから個々の部分式を取得し、一致したテキスト全体とは別に処理する。  
  
 次の表に、.NET 正規表現エンジンでサポートされているグループ化構成体と、これらの構成体がキャプチャまたは非キャプチャのいずれであるかを示します。  
  
|グループ化構成体|キャプチャまたは非キャプチャ|  
|------------------------|-------------------------------|  
|[一致した部分式](#matched_subexpression)|キャプチャ|  
|[一致した名前付き部分式](#named_matched_subexpression)|キャプチャ|  
|[グループ定義の均等化](#balancing_group_definition)|キャプチャ|  
|[非キャプチャ グループ](#noncapturing_group)|非キャプチャ|  
|[グループ オプション](#group_options)|非キャプチャ|  
|[ゼロ幅の肯定先読みアサーション](#zerowidth_positive_lookahead_assertion)|非キャプチャ|  
|[ゼロ幅の否定先読みアサーション](#zerowidth_negative_lookahead_assertion)|非キャプチャ|  
|[ゼロ幅の正の後読みアサーション](#zerowidth_positive_lookbehind_assertion)|非キャプチャ|  
|[ゼロ幅の負の後読みアサーション](#zerowidth_negative_lookbehind_assertion)|非キャプチャ|  
|[アトミック グループ](#atomic_groups)|非キャプチャ|  
  
 グループと正規表現オブジェクト モデルの詳細については、「 [グループ化構成体および正規表現オブジェクト](#Objects)」を参照してください。  
  
<a name="matched_subexpression"></a>
## <a name="matched-subexpressions"></a>一致した部分式  
 次のグループ化構成体は、一致した部分式をキャプチャします。  
  
 `(` *subexpression* `)`  
  
 *subexpression* は有効な正規表現パターンです。 かっこを使用するキャプチャには、正規表現の左かっこの順番に基づいて、左から右に自動的に 1 から始まる番号が付けられます。 番号が 0 になるキャプチャは、正規表現パターン全体と一致するテキストです。  
  
> [!NOTE]
> 既定では、 `(`*subexpression*`)` 言語要素が、一致する部分式をキャプチャします。 ただし、正規表現パターン一致メソッドの <xref:System.Text.RegularExpressions.RegexOptions> パラメーターに <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> フラグが含まれる場合や、`n` オプションがこの部分式に適用される場合は (このトピックで後述する「[グループ オプション](#group_options)」を参照)、一致した部分式はキャプチャされません。  
  
 キャプチャされたグループにアクセスする方法は 4 つあります。  
  
- 正規表現内で前方参照構成体を使用する。 `\`*number* という構文を使うと、一致した部分式が同じ正規表現内で参照されます。ここで、*number* はキャプチャされた部分式の序数です。  
  
- 正規表現内で名前付き前方参照構成体を使用する。 `\k<`*name*`>` という構文 (*name* はキャプチャ グループの名前)、または `\k<`*number*`>` という構文 (*number* はキャプチャ グループの序数) を使用すると、一致した部分式が同じ正規表現内で参照されます。 キャプチャ グループには、その序数と同じ既定の名前が付いています。 詳細については、このトピックで後述する「 [一致した名前付き部分式](#named_matched_subexpression) 」を参照してください。  
  
- `$`*number* 置換シーケンスを、<xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドの呼び出しで使用する。ここで、*number* はキャプチャされた部分式の序数です。  
  
- プログラムで <xref:System.Text.RegularExpressions.GroupCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> オブジェクトを使用する。 コレクション内の位置 0 にあるメンバーは、正規表現に一致した文字列全体を表します。 後続の各メンバーは、一致した部分式を表します。 詳しくは、「 [Grouping Constructs and Regular Expression Objects](#Objects) 」セクションをご覧ください。  
  
 次の例は、テキスト内で重複している単語を識別する正規表現を示しています。 正規表現パターンの 2 つのキャプチャ グループは、2 つの重複する単語を表します。 2 番目の単語は、入力文字列内の開始位置を報告するためにキャプチャされます。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping1.cs#1)]
 [!code-vb[RegularExpressions.Language.Grouping#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping1.vb#1)]  
  
 正規表現パターンは次のとおりです。  
  
`(\w+)\s(\1)\W`  
  
 次の表に、正規表現パターンがどのように解釈されるかを示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`(\w+)`|1 つ以上の単語文字に一致します。 これが最初のキャプチャ グループです。|  
|`\s`|空白文字と一致します。|  
|`(\1)`|最初のキャプチャ グループの文字列と一致します。 これが 2 番目のキャプチャ グループです。 例では、これをキャプチャ グループに割り当てて、重複する単語の開始位置を `Match.Index` オブジェクトを使用する。|  
|`\W`|空白や句読点などの単語文字以外の文字と一致します。 これにより、正規表現パターンが、最初のキャプチャ グループの単語で始まる単語と一致しなくなります。|  
  
<a name="named_matched_subexpression"></a>
## <a name="named-matched-subexpressions"></a>一致した名前付き部分式  
 次のグループ化構成体は、一致した部分式をキャプチャし、その部分式に名前または番号でアクセスできるようにします。  
  
`(?<name>subexpression)`  
  
 または  
  
`(?'name'subexpression)`  
  
 ここで、 *name* は有効なグループ名、 *subexpression* は有効な正規表現パターンです。 *name* は区切り記号を含まず、先頭が数字以外である必要があります。  
  
> [!NOTE]
> 正規表現パターン一致メソッドの <xref:System.Text.RegularExpressions.RegexOptions> パラメーターに <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> フラグが含まれる場合や、 `n` オプションがこの部分式に適用される場合は (このトピックで後述する「 [グループ オプション](#group_options) 」を参照)、部分式をキャプチャする唯一の方法は、キャプチャ グループの名前を明示的に指定することです。  
  
 キャプチャされた名前付きグループには次の方法でアクセスできます。  
  
- 正規表現内で名前付き前方参照構成体を使用する。 `\k<`*name*`>` という構文を使用すると、一致した部分式が同じ正規表現内で参照されます。ここで、*name* はキャプチャされた部分式の名前です。  
  
- 正規表現内で前方参照構成体を使用する。 `\`*number* という構文を使うと、一致した部分式が同じ正規表現内で参照されます。ここで、*number* はキャプチャされた部分式の序数です。 一致した名前付き部分式には、一致した部分式の後、左から右に連続した番号が付けられます。  
  
- `${`*name*`}` 置換シーケンスを、<xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドの呼び出しで使用する。ここで、*name* はキャプチャされた部分式の名前です。  
  
- `$`*number* 置換シーケンスを、<xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> メソッドの呼び出しで使用する。ここで、*number* はキャプチャされた部分式の序数です。  
  
- プログラムで <xref:System.Text.RegularExpressions.GroupCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> オブジェクトを使用する。 コレクション内の位置 0 にあるメンバーは、正規表現に一致した文字列全体を表します。 後続の各メンバーは、一致した部分式を表します。 キャプチャされた名前付きグループは、キャプチャされた番号付きグループの後にコレクションに格納されます。  
  
- プログラムで <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトのインデクサー (C# の場合) またはその <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> プロパティ (Visual Basic の場合) に部分式名を指定する。  
  
 簡単な正規表現パターンで、プログラムまたは正規表現言語構文を使用して番号付き (名前のない) グループおよび名前付きグループをどのように参照できるかを示します。 正規表現 `((?<One>abc)\d+)?(?<Two>xyz)(.*)` からは、次のように番号と名前の付いたキャプチャ グループが作成されます。 最初のキャプチャ グループ (番号 0) は、常にパターン全体を指します。  
  
|Number|name|パターン|  
|------------|----------|-------------|  
|0|0 (既定名)|`((?<One>abc)\d+)?(?<Two>xyz)(.*)`|  
|1|1 (既定名)|`((?<One>abc)\d+)`|  
|2|2 (既定名)|`(.*)`|  
|3|1|`(?<One>abc)`|  
|4|2 つ|`(?<Two>xyz)`|  
  
 次の例は、重複している単語、および重複している各単語の直後にある単語を識別する正規表現を示しています。 この正規表現パターンでは、重複している単語を表す `duplicateWord`と重複している単語の後にある単語を表す `nextWord`の 2 つの名前付き部分式を定義しています。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping2.cs#2)]
 [!code-vb[RegularExpressions.Language.Grouping#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping2.vb#2)]  
  
 正規表現パターンは次のとおりです。  
  
`(?<duplicateWord>\w+)\s\k<duplicateWord>\W(?<nextWord>\w+)`  
  
 次の表に、正規表現がどのように解釈されるかを示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`(?<duplicateWord>\w+)`|1 つ以上の単語文字に一致します。 このキャプチャ グループに `duplicateWord` という名前を付けます。|  
|`\s`|空白文字と一致します。|  
|`\k<duplicateWord>`|`duplicateWord`という名前のキャプチャ済みグループの文字列と一致します。|  
|`\W`|空白や句読点などの単語文字以外の文字と一致します。 これにより、正規表現パターンが、最初のキャプチャ グループの単語で始まる単語と一致しなくなります。|  
|`(?<nextWord>\w+)`|1 つ以上の単語文字に一致します。 このキャプチャ グループに `nextWord` という名前を付けます。|  
  
 グループ名は、正規表現で繰り返し使用できます。 たとえば、次の例が示すように、複数のグループに `digit` という名前を付けることができます。 名前が重複する場合、 <xref:System.Text.RegularExpressions.Group> オブジェクトの値は、入力文字列の最後の正常なキャプチャによって決定されます。 さらに、グループ名が重複されない場合と同じように、各キャプチャについての情報が <xref:System.Text.RegularExpressions.CaptureCollection> 格納されます。  
  
 次の例では、正規表現 `\D+(?<digit>\d+)\D+(?<digit>\d+)?` に `digit`という名前のグループの 2 回の出現が含まれています。 最初の `digit` という名前のグループは、1 桁以上の数字をキャプチャします。 2 番目の `digit` という名前のグループは、1 桁以上の数字の 0 回か 1 回の出現をキャプチャします。 例の出力が示すように、2 番目のキャプチャ グループがテキストと正常に一致する場合、そのテキストの値は <xref:System.Text.RegularExpressions.Group> オブジェクトの値を定義します。 2 番目のキャプチャ グループが入力文字列と一致しない場合、最後に成功した一致の値によって <xref:System.Text.RegularExpressions.Group> オブジェクトの値が定義されます。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#12](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/duplicate1.cs#12)]
 [!code-vb[RegularExpressions.Language.Grouping#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/duplicate1.vb#12)]  
  
 次の表に、正規表現がどのように解釈されるかを示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\D+`|1 個以上の 10 進数以外の文字と一致します。|  
|`(?<digit>\d+)`|1 個以上の 10 進数の文字と一致します。 一致を `digit` という名前のグループに割り当てます。|  
|`\D+`|1 個以上の 10 進数以外の文字と一致します。|  
|`(?<digit>\d+)?`|1 つ以上の 10 進数の文字の 0 回または 1 回の出現と一致します。 一致を `digit` という名前のグループに割り当てます。|  
  
<a name="balancing_group_definition"></a>
## <a name="balancing-group-definitions"></a>グループ定義の均等化  
 グループ定義の均等化では、既に定義されていたグループの定義を削除し、既に定義されていたグループと現在のグループの間隔を現在のグループに格納します。 このグループ化構成体の形式は次のとおりです。  
  
`(?<name1-name2>subexpression)`  
  
 または  
  
`(?'name1-name2' subexpression)`
  
 ここで、 *name1* は現在のグループ (省略可能) で、 *name2* は既に定義されていたグループで、 *subexpression* は有効な正規表現パターンです。 グループ定義の均等化では、 *name2* の定義を削除し、 *name2* と *name1* の間隔を *name1*に格納します。 *name2* グループが定義されていない場合、一致はバックトラックされます。 *name2* の最後の定義を削除すると、 *name2*の以前の定義がわかるため、この構成体によって、かっこや左右の角かっこなど入れ子になった構成体を追跡するカウンターとして *name2* グループのキャプチャのスタックを使用できます。  
  
 グループ定義の均等化では、 *name2* をスタックとして使用します。 入れ子になった各構成体の開始文字が、グループとその <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> コレクションに配置されます。 終了文字が一致すると、対応する開始文字がグループから削除され、 <xref:System.Text.RegularExpressions.Group.Captures%2A> コレクションが 1 つ減らされます。 入れ子になったすべての構成体の開始文字と終了文字が一致したら、*name2* は空になります。  
  
> [!NOTE]
> 入れ子になった構成体の適切な開始文字と終了文字を使用するように次の例の正規表現を変更すると、その正規表現を使用して、複数の入れ子になったメソッド呼び出しを含む数式やプログラム コード行などのほとんどの入れ子になった構成体を処理できるようになります。  
  
 次の例では、グループ定義の均等化を使用して、入力文字列内の左と右の山かっこ (<>) を一致させています。 この例では、一致する山かっこのペアを追跡するスタックのように使用される `Open` および `Close`という 2 つの名前付きグループを定義しています。 キャプチャされた各左山かっこは `Open` グループのキャプチャ コレクションに挿入され、キャプチャされた各右山かっこは `Close` グループのキャプチャ コレクションに挿入されます。 グループ定義の均等化によって、各左山かっこに一致する右山かっこが存在することが確認されます。 存在しない場合、最後のサブパターンの `(?(Open)(?!))`は、 `Open` グループが空でない場合 (したがって、いくつかの入れ子になった構成体に右山かっこがない場合) にのみ評価されます。 最後のサブパターンが評価されると、 `(?!)` サブパターンが必ず失敗するゼロ幅の否定先読みアサーションであるため、照合は失敗します。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping3.cs#3)]
 [!code-vb[RegularExpressions.Language.Grouping#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping3.vb#3)]  
  
 正規表現パターンは次のとおりです。  
  
`^[^<>]*(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*(?(Open)(?!))$`  
  
 この正規表現パターンは、次のように解釈されます。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`^`|文字列の先頭から始まります。|  
|`[^<>]*`|左または右の山かっこではない 0 個以上の文字と一致します。|  
|`(?'Open'<)`|左山かっこと一致し、そのかっこを `Open`という名前のグループに代入します。|  
|`[^<>]*`|左または右の山かっこではない 0 個以上の文字と一致します。|  
|`((?'Open'<)[^<>]*)+`|左山かっこの後に左または右の山かっこではない 0 個以上の文字が続くパターンの 1 回以上の出現と一致します。 これが 2 番目のキャプチャ グループです。|  
|`(?'Close-Open'>)`|右山かっこと一致し、`Open` グループと現在のグループの間の部分文字列を `Close` グループに代入して、`Open` グループの定義を削除します。|  
|`[^<>]*`|左または右の山かっこではない任意の文字の 0 回以上の出現と一致します。|  
|`((?'Close-Open'>)[^<>]*)+`|右山かっこの後に左または右の山かっこではない任意の文字の 0 回以上の出現が続くパターンの 1 回以上の出現と一致します。 右山かっこと一致したときに、 `Open` グループと現在のグループの間の部分文字列を `Close` グループに代入して、 `Open` グループの定義を削除します。 これが 3 番目のキャプチャ グループです。|  
|`(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*`|左山かっこの 1 回以上の出現の後に山かっこではない 0 個以上の文字が続き、その後に右山かっこの 1 回以上の出現が続き、その後に山かっこ以外の 0 回以上の出現が続くパターンの 0 回以上の出現と一致します。 右山かっこと一致したときに、 `Open` グループの定義を削除して、 `Open` グループと現在のグループの間の部分文字列を `Close` グループに代入します。 これが最初のキャプチャ グループです。|  
|`(?(Open)(?!))`|`Open` グループが存在し、空の文字列が一致する場合、照合を破棄してください。ただし、文字列内での正規表現エンジンの位置は進めないでください。 これはゼロ幅の否定先読みアサーションです。 空の文字列が常に入力文字列に暗黙的に存在するため、この照合は必ず失敗します。 この照合の失敗は、山かっこの数が一致していないことを示します。|  
|`$`|入力文字列の末尾と一致します。|  
  
 最後の部分式の `(?(Open)(?!))`は、入力文字列内の入れ子の構成体の数が一致しているかどうかを示します (各左山かっこに一致する右山かっこが存在するかどうかなど)。 詳しくは、[代替コンストラクト](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md)をご覧ください。 `Open` グループが定義されている場合は、正規表現エンジンによって、入力文字列内で部分式 `(?!)` の照合が試行されます。 `Open` グループは、入れ子の構成体の数が一致していない場合にのみ定義する必要があります。 したがって、入力文字列で照合されるパターンでは、照合は常に失敗します。 この場合、 `(?!)` は、常に失敗するゼロ幅の否定先読みアサーションです。入力文字列内の次の位置に、必ず空の文字列が暗黙的に存在するためです。  
  
 この例では、正規表現エンジンによって、次の表に示すように入力文字列 "\<abc><mno\<xyz>>" が評価されます。  
  
|手順|パターン|結果|  
|----------|-------------|------------|  
|1|`^`|入力文字列の先頭から照合を開始します。|  
|2|`[^<>]*`|左山かっこの前にある山かっこではない文字を検索します。一致する項目は見つかりません。|  
|3|`(((?'Open'<)`|"\<abc>" の左山かっこと一致し、そのかっこを `Open` グループに代入します。|  
|4|`[^<>]*`|"abc" と一致します。|  
|5|`)+`|"<abc" が 2 番目のキャプチャ グループの値になります。<br /><br /> 入力文字列内の次の文字は左山かっこではないので、正規表現エンジンは `(?'Open'<)[^<>]*)` サブパターンに戻りません。|  
|6|`((?'Close-Open'>)`|"\<abc>" の右山かっこと一致し、`Open` グループと右山かっこの間の部分文字列である "abc" を `Close` グループに代入して、`Open` グループの現在の値 ("<") を削除してグループを空にします。|  
|7|`[^<>]*`|右山かっこの後にある山かっこではない文字を検索します。一致する項目は見つかりません。|  
|8|`)+`|3 番目のキャプチャ グループの値は ">" です。<br /><br /> 入力文字列内の次の文字は右山かっこではないので、正規表現エンジンは `((?'Close-Open'>)[^<>]*)` サブパターンに戻りません。|  
|9|`)*`|最初のキャプチャ グループの値は "\<abc>" です。<br /><br /> 入力文字列内の次の文字は左山かっこなので、正規表現エンジンは `(((?'Open'<)` サブパターンに戻ります。|  
|10|`(((?'Open'<)`|"\<mno" の左山かっこと一致し、そのかっこを `Open` グループに代入します。 その <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> コレクションには、現在、単一の値 "<" が含まれています。|  
|11|`[^<>]*`|"mno" と一致します。|  
|12|`)+`|"<mno" が 2 番目のキャプチャ グループの値になります。<br /><br /> 入力文字列内の次の文字は左山かっこなので、正規表現エンジンは `(?'Open'<)[^<>]*)` サブパターンに戻ります。|  
|13|`(((?'Open'<)`|"\<xyz>" の左山かっこと一致し、そのかっこを `Open` グループに代入します。 現在、<xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> グループの `Open` コレクションには、"\<mno" の左山かっこと "\<xyz>" の左山かっこの 2 つのキャプチャが含まれています。|  
|14|`[^<>]*`|"xyz" と一致します。|  
|15|`)+`|"<xyz" が 2 番目のキャプチャ グループの値になります。<br /><br /> 入力文字列内の次の文字は左山かっこではないので、正規表現エンジンは `(?'Open'<)[^<>]*)` サブパターンに戻りません。|  
|16|`((?'Close-Open'>)`|"\<xyz>" の右山かっこと一致します。 "xyz" は `Open` グループと右山かっこの間の部分文字列を `Close` グループに代入して、`Open` グループの現在の値を削除します。 前のキャプチャの値 ("\<mno" の左山かっこ) が `Open` グループの現在の値になります。 <xref:System.Text.RegularExpressions.Group.Captures%2A> グループの `Open` コレクションには、現在、単一のキャプチャ ("\<xyz>" の左山かっこ) が含まれています。|  
|17|`[^<>]*`|山かっこではない文字を検索します。一致する項目は見つかりません。|  
|18|`)+`|3 番目のキャプチャ グループの値は ">" です。<br /><br /> 入力文字列内の次の文字は右山かっこなので、正規表現エンジンは `((?'Close-Open'>)[^<>]*)` サブパターンに戻ります。|  
|19|`((?'Close-Open'>)`|"xyz>>" の最後の右山かっこと一致し、"mno\<xyz>" (`Open` グループと右山かっこの間の部分文字列) を `Close` グループに代入して、`Open` グループの現在の値を削除します。 `Open` グループは空になります。|  
|20|`[^<>]*`|山かっこではない文字を検索します。一致する項目は見つかりません。|  
|21|`)+`|3 番目のキャプチャ グループの値は ">" です。<br /><br /> 入力文字列内の次の文字は右山かっこではないので、正規表現エンジンは `((?'Close-Open'>)[^<>]*)` サブパターンに戻りません。|  
|22|`)*`|最初のキャプチャ グループの値は "<mno\<xyz>>" です。<br /><br /> 入力文字列内の次の文字は左山かっこではないので、正規表現エンジンは `(((?'Open'<)` サブパターンに戻りません。|  
|23|`(?(Open)(?!))`|`Open` グループは定義されていないので、照合は試行されません。|  
|24|`$`|入力文字列の末尾と一致します。|  
  
<a name="noncapturing_group"></a>
## <a name="noncapturing-groups"></a>非キャプチャ グループ  
 次のグループ化構成体は、部分式と一致した部分文字列をキャプチャしません。  
  
`(?:subexpression)`
  
 *subexpression* は有効な正規表現パターンです。 非キャプチャ グループ構成体は、通常、量指定子がグループに適用されるがグループによってキャプチャされた部分文字列は対象にならない場合に使用されます。  
  
> [!NOTE]
> 正規表現に入れ子になったグループ化構成体が含まれる場合、外側の非キャプチャ グループ構成体は内側の入れ子になったグループ構成体には適用されません。  
  
 次の例は、非キャプチャ グループを含む正規表現を示しています。 出力には、キャプチャされたグループは含まれません。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/noncapture1.cs#5)]
 [!code-vb[RegularExpressions.Language.Grouping#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/noncapture1.vb#5)]  
  
 正規表現 `(?:\b(?:\w+)\W*)+\.` は、ピリオドで終了する文と一致します。 この正規表現は個々の単語ではなく文に焦点を合わせているので、グループ化構成体は量指定子としてのみ使用されます。 この正規表現パターンの解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(?:\w+)`|1 つ以上の単語文字に一致します。 一致したテキストをキャプチャされたグループに代入しません。|  
|`\W*`|0 個以上の単語文字に使用されない文字と一致します。|  
|`(?:\b(?:\w+)\W*)+`|ワード境界から始まる 1 個以上の単語文字、および 0 個以上の単語文字に使用されない文字が 1 回以上続くパターンと一致します。 一致したテキストをキャプチャされたグループに代入しません。|  
|`\.`|ピリオドと一致します。|  
  
<a name="group_options"></a>
## <a name="group-options"></a>グループ オプション  
 次のグループ化構成体は、指定したオプションを部分式に適用または無効にします。  
  
 `(?imnsx-imnsx:` *subexpression* `)`  
  
 *subexpression* は有効な正規表現パターンです。 たとえば、 `(?i-s:)` によって、大文字小文字の区別が有効になり単一行モードが無効になります。 指定できるインライン オプションの詳細については、「 [正規表現のオプション](../../../docs/standard/base-types/regular-expression-options.md)」を参照してください。  
  
> [!NOTE]
> <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> クラス コンストラクターまたは静的メソッドを使用すると、部分式ではなく正規表現全体に適用されるオプションを指定できます。 また、 `(?imnsx-imnsx)` 言語コンストラクトを使うと、正規表現内の特定の位置より後に適用されるインライン オプションを指定できます。  
  
 グループ オプション構成体はキャプチャ グループではありません。 つまり、 *subexpression* によってキャプチャされる文字列の一部は一致に含まれますが、キャプチャ グループに含まれることも <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトにデータを設定するために使用されることもありません。  
  
 たとえば、次の例の正規表現 `\b(?ix: d \w+)\s` では、グループ化構成体のインライン オプションを使用して、文字 "d" で始まるすべての単語を識別するときに、大文字と小文字を区別しない一致を有効にすると同時に、パターンの空白を無視します。 正規表現は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(?ix: d \w+)`|大文字と小文字を区別しない一致を使用してこのパターンの空白を無視し、"d" の後に単語文字に使用される文字が 1 個以上続くパターンと一致します。|  
|`\s`|空白文字と一致します。|  
  
 [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
 [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]  
  
<a name="zerowidth_positive_lookahead_assertion"></a>
## <a name="zero-width-positive-lookahead-assertions"></a>ゼロ幅の肯定先読みアサーション  
 次のグループ化構成体は、ゼロ幅の肯定先読みアサーションを定義します。  
  
 `(?=` *subexpression* `)`  
  
 ここで、*subexpression* は正規表現パターンです。 一致と見なされるためには、入力文字列が *subexpression*の正規表現パターンと一致する必要がありますが、一致した部分文字列は一致結果には含まれません。 ゼロ幅の肯定先読みアサーションはバックトラックしません。  
  
 通常、ゼロ幅の肯定先読みアサーションは正規表現パターンの末尾にあります。 一致と見なされるには文字列の末尾にある必要がありますが、一致には含まれない部分文字列を定義します。 これは、過度なバックトラッキングを防ぐためにも役立ちます。 ゼロ幅の肯定先読みアサーションを使用して、特定のキャプチャされたグループの先頭テキストが、そのキャプチャされたグループに対して定義されたパターンのサブセットと一致するテキストになるようにすることができます。 たとえば、キャプチャ グループが連続する単語文字と一致する場合に、ゼロ幅の肯定先読みアサーションを使用して、先頭の文字がアルファベット大文字になるように要求できます。  
  
 次の例では、ゼロ幅の肯定先読みアサーションを使用して、入力文字列内の動詞 "is" の前にある単語を照合します。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#6](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookahead1.cs#6)]
 [!code-vb[RegularExpressions.Language.Grouping#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookahead1.vb#6)]  
  
 この正規表現 `\b\w+(?=\sis\b)` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`(?=\sis\b)`|単語文字に使用される文字の後に、空白文字とワード境界で終了する文字列 "is" が続くかどうかを確認します。 該当する場合は一致と見なされます。|  
  
<a name="zerowidth_negative_lookahead_assertion"></a>
## <a name="zero-width-negative-lookahead-assertions"></a>ゼロ幅の否定先読みアサーション  
 次のグループ化構成体は、ゼロ幅の否定先読みアサーションを定義します。  
  
 `(?!` *subexpression* `)`  
  
 ここで、*subexpression* は正規表現パターンです。 一致と見なされるためには、入力文字列が *subexpression*の正規表現パターンと一致しない必要がありますが、一致した文字列は一致結果には含まれません。  
  
 通常、ゼロ幅の否定先読みアサーションは正規表現の先頭または末尾で使用されます。 正規表現の先頭の場合は、類似してもより一般的なパターンを照合するように正規表現の先頭で定義されているときに、一致しない必要がある特定のパターンを定義できます。 この場合は、バックトラッキングを制限するためによく使用されます。 正規表現の末尾の場合は、一致の末尾に出現できない部分式を定義できます。  
  
 次の例では、正規表現の先頭でゼロ幅の先読みアサーションを使用して、"un" で始まらない単語を照合する正規表現を定義します。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#7](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead1.cs#7)]
 [!code-vb[RegularExpressions.Language.Grouping#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead1.vb#7)]  
  
 この正規表現 `\b(?!un)\w+\b` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(?!un)`|次の 2 文字が "un" であるかどうかを確認します。 該当しない場合は一致と考えられます。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`\b`|ワード境界で照合を終了します。|  
  
 次の例では、正規表現の末尾でゼロ幅の先読みアサーションを使用して、区切り文字で終わらない単語を照合する正規表現を定義します。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#8](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead2.cs#8)]
 [!code-vb[RegularExpressions.Language.Grouping#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead2.vb#8)]  
  
 この正規表現 `\b\w+\b(?!\p{P})` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\w+`|1 つ以上の単語文字に一致します。|  
|`\b`|ワード境界で照合を終了します。|  
|`\p{P})`|次の文字が区切り記号 (ピリオドやコンマなど) ではない場合は一致と見なされます。|  
  
<a name="zerowidth_positive_lookbehind_assertion"></a>
## <a name="zero-width-positive-lookbehind-assertions"></a>ゼロ幅の正の後読みアサーション  
 次のグループ化構成体は、ゼロ幅の正の後読みアサーションを定義します。  
  
 `(?<=` *subexpression* `)`  
  
 ここで、*subexpression* は正規表現パターンです。 一致と見なされるためには、 *subexpression* が入力文字列の現在の位置の左側に出現する必要がありますが、 `subexpression` は一致結果には含まれません。 ゼロ幅の正の後読みアサーションはバックトラックしません。  
  
 通常、ゼロ幅の正の後読みアサーションは正規表現の先頭で使用されます。 定義されるパターンは一致の事前条件ですが、一致結果には含まれません。  
  
 たとえば、次の例では、21 世紀の西暦下 2 桁を照合します (つまり、一致する文字列の前に数字 "20" がある必要があります)。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#9](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookbehind1.cs#9)]
 [!code-vb[RegularExpressions.Language.Grouping#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookbehind1.vb#9)]  
  
 この正規表現パターン `(?<=\b20)\d{2}\b` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\d{2}`|2 桁の 10 進数と一致します。|  
|`(?<=\b20)`|ワード境界で 2 桁の 10 進数の前に 10 進数 "20" がある場合は照合を続行します。|  
|`\b`|ワード境界で照合を終了します。|  
  
 ゼロ幅の正の後読みアサーションは、キャプチャされたグループの最後の文字がそのグループの正規表現パターンと一致する文字のサブセットになる必要がある場合に、バックトラッキングを制限するためにも使用されます。 たとえば、グループが連続するすべての単語文字をキャプチャする場合に、ゼロ幅の正の後読みアサーションを使用して、最後の文字がアルファベットになるように要求できます。  
  
<a name="zerowidth_negative_lookbehind_assertion"></a>
## <a name="zero-width-negative-lookbehind-assertions"></a>ゼロ幅の負の後読みアサーション  
 次のグループ化構成体は、ゼロ幅の負の後読みアサーションを定義します。  
  
 `(?<!` *subexpression* `)`  
  
 ここで、*subexpression* は正規表現パターンです。 一致と見なされるためには、 *subexpression* が入力文字列の現在の位置の左側に出現しない必要があります。 ただし、 `subexpression` と一致しない部分文字列は一致結果には含まれません。  
  
 通常、ゼロ幅の負の後読みアサーションは正規表現の先頭で使用されます。 定義されるパターンでは、後に続く文字列内の一致が除外されます。 このアサーションは、キャプチャされたグループの最後の文字がそのグループの正規表現パターンと一致する 1 つ以上の文字にならない必要がある場合に、バックトラッキングを制限するためにも使用されます。 たとえば、グループが連続するすべての単語文字をキャプチャする場合に、ゼロ幅の正の後読みアサーションを使用して、最後の文字がアンダースコア (\_) にならないように要求できます。  
  
 次の例では、週末でない (土曜日や日曜日でない) 曜日の日付を照合します。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#10](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookbehind1.cs#10)]
 [!code-vb[RegularExpressions.Language.Grouping#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookbehind1.vb#10)]  
  
 この正規表現パターン `(?<!(Saturday|Sunday) )\b\w+ \d{1,2}, \d{4}\b` の解釈を次の表に示します。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`\w+`|1 個以上の単語文字の後に空白文字が続くパターンと一致します。|  
|`\d{1,2},`|1 桁または 2 桁の 10 進数の後に空白文字とコンマが続くパターンと一致します。|  
|`\d{4}\b`|4 桁の 10 進数と一致し、ワード境界で照合を終了します。|  
|<code>(?<!(Saturday&#124;Sunday) )</code>|文字列 "Saturday" または "Sunday" の後に空白が続くパターン以外が一致の前にある場合は、一致と見なされます。|  
  
<a name="atomic_groups"></a>
## <a name="atomic-groups"></a>アトミック グループ  
 次のグループ化構成体は、アトミック グループを表します (他の正規表現エンジンでは、非バックトラッキング部分式、アトミック部分式、または 1 回のみの部分式として知られています)。
  
 `(?>` *subexpression* `)`  
  
 ここで、*subexpression* は正規表現パターンです。  
  
 通常、正規表現にオプションまたは代替の一致パターンが含まれる場合に一致する文字列が見つからなかったときは、正規表現エンジンは複数の方向に分岐することで入力文字列とパターンを照合できます。 最初の分岐で一致する文字列が見つからなかった場合、正規表現エンジンは最初に一致した時点まで戻り (バックトラック)、2 番目の分岐を使用して照合を試みることができます。 すべての分岐が試されるまでこの処理を続行できます。  
  
 インデックスが 2 の `(?>`*subexpression*`)` 言語コンストラクトでは、バックトラッキングが無効になります。 正規表現エンジンは、入力文字列内の文字をできるだけ多く照合します。 一致する文字列が見つからなくなっても、バックトラックして代替パターン一致を試みることはありません。 つまり、部分式はその部分式単体と一致する文字列のみと一致します。部分式とその後続の部分式に基づく文字列の照合は試行しません。  
  
 バックトラッキングが成功しないことがわかっている場合は、このオプションを使用することをお勧めします。 正規表現エンジンで不要な検索が実行されないようにすることで、パフォーマンスが向上します。  
  
 次の例は、アトミック グループによってパターン一致の結果がどのように変わるかを示しています。 バックトラッキング正規表現は、ワード境界で一連の文字の繰り返しの後に同じ文字がもう一度出現するパターンと一致しますが、非バックトラッキング正規表現は一致しません。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#11](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/nonbacktracking1.cs#11)]
 [!code-vb[RegularExpressions.Language.Grouping#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/nonbacktracking1.vb#11)]  
  
 非バックトラッキング正規表現 `(?>(\w)\1+).\b` は、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`(\w)`|単語文字に使用される 1 文字と一致し、その文字を 1 番目のキャプチャ グループに代入します。|  
|`\1+`|最初にキャプチャされた部分文字列の値と 1 回以上一致します。|  
|`.`|任意の文字と一致します。|  
|`\b`|ワード境界で照合を終了します。|  
|`(?>(\w)\1+)`|重複する単語文字の 1 回以上の出現と一致しますが、バックトラックしてワード境界の最後の文字と一致することはありません。|  
  
<a name="Objects"></a>
## <a name="grouping-constructs-and-regular-expression-objects"></a>グループ化構成体および正規表現オブジェクト  
 正規表現キャプチャ グループと一致する部分文字列は、 <xref:System.Text.RegularExpressions.Group?displayProperty=nameWithType> オブジェクトで表されます。このオブジェクトは、 <xref:System.Text.RegularExpressions.GroupCollection?displayProperty=nameWithType> プロパティによって返される <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> オブジェクトから取得できます。 <xref:System.Text.RegularExpressions.GroupCollection> オブジェクトの値は次のように設定されます。  
  
- コレクション内の最初の <xref:System.Text.RegularExpressions.Group> オブジェクト (インデックス 0 の位置にあるオブジェクト) は、一致した文字列全体を表します。  
  
- 次の <xref:System.Text.RegularExpressions.Group> オブジェクト セットは、名前のない (番号付き) キャプチャ グループを表します。 正規表現で定義されている順序で左から右に並びます。 これらのグループのインデックス値の範囲は、1 からコレクション内の名前のないキャプチャ グループの数までです。 (特定のグループのインデックスは、その番号付き前方参照と同等です。 前方参照について詳しくは、「 [Backreference Constructs](../../../docs/standard/base-types/backreference-constructs-in-regular-expressions.md)」をご覧ください。)  
  
- 最後の <xref:System.Text.RegularExpressions.Group> オブジェクト セットは、名前付きキャプチャ グループを表します。 正規表現で定義されている順序で左から右に並びます。 最初の名前付きキャプチャ グループのインデックス値は、最後の名前のないキャプチャ グループのインデックスよりも 1 大きい数値になります。 正規表現に名前のないキャプチャ グループがない場合、最初の名前付きキャプチャ グループのインデックス値は 1 になります。  
  
 量指定子をキャプチャ グループに適用する場合、対応する <xref:System.Text.RegularExpressions.Group> オブジェクトの <xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType>、 <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType>、および <xref:System.Text.RegularExpressions.Capture.Length%2A?displayProperty=nameWithType> の各プロパティには、キャプチャ グループによってキャプチャされた最後の部分文字列が反映されます。 量指定子を持つグループによってキャプチャされたすべての部分文字列は、 <xref:System.Text.RegularExpressions.CaptureCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> オブジェクトから取得できます。  
  
 次の例では、 <xref:System.Text.RegularExpressions.Group> オブジェクトと <xref:System.Text.RegularExpressions.Capture> オブジェクトの関係を明確にします。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/objectmodel1.cs#4)]
 [!code-vb[RegularExpressions.Language.Grouping#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/objectmodel1.vb#4)]  
  
 正規表現パターン `(\b(\w+)\W+)+` は、文字列から単語を個別に抽出します。 このパターンは、次の表に示すように定義されています。  
  
|パターン|[説明]|  
|-------------|-----------------|  
|`\b`|ワード境界から照合を開始します。|  
|`(\w+)`|1 つ以上の単語文字に一致します。 これらの文字が集まって、単語を形成します これが 2 番目のキャプチャ グループです。|  
|`\W+`|1 個以上の単語文字に使用されない文字と一致します。|  
|`(\b(\w+)\W+)`|1 個以上の単語文字、および 1 個以上の単語文字に使用されない文字が 1 回以上続くパターンと一致します。 これが最初のキャプチャ グループです。|  
  
 2 番目のキャプチャ グループは、文の各単語と一致します。 最初のキャプチャ グループは、各単語およびその単語に続く句読点や空白と一致します。 インデックスが 2 の <xref:System.Text.RegularExpressions.Group> オブジェクトは、2 番目のキャプチャ グループと一致したテキストの情報を保持します。 キャプチャ グループによってキャプチャされたすべての単語は、 <xref:System.Text.RegularExpressions.CaptureCollection> プロパティによって返される <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> オブジェクトから取得できます。  
  
## <a name="see-also"></a>参照

- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [バックトラッキング](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)
