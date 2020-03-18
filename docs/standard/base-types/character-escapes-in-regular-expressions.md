---
title: .NET 正規表現での文字のエスケープ
description: .NET 正規表現での特殊文字とエスケープ文字について説明します。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- unescaped characters
- replacement patterns
- characters, escapes
- regular expressions, character escapes
- escape characters
- .NET Framework regular expressions, character escapes
- constructs, character escapes
ms.assetid: f49cc9cc-db7d-4058-8b8a-422bc08b29b0
ms.openlocfilehash: 82e60b3cb5eb777d48219209550367642f78d8c3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711429"
---
# <a name="character-escapes-in-regular-expressions"></a>正規表現での文字のエスケープ
正規表現の円記号 (\\) は、次のいずれかを示します。  
  
- 後に続く文字が、次のセクションの表に示す特殊文字であること。 たとえば、`\b` は、正規表現の一致がワード境界から開始する必要があることを示すアンカーであり、`\t` はタブを表します。`\x020` は空白を表します。  
  
- エスケープされていない言語構成要素として解釈される文字は、文字どおりに解釈する必要があること。 たとえば、中かっこ (`{`) は量指定子の定義を開始しますが、円記号とそれに続く中かっこ (`\{`) は、正規表現エンジンで中かっこに一致させる必要があることを示します。 同様に、1 つの円記号はエスケープされた言語構成要素の開始を示しますが、2 つの円記号 (`\\`) は、正規表現エンジンで円記号に一致させる必要があることを示します。  
  
> [!NOTE]
> 文字エスケープは、正規表現では認識されますが、置換パターンでは認識されません。  
  
## <a name="character-escapes-in-net"></a>.NET での文字のエスケープ  
 次の表は、.NET の正規表現でサポートされている文字エスケープの一覧です。  
  
|文字または文字シーケンス|説明|  
|---------------------------|-----------------|  
|次の文字を除くすべての文字:<br /><br /> . $ ^ { [ ( &#124; ) * + ? \ |**文字またはシーケンス**の列にリストされているもの以外の文字は、正規表現で特別な意味を持ちません。それらは、その文字自体と一致します。<br /><br /> **文字またはシーケンス**の列に含まれる文字は、特殊な正規表現言語要素です。 正規表現でそれらの文字と一致するためには、エスケープするか、[正の文字グループ](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)に含める必要があります。 たとえば、正規表現の `\$\d+` または `[$]\d+`は「$1200」と一致します。|  
|`\a`|ビープ音 (アラーム) 文字の `\u0007`。|  
|`\b`|`[`*character_group*`]` 文字クラスでバックスペースの `\u0008` と一致します  (「[文字クラス](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)」を参照してください)。文字クラスの外部では、`\b` はワード境界と一致するアンカーです (「[アンカー](../../../docs/standard/base-types/anchors-in-regular-expressions.md)」を参照してください)。|  
|`\t`|タブの `\u0009`。|  
|`\r`|キャリッジ リターンの `\u000D`。 `\r` の機能は `\n` という改行文字と同じではありません。|  
|`\v`|垂直タブの `\u000B`。|  
|`\f`|フォーム フィードの `\u000C`。|  
|`\n`|改行文字の `\u000A`。|  
|`\e`|エスケープ文字の `\u001B`。|  
|`\` *nnn*|ASCII 文字と一致します。*nnn* は、8 進文字コードを表す 2 桁または 3 桁で構成されます。 たとえば、`\040` は、空白文字を表します。 この構成体は、1 桁のみの場合 (`\2` など)、またはキャプチャ グループの番号に対応する場合には前方参照として解釈されます。 (「[前方参照構成体](../../../docs/standard/base-types/backreference-constructs-in-regular-expressions.md)」を参照してください。)|  
|`\x` *nn*|ASCII 文字と一致します。*nn* は 2 桁の 16 進文字コードです。|  
|`\c` *X*|ASCII の制御文字と一致します。X は制御文字です。 たとえば、`\cC` は CTRL-C です。|  
|`\u` *nnnn*|値が *nnnn* の 16 進数である UTF-16 コード単位と一致します。 **注:** .NET では、Unicode を指定するために使用する Perl5 の文字エスケープはサポートされません。 Perl 5 の文字エスケープは `\x{` *####* `…}` の形式です。ここで、 *####* `…` は一連の 16 進数です。 代わりに、`\u`*nnnn* を使用します。|  
|`\`|エスケープ文字として認識されない文字が後ろに付いている場合は、その文字と一致します。 たとえば、`\*` はアスタリスク (*) と一致し、`\x2A` と同じです。|  
  
## <a name="an-example"></a>例  
 正規表現の文字エスケープの使用例を次に示します。 2009 年の世界最大規模の都市の名前と人口を含む文字列を解析します。 各都市名は、タブ (`\t`) または縦棒 (&#124; または `\u007c`) で区切られます。 個々の都市とその人口は、復帰とライン フィードで互いに区切られています。  
  
 [!code-csharp[RegularExpressions.Language.Escapes#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.escapes/cs/escape1.cs#1)]
 [!code-vb[RegularExpressions.Language.Escapes#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.escapes/vb/escape1.vb#1)]  
  
 この正規表現 `\G(.+)[\t|\u007c](.+)\r?\n` の解釈を次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`\G`|最後の一致の終了位置から照合を開始します。|  
|`(.+)`|任意の文字と 1 回以上、一致します。 これが最初のキャプチャ グループです。|  
|`[\t\u007c]`|タブ (`\t`) または縦棒 (&#124;) と一致します。|  
|`(.+)`|任意の文字と 1 回以上、一致します。 これが 2 番目のキャプチャ グループです。|  
|`\r?\n`|復帰とそれに続く改行に、0 回または 1 回一致します。|  
  
## <a name="see-also"></a>関連項目

- [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
