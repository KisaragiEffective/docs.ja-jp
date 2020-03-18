---
title: .NET 正規表現での代替コンストラクト
description: 正規表現での条件一致に代替構成体を使用する方法について説明します。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- either/or matching
- alternative matching patterns
- regular expressions, alternation constructs
- alternation constructs
- optional matching patterns
- constructs, alternation
- .NET Framework regular expressions, alternation constructs
ms.assetid: 071e22e9-fbb0-4ecf-add1-8d2424f9f2d1
ms.openlocfilehash: 02664bd2812f89649ec933483161263bae530a75
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159690"
---
# <a name="alternation-constructs-in-regular-expressions"></a>正規表現での代替コンストラクト

代替構成体は、択一条件または条件一致を有効にするように正規表現を変更します。 .NET では、次の 3 つの代替構成体がサポートされています。

- [&#124; を使用したパターン マッチング](#Either_Or)
- [(?(expression)yes&#124;no) を使用した条件一致](#Conditional_Expr)
- [有効なキャプチャ グループに基づく条件一致](#Conditional_Group)

<a name="Either_Or"></a>
## <a name="pattern-matching-with-124"></a>&#124; を使用したパターン マッチング

縦棒 (`|`) 文字を使って、`|` 文字で各パターンを区切った一連のパターンのいずれかと照合できます。

肯定的文字クラスと同じように、 `|` 文字を使うと、複数の文字のいずれか 1 文字と照合できます。 次の例は、肯定的文字クラスと `|` 文字との択一パターン マッチを使って、文字列から単語 "gray"や "grey" を検索します。 この場合、 `|` 文字を使うと、より詳細な正規表現が生成されます。

[!code-csharp[RegularExpressions.Language.Alternation#1](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation1.cs#1)]
[!code-vb[RegularExpressions.Language.Alternation#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation1.vb#1)]

`|` 文字を使う正規表現 `\bgr(a|e)y\b` の解釈を次の表に示します。

|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`gr`|文字 "gr" と一致します。|  
|<code>(a&#124;e)</code>|"a" または "e" と一致します。|  
|`y\b`|ワード境界にある "y" と一致します。|  

また、`|` 文字を使って、複数の文字や部分式との択一照合を実行できます。これは、文字リテラルと正規表現言語要素を自由に組み合わせて使うことができます (文字クラスにこの機能はありません)。次の例では、`|` 文字を使って、米国の社会保障番号 (SSN) (*ddd*-*dd*-*dddd* という形式の 9 桁の数字)、または米国の雇用者番号 (EIN) (*dd*-*ddddddd* という形式の 9 桁の数字) のいずれかを抽出します。

[!code-csharp[RegularExpressions.Language.Alternation#2](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation2.cs#2)]
[!code-vb[RegularExpressions.Language.Alternation#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation2.vb#2)]  

この正規表現 `\b(\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` の解釈を次の表に示します。
  
|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|<code>(\d{2}-\d{7}&#124;\d{3}-\d{2}-\d{4})</code>|「2 桁と 7 桁の 10 進数がハイフンでつながれた文字列」または「3 桁、2 桁、4 桁の 10 進数がそれぞれハイフンでつながれた文字列」のいずれかと一致します。|  
|`\d`|ワード境界で照合を終了します。|  
  
<a name="Conditional_Expr"></a>
## <a name="conditional-matching-with-an-expression"></a>式を使用した条件一致

この言語要素では、最初のパターンに一致するかどうかに応じて、2 つのパターンのいずれかの照合を実行します。 構文は次のとおりです。  

`(?(` *expression* `)` *yes* `|` *no* `)`

ここで、 *expression* は照合する最初のパターン、 *yes* は *expression* が一致した場合に照合するパターン、 *no* は *expression* が一致しなかった場合に照合するパターン (省略可能) です。 *expression* は正規表現エンジンによってゼロ幅アサーションとして処理されるので、 *expression*が評価された後、正規表現エンジンによる入力ストリーム内の評価位置は前に進みません。 そのため、この構成体は次の構成体と同じです。

`(?(?=` *expression* `)` *yes* `|` *no* `)`

ここで、`(?=`*expression*`)` はゼロ幅アサーションの構成体として解釈されます (詳しくは、「[正規表現でのグループ化構成体](grouping-constructs-in-regular-expressions.md)」をご覧ください)。正規表現エンジンによって *expression* はアンカー (ゼロ幅アサーション) として解釈されるので、*expression* は、ゼロ幅アサーション (詳しくは「[正規表現のアンカー](anchors-in-regular-expressions.md)」を参照) か、*yes* にも含まれている部分式のいずれかである必要があります。 それ以外の場合、 *yes* パターンには一致しません。  
  
> [!NOTE]
> *expression* が名前付きキャプチャ グループや番号付きキャプチャ グループである場合、代替構成体はキャプチャ テストとして解釈されます。詳細については、次のセクション「[有効なキャプチャ グループに基づく条件一致](#Conditional_Group)」を参照してください。 つまり、正規表現エンジンは、キャプチャした部分文字列を照合しようとはせず、代わりにグループが存在するかどうかをテストします。  
  
次の例は、前の「[&#124; を使用したパターン マッチング](#Either_Or)」セクションで説明した例を少し変更したものです。 条件一致を使用して、ワード境界の後の最初の 3 文字が「2 桁の数字の後にハイフン」であるかどうかを判定します。 一致した場合は、米国の雇用者番号 (EIN) との照合を試みます。 一致しない場合は、米国の社会保険番号 (SSN) との照合を試みます。

[!code-csharp[RegularExpressions.Language.Alternation#3](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation3.cs#3)]
[!code-vb[RegularExpressions.Language.Alternation#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation3.vb#3)]

この正規表現パターン `\b(?(\d{2}-)\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` の解釈を次の表に示します。

|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`(?(\d{2}-)`|次の 3 文字が「2 桁の数字の後にハイフン」で構成されているかどうかを判定します。|  
|`\d{2}-\d{7}`|前のパターンに一致する場合は、「2 桁の数字の後にハイフン、その後に 7 桁の数字」を照合します。|  
|`\d{3}-\d{2}-\d{4}`|前のパターンに一致しない場合は、「3 桁の 10 進数、ハイフン、2 桁の 10 進数、もう 1 つのハイフン、および 4 桁の 10 進数」を照合します。|  
|`\b`|ワード境界に一致します。|  

<a name="Conditional_Group"></a>
## <a name="conditional-matching-based-on-a-valid-captured-group"></a>有効なキャプチャ グループに基づく条件一致

この言語要素では、指定されたキャプチャ グループに一致するかどうかに応じて、2 つのパターンのいずれかを照合します。 構文は次のとおりです。

`(?(` *name* `)` *yes* `|` *no* `)`

or

`(?(` *number* `)` *yes* `|` *no* `)`

ここで、 *name* はキャプチャ グループの名前、 *number* はキャプチャ グループの番号です。 *yes* は、 *name* または *number* が一致する場合に照合する式です。 *no* き、一致しない場合に照合する式 (省略可能) です。

*name* が正規表現パターンで使用されているキャプチャ グループの名前に一致しない場合、その代替構成体は式のテスト (前のセクションで説明したもの) として解釈されます。 通常、これは *expression* が `false`に評価されることを意味します。 *number* が正規表現パターンで使用されている番号付きキャプチャ グループに対応しない場合は、正規表現エンジンが <xref:System.ArgumentException>をスローします。

次の例は、前の「[&#124; を使用したパターン マッチング](#Either_Or)」セクションで説明した例を少し変更したものです。 「2 桁の数字の後にハイフン」で構成される `n2` という名前付きキャプチャ グループを使用しています。 代替構成体により、このキャプチャ グループが入力文字列に一致したかどうかテストされます。 一致した場合は、場合に、米国の 9 桁の雇用者番号 (EIN) との照合を試みます。 一致しなかった場合は、米国の 9 桁の社会保険番号 (SSN) との照合を試みます。

[!code-csharp[RegularExpressions.Language.Alternation#4](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation4.cs#4)]
[!code-vb[RegularExpressions.Language.Alternation#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation4.vb#4)]

この正規表現パターン `\b(?<n2>\d{2}-)?(?(n2)\d{7}|\d{3}-\d{2}-\d{4})\b` の解釈を次の表に示します。

|パターン|説明|  
|-------------|-----------------|  
|`\b`|ワード境界から開始します。|  
|`(?<n2>\d{2}-)?`|「2 桁の数字の後にハイフン」の 0 個または 1 個の出現と照合します。 このキャプチャ グループに `n2` という名前を付けます。|  
|`(?(n2)`|`n2` への一致が入力文字列内に見つかるかどうかテストします。|  
|`\d{7}`|`n2` が一致した場合は、7 桁の 10 進数を照合します。|  
|<code>&#124;\d{3}-\d{2}-\d{4}</code>|`n2` が一致しなかった場合は、「3 桁の 10 進数、ハイフン、2 桁の 10 進数、もう 1 つのハイフン、および 4 桁の 10 進数」を照合します。|  
|`\b`|ワード境界に一致します。|  

この例を少し変更して、名前付きグループではなく番号付きグループを使用する例を次に示します。 正規表現パターンは `\b(\d{2}-)?(?(1)\d{7}|\d{3}-\d{2}-\d{4})\b`です。

[!code-csharp[RegularExpressions.Language.Alternation#5](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation5.cs#5)]
[!code-vb[RegularExpressions.Language.Alternation#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation5.vb#5)]

## <a name="see-also"></a>関連項目

- [正規表現言語 - クイック リファレンス](regular-expression-language-quick-reference.md)
