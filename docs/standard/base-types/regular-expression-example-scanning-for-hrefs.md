---
title: '正規表現の例: HREF のスキャン'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, examples
- parsing text with regular expressions, examples
- regular expressions, examples
- .NET Framework regular expressions, examples
- regular expressions [.NET Framework], examples
- pattern-matching with regular expressions, examples
ms.assetid: fae2c15b-7adf-4b15-b118-58eb3906994f
ms.openlocfilehash: d8546980dd0cf58ca7c095750f2749d5a6bc7723
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73084225"
---
# <a name="regular-expression-example-scanning-for-hrefs"></a>正規表現の例: HREF のスキャン
次の例では、入力文字列を検索して、文字列中のすべての href="…" 値とその場所を表示します。  
  
## <a name="the-regex-object"></a>Regex オブジェクト  
 `DumpHRefs` メソッドは、ユーザー コードから複数回呼び出される可能性があるため、`static` (Visual Basic の場合は `Shared`) <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> メソッドを使用します。 これにより、正規表現エンジンが正規表現をキャッシュできるようになり、メソッドを呼び出すたびに新しい <xref:System.Text.RegularExpressions.Regex> オブジェクトをインスタンス化するオーバーヘッドを回避できます。 <xref:System.Text.RegularExpressions.Match> オブジェクトは、文字列内のすべての一致を反復処理するために使用されます。  
  
 [!code-csharp[RegularExpressions.Examples.HREF#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#1)]
 [!code-vb[RegularExpressions.Examples.HREF#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#1)]  
  
 `DumpHRefs` メソッドを呼び出す例を次に示します。  
  
 [!code-csharp[RegularExpressions.Examples.HREF#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#2)]
 [!code-vb[RegularExpressions.Examples.HREF#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#2)]  
  
 この正規表現パターン `href\s*=\s*(?:["'](?<1>[^"']*)["']|(?<1>\S+))` の解釈を次の表に示します。  
  
|パターン|説明|  
|-------------|-----------------|  
|`href`|リテラル文字列 "href" と一致します。 一致では、大文字と小文字を区別しません。|  
|`\s*`|0 個以上の空白文字と一致します。|  
|`=`|等号と一致します。|  
|`\s*`|0 個以上の空白文字と一致します。|  
|`(?:\["'\](?<1>\[^"'\]*)["']|(?<1>\S+))`|次のいずれかと一致し、キャプチャ グループに結果を代入しません。<br /> <ul><li><p>\- 引用符またはアポストロフィ、引用符またはアポストロフィ以外の任意の文字の 0 回以上の繰り返し、引用符またはアポストロフィの順に続く文字列。 このパターンには `1` という名前のグループが含まれています。</p></li><li><p>\- 1 個以上の空白以外の文字。 このパターンには `1` という名前のグループが含まれています。</p></li></ul>|  
|`(?<1>[^"']*)`|引用符またはアポストロフィ以外の任意の文字の 0 回以上の繰り返しを `1` という名前のキャプチャ グループに代入します。|  
|`(?<1>\S+)`|1 個以上の空白以外の文字を `1` という名前のキャプチャ グループに代入します。|  
  
## <a name="match-result-class"></a>Match 結果クラス  
 検索結果は <xref:System.Text.RegularExpressions.Match> クラス内に格納されます。これにより、検索処理によって抽出されたすべての部分文字列にアクセスできます。 このクラスは、検索対象となった文字列や、使用された正規表現も記憶しているため、<xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> メソッドを呼び出して、最後の検索が終了した位置から別の検索を実行することができます。  
  
## <a name="explicitly-named-captures"></a>明示的に指定したキャプチャ  
 従来の正規表現では、キャプチャするかっこに自動的に連番が割り当てられます。 その結果、2 つの問題が発生します。 1 つ目の問題は、かっこのペアの挿入や削除を行って正規表現が修正されると、新しい番号を反映するために、番号付きのキャプチャを参照するコードをすべて書き直す必要があることです。 2 つ目の問題は、ある文字列の検索用に 2 つの代替表現を指定する際、異なるかっこのペアを使用することが多いため、実際にどちらの代替表現から結果が返されたのかを判断するのが難しい場合があることです。  
  
 これらの問題に対処するために、<xref:System.Text.RegularExpressions.Regex> クラスでは指定されたスロットに一致文字列をキャプチャするための構文 `(?<name>…)` をサポートしています (スロットには、文字列または整数の名前を付けることができます。整数の名前を付けた方が、よりすばやく再呼び出しできます)。 これにより、同じ文字列に対する代替表現の一致結果をすべて同じ場所に渡すことができます。 競合が発生する場合は、スロットにキャプチャされた最後の一致文字列が、適切な一致であると見なされます。 (ただし、1 つのスロットで複数の一致文字列の完全なリストを使用することもできます。 詳細については、<xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> コレクションを参照してください。)  
  
## <a name="see-also"></a>関連項目

- [.NET の正規表現](../../../docs/standard/base-types/regular-expressions.md)
